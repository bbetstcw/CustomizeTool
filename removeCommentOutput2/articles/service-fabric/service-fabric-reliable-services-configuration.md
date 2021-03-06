<properties
   pageTitle="Overview of the Azure Service Fabric Reliable Services Configuration | Windows Azure"
   description="Learn about configuring stateful Reliable Services in Azure Service Fabric."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
	ms.service="Service-Fabric"
	ms.date="10/28/2015"
	wacn.date=""/>

# Configuring Stateful Reliable Services
Stateful Reliable Service's default configuration can be modified via the configuration package (Config) or in the service implementation (Code).

+ **Config** - Configuration via the config package is accomplished by changing the "Settings.xml" file generated in the Visual Studio package root under the "Config" folder for each service in the application.
+ **Code**   - Configuration via code is accomplished by overriding StatefulService.CreateReliableStateManager, and creating a ReliableStateManager using a  ReliableStateManagerConfiguration object with the appropriate options set.

By default, the Service Fabric runtime looks for pre-defined section names in the "Settings.xml" file and consumes the configuration values while creating the underlying runtime components.

> [AZURE.NOTE] Do **NOT** delete the section names of the following configurations in the "Settings.xml" file that is generated in the Visual Studio Solution, unless you plan to configure your service via code.
Renaming the config package or section names will require a code change when configuring the ReliableStateManager.


## Replicator Security Configuration
Replicator Security Configurations are used to secure the communication channel that is used during replication. This means services will not be able to see each other's replication traffic, ensuring the data that is made highly available is also secure.
By default, an empty security configuration section does not enable replication security.

### Default Section Name
ReplicatorSecurityConfig

### Configuration Names
Refer to [Replication Security](/documentation/articles/service-fabric-replication-security)

> [AZURE.NOTE] To change this section name, override the replicatorSecuritySectionName parameter to the ReliableStateManagerConfiguration constructor when creating the ReliableStateManager for this service.


## Replicator Configuration
Replicator Configurations are used to configure the replicator that is responsible for making the stateful Reliable Service's state highly reliable by replicating and persisting the state locally.
The default configuration is generated by the Visual Studio template and should suffice. This section talks about additional configurations that are available to tune the replicator.

### Default Section Name
ReplicatorConfig

> [AZURE.NOTE] To change this section name, override the replicatorSettingsSectionName parameter to the ReliableStateManagerConfiguration constructor when creating the ReliableStateManager for this service.


### Configuration Names
|Name|Unit|Default Value|Remarks|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Seconds|0.05|Time period for which the replicator at the secondary waits after receiving an operation before sending back an acknowledgement to the primary. Any other acknowledgements to be sent for operations processed within this interval are sent as one response.|
|ReplicatorEndpoint|N/A|N/A - RequiredParameter|IP address and port that the primary/secondary replicator will use to communicate with other replicators in the replica set. This should reference a TCP Resource Endpoint in the service manifest. Refer to [Service Manifest Resources](/documentation/articles/service-fabric-service-manifest-resources) to read more about defining endpoint resources in service manifest. |
|MaxPrimaryReplicationQueueSize|Number of operations|8192|Maximum number of operations in the primary queue. An Operation is freed up after the primary replicator receives an acknowledgement from all the secondary replicators.This value must be greater than 64 and a power of 2.|
|MaxSecondaryReplicationQueueSize|Number of operations|16384|Maximum number of operations in the secondary queue. An Operation is freed up after making its state highly available through persistence. This value must be greater than 64 and a power of 2.|
|CheckpointThresholdInMB|MB|200|Amount of log file space after which the state is checkpointed.|
|MaxRecordSizeInKB|KB|1024|Largest record size which the replicator may write in the log. This value must be a multiple of 4 and greater than 16.|
|OptimizeLogForLowerDiskUsage|boolean|true|When true the log is configured so that the replica's dedicated log file is created using an NTFS sparse file. This lowers the actual disk space usage for the file. When false the file is created with fixed allocations which provide the best write performance.|
|MaxRecordSizeInKB|KB|1024|Largest record size which the replicator may write in the log. This value must be a multiple of 4 and greater than 16.|
|SharedLogId|guid|""|Specifies a unique guid to use for identifying the shared log file used with this replica. Typically services should not use this setting however if SharedLogId is specified then SharedLogPath must also be specified.|
|SharedLogPath|Fully qualified pathname|""|Specifies the fully qualified path where the shared log file for this replica will be created. Typically services should not use this setting however if SharedLogPath is specified then SharedLogId must also be specified.|


## Sample Configuration Via Code
```csharp
protected override IReliableStateManager CreateReliableStateManager()
{
    return new ReliableStateManager(
        new ReliableStateManagerConfiguration(
            new ReliableStateManagerReplicatorSettings
            {
                RetryInterval = TimeSpan.FromSeconds(3)
            }));
}
```


## Sample Configuration File
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```


## Remarks
BatchAcknowledgementInterval controls replication latency. A value of '0' results in the lowest possible latency, at the cost of throughput (as more acknowledgement messages must be sent and processed, each containing fewer acknowledgements).
The larger the value for BatchAcknowledgementInterval, the higher the overall replication throughput, at the cost of higher operation latency. This directly translates to the latency of transaction commits.

The value for CheckpointThresholdInMB controls the amount of disk space that the replicator can use to store state information in the replica's dedicated log file. Increasing this to a higher value than the default could result in faster reconfiguration times when a new replica is added to the set due to the partial state transfer that takes place due to availablity of more history of operations in the log, while potentially increasing the recovery time of a replica after a crash.

The OptimizeForLowerDiskUsage setting allows log file space to be "over-provisioned" so that active replicas can store more state information in their log files while inactive replicas would use less disk
space. Although this allows more replicas to be hosted on a node than would otherwise happen because of a lack of disk space, by setting OptimizeForLowerDiskUsage to false the state information is written to
the log files more quickly.

The MaxRecordSizeInKB defines the maximum size of a record that can be written by the replicator into the log file. In most all cases the default 1024KB record size is optimal however if the service is
causing larger data items to be part of the state information then this value might need to be increased. There is little benefit in making the MaxRecordSizeInKB smaller than 1024 as smaller records
only use the space needed for the smaller record. It is expected that it would need to be changed in only rare cases.

The SharedLogId and SharedLogPath settings are always used together and allow a service to use a separate shared log from the default shared log for the node. For best efficiency, as many services as
possible should specify the same shared log. Shared log files should be placed on disks that are used solely for the shared log file so that head movement contention is reduced. It is expected that it would need to be changed in only rare cases.
