<properties
	pageTitle="How to use Service Bus queues with Ruby | Windows Azure"
	description="Learn how to use Service Bus queues in Azure. Code samples written in Ruby."
	services="service-bus"
	documentationCenter="ruby"
	authors="sethmanheim"
	manager="timlt"
	editor=""/>

<tags
	ms.service="service-bus"
	ms.date="08/31/2015"
	wacn.date=""/>

# How to Use Service Bus Queues

[AZURE.INCLUDE [service-bus-selector-queues](../includes/service-bus-selector-queues.md)]

This guide will show you how to use Service Bus queues. The samples are
written in Ruby and use the Azure gem. The scenarios
covered include **creating queues, sending and receiving messages**, and
**deleting queues**. For more information on queues, see the [Next Steps](#next-steps) section.

## What are Service Bus queues?

Service Bus Queues support a **brokered messaging communication**
model. When using queues, components of a distributed application do not
communicate directly with each other, they instead exchange messages via
a queue, which acts as an intermediary. A message producer (sender)
hands off a message to the queue and then continues its processing.
Asynchronously, a message consumer (receiver) pulls the message from the
queue and processes it. The producer does not have to wait for a reply
from the consumer in order to continue to process and send further
messages. Queues offer **First In, First Out (FIFO)** message delivery
to one or more competing consumers. That is, messages are typically
received and processed by the receivers in the order in which they were
added to the queue, and each message is received and processed by only
one message consumer.

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

Service Bus queues are a general-purpose technology that can be used for
a wide variety of scenarios:

-   Communication between web and worker roles in a multi-tier
    Azure application
-   Communication between on-premises apps and Azure hosted apps
    in a hybrid solution
-   Communication between components of a distributed application
    running on-premises in different organizations or departments of an
    organization

Using queues can enable you to scale out your applications better, and
enable more resiliency to your architecture.

## Create a service namespace
To begin using Service Bus queues in Azure, you must first create a service namespace. A service namespace provides a scoping container for addressing Service Bus resources within your application. You must create the
namespace through the command-line interface because the Portal does not create the service bus with an ACS connection.

To create a service namespace:

1. Open an Azure Powershell console.

2. Type the command to create an Azure service bus namespace as shown below. Provide your own namespace value and specify the same region as your application.

    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'China North' -NamespaceType 'Messaging' -CreateACSNamespace $true

    ![Create Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)

## Obtain management credentials for the namespace
In order to perform management operations, such as creating a queue on the new
namespace, you must obtain the management credentials for the namespace.

The PowerShell cmdlet you ran to create the Azure service bus namespace displays
the key you can use to manage the namespace. Copy the **DefaultKey** value. You
will use this value in your code later in this tutorial.

![Copy key](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)

> [AZURE.NOTE]
> You can also find this key if you log on to the
> [Azure Management Portal](http://manage.windowsazure.cn/) and navigate to the
> connection information for your service bus namespace.

## Create a Ruby application

Create a Ruby application. For instructions, see [Create a Ruby Application on Azure](/develop/ruby/tutorials/web-app-with-linux-vm/).

## Configure your application to use Service Bus

To use Azure service bus, you need to download and use the Ruby azure package, which includes a set of convenience libraries that communicate with the storage REST services.

### Use RubyGems to obtain the package

1. Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).

2. Type "gem install azure" in the command window to install the gem and dependencies.

### Import the package

Use your favorite text editor, add the following to the top of the Ruby file where you intend to use storage:

    require "azure"

## Set up an Azure Service Bus connection

The azure module will read the environment variables **AZURE_SERVICEBUS_NAMESPACE** and **AZURE_SERVICEBUS_ACCESS_KEY**
for information required to connect to your Azure service bus namespace. If these environment variables are not set, you must specify the namespace information before using **Azure::ServiceBusService** with the following code:

    Azure.config.sb_namespace = "<your azure service bus namespace>"
    Azure.config.sb_access_key = "<your azure service bus access key>"

Set the service bus namespace value to the value you created rather than the entire URL. For example, use **"yourexamplenamespace"**, not "yourexamplenamespace.servicebus.chinacloudapi.cn".

## How to create a queue

The **Azure::ServiceBusService** object lets you work with queues. To create a queue, use the **create_queue()** method. The following example creates a queue or print out the error if there is any.

    azure_service_bus_service = Azure::ServiceBusService.new
    begin
      queue = azure_service_bus_service.create_queue("test-queue")
    rescue
      puts $!
    end

You can also pass in a **Azure::ServiceBus::Queue** object with additional options, which allows you to override default queue settings such as message time to live or maximum queue size. The following example shows setting the maximum queue size to 5GB and time to live to 1 minute:

    queue = Azure::ServiceBus::Queue.new("test-queue")
    queue.max_size_in_megabytes = 5120
    queue.default_message_time_to_live = "PT1M"

    queue = azure_service_bus_service.create_queue(queue)

## How to send messages to a queue

To send a message to a Service Bus queue, you application will call the **send_queue_message()** method on the **Azure::ServiceBusService** object. Messages sent to (and received from) service bus queues are **Azure::ServiceBus::BrokeredMessage** objects, and have a set of standard properties (such as **label** and **time_to_live**), a dictionary that is used to hold custom application specific properties, and a body of arbitrary application data. An application can set the body of the message by passing a string value as the message and any required standard properties will be populated with default values.

The following example demonstrates how to send a test message to the queue named "test-queue" using **send_queue_message()**:

    message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
    message.correlation_id = "test-correlation-id"
    azure_service_bus_service.send_queue_message("test-queue", message)

Service bus queues support a maximum message size of 256 KB (the header, which includes the standard and custom application properties, can have a maximum size of 64 KB). There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue. This queue size is defined at creation time, with an upper limit of 5 GB.

## How to receive messages from a queue

Messages are received from a queue using the **receive_queue_message()** method on the **Azure::ServiceBusService** object. By default, messages are read and locked without being deleted from the queue. However, you can delete messages from the queue as they are read by setting the **:peek_lock** option to **false**.

The default behavior makes the reading and deleting into a two stage operation, which makes it possible to support applications that cannot tolerate missing messages. When service bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application. After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling **delete_queue_message()** method and providing the message to be deleted as a parameter. The **delete_queue_message()** method will mark the message as being consumed and remove it from the queue.

If the **:peek_lock** parameter is set to **false**, reading and deleting the message becomes the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure. To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it. Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.

The example below demonstrates how messages can be received and processed using **receive_queue_message()**. The example first receives and deletes a message by using **:peek_lock** set to **false**, then it receives another message and then deletes the message using **delete_queue_message()**:

    message = azure_service_bus_service.receive_queue_message("test-queue",
	  { :peek_lock => false })
    message = azure_service_bus_service.receive_queue_message("test-queue")
    azure_service_bus_service.delete_queue_message(message)

## How to handle application crashes and unreadable messages

Service bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message. If a receiver application is unable to process the message for some reason, then it can call the **unlock_queue_message()** method on the **Azure::ServiceBusService** object. This will cause service bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.

There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (e.g., if the application crashes), then service bus will unlock the message automatically and make it available to be received again.

In the event that the application crashes after processing the message but before the **delete_queue_message()** method is called, then the message will be redelivered to the application when it restarts. This is often called **At Least Once Processing**, that is, each message will be processed at least once but in certain situations the same message may be redelivered. If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery. This is often achieved using the **message_id** property of the message, which will remain constant across delivery attempts.

## Next Steps

Now that you've learned the basics of Service Bus queues, follow these links to learn more.

-   Overview of [queues, topics, and subscriptions](/documentation/articles/service-bus-queues-topics-subscriptions)
-   Visit the [Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub

For a comparision between the Azure Service Bus Queues discussed in this article and Azure Queues discussed in the [How to use the Azure Queue Service](/develop/ruby/how-to-guides/queue-service/) article, see [Azure Queues and Azure Service Bus Queues - Compared and Contrasted](/documentation/articles/service-bus-azure-and-service-bus-queues-compared-contrasted)
 