---
title: Georedundante Notfallwiederherstellung in Azure Service Bus | Microsoft-Dokumentation
description: Verwenden von geografischen Regionen für das Failover und zum Durchführen der Notfallwiederherstellung in Azure Service Bus
services: service-bus-messaging
documentationcenter: ''
author: christianwolf42
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: sethm
ms.openlocfilehash: 652adcf78add8ae699a7f827a915e90ce1694c61
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
ms.locfileid: "30237344"
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Georedundante Notfallwiederherstellung in Azure Service Bus

Falls gesamte Azure-Regionen oder -Datencenter ausfallen (wenn keine [Verfügbarkeitszonen](../availability-zones/az-overview.md) verwendet werden), ist es von entscheidender Bedeutung, dass die Datenverarbeitung in einer anderen Region oder in einem anderen Datencenter fortgesetzt werden kann. Daher sind die *georedundante Notfallwiederherstellung* und die *Georeplikation* wichtige Funktionen für jedes Unternehmen. Azure Service Bus unterstützt die georedundante Notfallwiederherstellung und die Georeplikation auf Namespaceebene. 

Das Feature für die georedundante Notfallwiederherstellung ist für die Service Bus Premium-SKU global verfügbar. 

## <a name="outages-and-disasters"></a>Ausfälle und Notfälle

Es ist wichtig, dass Sie den Unterschied zwischen „Ausfällen“ und „Notfällen“ kennen. Ein *Ausfall* ist die vorübergehende Nichtverfügbarkeit von Azure Service Bus und kann einige Komponenten des Diensts, z.B. einen Nachrichtenspeicher, oder selbst das gesamte Rechenzentrum betreffen. Nachdem das Problem behoben wurde, ist Service Bus aber wieder verfügbar. In der Regel führt ein Ausfall nicht zum Verlust von Nachrichten oder anderen Daten. Ein Beispiel für einen Ausfall ist ein Stromausfall im Rechenzentrum. Bei einigen Ausfällen handelt es sich nur um kurze Verbindungsunterbrechungen aufgrund von vorübergehenden Problemen bzw. Netzwerkproblemen. 

Ein *Notfall* ist als dauerhafter oder längerer Ausfall eines Service Bus-Clusters, einer Azure-Region oder eines Datencenters definiert. Die Region oder das Datencenter wird ggf. wieder verfügbar, kann aber auch für mehrere Stunden oder Tage ausfallen. Beispiele für Notfälle sind Feuer, Überflutung und Erdbeben. Ein Notfall, der dauerhaft wird, kann zum Verlust von Nachrichten, Ereignissen oder anderen Daten führen. In den meisten Fällen kommt es allerdings zu keinem Datenverlust, und Nachrichten können wiederhergestellt werden, sobald das Rechenzentrum gesichert ist.

Die Funktion der georedundanten Notfallwiederherstellung von Azure Service Bus ist eine Lösung zur Notfallwiederherstellung. Die Konzepte und der Workflow, die in diesem Artikel beschrieben werden, gelten für Notfallszenarien und nicht für vorübergehende Ausfälle. Eine ausführliche Erläuterung der Notfallwiederherstellung in Microsoft Azure finden Sie in [diesem Artikel](/azure/architecture/resiliency/disaster-recovery-azure-applications).   

## <a name="basic-concepts-and-terms"></a>Allgemeine Konzepte und Begriffe

Bei der Funktion zur Notfallwiederherstellung wird die Notfallwiederherstellung von Metadaten implementiert, und sie basiert auf speziellen primären und sekundären Namespaces. Beachten Sie, dass die Funktion zur georedundanten Notfallwiederherstellung nur für die [Premium-SKU](service-bus-premium-messaging.md) verfügbar ist. Sie müssen keine Änderungen an den Verbindungszeichenfolgen vornehmen, da die Verbindung über einen Alias hergestellt wird.

In diesem Artikel werden die folgenden Begriffe verwendet:

-  *Alias*: Der Name für die Konfiguration einer von Ihnen eingerichteten Notfallwiederherstellung. Der Alias stellt einen einzelnen, stabilen, vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) als Verbindungszeichenfolge bereit. Anwendungen verwenden diese Alias-Verbindungszeichenfolge, um eine Verbindung mit einem Namespace herzustellen. 

-  *Primärer/sekundärer Namespace:* Die Namespaces, die dem Alias entsprechen. Der primäre Namespace ist aktiv und empfängt Nachrichten. (Dies kann ein bereits vorhandener oder ein neuer Namespace sein.) Der sekundäre Namespace ist passiv und empfängt keine Nachrichten. Die Metadaten zwischen beiden Namespaces werden synchronisiert, sodass beide nahtlos und ohne Änderung des Anwendungscodes oder der Verbindungszeichenfolge Nachrichten entgegennehmen können. Sie müssen den Alias verwenden, um sicherzustellen, dass nur der aktive Namespace Nachrichten empfängt. 

-  *Metadaten:* Entitäten, z.B. Warteschlangen, Themen und Abonnements und die dazugehörigen Eigenschaften des Diensts, die dem Namespace zugeordnet sind. Beachten Sie, dass nur Entitäten und ihre Einstellungen automatisch repliziert werden. Nachrichten werden nicht repliziert. 

-  *Failover:* Der Vorgang zum Aktivieren des sekundären Namespace.

## <a name="setup-and-failover-flow"></a>Setup und Failoverablauf

Im folgenden Abschnitt finden Sie eine Übersicht über den Failoverprozess und erfahren, wie Sie das erste Failover einrichten. 

