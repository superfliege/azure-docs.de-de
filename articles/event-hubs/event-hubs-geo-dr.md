---
title: Georedundante Notfallwiederherstellung in Azure Event Hubs | Microsoft-Dokumentation
description: "Verwenden von geografischen Regionen für das Failover und zum Durchführen der Notfallwiederherstellung in Azure Event Hubs"
services: event-hubs
documentationcenter: 
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: sethm
ms.openlocfilehash: 94c2782b3166fbc65ae755291a82a2a14556b96f
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="azure-event-hubs-geo-disaster-recovery-preview"></a>Georedundante Notfallwiederherstellung in Azure Event Hubs (Vorschau)

Wenn in regionalen Rechenzentren Ausfallzeiten auftreten, ist es wichtig, dass die Datenverarbeitung in einer anderen Region oder einem anderen Rechenzentrum fortgesetzt wird. Daher sind die *georedundante Notfallwiederherstellung* und die *Georeplikation* wichtige Funktionen für jedes Unternehmen. Azure Event Hubs unterstützt die georedundante Notfallwiederherstellung und die Georeplikation auf Namespaceebene. 

Die Funktion der georedundanten Notfallwiederherstellung von Azure Event Hubs ist eine Lösung zur Notfallwiederherstellung. Die Konzepte und der Workflow, die in diesem Artikel beschrieben werden, gelten für Notfallszenarien und nicht für vorübergehende Ausfälle.

Eine ausführliche Erläuterung der Notfallwiederherstellung in Microsoft Azure finden Sie in [diesem Artikel](/azure/architecture/resiliency/disaster-recovery-azure-applications). 

## <a name="terminology"></a>Terminologie

**Kopplung**: Der primäre Namespace wird als *aktiv* bezeichnet und empfängt Nachrichten. Der Failovernamespace ist *passiv* und empfängt keine Nachrichten. Die Metadaten zwischen beiden Namespaces sind synchronisiert, sodass beide ohne Änderungen am Anwendungscode nahtlos Nachrichten annehmen können. Das Einrichten der Konfiguration für die Notfallwiederherstellung zwischen der aktiven und der passiven Region wird als *Koppeln* der Regionen bezeichnet.

**Alias**: Ein Name für die Konfiguration einer Notfallwiederherstellung, die Sie einrichten. Der Alias stellt einen einzelnen, stabilen, vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) als Verbindungszeichenfolge bereit. Anwendungen verwenden diese Alias-Verbindungszeichenfolge, um eine Verbindung mit einem Namespace herzustellen.

**Metadaten**: Bezieht sich auf Event Hub-Namen, Consumergruppen, Partitionen, Durchsatzeinheiten, Entitäten und Eigenschaften, die dem Namespace zugeordnet sind.

## <a name="enable-geo-disaster-recovery"></a>Aktivieren der georedundanten Notfallwiederherstellung

Die georedundante Notfallwiederherstellung in Event Hubs wird in drei Schritten aktiviert: 

1. Erstellen Sie eine Geo-Kopplung, die eine Alias-Verbindungszeichenfolge erstellt und die Live-Replikation von Metadaten ermöglicht. 
2. Aktualisieren Sie die vorhandene Client-Verbindungszeichenfolge auf den Alias, der in Schritt 1 erstellt wurde.
3. Initiieren Sie das Failover: Die Geo-Kopplung wird unterbrochen, und der Alias verweist auf den sekundären Namespace als seinen neuen primären Namespace.

Die folgende Abbildung zeigt diesen Workflow:

![Ablauf der Geo-Kopplung][1] 

### <a name="step-1-create-a-geo-pairing"></a>Schritt 1: Erstellen einer Geo-Kopplung

Für das Erstellen einer Kopplung zwischen zwei Regionen benötigen Sie einen primären und einen sekundären Namespace. Anschließend erstellen Sie einen Alias, um die Geo-Kopplung zu bilden. Sobald die Namespaces mit einem Alias gekoppelt sind, werden die Metadaten regelmäßig in beide Namespaces repliziert. 

Dies wird im folgenden Code veranschaulicht:

