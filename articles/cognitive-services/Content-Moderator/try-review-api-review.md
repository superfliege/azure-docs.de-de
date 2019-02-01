---
title: Moderieren von Inhalten mit Überprüfungen durch Personen mit der API-Konsole – Content Moderator
titlesuffix: Azure Cognitive Services
description: Verwenden Sie die Überprüfungsvorgänge der Überprüfungs-API, um Bild- oder Textüberprüfungen für die Moderation durch Personen zu erstellen.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 7e4c750e7470c6a76e56bd67d6a134b1b61535eb
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55218389"
---
# <a name="create-reviews-from-the-api-console"></a>Erstellen von Überprüfungen über die API-Konsole

Verwenden Sie die [Überprüfungsvorgänge](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4) der Überprüfungs-API, um Bild- und Textüberprüfungen für die Moderation durch Personen zu erstellen. Menschliche Moderatoren verwenden zur Überprüfung von Inhalten das Prüfungstool. Verwenden Sie diesen Vorgang basierend auf der Geschäftslogik Ihrer Beitragsmoderation. Verwenden Sie es, nachdem Sie Ihre Inhalte mit einer der Bild- oder Text-APIs von Content Moderator oder einer anderen Cognitive Services-API durchsucht haben. 

Nachdem ein menschlicher Moderator die automatisch zugewiesenen Tags und Vorhersagedaten überprüft und eine finale Moderationsentscheidung getroffen hat, sendet die Überprüfungs-API alle Informationen an Ihren API-Endpunkt.

## <a name="use-the-api-console"></a>Verwenden der API-Konsole
Um die API über die Online-Konsole zu testen, benötigen Sie einige Werte, die Sie in die Konsole eingeben müssen:

- **teamName**: Der Teamname, den Sie beim Einrichten Ihres Prüfungstoolkontos erstellt haben. 
- **ContentId**: Diese Zeichenfolge wird an die API übergeben und durch den Rückruf zurückgegeben. Die „ContentId“ ist nützlich, um interne Bezeichner oder Metadaten mit den Ergebnissen eines Moderationsauftrags zu verknüpfen.
- **Metadaten**: Benutzerdefinierte Schlüssel-Wert-Paare, die während des Rückrufs an Ihren API-Endpunkt zurückgegeben werden. Wenn dieser Schlüssel ein im Prüfungstool definierter kurzer Code ist, wird er als Tag angezeigt.
- **Ocp-Apim-Subscription-Key**: Befindet sich auf der Registerkarte **Einstellungen**. Weitere Informationen finden Sie in der [Übersicht](overview.md).

Die einfachste Möglichkeit, auf eine Testkonsole zuzugreifen, ist über das Fenster **Anmeldeinformationen**.

1.  Wählen Sie im Fenster **Anmeldeinformationen** die Option [Überprüfungs-API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

  Die Seite **Überprüfung – Erstellen** wird geöffnet.

2.  Wählen Sie für **API-Testkonsole öffnen** die Region aus, die Ihrem Standort am ehesten nahekommt.

  ![Regionsauswahl auf der Seite „Überprüfung – Erstellen“](images/test-drive-region.png)

  Die API-Konsole **Überprüfung – Erstellen** wird geöffnet.
  
3.  Geben Sie Werte für die gewünschten Abfrageparameter, den Inhaltstyp und Ihren Abonnementschlüssel ein. Geben Sie im Feld **Anforderungstext** den Inhalt (z.B. den Speicherort des Bildes), Metadaten und andere mit dem Inhalt verbundene Informationen an.

  ![Abfrageparameter, Header und Feld „Anforderungstext“ in der Konsole „Überprüfen – Erstellen“](images/test-drive-review-1.PNG)
  
4.  Wählen Sie **Senden** aus. Es wird eine Überprüfungs-ID erstellt. Kopieren Sie diese ID für die folgenden Schritte.

  ![Feld „Antwortinhalt“ mit Überprüfungs-ID in der Konsole „Überprüfen – Erstellen“](images/test-drive-review-2.PNG)
  
5.  Wählen Sie **Abrufen**, und öffnen Sie die API durch Auswählen der Schaltfläche, die Ihrer Region entspricht. Geben Sie auf der Ergebnisseite die Werte für **teamName**, **ReviewID** und **subscription key** ein. Wählen Sie die Schaltfläche **Senden** auf der Seite. 

  ![Abrufergebnisse in der Konsole „Überprüfen – Erstellen“](images/test-drive-review-3.PNG)
  
6.  Es werden die Ergebnisse des Suchvorgangs angezeigt.

  ![Feld „Antwortinhalt“ in der Konsole „Überprüfen – Erstellen“](images/test-drive-review-4.PNG)
  
7.  Wählen Sie auf dem Content Moderator-Dashboard **Überprüfung** > **Bild**. Das durchsuchte Bild wird angezeigt und kann für die Überprüfung durch Personen verwendet werden.

  ![Prüfungstool-Bild eines Fußballs](images/test-drive-review-5.PNG)

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die REST-API in Ihrem Code, oder beginnen Sie mit dem [.NET-Schnellstart für Überprüfungen](moderation-reviews-quickstart-dotnet.md), die in Ihre Anwendung integriert werden sollen.
