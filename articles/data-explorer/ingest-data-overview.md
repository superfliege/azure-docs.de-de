---
title: Datenerfassung im Azure-Daten-Explorer
description: Erfahren Sie mehr über die verschiedenen Möglichkeiten, wie Sie Daten im Azure-Daten-Explorer erfassen (laden) können.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f1df22c505bffdfaf60bf9c6eec3ad4e698fff02
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139525"
---
# <a name="azure-data-explorer-data-ingestion"></a>Datenerfassung im Azure-Daten-Explorer

Die Datenerfassung ist der Prozess, mit dem Datensätze aus einer oder mehreren Quellen geladen werden, um eine Tabelle im Azure-Daten-Explorer zu erstellen oder zu aktualisieren. Nach der Erfassung sind die Daten für Abfragen verfügbar. Das folgende Diagramm zeigt den vollständigen Ablauf für die Arbeit im Azure-Daten-Explorer, einschließlich der Datenerfassung **(2)**.

![Allgemeiner Datenfluss](media/ingest-data-overview/overall-data-flow.png)

Der Datenverwaltungsdienst von Azure-Daten-Explorer, der für die Datenerfassung zuständig ist, bietet die folgenden Funktionen:

1. **Datenabruf**: Abrufen von Daten aus externen Quellen (Event Hubs) oder Lesen von Erfassungsanforderungen aus einer Azure-Warteschlange.

1. **Batchverarbeitung**: Batchdaten, die zur gleichen Datenbank und Tabelle übertragen werden, um den Durchsatz der Datenerfassung zu optimieren.

1. **Validierung**: Vorläufige Validierung und Formatkonvertierung, falls erforderlich.

1. **Datenbearbeitung**: Abgleich des Schemas, Organisation, Indizierung, Kodierung und Komprimierung der Daten.

1. **Persistenzpunkt im Erfassungsablauf**: Verwalten der Erfassungslast der Engine und Verarbeiten von Wiederholungsversuche bei vorübergehenden Ausfällen.

1. **Committen der Datenerfassung**: Stellt die Daten für Abfragen zur Verfügung.

> [!NOTE]
> Die effektive Aufbewahrungsrichtlinie der erfassten Daten wird von der Aufbewahrungsrichtlinie der Datenbank abgeleitet. Weitere Informationen finden Sie in [Aufbewahrungsrichtlinie](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy). Für das Erfassen von Daten sind Berechtigungen für einen **Tabelleningestor** oder einen **Datenbankingestor** erforderlich.

## <a name="ingestion-methods"></a>Erfassungsmethoden

Azure-Daten-Explorer unterstützt mehrere Erfassungsmethoden mit jeweils eigenen Zielszenarien, Vor- und Nachteilen. Azure-Daten-Explorer bietet Konnektoren für gängige Dienste, programmgesteuerte Erfassung über SDKs und direkten Zugriff auf die Engine für Untersuchungszwecke.

### <a name="ingestion-using-connectors"></a>Erfassung mit Konnektoren

Der Azure-Daten-Explorer unterstützt derzeit den Event Hub-Konnektor, der mit dem Verwaltungsassistenten im Azure-Portal verwaltet werden kann. Weitere Informationen finden Sie in [Schnellstart: Erfassen von Daten aus Event Hub in Azure-Daten-Explorer](ingest-data-event-hub.md).

### <a name="programmatic-ingestion"></a>Programmgesteuerte Erfassung

Azure-Daten-Explorer bietet SDKs, die für Abfragen und die Datenerfassung verwendet werden können. Die programmgesteuerte Erfassung ist dafür optimiert, die Erfassungskosten (COGs) zu senken, indem Speichertransaktionen während und nach dem Erfassungsprozess minimiert werden.

**Verfügbaren SDKs und Open-Source-Projekte**:

Kusto bietet Client SDKs, die zum Erfassen und Abfragen von Daten mit folgenden Programmen genutzt werden können:

