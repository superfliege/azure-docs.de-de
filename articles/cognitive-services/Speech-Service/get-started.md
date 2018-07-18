---
title: Kostenloses Testen des Speech-Diensts | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Speech-Dienst kostenlos testen können.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/17/2018
ms.author: v-jerkin
ms.openlocfilehash: 3feef04694336d9173b419285a96fcaef543e18f
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "35378938"
---
# <a name="try-the-speech-service-for-free"></a>Kostenloses Testen des Speech-Diensts

Die ersten Schritte mit dem Speech-Dienst sind einfach und erschwinglich. Mit einer 30-tägigen kostenlosen Testversion können Sie die Funktionen des Diensts kennenlernen und feststellen, ob er sich für Ihre Anwendungen eignet.

Wenn Sie mehr Zeit benötigen, können Sie sich für ein Microsoft Azure-Konto registrieren: Es beinhaltet ein Guthaben von 200 US-Dollar, das Sie bis zu 30 Tage lang auf ein kostenpflichtiges Abonnement für den Speech-Dienst anwenden können.

Schließlich bietet der Speech-Dienst einen kostenlosen Tarif mit geringem Datenvolumen, der sich für die Entwicklung von Anwendungen eignet. Dieses kostenlose Abonnement können Sie auch nach Ablauf des Guthabens behalten.

## <a name="free-trial"></a>Kostenlose Testversion

Mit der 30-tägigen kostenlosen Testversion haben Sie in einem begrenzten Zeitraum Zugriff auf den S0-Standardtarif. Gehen Sie folgendermaßen vor, um sich für eine 30-tägige kostenlose Testversion zu registrieren.

1. Navigieren Sie zur Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/).

1. Wechseln Sie zur Registerkarte für die Spracheingabe-APIs, und klicken Sie auf die Schaltfläche **API-Schlüssel abrufen** neben „Sprachdienste“.

   ![Registerkarte „Spracheingabe-APIs“](media/index/try-speech-api-free-trial1.png)<br>
   ![API-Schlüssel](media/index/try-speech-api-free-trial2.png)

3. Stimmen Sie den Bedingungen zu, und wählen Sie im Dropdownmenü Ihr Gebietsschema aus.

   ![Akzeptieren der Bedingungen](media/index/try-speech-api-free-trial3.png)

