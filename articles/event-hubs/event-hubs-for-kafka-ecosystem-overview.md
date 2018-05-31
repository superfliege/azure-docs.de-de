---
title: Azure Event Hubs für Kafka-Ökosysteme | Microsoft-Dokumentation
description: Übersicht und Einführung in Kafka-fähiges Azure Event Hubs
services: event-hubs
documentationcenter: .net
author: djrosanova
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 05/07/2018
ms.author: darosa
ms.openlocfilehash: 51e32737be4eaf3c61cdbe50b82a05c205800ac4
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077605"
---
# <a name="event-hubs-for-kafka-ecosystems"></a>Event Hubs für Kafka-Ökosysteme

Event Hubs für Kafka-Ökosysteme bietet einen Kafka-Endpunkt, der von Ihren vorhandenen Kafka-basierten Anwendungen als Alternative zum Betreiben eines eigenen Kafka-Clusters verwendet werden kann. Event Hubs für Kafka-Ökosysteme unterstützt [Apache Kafka 1.0](https://kafka.apache.org/10/documentation.html) und neuere Clientversionen und funktioniert mit Ihren vorhandenen Kafka-Anwendungen, einschließlich MirrorMaker. Ändern Sie Ihre Verbindungszeichenfolge, und streamen Sie Ereignisse von Ihren Anwendungen, die das Kafka-Protokoll verwenden, an Event Hubs.

## <a name="what-does-event-hubs-for-kafka-ecosystems-provide"></a>Was bietet Event Hubs für Kafka-Ökosysteme?

Event Hubs für Kafka-Ökosysteme bietet einen auf Azure Event Hubs aufbauenden Protokollheader, der binärkompatibel mit Kafka ab Version 1.0 ist – und zwar sowohl zum Lesen aus Kafka-Themen als auch zum Schreiben in diese. Konzeptionell sind Kafka und Event Hubs nahezu identisch: Es handelt sich bei beiden um partitionierte Protokolle für das Streaming von Daten. In der folgenden Tabelle werden die Konzepte von Kafka und Event Hubs einander zugeordnet.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Konzeptionelle Zuordnung zwischen Kafka und Event Hubs

| Kafka-Konzept | Event Hubs-Konzept|
| --- | --- |
| Cluster | Namespace |
| Thema | Event Hubs |
| Partition | Partition|
| Consumergruppe | Consumergruppe |
| Offset | Offset|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Hauptunterschiede zwischen Kafka und Event Hubs

Während es sich bei [Apache Kafka](https://kafka.apache.org/) um Software handelt, die Sie an jedem Speicherort ausführen können, ist Event Hubs ein Clouddienst ähnlich Azure Blob Storage. Es sind keine Server oder Netzwerke zu verwalten und keine Broker zu konfigurieren. Sie erstellen einen Namespace (einen FQDN, in dem sich Ihre Themen befinden) und erstellen Event Hubs oder Themen in diesem Namespace. Weitere Informationen zur Verwendung von Event Hubs und Namespaces finden Sie unter [Was ist Event Hubs](event-hubs-what-is-event-hubs.md). Als Clouddienst verwendet Event Hubs eine einzelne stabile virtuelle IP-Adresse als Endpunkt, damit Clients keine Informationen über die Broker oder die Computer in einem Cluster benötigen. 

Die Skalierung in Event Hubs wird dadurch gesteuert, wie viele Durchsatzeinheiten Sie erwerben. Dabei berechtigt jede Durchsatzeinheit Sie zu 1 MB pro Sekunde oder 1.000 eingehenden Ereignissen pro Sekunde. Standardmäßig wird Event Hubs mit dem Feature [Automatische Vergrößerung](event-hubs-auto-inflate.md) über Durchsatzeinheiten zentral hochskaliert, wenn Sie Ihr Limit erreichen. Dieses Feature funktioniert auch mit Event Hubs für Kafka-Ökosysteme. 

### <a name="security-and-authentication"></a>Sicherheit und Authentifizierung

Azure Event Hubs erfordert SSL oder TLS für sämtliche Kommunikation und verwendet Shared Access Signatures (SAS) für die Authentifizierung. Diese Anforderung gilt auch für einen Kafka-Endpunkt in Event Hubs. Aus Gründen der Kompatibilität mit Kafka verwendet Event Hubs SASL PLAIN für die Authentifizierung und SASL SSL für die Transportsicherheit. Weitere Informationen zur Sicherheit in Event Hubs finden Sie unter [Event Hubs – Authentifizierung und Sicherheit](event-hubs-authentication-and-security-model-overview.md).

## <a name="other-event-hubs-features-available-for-kafka"></a>Andere verfügbare Event Hubs-Features für Kafka

Event Hubs für Kafka-Ökosysteme ermöglicht Ihnen, mit einem Protokoll zu schreiben und mit einem anderen zu lesen, sodass Ihre aktuellen Kafka-Produzenten weiterhin über Kafka veröffentlichen können. Sie können außerdem Leser mit Event Hubs hinzufügen, z.B. Azure Stream Analytics oder Azure Functions. Darüber hinaus funktionieren Event Hubs-Features wie das [Erfassen](event-hubs-capture-overview.md) und die [geografische Notfallwiederherstellung](event-hubs-geo-dr.md) auch mit Event Hubs für Kafka-Ökosysteme.

## <a name="features-that-are-not-supported-in-the-preview"></a>Features, die in der Vorschauversion nicht unterstützt werden

In der Public Preview der Integration von Event Hubs für Kafka-Ökosysteme werden die folgenden Kafka-Features nicht unterstützt:

*   Idempotenter Produzent
*   Transaktion
*   Komprimierung
*   Größenbasierte Aufbewahrung
*   Protokollkomprimierung
*   Hinzufügen von Partitionen zu einem vorhandenen Thema
*   Unterstützung der HTTP-Kafka-API
*   Kafka Connect
*   Kafka Streams

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde eine Einführung in Event Hubs für Kafka-Ökosysteme vermittelt. Weitere Informationen finden Sie unter den folgenden Links:

* [Erstellen von Kafka-fähigen Event Hubs](event-hubs-create-kafka-enabled.md)
* [Streamen von Ereignissen in Event Hubs aus Kafka-Anwendungen](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* Erste Schritte mit einem [Event Hubs-Tutorial](event-hubs-dotnet-standard-getstarted-send.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

 
 

