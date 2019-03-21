---
title: Abrufen des SDK für sprachaktivierte Geräte
titleSuffix: Azure Cognitive Services
description: Speech Services kann mit verschiedensten Geräten und Audioquellen verwendet werden. Jetzt können Sie einen Schritt weiter gehen und Ihre Sprachanwendungen mit angepasster Hardware und Software nutzen. In diesem Artikel erfahren Sie, wie Sie auf das Speech-Geräte-SDK zugreifen und mit dem Entwickeln beginnen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 3c5874625ee9d1932c401238c1586ad89d5d206d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57856721"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Abrufen des Cognitive Services Speech-Geräte-SDK

Das Speech-Geräte-SDK ist als eingeschränkte Vorschauversion verfügbar, daher müssen Sie sich für das Programm registrieren. Derzeit bevorzugt Microsoft Großunternehmen als Kandidaten für den Zugriff auf dieses Produkt.

## <a name="request-access"></a>Zugriff anfordern

So erhalten Sie Zugriff auf das Speech-Geräte-SDK

1. Navigieren Sie zum [Registrierungsformular](https://aka.ms/sdsdk-signup) für das Speech-Geräte-SDK.
1. Lesen Sie die [Lizenzbedingungen](speech-devices-sdk-license.md).
1. Wenn Sie den Lizenzbedingungen zustimmen, wählen Sie **Ich stimme zu** aus.
1. Beantworten Sie die Fragen im Formular.
1. Senden Sie das Formular ab.
1. Wenn Ihre E-Mail-Adresse noch nicht in Azure Active Directory (Azure AD) vorhanden ist, erhalten Sie nach der Zugriffsgenehmigung eine Einladungs-E-Mail ähnlich der folgenden. Wenn Ihre E-Mail-Adresse bereits in Azure AD vorhanden ist, erhalten Sie nach der Zugriffsgenehmigung eine E-Mail-Nachricht vom Microsoft Speech-Team und können direkt mit dem [Herunterladen des Speech-Geräte-SDK](#download-the-speech-devices-sdk) fortfahren.

## <a name="approval-e-mail"></a>Genehmigungs-E-Mail

```
From: Microsoft Speech Team from Microsoft (via Microsoft) <invites@microsoft.com>
Subject: You're invited to the Microsoft organization
```

![E-Mail-Nachricht](media/speech-devices-sdk/get-sdk-1.png)

## <a name="accept-access"></a>Annehmen des Zugriffs

Führen Sie die folgenden Schritte aus, um die E-Mail-Adresse, die Sie während der Registrierung angegeben haben, mit Azure AD zu verknüpfen. Mit diesem Vorgang erhalten Sie Zugriff auf die [Downloadwebsite](https://shares.datatransfer.microsoft.com/) für das Speech-Geräte-SDK.

1. Wählen Sie in der erhaltenen E-Mail-Nachricht **Erste Schritte** aus. Wenn Ihre Organisation bereits Office 365-Kunde ist, werden Sie aufgefordert, sich anzumelden, und können mit Schritt 7 fortfahren.

2. Wählen Sie im daraufhin geöffneten Browserfenster **Weiter** aus.

    ![Authentifizierungsfenster](media/speech-devices-sdk/get-sdk-2.png)

3. Erstellen Sie ein Microsoft-Konto, wenn Sie noch keines besitzen. Geben Sie dieselbe E-Mail-Adresse ein, unter der Sie die Einladungs-E-Mail erhalten haben.

    ![Erstellen eines Microsoft-Kontos](media/speech-devices-sdk/get-sdk-3.png)

4. Wählen Sie **Weiter** aus, um ein Kennwort zu erstellen.

5. Wenn Sie dazu aufgefordert werden, Ihre E-Mail-Adresse zu bestätigen, nutzen Sie den Überprüfungscode aus der Einladungs-E-Mail.

7. Fügen Sie den Sicherheitscode aus der E-Mail-Nachricht im Dialogfeld ein, oder geben Sie ihn ein. In diesem Beispiel wird der Sicherheitscode **8406** verwendet. Klicken Sie auf **Weiter**.

    ![Bestätigen der E-Mail-Adresse](media/speech-devices-sdk/get-sdk-6.png)

8. Wenn im Browser die Zugriffsbereichsanwendung angezeigt wird, haben Sie bestätigt, dass Ihre E-Mail-Adresse in Azure AD vorhanden ist. Damit haben Sie Zugriff auf die Downloadwebsite für das Speech-Geräte-SDK.

## <a name="download-the-speech-devices-sdk"></a>Herunterladen des Speech-Geräte-SDK

Navigieren Sie zur [Downloadwebsite für das Speech-Geräte-SDK](https://shares.datatransfer.microsoft.com/). Melden Sie sich mit dem Microsoft-Konto an, das Sie zuvor erstellt haben.

![Downloadwebsite für das SDK](media/speech-devices-sdk/get-sdk-7.png)

So laden Sie das Speech-Geräte-SDK, den zugehörigen Beispielcode und Referenzmaterial herunter

1. Laden Sie das Tool Aspera Connect herunter, und installieren Sie es, wenn Sie im Browser dazu aufgefordert werden.

    ![Herunterladen von Aspera Connect](media/speech-devices-sdk/get-sdk-8.png)

1. Wählen Sie **Ja** aus, um zu Aspera Connect zu wechseln.

    ![Wechseln zu Aspera Connect](media/speech-devices-sdk/get-sdk-9.png)

1. Wählen Sie **Zulassen** aus, um das Herunterladen der Dateien mit Aspera Connect zu bestätigen.

    ![Herunterladen mithilfe von Aspera Connect](media/speech-devices-sdk/get-sdk-10.png)

1. Schließen Sie das Übertragungsfenster von Aspera Connect, nachdem die Dateien heruntergeladen wurden.

    ![Übertragungsfenster von Aspera Connect](media/speech-devices-sdk/get-sdk-11.png)

Standardmäßig werden die Dateien in Ihren Ordner **Downloads** heruntergeladen. Sie können sich jetzt von dieser Website abmelden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erste Schritte mit dem Speech-Geräte-SDK](speech-devices-sdk-qsg.md)
