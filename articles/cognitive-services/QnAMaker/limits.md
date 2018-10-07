---
title: 'Einschränkungen und Begrenzungen: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Umfassende Liste der für QnA Maker geltenden Grenzwerte.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: efdcf57fb0f175be51e6b9f8e72e4a02fcbeb5f3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162320"
---
# <a name="qna-maker-limits"></a>Grenzwerte für QnA Maker
Umfassende Liste der für QnA Maker geltenden Grenzwerte.

## <a name="knowledge-bases"></a>Knowledge Bases

* Die maximale Anzahl von Knowledge Bases basiert auf den [Grenzwerten des Azure Search-Tarifs](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity).

|**Azure Search-Tarif** | **Free** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maximal zulässige Anzahl der veröffentlichten Knowledge Bases (max. Indizes – 1, für Tests reserviert)|2|14|49|199|199|2.999|

## <a name="extraction-limits"></a>Grenzwerte für die Extraktion
* Maximale Anzahl der Dateien, die extrahiert werden können, und maximale Dateigröße: siehe [QnA Maker – Preise](https://azure.microsoft.com/en-in/pricing/details/cognitive-services/qna-maker/)
* Maximale Anzahl der Deep-Links, die zum Extrahieren von Fragen und Antworten (QnA) aus FAQ-HTML-Seiten durchlaufen werden können: 20

## <a name="metadata-limits"></a>Grenzwerte für Metadaten
* Die maximale Anzahl von Metadatenfeldern pro Knowledge Base basiert auf den [Grenzwerten des Azure Search-Tarifs](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity).

|**Azure Search-Tarif** | **Free** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maximale Anzahl der Metadatenfelder pro QnA Maker-Dienst (für alle Knowledge Bases)|1000|100*|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Grenzwerte für die Inhalte einer Knowledge Base
Allgemeine Grenzwerte für die Inhalte in der Knowledge Base:
* Länge des Antworttexts: 250.000
* Länge des Fragentexts: 1.000
* Länge des Texts für den Metadatenschlüssel/-wert: 100
* Unterstützte Zeichen für den Metadatennamen: Buchstaben, Ziffern und „_“  
* Unterstützte Zeichen für den Metadatenwert: alle Zeichen außer „:“ und „|“ 
* Länge des Dateinamens: 200
* Unterstützte Dateiformate: „.tsv“, „.pdf“, „.txt“, „.docx“, „.xlsx“.
* Maximale Anzahl von alternativen Fragen: 100
* Maximale Anzahl von Frage-Antwort-Paaren: abhängig vom ausgewählten [Azure Search-Tarif](https://docs.microsoft.com/en-in/azure/search/search-limits-quotas-capacity#document-limits) 

## <a name="create-knowledge-base-call-limits"></a>Grenzwerte für Aufrufe zum Erstellen einer Knowledge Base
Dabei handelt es sich um die Grenzwerte für die einzelnen Aktionen zum Erstellen einer Knowledge Base, d.h. Klicken auf *Create KB* (Knowledge Base erstellen) oder Aufrufen der CreateKnowledgeBase-API.
* Maximale Anzahl von alternativen Fragen pro Antwort: 100
* Maximale Anzahl von URLs: 10
* Maximale Anzahl von Dateien: 10

## <a name="update-knowledge-base-call-limits"></a>Grenzwerte für Aufrufe zum Aktualisieren einer Knowledge Base
Dabei handelt es sich um die Grenzwerte für die einzelnen Aktualisierungsaktionen, d.h. Klicken auf *Save and train* (Speichern und trainieren) oder Aufrufen der UpdateKnowledgeBase-API.
* Länge jedes Quellnamens: 300
* Maximale Anzahl der hinzugefügten oder gelöschten alternativen Fragen: 100
* Maximale Anzahl der hinzugefügten oder gelöschten Metadatenfelder: 10
* Maximale Anzahl der URLs, die aktualisiert werden können: 5
