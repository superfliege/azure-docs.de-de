---
title: Georedundante Notfallwiederherstellung in Azure Service Bus | Microsoft-Dokumentation
description: "Verwenden von geografischen Regionen für das Failover und zum Durchführen der Notfallwiederherstellung in Azure Service Bus"
services: service-bus-messaging
documentationcenter: 
author: christianwolf42
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: sethm
ms.openlocfilehash: 2c509b56282ace92e536dc85f1a28f83a4701940
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2017
---
# <a name="azure-service-bus-geo-disaster-recovery-preview"></a>Georedundante Notfallwiederherstellung in Azure Service Bus (Vorschau)

Wenn in regionalen Rechenzentren Ausfallzeiten auftreten, ist es wichtig, dass die Datenverarbeitung in einer anderen Region oder einem anderen Rechenzentrum fortgesetzt wird. Daher sind die *georedundante Notfallwiederherstellung* und die *Georeplikation* wichtige Funktionen für jedes Unternehmen. Azure Service Bus unterstützt die georedundante Notfallwiederherstellung und die Georeplikation auf Namespaceebene. 

Die Vorschauversion der georedundanten Notfallwiederherstellung ist derzeit nur in zwei Regionen verfügbar: **USA, Norden-Mitte** und **USA, Süden-Mitte**.

## <a name="outages-and-disasters"></a>Ausfälle und Notfälle

Im Artikel [Bewährte Methoden zum Schützen von Anwendungen vor Service Bus-Ausfällen und Notfällen](service-bus-outages-disasters.md) wird zwischen „Ausfällen“ und „Notfällen“ unterschieden. Dieser Unterschied muss beachtet werden. Ein *Ausfall* ist die vorübergehende Nichtverfügbarkeit von Azure Service Bus und kann einige Komponenten des Diensts, z.B. einen Nachrichtenspeicher, oder selbst das gesamte Rechenzentrum betreffen. Nachdem das Problem behoben wurde, ist Service Bus jedoch wieder verfügbar. In der Regel führt ein Ausfall nicht zum Verlust von Nachrichten oder anderen Daten. Ein Beispiel für einen Ausfall ist ein Stromausfall im Rechenzentrum.