4. Melden Sie sich mit Ihrem Microsoft-, Facebook-, LinkedIn- oder GitHub-Konto an. Alternativ können Sie sich für ein kostenloses Microsoft-Konto registrieren:

    * Wechseln Sie zum [Microsoft-Kontoportal](https://account.microsoft.com/account).
    * Klicken Sie auf **Bei Microsoft anmelden**.

    ![Anmelden](media/index/try-speech-api-free-trial4.png)

    * Wenn Sie aufgefordert werden, sich anzumelden, klicken Sie auf „Jetzt erstellen“.

    ![Erstellen eines neuen Kontos](media/index/try-speech-api-free-trial5.png)

    * Geben Sie in den folgenden Schritten Ihre E-Mail-Adresse oder Telefonnummer ein, weisen Sie ein Kennwort zu, und befolgen Sie die Anweisungen, um Ihr neues Microsoft-Konto zu überprüfen.

Nachdem Sie sich angemeldet haben, wird Ihre kostenlose Testversion gestartet. Auf der angezeigten Webseite sind alle Cognitive Services aufgeführt, für die Sie derzeit über Testabonnements verfügen. Neben „Sprachdienste“ werden zwei Abonnementschlüssel angezeigt. Sie können beide Schlüssel in Ihren Anwendungen verwenden.

> [!NOTE]
> Alle kostenlosen Testabonnements sind der Region „USA, Westen“ zugewiesen. Stellen Sie sicher, dass Sie beim Senden von Anforderungen den Ihrer Region entsprechenden Endpunkt verwenden.

## <a name="new-azure-account"></a>Neues Azure-Konto

Neue Azure-Konten erhalten ein Guthaben von 200 US-Dollar, das bis zu 30 Tage gilt. Dieses Guthaben kann zum weiteren Testen des Speech-Diensts oder zur Anwendungsentwicklung verwendet werden.

Gehen Sie folgendermaßen vor, um sich für ein neues Azure-Konto zu registrieren.

1. Wechseln Sie zur [Azure-Registrierungsseite](https://azure.microsoft.com/free/ai/). 

1. Klicken Sie auf **Kostenlos einsteigen**.

    ![Kostenlos starten](media/index/try-speech-api-new-azure1.png)

3. Melden Sie sich mit Ihrem Microsoft-Konto an. Wenn Sie noch keines besitzen:

    * Wechseln Sie zum [Microsoft-Kontoportal](https://account.microsoft.com/account).
    * Klicken Sie auf **Bei Microsoft anmelden**.
    * Wenn Sie aufgefordert werden, sich anzumelden, klicken Sie auf „Jetzt erstellen“.
    * Geben Sie in den folgenden Schritten Ihre E-Mail-Adresse oder Telefonnummer ein, weisen Sie ein Kennwort zu, und befolgen Sie die Anweisungen, um Ihr neues Microsoft-Konto zu überprüfen.

1. Geben Sie die übrigen Informationen ein, die zum Registrieren für ein Konto erforderlich sind. Geben Sie das Land und Ihren Namen sowie eine Telefonnummer und eine E-Mail-Adresse an.

    ![Eingeben der Informationen](media/index/try-speech-api-new-azure2.png)

    Bestätigen Sie Ihre Identität per Telefon und durch Angabe einer Kreditkartennummer, und stimmen Sie dann den Bedingungen des Azure-Benutzervertrags zu. (Ihre Kreditkarte wird nicht belastet.)

    ![Annehmen der Vereinbarung](media/index/try-speech-api-new-azure3.png)

Damit ist Ihr kostenloses Azure-Konto erstellt. Führen Sie die Schritte im nächsten Abschnitt aus, um ein Abonnement für den Speech-Dienst zu erstellen.

## <a name="create-a-speech-resource-in-azure"></a>Erstellen einer Speech-Ressource in Azure

Gehen Sie wie folgt vor, um Ihrem Azure-Konto eine Ressource des Speech-Diensts hinzuzufügen.

1. Melden Sie sich mit Ihrem Microsoft-Konto beim [Azure-Portal](https://ms.portal.azure.com/) an.

1. Klicken Sie links oben im Portal auf **Ressource erstellen** (grünes **+**-Symbol).

    ![Erstellen von Ressourcen](media/index/try-speech-api-create-speech1.png)

1. Suchen Sie im Fenster „Neu“ nach „Speech“.

    ![Klicken auf „Speech“](media/index/try-speech-api-create-speech2.png)

1. Klicken Sie in den Suchergebnissen auf „Speech (Vorschau)“.

1. Klicken Sie unten im Bereich für den Speech-Dienst auf **Erstellen**.

    ![Klicken Sie auf "Erstellen"](media/index/try-speech-api-create-speech3.png)

1. Geben Sie im Bereich „Erstellen“ Folgendes ein:

    * Einen Namen für die neue Ressource. Über den Namen können Sie mehrere Abonnements für denselben Dienst unterscheiden.
    * Wählen Sie das Azure-Abonnement aus, dem die neue Ressource zugeordnet ist, um zu bestimmen, wie die Gebühren berechnet werden.
    * Wählen Sie die Region aus, in der die Ressource verwendet wird. Derzeit ist der Speech-Dienst in den Regionen „Asien, Osten“, „Europa, Norden“ und „USA, Westen“ verfügbar.
    * Wählen Sie den Tarif aus: entweder F0 (begrenztes kostenloses Abonnement) oder S0 (Standardabonnement). Klicken Sie auf **Alle Preisinformationen anzeigen**, um vollständige Informationen zu Preisen und Verwendungskontingenten für die einzelnen Tarife zu erhalten.
    * Erstellen Sie eine neue Ressourcengruppe für dieses Abonnement für den Speech-Dienst, oder weisen Sie dieses einer vorhandenen Ressourcengruppe zu. Anhand von Ressourcengruppen können Sie Ihre verschiedenen Azure-Abonnements organisieren.
    * Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, um zukünftig einfach auf Ihr Abonnement zugreifen zu können.
    * Klicken Sie auf **Erstellen**.

    ![Klicken auf „Erstellen“ im Bereich](media/index/try-speech-api-create-speech4.png)

    Es kann einen Moment dauern, bis die neue Speech-Ressource erstellt und bereitgestellt wird. Der Bereich „Schnellstart“ wird mit Informationen zu der neuen Ressource angezeigt.

    ![Bereich „Schnellstart“](media/index/try-speech-api-create-speech5.png)

1. Klicken Sie im Bereich „Schnellstart“ auf den Link **Schlüssel** unter Schritt 1, um Ihre Abonnementschlüssel anzuzeigen. Jedem Abonnement sind zwei Schlüssel zugewiesen, Sie können beide in Ihrem Abonnement verwenden. Klicken Sie jeweils auf die Schaltfläche neben einem Schlüssel, um ihn in die Zwischenablage zu kopieren und in den Code einzufügen.

> [!NOTE]
> Sie können beliebig viele Abonnements mit dem Tarif „Standard“ in einer oder mehreren Regionen erstellen. Sie können dagegen nur ein Abonnement mit dem Tarif „Free“ erstellen.

## <a name="next-steps"></a>Nächste Schritte

Laden Sie ein SDK und einige Codebeispiele herunter, um den Speech-Dienst zu testen.

> [!div class="nextstepaction"]
> [Speech SDKs](speech-sdk.md)

> [!div class="nextstepaction"]
> [Beispielcode](samples.md)
