---
title: Erstellen eines benutzerdefinierten Aktivierungsworts
description: Erstellen eines benutzerdefinierten Aktivierungsworts für das SDK für sprachaktivierte Geräte.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 615a901c70fff92141442699ea6e4b8fce1c9ace
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282572"
---
# <a name="create-a-custom-wake-word-using-speech-service"></a>Erstellen eines benutzerdefinierten Aktivierungsworts mithilfe des Speech Service

Ihr Gerät lauscht immer auf ein Aktivierungswort (oder einen Aktivierungsausdruck). „Hey Cortana“ ist beispielsweise ein Aktivierungswort für den Cortana-Assistenten. Wenn der Benutzer das Aktivierungswort nennt, beginnt das Gerät, sämtliche nachfolgende Audioaufnahmen an die Cloud zu senden, bis der Benutzer aufhört zu sprechen. Durch das Anpassen Ihres Aktivierungsworts können Sie Ihr Gerät auf effektive Weise differenzieren und Ihr Branding stärken.

In diesem Artikel erfahren Sie, wie Sie für Ihr Gerät ein benutzerdefiniertes Aktivierungswort erstellen.

## <a name="choosing-an-effective-wake-word"></a>Auswählen eines effektiven Aktivierungsworts

Berücksichtigen Sie bei der Auswahl eines Aktivierungsworts die folgenden Richtlinien.

* Ihr Aktivierungswort sollte ein englisches Wort oder ein englischer Ausdruck sein. Es sollte nicht länger als zwei Sekunden dauern, es auszusprechen.

* Wörter mit 4 – 7 Silben eignen sich am besten. So ist „Hey, Computer“ beispielsweise ein gutes Aktivierungswort, während nur „Hey“ ein schlechtes Aktivierungswort ist.

* Bei Aktivierungswörtern sollten englische Ausspracheregeln befolgt werden.

* Durch ein einzigartiges Wort oder ein Kunstwort, das die allgemeinen englischen Ausspracheregeln befolgt, könnten Ergebnisse mit falsch positiven Werten verringert werden. „Computerama“ würde sich beispielsweise gut als Aktivierungswort eignen.

* Wählen Sie kein gängiges Wort aus. Die Wörter „eat“ und „go“ werden beispielsweise häufig in gewöhnlichen Konversationen gesagt. Sie könnten falsche Auslöser für Ihr Gerät sein.

* Vermeiden Sie die Verwendung eines Aktivierungsworts, das verschiedene Aussprachevarianten haben könnte. Benutzern müsste die „richtige“ Aussprache bekannt sein, um eine Antwort von ihrem Gerät erhalten zu können. „509“ könnte beispielsweise als „five zero nine“, „five oh nine“ oder als „five hundred and nine“ ausgesprochen werden. „R.E.I.“ könnte als „R E I“ oder als „Ray“ ausgesprochen werden. „Live“ könnte als [līv] oder als [liv] ausgesprochen werden.

* Verwenden Sie keine Sonderzeichen, Symbole oder Ziffern. „Go#“ und „20 + cats“ wären beispielsweise keine guten Aktivierungswörter. „Go sharp“ oder „twenty plus cats“ könnte jedoch funktionieren. Sie können weiterhin die Symbole in Ihrem Branding sowie Marketing und Dokumentation zur Untermauerung der richtigen Aussprache verwenden.

> [!NOTE]
> Wenn Sie als Aktivierungswort ein markenrechtlich geschütztes Wort auswählen, sollten Sie sicherstellen, dass Sie Eigentümer dieser Marke sind oder über eine Berechtigung vom Eigentümer der Marke zur Verwendung des Worts verfügen. Microsoft haftet nicht für rechtliche Probleme, die möglicherweise durch Ihre Auswahl eines Aktivierungsworts auftreten.

## <a name="creating-your-wake-word"></a>Erstellen Ihres Aktivierungsworts

Bevor Sie auf Ihrem Gerät ein benutzerdefiniertes Aktivierungswort verwenden können, müssen Sie es mithilfe des Microsoft-Diensts für die benutzerdefinierte Generierung eines Aktivierungsworts erstellen. Nachdem Sie ein Aktivierungswort angegeben haben, erstellt der Dienst eine Datei, die Sie anschließend zur Aktivierung Ihres Aktivierungsworts auf Ihrem Gerät in Ihrem Development Kit bereitstellen.

1. Wechseln Sie zum [Portal des Custom Speech Service](https://cris.ai/).

2. Erstellen Sie ein neues Konto. Verwenden Sie dabei die E-Mail-Adresse, an die Ihnen die Einladung für Azure Active Directory gesendet wurde. 

    ![Neues Konto erstellen](media/speech-devices-sdk/wake-word-1.png)
 
3.  Füllen Sie nach der Anmeldung das Formular aus, und klicken Sie anschließend auf **Start my journey** (Journey starten).

    ![Erfolgreich angemeldet](media/speech-devices-sdk/wake-word-3.png)
 
4. Die Seite **Custom Wake Word** (Benutzerdefiniertes Aktivierungswort) ist nicht für die Öffentlichkeit verfügbar. Folglich ist kein Link vorhanden, über den Sie zu der Seite gelangen. Klicken Sie stattdessen auf den folgenden Link, oder fügen Sie diesen ein: https://cris.ai/customkws.

    ![Ausgeblendete Seite](media/speech-devices-sdk/wake-word-4.png)
 
6. Geben Sie das gewünschte Aktivierungswort ein, und klicken Sie anschließend auf **Absenden**.

    ![Eingabe Ihres Aktivierungsworts](media/speech-devices-sdk/wake-word-5.png)
 
7. Es kann einige Minuten dauern, bis die Dateien generiert werden. Auf der Registerkarte Ihres Browsers müssten Sie einen sich drehenden Kreis sehen. Danach erscheint eine Informationsleiste, in der Sie gefragt werden, ob Sie eine `.zip`-Datei herunterladen möchten.

    ![Empfangen einer ZIP-Datei](media/speech-devices-sdk/wake-word-6.png)

8. Speichern Sie die `.zip`-Datei auf Ihrem Computer. Sie benötigen diese Datei für die Bereitstellung des benutzerdefinierten Aktivierungsworts im Development Kit, entsprechend den Anweisungen unter [Erste Schritte mit dem SDK für sprachaktivierte Geräte](speech-devices-sdk-qsg.md).

9. Sie können sich jetzt **abmelden**.

## <a name="next-steps"></a>Nächste Schritte

Richten Sie zunächst ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) ein, und registrieren Sie sich für das Speech-Geräte-SDK.

> [!div class="nextstepaction"]
> [Registrieren für das Speech-Geräte-SDK](get-speech-devices-sdk.md)

