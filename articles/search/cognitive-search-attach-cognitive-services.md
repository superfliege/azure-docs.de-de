---
title: Anfügen einer Cognitive Services-Ressource an eine Qualifikationsgruppe – Azure Search
description: Anweisungen zum Hinzufügen eines Cognitive Services All-in-One-Abonnements zu einer Pipeline für kognitive Anreicherung in Azure Search.
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: bad64f439d45581f8f4b55ea1ac849db1e27cb76
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024585"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Anfügen einer Cognitive Services-Ressource an eine Qualifikationsgruppe in Azure Search 

KI-Algorithmen steuern die [Pipelines für die kognitive Indizierung](cognitive-search-concept-intro.md), die zum Verarbeiten unstrukturierter Daten in Azure Search verwendet werden. Diese Algorithmen basieren auf [Cognitive Services-Ressourcen](https://azure.microsoft.com/services/cognitive-services/), einschließlich [Maschinelles Sehen](https://azure.microsoft.com/services/cognitive-services/computer-vision/) für Bildanalyse und optische Zeichenerkennung (OCR) und [Textanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/) für Entitätserkennung, Schlüsselbegriffserkennung und andere Anreicherungen.

Sie können eine begrenzte Anzahl von Dokumenten kostenlos anreichern oder eine abrechenbare Cognitive Services-Ressource für größere und häufigere Workloads anfügen. In diesem Artikel erfahren Sie, wie Sie Ihrer kognitiven Qualifikationsgruppe eine Cognitive Services-Ressource zuordnen, um Daten während der [Azure Search-Indizierung](search-what-is-an-index.md) anzureichern.

Wenn Ihre Pipeline aus Qualifikationen besteht, die nicht mit Cognitive Services-APIs in Beziehung stehen, sollten Sie dennoch eine Cognitive Services-Ressource anfügen. Dadurch wird die **kostenlose** Ressource überschrieben, mit der die Anreicherungen pro Tag auf eine kleine Menge beschränkt sind. Für Qualifikationen, die nicht an Cognitive Services-APIs gebunden sind, fallen keine Gebühren an. Zu diesen Qualifikationen gehören z.B. [benutzerdefinierte Qualifikationen](cognitive-search-create-custom-skill-example.md), [Textzusammenführung](cognitive-search-skill-textmerger.md), [Textteilung](cognitive-search-skill-textsplit.md) und [Shaper](cognitive-search-skill-shaper.md).

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie eine kostenpflichtige Cognitive Services-Ressource verwenden. Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumentaufschlüsselungsphase in Azure Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion werden auf der [Preisseite von Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400) beschrieben.

## <a name="use-free-resources"></a>Verwenden kostenloser Ressourcen

Sie können eine eingeschränkte, kostenlose Verarbeitungsoption verwenden, um das Cognitive Search-Tutorial und die Schnellstartübungen abzuschließen. 

**Free (begrenzte Anreicherung)** ist auf 20 Dokumente pro Tag pro Abonnement begrenzt.

1. Öffnen Sie den Assistenten **Daten importieren**.

   ![Befehl „Daten importieren“](media/search-get-started-portal/import-data-cmd2.png "Befehl „Daten importieren“")

1. Wählen Sie eine Datenquelle aus, und fahren Sie mit **Kognitive Suche hinzufügen (Optional)** fort. Eine schrittweise Anleitung für diesen Assistenten finden Sie unter [Importieren, Indizieren und Abfragen mithilfe von Portal-Tools](search-get-started-portal.md).

1. Erweitern Sie **Cognitive Services-Instanz anfügen**, und wählen Sie **Free (begrenzte Anreicherung)** aus.

   ![Erweiterter Bereich „Cognitive Services-Instanz anfügen“](./media/cognitive-search-attach-cognitive-services/attach1.png "Cognitive Services-Instanz anfügen, erweitert")

Fahren Sie mit dem nächsten Schritt, **Anreicherungen hinzufügen**, fort. Eine Beschreibung der im Portal verfügbaren Qualifikationen finden Sie unter [Schritt 2: Kognitive Qualifikationen hinzufügen](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) in der Schnellstartanleitung zur kognitiven Suche.

## <a name="use-billable-resources"></a>Verwenden abrechenbarer Ressourcen

Für Workloads, die sich täglich auf mehr als 20 Anreicherungen belaufen, müssen Sie eine abrechenbare Cognitive Services-Ressource anfügen. 

Ihnen werden nur Qualifikationen in Rechnung gestellt, mit denen die Cognitive Services-APIs aufgerufen werden. Qualifikationen, die nicht auf APIs basieren, z.B. [benutzerdefinierte Qualifikationen](cognitive-search-create-custom-skill-example.md), [Textzusammenführung](cognitive-search-skill-textmerger.md), [Textteilung](cognitive-search-skill-textsplit.md) und [Shaper](cognitive-search-skill-shaper.md), werden nicht in Rechnung gestellt.

1. Öffnen Sie den **Daten importieren**-Assistenten, wählen Sie eine Datenquelle aus, und fahren Sie mit **Kognitive Suche hinzufügen (Optional)** fort. 

1. Erweitern Sie **Cognitive Services-Instanz anfügen**, und wählen Sie dann **Neue Cognitive Services-Ressource erstellen** aus. Eine neue Registerkarte wird geöffnet, sodass Sie die Ressource erstellen können. 

   ![Erstellen einer Cognitive Services-Ressource](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Erstellen einer Cognitive Services-Ressource")

1. Wählen Sie in „Speicherort“ die gleiche Region wie Azure Search aus, um Kosten für Regionen übergreifende ausgehende Bandbreite zu vermeiden.

1. Wählen Sie in „Tarif“ **S0** aus, um die gesamte Sammlung der Cognitive Services-Features einschließlich der Seh- und Sprachfeatures zu erhalten, die die von Azure Search verwendeten vordefinierten Fertigkeiten unterstützen. 

   Für den Tarif „S0“ finden Sie Sätze für bestimmte Arbeitsauslastungen auf der Seite [Cognitive Services – Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).
  
   + Stellen Sie sicher, dass unter **Wählen Sie ein Angebot aus:** die Option *Cognitive Services* ausgewählt ist.
   + Die Preise für die *Standardtextanalyse* unter den Sprachfeatures gelten für die KI-Indizierung. 
   + Die Preise für *Maschinelles Sehen S1* sind unter den Sehfeatures aufgeführt.

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
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
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

Als Beispiel dient eine Pipeline, die Dokumententschlüsselung jeder PDF-Datei mit Bild- und Textextraktion, optische Zeichenerkennung (OCR) von Bildern und die Erkennung von Entitäten für Organisationen umfasst. 

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
