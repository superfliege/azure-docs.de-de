---
title: Implementieren von Features in Azure Service Fabric-Actors | Microsoft-Dokumentation
description: Beschreibt, wie Sie einen eigenen Actordienst schreiben, der Funktionen auf Dienstebene in gleicher Weise implementiert wie bei der Vererbung von StatefulService.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 6aff9e9599d31942f994f3cb4e5e9219f33dc7e1
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205519"
---
# <a name="implementing-service-level-features-in-your-actor-service"></a>Implementieren von Features auf Dienstebene in Ihren Actordienst
Wie in [Dienstebenen](service-fabric-reliable-actors-platform.md#service-layering) beschrieben, ist der Actordienst selbst ein zuverlässiger Dienst.  Sie können einen eigenen Dienst schreiben, der von `ActorService` abgeleitet ist und Funktionen auf Dienstebene auf die gleiche Weise implementiert wie bei der Vererbung von StatefulService. Beispiel:

- Dienstsicherung und -wiederherstellung
- Gemeinsame Verwendung von Funktionen für alle Akteure, z. B. ein Schaltkreisunterbrecher
- Remoteprozeduraufrufe für den Akteurdienst selbst sowie für einzelne Akteure

## <a name="using-the-actor-service"></a>Verwenden des Akteurdiensts
Akteurinstanzen haben Zugriff auf den Akteurdienst, in dem sie ausgeführt werden. Über den Akteurdienst können Akteurinstanzen den Dienstkontext programmgesteuert abrufen. Der Dienstkontext enthält die Partitions-ID, den Dienstnamen, den Anwendungsnamen und andere spezifische Informationen für die Service Fabric-Plattform:

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```

Wie alle Reliable Services muss der Akteurdienst mit einem Diensttyp in der Service Fabric-Laufzeit registriert werden. Damit der Akteurdienst Ihre Akteurinstanzen ausführen kann, muss auch Ihr Akteurtyp beim Akteurdienst registriert werden. Die `ActorRuntime` -Registrierungsmethode führt dies für Actors aus. Im einfachsten Fall müssen Sie nur den Akteurtyp registrieren, der Akteurdienst wird dann implizit mit den Standardeinstellungen verwendet:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

Sie können auch ein Lambda verwenden, das von der Registrierungsmethode bereitgestellt wird, um selbst den Akteurdienst zu erstellen. So können Sie dann den Akteurdienst konfigurieren und explizit Akteurinstanzen erstellen, in die Sie Abhängigkeiten zu Ihrem Akteur über den Konstruktor einführen können:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

## <a name="actor-service-methods"></a>Akteurdienstmethoden
Der Akteurdienst implementiert `IActorService` (C#) oder `ActorService` (Java), womit wiederum `IService` (C#) oder `Service` (Java) implementiert wird. Dies ist die Schnittstelle, die für das Reliable Services-Remoting verwendet wird, das Remoteprozeduraufrufe von Dienstmethoden erlaubt. Sie enthält Methoden auf Dienstebene, die mit dem Dienstremoting aufgerufen werden können und Ihnen ermöglichen, Actors [aufzuzählen](service-fabric-reliable-actors-enumerate.md) und zu [löschen](service-fabric-reliable-actors-delete-actors.md).


## <a name="custom-actor-service"></a>Benutzerdefinierter Akteurdienst
Mit dem Lambda für die Akteurregistrierung können Sie einen eigenen benutzerdefinierten Akteurdienst registrieren, der von `ActorService` (C#) und `FabricActorService` (Java) abgeleitet wird. In diesem benutzerdefinierten Akteurdienst können Sie eigene Funktionen auf Dienstebene implementieren, indem Sie eine Dienstklasse erstellen, die `ActorService` (C#) oder `FabricActorService` (Java) erbt. Ein benutzerdefinierter Akteurdienst erbt die gesamte Laufzeitfunktionalität des Akteurs von `ActorService` (C#) oder `FabricActorService` (Java). Er kann zum Implementieren eigener Dienstmethoden verwendet werden.

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

## <a name="implementing-actor-backup-and-restore"></a>Implementieren der Sicherung und Wiederherstellung von Akteuren
Der benutzerdefinierte Actordienst kann eine Methode zum Sichern von Actordaten bereitstellen. Diese nutzt die Vorteile des Remoting-Listeners, der bereits im `ActorService` enthalten ist.  Ein Beispiel finden Sie unter [Implementieren von Sicherung und Wiederherstellung von Actors](service-fabric-reliable-actors-backup-and-restore.md).

## <a name="actor-using-remoting-v2interfacecompatible-stack"></a>Actor mit dem Remoting V2(InterfaceCompatible)-Stapel
Der Remoting V2(InterfaceCompatible)-Stapel (auch als „V2_1“ bezeichnet) verfügt über alle Features des Remoting V2-Stapels und ist darüber hinaus ein Stapel, dessen Schnittstelle mit dem Remoting V1-Stapel kompatibel ist, der allerdings nicht mit V2 und V1 abwärtskompatibel ist. Um ohne Beeinträchtigung der Dienstverfügbarkeit ein Upgrade von V1 auf V2_1 durchzuführen, führen Sie die Schritte im folgenden [Artikel](#actor-service-upgrade-to-remoting-v2interfacecompatible-stack-without-impacting-service-availability) durch.

Folgende Änderungen sind erforderlich, um den Remoting V2_1-Stapel verwenden zu können.
 1. Fügen Sie das folgende Assembly-Attribut zu Actor-Schnittstellen hinzu.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

 2. Erstellen und aktualisieren Sie ActorService- und Akteurclient-Projekte, sodass in ihnen der V2-Stapel verwendet wird.

#### <a name="actor-service-upgrade-to-remoting-v2interfacecompatible-stack-without-impacting-service-availability"></a>Upgrade für den Actordienst auf den Remoting V2(InterfaceCompatible)-Stapel ohne Beeinträchtigung der Dienstverfügbarkeit
Diese Änderung ist ein Upgrade in 2 Schritten. Führen Sie die Schritte in der Reihenfolge aus, in der sie aufgeführt sind.

1.  Fügen Sie das folgende Assembly-Attribut zu Actor-Schnittstellen hinzu. Mit diesem Attribut werden zwei Listener für ActorService gestartet, der Listener V1 (vorhanden) und V2_1. Aktualisieren Sie ActorService mit dieser Änderung.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
  ```

2. Aktualisieren Sie ActorClients (Akteurclients), nachdem Sie das vorgenannte Upgrade abgeschlossen haben.
Mit diesem Schritt wird sichergestellt, dass der Actorproxy den Remoting V2_1-Stapel verwendet.

3. Dieser Schritt ist optional. Ändern Sie das oben aufgeführte Attribut, um den V1-Listener zu entfernen.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
    ```

## <a name="actor-using-remoting-v2-stack"></a>Akteur (Actor), der den Remoting V2-Stapel verwendet
Ab dem 2.8-NuGet-Paket können Benutzer den Remoting V2-Stapel verwenden, der leistungsfähiger ist und Funktionalität wie etwa benutzerdefinierte Serialisierung bietet. Remoting V2 ist nicht abwärtskompatibel mit vorhandenen dem Remotingstapel (dieser wird jetzt als V1-Remotingstapel bezeichnet).

Folgende Änderungen sind erforderlich, um den Remoting V2-Stapel verwenden zu können.
 1. Fügen Sie das folgende Assembly-Attribut zu Actor-Schnittstellen hinzu.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

 2. Erstellen und aktualisieren Sie ActorService- und Akteurclient-Projekte, sodass in ihnen der V2-Stapel verwendet wird.

#### <a name="actor-service-upgrade-to-remoting-v2-stack-without-impacting-service-availability"></a>ActorService-Upgrade auf den Remoting V2-Stapel, ohne dass die Dienstverfügbarkeit beeinträchtigt wird
Diese Änderung ist ein Upgrade in 2 Schritten. Führen Sie die Schritte in der Reihenfolge aus, in der sie aufgeführt sind.

1.  Fügen Sie das folgende Assembly-Attribut zu Actor-Schnittstellen hinzu. Mit diesem Attribut werden zwei Listener für ActorService gestartet, V1- (vorhandener) und V2-Listener. Aktualisieren Sie ActorService mit dieser Änderung.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
  ```

2. Aktualisieren Sie ActorClients (Akteurclients), nachdem Sie das vorgenannte Upgrade abgeschlossen haben.
Mit diesem Schritt wird sichergestellt, dass der Akteur-Proxy den Remoting V2-Stapel verwendet.

3. Dieser Schritt ist optional. Ändern Sie das oben aufgeführte Attribut, um den V1-Listener zu entfernen.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
    ```

## <a name="next-steps"></a>Nächste Schritte
* [Actor-Zustandsverwaltung](service-fabric-reliable-actors-state-management.md)
* [Actor-Lebenszyklus und Garbage Collection](service-fabric-reliable-actors-lifecycle.md)
* [Actors API reference documentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET-Beispielcode](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-Beispielcode](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
