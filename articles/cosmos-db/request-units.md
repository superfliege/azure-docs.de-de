---
title: Anforderungseinheiten und Schätzen des Durchsatzes – Azure Cosmos DB | Microsoft-Dokumentation
description: Enthält Informationen zu den Grundlagen von Anforderungseinheiten in Azure Cosmos DB sowie zur Angabe und zur Schätzung des Bedarfs an Anforderungseinheiten.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: 0aa87aeaf852d7309c29c1298e326c101a944904
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="request-units-in-azure-cosmos-db"></a>Anforderungseinheiten in Azure Cosmos DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) ist eine global verteilte Datenbank von Microsoft mit mehreren Modellen. Mit Azure Cosmos DB müssen Sie keine virtuellen Computer mieten, Software bereitstellen oder Datenbanken überwachen. Azure Cosmos DB wird von Microsoft-Entwicklern betrieben und ständig überwacht, um erstklassige Verfügbarkeit, Leistung und Datensicherheit zu gewährleisten. Sie können über APIs Ihrer Wahl auf Ihre Daten zugreifen, z.B. über die [SQL-API](documentdb-introduction.md), die [MongoDB-API](mongodb-introduction.md) oder die [Tabellen-API](table-introduction.md). Auf Graph können Sie über die [Gremlin-API](graph-introduction.md) zugreifen. Alle APIs werden nativ unterstützt. 

Die Währung von Azure Cosmos DB ist die **Anforderungseinheit (Request Unit, RU)**. Dank der Anforderungseinheiten ist es nicht erforderlich, Kapazitäten für Lese- und Schreibvorgänge zu reservieren oder CPU, Arbeitsspeicher und IOPS bereitzustellen. Azure Cosmos DB unterstützt eine Reihe von APIs mit anderen Vorgängen, die von Lese- und Schreibvorgängen bis hin zu komplexen Graph-Abfragen reichen. Da nicht alle Anforderungen gleich sind, wird ihnen eine normalisierte Menge von **Anforderungseinheiten** zugewiesen, die auf dem für das Bedienen der Anforderung erforderlichen Rechenaufwand basiert. Die Anzahl der Anforderungseinheiten für einen Vorgang ist deterministisch, und Sie können die Anzahl der von den einzelnen Vorgängen genutzten Anforderungseinheiten in Azure Cosmos DB über einen Antwortheader verfolgen. 

