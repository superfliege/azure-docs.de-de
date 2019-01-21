---
title: Anfügen einer Cognitive Services-Ressource an eine Qualifikationsgruppe – Azure Search
description: Anweisungen zum Hinzufügen eines Cognitive Services All-in-One-Abonnements zu einer Pipeline für kognitive Anreicherung in Azure Search.
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 5bffeacaa07f90a11c374061eb6c0d36fc8f86a9
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2019
ms.locfileid: "54351457"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Anfügen einer Cognitive Services-Ressource an eine Qualifikationsgruppe in Azure Search 

Die KI-Algorithmen, mit denen [Pipelines für die kognitive Suche](cognitive-search-concept-intro.md) zur Verarbeitung unstrukturierter Daten ausgeführt werden, basieren auf [**Cognitive Services-Ressourcen**](https://azure.microsoft.com/services/cognitive-services/). Ressourcen wie [**Maschinelles Sehen**](https://azure.microsoft.com/services/cognitive-services/computer-vision/) bieten Bildanalyse und optische Zeichenerkennung (Optical Character Recognition, OCR) für das Extrahieren von Text und Struktur aus Bilddateien, während die [**Textanalyse**](https://azure.microsoft.com/services/cognitive-services/text-analytics/) eine Verarbeitung natürlicher Sprache wie die Entitäts- und Schlüsselbegriffserkennung bietet, um nur einige zu nennen.

Sie können eine begrenzte Anzahl von Dokumenten kostenlos anreichern oder eine abrechenbare Cognitive Services-Ressource für größere und häufigere Workloads anfügen. In diesem Artikel erfahren Sie, wie Sie Ihrer kognitiven Qualifikationsgruppe eine Cognitive Services-Ressource zuordnen, um Daten während der [Azure Search-Indizierung](search-what-is-an-index.md) anzureichern.

Wenn Ihre Pipeline ausschließlich aus [benutzerdefinierten Qualifikationen](cognitive-search-create-custom-skill-example.md) besteht, müssen Sie keine Cognitive Services-Ressource anfügen.

> [!NOTE]
> Seit dem 21. Dezember 2018 können Sie einer Azure Search-Qualifikationsgruppe eine Cognitive Services-Ressource zuordnen. Dadurch können wir für die Ausführung von Qualifikationsgruppen Gebühren verlangen. Ab diesem Datum haben wir außerdem damit begonnen, die Bildextraktion als Teil der Aufschlüsselung von Dokumenten zu berechnen. Die Textextraktion aus Dokumenten wird weiterhin ohne Zusatzkosten angeboten.
>
> Die Ausführung [integrierter kognitiver Qualifikationen](cognitive-search-predefined-skills.md) wird nach dem [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services) berechnet, und zwar zum selben Tarif wie beim direkten Ausführen der Aufgabe. Die Extraktion von Bildern ist ein abrechenbares Azure Search-Ereignis, das derzeit zu Vorschaupreisen angeboten wird. Ausführlichere Informationen finden Sie auf der [Seite „Azure Search – Preise“](https://go.microsoft.com/fwlink/?linkid=2042400) oder unter [Funktionsweise der Abrechnung](search-sku-tier.md#how-billing-works).


## <a name="use-free-resources"></a>Verwenden kostenloser Ressourcen

Sie können eine eingeschränkte, kostenlose Verarbeitungsoption verwenden, um das Cognitive Search-Tutorial und die Schnellstartübungen abzuschließen. 

> [!Important]
> Ab dem 1. Februar 2019 ist **Free (begrenzte Anreicherung)** auf 20 Dokumente pro Tag begrenzt. 

1. Öffnen Sie den Assistenten **Daten importieren**.

   ![Befehl „Daten importieren“](media/search-get-started-portal/import-data-cmd2.png "Befehl „Daten importieren“")

1. Wählen Sie eine Datenquelle aus, und fahren Sie mit **Kognitive Suche hinzufügen (Optional)** fort. Eine schrittweise Anleitung für diesen Assistenten finden Sie unter [Importieren, Indizieren und Abfragen mithilfe von Portal-Tools](search-get-started-portal.md).

1. Erweitern Sie **Cognitive Services-Instanz anfügen**, und wählen Sie **Free (begrenzte Anreicherung)** aus.

   ![Erweiterter Bereich „Cognitive Services-Instanz anfügen“](./media/cognitive-search-attach-cognitive-services/attach1.png "Cognitive Services-Instanz anfügen, erweitert")

Fahren Sie mit dem nächsten Schritt, **Anreicherungen hinzufügen**, fort. Eine Beschreibung der im Portal verfügbaren Qualifikationen finden Sie unter [Schritt 2: Kognitive Qualifikationen hinzufügen](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) in der Schnellstartanleitung zur kognitiven Suche.

## <a name="use-billable-resources"></a>Verwenden abrechenbarer Ressourcen

Für Workloads, die sich täglich auf mehr als 20 Dokumente belaufen, benötigen Sie eine abrechenbare Cognitive Services-Ressource.

1. Wählen Sie im Assistenten **Daten importieren** unter **Cognitive Services-Instanz anfügen** eine vorhandene Ressource aus, oder klicken Sie auf **Neue Cognitive Services-Ressource erstellen**.

1. Wenn Sie **Neue Cognitive Services-Ressource erstellen** ausgewählt haben, wird eine neue Registerkarte geöffnet, auf der Sie die Ressource erstellen können. Geben Sie der Ressource einen eindeutigen Namen.

1. Wählen Sie den gleichen Standort wie Azure Search aus. Derzeit wird die Indizierung kognitiver Qualifikationen in den folgenden Regionen unterstützt:

  * USA, Westen-Mitte
  * USA Süd Mitte
  * USA (Ost)
  * USA (Ost) 2
  * USA, Westen 2
  * Kanada, Mitte
  * Europa, Westen
  * UK, Süden
  * Nordeuropa
  * Brasilien Süd
  * Asien, Südosten
  * Indien, Mitte
  * Australien (Osten)

1. Wählen Sie den All-in-One-Tarif **S0** aus. Dieser Tarif bietet die Funktionen für Sehen und Sprache, die die vordefinierten Qualifikationen in der kognitiven Suche unterstützen.

    ![Erstellen einer neuen Cognitive Services-Ressource](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Erstellen einer neuen Cognitive Services-Ressource")

1. Klicken Sie auf **Erstellen**, um die neue Cognitive Services-Ressource bereitzustellen. 

1. Kehren Sie zur vorherigen Registerkarte mit dem Assistenten **Daten importieren** zurück. Klicken Sie auf **Aktualisieren**, um die Cognitive Services-Ressource anzuzeigen, und wählen Sie dann die Ressource aus.

   ![Ausgewählte Cognitive Services-Ressource](./media/cognitive-search-attach-cognitive-services/attach2.png "Ausgewählte Cognitive Services-Ressource")

1. Erweitern Sie den Abschnitt **Anreicherungen hinzufügen**, um die spezielle kognitive Qualifikation auszuwählen, die Sie für Ihre Daten ausführen möchten, und fahren Sie mit dem Rest des Flows fort. Eine Beschreibung der im Portal verfügbaren Qualifikationen finden Sie unter [Schritt 2: Kognitive Qualifikationen hinzufügen](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) in der Schnellstartanleitung zur kognitiven Suche.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Anfügen einer vorhandenen Qualifikationsgruppe an eine Cognitive Services-Ressource

Wenn Sie bereits über eine Qualifikationsgruppe verfügen, können Sie diese an eine neue oder andere Cognitive Services-Ressource anfügen.

1. Klicken Sie auf der Seite **Dienstübersicht** auf **Qualifikationsgruppen**.

   ![Registerkarte „Qualifikationsgruppen“](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Registerkarte „Qualifikationsgruppen“")

1. Klicken Sie auf den Namen der Qualifikationsgruppe, und wählen Sie dann eine vorhandene Ressource aus, oder erstellen Sie eine neue Ressource. Klicken Sie zum Bestätigen der Änderungen auf **OK**. 

   ![Ressourcenliste „Qualifikationsgruppe“](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "Ressourcenliste „Qualifikationsgruppe“")

Zur Erinnerung: **Free (begrenzte Anreicherung)** ist auf 20 Dokumente täglich beschränkt, und **Neue Cognitive Services-Ressource erstellen** wird zum Bereitstellen einer neuen abrechenbaren Ressource verwendet. Wenn Sie eine neue Ressource erstellen, wählen Sie **Aktualisieren** aus, um die Liste der Cognitive Services-Ressourcen zu aktualisieren, und wählen Sie dann die Ressource aus.

## <a name="attach-cognitive-services-programmatically"></a>Programmgesteuertes Anfügen von Cognitive Services

Wenn Sie die Qualifikationsgruppe programmgesteuert definieren, fügen Sie einen `cognitiveServices`-Abschnitt zur Qualifikationsgruppe hinzu. Nehmen Sie in den Abschnitt den Schlüssel der Cognitive Services-Ressource auf, die Sie der Qualifikationsgruppe zuordnen möchten. Denken Sie daran, dass sich die Ressource in der gleichen Region wie Azure Search befinden muss. Nehmen Sie außerdem `@odata.type` auf, und legen Sie ihn auf `#Microsoft.Azure.Search.CognitiveServicesByKey` fest. 

Das folgende Beispiel zeigt dieses Muster. Beachten Sie den Abschnitt „cognitiveServices“ am Ende der Definition.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Beispiel: Schätzen der Kosten

Zum Schätzen der Kosten für die Indizierung der kognitiven Suche gehen Sie von einem durchschnittlichen Dokument aus, damit einige Zahlen zur Verfügung stehen. Beispielsweise kann zu Schätzzwecken Folgendes angenommen werden:

+ 1.000 PDFs
+ Jeweils sechs Seiten
+ Ein Bild pro Seite (6.000 Bilder)
+ 3.000 Zeichen pro Seite

Als Beispiel dient eine Pipeline, die Dokumententschlüsselung jeder PDF-Datei mit Bild- und Textextraktion, optische Zeichenerkennung (OCR) von Bildern und Erkennung benannter Entitäten für Organisationen umfasst. 

In dieser Übung wird der höchste Preis pro Transaktion verwendet. Die tatsächlichen Kosten können aufgrund gestaffelter Preise niedriger sein. Weitere Informationen finden Sie unter [Cognitive Services-Preise](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Im Fall der Dokumententschlüsselung mit Text- und Bildinhalten ist die Textextraktion derzeit kostenlos. Bei 6.000 Bildern und einem angenommen Preis von 1 € für jeweils 1.000 extrahierte Bilder belaufen sich die Kosten auf 6,00 € für diesen Schritt.

2. Für die optische Zeichenerkennung (OCR) von 6.000 Bildern in Englisch verwendet die kognitive Qualifikation „OCR“ den besten Algorithmus (DescribeText). Angenommen, jeweils 1.000 zu analysierende Bilder kosten 2,50 €, dann würden für diesen Schritt 15,00 € anfallen.

3. Für die Extraktion von Entitäten würden insgesamt 3 Textdatensätze pro Seite entstehen. Jeder Datensatz umfasst 1.000 Zeichen. 3 Textdatensätze/Seite * 6.000 Seiten = 18.000 Textdatensätze. Angenommen, die Kosten betragen 2,00 €/1.000 Textdatensätze, dann würde dieser Schritt 36,00 € kosten.

Insgesamt würden Sie also etwa 57,00 € zahlen, um 1.000 PDF-Dokumente dieser Art mit der beschriebenen Qualifikationsgruppe zu erfassen. 

## <a name="next-steps"></a>Nächste Schritte
+ [Azure Search – Preise](https://azure.microsoft.com/pricing/details/search/)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Erstellen eines Skillsets (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zuordnen angereicherter Felder](cognitive-search-output-field-mapping.md)
