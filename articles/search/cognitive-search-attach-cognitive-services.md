---
title: Hinzufügen von Cognitive Services zu Ihren Qualifikationen – Azure Search
description: Anweisungen zum Hinzufügen eines Cognitive Services All-in-One-Abonnements zu einer kognitiven Qualifikationsgruppe
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 7f604d1b94e51db11e6d6992b7f070d64ae869dd
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53317235"
---
# <a name="associate-cognitive-services-resource-with-a-skillset"></a>Zuordnen einer Cognitive Services-Ressource zu einer Qualifikationsgruppe 

> [!NOTE]
> Ab dem 21. Dezember 2018 können Sie Cognitive Services-Ressourcen einer Azure Search-Qualifikationsgruppe zuordnen. Dies ermöglicht uns, für die Ausführung von Qualifikationsgruppen mit der Gebührenberechnung zu beginnen. Außerdem beginnen wir an diesem Datum damit, die Bildextraktion als Teil der Aufschlüsselung von Dokumenten zu berechnen. Die Textextraktion aus Dokumenten wird weiterhin ohne Zusatzkosten angeboten.
>
> Die Ausführung interner Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion entsprechen den Vorschaupreisen. Sie werden auf der [Preisseite von Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400) beschrieben.


Die kognitive Suche extrahiert Daten und reichert diese an, um sie in Azure Search durchsuchbar zu machen. Extraktions- und Anreicherungsschritte werden als *kognitive Qualifikationen* bezeichnet. Die während der Indizierung von Inhalten aufgerufene Auswahl von Qualifikationen wird in einer *Qualifikationsgruppe* definiert. Ein Skillset kann [vordefinierte Skills](cognitive-search-predefined-skills.md) oder benutzerdefinierte Skills verwenden (Weitere Informationen finden Sie unter [Beispiel: Erstellen eines benutzerdefinierten Skills](cognitive-search-create-custom-skill-example.md)).

In diesem Artikel erfahren Sie, wie Sie die [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)-Ressource Ihrer kognitiven Qualifikationsgruppe zuordnen.

Durch die Cognitive Services-Ressource, die Sie auswählten, werden die integrierten kognitiven Qualifikationen gefördert. Diese Ressource wird auch zu Abrechnungszwecken verwendet. Alle Anreicherungen, die Sie mit den integrierten kognitiven Qualifikationen durchführen, werden für die von Ihnen ausgewählte Cognitive Services-Ressource abgerechnet. Sie werden mit der gleichen Gebühr abgerechnet, als ob Sie dieselbe Aufgabe mit einer Cognitive Services-Ressource ausgeführt hätten. Weitere Informationen finden Sie unter [Cognitive Services-Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).

## <a name="limits-when-no-cognitive-services-resource-is-selected"></a>Grenzwerte, wenn keine Cognitive Services-Ressource ausgewählt ist
Wenn Sie ab dem 1. Februar 2019 Ihrer Qualifikationsgruppe kein Cognitive Services Abonnement zuordnen, können Sie nur eine kleine Anzahl von Dokumenten kostenlos anreichern (20 Dokumente pro Tag). 

## <a name="associating-a-cognitive-services-resource-with-a-new-skillset"></a>Zuordnen einer Cognitive Services-Ressource zu einer neuen Qualifikationsgruppe

1. Nach Herstellen der Verbindung mit Ihrer Datenquelle navigieren Sie in der *Datenimport*-Umgebung zum optionalen Schritt *Kognitive Suche hinzufügen*. 

1. Erweitern Sie den Abschnitt *Cognitive Services-Instanz anfügen*. Dadurch werden all Ihre Cognitive Services-Ressourcen in den gleichen Regionen wie Ihr Suchdienst angezeigt. 
![Cognitive Services-Instanz anfügen, erweitert](./media/cognitive-search-attach-cognitive-services/attach1.png "Cognitive Services-Instanz anfügen, erweitert")

1. Wählen Sie eine vorhandene Cognitive Services-Ressource aus, oder *erstellen Sie eine neue Cognitive Services-Ressource*. Bei Auswahl der Ressource *Free (begrenzte Anreicherung)* können Sie nur eine kleine Anzahl von Dokumenten kostenlos anreichern (20 Dokumente pro Tag). Wenn Sie auf *Neue Cognitive Services-Ressource erstellen* geklickt haben, wird eine neue Registerkarte geöffnet, auf der Sie die Cognitive Services-Ressource erstellen können. 

1. Wenn Sie eine neue Ressource erstellt haben, klicken Sie auf *Aktualisieren*, um die Liste der Cognitive Services-Ressourcen zu aktualisieren und die Ressource auszuwählen. 
![Ausgewählte Cognitive Services-Ressource](./media/cognitive-search-attach-cognitive-services/attach2.png "Ausgewählte Cognitive Services-Ressource")

1. Sobald Sie dies getan haben, können Sie den Abschnitt *Anreicherungen hinzufügen* erweitern, um die spezielle kognitive Qualifikation auszuwählen, die Sie für Ihre Daten ausführen möchten, und mit dem Rest des Flows fortfahren.

## <a name="associating-a-cognitive-services-resource-with-an-existing-skillset"></a>Zuordnen einer Cognitive Services-Ressource zu einer vorhandenen Qualifikationsgruppe

