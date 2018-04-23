---
title: Grenzwerte für den Azure Search-Dienst | Microsoft Docs
description: Grenzwerte für den Dienst, die bei der Kapazitätsplanung verwendet werden, sowie Höchstwerte für Anforderungen und Antworten für Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: heidist
ms.openlocfilehash: 59b6efd8459f675bc44891d7b04251260fdd38e9
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="service-limits-in-azure-search"></a>Grenzwerte für den Azure Search-Dienst
Die Grenzwerte für Speicher, Workloads und Mengen von Indizes, Dokumenten und anderen Objekten hängen davon ab, ob die [Bereitstellung von Azure Search](search-create-service-portal.md) im Tarif **Free**, **Basic** oder **Standard** erfolgt.

+ **Free** ist ein gemeinsamer mehrinstanzfähiger Dienst, der Teil Ihres Azure-Abonnements ist.

+ **Basic** bietet spezifische Computerressourcen für Produktionsworkloads mit geringerem Umfang.

+ **Standard** wird auf dedizierten Computern ausgeführt. Sie bieten höhere Speicher- und Verarbeitungskapazität auf jeder Ebene. Standard ist in vier Ebenen verfügbar: S1, S2, S3 und S3 HD.

  S3 High Density (S3 HD) ist für bestimmte Workloads konzipiert: [Mehrinstanzenfähigkeit](search-modeling-multitenant-saas-applications.md) und große Mengen von kleinen Indizes (eine Million Dokumente pro Index, 3000 Indizes pro Dienst). Bei diesem Tarif ist das [Indexerfeature](search-indexer-overview.md) nicht verfügbar. Bei S3 HD muss der Push-Ansatz für die Datenerfassung verwendet werden, wobei Daten mithilfe von API-Aufrufen per Push von der Quelle an den Index übertragen werden. 

> [!NOTE]
> Ein Dienst wird für einen bestimmten Tarif bereitgestellt. Das Wechseln von Tarifen, um die Kapazität zu erhöhen, umfasst die Bereitstellung eines neuen Diensts (es gibt kein direktes Upgrade). Weitere Informationen finden Sie unter [Auswählen einer SKU oder eines Tarifs](search-sku-tier.md). Weitere Informationen zum Anpassen der Kapazität in einem Dienst, den Sie bereits bereitgestellt haben, finden Sie unter [Skalieren von Ressourcenebenen für Abfrage und Indizierung von Workloads in Azure Search](search-capacity-planning.md).
>

## <a name="subscription-limits"></a>Grenzwerte für Abonnements
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Speichergrenzwerte
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Indexgrenzwerte

| Ressource | Kostenlos | Basic&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD |
| -------- | ---- | ------------------- | --- | --- | --- | --- |
| Maximale Anzahl von Indizes |3 |5 oder 15 |50 |200 |200 |1000 pro Partition oder 3000 pro Dienst |
| Maximale Anzahl von Feldern pro Index |1000 |100 |1000 |1000 |1000 |1000 |
| Maximale Anzahl von Bewertungsprofilen pro Index |100 |100 |100 |100 |100 |100 |
| Maximale Anzahl von Funktionen pro Profil |8 |8 |8 |8 |8 |8 |

<sup>1</sup> Bei Diensten mit dem Tarif „Basic“, die nach Ende 2017 erstellt wurden, wurde das Limit auf 15 Indizes, Datenquellen und Indexer erhöht. Für zuvor erstellte Dienste gilt eine Begrenzung auf 5. Der Basic-Tarif ist die einzige SKU mit einem unteren Grenzwert von 100 Feldern pro Index.

## <a name="document-limits"></a>Dokumentgrenzwerte 

In den meisten Regionen ist die Dokumentanzahl bei den Azure Search-Tarifen (Basic, S1, S2, S3, S3 HD) für alle Dienste, die nach November/Dezember 2017 erstellt wurden, unbegrenzt. In diesem Abschnitt sind die Regionen angegeben, in denen Grenzwerte gelten. Außerdem wird erläutert, wie Sie ermitteln, ob Ihr Dienst betroffen ist. 

