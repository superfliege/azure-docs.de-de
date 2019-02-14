---
title: 'Tutorial: Facebook-Inhaltsmoderation – Azure Content Moderator'
titlesuffix: Azure Cognitive Services
description: In diesem Tutorial wird beschrieben, wie Sie Content Moderator auf Machine Learning-Basis verwenden, um Facebook-Posts und -Kommentare zu moderieren.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 86c89164e3ccd5bf5df303b98cf6d336f3916e2b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878051"
---
# <a name="tutorial-facebook-content-moderation-with-content-moderator"></a>Tutorial: Facebook-Inhaltsmoderation mit Content Moderator

In diesem Tutorial wird beschrieben, wie Sie Content Moderator auf Machine Learning-Basis verwenden, um Facebook-Posts und -Kommentare zu moderieren.

Im Tutorial werden folgende Schritte erläutert:

1. Erstellen eines Content Moderator-Teams
2. Erstellen von Azure-Funktionen, die auf HTTP-Ereignisse von Content Moderator und Facebook lauschen
3. Erstellen einer Facebook-Seite und -App und Herstellen der Verbindung mit Content Moderator

Wenn wir fertig sind, sendet Facebook die von den Besuchern geposteten Inhalte an Content Moderator. Basierend auf den Übereinstimmungsschwellenwerten veröffentlichen Ihre Content Moderator-Workflows entweder die Inhalte oder erstellen im Prüfungstool Überprüfungen. 

In der folgenden Abbildung sind die Bausteine der Lösung dargestellt.

![Facebook-Beitragsmoderation](images/tutorial-facebook-moderation.png)

## <a name="create-a-content-moderator-team"></a>Erstellen eines Content Moderator-Teams

Informationen zur Registrierung für Content Moderator und zum Zusammenstellen eines Teams finden Sie unter [Schnellstart: Erste Schritte mit Content Moderato](quick-start.md).

## <a name="configure-image-moderation-workflow-threshold"></a>Konfigurieren des Workflows für die Bildmoderation (Schwellenwert)

Verwenden Sie die Seite [Workflows](review-tool-user-guide/workflows.md), um einen benutzerdefinierten Bildworkflow (Schwellenwert) zu konfigurieren. Notieren Sie sich den **Namen** des Workflows.

## <a name="3-configure-text-moderation-workflow-threshold"></a>3. Konfigurieren des Workflows für die Textmoderation (Schwellenwert)

Führen Sie ähnliche Schritte wie auf der Seite [Workflows](review-tool-user-guide/workflows.md) aus, um einen Schwellenwert und Workflow für benutzerdefinierten Text zu konfigurieren. Notieren Sie sich den **Namen** des Workflows.

![Konfigurieren des Textworkflows](images/text-workflow-configure.PNG)

Testen Sie Ihren Workflow mit der Schaltfläche „Workflow ausführen“.

