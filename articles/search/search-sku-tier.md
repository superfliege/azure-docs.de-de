---
title: Auswählen einer SKU oder eines Tarifs für den Azure Search-Dienst – Azure Search
description: 'Azure Search kann unter folgenden SKUs bereitgestellt werden: Free, Basic und Standard. Standard ist mit verschiedenen Ressourcenkonfigurationen und Kapazitäten verfügbar.'
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 04/15/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: b50d0c0ca9a4000cc0c725453a3ef04b4bed9275
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681564"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Auswählen eines Tarifs für Azure Search

In Azure Search erfolgt das [Erstellen einer Ressource](search-create-service-portal.md) basierend auf einem Tarif oder einer SKU, der/die für die Lebensdauer des Diensts fest ist. Folgende Tarife sind verfügbar: **Free**, **Basic**, **Standard** und **Speicheroptimiert**.  **Standard** und **Speicheroptimiert** sind mit verschiedenen Konfigurationen und Kapazitäten verfügbar. 

Die meisten Kunden beginnen zu Evaluierungszwecken mit dem Tarif **Free** und wechseln dann für Entwicklungs- und Produktionsbereitstellungen zu einem der höheren kostenpflichtigen Tarife. Sie können alle Schnellstartanleitungen und Tutorials im Tarif **Free** absolvieren, einschließlich jener für ressourcenintensive kognitive Suchen.

> [!NOTE]
> Die Tarife vom Typ „Speicheroptimiert“ sind derzeit als günstigere Vorschauversion zum Testen und Experimentieren erhältlich, um Feedback zu sammeln. Die endgültigen Preise werden später bekannt gegeben, wenn diese Tarife allgemein verfügbar sind. Wir raten davon ab, diese Tarife für Produktionsanwendungen zu verwenden.

Tarife geben die Merkmale der Hardware wieder, die den Dienst hostet (anstelle von Funktionen), und unterscheiden sich durch:

+ Anzahl der Indizes, die Sie erstellen können
+ Größe und Geschwindigkeit von Partitionen (physischer Speicher)

Obwohl alle Tarife, einschließlich des Tarifs **Free**, in der Regel Featureparität anbieten, können größere Workloads Anforderungen für höhere Tarife diktieren. Beispielsweise ist die [KI-Indizierung mit Cognitive Services](cognitive-search-concept-intro.md) mit Vorgängen zum Ausführen von Qualifikationen mit langer Laufzeit verbunden, sodass bei einem kostenlosen Dienst ein Timeout auftritt, sofern es sich nicht um ein kleines Dataset handelt.

> [!NOTE] 
> Die Ausnahme bei der Featureparität sind die [Indexer](search-indexer-overview.md), die bei S3HD nicht verfügbar sind.
>

Innerhalb eines Tarifs können Sie [Replikat- und Partitionsressourcen anpassen](search-capacity-planning.md), um hoch- oder herunterzuskalieren. Sie können mit jeweils ein oder zwei Replikaten und Partitionen beginnen und Ihre Rechenleistung dann für eine hohe Indizierungsworkload vorübergehend erhöhen. Die Möglichkeit, Ressourcenniveaus innerhalb eines Tarifs zu optimieren, sorgt für mehr Flexibilität, verkompliziert aber auch etwas Ihre Analyse. Möglicherweise müssen Sie experimentieren, um herauszufinden, ob ein niedrigerer Tarif mit höheren Ressourcen-/Replikatangeboten einen besseren Wert und höhere Leistung bietet, als ein höherer Tarif mit weniger Ressourcen. Weitere Informationen dazu, wann und warum Sie Kapazität anpassen sollten, finden Sie unter [Überlegungen zur Leistung und Optimierung von Azure Search](search-performance-optimization.md).

## <a name="tiers-for-azure-search"></a>Tarife für Azure Search

In der folgenden Tabelle sind die verfügbaren Tarife aufgeführt. Informationen zu Tarifen finden Sie zudem auf der [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/search/), unter [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md) und auf der Portalseite beim Bereitstellen eines Diensts.