Sie müssen einen Durchsatz von 100 RU/Sekunde reservieren, um eine vorhersagbare Leistung bereitstellen zu können. Sie können [Ihre Durchsatzanforderungen schätzen](request-units.md#estimating-throughput-needs), indem Sie den [Anforderungseinheitenrechner](https://www.documentdb.com/capacityplanner) von Azure Cosmos DB verwenden.

![Durchsatzrechner][5]

Nach Lesen dieses Artikels können Sie die folgenden Fragen beantworten:  

* Was sind Anforderungseinheiten und Anforderungsgebühren in Azure Cosmos DB?
* Wie gebe ich die Kapazität der Anforderungseinheiten für einen Container oder eine Gruppe von Containern in Azure Cosmos DB an?
* Wie schätze ich die benötigten Anforderungseinheiten für meine Anwendung?
* Was geschieht, wenn ich die Kapazität der Anforderungseinheiten für einen Container oder eine Gruppe von Containern in Azure Cosmos DB überschreite?

Azure Cosmos DB ist eine Datenbank mit mehreren Modellen. Beachten Sie daher, dass dieser Artikel für alle Datenmodelle und APIs in Azure Cosmos DB gilt. In diesem Artikel werden generische Begriffe verwendet, z.B. *Container* und *Element*, um generisch auf eine Sammlung, einen Graph oder eine Tabelle und ein Dokument, einen Knoten oder eine Entität zu verweisen.

## <a name="request-units-and-request-charges"></a>Anforderungseinheiten und Anforderungsgebühren
Azure Cosmos DB bietet eine schnelle, vorhersagbare Leistung durch die *Reservierung* von Ressourcen, die dem benötigten Durchsatz für Ihre Anwendung entsprechen.  Da sich Anwendungsauslastung und Zugriffsmuster mit der Zeit ändern, können Sie mit Azure Cosmos DB den Umfang des reservierten Durchsatzes, der Ihrer Anwendung zur Verfügung steht, ganz einfach erhöhen oder verringern.

Bei Azure Cosmos DB wird der reservierte Durchsatz in Anforderungseinheiten pro Sekunde angegeben. Sie können sich Anforderungseinheiten als Währung für den Durchsatz vorstellen, wobei Sie eine Anzahl garantierter Anforderungseinheiten auf Sekundenbasis für Ihre Anwendung *reservieren* .  Jeder Vorgang in Azure Cosmos DB – das Schreiben eines Dokuments, das Durchführen einer Abfrage, das Aktualisieren eines Dokuments – beansprucht CPU, Arbeitsspeicher und IOPS.  Mit anderen Worten: Für jeden Vorgang fällt eine *Anforderungsgebühr* an, die in *Anforderungseinheiten* ausgedrückt wird.  Wenn Sie die Faktoren, die sich auf die berechneten Anforderungseinheiten auswirken, sowie die Durchsatzanforderungen Ihrer Anwendung genau kennen, können Sie die Kosten für Ihre Anwendung optimieren. Der Daten-Explorer im Azure-Portal ist auch ein hervorragendes Tool zum Testen des Kerns einer Abfrage.

Sie sollten die ersten Schritte mit dem Betrachten des folgenden Videos beginnen, in dem Azure Cosmos DB-Programm-Manager Andrew Liu Anforderungseinheiten erläutert.

> [!VIDEO https://www.youtube.com/embed/stk5WSp5uX0]
> 
> 

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Durchsatzisolation in global verteilten Datenbanken

Wenn Sie Ihre Datenbank in mehreren Regionen repliziert haben, stellt Azure Cosmos DB mittels Durchsatzisolation sicher, dass die RU-Nutzung in einer Region die RU-Nutzung in einer anderen Region nicht beeinträchtigt. Schreiben Sie beispielsweise Daten in eine Region und lesen Daten aus einer anderen Region, werden für den Schreibvorgang in Region *A* und den Lesevorgang in Region *B* jeweils separate RUs eingesetzt. Eine RU wird nicht unter den Regionen aufgeteilt, in denen Sie sie bereitgestellt haben. Für jede Region, in der die Datenbank repliziert wird, wird die volle Anzahl an RUs bereitgestellt. Weitere Informationen über die globale Replikation finden Sie unter [Wie werden Daten mit Azure Cosmos DB global verteilt?](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>Aspekte zu Anforderungseinheiten
Beim Abschätzen der Anzahl der bereitzustellenden Anforderungseinheiten sollten Sie unbedingt die folgenden Variablen berücksichtigen:

* **Elementgröße**. Mit zunehmender Größe werden auch mehr Anforderungseinheiten für das Lesen und Schreiben der Daten genutzt.
* **Anzahl der Elementeigenschaften**. Eine Standardindizierung aller Eigenschaften vorausgesetzt, werden mehr Einheiten für das Schreiben eines Dokuments, eines Knotens oder einer Entität genutzt, wenn die Anzahl der Eigenschaften steigt.
* **Datenkonsistenz**. Bei Verwendung der Datenkonsistenzmodelle „Strong“ oder „Bounded Staleness“ werden zusätzliche Anforderungseinheiten zum Lesen von Elementen genutzt.
* **Indizierte Eigenschaften**. Eine Indexrichtlinie für jeden Container gibt an, welche Eigenschaften standardmäßig indiziert werden. Sie können die Nutzung von Anforderungseinheiten für Schreibvorgänge reduzieren, indem Sie die Anzahl der indizierten Eigenschaften begrenzen oder die verzögerte Indizierung aktivieren.
* **Dokumentindizierung**. Standardmäßig wird jedes Element automatisch indiziert. Sie verbrauchen weniger Anforderungseinheiten, wenn Sie einige Elemente nicht indizieren.
* **Abfragemuster**. Die Komplexität einer Abfrage wirkt sich darauf aus, wie viele Anforderungseinheiten für einen Vorgang verbraucht werden. Die Anzahl der Abfrageergebnisse und Prädikate, die Art der Prädikate, Projektionen, die Anzahl von UDFs und die Größe der Quelldaten beeinflussen die Kosten von Abfragevorgängen.
* **Skriptnutzung**.  Wie bei Abfragen beanspruchen gespeicherte Prozeduren und Trigger Anforderungseinheiten basierend auf der Komplexität des ausgeführten Vorgangs. Untersuchen Sie während der Entwicklung Ihrer Anwendung den "x-ms-request-charge"-Header, um herauszufinden, wie viel Anforderungseinheiten die einzelnen Vorgänge verbrauchen.

## <a name="estimating-throughput-needs"></a>Schätzen der Durchsatzanforderungen
Eine Anforderungseinheit ist eine normalisierte Kennzahl für die Anforderungsverarbeitungskosten. Eine einzelne Anforderungseinheit stellt die Verarbeitungskapazität dar, die erforderlich ist, um ein einzelnes, aus 10 eindeutigen Eigenschaftswerten (außer Systemeigenschaften) bestehendes Element von 1 KB zu lesen (per „self link“ oder ID). Eine Anforderung zum Erstellen (Einfügen), Ersetzen oder Löschen des gleichen Elements verbraucht mehr Verarbeitungsleistung des Diensts und daher mehr Anforderungseinheiten.   

> [!NOTE]
> Die Baseline einer Anforderungseinheit für ein Element von 1 KB entspricht einem einfachen GET-Vorgang per „self link“ oder ID des Elements.
> 
> 

In der Tabelle unten ist beispielsweise angegeben, wie viele Anforderungseinheiten bei Elementen drei unterschiedlichen Größen (1 KB, 4 KB und 64 KB) und bei zwei unterschiedlichen Leistungsebenen (500 Lesevorgänge/Sekunde + 100 Schreibvorgänge/Sekunde und 500 Lesevorgänge/Sekunde + 500 Schreibvorgänge/Sekunde) bereitgestellt werden sollten. Für die Datenkonsistenz wurde *Session* konfiguriert, und die Indizierungsrichtlinie wurde auf *None* festgelegt.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Elementgröße</strong></p></td>
            <td valign="top"><p><strong>Lesevorgänge/Sekunde</strong></p></td>
            <td valign="top"><p><strong>Schreibvorgänge/Sekunde</strong></p></td>
            <td valign="top"><p><strong>Anforderungseinheiten</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1) + (100 * 5) = 1.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1) + (500 * 5) = 3.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1,3) + (100 * 7) = 1.350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1,3) + (500 * 7) = 4.150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 10) + (100 * 48) = 9.800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 10) + (500 * 48) = 29.000 RU/s</p></td>
        </tr>
    </tbody>
