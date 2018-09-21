---
title: Auswählen einer SKU oder eines Tarifs für den Azure Search-Dienst | Microsoft-Dokumentation
description: 'Azure Search kann unter folgenden SKUs bereitgestellt werden: Free, Basic und Standard. Standard ist mit verschiedenen Ressourcenkonfigurationen und Kapazitäten verfügbar.'
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: heidist
ms.openlocfilehash: 140daf4903c64d734182545cd4dc58db60274852
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576119"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Auswählen eines Tarifs für Azure Search

In Azure Search erfolgt die [Bereitstellung eines Diensts](search-create-service-portal.md) für einen bestimmten Tarif oder eine bestimmte SKU. Folgende Optionen stehen zur Verfügung: **Free**, **Basic** und **Standard**, wobei **Standard** mit verschiedenen Konfigurationen und Kapazitäten verfügbar ist. 

Dieser Artikel soll Ihnen bei der Auswahl eines Tarifs helfen. Er ergänzt die [Preisseite](https://azure.microsoft.com/pricing/details/search/) und die Seite [Dienstgrenzwerte](search-limits-quotas-capacity.md) mit einer Auswahl von Abrechnungskonzepten und Verbrauchsmustern, die verschiedenen Tarifen zugeordnet sind. Er empfiehlt auch eine iterative Vorgehensweise, um zu verstehen, welcher Tarif Ihren Anforderungen am besten entspricht. 

Tarife bestimmen Kapazität, nicht Features. Wenn sich die Kapazität eines Tarifs als zu gering herausstellt, müssen Sie einen neuen Dienst in einem höheren Tarif bereitstellen und anschließend [die Indizes neu laden](search-howto-reindex.md). Für einen Dienst kann kein direktes SKU-Upgrade durchgeführt werden.

Die Verfügbarkeit von Features ist kein primärer Tarifaspekt. Alle Tarife, einschließlich des Tarifs **Kostenlos**, bieten Featureparität, ausgenommen die Indexer-Unterstützung für S3HD. Die Indizierungs- und Ressourceneinschränkungen können jedoch den Umfang der Featureverwendung empfindlich einschränken. Beispielsweise ist die Indizierung für [kognitive Suche](cognitive-search-concept-intro.md) mit Vorgängen mit langer Laufzeit verbunden, sodass bei einem kostenlosen Dienst ein Timeout auftritt, wenn das Dataset sehr klein ist.

> [!TIP]
> Die meisten Kunden beginnen mit dem Tarif **Kostenlos** für die Evaluierung und steigen dann für die Entwicklung zu **Standard** auf. Nach dem Auswählen eines Tarifs und [Bereitstellen eines Suchdiensts](search-create-service-portal.md) können Sie die [Anzahl der Replikate und Partitionen](search-capacity-planning.md) zur Leistungsoptimierung erhöhen. Weitere Informationen dazu, wann und warum Sie Kapazität anpassen sollten, finden Sie unter [Überlegungen zur Leistung und Optimierung von Azure Search](search-performance-optimization.md).
>

## <a name="billing-concepts"></a>Abrechnungskonzepte

Zu den Konzepten, die Sie für die Tarifauswahl kennen müssen, zählen Kapazitätsdefinitionen, Dienstgrenzwerte und Diensteinheiten. 

### <a name="capacity"></a>Capacity

Kapazität wird in *Replikate* und *Partitionen* strukturiert. 

+ Replikate sind Instanzen des Suchdiensts, in denen jedes Replikat eine Lastenausgleichskopie eines Indexes hostet. Beispielsweise verfügt ein Dienst mit 6 Replikaten über 6 Kopien jedes in den Dienst geladenen Indexes. 

+ Partitionen speichern Indizes und teilen durchsuchbare Daten automatisch auf: zwei Partitionen teilen den Index in Hälften, drei Partitionen in Drittel usw. Im Hinblick auf die Kapazität ist die *Partitionsgröße* das primäre unterscheidende Feature der Tarife.

> [!NOTE]
> Alle **Standard**-Tarife unterstützen [flexible Kombinationen von Replikaten und Partitionen](search-capacity-planning.md#chart), damit Sie durch Ändern des Ausgleichs [Ihr System nach Geschwindigkeit oder Speicher gewichten](search-performance-optimization.md) können. **Basic** bietet bis zu drei Replikate für Hochverfügbarkeit, enthält jedoch nur eine Partition. **Kostenlos**-Tarife bieten keine dedizierten Ressourcen: Computingressourcen werden von mehreren kostenlosen Diensten geteilt.

### <a name="search-units"></a>Sucheinheiten

Das wichtigste Abrechnungskonzept, das Sie kennen sollten, ist eine *Sucheinheit* (Search Unit, SU), die Abrechnungseinheit für Azure Search. Da die Funktion von Azure Search sowohl von Replikaten als auch Partitionen abhängig ist, ist es nicht sinnvoll, entweder nach dem einen oder dem anderen abzurechnen. Stattdessen basiert die Abrechnung auf einer Kombination beider. In einer Formel ist eine SU das Produkt der von einem Dienst verwendeten Replikate und Partitionen: (R X P = SU). Jeder Dienst beginnt mit mindestens 1 SU (ein Replikat multipliziert mit einer einzigen Partition), aber ein realistischeres Modell wäre ein Dienst mit 3 Replikaten und 3 Partitionen, der mit 9 SUs abgerechnet wird. 

Obwohl jeder Tarif zunehmend höhere Kapazität bietet, können Sie einen Teil der Gesamtkapazität online bringen und den Rest als Reserve zurückhalten. Im Hinblick auf die Abrechnung bestimmt die Anzahl der Partitionen und Replikate, die Sie online bringen, mithilfe der SU-Formel berechnet, was Sie tatsächlich bezahlen.

Es wird stündlich pro SU abgerechnet, wobei jeder Tarif eine andere Rate hat. Die Preise für die einzelnen Tarife finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/search/).

### <a name="limits"></a>Einschränkungen

Bedient Hostressourcen, z.B. Indizes, Indexer usw. Jeder Tarif erzwingt [Dienstgrenzwerte](search-limits-quotas-capacity.md) bezüglich der Menge an Ressourcen, die Sie erstellen können. Daher ist eine Obergrenze für die Anzahl der Indizes (und anderer Objekte) das zweite Unterscheidungsmerkmal zwischen Tarifen. Wenn Sie die einzelnen Optionen im Portal durchgehen, beachten Sie die Grenzwerte für die Anzahl der Indizes. Andere Ressourcen, z.B. Indexer, Datenquellen und Skillsets, sind an Indexgrenzwerte gebunden.

## <a name="consumption-patterns"></a>Verbrauchsmuster

Die meisten Kunden beginnen mit dem **Kostenlos**-Tarif, den sie unbegrenzt beibehalten, und wählen dann einen der **Standard**-Tarife für ernsthafte Entwicklungs- oder Produktionsworkloads. 

![Azure Search-Tarife](./media/search-sku-tier/tiers.png "Azure Search-Tarife")

An den jeweiligen Enden sind **Basic** und **S3 HD** für wichtige, aber untypische Verbrauchsmuster vorhanden. **Basic** ist für kleine Produktionsworkloads bestimmt: Er bietet SLA, dedizierte Ressourcen, hohe Verfügbarkeit, aber moderaten Speicher, der höchstens 2 GB insgesamt erreicht. Dieser Tarif wurde für Kunden entwickelt, die die verfügbare Kapazität konsistent unterschritten haben. Am anderen Ende ist **S3 HD** für Workloads vorgesehen, die für unabhängige Softwareanbieter, Partner, [mehrinstanzfähige Lösungen](search-modeling-multitenant-saas-applications.md) oder jede Konfiguration typisch sind, die eine große Anzahl kleiner Indizes aufruft. Es ist häufig klar, wenn der **Basic**- oder **S3 HD**-Tarif passend ist, aber zur Bestätigung können Sie in [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) posten oder den [Azure Support kontaktieren](https://azure.microsoft.com/support/options/), um weitere Hinweise zu erhalten.

Betrachten wir nun die gebräuchlicheren Standardtarife: **S1-S3** bilden eine Steigerung mit zunehmenden Kapazitätsstufen, wobei die Wendepunkte bei Partitionsgröße und maximaler Anzahl von Indizes, Indexern und anderen folgerichtigen Ressourcen liegen:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Partitionsgröße|  25 GB | 100 GB | 250 GB |  |  |  |  |
| Index und Indexergrenzwerte| 50 | 200 | 200 |  |  |  |  |

**S1** ist eine gebräuchliche Wahl, wenn dedizierte Ressourcen und mehrere Partitionen zur Notwendigkeit werden. Mit Partitionen von 25 GB für bis zu 12 Partitionen liegt der Pro-Dienst-Grenzwert für **S1** bei 300 GB insgesamt, wenn Sie Partitionen über Replikate maximieren (Informationen über ausgewogenere Kompositionen finden Sie unter [Standard-Tarife: Partitions- und Replikatskombinationen](search-capacity-planning.md#chart).)

Portal- und Preisseiten legen den Schwerpunkt auf Partitionsgröße und Speicher, aber für jeden Tarif nehmen die Computefunktionen (Datenträgerkapazität, Geschwindigkeit, CPUs) linear mit dem Preis zu. Ein **S2**-Replikat ist schneller als **S1**, und **S3** ist schneller als **S2**. **S3**-Tarife durchbrechen das allgemeine lineare Computepreismuster mit überproportional schnellerer E/A. Wenn Sie einen E/A-Engpass befürchten, wählen Sie einen **S3**-Tarif, der Ihnen viel mehr IOPS bietet als niedrigere Tarife.

**S3** und **S3 HD** werden durch eine Infrastruktur mit identischer hoher Kapazität unterstützt, aber jeder erreicht seinen maximalen Grenzwert auf unterschiedliche Weise. **S3** ist für eine kleinere Anzahl sehr umfangreicher Indizes ausgelegt. Damit ist der maximale Grenzwert ressourcengebunden (2,4 TB für jeden Dienst). **S3 HD** ist für eine große Zahl sehr kleiner Indizes ausgelegt. Bei 1.000 Indizes erreicht **S3 HD** seine Grenzwerte in Form von Indexeinschränkungen. Wenn Sie als **S3 HD**-Kunde mehr als 1.000 Indizes benötigen, erhalten Sie beim Microsoft-Support Informationen zur entsprechenden Vorgehensweise.

> [!NOTE]
> Bisher mussten Dokumentgrenzwerte berücksichtigt werden, doch bei den meisten nach Januar 2018 bereitgestellten Azure Search-Diensten spielen sie keine Rolle mehr. Weitere Informationen zu Bedingungen, für die immer noch Dokumentgrenzwerte gelten, finden Sie unter [Dienstgrenzwerte: Dokumentgrenzwerte](search-limits-quotas-capacity.md#document-limits).
>

## <a name="evaluate-capacity"></a>Auswerten der Kapazität

Zwischen der Kapazität und den Kosten für die Dienstausführung besteht ein direkter Zusammenhang. Da Tarife Grenzwerte auf zwei Ebenen (Speicher und Ressourcen) erzwingen, sollten Sie beide berücksichtigen, da derjenige, den Sie zuerst erreichen, der effektive Grenzwert ist. 

In der Regel diktieren geschäftliche Anforderungen die Anzahl der Indizes, die Sie benötigen. Beispiel: ein globaler Index für ein großes Repository von Dokumenten, oder vielleicht mehrere Indizes basierend auf Region, Anwendung oder Geschäftsnische.

Um die Größe eines Indexes zu bestimmen, müssen Sie [einen erstellen](search-create-index-portal.md). Die Struktur der Daten in Azure Search ist in erster Linie ein [invertierter Index](https://en.wikipedia.org/wiki/Inverted_index), der über andere Eigenschaften als Quelldaten verfügt. Bei einem invertierten Index werden Größe und Komplexität vom Inhalt bestimmt, nicht notwendigerweise von der Menge der Daten, die Sie einspeisen. Aus einer großen Datenquelle mit massiver Redundanz könnte ein kleinerer Index resultieren als aus einem kleineren Dataset mit stark variierendem Inhalt.  Daher ist es kaum möglich, die Indexgröße aus der Größe des ursprünglichen Datasets abzuleiten.

### <a name="step-1-develop-rough-estimates-using-the-free-tier"></a>Schritt 1: Entwickeln grober Schätzungen mit dem Free-Tarif

Eine Möglichkeit zum Schätzen der Kapazität ist, mit dem **Kostenlos**-Tarif zu beginnen. Bedenken Sie, dass der **Kostenlos**-Dienst bis zu 3 Indizes, 50 MB Speicherplatz und 2 Minuten Indizierungszeit bietet. Es kann schwierig sein, mit diesen Einschränkungen eine veranschlagte Indexgröße zu schätzen, aber das folgende Beispiel veranschaulicht eine Methode:

+ [Erstellen eines kostenlosen Diensts](search-create-service-portal.md)
+ Bereiten Sie ein kleines, repräsentatives Dataset vor (gehen Sie von 5.000 Dokumenten und einer Beispielgröße von zehn Prozent aus).
+ [Erstellen Sie einen anfänglichen Index](search-create-index-portal.md), und notieren Sie sich die Größe im Portal (angenommen 30 MB).

Angenommen, dass das Beispiel repräsentativ ist und zehn Prozent der gesamten Datenquelle umfasst, dann werden aus einem Index von 30 MB ca. 300 MB, wenn alle Dokumente indiziert werden. Aufgrund dieses vorläufigen Werts könnten Sie die Menge auf ein Budget für zwei Indizes (Entwicklung und Produktion) auf Speicheranforderungen von insgesamt 600 MB verdoppeln. Da diese Anforderung mühelos vom **Basic**-Tarif erfüllt wird, würden Sie dort beginnen.

### <a name="step-2-develop-refined-estimates-using-a-billable-tier"></a>Schritt 2: Entwickeln präziser Schätzungen mit einem kostenpflichtigen Tarif

Einige Kunden bevorzugen, mit dedizierten Ressourcen zu beginnen, die für größere Sampling- und Verarbeitungszeiten geeignet sind, und entwickeln dann während der Entwicklung realistische Schätzungen von Indexmenge, Größe und Abfragevolumen. Zu Beginn wird ein Dienst basierend auf einer Schätzung bereitgestellt, und wenn das Entwicklungsprojekt fortgeschritten ist, wissen Teams in der Regel, ob der derzeitige Dienst für die voraussichtlichen Produktionsworkloads über- oder unterdimensioniert ist. 

1. [Überprüfen Sie in jedem Tarif Dienstgrenzwerte](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#index-limits), um zu bestimmen, ob niedrigere Tarife die Menge der Indizes, die Sie benötigen, unterstützen können. In den Tarifen **Basic**-**S1**- **S2** betragen die Grenzwerte für Indizes jeweils 15-50-200.

1. [Erstellen Sie einen Dienst unter einem abzurechnenden Tarif](search-create-service-portal.md):

    + Starten Sie niedrig, mit **Basic** oder **S1**, wenn Sie am Anfang des Lernprozesses stehen.
    + Starten Sie hoch, mit **S2** oder sogar **S3**, wenn umfangreiche Indizierungs- und Abfrageworkloads auf der Hand liegen.

1. [Erstellen Sie einen anfänglichen Index](search-create-index-portal.md), um zu bestimmen, wie Quelldaten in einen Index übersetzt werden. Dies ist die einzige Möglichkeit, die Größe des Indexes zu schätzen.

1. [Überwachen Sie Speicher, Dienstgrenzwerte, Abfragevolumen und Latenz](search-monitor-usage.md) im Portal. Das Portal zeigt die Abfragen pro Sekunde, eingeschränkte Abfragen und die Wartezeit bei Suchvorgängen an – alles, anhand dessen Sie entscheiden können, ob Sie den richtigen Tarif gewählt haben. Abgesehen von Portalmetriken können Sie umfassende Überwachung, z.B. die Durchklickanalyse, durch Aktivieren der [Datenverkehrsanalyse](search-traffic-analytics.md) konfigurieren. 

Indexzahl und -größe sind gleichermaßen für die Analyse relevant, da die maximalen Grenzwerte durch vollständige Auslastung des Speichers (Partitionen) oder maximale Grenzwerte für Ressourcen (Indizes, Indexer usw.) erreicht werden, je nachdem, was zuerst eintritt. Im Portal können Sie beides verfolgen, da die aktuelle Verwendung und die maximalen Grenzwerte nebeneinander auf der Seite „Übersicht“ angezeigt werden.

> [!NOTE]
> Der Speicherbedarf kann übermäßig zunehmen, wenn Dokumente überflüssige Daten enthalten. Im Idealfall enthalten Dokumente nur die Daten, die Sie für die Suchabfrage benötigen. Binärdaten sind nicht durchsuchbar und sollten separat (etwa in einer Azure-Tabelle oder in einem Blobspeicher) und mit einem Feld im Index gespeichert werden, das einen URL-Verweis auf die externen Daten enthält. Die maximale Größe eines einzelnen Dokuments beträgt 16 MB (oder weniger, wenn Sie im Rahmen einer einzelnen Anforderung mehrere Dokumente gleichzeitig hochladen). Weitere Informationen finden Sie unter [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md).
>

**Überlegungen zum Abfragevolumen**

Abfragen pro Sekunde (Queries-Per-Second, QPS) ist eine Metrik, die häufig bei der Leistungsoptimierung verwendet wird, aber in der Regel bei der Tarifauswahl keine Rolle spielt, solange Sie nicht am Anfang ein sehr hohes Abfragevolumen erwarten.

Alle Standardtarife können eine Ausgewogenheit zwischen Replikaten und Partitionen bieten, unterstützen eine kürzere Abfrageverarbeitungszeit durch zusätzliche Replikate für den Lastenausgleich und zusätzliche Partitionen für die parallele Verarbeitung. Sie können die Leistung optimieren, sobald der Dienst bereitgestellt wird.

Kunden, die von Anfang an ein hohes durchgängiges Abfragevolumen erwarten, sollten höhere Tarife in Verbindung mit leistungsfähigerer Hardware in Erwägung ziehen. Sie können dann Partitionen und Replikate offline schalten oder auch zu einem niedrigeren Tarifdienst wechseln, wenn diese Abfragevolumen ausbleiben. Weitere Informationen zum Berechnen des Abfragedurchsatzes finden Sie unter [Überlegungen zur Leistung und Optimierung von Azure Search](search-performance-optimization.md).


**Vereinbarungen zum Servicelevel (SLAs)**

Für Funktionen des **Kostenlos**-Tarifs und der Vorschauversion gelten keine [Vereinbarungen zum Servicelevel (Service Level Agreements, SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Für alle abrechenbaren Tarife gelten SLAs, wenn Sie genügend Redundanz für Ihren Dienst bereitstellen. Zwei oder mehr Replikate sind für die Abfrage-SLA (Lesezugriff) erforderlich. Drei oder mehr Replikate sind für die Abfrage- und Indizierungs-SLA (Lese-/Schreibzugriff) erforderlich. Die Anzahl der Partitionen wird für die SLA nicht berücksichtigt. 

## <a name="tips-for-tier-evaluation"></a>Tipps zur Tarifbewertung

+ Erfahren Sie, wie Sie effiziente Indizes erstellen, und welche Aktualisierungsmethoden die geringste Auswirkung haben. Wir empfehlen die [Datenverkehrsanalyse](search-traffic-analytics.md) für Erkenntnisse zur Abfrageaktivität.

+ Erstellen Sie Metriken zu Abfragen, und sammeln Sie Daten zu Verwendungsmustern (Abfragen während der Geschäftszeiten, Indizierung außerhalb der Spitzenzeiten), und treffen Sie anhand dieser Daten Entscheidungen zur zukünftigen Dienstbereitstellung. Da es auf stündlicher oder täglicher Ebene nicht praktikabel ist, können Sie Partitionen und Ressourcen dynamisch anpassen, um geplante Änderungen von Abfragevolumen oder ungeplante, jedoch nachhaltige Änderungen aufzunehmen, wenn Ebenen beständig genug sind, um die Durchführung von Aktionen zu garantieren.

+ Beachten Sie, dass der einzige Nachteil einer unterdimensionierten Bereitstellung ist, dass Sie ggf. einen Dienst entfernen müssen, wenn die tatsächlichen Anforderungen Ihre Schätzungen überschreiten. Um Dienstunterbrechungen zu vermeiden, sollten Sie einen neuen Dienst im gleichen Abonnement in einem höheren Tarif erstellen und parallel ausführen, bis alle Apps und Anforderungen auf den neuen Endpunkt abzielen.

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit einem **Kostenlos**-Tarif, und erstellen Sie einen anfänglichen Index mit einer Teilmenge der Daten, um deren Eigenschaften zu verstehen. Die Datenstruktur in Azure Search ist ein invertierter Index, wobei Größe und Komplexität des invertierten Indexes sich nach dem Inhalt richten. Denken Sie daran, dass hoch redundanter Inhalt eher zu einem kleineren Index tendiert als sehr unregelmäßiger Inhalt. Daher bestimmen eher die Inhaltseigenschaften als die Größe des Datasets die Indexspeicheranforderungen.

Sobald Sie eine erste Vorstellung von der Indexgröße haben, [stellen Sie einen abrechenbaren Dienst bereit](search-create-service-portal.md) in einem der in diesem Artikel erläuterten Tarife, entweder **Basic** oder **Standard**. Lockern Sie künstliche Einschränkungen für Teilmengen von Daten, und [erstellen Sie den Index neu](search-howto-reindex.md), um alle Daten einzuschließen, die tatsächlich durchsuchbar sein sollen.

[Ordnen Sie Partitionen und Replikate zu](search-capacity-planning.md), wie es erforderlich ist, um die gewünschte Leistung und Skalierung zu erzielen.

Wenn Leistung und Kapazität in Ordnung sind, haben Sie Ihr Ziel erreicht. Erstellen Sie andernfalls erneut einen Suchdienst in einem anderen Tarif, der Ihren Anforderungen eher entspricht.

> [!NOTE]
> Um weitere Hilfe zu Fragen zu erhalten, posten Sie in [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search), oder [wenden Sie sich an den Azure-Support](https://azure.microsoft.com/support/options/).