|Tarif | Capacity |
|-----|-------------|
|Kostenlos | Für andere Abonnenten freigegeben. Nicht skalierbar, auf 3 Indizes und 50 MB Speicher begrenzt. |
|Basic | Dedizierte Computingressourcen für Produktionsworkloads mit geringerem Umfang. Eine 2-GB-Partition und bis zu drei Replikate. |
|Standard 1 (S1) | Ab S1 werden dedizierte Computer mit höherer Speicher- und Verarbeitungskapazität auf jeder Ebene verwendet. Die Partitionsgröße liegt bei diesem Tarif bei 25 GB pro Partition (max. 300 GB pro Dienst). |
|Standard 2 (S2) | Ähnlich wie S1, jedoch mit 100 GB pro Partition (max. 1,2 TB pro Dienst) |
|Standard 3 (S3) | 200 GB pro Partition (max. 2,4 TB pro Dienst) |
|Standard 3 High-density (S3-HD) | High-density ist ein *Hostingmodus* für S3. Die zugrunde liegende Hardware ist für eine große Anzahl kleinerer Indizes optimiert und für Szenarios mit Mehrmandantenfähigkeit konzipiert. Die Gebühr pro Einheit unterscheidet sich zwischen S3-HD und S3 nicht, die Hardware ist bei S3-HD jedoch für schnelle Dateilesevorgänge bei einer großen Anzahl kleinerer Indizes optimiert.|
|Speicheroptimiert 1 (L1) | 1 TB pro Partition (max. 12 TB pro Dienst) |
|Speicheroptimiert 2 (L2) | 2 TB pro Partition (max. 24 TB pro Dienst) |

> [!NOTE] 
> Die speicheroptimierten Tarife bieten eine höhere Speicherkapazität zu einem niedrigeren Preis pro TB als die Standard-Tarife. Der größte Nachteil ist die höhere Wartezeit. Diese müssen Sie bei Ihren spezifischen Anwendungsanforderungen berücksichtigen.  Weitere Informationen zu den Leistungsaspekten dieses Tarifs finden Sie unter [Bereitstellungsstrategien und bewährte Methoden zur Optimierung der Leistung in Azure Search](search-performance-optimization.md).
>

## <a name="how-billing-works"></a>Funktionsweise der Abrechnung

Bei Azure Search können auf drei verschiedene Weisen Kosten anfallen, und es gibt feste und variable Komponenten. Dieser Abschnitt befasst sich der Reihe nach mit jeder Abrechnungskomponente.

### <a name="1-core-service-costs-fixed-and-variable"></a>1. Basiskosten für den Dienst (fest und variabel)

Die Mindestgebühr für den Dienst selbst ist die erste Sucheinheit (1 Replikat x 1 Partition). Dieser Betrag ist für die gesamte Lebensdauer des Diensts festgelegt, da der Dienst nicht mit weniger als dieser Konfiguration ausgeführt werden kann. 

Über diese Mindestanforderung hinaus können Sie Replikate und Partitionen eigenständig hinzufügen. Beispielsweise können Sie nur Replikate oder nur Partitionen hinzufügen. Inkrementelle Kapazitätserweiterungen durch Replikate und Partitionen bilden die variable Kostenkomponente. 

