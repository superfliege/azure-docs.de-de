---
title: Erstellen einer ersten Instanz der benutzerdefinierten Bing-Suche – Microsoft Cognitive Services
description: Zum Verwenden einer benutzerdefinierten Bing-Suche müssen Sie eine Instanz der benutzerdefinierten Suche erstellen, mit der Ihre Ansicht bzw. das Segment des Webs definiert wird. Die Instanz enthält Einstellungen, mit denen die öffentlichen Domänen, Unterwebsites und Webseiten, die mit Bing durchsucht werden sollen, und alle Rangfolgeanpassungen angegeben werden.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: v-brapel
ms.openlocfilehash: 35f0bca01de1c2087f6ae30949cca9b03192b838
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374739"
---
# <a name="create-your-first-bing-custom-search-instance"></a>Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche
Zum Verwenden einer benutzerdefinierten Bing-Suche müssen Sie eine Instanz der benutzerdefinierten Suche erstellen, mit der Ihre Ansicht bzw. das Segment des Webs definiert wird. Die Instanz enthält Einstellungen, mit denen die öffentlichen Domänen, Websites und Webseiten, die mit Bing durchsucht werden sollen, und alle Rangfolgeanpassungen angegeben werden. Verwenden Sie zum Erstellen der Instanz das [Portal](https://customsearch.ai) für die benutzerdefinierte Bing-Suche. 

## <a name="create-a-custom-search-instance"></a>Erstellen einer Instanz für die benutzerdefinierte Suche

Erstellen Sie wie folgt eine Instanz für die benutzerdefinierte Bing-Suche:

1.  Rufen Sie einen Schlüssel für die API für die benutzerdefinierte Bing-Suche ab. Weitere Informationen finden Sie unter [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  Melden Sie sich mit einem Microsoft-Konto (MSA) am Portal an. Klicken Sie auf die Schaltfläche **Anmelden**. Führen Sie die unten angegebenen zusätzlichen Schritte aus, falls Sie das Portal zum ersten Mal verwenden. Beginnen Sie andernfalls mit Schritt 3.
    - Klicken Sie auf **Microsoft-Konto erstellen**, wenn Sie kein MSA besitzen. Im Portal werden die Berechtigungen für den Zugriff auf Ihre Daten abgefragt. Klicken Sie auf **Ja**.
    - Stimmen Sie den Nutzungsbedingungen für Cognitive Services zu. Setzen Sie das Häkchen unter **Ich stimme zu**, und klicken Sie auf **Stimme zu**.  
3.  Klicken Sie nach dem Anmelden auf **Neue Instanz**, und geben Sie einen Namen für die Instanz an. Verwenden Sie einen Namen, der aussagekräftig ist und den Typ des Inhalts beschreibt, der von der Suche zurückgegeben wird. Sie können den Namen jederzeit ändern. 
4.  Geben Sie auf der **Suchoberfläche** auf der Registerkarte **Aktiv** die URL für mindestens eine Site ein, die Sie in Ihre Suche einbeziehen möchten.
5.  Um zu bestätigen, dass Ihre Instanz Ergebnisse zurückgibt, geben Sie eine Abfrage im Vorschaubereich auf der rechten Seite ein. Geben Sie eine neue Site an, falls keine Ergebnisse vorhanden sind. Bing gibt nur Ergebnisse für öffentliche Websites zurück, die indiziert wurden.
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
