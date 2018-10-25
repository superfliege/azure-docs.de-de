---
title: Anforderungseinheiten und Schätzen des Durchsatzes – Azure Cosmos DB | Microsoft-Dokumentation
description: Enthält Informationen zu den Grundlagen von Anforderungseinheiten in Azure Cosmos DB sowie zur Angabe und zur Schätzung des Bedarfs an Anforderungseinheiten.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: rimman
ms.openlocfilehash: 23a3e629e12e2a4d417757c9fef5db804bb72c9e
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248753"
---
# <a name="throughput-and-request-units-in-azure-cosmos-db"></a>Durchsatz und Anforderungseinheiten in Azure Cosmos DB

Azure Cosmos DB-Ressourcen werden basierend auf dem bereitgestellten Durchsatz und Speicherplatz abgerechnet. In Azure Cosmos DB wird der Durchsatz in **Anforderungseinheiten pro Sekunde (RU/s)** angegeben. Azure Cosmos DB unterstützt verschiedene APIs mit unterschiedlichen Vorgängen, die von einfachen Lese- und Schreibvorgängen bis hin zu komplexen Graphabfragen reichen. Jede Anforderung verbraucht Anforderungseinheiten gemäß der Menge des für die Anforderung erforderlichen Rechenaufwands. Die Anzahl von Anforderungseinheiten für einen Vorgang ist deterministisch. Sie können die Anzahl von Anforderungseinheiten, die von einem beliebigen Vorgang in Azure Cosmos DB verbraucht werden, über den Antwortheader nachverfolgen. Sie müssen einen Durchsatz von 100 RU/s reservieren, um eine vorhersagbare Leistung bereitstellen zu können. Sie können Ihre Durchsatzanforderungen mit dem [Anforderungseinheitenrechner](https://www.documentdb.com/capacityplanner) von Azure Cosmos DB schätzen.

In Azure Cosmos DB können Sie Durchsatz in zwei Größen bereitstellen: 

1. **Azure Cosmos DB-Container:** Der für einen Container bereitgestellte Durchsatz ist ausschließlich für diesen Container reserviert. Beim Zuweisen von Durchsatz (RU/s) für einzelne Container können diese **mit fester Größe** oder **unbegrenzter Größe** erstellt werden. 

  Container mit fester Größe haben einen maximalen Durchsatz von 10.000 RU/s und ein Speicherlimit von 10 GB. Um einen unbegrenzten Container zu erstellen, müssen Sie einen Mindestdurchsatz von 1.000 RU/s und einen [Partitionsschlüssel](partition-data.md) angeben. Da Ihre Daten möglicherweise auf mehrere Partitionen aufgeteilt werden, ist es notwendig, einen Partitionsschlüssel mit hoher Kardinalität (Hunderte bis Millionen unterschiedlicher Werte) auszuwählen. Durch Auswahl eines Partitionsschlüssels mit vielen unterschiedlichen Werten kann Azure Cosmos DB sicherstellen, dass Anforderungen für eine Sammlung, eine Tabelle oder einen Graph einheitlich skaliert werden. 

2. **Azure Cosmos DB-Datenbank:** Der für eine Datenbank bereitgestellte Durchsatz wird von allen Containern in dieser Datenbank gemeinsam genutzt. Bei der Bereitstellung von Durchsatz auf Datenbankebene können Sie bestimmte Container ausschließen und stattdessen auf Containerebene Durchsätze für diese Container bereitstellen. Für den Durchsatz auf Datenbankebene müssen alle Sammlungen mit einem Partitionsschlüssel erstellt werden. Beim Zuweisen von Durchsatz auf Datenbankebene sollten die Container, die zu dieser Datenbank gehören, mit einem Partitionsschlüssel erstellt werden, da jede Sammlung ein **unbegrenzter** Container ist.  

Basierend auf dem bereitgestellten Durchsatz ordnet Azure Cosmos DB physische Partitionen zum Hosten Ihrer Container zu, und Daten werden gemäß ihrem Wachstum zwischen Partitionen aufgeteilt. Die folgende Abbildung zeigt die Bereitstellung des Durchsatzes auf verschiedenen Ebenen:

  ![Bereitstellen von Anforderungseinheiten für einzelne Container und Gruppen von Containern](./media/request-units/provisioning_set_containers.png)

> [!NOTE] 
> Die Bereitstellung von Durchsatz auf Container- und Datenbankebene wird gesondert angeboten. Für einen Wechsel zwischen diesen Bereitstellungsmethoden müssen Daten von der Quelle zum Ziel migriert werden. Dies bedeutet, dass Sie eine neue Datenbank oder eine neue Sammlung erstellen müssen und anschließend Daten mithilfe der [Bulk-Executor-Bibliothek](bulk-executor-overview.md) oder mithilfe von [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) migrieren müssen.

## <a name="request-units-and-request-charges"></a>Anforderungseinheiten und Anforderungsgebühren

Azure Cosmos DB bietet eine schnelle, vorhersagbare Leistung durch die Reservierung von Ressourcen, um die Durchsatzanforderungen Ihrer Anwendung zu erfüllen. Anwendungsauslastung und Zugriffsmuster ändern sich im Lauf der Zeit. Azure Cosmos DB kann Sie dabei unterstützen, den reservierten Durchsatz, der für Ihre Anwendung zur Verfügung steht, ganz einfach zu erhöhen oder zu verringern.

Bei Azure Cosmos DB wird der reservierte Durchsatz in verarbeiteten Anforderungseinheiten pro Sekunde angegeben. Sie können sich Anforderungseinheiten als Währung für den Durchsatz vorstellen. Sie reservieren eine bestimmte Anzahl von Anforderungseinheiten, die Ihrer Anwendung garantiert pro Sekunde zur Verfügung stehen. Jeder Vorgang in Azure Cosmos DB – beispielsweise das Schreiben eines Dokuments, das Durchführen einer Abfrage oder das Aktualisieren eines Dokuments – verbraucht CPU, Arbeitsspeicher und IOPS. Mit anderen Worten: Für jeden Vorgang fällt eine Anforderungsgebühr an, die in Anforderungseinheiten ausgedrückt wird. Wenn Sie die Faktoren, die sich auf die Gebühren für Anforderungseinheiten auswirken, sowie die Durchsatzanforderungen Ihrer Anwendung genau kennen, können Sie Ihre Anwendung so kosteneffektiv wie möglich ausführen. 

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Durchsatzisolation in global verteilten Datenbanken

Wenn Sie Ihre Datenbank in mehr als einer Region replizieren, sorgt Azure Cosmos DB für Durchsatzisolation, um sicherzustellen, dass die Nutzung von Anforderungseinheiten in einer Region die Nutzung von Anforderungseinheiten in einer anderen Region nicht beeinträchtigt. Wenn Sie beispielsweise Daten in eine Region schreiben und Daten aus einer anderen Region lesen, werden für den Schreibvorgang in Region A keine Anforderungseinheiten für den Lesevorgang in Region B abgezogen. Anforderungseinheiten werden nicht auf die Regionen aufgeteilt, in denen Sie Ihre Datenbank bereitgestellt haben. In jeder Region, in der die Datenbank repliziert wird, wird die volle Anzahl von Anforderungseinheiten bereitgestellt. Weitere Informationen über die globale Replikation finden Sie unter [Wie werden Daten mit Azure Cosmos DB global verteilt?](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>Aspekte zu Anforderungseinheiten
Bei der Schätzung der Anzahl von Anforderungseinheiten, die Sie bereitstellen möchten, sollten Sie unbedingt die folgenden Variablen berücksichtigen:

* **Elementgröße**. Mit zunehmender Größe werden auch mehr Anforderungseinheiten für das Lesen und Schreiben der Daten verbraucht.
* **Anzahl der Elementeigenschaften**. Eine Standardindizierung aller Eigenschaften vorausgesetzt, werden mehr Einheiten für das Schreiben eines Dokuments, eines Knotens oder einer Entität verbraucht, wenn die Anzahl der Eigenschaften steigt.
* **Datenkonsistenz**. Bei Verwendung von Datenkonsistenzmodellen wie „Stark“ oder „Begrenzte Veraltung“ werden zum Lesen von Elementen zusätzliche Anforderungseinheiten verbraucht.
* **Indizierte Eigenschaften**. Eine Indexrichtlinie für jeden Container gibt an, welche Eigenschaften standardmäßig indiziert werden. Sie können die Nutzung von Anforderungseinheiten für Schreibvorgänge reduzieren, indem Sie die Anzahl der indizierten Eigenschaften begrenzen oder die verzögerte Indizierung aktivieren.
* **Dokumentindizierung**. Standardmäßig wird jedes Element automatisch indiziert. Sie verbrauchen weniger Anforderungseinheiten, wenn Sie einige Elemente nicht indizieren.
* **Abfragemuster**. Die Komplexität einer Abfrage wirkt sich darauf aus, wie viele Anforderungseinheiten für einen Vorgang verbraucht werden. Die Anzahl von Abfrageergebnissen und Prädikaten, die Art der Prädikate, die Anzahl von benutzerdefinierten Funktionen, die Größe der Quelldaten und die Projektionen – all diese Faktoren beeinflussen die Kosten von Abfragevorgängen.
* **Skriptnutzung**. Wie bei Abfragen beanspruchen gespeicherte Prozeduren und Trigger Anforderungseinheiten basierend auf der Komplexität des ausgeführten Vorgangs. Sehen Sie sich beim Entwickeln Ihrer Anwendung den request-charge-Header an, um zu ermitteln, wie viele Anforderungseinheiten durch die einzelnen Vorgänge verbraucht werden.

## <a name="estimating-throughput-needs"></a>Schätzen der Durchsatzanforderungen
Eine Anforderungseinheit ist eine normalisierte Kennzahl für die Anforderungsverarbeitungskosten. Eine einzelne Anforderungseinheit stellt die Verarbeitungskapazität dar, die erforderlich ist, um ein einzelnes, aus 10 eindeutigen Eigenschaftswerten (außer Systemeigenschaften) bestehendes Element von 1 KB zu lesen (per „self link“ oder ID). Eine Anforderung zum Erstellen (Einfügen), Ersetzen oder Löschen des gleichen Elements verbraucht mehr Verarbeitungsleistung des Diensts und erfordert daher mehr Anforderungseinheiten. 

> [!NOTE]
> Die Baseline einer Anforderungseinheit für ein Element von 1 KB entspricht einem einfachen GET-Vorgang per „self link“ oder ID des Elements.
> 
> 

In der Tabelle unten ist beispielsweise angegeben, wie viele Anforderungseinheiten bei Elementen drei unterschiedlichen Größen (1 KB, 4 KB und 64 KB) und bei zwei unterschiedlichen Leistungsebenen (500 Lesevorgänge/Sekunde + 100 Schreibvorgänge/Sekunde und 500 Lesevorgänge/Sekunde + 500 Schreibvorgänge/Sekunde) bereitgestellt werden sollten. In diesem Beispiel wurde die Datenkonsistenz auf **Sitzung** und die Indizierungsrichtlinie auf **Keine** festgelegt.

| Elementgröße | Lesevorgänge/Sekunde | Schreibvorgänge/Sekunde | Anforderungseinheiten
| --- | --- | --- | --- |
| 1 KB | 500 | 100 | (500 * 1) + (100 * 5) = 1.000 RU/s
| 1 KB | 500 | 500 | (500 * 1) + (500 * 5) = 3.000 RU/s
| 4 KB | 500 | 100 | (500 * 1,3) + (100 * 7) = 1.350 RU/s
| 4 KB | 500 | 500 | (500 * 1,3) + (500 * 7) = 4.150 RU/s
| 64 KB | 500 | 100 | (500 * 10) + (100 * 48) = 9.800 RU/s
| 64 KB | 500 | 500 | (500 * 10) + (500 * 48) = 29.000 RU/s

### <a name="use-the-request-unit-calculator"></a>Verwenden des Rechners für Anforderungseinheiten
Um Ihre Schätzungen für den Durchsatz zu optimieren, können Sie den webbasierten [Rechner für Anforderungseinheiten](https://www.documentdb.com/capacityplanner) verwenden. Der Rechner unterstützt Sie dabei, die erforderlichen Anforderungseinheiten für typische Vorgänge wie z.B. die folgenden einzuschätzen:

* Erstellen von Elementen (Schreiben)
* Lesen von Elementen
* Löschen von Elementen
* Aktualisieren von Elementen

Das Tool unterstützt auch die Schätzung des Datenspeicherbedarfs auf der Grundlage der von Ihnen bereitgestellten Beispielelemente.

So verwenden Sie das Tool:

1. Laden Sie mindestens ein repräsentatives Element (z.B. ein JSON-Beispieldokument) hoch.
   
    ![Hochladen von Elementen in den Rechner für Anforderungseinheiten][2]
2. Geben Sie zum Schätzen des Speicherplatzbedarfs die Gesamtanzahl von Elementen (z.B. Dokumente, Zeilen oder Scheitelpunkte) ein, die Sie voraussichtlich speichern werden.
3. Geben Sie die pro Sekunde benötige Anzahl von Erstellungs-, Lese-, Aktualisierungs- und Löschvorgängen an. Laden Sie zum Ermitteln der bei Elementaktualisierungsvorgängen voraussichtlich anfallenden Gebühren für Anforderungseinheiten eine Kopie des Beispielelements aus Schritt 1 mit typischen Feldaktualisierungen hoch. Wenn bei Elementaktualisierungen z.B. üblicherweise die beiden Eigenschaften *lastLogin* und *userVisits* geändert werden, kopieren Sie ein Beispielelement, aktualisieren Sie die Werte für diese beiden Eigenschaften, und laden Sie das kopierte Element anschließend hoch.
   
    ![Eingeben der Durchsatzanforderungen in den Rechner für Anforderungseinheiten][3]
4. Klicken Sie auf **Berechnen**, und prüfen Sie die Ergebnisse.
   
    ![Ergebnisse des Rechners für Anforderungseinheiten][4]

> [!NOTE]
> Wenn Sie über Elementtypen verfügen, die sich im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, laden Sie ein Beispiel für jeden *Typ* eines typischen Elements in das Tool hoch, und berechnen Sie dann die Ergebnisse.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Verwenden des Azure Cosmos DB-Antwortheaders „request-charge“
Jede Antwort des Azure Cosmos DB-Diensts enthält einen benutzerdefinierten Header (`x-ms-request-charge`), der die für eine bestimmte Anforderung verbrauchten Anforderungseinheiten enthält. Sie können auch über die Azure Cosmos DB SDKs auf diesen Header zugreifen. Im .NET SDK ist **RequestCharge** eine Eigenschaft des **ResourceResponse**-Objekts. Für Abfragen stellt der Azure Cosmos DB-Daten-Explorer im Azure-Portal Informationen zu Anforderungsgebühren für ausgeführten Abfragen bereit. Weitere Informationen zum Abrufen und Festlegen des Durchsatzes mit unterschiedlichen APIs mit mehreren Modellen finden Sie im Artikel [Festlegen und Abrufen des Durchsatzes in Azure Cosmos DB](set-throughput.md).

Eine Methode zum Einschätzen des von Ihrer Anwendung benötigten reservierten Durchsatzes besteht darin, die Gebühren für Anforderungseinheiten für die Ausführung typischer Vorgänge aufzuzeichnen und sie mit einem repräsentativen Element zu vergleichen, das von Ihrer Anwendung verwendet wird. Danach schätzen Sie die Anzahl von Vorgängen, deren Ausführung Sie pro Sekunde erwarten. Stellen Sie sicher, dass auch typische Abfragen und die Nutzung von Azure Cosmos DB-Skripts gemessen und berücksichtigt werden.

> [!NOTE]
> Wenn Sie über Elementtypen verfügen, die sich im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, notieren Sie sich für jeden *Typ* eines typischen Elements die gültige Gebühr für die berechneten Anforderungseinheiten des jeweiligen Vorgangs.
> 
> 

Sie können beispielsweise die folgenden Schritte ausführen:

1. Notieren Sie die berechneten Anforderungseinheiten für das Erstellen (Einfügen) eines typischen Elements. 
2. Notieren Sie die berechneten Anforderungseinheiten für das Lesen eines typischen Elements.
3. Notieren Sie die berechneten Anforderungseinheiten für das Aktualisieren eines typischen Elements.
4. Notieren Sie die berechneten Anforderungseinheiten für typische, häufig ausgeführte Elementabfragen.
5. Notieren Sie sich die Gebühren für die Anforderungseinheiten für benutzerdefinierte Skripts (gespeicherte Prozeduren, Trigger, benutzerdefinierte Funktionen), die von der Anwendung genutzt werden.
6. Berechnen Sie die erforderlichen Anforderungseinheiten anhand der geschätzten Anzahl von Vorgängen, die erwartungsgemäß pro Sekunde ausgeführt werden.

## <a name="a-request-unit-estimate-example"></a>Beispiel für die Schätzung von Anforderungseinheiten
Sehen Sie sich das folgende Dokument an, das etwa 1 KB groß ist:

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

Die folgende Tabelle zeigt die ungefähren Gebühren für Anforderungseinheiten für typische Vorgänge für dieses Element. (Bei den geschätzten Gebühren für Anforderungseinheiten wird angenommen, dass die Konsistenzebene für das Konto auf **Sitzung** festgelegt wurde und alle Elemente automatisch indiziert werden.)

| Vorgang | Gebühr für Anforderungseinheiten |
| --- | --- |
| Create item (Element erstellen) |~15 RUs |
| Read item (Element lesen) |~1 RU |
| Element nach ID abfragen |~2,5 RUs |

Die folgende Tabelle zeigt die ungefähren Gebühren für Anforderungseinheiten für typische Abfragen, die in der Anwendung verwendet werden:

| Abfragen | Gebühr für Anforderungseinheiten | Anzahl zurückgegebener Elemente |
| --- | --- | --- |
| Nahrungsmittel nach ID auswählen |~2,5 RUs |1 |
| Nahrungsmittel nach Hersteller auswählen |~7 RUs |7 |
| Nach Nahrungsmittelgruppe auswählen und nach Gewicht bestellen |~70 RUs |100 |
| Die 10 beliebtesten Nahrungsmittel in einer Nahrungsmittelgruppe auswählen |~10 RUs |10 |

> [!NOTE]
> Die Gebühren für Anforderungseinheiten variieren je nach Anzahl der zurückgegebenen Elemente.
> 
> 

Mit diesen Informationen können Sie den Bedarf an Anforderungseinheiten für diese Anwendung anhand der Anzahl der erwarteten Vorgänge und Abfragen pro Sekunde schätzen:

| Vorgang/Abfrage | Geschätzte Anzahl pro Sekunde | Erforderliche Anforderungseinheiten |
| --- | --- | --- |
| Create item (Element erstellen) |10 |150 |
| Read item (Element lesen) |100 |100 |
| Nahrungsmittel nach Hersteller auswählen |25 |175 |
| Nach Nahrungsmittelgruppe auswählen |10 |700 |
| 10 beliebteste auswählen |15 |150 insgesamt |

In diesem Fall erwarten Sie einen durchschnittlichen Durchsatzbedarf von 1.275 RU/s. Aufgerundet auf den nächsten Hunderterwert würden Sie für den Container (oder die Gruppe von Containern) dieser Anwendung 1.300 RU/s bereitstellen.

## <a id="RequestRateTooLarge"></a> Überschreiten von Grenzwerten für den reservierten Durchsatz in Azure Cosmos DB
Der Verbrauch von Anforderungseinheiten wird als Rate pro Sekunde bemessen. Bei Anwendungen, die die bereitgestellte Rate an Anforderungseinheiten überschreiten, wird die Rate für Anforderungen begrenzt, bis die Rate wieder unter das bereitgestellte Durchsatzniveau fällt. Wenn die Rate für eine Anforderung begrenzt wurde, beendet der Server die Anforderung präventiv mit `RequestRateTooLargeException` (HTTP-Statuscode 429) und gibt den `x-ms-retry-after-ms`-Header zurück. Der Header gibt den Zeitraum in Millisekunden an, den ein Benutzer abwarten muss, bevor er erneut versucht, die Anforderung zu senden.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Wenn Sie das .NET Client SDK und LINQ-Abfragen verwenden, müssen Sie sich mit dieser Ausnahme normalerweise nie beschäftigen. Die aktuelle Version des .NET Client SDK fängt diese Antwort implizit ab, berücksichtigt den vom Server angegebenen retry-after-Header und versucht automatisch, die Anforderung erneut zu senden. Wenn nicht mehrere Clients gleichzeitig auf Ihr Konto zugreifen, wird die nächste Wiederholung erfolgreich ausgeführt.

Wenn mehrere Clients kumulativ oberhalb der Anforderungsrate arbeiten, reicht das standardmäßige Wiederholungsverhalten möglicherweise nicht aus, und der Client löst für die Anwendung eine `DocumentClientException` mit dem Statuscode 429 aus. In diesen Fällen sollten Sie in Betracht ziehen, das Wiederholungsverhalten und die zugehörige Logik in den Fehlerbehandlungsroutinen Ihrer Anwendung zu verarbeiten oder den für den Container (oder die Gruppe von Containern) bereitgestellten Durchsatz zu erhöhen.

## <a name="next-steps"></a>Nächste Schritte
 
- Erfahren Sie, wie Sie das Azure-Portal und die SDKs verwenden, um den [Durchsatz für Azure Cosmos DB festzulegen und abzurufen](set-throughput.md).
- Weitere Informationen finden Sie unter [Leistungs- und Skalierungstests mit Azure Cosmos DB](performance-testing.md).
- Weitere Informationen zum reservierten Durchsatz bei Azure Cosmos DB-Datenbanken finden Sie unter [Azure Cosmos DB – Preise](https://azure.microsoft.com/pricing/details/cosmos-db/) und [Partitionieren und Skalieren in Azure Cosmos DB](partition-data.md).
- Weitere Informationen zu Azure Cosmos DB finden Sie in der [Dokumentation für Azure Cosmos DB](https://azure.microsoft.com/documentation/services/cosmos-db/). 

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png


