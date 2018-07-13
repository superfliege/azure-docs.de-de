---
title: Exportieren und Löschen von Kundendaten – LUIS – Azure Cognitive Services | Microsoft-Dokumentation
description: Referenz zum Exportieren und Löschen von Kundendaten aus LUIS.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/23/2018
ms.author: v-geberr;
ms.openlocfilehash: f684b8ab875e2fbb774dc4a29bce25be41b24e6d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35378155"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Exportieren und Löschen Ihrer Kundendaten aus LUIS in Cognitive Services

## <a name="summary-of-customer-data-request-features"></a>Zusammenfassung der Features für Kundendatenanforderungen
Language Understanding Intelligent Service (LUIS) speichert zwar Kundeninhalte, damit der Dienst ausgeführt werden kann, aber der LUIS-Benutzer hat die vollständige Kontrolle darüber, welche Daten er abrufen, exportieren oder löschen will. Dies kann er entweder über das LUIS-[Webportal](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-reference-regions) oder die [programmgesteuerten LUIS-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) tun.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Kundeninhalte werden verschlüsselt im regionalen Microsoft Azure-Speicher gespeichert und umfassen Folgendes:

- Inhalte zum Benutzerkonto, die bei der Registrierung erfasst wurden
- Trainingsdaten, die zum Erstellen der Modelle erforderlich sind (z.B. Absichten und Entitäten)
- Benutzerabfragen, die zur Laufzeit protokolliert werden, um die Benutzerprotokolle verbessern zu können
  - Benutzer können die Abfrageprotokollierung deaktivieren, indem sie `&log=false` an die Anforderungen anfügen. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-resources-faq#how-can-i-disable-the-logging-of-utterances).

## <a name="deleting-customer-data"></a>Löschen von Kundendaten
LUIS-Benutzer haben die vollständige Kontrolle und können sämtliche Benutzerinhalte entweder über das LUIS-Webportal oder die programmgesteuerten LUIS-APIs löschen. In der folgenden Tabelle werden Links zu Hilfen für beide Möglichkeiten angezeigt.

| | **Benutzerkonto** | **Anwendung** | **Äußerung(en)** | **Abfragen von Endbenutzern** |
| --- | --- | --- | --- | --- |
| **Portal** | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-how-to-account-settings) | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) |
| **APIs** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Exportieren von Kundendaten
LUIS-Benutzer haben zwar die vollständige Kontrolle und können die Daten im Portal abrufen, sie müssen sie jedoch über die programmgesteuerte LUIS-APIs exportieren. In der folgenden Tabelle werden Links aufgeführt, über die Sie Unterstützung beim Exportieren von Daten über die programmgesteuerte LUIS-API erhalten.

| | **Benutzerkonto** | **Anwendung** | **Äußerung(en)** | **Abfragen von Endbenutzern** |
| --- | --- | --- | --- | --- |
| **APIs** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [LUIS regions reference (Referenz zu LUIS-Regionen)](./luis-reference-regions.md)