</table>

### <a name="use-the-request-unit-calculator"></a>Verwenden des Rechners für Anforderungseinheiten
Damit Kunden ihre Durchsatzschätzungen optimieren können, gibt es einen webbasierten [Rechner für Anforderungseinheiten](https://www.documentdb.com/capacityplanner), um den Bedarf an Anforderungseinheiten für normale Vorgänge zu schätzen. Dazu gehören:

* Erstellen von Elementen (Schreiben)
* Lesen von Elementen
* Löschen von Elementen
* Aktualisieren von Elementen

Das Tool unterstützt auch die Schätzung des Datenspeicherbedarfs auf der Grundlage der bereitgestellten Beispielelemente.

Die Verwendung des Tools ist einfach:

1. Laden Sie mindestens ein repräsentatives Element (z.B. ein JSON-Beispieldokument) hoch.
   
    ![Hochladen von Elementen in den Rechner für Anforderungseinheiten][2]
2. Geben Sie zum Schätzen des Speicherplatzbedarfs die Gesamtanzahl von Elementen (z.B. Dokumente, Zeilen oder Vertices) ein, die Sie voraussichtlich speichern werden.
3. Geben Sie die pro Sekunde benötige Anzahl von Erstellungs-, Lese-, Aktualisierungs- und Löschvorgängen an. Laden Sie zum Ermitteln der bei Elementaktualisierungen voraussichtlich anfallenden Gebühren für Anforderungseinheiten eine Kopie des Beispielelements aus Schritt 1 mit typischen Feldaktualisierungen hoch.  Wenn bei Elementaktualisierungen also üblicherweise die beiden Eigenschaften *lastLogin* und *userVisits* geändert werden, kopieren Sie ein Beispielelement, aktualisieren Sie die Werte für diese beiden Eigenschaften, und laden Sie das kopierte Element anschließend hoch.
   
    ![Eingeben der Durchsatzanforderungen in den Rechner für Anforderungseinheiten][3]
4. Klicken Sie auf „Berechnen“, und prüfen Sie die Ergebnisse.
   
    ![Ergebnisse des Rechners für Anforderungseinheiten][4]

> [!NOTE]
> Wenn sich die Elementtypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, laden Sie ein Beispiel für jeden *Typ* eines normalen Elements an das Tools hoch, und berechnen Sie dann die Ergebnisse.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Verwenden des Azure Cosmos DB-Antwortheaders „request-charge“
Jede Antwort des Azure Cosmos DB-Diensts enthält einen benutzerdefinierten Header (`x-ms-request-charge`), der die für eine bestimmte Anforderung verbrauchten Anforderungseinheiten enthält. Auf diesen Header kann auch über die Azure Cosmos DB-SDKs zugegriffen werden. Im .NET-SDK ist `RequestCharge` eine Eigenschaft des `ResourceResponse`-Objekts.  Für Abfragen stellt der Azure Cosmos DB-Daten-Explorer im Azure-Portal Informationen zu Anforderungsgebühren für ausgeführten Abfragen bereit.

Vor diesem Hintergrund besteht eine Methode zum Abschätzen des von der Anwendung benötigten Durchsatzes darin, typische Vorgänge mit einem repräsentativen, von Ihrer Anwendung verwendeten Element auszuführen, sich dabei die berechneten Anforderungseinheiten zu notieren und anschließend die Anzahl von Vorgängen zu schätzen, die erwartungsgemäß pro Sekunde ausgeführt werden.  Stellen Sie sicher, dass auch typische Abfragen und die Nutzung von Azure Cosmos DB-Skripts gemessen und berücksichtigt werden.

> [!NOTE]
> Wenn sich die Elementtypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, erfassen Sie für jeden typischen *Elementtyp* jeweils die berechneten Anforderungseinheiten des jeweiligen Vorgangs.
> 
> 

Beispiel: 

1. Notieren Sie die berechneten Anforderungseinheiten für das Erstellen (Einfügen) eines typischen Elements. 
2. Notieren Sie die berechneten Anforderungseinheiten für das Lesen eines typischen Elements.
3. Notieren Sie die berechneten Anforderungseinheiten für das Aktualisieren eines typischen Elements.
4. Notieren Sie die berechneten Anforderungseinheiten für typische, häufig ausgeführte Elementabfragen.
5. Notieren Sie die berechneten Anforderungseinheiten für benutzerdefinierte Skripts (gespeicherte Prozeduren, Trigger, benutzerdefinierte Funktionen), die von der Anwendung genutzt werden.
6. Berechnen Sie die erforderlichen Anforderungseinheiten anhand der geschätzten Anzahl von Vorgängen, die erwartungsgemäß pro Sekunde ausgeführt werden.

## <a name="a-request-unit-estimate-example"></a>Beispiel für die Schätzung von Anforderungseinheiten
Betrachten Sie das folgende Dokument von etwa 1 KB:

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> Dokumente werden in Azure Cosmos DB minimiert, daher beträgt die vom System berechnete Größe des obigen Dokuments etwas weniger als 1 KB.
> 
> 

Die folgende Tabelle zeigt die ungefähre Anzahl berechneter Anforderungseinheiten für normale Vorgänge für dieses Element (bei der ungefähren Anzahl berechneter Anforderungseinheiten wird angenommen, dass die Kontokonsistenzebene auf *Session* festgelegt ist und dass alle Elemente automatisch indiziert werden):

| Vorgang | Berechnete Anforderungseinheiten |
| --- | --- |
| Create item (Element erstellen) |~15 RUs |
| Read item (Element lesen) |~1 RU |
| Element abfragen nach ID |~2,5 RUs |

Diese Tabelle zeigt darüber hinaus die ungefähre Anzahl berechneter Anforderungseinheiten für normale Abfragen, die in der Anwendung verwendet werden:

| Abfragen | Berechnete Anforderungseinheiten | Anzahl zurückgegebener Elemente |
| --- | --- | --- |
| Nahrungsmittel nach ID auswählen |~2,5 RUs |1 |
| Nahrungsmittel nach Hersteller auswählen |~7 RUs |7 |
| Nach Nahrungsmittelgruppe auswählen und nach Gewicht bestellen |~70 RUs |100 |
| Die 10 beliebtesten Nahrungsmittel in einer Nahrungsmittelgruppe auswählen |~10 RUs |10 |

> [!NOTE]
> RU-Gebühren richten sich nach der Anzahl zurückgegebener Elemente.
> 
> 

Mit diesen Informationen können Sie den RU-Bedarf für diese Anwendung angesichts der Anzahl erwarteter Vorgänge und Abfragen pro Sekunde abschätzen:

| Vorgang/Abfrage | Geschätzte Anzahl pro Sekunde | Erforderliche RUs |
| --- | --- | --- |
| Create item (Element erstellen) |10 |150 |
| Read item (Element lesen) |100 |100 |
| Nahrungsmittel nach Hersteller auswählen |25 |175 |
| Nach Nahrungsmittelgruppe auswählen |10 |700 |
| 10 beliebteste auswählen |15 |150 insgesamt |

In diesem Fall erwarten wir einen durchschnittlichen Durchsatzbedarf von 1.275 RU/s.  Wir runden auf den nächsten Hunderterwert auf und würden für den Container (oder die Gruppe von Containern) dieser Anwendung 1.300 RU/s bereitstellen.

## <a id="RequestRateTooLarge"></a> Überschreiten von Grenzwerten für den reservierten Durchsatz in Azure Cosmos DB
Der Verbrauch von Anforderungseinheiten wird als Rate pro Sekunde bemessen. Für Anwendungen, die die bereitgestellte Anforderungseinheitenrate überschreiten, werden begrenzt, bis die Rate wieder unter das bereitgestellte Durchsatzniveau fällt. Bei eine Begrenzung für eine Anforderung auftritt, beendet der Server die Anforderung präemptiv mit `RequestRateTooLargeException` (HTTP-Statuscode 429) und gibt den Header `x-ms-retry-after-ms` zurück. Darin ist die Zeitspanne in Millisekunden angegeben, die der Benutzer abwarten muss, bevor ein Wiederholungsversuch für die Anforderung unternommen werden kann.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Wenn Sie das .NET Client SDK und LINQ-Abfragen verwenden, werden Sie sich normalerweise nicht mit dieser Ausnahme beschäftigen müssen, da die aktuelle Version des .NET Client SDK diese Antwort implizit abfängt, den vom Server angegebenen retry-after-Header beachtet und die Anforderung automatisch wiederholt. Wenn nicht mehrere Clients gleichzeitig auf Ihr Konto zugreifen, wird die nächste Wiederholung erfolgreich ausgeführt.

Wenn mehrere Clients kumulativ oberhalb der Anforderungsrate arbeiten, reicht das Standard-Wiederholungsverhalten möglicherweise nicht aus, und der Client löst für die Anwendung eine `DocumentClientException` mit dem Statuscode 429 aus. In diesen Fällen sollten Sie in Betracht ziehen, das Wiederholungsverhalten und die zugehörige Logik in die Fehlerbehandlungsroutinen der Anwendung aufzunehmen oder den für den Container (oder die Gruppe von Containern) bereitgestellten Durchsatz zu erhöhen.

## <a name="next-steps"></a>Nächste Schritte
 
Informationen zum Festlegen und Abrufen des Durchsatzes mit dem Azure-Portal und SDKs finden Sie im folgenden Artikel:

* [Festlegen und Abrufen des Durchsatzes für Azure Cosmos DB-Container](set-throughput.md)

Weitere Informationen zum reservierten Durchsatz mit Azure Cosmos DB-Datenbanken finden Sie in folgenden Ressourcen:

* [Azure Cosmos DB-Preise](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [Partitionierung von Daten in Azure Cosmos DB](partition-data.md)

Weitere Informationen zu Azure Cosmos DB finden Sie in der [Dokumentation](https://azure.microsoft.com/documentation/services/cosmos-db/) zu Azure Cosmos DB. 

Im Artikel [Leistungs- und Skalierungstests mit Azure Cosmos DB](performance-testing.md) finden Sie eine Einführung in Leistungs- und Skalierungstests mit Azure Cosmos DB.

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png

