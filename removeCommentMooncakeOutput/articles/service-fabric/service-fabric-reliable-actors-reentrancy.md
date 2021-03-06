<properties
   pageTitle="Reliable Actors Reentrancy"
   description="Introduction to Reentrancy for Service Fabric Reliable Actors"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor="vturecek"/>

<tags
	ms.service="service-fabric"
	ms.date="11/14/2015"
	wacn.date=""/>


# Reliable Actor Reentrancy
Fabric Actors, by default, allow logical call context-based reentrancy. This allows for actors to be reentrant if they are in the same call context chain. For example if Actor A sends message to Actor B who sends message to Actor C. As part of the message processing if Actor C calls Actor A, the message is reentrant so will be allowed. Any other messages that are part of different call context will be blocked on Actor A until it completes processing.

Actors that want to disallow logical call context-based reentrancy can disable it by decorating the actor class with `ReentrantAttribute(ReentrancyMode.Disallowed)`.

```csharp
public enum ReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```

The following code shows actor class that sets the reentrancy mode to `ReentrancyMode.Disallowed`. In this case if an actor sends a reentrant message to another actor an exception of type `FabricException` will be thrown.

```csharp
[Reentrant(ReentrancyMode.Disallowed)]
class VoicemailBoxActor : StatefulActor<VoicemailBox>, IVoicemailBoxActor
{
    ...
}
```