1. Wählen Sie auf der Seite „Dienstübersicht“ die Registerkarte *Qualifikationsgruppen* aus. ![Registerkarte „Qualifikationsgruppen“](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Registerkarte „Qualifikationsgruppen“")

1. *Klicken* Sie auf die Qualifikationsgruppe, die Sie ändern möchten. Dadurch wird ein Blatt geöffnet, auf dem Sie eine Qualifikationsgruppe bearbeiten können.

1. Wählen Sie eine vorhandene Cognitive Services-Ressource aus, oder *erstellen Sie eine neue Cognitive Services-Ressource*. Bei Auswahl der Ressource *Free (begrenzte Anreicherung)* können Sie nur eine kleine Anzahl von Dokumenten kostenlos anreichern (20 Dokumente pro Tag). Wenn Sie auf *Neue Cognitive Services-Ressource erstellen* geklickt haben, wird eine neue Registerkarte geöffnet, auf der Sie die Cognitive Services-Ressource erstellen können. <n/> 
<img src="./media/cognitive-search-attach-cognitive-services/attach-existing2.png" width="350">

1. Wenn Sie eine neue Ressource erstellt haben, klicken Sie auf *Aktualisieren*, um die Liste der Cognitive Services-Ressourcen zu aktualisieren und die Ressource auszuwählen.
1. Klicken Sie zum Bestätigen der Änderungen auf *OK*.

## <a name="associating-a-cognitive-services-resource-programmatically"></a>Programmgesteuertes Zuordnen einer Cognitive Services-Ressource

Wenn Sie die Qualifikationsgruppe programmgesteuert definieren, fügen einen `cognitiveServices`-Abschnitt hinzu. Der Abschnitt sollte den Schlüssel der Cognitive Services-Ressource enthalten, die Sie der Qualifikationsgruppe zuordnen möchten, sowie @odata.type, das auf „#Microsoft.Azure.Search.CognitiveServicesByKey“ festgelegt sein sollte. Dieses Muster wird im unten gezeigten Beispiel dargestellt.

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
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey"
        "description": "mycogsvcs",
        "key": "your key goes here"
    }
}
```
## <a name="example-estimating-the-cost-of-document-cracking-and-enrichment"></a>Beispiel: Schätzung der Kosten für das Aufschlüsseln und Anreichern von Dokumenten
Möglicherweise möchten Sie schätzen, wie viel es kostet, eine bestimmte Art von Dokument anzureichern. Die Übung unten ist nur ein Beispiel, das aber hilfreich sein kann.

Stellen Sie sich vor, wir haben 1000 PDFs, und im Durchschnitt umfasst jedes dieser Dokumente 6 Seiten. Nehmen wir für diese Übung weiterhin an, dass jede Seite ein Bild enthält. Außerdem gehen wir von durchschnittlich 3.000 Zeichen pro Seite aus. 

Angenommen, wir möchten die folgenden Schritte als Teil der Anreicherungspipeline ausführen:
1. Im Rahmen der Dokumentenaufschlüsselung sollen die Inhalte und Bilder aus dem Dokument extrahiert werden.
1. Im Rahmen der Anreicherung soll jede der extrahierten Seiten per OCR eingelesen und der Text für alle Seiten kombiniert werden. Anschließend jede der Organisationen im kombinierten Text aller Bilder extrahiert werden.

Lassen Sie uns nun schätzen, wie viel es kosten würde, diese Dokumente Schritt für Schritt zu erfassen.

Für die 1000 Dokumente:

1. Dokumentenaufschlüsselung – Es würden insgesamt 6.000 Bilder extrahiert werden. Angenommen, jeweils 1.000 extrahierte Bilder kosten 1,00 €, dann würden Kosten in Höhe von 6,00 € anfallen.

2. Wir würden den Text aus jedem dieser 6.000 Bilder extrahieren. Im Englischen verwendet die kognitive Qualifikation „OCR“ den besten Algorithmus (DescribeText). Angenommen, jeweils 1.000 zu analysierende Bilder kosten 2,50 €, dann würden für diesen Schritt 15,00 € anfallen.

3. Für die Extraktion von Entitäten würden insgesamt 3 Textdatensätze pro Seite entstehen (jeder Datensatz umfasse 1.000 Zeichen). 3 Textdatensätze/Seite * 6.000 Seiten = 18.000 Textdatensätze. Angenommen, die Kosten betragen 2,00 €/1.000 Textdatensätze, dann würde dieser Schritt uns 36,00 € kosten.

Insgesamt würden wir 57,00 € zahlen, um 1.000 PDF-Dokumente dieser Art mit der beschriebenen Qualifikationsgruppe zu erfassen.  In dieser Übung gingen wir von dem teuersten Preis pro Transaktion aus, er kann aufgrund von Staffelpreisen niedriger ausfallen. Weitere Informationen finden Sie unter [Cognitive Services-Preise](https://azure.microsoft.com/pricing/details/cognitive-services).



## <a name="next-steps"></a>Nächste Schritte
+ [Azure Search – Preise](https://azure.microsoft.com/pricing/details/search/)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Erstellen eines Skillsets (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zuordnen angereicherter Felder](cognitive-search-output-field-mapping.md)