Überprüfen Sie auf der Kachel „Nutzung“ auf der Übersichtsseite Ihres Diensts, ob Dokumentgrenzwerte für Ihren Dienst gelten. Die Dokumentanzahl ist entweder unbegrenzt, oder es gilt ein Grenzwert basierend auf dem Tarif dafür.

  ![Kachel „Nutzung“](media/search-limits-quotas-capacity/portal-usage-tile.png)

### <a name="regions-and-services-having-document-limits"></a>Regionen und Dienste mit Dokumentgrenzwerten

Dienste mit Grenzwerten wurden entweder vor Ende 2017 erstellt oder werden in Rechenzentren ausgeführt, die Cluster mit geringerer Kapazität zum Hosten von Azure Search-Diensten verwenden. Betroffene Rechenzentren befinden sich in den folgenden Regionen:

+ Australien (Osten)
+ Asien, Osten
+ Indien, Mitte
+ Japan, Westen
+ USA, Westen-Mitte

Für Dienste, die Dokumentgrenzwerten unterliegen, gelten die folgenden Obergrenzen:

|  Kostenlos | Basic | S1 | S2 | S3 | S3&nbsp;HD |
|-------|-------|----|----|----|-------|
|  10.000 |1 Mio. |15 Millionen pro Partition oder 180 Millionen pro Dienst |60 Millionen pro Partition oder 720 Millionen pro Dienst |120 Millionen pro Partition oder 1,4 Milliarden pro Dienst |1 Millionen pro Index oder 200 Millionen pro Partition |

> [!Note] 
> Bei S3 High Density-Diensten, die nach Ende 2017 erstellt wurden, wurde das Limit „200 Millionen Dokumente pro Partition“ entfernt, das Limit „1 Million Dokumente pro Index“ bleibt jedoch bestehen.


### <a name="document-size-limits-per-api-call"></a>Dokumentgrößenbeschränkungen pro API-Aufruf

Die maximale Dokumentgröße beim Aufrufen einer Index-API beträgt etwa 16 Megabytes.

Die Dokumentgröße ist tatsächlich eine Begrenzung der Größe des Anforderungstexts der Index-API. Da Sie einen Batch von mehreren Dokumenten auf einmal an die Index-API übergeben können, hängt die realistische maximale Größe davon ab, wie viele Dokumente im Batch vorhanden sind. Für einen Batch mit einem einzelnen Dokument beträgt die maximale Dokumentgröße 16 MB von JSON.

Um die Dokumentgröße niedrig zu halten, achten Sie darauf, nicht abfragbare Daten von der Anforderung auszuschließen. Bilder und andere binäre Daten können nicht direkt abgefragt werden und sollten nicht im Index gespeichert werden. Um nicht abfragbare Daten in Suchergebnisse zu integrieren, definieren Sie ein nicht durchsuchbares Feld, in dem ein URL-Verweis auf die Ressource gespeichert wird.

## <a name="indexer-limits"></a>Indexergrenzwerte

Bei Diensten mit dem Tarif „Basic“, die nach Ende 2017 erstellt wurden, wurde das Limit auf 15 Indizes, Datenquellen und Indexer erhöht.