Die Abrechnung basiert auf einer [Formel (Replikate x Partitionen x Preis)](#search-units). Der abgerechnete Preis hängt vom ausgewählten Tarif ab.

Im folgenden Screenshot wird der Preis pro Einheit für die Tarife Free, Basic und S1 dargestellt (S2, S3, L1 und L2 sind nicht enthalten). Wenn Sie einen Dienst mit dem Tarif **Basic**, **Standard** oder **Speicheroptimiert** erstellt haben, belaufen sich Ihre monatlichen Kosten im Durchschnitt auf den Wert, der für *price-1* bzw. für *price-2* angegeben ist. Die Kosten pro Einheit steigen mit jedem Tarif, da sich bei den höheren Tarifen jeweils die Rechenleistung und die Speicherkapazität erhöhen. Die Preise für Azure Search sind auf der Seite [Azure Search – Preise](https://azure.microsoft.com/pricing/details/search/) veröffentlicht.

![Preise pro Einheit](./media/search-sku-tier/per-unit-pricing.png "Preise pro Einheit")

Bei der Kalkulation einer Suchlösung ist zu beachten, dass Preis und Kapazität nicht linear sind (die Verdoppelung der Kapazität führt zu mehr als verdoppelten Kosten). Ein Beispiel zur Funktionsweise der Formel finden Sie unter [Zuordnen von Replikaten und Partitionen](search-capacity-planning.md#how-to-allocate-replicas-and-partitions).


### <a name="2-data-egress-charges-during-indexing"></a>2. Gebühren für ausgehende Daten während der Indizierung

Die Verwendung von [Azure Search Indexer](search-indexer-overview.md) kann je nach Position der Dienste zu Auswirkungen auf die Abrechnung führen. Sie können die Gebühren für ausgehende Daten vollständig vermeiden, wenn Sie den Azure Search-Dienst in derselben Region wie Ihre Daten erstellen. Die folgenden Punkte stammen von der Seite [Preisübersicht Bandbreite](https://azure.microsoft.com/pricing/details/bandwidth/).

+ Microsoft berechnet keine Gebühren für eingehende Daten in alle Dienste in Azure oder für ausgehende Daten aus Azure Search.

+ In Lösungen mit mehreren Diensten fallen keine Gebühren für Datenübertragungen an, wenn sich alle Dienste in derselben Region befinden.

Für ausgehende Daten fallen Gebühren an, wenn sich die Dienste in verschiedenen Regionen befinden. Diese Gebühren sind nicht Teil Ihrer Azure Search-Rechnung, sie werden hier jedoch erwähnt, denn wenn Sie Daten oder um KI erweiterte Indexer verwenden, um Daten aus verschiedenen Regionen mithilfe von Pull zu übertragen, sind diese Kosten in Ihrer Gesamtrechnung aufgeführt. 

### <a name="3-ai-enriched-indexing-using-cognitive-services"></a>3. Um KI erweiterte Indizierung unter Verwendung von Cognitive Services

Für die [KI-Indizierung mit Cognitive Services](cognitive-search-concept-intro.md) sollten Sie die Anfügung einer abrechenbaren Cognitive Services-Ressource zum S0-Tarif für die nutzungsbasierte Bezahlung der Verarbeitung einplanen. Mit dem Anfügen von Cognitive Services sind keine „festen Kosten“ verbunden. Sie bezahlen nur für die benötigte Verarbeitung.

Für die Bildextraktion während der Dokumententschlüsselung fällt eine Azure Search-Gebühr an, die basierend auf der Anzahl der aus den Dokumenten extrahierten Bildern abgerechnet wird. Das Extrahieren von Text ist derzeit kostenlos. 

Andere auf [integrierten kognitiven Fähigkeiten](cognitive-search-predefined-skills.md) basierende Erweiterungen, z. B. die Verarbeitung natürlicher Sprache, werden für eine Cognitive Services-Ressource zu dem Preis abgerechnet, der bei direkter Ausführung der Aufgabe mithilfe von Cognitive Services anfällt. Weitere Informationen finden Sie unter [Anfügen einer Cognitive Services-Ressource an eine Qualifikationsgruppe](cognitive-search-attach-cognitive-services.md).

<a name="search-units"></a>

### <a name="billing-based-on-search-units"></a>Auf Sucheinheiten basierende Abrechnung

Für Azure Search-Vorgänge ist das wichtigste Abrechnungskonzept, das Sie verstehen sollten, eine *Sucheinheit* (Search Unit, SU). Da Azure Search bei der Indizierung und bei Abfragen sowohl von Replikaten als auch von Partitionen abhängig ist, ist es nicht sinnvoll, entweder nur nach dem einen oder nur nach dem anderen abzurechnen. Stattdessen basiert die Abrechnung auf einer Kombination beider. 

SU ist das Produkt der von einem Dienst verwendeten *Replikate* und *Partitionen*: **`(R X P = SU)`**

Jeder Dienst beginnt mit mindestens einer SU (einem Replikat multipliziert mit einer Partition). Das Maximum für jeden Dienst liegt bei 36 SUs. Dies lässt sich auf verschiedene Arten erreichen – beispielsweise mit sechs Partitionen mal sechs Replikate oder drei Partitionen mal 12 Replikate. Es ist üblich, weniger als die Gesamtkapazität zu verwenden. Z. B. ein Dienst mit 3 Replikaten und 3 Partitionen, abgerechnet als 9 SUs. In [diesem Diagramm](search-capacity-planning.md#chart) erkennen Sie gültige Kombinationen auf einen Blick.

Es wird **stündlich pro SU** abgerechnet, wobei jeder Tarif einen progressiv steigenden Abrechnungssatz hat. Höhere Tarife enthalten größere und schnellere Partitionen, was zu einem insgesamt höheren Stundensatz für den jeweiligen Tarif beiträgt. Die Preise für die einzelnen Tarife finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/search/). 

Die meisten Kunden schalten nur einen Teil der Gesamtkapazität online und halten den Rest in Reserve. Im Hinblick auf die Abrechnung bestimmt die Anzahl der Partitionen und Replikate, die Sie online bringen, mithilfe der SU-Formel berechnet, was Sie tatsächlich stündlich bezahlen.

### <a name="billing-for-image-extraction-in-cognitive-search"></a>Abrechnung für das Extrahieren von Bildern bei der kognitiven Suche

Wenn Sie Bilder aus Dateien in einer Indizierungspipeline für die kognitive Suche extrahieren, wird Ihnen in Ihrer Azure Search-Rechnung dieser Vorgang in Rechnung gestellt. In einer [Indexerkonfiguration](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-parameters) ist **imageAction** der Parameter, der die Bildextraktion auslöst. Wenn **imageAction** auf „None“ festgelegt ist (Standardeinstellung), fallen keine Gebühren für die Bildextraktion an.

Die Preise können sich ändern, dies wird jedoch immer auf der [Seite „Azure Search – Preise“](https://azure.microsoft.com/pricing/details/search/) dokumentiert. 

### <a name="billing-for-built-in-skills-in-cognitive-search"></a>Abrechnung für integrierte Qualifikationen bei der kognitiven Suche

Beim Einrichten einer Anreicherungspipeline basieren alle in der Pipeline verwendeten [integrierten Qualifikationen](cognitive-search-predefined-skills.md) auf Machine Learning-Modellen. Diese Modelle werden von Cognitive Services bereitgestellt. Die Verwendung dieser Modelle während der Indizierung wird mit der gleichen Gebühr abgerechnet, als ob Sie die Ressource direkt angefordert hätten.

Nehmen wir beispielsweise eine Pipeline an, die aus optischer Zeichenerkennung (Optical Character Recognition, OCR) für gescannte JPEG-Bilddateien besteht, wobei der resultierende Text mittels Push in einen Azure Search-Index für Freiform-Suchabfragen übertragen wird. Ihre Indizierungspipeline würde einen Indexer mit der [Qualifikation „OCR“](cognitive-search-skill-ocr.md) enthalten, und diese Qualifikation wäre [einer Cognitive Services-Ressource zugeordnet](cognitive-search-attach-cognitive-services.md). Wenn Sie den Indexer ausführen, werden die entsprechenden Gebühren auf Ihrer Rechnung der Cognitive Services-Ressourcen für die OCR-Ausführung aufgeführt.

## <a name="tips-for-reducing-costs"></a>Tipps zur Reduzierung von Kosten

Sie können den Dienst nicht herunterfahren, um die Rechnung zu senken. Dedizierte Ressourcen werden während der gesamten Lebensdauer des Diensts 24-7 betrieben und sind für Ihre exklusive Nutzung zugeordnet. Die einzige Möglichkeit, um eine Rechnung zu senken, besteht darin, Replikate und Partitionen auf ein niedriges Niveau zu senken, das immer noch eine akzeptable Leistung und [SLA-Compliance](https://azure.microsoft.com/support/legal/sla/search/v1_0/) bietet. 

Ein Hebel zur Reduzierung der Kosten ist die Auswahl eines Tarifs mit einem niedrigeren Stundensatz. Stundensätze von S1 sind niedriger als die von S2 oder S3. Angenommen, Sie stellen einen Dienst bereit, der auf das untere Ende Ihrer projizierten Auslastungen abzielt. Wenn dieser Dienst Ihre Anforderungen nicht mehr erfüllt, können Sie einen zweiten Dienst in einem höheren Tarif erstellen, Ihre Indizes für diesen zweiten Dienst neu erstellen und dann den ersten Dienst löschen. 

Wenn Sie eine Kapazitätsplanung für lokale Server durchgeführt haben, wissen Sie, dass es üblich ist, „aufzukaufen“, damit Sie projiziertes Wachstum kompensieren können. Bei einem Clouddienst können Sie jedoch bei den Kosteneinsparungen aggressiver vorgehen, da Sie nicht an einen bestimmten Kauf gebunden sind. Sie können jederzeit in einen Dienst mit höherem Tarif wechseln, wenn der aktuelle nicht ausreicht.

### <a name="capacity-drill-down"></a>Kapazitätsdrilldown

In Azure Search wird Kapazität in *Replikate* und *Partitionen* strukturiert. 

+ Replikate sind Instanzen des Suchdiensts, in denen jedes Replikat eine Lastenausgleichskopie eines Indexes hostet. Beispielsweise verfügt ein Dienst mit 6 Replikaten über 6 Kopien jedes in den Dienst geladenen Indexes. 

+ Partitionen speichern Indizes und teilen durchsuchbare Daten automatisch auf: zwei Partitionen teilen den Index in Hälften, drei Partitionen in Drittel usw. Im Hinblick auf die Kapazität ist die *Partitionsgröße* das primäre unterscheidende Feature der Tarife.

> [!NOTE]
> Alle Tarife vom Typ **Standard** und **Speicheroptimiert** unterstützen [flexible Kombinationen von Replikaten und Partitionen](search-capacity-planning.md#chart), sodass Sie durch Ändern der Balance [Ihr System nach Geschwindigkeit oder Speicher gewichten](search-performance-optimization.md) können. **Basic** bietet bis zu drei Replikate für Hochverfügbarkeit, enthält jedoch nur eine Partition. **Free**-Tarife bieten keine dedizierten Ressourcen: Computingressourcen werden von mehreren Abonnenten gemeinsam genutzt.

### <a name="more-about-service-limits"></a>Weitere Informationen zu Dienstgrenzwerten

Bedient Hostressourcen, z.B. Indizes, Indexer usw. Jeder Tarif erzwingt [Dienstgrenzwerte](search-limits-quotas-capacity.md) bezüglich der Menge an Ressourcen, die Sie erstellen können. Daher ist eine Obergrenze für die Anzahl der Indizes (und anderer Objekte) das zweite Unterscheidungsmerkmal zwischen Tarifen. Wenn Sie die einzelnen Optionen im Portal durchgehen, beachten Sie die Grenzwerte für die Anzahl der Indizes. Andere Ressourcen, z.B. Indexer, Datenquellen und Skillsets, sind an Indexgrenzwerte gebunden.

## <a name="consumption-patterns"></a>Verbrauchsmuster

Die meisten Kunden beginnen mit dem Tarif **Free**, den sie unbegrenzt behalten, und wählen dann einen der Tarife vom Typ **Standard** oder **Speicheroptimiert** für Entwicklungs- oder Produktionsworkloads aus. 

![Azure Search-Tarife](./media/search-sku-tier/tiers.png "Azure Search-Tarife")

An den jeweiligen Enden sind **Basic** und **S3 HD** für wichtige, aber untypische Verbrauchsmuster vorhanden. **Basic** ist für kleine Produktionsworkloads bestimmt: Er bietet SLA, dedizierte Ressourcen, hohe Verfügbarkeit, aber moderaten Speicher, der höchstens 2 GB insgesamt erreicht. Dieser Tarif wurde für Kunden entwickelt, die die verfügbare Kapazität konsistent unterschritten haben. Am anderen Ende ist **S3 HD** für Workloads vorgesehen, die für unabhängige Softwareanbieter, Partner, [mehrinstanzfähige Lösungen](search-modeling-multitenant-saas-applications.md) oder jede Konfiguration typisch sind, die eine große Anzahl kleiner Indizes aufruft. Es ist häufig klar, wenn der **Basic**- oder **S3 HD**-Tarif passend ist, aber zur Bestätigung können Sie in [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) posten oder den [Azure Support kontaktieren](https://azure.microsoft.com/support/options/), um weitere Hinweise zu erhalten.

Betrachten wir nun die gebräuchlicheren Standardtarife: **S1-S3** bilden eine Steigerung mit zunehmenden Kapazitätsstufen, wobei die Wendepunkte bei Partitionsgröße und maximaler Anzahl von Indizes, Indexern und anderen folgerichtigen Ressourcen liegen:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Partitionsgröße|  25 GB | 100 GB | 200 GB |  |  |  |  |
| Index und Indexergrenzwerte| 50 | 200 | 200 |  |  |  |  |

**S1** ist eine gebräuchliche Wahl, wenn dedizierte Ressourcen und mehrere Partitionen zur Notwendigkeit werden. Mit Partitionen von 25 GB für bis zu 12 Partitionen liegt der Pro-Dienst-Grenzwert für **S1** bei 300 GB insgesamt, wenn Sie Partitionen über Replikate maximieren (Informationen über ausgewogenere Kompositionen finden Sie unter [Standard-Tarife: Partitions- und Replikatskombinationen](search-capacity-planning.md#chart).)

Portal- und Preisseiten legen den Schwerpunkt auf Partitionsgröße und Speicher, aber für jeden Tarif nehmen die Computefunktionen (Datenträgerkapazität, Geschwindigkeit, CPUs) linear mit dem Preis zu. Ein **S2**-Replikat ist schneller als **S1**, und **S3** ist schneller als **S2**. **S3**-Tarife durchbrechen das allgemeine lineare Computepreismuster mit überproportional schnellerer E/A. Wenn Sie einen E/A-Engpass befürchten, wählen Sie einen **S3**-Tarif, der Ihnen viel mehr IOPS bietet als niedrigere Tarife.

**S3** und **S3 HD** werden durch eine Infrastruktur mit identischer hoher Kapazität unterstützt, aber jeder erreicht seinen maximalen Grenzwert auf unterschiedliche Weise. **S3** ist für eine kleinere Anzahl sehr umfangreicher Indizes ausgelegt. Damit ist der maximale Grenzwert ressourcengebunden (2,4 TB für jeden Dienst). **S3 HD** ist für eine große Zahl sehr kleiner Indizes ausgelegt. Bei 1.000 Indizes erreicht **S3 HD** seine Grenzwerte in Form von Indexeinschränkungen. Wenn Sie als **S3 HD**-Kunde mehr als 1.000 Indizes benötigen, erhalten Sie beim Microsoft-Support Informationen zur entsprechenden Vorgehensweise.

> [!NOTE]
> Bisher mussten Dokumentgrenzwerte berücksichtigt werden, doch bei neuen Diensten spielen sie keine Rolle mehr. Weitere Informationen zu Bedingungen, unter denen immer noch Dokumentgrenzwerte gelten, finden Sie unter [Dienstgrenzwerte: Dokumentgrenzwerte](search-limits-quotas-capacity.md#document-limits).
>

Die speicheroptimierten Tarife **L1 und L2** sind ideal für Anwendungen mit hohen Datenanforderungen und relativ geringer Endbenutzeranzahl, bei denen die Minimierung der Abfragewartezeit nicht an erster Stelle steht.  

|  | L1 | L2 |  |  |  |  |  |
|--|----|----|--|--|--|--|--|
| Partitionsgröße|  1 TB | 2 TB |  |  |  |  |  |
| Index und Indexergrenzwerte| 10 | 10 |  |  |  |  |  |

*L2* bietet insgesamt eine doppelt so hohe Speicherkapazität wie *L1*.  Orientieren Sie sich bei der Wahl Ihres Tarifs an der maximalen Datenmenge, die für Ihren Index voraussichtlich erforderlich sein wird.  Die Partitionen des Tarifs *L1* können in 1-TB-Schritten auf maximal 12 TB hochskaliert werden. Bei *L2* ist eine Erhöhung in 2-TB-Schritten pro Partition auf bis zu 24 TB möglich.

## <a name="evaluate-capacity"></a>Auswerten der Kapazität

Zwischen der Kapazität und den Kosten für die Dienstausführung besteht ein direkter Zusammenhang. Da Tarife Grenzwerte auf zwei Ebenen (Speicher und Ressourcen) erzwingen, sollten Sie beide berücksichtigen, da derjenige, den Sie zuerst erreichen, der effektive Grenzwert ist. 

In der Regel diktieren geschäftliche Anforderungen die Anzahl der Indizes, die Sie benötigen. Beispiel: ein globaler Index für ein großes Repository von Dokumenten, oder vielleicht mehrere Indizes basierend auf Region, Anwendung oder Geschäftsnische.

Um die Größe eines Indexes zu bestimmen, müssen Sie [einen erstellen](search-create-index-portal.md). Die Struktur der Daten in Azure Search ist in erster Linie ein [invertierter Index](https://en.wikipedia.org/wiki/Inverted_index), der über andere Eigenschaften als Quelldaten verfügt. Bei einem invertierten Index werden Größe und Komplexität vom Inhalt bestimmt, nicht notwendigerweise von der Menge der Daten, die Sie einspeisen. Aus einer großen Datenquelle mit massiver Redundanz könnte ein kleinerer Index resultieren als aus einem kleineren Dataset mit stark variierendem Inhalt. Daher ist es kaum möglich, die Indexgröße aus der Größe des ursprünglichen Datasets abzuleiten.

> [!NOTE] 
> Zwar mag sich das Abschätzen zukünftiger Anforderungen an Indizes und Speicher wie Mutmaßungen anfühlen, doch es lohnt sich. Wenn sich die Kapazität eines Tarifs als zu gering herausstellt, müssen Sie einen neuen Dienst in einem höheren Tarif bereitstellen und anschließend [die Indizes neu laden](search-howto-reindex.md). Für einen Dienst kann kein direktes SKU-Upgrade durchgeführt werden.
>

### <a name="step-1-develop-rough-estimates-using-the-free-tier"></a>Schritt 1: Entwickeln grober Schätzungen mit dem Free-Tarif

Eine Möglichkeit zum Schätzen der Kapazität ist, mit dem **Kostenlos**-Tarif zu beginnen. Bedenken Sie, dass der **Kostenlos**-Dienst bis zu 3 Indizes, 50 MB Speicherplatz und 2 Minuten Indizierungszeit bietet. Es kann schwierig sein, mit diesen Einschränkungen eine veranschlagte Indexgröße zu schätzen, aber das folgende Beispiel veranschaulicht eine Methode:

+ [Erstellen eines kostenlosen Diensts](search-create-service-portal.md)
+ Bereiten Sie ein kleines, repräsentatives Dataset vor (gehen Sie von 5.000 Dokumenten und einer Beispielgröße von zehn Prozent aus).
+ [Erstellen Sie einen anfänglichen Index](search-create-index-portal.md), und notieren Sie sich die Größe im Portal (angenommen 30 MB).

Angenommen, dass das Beispiel repräsentativ ist und zehn Prozent der gesamten Datenquelle umfasst, dann werden aus einem Index von 30 MB ca. 300 MB, wenn alle Dokumente indiziert werden. Aufgrund dieses vorläufigen Werts könnten Sie die Menge auf ein Budget für zwei Indizes (Entwicklung und Produktion) auf Speicheranforderungen von insgesamt 600 MB verdoppeln. Da diese Anforderung mühelos vom **Basic**-Tarif erfüllt wird, würden Sie dort beginnen.

### <a name="step-2-develop-refined-estimates-using-a-billable-tier"></a>Schritt 2: Entwickeln präziser Schätzungen mit einem kostenpflichtigen Tarif

Einige Kunden bevorzugen, mit dedizierten Ressourcen zu beginnen, die für größere Sampling- und Verarbeitungszeiten geeignet sind, und entwickeln dann während der Entwicklung realistische Schätzungen von Indexmenge, Größe und Abfragevolumen. Zu Beginn wird ein Dienst basierend auf einer Schätzung bereitgestellt, und wenn das Entwicklungsprojekt fortgeschritten ist, wissen Teams in der Regel, ob der derzeitige Dienst für die voraussichtlichen Produktionsworkloads über- oder unterdimensioniert ist. 

1. [Überprüfen Sie in jedem Tarif Dienstgrenzwerte](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#index-limits), um zu bestimmen, ob niedrigere Tarife die Menge der Indizes, die Sie benötigen, unterstützen können. In den Tarifen **Basic**-**S1**-**S2** betragen die Grenzwerte für Indizes jeweils 15-50-200.  Der Tarif **Speicheroptimiert** ist für eine geringe Anzahl sehr großer Indizes vorgesehen und daher auf zehn Indizes beschränkt.

1. [Erstellen Sie einen Dienst unter einem abzurechnenden Tarif](search-create-service-portal.md):

    + Starten Sie niedrig, mit **Basic** oder **S1**, wenn Sie am Anfang des Lernprozesses stehen.
    + Starten Sie hoch, mit **S2** oder sogar **S3**, wenn umfangreiche Indizierungs- und Abfrageworkloads auf der Hand liegen.
    + Verwenden Sie einen speicheroptimierten Tarif (**L1** oder **L2**), wenn Sie sehr viele Daten indizieren und die Abfragelast relativ gering ist (etwa im Fall einer internen Geschäftsanwendung).

1. [Erstellen Sie einen anfänglichen Index](search-create-index-portal.md), um zu bestimmen, wie Quelldaten in einen Index übersetzt werden. Dies ist die einzige Möglichkeit, die Größe des Indexes zu schätzen.

1. [Überwachen Sie Speicher, Dienstgrenzwerte, Abfragevolumen und Latenz](search-monitor-usage.md) im Portal. Das Portal zeigt die Abfragen pro Sekunde, gedrosselte Abfragen und die Wartezeit bei Suchvorgängen an. Auf der Grundlage dieser Werte können Sie entscheiden, ob Sie den passenden Tarif gewählt haben. Abgesehen von Portalmetriken können Sie umfassende Überwachung, z.B. die Durchklickanalyse, durch Aktivieren der [Datenverkehrsanalyse](search-traffic-analytics.md) konfigurieren. 

Indexzahl und -größe sind gleichermaßen für die Analyse relevant, da die maximalen Grenzwerte durch vollständige Auslastung des Speichers (Partitionen) oder maximale Grenzwerte für Ressourcen (Indizes, Indexer usw.) erreicht werden, je nachdem, was zuerst eintritt. Im Portal können Sie beides verfolgen, da die aktuelle Verwendung und die maximalen Grenzwerte nebeneinander auf der Seite „Übersicht“ angezeigt werden.

> [!NOTE]
> Der Speicherbedarf kann übermäßig zunehmen, wenn Dokumente überflüssige Daten enthalten. Im Idealfall enthalten Dokumente nur die Daten, die Sie für die Suchabfrage benötigen. Binärdaten sind nicht durchsuchbar und sollten separat (etwa in einer Azure-Tabelle oder in einem Blobspeicher) und mit einem Feld im Index gespeichert werden, das einen URL-Verweis auf die externen Daten enthält. Die maximale Größe eines einzelnen Dokuments beträgt 16 MB (oder weniger, wenn Sie im Rahmen einer einzelnen Anforderung mehrere Dokumente gleichzeitig hochladen). Weitere Informationen finden Sie unter [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md).
>

**Überlegungen zum Abfragevolumen**

Abfragen pro Sekunde (Queries-Per-Second, QPS) ist eine Metrik, die häufig bei der Leistungsoptimierung verwendet wird, aber im Allgemeinen bei der Tarifauswahl keine Rolle spielt, solange Sie nicht am Anfang ein hohes Abfragevolumen erwarten.

Die Standardtarife können eine Ausgewogenheit zwischen Replikaten und Partitionen bieten, wobei kürzere Abfrageverarbeitungszeiten durch zusätzliche Replikate für den Lastenausgleich und zusätzliche Partitionen für die parallele Verarbeitung unterstützt werden. Sie können die Leistung optimieren, sobald der Dienst bereitgestellt wird.

Kunden, die von Anfang an ein hohes durchgängiges Abfragevolumen erwarten, sollten höhere Tarife vom Typ **Standard** in Verbindung mit leistungsfähigerer Hardware in Erwägung ziehen. Sie können dann Partitionen und Replikate offline schalten oder auch zu einem niedrigeren Tarifdienst wechseln, wenn diese Abfragevolumen ausbleiben. Weitere Informationen zum Berechnen des Abfragedurchsatzes finden Sie unter [Überlegungen zur Leistung und Optimierung von Azure Search](search-performance-optimization.md).

Die speicheroptimierten Tarife eigenen sich für Workloads mit großen Datenmengen und unterstützen insgesamt mehr verfügbaren Indexspeicher, allerdings dürfen die Anforderungen hinsichtlich der Abfragewartezeit nicht zu hoch sein.  Dabei sollten weiterhin zusätzliche Replikate für den Lastenausgleich und zusätzliche Partitionen für die parallele Verarbeitung genutzt werden. Sie können die Leistung optimieren, sobald der Dienst bereitgestellt wird.

**Vereinbarungen zum Servicelevel (SLAs)**

Für Funktionen des **Kostenlos**-Tarifs und der Vorschauversion gelten keine [Vereinbarungen zum Servicelevel (Service Level Agreements, SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Für alle abrechenbaren Tarife gelten SLAs, wenn Sie genügend Redundanz für Ihren Dienst bereitstellen. Zwei oder mehr Replikate sind für die Abfrage-SLA (Lesezugriff) erforderlich. Drei oder mehr Replikate sind für die Abfrage- und Indizierungs-SLA (Lese-/Schreibzugriff) erforderlich. Die Anzahl der Partitionen wird für die SLA nicht berücksichtigt. 

## <a name="tips-for-tier-evaluation"></a>Tipps zur Tarifbewertung

+ Erfahren Sie, wie Sie effiziente Indizes erstellen, und welche Aktualisierungsmethoden die geringste Auswirkung haben. Wir empfehlen die [Datenverkehrsanalyse](search-traffic-analytics.md) für Erkenntnisse zur Abfrageaktivität.

+ Erstellen Sie Metriken zu Abfragen, und sammeln Sie Daten zu Verwendungsmustern (Abfragen während der Geschäftszeiten, Indizierung außerhalb der Spitzenzeiten), und treffen Sie anhand dieser Daten Entscheidungen zur zukünftigen Dienstbereitstellung. Da ein stündliches oder tägliches Intervall nicht praktikabel ist, können Sie Partitionen und Ressourcen dynamisch anpassen, um geplante Änderungen von Abfragevolumen oder ungeplante, jedoch nachhaltige Änderungen zu berücksichtigen, wenn Ebenen beständig genug sind, um die Durchführung von Aktionen zu garantieren.

+ Beachten Sie, dass der einzige Nachteil einer unterdimensionierten Bereitstellung ist, dass Sie ggf. einen Dienst entfernen müssen, wenn die tatsächlichen Anforderungen Ihre Schätzungen überschreiten. Um Dienstunterbrechungen zu vermeiden, sollten Sie einen neuen Dienst im gleichen Abonnement in einem höheren Tarif erstellen und parallel ausführen, bis alle Apps und Anforderungen auf den neuen Endpunkt abzielen.

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit einem **Kostenlos**-Tarif, und erstellen Sie einen anfänglichen Index mit einer Teilmenge der Daten, um deren Eigenschaften zu verstehen. Die Datenstruktur in Azure Search ist ein invertierter Index, wobei Größe und Komplexität des invertierten Indexes sich nach dem Inhalt richten. Denken Sie daran, dass hoch redundanter Inhalt eher zu einem kleineren Index tendiert als sehr unregelmäßiger Inhalt. Daher bestimmen eher die Inhaltseigenschaften als die Größe des Datasets die Indexspeicheranforderungen.

Sobald Sie eine erste Vorstellung von der Indexgröße haben, können Sie in einem der in diesem Artikel erläuterten Tarife (**Basic**, **Standard** oder **Speicheroptimiert**) [einen abrechenbaren Dienst bereitstellen](search-create-service-portal.md). Lockern Sie künstliche Einschränkungen im Zusammenhang mit der Dimensionierung von Daten, und [erstellen Sie den Index neu](search-howto-reindex.md), um alle Daten einzuschließen, die tatsächlich durchsuchbar sein sollen.

[Ordnen Sie Partitionen und Replikate zu](search-capacity-planning.md), wie es erforderlich ist, um die gewünschte Leistung und Skalierung zu erzielen.

Wenn Leistung und Kapazität in Ordnung sind, haben Sie Ihr Ziel erreicht. Erstellen Sie andernfalls erneut einen Suchdienst in einem anderen Tarif, der Ihren Anforderungen eher entspricht.

> [!NOTE]
> Um weitere Hilfe zu Fragen zu erhalten, posten Sie in [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search), oder [wenden Sie sich an den Azure-Support](https://azure.microsoft.com/support/options/).