```csharp
ArmDisasterRecovery adr = await client.DisasterRecoveryConfigs.CreateOrUpdateAsync(
                                    config.PrimaryResourceGroupName,
                                    config.PrimaryNamespace,
                                    config.Alias,
                                    new ArmDisasterRecovery(){ PartnerNamespace = config.SecondaryNamespace});
```

### <a name="step-2-update-existing-client-connection-strings"></a>Schritt 2: Aktualisieren der vorhandenen Client-Verbindungszeichenfolgen

Sobald die Geo-Kopplung abgeschlossen ist, müssen die Verbindungszeichenfolgen aktualisiert werden, die auf den primären Namespace verweisen, um auf die Alias-Verbindungszeichenfolge zu verweisen. Rufen Sie die Verbindungszeichenfolgen wie im folgenden Beispiel dargestellt ab:

```csharp
var accessKeys = await client.Namespaces.ListKeysAsync(config.PrimaryResourceGroupName,
                                                       config.PrimaryNamespace, "RootManageSharedAccessKey");
var aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
var aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;
```

### <a name="step-3-initiate-a-failover"></a>Schritt 3: Initiieren eines Failovers

Wenn ein Notfall eintritt oder Sie ein Failover auf den sekundären Namespace initiieren möchten, beginnen die Metadaten und Daten damit, in den sekundären Namespace zu fließen. Da die Anwendungen die Alias-Verbindungszeichenfolgen verwenden, ist keine weitere Aktion erforderlich, da diese automatisch aus den Event Hubs im sekundären Namespace lesen und in diese schreiben. 

Der folgende Code veranschaulicht, wie ein Failover ausgelöst wird:

```csharp
await client.DisasterRecoveryConfigs.FailOverAsync(config.SecondaryResourceGroupName,
                                                   config.SecondaryNamespace, config.Alias);
```

Sobald ein Failover abgeschlossen ist und Sie die Daten benötigen, die im primären Namespace vorhanden sind, müssen Sie eine explizite Verbindungszeichenfolge für die Event Hubs im primären Namespace verwenden, um die Daten zu extrahieren.

### <a name="other-operations-optional"></a>Andere Vorgänge (optional)

Sie können die Geo-Kopplung wie im folgenden Code dargestellt ebenfalls unterbrechen oder einen Alias löschen. Beachten Sie, dass Sie zunächst die Geo-Kopplung unterbrechen müssen, um eine Alias-Verbindungszeichenfolge zu löschen:

```csharp
// Break pairing
await client.DisasterRecoveryConfigs.BreakPairingAsync(config.PrimaryResourceGroupName,
                                                       config.PrimaryNamespace, config.Alias);

// Delete alias connection string
// Before the alias is deleted, pairing must be broken
await client.DisasterRecoveryConfigs.DeleteAsync(config.PrimaryResourceGroupName,
                                                 config.PrimaryNamespace, config.Alias);
```

## <a name="considerations-for-public-preview"></a>Überlegungen für die öffentliche Vorschauversion

Beachten Sie folgende Überlegungen für dieses Release:

1. Die Funktion für die georedundante Notfallwiederherstellung ist nur in den Regionen „USA, Norden-Mitte“ und „USA, Süden-Mitte“ verfügbar. 
2. Das Feature wird nur für neu erstellte Namespaces unterstützt.
3. Für die Vorschauversion ist nur die Replikation der Metadaten aktiviert. Tatsächliche Daten werden nicht repliziert.
4. Bei der Vorschauversion entstehen keine Kosten für das Aktivieren des Features. Allerdings fallen Kosten für die reservierten Durchsatzeinheiten der primären und sekundären Namespaces an.

## <a name="next-steps"></a>Nächste Schritte

* Im [Beispiel auf GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) wird ein einfacher Workflow erläutert, der eine Geo-Kopplung erstellt und ein Failover für ein Notfallwiederherstellungsszenario initiiert.
* Der [REST API-Verweis](/rest/api/eventhub/disasterrecoveryconfigs) beschreibt APIs für das Konfigurieren der georedundanten Notfallwiederherstellung.

Weitere Informationen zu Event Hubs erhalten Sie unter den folgenden Links:

* Erste Schritte mit einem [Event Hubs-Tutorial](event-hubs-dotnet-standard-getstarted-send.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
* [Sample applications that use Event Hubs (Beispielanwendung mit Verwendung von Event Hubs)](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/eh-geodr1.png

