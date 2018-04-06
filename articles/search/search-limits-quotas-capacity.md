---
title: Grenzwerte für den Azure Search-Dienst | Microsoft Docs
description: Grenzwerte für den Dienst, die bei der Kapazitätsplanung verwendet werden, sowie Höchstwerte für Anforderungen und Antworten für Azure Search.
services: search
documentationcenter: ''
author: HeidiSteen
manager: jhubbard
editor: ''
tags: azure-portal
ms.assetid: 857a8606-c1bf-48f1-8758-8032bbe220ad
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 03/26/2018
ms.author: heidist
ms.openlocfilehash: fb2234e79e8deb98a94068f31a40c8f0b415d7ba
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="service-limits-in-azure-search"></a>Grenzwerte für den Azure Search-Dienst
Die Grenzwerte für Speicher, Workloads und Mengen von Indizes, Dokumenten und anderen Objekten hängen davon ab, ob die [Bereitstellung von Azure Search](search-create-service-portal.md) im Tarif **Free**, **Basic** oder **Standard** erfolgt.

* **Free** ist ein gemeinsamer mehrinstanzfähiger Dienst, der Teil Ihres Azure-Abonnements ist. 
* **Basic** bietet spezifische Computerressourcen für Produktionsworkloads mit geringerem Umfang.
* **Standard** wird auf dedizierten Computern ausgeführt. Sie bieten höhere Speicher- und Verarbeitungskapazität auf jeder Ebene. Standard ist in vier Ebenen verfügbar: S1, S2, S3 und S3 High Density (S3 HD).

> [!NOTE]
> Ein Dienst wird für einen bestimmten Tarif bereitgestellt. Das Wechseln von Tarifen, um die Kapazität zu erhöhen, umfasst die Bereitstellung eines neuen Diensts (es gibt kein direktes Upgrade). Weitere Informationen finden Sie unter [Auswählen einer SKU oder eines Tarifs](search-sku-tier.md). Weitere Informationen zum Anpassen der Kapazität in einem Dienst, den Sie bereits bereitgestellt haben, finden Sie unter [Skalieren von Ressourcenebenen für Abfrage und Indizierung von Workloads in Azure Search](search-capacity-planning.md).
>

## <a name="subscription-limits"></a>Grenzwerte für Abonnements
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="service-limits"></a>Diensteinschränkungen
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

## <a name="index-limits"></a>Indexgrenzwerte

| Ressource | Kostenlos | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Maximale Anzahl von Feldern pro Index |1000 |100 <sup>1</sup> |1000 |1000 |1000 |1000 |
| Maximale Anzahl von Bewertungsprofilen pro Index |100 |100 |100 |100 |100 |100 |
| Maximale Anzahl von Funktionen pro Profil |8 |8 |8 |8 |8 |8 |

<sup>1</sup> Der Basic-Tarif ist die einzige SKU mit einem unteren Grenzwert von 100 Feldern pro Index.

## <a name="indexer-limits"></a>Indexergrenzwerte

| Ressource | Kostenlos | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Maximale Indizierungslast pro Aufruf |10.000 Dokumente |Nur durch maximale Dokumentanzahl beschränkt |Nur durch maximale Dokumentanzahl beschränkt |Nur durch maximale Dokumentanzahl beschränkt |Nur durch maximale Dokumentanzahl beschränkt |N/V <sup>1</sup> |
| Maximale Ausführungszeit | 1-3 Minuten <sup>2</sup> |24 Stunden |24 Stunden |24 Stunden |24 Stunden |N/V <sup>1</sup> |
| Blobindexer: maximale Blobgröße, MB |16 |16 |128 |256 |256 |N/V <sup>1</sup> |
| Blobindexer: maximale Anzahl der Zeichen des aus einem Blob extrahierten Inhalts |32.000 |64.000 |4 Millionen |4 Millionen |4 Millionen |N/V <sup>1</sup> |

<sup>1</sup> S3 HD unterstützt derzeit keine Indexer. Wenden Sie sich an den Azure-Support, wenn Sie diese Funktion dringend benötigen.

<sup>2</sup> Die maximale Indexerausführungszeit im Free-Tarif beträgt drei Minuten für Blobquellen und eine Minute für alle anderen Datenquellen.


## <a name="document-size-limits"></a>Dokumentgrößenlimits
| Ressource | Kostenlos | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Individuelle Dokumentgröße pro Index-API |<16 MB |<16 MB |<16 MB |<16 MB |<16 MB |<16 MB |

Bezieht sich auf die maximale Dokumentgröße beim Aufrufen einer Index-API. Die Dokumentgröße ist tatsächlich eine Begrenzung der Größe des Anforderungstexts der Index-API. Da Sie einen Batch von mehreren Dokumenten auf einmal an die Index-API übergeben können, hängt die tatsächliche maximale Größe davon ab, wie viele Dokumente im Batch vorhanden sind. Für einen Batch mit einem einzelnen Dokument beträgt die maximale Dokumentgröße 16 MB von JSON.

Um die Dokumentgröße niedrig zu halten, achten Sie darauf, nicht abfragbare Daten von der Anforderung auszuschließen. Bilder und andere binäre Daten können nicht direkt abgefragt werden und sollten nicht im Index gespeichert werden. Um nicht abfragbare Daten in Suchergebnisse zu integrieren, definieren Sie ein nicht durchsuchbares Feld, in dem ein URL-Verweis auf die Ressource gespeichert wird.

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