| Ressource | Free&nbsp;<sup>1</sup> | Basic&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|
| -------- | ----------------- | ----------------- | --- | --- | --- | --- |
| Maximale Anzahl von Indexern |3 |5 oder 15|50 |200 |200 |N/V |
| Maximale Datenquellen |3 |5 oder 15 |50 |200 |200 |N/V |
| Maximale Indizierungslast pro Aufruf |10.000 Dokumente |Nur durch maximale Dokumentanzahl beschränkt |Nur durch maximale Dokumentanzahl beschränkt |Nur durch maximale Dokumentanzahl beschränkt |Nur durch maximale Dokumentanzahl beschränkt |N/V |
| Maximale Ausführungszeit | 1–3 Minuten |24 Stunden |24 Stunden |24 Stunden |24 Stunden |N/V  |
| Blobindexer: maximale Blobgröße, MB |16 |16 |128 |256 |256 |N/V  |
| Blobindexer: maximale Anzahl der Zeichen des aus einem Blob extrahierten Inhalts |32.000 |64.000 |4 Millionen |4 Millionen |4 Millionen |N/V |

<sup>1</sup> Die maximale Indexerausführungszeit bei Diensten im Free-Tarif beträgt drei Minuten für Blobquellen und eine Minute für alle anderen Datenquellen.

<sup>2</sup> Bei Diensten mit dem Tarif „Basic“, die nach Ende 2017 erstellt wurden, wurde das Limit auf 15 Indizes, Datenquellen und Indexer erhöht. Für zuvor erstellte Dienste gilt eine Begrenzung auf 5.

<sup>3</sup> S3 HD-Dienste beinhalten keine Indexerunterstützung.

## <a name="queries-per-second-qps"></a>Abfragen pro Sekunde (QPS)

QPS-Schätzungen müssen unabhängig von jedem Kunde erstellt werden. Indexgröße und Komplexität, Abfragegröße und Komplexität sowie der Umfang des Datenverkehrs sind Hauptentscheidungskriterium für den QPS-Wert. Es gibt keine Möglichkeit, sinnvolle Schätzungen abzugeben, wenn diese Faktoren unbekannt sind.

Schätzungen sind besser vorhersagbar, wenn sie für Dienste berechnet werden, die auf dedizierten Ressourcen ausgeführt werden (Basic- und Standard-Tarife). Sie können den QPS-Wert genauer schätzen, da Sie die Kontrolle über mehr Parameter haben. Anleitungen zur Herangehensweise für Schätzungen finden Sie unter [Leistung und Optimierung von Azure Search](search-performance-optimization.md).

## <a name="api-request-limits"></a>API-Anforderungsgrenzwerte
* Maximal 16 MB pro Anforderung <sup>1</sup>
* Maximale URL-Länge von 8 KB
* Maximal 1000 Dokumente pro Batch von Hochlade-, Zusammenführungs- oder Löschvorgängen für Indizes
* Maximal 32 Felder in $orderby-Klausel
* Maximale Suchbegriffgröße ist 32.766 Byte (32 KB minus 2 Bytes) von UTF-8-codiertem Text

<sup>1</sup> In Azure Search darf der Inhalt einer Anforderung nicht größer als 16 MB sein. Dies beschränkt möglicherweise den Inhalt einzelner Felder oder Sammlungen, für die ansonsten keine theoretischen Beschränkungen gelten. (Weitere Informationen zur Feldzusammensetzung und den Beschränkungen finden Sie unter [Supported data types](https://msdn.microsoft.com/library/azure/dn798938.aspx) (Unterstützte Datentypen).)

## <a name="api-response-limits"></a>API-Antwortengrenzwerte
* Maximale Rückgabe von 1000 Dokumenten pro Seite mit Suchergebnissen
* Maximale Rückgabe von 100 Vorschlägen pro Anforderung der Vorschlags-API

## <a name="api-key-limits"></a>API-Schlüsselgrenzwerte
API-Schlüssel werden für die Dienstauthentifizierung verwendet. Es gibt zwei Arten. Administratorschlüssel werden im Anforderungsheader angegeben und gewähren vollständigen Lese-und Schreibzugriff für den Dienst. Abfrageschlüssel sind schreibgeschützt, die in der URL angegeben und normalerweise an Clientanwendungen verteilt werden.

* Maximal 2 Administratorschlüssel pro Dienst
* Maximal 50 Abfrageschlüssel pro Dienst
