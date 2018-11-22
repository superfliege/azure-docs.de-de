---
title: 'Schnellstart: Erstellen der ersten Instanz der benutzerdefinierten Bing-Suche'
titlesuffix: Azure Cognitive Services
description: Zum Verwenden einer benutzerdefinierten Bing-Suche müssen Sie eine Instanz der benutzerdefinierten Suche erstellen, mit der Ihre Ansicht bzw. das Segment des Webs definiert wird. Die Instanz enthält Einstellungen, mit denen die öffentlichen Domänen, Unterwebsites und Webseiten, die mit Bing durchsucht werden sollen, und alle Rangfolgeanpassungen angegeben werden.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2017
ms.author: aahi
ms.openlocfilehash: c9b37486d664920bbc4b85a0715ce7f5ea910365
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52161539"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Schnellstart: Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche
Zum Verwenden einer benutzerdefinierten Bing-Suche müssen Sie eine Instanz der benutzerdefinierten Suche erstellen, mit der Ihre Ansicht bzw. das Segment des Webs definiert wird. Die Instanz enthält Einstellungen, mit denen die öffentlichen Domänen, Websites und Webseiten, die mit Bing durchsucht werden sollen, und alle Rangfolgeanpassungen angegeben werden. Verwenden Sie zum Erstellen der Instanz das [Portal](https://customsearch.ai) für die benutzerdefinierte Bing-Suche. 

## <a name="create-a-custom-search-instance"></a>Erstellen einer Instanz für die benutzerdefinierte Suche

Erstellen Sie wie folgt eine Instanz für die benutzerdefinierte Bing-Suche:

1.  Rufen Sie einen Schlüssel für die API für die benutzerdefinierte Bing-Suche ab. Weitere Informationen finden Sie unter [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  Klicken Sie auf die Schaltfläche **Anmelden**, und melden Sie sich mit einem Microsoft-Konto (MSA) beim Portal an. 
    - Klicken Sie auf **Microsoft-Konto erstellen**, wenn Sie kein MSA besitzen. Im Portal werden die Berechtigungen für den Zugriff auf Ihre Daten abgefragt. Klicken Sie auf **Ja**.
    - Stimmen Sie den Nutzungsbedingungen für Cognitive Services zu. Setzen Sie das Häkchen unter **Ich stimme zu**, und klicken Sie auf **Stimme zu**.  
3.  Klicken Sie nach dem Anmelden auf **Neue Instanz**, und geben Sie einen Namen für die Instanz an. Verwenden Sie einen Namen, der aussagekräftig ist und den Typ des Inhalts beschreibt, der von der Suche zurückgegeben wird. Sie können den Namen jederzeit ändern. 
4.  Geben Sie auf der **Suchoberfläche** auf der Registerkarte **Aktiv** die URL für mindestens eine Website ein, die Sie in Ihre Suche einbeziehen möchten.
5.  Um zu bestätigen, dass Ihre Instanz Ergebnisse zurückgibt, geben Sie eine Abfrage im Vorschaubereich auf der rechten Seite ein. Geben Sie eine neue Website an, falls keine Ergebnisse zurückgegeben werden. Bing gibt nur Ergebnisse für öffentliche Websites zurück, die indiziert wurden.
6.  Klicken Sie auf **Veröffentlichen**, um Konfigurationsänderungen für die Produktion zu veröffentlichen. Klicken Sie bei entsprechender Aufforderung auf **Veröffentlichen**, um dies zu bestätigen.
7.  Klicken Sie auf **Produktion** > **Endpunkte**, und kopieren Sie die **Custom Configuration ID** (Benutzerdefinierte Konfigurations-ID). Sie benötigen diese ID zum Aufrufen der API für die benutzerdefinierte Bing-Suche.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit der Verwendung der hier von Ihnen erstellten Instanz für die benutzerdefinierte Suche fort, indem Sie die Anleitungen in diesen Leitfäden befolgen:

- [Konfigurieren der Benutzeroberfläche für die benutzerdefinierte Suche](./define-your-custom-view.md)
- [Aufrufen der benutzerdefinierten Suche](./search-your-custom-view.md)
- [Freigeben der benutzerdefinierten Suche](./share-your-custom-search.md)
- [Konfigurieren der gehosteten Benutzeroberfläche](./hosted-ui.md)
- [Verwenden von Decorator-Markierungen zum Hervorheben von Text](./hit-highlighting.md)
- [Einteilen von Webseiten](./page-webpages.md)
