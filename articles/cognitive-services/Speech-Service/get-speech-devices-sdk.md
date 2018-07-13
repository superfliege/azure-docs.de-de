---
title: Abrufen des Speech-Geräte-SDK | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Zugriff auf das Speech-Geräte-SDK erhalten.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 4706ea623dccd2dbb4164bd9cccf22cff121884a
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "35378986"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Abrufen des Cognitive Services Speech-Geräte-SDK

## <a name="requesting-access"></a>Anfordern des Zugriffs

Das Speech-Geräte-SDK ist als eingeschränkte Vorschauversion verfügbar, daher müssen Sie sich für das Programm registrieren. Derzeit bevorzugt Microsoft Großunternehmen als Kandidaten für den Zugriff auf dieses Produkt.

Gehen Sie folgendermaßen vor, um Zugriff auf das Speech-Geräte-SDK zu erhalten:

1. Navigieren Sie zum [Registrierungsformular](https://aka.ms/sdsdk-signup) für das Speech-Geräte-SDK.
1. Lesen Sie die [Lizenzbedingungen](speech-devices-sdk-license.md).
1. Wenn Sie den Lizenzbedingungen zustimmen, wählen Sie „Ich stimme zu“ aus.
1. Beantworten Sie die Fragen im Formular.
1. Senden Sie das Formular ab. 
1. Wenn Ihre E-Mail-Adresse noch nicht in Azure Active Directory vorhanden ist, erhalten Sie nach der Genehmigung die nachfolgende Einladungs-E-Mail. Wenn Ihre E-Mail-Adresse bereits in Azure Active Directory vorhanden ist, erhalten Sie nach der Genehmigung eine E-Mail-Nachricht vom Microsoft Speech-Team und können direkt mit dem [Herunterladen des Speech-Geräte-SDK](#download-the-speech-devices-sdk) fortfahren.

## <a name="approval-e-mail"></a>Genehmigungs-E-Mail

```
From: Microsoft Speech Team from Microsoft (via Microsoft) <invites@microsoft.com> 
Subject: You're invited to the Microsoft organization 
```

![E-Mail-Nachricht](media/speech-devices-sdk/get-sdk-1.png)

## <a name="accept-access"></a>Annehmen des Zugriffs
Führen Sie die folgenden Schritte aus, um Azure Active Directory mit der E-Mail-Adresse, die Sie bei der Registrierung angegeben haben, beizutreten. Mit diesem Vorgang erhalten Sie Zugriff auf die [Downloadwebsite](https://shares.datatransfer.microsoft.com/) für das Speech-Geräte-SDK.

1. Klicken Sie in der erhaltenen E-Mail-Nachricht auf **Erste Schritte**. Wenn Ihre Organisation bereits Office 365-Kunde ist, werden Sie aufgefordert, sich anzumelden, und können mit Schritt 8 fortfahren.

2. Klicken Sie im geöffneten Browserfenster auf **Weiter**.

    ![Authentifizierungsfenster](media/speech-devices-sdk/get-sdk-2.png)

3. Erstellen Sie ein Microsoft-Konto, wenn Sie noch keines besitzen. Geben Sie dieselbe E-Mail-Adresse ein, an die die oben in Schritt 6 genannte Einladungs-E-Mail gesendet wurde.

    ![Erstellen eines Microsoft-Kontos](media/speech-devices-sdk/get-sdk-3.png)

4. Klicken Sie auf **Weiter**, um ein Kennwort zu erstellen.

5. Wenn Sie dazu aufgefordert werden, Ihre E-Mail-Adresse zu bestätigen, wechseln Sie zu Ihrem E-Mail-Posteingang, um den an Sie gesendeten Überprüfungscode einzusehen.
 
7. Fügen oder geben Sie den Sicherheitscode aus der E-Mail-Nachricht im Dialogfeld ein. In diesem Beispiel lautet er „8406“. Klicken Sie auf **Weiter**.

    ![E-Mail-Adresse bestätigen](media/speech-devices-sdk/get-sdk-6.png)
 
8. Wenn im Browserfenster die Zugriffsbereichsanwendung angezeigt wird, haben Sie bestätigt, dass Ihre E-Mail-Adresse (aus Schritt 6) jetzt in Azure Active Directory vorhanden ist. Damit haben Sie Zugriff auf die Downloadwebsite für das Speech-Geräte-SDK.

## <a name="download-the-speech-devices-sdk"></a>Herunterladen des Speech-Geräte-SDK

Navigieren Sie zu der [Downloadwebsite für das Speech-Geräte-SDK](https://shares.datatransfer.microsoft.com/), und melden Sie sich mit dem zuvor erstellten Microsoft-Konto an. Jetzt können Sie mit den folgenden Schritten das Speech-Geräte-SDK, den zugehörigen Beispielcode und Referenzmaterial herunterladen.

![Downloadwebsite für das SDK](media/speech-devices-sdk/get-sdk-7.png)

1. Laden Sie das Tool Aspera Connect herunter, und installieren Sie es, wenn Sie im Browser dazu aufgefordert werden.

    ![Herunterladen von Aspera Connect](media/speech-devices-sdk/get-sdk-8.png)
 
1. Klicken Sie auf **Ja**, um zu Aspera Connect zu wechseln.

    ![Wechseln zu Aspera Connect](media/speech-devices-sdk/get-sdk-9.png)
 
1. Klicken Sie auf **Zulassen**, um das Herunterladen der Dateien mit Aspera Connect zu bestätigen.

    ![Herunterladen mit Aspera Connect](media/speech-devices-sdk/get-sdk-10.png)
 
1. Schließen Sie das Übertragungsfenster von Aspera Connect, nachdem die Dateien heruntergeladen wurden.

    ![Übertragungsfenster von Aspera Connect](media/speech-devices-sdk/get-sdk-11.png)
 
Standardmäßig werden die Dateien in Ihren Ordner **Downloads** heruntergeladen. Sie können sich jetzt von dieser Website abmelden. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erste Schritte mit dem Speech-Geräte-SDK](speech-devices-sdk-qsg.md)
