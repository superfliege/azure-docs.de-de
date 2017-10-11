---
title: Erstellen Ihrer ersten Azure akteurbasiertes-Microservice in c# | Microsoft Docs
description: "Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen, Debuggen und Bereitstellen einer einfachen akteurbasiertes-Diensts mithilfe von Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 3f447e049ccd33c77f422e8aa703ad6646f9ffa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-reliable-actors"></a>Erste Schritte mit Reliable Actors
> [!div class="op_single_selector"]
> * [C#-unter Windows](service-fabric-reliable-actors-get-started.md)
> * [Java unter Linux](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Dieser Artikel erläutert die Grundlagen der Azure Service Fabric Reliable Actors und führt Sie durch das Erstellen, Debuggen und Bereitstellen einer einfachen zuverlässige Akteurs-Anwendung in Visual Studio.

## <a name="installation-and-setup"></a>Installation und Einrichtung
Bevor Sie beginnen, stellen Sie sicher, dass Sie die Service Fabric-Entwicklungsumgebung, die auf dem Computer eingerichtet haben.
Wenn Sie ihn einrichten müssen, finden Sie ausführliche Anweisungen [zum Einrichten der Entwicklungsumgebung](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Grundlegende Konzepte
Um mit Reliable Actors beginnen, müssen Sie nur einige grundlegende Konzepte zu verstehen:

* **Akteur Dienst**. Reliable Actors werden in zuverlässige Dienste verpackt, die in der Service Fabric-Infrastruktur bereitgestellt werden kann. Akteur Instanzen werden in eine benannte Instanz aktiviert.
* **Registrierung der Akteur**. Als mit zuverlässige Dienste ein zuverlässige Akteur-Dienst mit der Service Fabric-Laufzeit registriert werden muss. Darüber hinaus muss der Akteurtyp mit der Laufzeit Akteur registriert werden.
* **Akteur Schnittstelle**. Der Akteur-Schnittstelle wird verwendet, um eine stark typisierte öffentliche Schnittstelle von einem Akteur zu definieren. In der Terminologie zuverlässige Akteur-Modell definiert die Akteur-Schnittstelle die Typen von Nachrichten, die der Akteur zu bringen und zu verarbeiten. Der Akteur-Schnittstelle wird von andere Akteure und -Clientanwendungen verwendet, um "(asynchron) Nachrichten an der Akteur senden". Reliable Actors können mehrere Schnittstellen implementieren.
* **ActorProxy Klasse**. Die ActorProxy-Klasse wird von Clientanwendungen verwendet, zum Aufrufen der Methoden, die über die Akteur-Schnittstelle verfügbar gemacht werden. Die ActorProxy-Klasse bietet zwei wichtige Funktionen:
  
  * Namensauflösung: Suchen den Akteur im Cluster (Suchen Sie den Knoten des Clusters, in dem sie gehostet wird) werden kann.
  * Fehlerbehandlung: kann Methodenaufrufe wiederholen und erneut den Speicherort der Akteur aufgelöst, after, z. B. ein Fehler auf, die den Akteur, der auf einen anderen Knoten im Cluster verschoben werden muss.

Die folgenden Regeln, die Akteur Schnittstellen betreffen, werden bspw.:

* Schnittstellenmethoden Akteur können nicht überladen werden.
* Akteur-Schnittstelle, die Methoden, nicht benötigen, Ref oder optionale Parameter.
* Generische Schnittstellen werden nicht unterstützt.

## <a name="create-a-new-project-in-visual-studio"></a>Erstellen eines neuen Projekts in Visual Studio
Starten Sie Visual Studio 2015 oder Visual Studio 2017 als Administrator, und erstellen Sie ein neues Service Fabric-Anwendungsprojekt:

![Service Fabric-Tools für Visual Studio – neues Projekt][1]

Im nächsten Dialogfeld können Sie den Typ des Projekts auswählen, die Sie erstellen möchten.

![Service Fabric-Projektvorlagen][5]

Für das Projekt "HelloWorld" ermöglicht die Verwendung des Reliable Actors für Service Fabric-Diensts.

Nachdem Sie die Projektmappe erstellt haben, sehen Sie die folgende Struktur:

![Service Fabric-Projektstruktur][2]

## <a name="reliable-actors-basic-building-blocks"></a>Zuverlässige Akteure grundlegenden Bausteine
Eine typische Reliable Actors Lösung besteht aus drei Projekte:

* **Das Anwendungsprojekt (MyActorApplication)**. Dies ist das Projekt, das alle Dienste, die für die Bereitstellung zusammen verpackt. Es enthält die *ApplicationManifest.xml* und PowerShell-Skripts zum Verwalten der Anwendung.
* **Die benutzeroberflächenprojekt (MyActor.Interfaces)**. Dies ist das Projekt, das die Schnittstellendefinition für den Akteur enthält. MyActor.Interfaces im Projekt können Sie die Schnittstellen definieren, die von den Akteuren in der Projektmappe verwendet wird. Die Akteur-Schnittstellen können in jedem Projekt mit den gewünschten Namen definiert werden, aber die Schnittstelle der Akteur-Vertrag definiert, der von der Implementierung Akteur gemeinsam verwendet wird, und die Clients, die den Akteur aufrufen, daher in der Regel ist es sinnvoll, ihn in einer Assembly zu definieren, ist unabhängig von der Implementierung Akteur und von mehreren anderen Projekten gemeinsam genutzt werden kann.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Das Dienstprojekt Akteur (MyActor)**. Dies ist das Projekt verwendet, um die Service Fabric-Dienst zu definieren, der den Akteur hosten soll. Es enthält die Implementierung des Darstellers. Eine Akteur-Implementierung ist eine Klasse, die von diesem Basistyp abgeleitet ist `Actor` und implementiert die Schnittstellen, die im Projekt MyActor.Interfaces definiert sind. Eine Akteursklasse muss einen Konstruktor, der akzeptiert auch implementieren eine `ActorService` Instanz und ein `ActorId` und übergibt sie an den Basistext `Actor` Klasse. Dies ermöglicht Konstruktor Abhängigkeitsinjektion Platform-Abhängigkeiten.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Der Akteur-Dienst muss zu einem Diensttyp in der Service Fabric Runtime registriert werden. Damit der Akteur-Dienst die Akteur-Instanzen ausführen muss der Akteurtyp auch mit dem Akteur-Dienst registriert sein. Die `ActorRuntime` Registrierungsmethode führt diese Arbeit für Akteure.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Wenn Sie, an einem neuen Projekt in Visual Studio beginnen, und Sie nur ein Akteur-Definition aufweisen, ist die Registrierung standardmäßig im Code enthalten, die Visual Studio generiert. Wenn Sie andere Akteure in den Dienst definieren, müssen Sie mithilfe der Akteur-Registrierung hinzufügen:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> Die Laufzeit Service Fabric Akteure gibt einige [Ereignisse und Leistungsindikatoren im Zusammenhang mit Akteur Methoden](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Sie sind hilfreich, in der Diagnose und Leistungsüberwachung.
> 
> 

## <a name="debugging"></a>Debuggen
Der Service Fabric-Tools für Visual Studio unterstützt das Debuggen auf dem lokalen Computer. Sie können eine Debugsitzung starten, indem Sie die F5-Taste drücken. Visual Studio erstellt (falls erforderlich) Pakete. Außerdem stellt die Anwendung auf dem lokalen Service Fabric-Cluster bereit und fügt den Debugger.

Während des Bereitstellungsprozesses sehen Sie den Fortschritt auf der **Ausgabe** Fenster.

![Service Fabric Debuggen Fenster "Ausgabe"][3]

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [wie Reliable Actors Service Fabric-Plattform verwenden](service-fabric-reliable-actors-platform.md).

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