Ein *Notfall* wird als dauerhafter Verlust einer Service Bus-[Skalierungseinheit](service-bus-architecture.md#service-bus-scale-units) oder eines Rechenzentrums definiert. Das Rechenzentrum kann danach wieder zur Verfügung stehen oder für Stunden oder Tage ausfallen. Häufig gehen bei einem Notfall einige oder alle Nachrichten oder anderen Daten verloren. Beispiele für Notfälle sind Feuer, Überflutung und Erdbeben.

Die Funktion der georedundanten Notfallwiederherstellung von Azure Service Bus ist eine Lösung zur Notfallwiederherstellung. Die Konzepte und der Workflow, die in diesem Artikel beschrieben werden, gelten für Notfallszenarien und nicht für vorübergehende Ausfälle.  

## <a name="basic-concepts-and-terms"></a>Allgemeine Konzepte und Begriffe

Die Funktion zur Notfallwiederherstellung implementiert die Notfallwiederherstellung von Metadaten und basiert auf einem primären und einem sekundären Namespace zur Notfallwiederherstellung. Beachten Sie, dass die Funktion zur georedundanten Notfallwiederherstellung nur für [Premium-Namespaces](service-bus-premium-messaging.md) verfügbar ist. Sie müssen keine Änderungen an den Verbindungszeichenfolgen vornehmen, da die Verbindung über einen Alias hergestellt wird.

In diesem Artikel werden die folgenden Begriffe verwendet:

-  *Alias:* Die Hauptverbindungszeichenfolge.

-  *Primärer/sekundärer Namespace:* Beschreibt die Namespaces, die dem Alias entsprechen. Der primäre Namespace ist „aktiv“ und empfängt Nachrichten, der sekundäre Namespace ist „passiv“ und empfängt keine Nachrichten. Die Metadaten zwischen beiden Namespaces sind synchronisiert, sodass beide ohne Änderungen am Anwendungscode nahtlos Nachrichten annehmen können.

-  *Metadaten:* Ihre Darstellung von Objekten in Azure Service Bus. Derzeit werden nur Metadaten unterstützt.

-  *Failover:* Der Vorgang zum Aktivieren des sekundären Namespace. Sie müssen Nachrichten aus dem ehemals primären Namespace pullen, nachdem dieser wieder verfügbar geworden ist, und den Namespace dann löschen. Zum Erstellen eines weiteren Failovers fügen Sie der Kopplung einen neuen sekundären Namespace hinzu.

## <a name="failover-workflow"></a>Failoverworkflow

Der folgende Abschnitt bietet angefangen beim Einrichten des ersten Failovers eine Übersicht über den gesamten Failoverprozess.

![1][]

Sie richten zunächst einen primären und einen sekundären Namespace ein und erstellen dann eine Kopplung. Über diese Kopplung erhalten Sie einen Alias, mit dem Sie eine Verbindung herstellen können. Da Sie einen Alias verwenden, müssen Sie keine Verbindungszeichenfolgen ändern. Nur neue Namespaces können der Failoverkopplung hinzugefügt werden. Schließlich müssen Sie Triggerlogik hinzufügen (z.B. Geschäftslogik, die erkennt, wenn der Namespace nicht verfügbar ist, und das Failover initiiert). Mit der Service Bus-Funktion für die [Nachrichtensuche](message-browsing.md) können Sie die Namespaceverfügbarkeit überprüfen.

Nachdem Sie die Überwachung und die Notfallwiederherstellung eingerichtet haben, können Sie mit dem Failoverprozess fortfahren. Wenn der Trigger ein Failover initiiert oder Sie das Failover manuell initiieren, sind zwei Schritte erforderlich:

1. Bei einem weiteren Ausfall soll das Failover wieder ausgeführt werden können. Richten Sie daher einen zweiten passiven Namespace ein, und aktualisieren Sie die Kopplung. 
2. Nachdem der neue Namespace verfügbar ist, können Nachrichten aus dem ehemals primären Namespace gepullt werden. Anschließend können Sie den alten primären Namespace wiederverwenden oder löschen.

![2][]

## <a name="set-up-disaster-recovery"></a>Einrichten der Notfallwiederherstellung

In diesem Abschnitt wird beschrieben, wie Sie Ihren eigenen Code für die georedundante Notfallwiederherstellung in Service Bus erstellen. Dazu benötigen Sie zwei Namespaces an unabhängigen Standorten, z.B. „USA, Süden-Mitte“ und „USA, Norden-Mitte“. Im folgenden Beispiel wird Visual Studio 2017 verwendet.

1.  Erstellen Sie ein neues Projekt vom Typ **Konsolen-App (.NET Framework)** in Visual Studio, und geben Sie einen Namen an, z.B. **SBGeoDR**.

2.  Installieren Sie die folgenden NuGet-Pakete:
    1.  Microsoft.IdentityModel.Clients.ActiveDirectory
    2.  Microsoft.Azure.Management.ServiceBus

3. Stellen Sie sicher, dass Version 10.0.3 des Newtonsoft.Json-NuGet-Pakets verwendet wird.

3.  Fügen Sie im Code die folgenden `using`-Anweisungen ein:

    ```csharp
    using System.Threading;
    using Microsoft.Azure.Management.ServiceBus;
    using Microsoft.Azure.Management.ServiceBus.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

4. Ändern Sie die `main()`-Methode, um zwei Premium-Namespaces hinzuzufügen:

    ```csharp
    // 1. Create primary namespace (optional).

    var namespaceParams = new SBNamespace()
    {
        Location = "South Central US",
        Sku = new SBSku()
        {
            Name = SkuName.Premium,
            Capacity = 1
        }

    };

    var namespace1 = client.Namespaces.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, namespaceParams);

    // 2. Create secondary namespace (optional if you already have an empty namespace available).

    var namespaceParams2 = new SBNamespace()
    {
        Location = "North Central US",
        Sku = new SBSku()
        {
            Name = SkuName.Premium,
            Capacity = 1
        }

    };

    // If you re-run this program while namespaces are still paired this operation will fail with a bad request.
    // This is because we block all updates on secondary namespaces once it is paired.

    var namespace2 = client.Namespaces.CreateOrUpdate(resourceGroupName, geoDRSecondaryNS, namespaceParams2);
    ```

5. Aktivieren Sie die Kopplung zwischen den beiden Namespaces, und rufen Sie den Alias ab, den Sie später zum Herstellen der Verbindung mit den Entitäten verwenden:

    ```csharp
    // 3. Pair the namespaces to enable DR.

    ArmDisasterRecovery drStatus = client.DisasterRecoveryConfigs.CreateOrUpdate(
        resourceGroupName,
        geoDRPrimaryNS,
        alias,
        new ArmDisasterRecovery { PartnerNamespace = geoDRSecondaryNS });

    // A similar loop can be used to check if other operations (Failover, BreakPairing, Delete) 
    // mentioned below have been successful.
    while (drStatus.ProvisioningState != ProvisioningStateDR.Succeeded)
    {
        Console.WriteLine("Waiting for DR to be set up. Current state: " +
        drStatus.ProvisioningState);
        drStatus = client.DisasterRecoveryConfigs.Get(
        resourceGroupName,
        geoDRPrimaryNS,
        alias);

        Thread.CurrentThread.Join(TimeSpan.FromSeconds(30));
    }
    ```

Sie haben erfolgreich zwei gekoppelte Namespaces eingerichtet. Nun können Sie Entitäten zum Überwachen der Metadatensynchronisierung erstellen. Wenn Sie direkt danach ein Failover ausführen möchten, sollten Sie einige Zeit einrechnen, bis die Metadaten synchronisiert sind. Sie können eine kurze Ruhezeit hinzufügen, z.B.:

```csharp
client.Topics.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, "myTopic", new SBTopic());
client.Subscriptions.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, "myTopic", "myTopic-Sub1", new SBSubscription());