![Testen des Textworkflows](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Erstellen von Azure-Funktionen

Melden Sie sich am [Azure-Verwaltungsportal](https://portal.azure.com/) an, um Ihre Azure-Funktionen zu erstellen. Folgen Sie diesen Schritten:

1. Erstellen Sie eine Azure-Funktionen-App gemäß der Anleitung auf der Seite [Erstellen einer Funktionen-App im Azure-Portal](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal).
2. Öffnen Sie die neu erstellte Funktionen-App.
3. Navigieren Sie in der App zu **Plattformfeatures > Anwendungseinstellungen**.
4. Definieren Sie die folgenden [Anwendungseinstellungen](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings):

> [!NOTE]
> **cm: Region** sollte den Namen der Region enthalten (ohne Leerzeichen).
> Beispielsweise **westeurope** anstelle von „West Europe“, **westcentralus** anstelle von „West Central US“ usw.
>

| App-Einstellung | BESCHREIBUNG   | 
| -------------------- |-------------|
| cm:TeamId   | Ihre Content Moderator-Team-ID  | 
| cm:SubscriptionKey | Ihr Content Moderator-Abonnementschlüssel – Siehe [Anmeldeinformationen](review-tool-user-guide/credentials.md) | 
| cm:Region | Ihr Content Moderator-Regionsname, ohne Leerzeichen. Siehe vorherigen Hinweis. |
| cm:ImageWorkflow | Name des Workflows für Bilder |
| cm:TextWorkflow | Name des Workflows für Text |
| cm:CallbackEndpoint | URL für die CMListener-Funktionen-App, die Sie später in diesem Leitfaden erstellen |
| fb:VerificationToken | Das geheime Token, das auch zum Abonnieren der Facebook-Feedereignisse verwendet wird |
| fb:PageAccessToken | Das Zugriffstoken für die Facebook-Graph-API läuft nicht ab und lässt die Funktion zum Ausblenden/Löschen von Posts in Ihrem Namen zu |

5. Erstellen Sie eine neue **HttpTrigger-CSharp**-Funktion mit dem Namen **FBListener**. Diese Funktion empfängt Ereignisse von Facebook. Erstellen Sie diese Funktion, indem Sie folgenden Schritte ausführen:

    1. Lassen Sie die Seite [Erstellen einer Funktionen-App im Azure-Portal](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) zu Referenzzwecken geöffnet.
    2. Klicken Sie auf das Pluszeichen (**+**), um die neue Funktion zu erstellen.
    3. Wählen Sie anstelle der integrierten Vorlagen die Option **Get started on your own/custom function** (Selbstständig einsteigen/Benutzerdefinierte Funktion).
    4. Klicken Sie auf die Kachel, auf der **HttpTrigger-CSharp** angezeigt wird.
    5. Geben Sie den Namen **FBListener** ein. Das Feld **Autorisierungsstufe** sollte auf **Funktion** festgelegt sein.
    6. Klicken Sie auf **Create**.
    7. Ersetzen Sie den Inhalt von **run.csx** durch den Inhalt von [**FbListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FbListener/run.csx).

6. Erstellen Sie eine neue **HttpTrigger-CSharp**-Funktion mit dem Namen **CMListener**. Diese Funktion empfängt Ereignisse von Content Moderator. Führen Sie diese Schritte aus, um die Funktion zu erstellen.

    1. Lassen Sie die Seite [Erstellen einer Funktionen-App im Azure-Portal](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) zu Referenzzwecken geöffnet.
    2. Klicken Sie auf das Pluszeichen (**+**), um die neue Funktion zu erstellen.
    3. Wählen Sie anstelle der integrierten Vorlagen die Option **Get started on your own/custom function** (Selbstständig einsteigen/Benutzerdefinierte Funktion).
    4. Klicken Sie auf die Kachel, auf der **HttpTrigger-CSharp** angezeigt wird.
    5. Geben Sie den Namen **CMListener** ein. Das Feld **Autorisierungsstufe** sollte auf **Funktion** festgelegt sein.
    6. Klicken Sie auf **Create**.
    7. Ersetzen Sie den Inhalt von **run.csx** durch den Inhalt von [**CMListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/CmListener/run.csx).

## <a name="configure-the-facebook-page-and-app"></a>Konfigurieren der Facebook-Seite und der App
1. Erstellen Sie eine Facebook-App.

    1. Navigieren Sie zur [Facebook-Entwicklerwebsite](https://developers.facebook.com/).
    2. Klicken Sie auf **My Apps** (Meine Apps).
    3. Fügen Sie eine neue App hinzu.
    4. Wählen Sie **Webhooks > Get Started** (Webhooks > Erste Schritte).
    5. Wählen Sie **Page > Subscribe to this topic** (Seite > Dieses Thema abonnieren).
    6. Geben Sie die **FBListener Url** als Rückruf-URL und das von Ihnen in den **Funktionen-App-Einstellungen** konfigurierte **Überprüfungstoken** an.
    7. Scrollen Sie nach dem Abonnieren nach unten zum Feed, und wählen Sie **Subscribe** (Abonnieren).

2. Erstellen Sie eine Facebook-Seite.

    1. Navigieren Sie zu [Facebook](https://www.facebook.com/bookmarks/pages), und erstellen Sie eine **neue Facebook-Seite**.
    2. Lassen Sie für die Facebook-App den Zugriff auf diese Seite zu, indem Sie die folgenden Schritte ausführen:
        1. Navigieren Sie zum [Graph-API-Explorer](https://developers.facebook.com/tools/explorer/).
        2. Wählen Sie die Option **Anwendung**.
        3. Wählen Sie die Option **Page Access Token** (Seitenzugriffstoken), und senden Sie eine **Get**-Anforderung.
        4. Klicken Sie in der Antwort auf die **Page ID** (Seiten-ID).
        5. Fügen Sie jetzt **/subscribed_apps** an die URL an, und senden Sie eine **Get**-Anforderung (leere Antwort).
        6. Übermitteln Sie eine **POST**-Anforderung. Sie erhalten eine Antwort der Form **success: true**.

3. Erstellen Sie ein nicht ablaufendes Graph-API-Zugriffstoken.

    1. Navigieren Sie zum [Graph-API-Explorer](https://developers.facebook.com/tools/explorer/).
    2. Wählen Sie die Option **Application** (Anwendung).
    3. Wählen Sie die Option **Get User Access Token** (Benutzerzugriffstoken abrufen).
    4. Wählen Sie unter **Select Permissions** (Berechtigungen auswählen) die Optionen **manage_pages** und **publish_pages**.
    5. Wir verwenden das **Zugriffstoken** (kurzlebiges Token) im nächsten Schritt.

4. Für die nächsten Schritte verwenden wir Postman.

    1. Öffnen Sie **Postman** (oder rufen Sie die Anwendung [hier](https://www.getpostman.com/) ab).
    2. Importieren Sie diese beiden Dateien:
        1. [Postman-Sammlung](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Postman-Umgebung](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Aktualisieren Sie diese Umgebungsvariablen:
    
    | Schlüssel | Wert   | 
    | -------------------- |-------------|
    | appId   | Fügen Sie hier Ihre Facebook-App-ID ein.  | 
    | appSecret | Fügen Sie hier Ihr Facebook-App-Geheimnis ein. | 
    | short_lived_token | Fügen Sie das kurzlebige Benutzerzugriffstoken ein, das Sie im vorherigen Schritt generiert haben. |
    4. Führen Sie nun die drei APIs aus, die in der Sammlung aufgeführt sind: 
        1. Wählen Sie **Generate Long-Lived Access Token** (Langlebiges Zugriffstoken generieren), und klicken Sie auf **Send** (Senden).
        2. Wählen Sie **Get User ID** (Benutzer-ID abrufen), und klicken Sie auf **Send** (Senden).
        3. Wählen Sie **Get Permanent Page Access Token** (Dauerhaftes Seitenzugriffstoken abrufen), und klicken Sie auf **Send** (Senden).
    5. Kopieren Sie den Wert für **access_token** aus der Antwort, und weisen Sie ihn der App-Einstellung **fb:PageAccessToken** zu.

Das ist alles!

Die Lösung sendet alle Bilder und Texte, die auf Ihrer Facebook-Seite gepostet werden, an Content Moderator. Die Workflows, die Sie zuvor konfiguriert haben, werden aufgerufen. Die Inhalte, die die in den Workflows definierten Kriterien nicht erfüllen, führen zu Überprüfungen im Prüfungstool. Der Rest des Inhalts wird veröffentlicht.

## <a name="license"></a>Lizenz

Alle Microsoft Cognitive Services-SDKs und -Beispiele sind mit der MIT-Lizenz lizenziert. Weitere Informationen finden Sie unter [The MIT License (MIT)](https://microsoft.mit-license.org/) (Die MIT-Lizenz (MIT)).

## <a name="next-steps"></a>Nächste Schritte

1. [Sehen Sie sich eine Demonstration (Video)](https://channel9.msdn.com/Events/Build/2017/T6033) dieser Lösung auf der Microsoft Build 2017 an.
1. [Facebook-Beispiel auf GitHub](https://github.com/MicrosoftContentModerator/samples-fbPageModeration)
1. https://docs.microsoft.com/azure/azure-functions/functions-create-github-webhook-triggered-function
2. http://ukimiawz.github.io/facebook/2015/08/12/webhook-facebook-subscriptions/
3. http://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token