* [Python SDK](https://docs.microsoft.com/azure/kusto/api/python/kusto-python-client-library)

* [.NET SDK](https://docs.microsoft.com/azure/kusto/api/netfx/about-the-sdk)

* [Java SDK](https://docs.microsoft.com/azure/kusto/api/java/kusto-java-client-library)

* [Node SDK]

* [REST-API](https://docs.microsoft.com/azure/kusto/api/netfx/kusto-ingest-client-rest)

**Techniken der programmgesteuerten Erfassung**:

* Erfassen von Daten über den Datenverwaltungsdienst von Azure-Daten-Explorer (mit hohem Durchsatz und zuverlässige Erfassung)

  * [**Batch-Erfassung** ](https://docs.microsoft.com/azure/kusto/api/netfx/kusto-ingest-queued-ingest-sample) (bereitgestellt vom SDK): Der Client lädt die Daten auf den Azure Blob Storage hoch (der vom Datenverwaltungsdienst des Azure-Daten-Explorers angegeben wird) und sendet eine Benachrichtigung an eine Azure-Warteschlange. Dies ist die empfohlene Technik für die hochvolumige, zuverlässige und kostengünstige Datenerfassung.

* Erfassen von Daten direkt in die Azure-Daten-Explorer-Engine (am besten geeignet für Untersuchungen und Prototyping):

  * **Inline-Erfassung**: Der Steuerbefehl (.ingest inline) mit In-Band-Daten ist für Ad-hoc-Tests vorgesehen.

  * **Erfassen aus der Abfrage**: Der Steuerbefehl (.set,.set,.set-or-append,.set-or-replace), der auf Abfrageergebnisse verweist, wird zum Erzeugen von Berichten oder kleinen temporären Tabellen verwendet.

  * **Erfassen aus dem Speicher**:Der Steuerbefehl (.ingest in) mit extern gespeicherten Daten (z.B. Azure Blob Storage) ermöglicht eine effiziente Massenaufnahme von Daten.

**Latenz der verschiedenen Methoden**:

| Methode | Latency |
| --- | --- |
| **Inline-Erfassung** | Unmittelbar |
| **Erfassen aus der Abfrage** | Abfragezeit + Verarbeitungszeit |
| **Erfassung aus dem Speicher** | Downloadzeit + Verarbeitungszeit |
| **Erfassen aus der Warteschlange** | Batchverarbeitungszeit + Verarbeitungszeit |
| |

Verarbeitungszeit hängt von der Größe der Daten ab – in der Regel weniger als ein paar Sekunden. Die Batchverarbeitungszeit beträgt standardmäßig fünf Minuten.

## <a name="choosing-the-most-appropriate-ingestion-method"></a>Auswählen der optimalen Erfassungsmethode

Bevor Sie mit der Datenerfassung beginnen, sollten Sie sich folgende Fragen stellen.

* Wo befinden sich meine Daten? 
* In welchem Format liegen die Daten vor und kann das geändert werden? 
* Welche Felder müssen abgefragt werden? 
* Wie hoch ist die erwartete Datenmenge und die Geschwindigkeit? 
* Wie viele Ereignistypen werden erwartet (entsprechend der Anzahl von Tabellen)? 
* Wie oft wird eine Änderung des Ereignisschemas erwartet? 
* Wie viele Knoten werden die Daten generieren? 
* Welches Quell-BS wird verwendet? 
* Wie lauten die Latenzanforderungen? 
* Kann eine der vorhandenen verwalteten Erfassungspipelines verwendet werden? 

Für Organisationen mit einer bestehenden Infrastruktur, die auf einem Messaging-Dienst wie Event Hub basiert, ist die Verwendung eines Konnektors wahrscheinlich die am besten geeignete Lösung. Die Erfassung in der Warteschlange eignet sich für große Datenmengen.

## <a name="supported-data-formats"></a>Unterstützte Datenformate

Für alle anderen Erfassungsmethoden als das Erfassen aus der Abfrage müssen die Daten in einem der unterstützten Datenformate formatiert werden, damit der Azure-Daten-Explorer sie analysieren kann.

* CSV, TSV, PSV, SCSV, SOH
* JSON (getrennte Zeilen, mehrzeilig) Avro
* ZIP und GZIP 

> [!NOTE]
> Beim Erfassen von Daten werden Datentypen aus den Spalten der Zieltabelle abgeleitet. Wenn ein Datensatz unvollständig ist oder ein Feld nicht als erforderlicher Datentyp analysiert werden kann, werden die entsprechenden Tabellenspalten mit Nullwerten gefüllt.

## <a name="schema-mapping"></a>Schemazuordnung

Die Schemazuordnung hilft dabei, Quelldatenfelder deterministisch an Spalten der Zieltabelle zu binden.

* [CSV-Zuordnung](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#csv-mapping) (optional) arbeitet mit allen ordinalbasierten Formaten und kann als Erfassungsbefehlsparameter übergeben oder [in der Tabelle](https://docs.microsoft.com/azure/kusto/management/tables?branch=master#create-ingestion-mapping) vorab erstellt und vom Erfassungsbefehlsparameter referenziert werden.
* [JSON-Zuordnung](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#json-mapping) (obligatorisch) und [Avro-Zuordnung](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#avro-mapping) (obligatorisch) können als Erfassungsbefehlsparameter übergeben oder [in der Tabelle vorab erstellt](https://docs.microsoft.com/azure/kusto/management/tables#create-ingestion-mapping) und vom Erfassungsbefehlsparameter referenziert werden.

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erfassen von Daten aus Event Hub in Azure-Daten-Explorer](ingest-data-event-hub.md)

[Schnellstart: Erfassen von Daten mit der Azure-Daten-Explorer-Bibliothek für Python](python-ingest-data.md)