// sleeping to allow metadata to sync across primary and secondary
Thread.Sleep(1000 * 60);
```

Nun können Sie Entitäten über das Portal oder über Azure Resource Manager hinzufügen und anzeigen, wie sie synchronisiert werden. Sofern Sie das Failover nicht manuell ausführen möchten, sollten Sie eine App erstellen, die den primären Namespace überwacht und ein Failover initiiert, wenn er nicht mehr verfügbar ist. 

## <a name="initiate-a-failover"></a>Initiieren eines Failovers

Mit dem folgenden Code wird veranschaulicht, wie ein Failover initiiert wird:

```csharp
// Note that this failover operation is always run against the secondary namespace 
// (because primary might be down at time of failover).

client.DisasterRecoveryConfigs.FailOver(resourceGroupName, geoDRSecondaryNS, alias);
```

Fügen Sie nach dem Auslösen des Failovers einen neuen passiven Namespace hinzu, und erstellen Sie die Kopplung neu. Den Code zum Erstellen einer neuen Kopplung finden Sie im vorherigen Abschnitt. Zudem müssen Sie die Nachrichten aus dem alten primären Namespace entfernen, nachdem das Failover abgeschlossen ist. Beispiele zum Empfangen von Nachrichten aus einer Warteschlange finden Sie unter [Erste Schritte mit Warteschlangen](service-bus-dotnet-get-started-with-queues.md).

## <a name="how-to-disable-geo-disaster-recovery"></a>Deaktivieren der georedundanten Notfallwiederherstellung

Der folgende Code veranschaulicht, wie eine Namespacekopplung deaktiviert wird:

```csharp
client.DisasterRecoveryConfigs.BreakPairing(resourceGroupName, geoDRPrimaryNS, alias);
```

Mit dem folgenden Code wird der erstellte Alias gelöscht:

```csharp
// Delete the DR config (alias).
// Note that this operation must run against the namespace to which the alias is currently pointing.
// If you break the pairing and want to delete the namespaces afterwards, you must delete the alias first.

client.DisasterRecoveryConfigs.Delete(resourceGroupName, geoDRPrimaryNS, alias);
```

## <a name="steps-after-a-failover-failback"></a>Schritte nach einem Failover (Failback)

Führen Sie nach einem Failover die folgenden beiden Schritte aus:

1.  Erstellen Sie einen neuen passiven sekundären Namespace. Den entsprechenden Code finden Sie im vorherigen Abschnitt.
2.  Entfernen Sie die verbleibenden Nachrichten aus der Warteschlange.

## <a name="alias-connection-string-and-test-code"></a>Alias-Verbindungszeichenfolge und Testcode

Wenn Sie den Failoverprozess testen möchten, können Sie eine Beispielanwendung schreiben, die Nachrichten über den Alias an den primären Namespace pusht. Stellen Sie dazu sicher, dass Sie die Alias-Verbindungszeichenfolge von einem aktiven Namespace abrufen. Die aktuelle Vorschauversion enthält keine andere Schnittstelle für den direkten Abruf der Verbindungszeichenfolge. Mit dem folgenden Beispielcode wird vor und nach dem Failover eine Verbindung hergestellt:

```csharp
var accessKeys = client.Namespaces.ListKeys(resourceGroupName, geoDRPrimaryNS, "RootManageSharedAccessKey");
var aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
var aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;

if(aliasPrimaryConnectionString == null)
{
    accessKeys = client.Namespaces.ListKeys(resourceGroupName, geoDRSecondaryNS, "RootManageSharedAccessKey");
    aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
    aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Hier](/rest/api/servicebus/disasterrecoveryconfigs) finden Sie Informationen zur REST-API-Referenz für die georedundante Notfallwiederherstellung.
- Führen Sie das [Beispiel auf GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2) zur georedundanten Notfallwiederherstellung aus.
- Sehen Sie sich das [Beispiel zur georedundanten Notfallwiederherstellung, mit dem Nachrichten an einen Alias gesendet werden](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) an.

Weitere Informationen zum Service Bus-Messaging finden Sie in folgenden Artikeln:

* [Service Bus – Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [REST-API](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geo1.png
[2]: ./media/service-bus-geo-dr/geo2.png