![1][]

### <a name="setup"></a>Einrichtung

Zuerst erstellen bzw. verwenden Sie einen vorhandenen primären Namespace und einen neuen sekundären Namespace und koppeln diese anschließend. Über diese Kopplung erhalten Sie einen Alias, mit dem Sie eine Verbindung herstellen können. Da Sie einen Alias verwenden, müssen Sie keine Verbindungszeichenfolgen ändern. Nur neue Namespaces können der Failoverkopplung hinzugefügt werden. Abschließend sollten Sie einige Überwachungsfunktionen hinzufügen, um zu ermitteln, ob ein Failover erforderlich ist. In den meisten Fällen ist der Dienst Teil eines großen Ökosystems, sodass automatische Failover selten möglich sind, da Failover sehr häufig synchron mit dem restlichen Subsystem oder der Infrastruktur durchgeführt werden müssen.

### <a name="example"></a>Beispiel

In einem Beispiel für dieses Szenario geht es um eine POS-Lösung (Point of Sale), die entweder Nachrichten oder Ereignisse ausgibt. Service Bus übergibt diese Ereignisse an eine Lösung für die Zuordnung oder Neuformatierung, von der dann zugeordnete Daten zur weiteren Verarbeitung an ein anderes System weitergeleitet werden. An diesem Punkt werden all diese Systeme ggf. in derselben Azure-Region gehostet. Die Entscheidung darüber, wann für welchen Teil ein Failover durchgeführt wird, richtet sich nach dem Datenfluss in Ihrer Infrastruktur. 

Sie können das Failover entweder mit Überwachungssystemen oder mit benutzerdefinierten Überwachungslösungen automatisieren. Für diese Art der Automatisierung sind aber zusätzliche Planungs- und Arbeitsschritte erforderlich, was den Rahmen dieses Artikels sprengen würde.

### <a name="failover-flow"></a>Failoverablauf

Im Zuge der Initiierung des Failovers müssen zwei Schritte ausgeführt werden:

1. Bei einem weiteren Ausfall muss ein erneutes Failover möglich sein. Richten Sie daher einen weiteren passiven Namespace ein, und aktualisieren Sie die Kopplung. 

2. Übertragen Sie Nachrichten mithilfe von Pull aus dem primären Namespace, nachdem er wieder verfügbar ist. Verwenden Sie diesen Namespace danach für das reguläre Messaging außerhalb Ihrer eingerichteten geografischen Wiederherstellung, oder löschen Sie den alten primären Namespace.

> [!NOTE]
> Es wird nur die Semantik für „Fail Forward“ unterstützt. In diesem Szenario führen Sie das Failover und anschließend die Reparatur mit einem neuen Namespace durch. Ein Failback wird nicht unterstützt, z.B. in einem SQL-Cluster. 

![2][]

## <a name="management"></a>Verwaltung

Falls Sie einen Fehler gemacht haben (z.B. eine Kopplung der falschen Regionen während des anfänglichen Setups), können Sie die Kopplung der beiden Namespaces jederzeit trennen. Löschen Sie den Alias, falls Sie die gekoppelten Namespaces als reguläre Namespaces verwenden möchten.

## <a name="use-existing-namespace-as-alias"></a>Verwenden des vorhandenen Namespace als Alias

Bei einem Szenario, für das Sie die Verbindungen von Producern und Consumern nicht ändern können, können Sie Ihren Namespacenamen als Aliasnamen verwenden. Den Beispielcode finden Sie auf [GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name).

## <a name="samples"></a>Beispiele

In den [Beispielen auf GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/) wird veranschaulicht, wie Sie ein Failover einrichten und initiieren. In diesen Beispielen werden die folgenden Konzepte veranschaulicht:

- Ein .NET-Beispiel und Einstellungen, die in Azure Active Directory erforderlich sind, um unter Verwendung von Azure Resource Manager und Service Bus eine georedundante Notfallwiederherstellung einzurichten und zu aktivieren
- Schritte, die zum Ausführen des Beispielcodes erforderlich sind
- Verwenden eines vorhandenen Namespace als Alias
- Schritte zur alternativen Aktivierung der georedundanten Notfallwiederherstellung über PowerShell oder die Befehlszeilenschnittstelle
- [Senden und Empfangen](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) aus dem aktuellen primären oder sekundären Namespace unter Verwendung des Alias

## <a name="considerations"></a>Überlegungen

Beachten Sie für diesen Release Folgendes:

1. Berücksichtigen Sie bei der Failoverplanung auch den Zeitfaktor. Falls beispielsweise länger als 15 bis 20 Minuten keine Konnektivität vorhanden ist, empfiehlt es sich unter Umständen, das Failover zu initiieren. 
 
2. Die Tatsache, dass keine Daten repliziert werden, bedeutet, dass derzeit keine aktiven Sitzungen repliziert werden. Außerdem kann es sein, dass die Duplikaterkennung und geplante Nachrichten nicht funktionieren. Neue Sitzungen, neue geplante Nachrichten und neue Duplikate funktionieren. 

3. Die Durchführung eines Failovers für eine komplexe verteilte Infrastruktur sollte mindestens einmal [durchgespielt](/azure/architecture/resiliency/disaster-recovery-azure-applications#disaster-simulation) werden. 

4. Das Synchronisieren von Entitäten kann einige Zeit dauern, z.B. eine Minute für 50 bis 100 Entitäten. Abonnements und Regeln zählen ebenfalls zu den Entitäten. 

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
