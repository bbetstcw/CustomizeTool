<properties
   pageTitle="Reliable Services WCF communication stack | Windows Azure"
   description="The built-in WCF communication stack in Services Fabric provides client-service WCF communication for Reliable Services."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
	ms.service="service-fabric"
	ms.date="11/17/2015"
	wacn.date=""/>

# WCF based communication stack for Reliable Services
Reliable services framework allows Service authors to decide the communication stack they want to use for their service. They can plugin the communication stack of their choice via the `ICommunicationListener` returned from the [CreateServiceReplicaListeners or CreateServiceInstanceListeners](/documentation/articles/service-fabric-reliable-service-communication) methods. The framework provides a WCF based implementation of the communication stack, for service authors who want to use WCF based communication.

## WCF Communication Listener
The WCF specific implementation of `ICommunicationListener` is provided by the `Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener` class.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    // TODO: If your service needs to handle user requests, return a list of ServiceReplicaListeners here.
    return new[] { new ServiceReplicaListener(parameters =>
        new WcfCommunicationListener(typeof(ICalculator), this)
        {
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // which identifies the endpoint that the wcf servicehost should listen on.
            //
            EndpointResourceName = "ServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            Binding = this.CreateListenBinding()
        }
    )};
}

```

## Writing clients for WCF communication stack
For writing clients to communicate with services using WCF, the framework provides `WcfClientCommunicationFactory`, which is the WCF specific implementation of [`ClientCommunicationFactoryBase`](/documentation/articles/service-fabric-reliable-service-communication).

```csharp

public WcfCommunicationClientFactory(
    ServicePartitionResolver servicePartitionResolver = null,
    Binding binding = null,
    object callback = null,
    IList<IExceptionHandler> exceptionHandlers = null,
    IEnumerable<Type> doNotRetryExceptionTypes = null)

```

The WCF communication channel can be accessed from the `WcfCommunicationClient` created by the `WcfCommunicationClientFactory`

```csharp

public class WcfCommunicationClient<TChannel> : ICommunicationClient where TChannel : class
{
    public TChannel Channel { get; }
    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}

```

Client code can use the `WcfCommunicationClientFactory` along with the `ServicePartitionClient` to determine the service endpoint and make communicate with the service.

```csharp

//
// Create a service resolver for resolving the endpoints of the calculator service.
//
ServicePartitionResolver serviceResolver = new ServicePartitionResolver(() => new FabricClient());

//
// Create the binding.
//
NetTcpBinding binding = CreateClientConnectionBinding();

var clientFactory = new WcfCommunicationClientFactory<ICalculator>(
    serviceResolver,// ServicePartitionResolver
    binding,        // Client binding
    null,           // Callback object
    null);          // donot retry Exception types


//
// Create a client for communicating with the calc service which has been created with
// Singleton partition scheme.
//
var calculatorServicePartitionClient = new ServicePartitionClient<WcfCommunicationClient<ICalculator>>(
    clientFactory,
    ServiceName);

//
// Call the service to perform the operation.
//
var result = calculatorServicePartitionClient.InvokeWithRetryAsync(
    client => client.Channel.AddAsync(2, 3)).Result;

```
 
## Next Steps
* [Remote procedure call with Reliable Services remoting](/documentation/articles/service-fabric-reliable-services-communication-remoting)

* [Web API with OWIN in Reliable Services](/documentation/articles/service-fabric-reliable-services-communication-webapi)
