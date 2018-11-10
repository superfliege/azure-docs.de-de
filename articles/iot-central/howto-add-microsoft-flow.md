---
title: Erstellen von Workflows mit dem IoT Central-Connector in Microsoft Flow | Microsoft-Dokumentation
description: Verwenden Sie den IoT Central-Connector in Microsoft Flow zum Auslösen von Workflows und Erstellen, Aktualisieren und Löschen von Geräten in Workflows.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: cc54a3a375fa2780e289a20b699df5d65aaf08cc
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155574"
---
# <a name="build-workflows-with-the-iot-central-connector-in-microsoft-flow"></a>Erstellen von Workflows mit dem IoT Central-Connector in Microsoft Flow

*Dieses Thema gilt für Generatoren und Administratoren.*

Verwenden Sie Microsoft Flow, um Workflows viele für Geschäftskunden unverzichtbare Anwendungen und Dienste übergreifend zu automatisieren. Mit dem IoT Central-Connector in Microsoft Flow können Sie Workflows auslösen, wenn eine Regel in IoT Central ausgelöst wird. In einem von IoT Central ausgelösten Workflow oder einer anderen Anwendung können Sie die Aktionen im IoT Central-Connector zum Erstellen eines Geräts, Aktualisieren der Eigenschaften und Einstellungen eines Geräts oder Löschen eines Geräts einsetzen. Machen Sie sich mit [diesen Microsoft Flow-Vorlagen](https://aka.ms/iotcentralflowtemplates) vertraut, die eine Verbindung von IoT Central mit anderen Diensten wie mobilen Benachrichtigungen und Microsoft Teams herstellen.

> [!NOTE] 
> Sie müssen sich mit einem persönlichen, Geschäfts-, Schul- oder Unikonto bei Microsoft Flow anmelden. [Hier](https://aka.ms/microsoftflowplans) erfahren Sie mehr über Microsoft Flow-Pläne.

## <a name="trigger-a-workflow-when-a-rule-is-triggered"></a>Auslösen eines Workflows bei Auslösen einer Regel

In diesem Abschnitt erfahren Sie, wie Sie eine mobile Benachrichtigung in der mobilen Flow-App auslösen, wenn eine Regel in IoT Central ausgelöst wird.

1. Beginnen Sie mit dem [Erstellen einer Regel in IoT Central](howto-create-telemetry-rules.md). Klicken Sie auf die **Microsoft Flow-Aktion** als neue Aktion, nachdem Sie die Regelbedingungen gespeichert haben. In Ihrem Browser sollte eine neue Registerkarte oder ein neues Fenster geöffnet werden, um Ihnen den Zugang zu Microsoft Flow zu ermöglichen.

    ![Erstellen einer neuen Microsoft Flow-Aktion](media/howto-add-microsoft-flow/createflowaction.PNG)

1. Melden Sie sich bei Microsoft Flow an. Hierzu müssen Sie nicht dasselbe Konto verwenden wie in IoT Central. Sie gelangen auf eine Übersichtsseite, auf der ein IoT Central-Connector angezeigt wird, der eine Verbindung mit einer benutzerdefinierten Aktion herstellt.

1. Melden Sie sich am IoT Central-Connector an, und klicken Sie auf **Weiter**. Sie werden an den Microsoft Flow-Designer weitergeleitet, um Ihren Workflow zu erstellen. Der Workflow verfügt über einen IoT Central-Auslöser, in den Ihre Anwendung und Regel bereits eingetragen sind.

1. Wählen Sie **+ Neuer Schritt** und **Aktion hinzufügen** aus. An diesem Punkt können Sie Ihrem Workflow eine beliebige Aktion hinzufügen. Als Beispiel senden wir eine Mobiltelefonbenachrichtigung. Suchen Sie nach **Benachrichtigung**, und wählen Sie **Benachrichtigungen – Mobiltelefonbenachrichtigung an mich senden** aus.

1. Füllen Sie in der Aktion das Feld „Text“ mit dem Inhalt der Benachrichtigung aus. Sie können *Dynamischen Inhalt* aus Ihrer IoT Central-Regel einschließen, um Ihrer Benachrichtigung wichtige Informationen wie den Namen des Geräts und den Zeitstempel zu übergeben.

    > [!NOTE]
    > Klicken Sie im Fenster für dynamischen Inhalt auf den Text „Mehr anzeigen“, um Mess- und Eigenschaftswerte abzurufen, die die Regel ausgelöst haben.

    ![Flow-Bearbeitungsaktion mit geöffnetem dynamischem Bereich](./media/howto-add-microsoft-flow/flowdynamicpane.PNG)

1. Wenn Sie die Bearbeitung Ihrer Aktion abgeschlossen haben, klicken Sie auf **Speichern**. Sie werden zur Übersichtsseite des Workflows weitergeleitet. Hier können Sie den Ausführungsverlauf anzeigen und für Kollegen freigeben.

    > [!NOTE]
    > Wenn Sie möchten, dass andere Benutzer Ihrer IoT Central-App diese Regel bearbeiten, müssen Sie sie für sie in Microsoft Flow freigeben. Fügen Sie ihre Microsoft Flow-Konten als Besitzer Ihrem Workflow hinzu.

1. Wenn Sie zu Ihrer IoT Central-App zurückkehren, sehen Sie, dass dieser Regel im Bereich „Aktionen“ eine Microsoft Flow-Aktion zugeordnet ist.

Mit dem IoT Central-Connector in Microsoft Flow können Sie immer damit beginnen, einen Workflow zu erstellen. Anschließend können Sie auswählen, mit welcher IoT Central-App und welcher Regel Sie eine Verbindung herstellen möchten.

## <a name="create-a-device-in-a-workflow"></a>Erstellen eines Geräts in einem Workflow

In diesem Abschnitt erfahren Sie, wie Sie auf einem mobilen Gerät mithilfe der mobilen Microsoft Flow-App auf Schaltflächendruck ein neues Gerät in IoT Central erstellen. Sie können mit dieser Aktion in Flow ERP-Systeme wie Dynamics in IoT Central integrieren, indem Sie ein neues Gerät erstellen, wenn an anderer Stelle ein Gerät hinzugefügt wird.

1. Erstellen Sie zunächst einen leeren Workflow in Microsoft Flow.

1. Suchen Sie nach **Flow-Schaltfläche für mobile Verwendung** als Auslöser.

1. Fügen Sie diesem Trigger die Texteingabe **Gerätename** hinzu. Ändern Sie den Beschreibungstext in **Geben Sie den Gerätenamen Ihres neuen Geräts ein**.

1. Fügen Sie eine neue Aktion hinzu. Suchen Sie nach der Aktion **Azure IoT Central – Erstellen eines Geräts**.

1. Wählen Sie in den Dropdownmenüs Ihre Anwendung und anschließend eine Gerätevorlage aus, um ein Gerät zu erstellen. Sie sehen, dass die Aktion erweitert wird, um alle Eigenschaften und Einstellungen des Geräts anzuzeigen.

1. Wählen Sie das Feld „Gerätename“ aus. Wählen Sie im dynamischen Inhaltsbereich **Gerätename** aus. Dieser Wert wird aus der Eingabe des Benutzers über die mobile App übergeben und entspricht dem Namen Ihres neuen Geräts in IoT Central. In diesem Beispiel ist das einzige erforderliche Feld der Gerätenamen, gekennzeichnet mit einem roten Sternchen. Eine andere Gerätevorlage weist möglicherweise mehrere erforderliche Felder auf, die zum Erstellen eines neuen Geräts ausgefüllt werden müssen.

    ![Dynamischer Bereich der Flow-Aktion zum Erstellen eines Geräts](./media/howto-add-microsoft-flow/flowcreatedevice.PNG)
1. (Optional) Füllen Sie andere Felder aus, die Sie zum Erstellen neuer Geräte für geeignet halten.

1. Speichern Sie abschließend Ihren Workflow.

1. Probieren Sie den Workflow in der mobilen Microsoft Flow-App aus. Wechseln Sie in der App zur Registerkarte **Schaltflächen**. Ihr Workflow „Schaltfläche -> Neues Gerät erstellen“ sollte angezeigt werden. Geben Sie den Namen des neuen Geräts ein, und beobachten Sie, wie es in IoT Central angezeigt wird!

    ![Screenshot des Erstellens eines mobilen Geräts in Flow](./media/howto-add-microsoft-flow/flowmobilescreenshot.png)

## <a name="update-a-device-in-a-workflow"></a>Aktualisieren eines Geräts in einem Workflow

In diesem Abschnitt erfahren Sie, wie Sie auf einem mobilen Gerät mithilfe der mobilen Microsoft Flow-App auf Schaltflächendruck in IoT Central Geräteeinstellungen und -eigenschaften aktualisieren.

1. Erstellen Sie zunächst einen leeren Workflow in Microsoft Flow.

1. Suchen Sie nach **Flow-Schaltfläche für mobile Verwendung** als Auslöser.

1. Fügen Sie diesem Auslöser eine Eingabe wie die Texteingabe **Speicherort** hinzu, die einer Einstellung oder Eigenschaft entspricht, die Sie ändern möchten. Ändern Sie den Beschreibungstext.

1. Fügen Sie eine neue Aktion hinzu. Suchen Sie nach der Aktion **Azure IoT Central – Aktualisieren eines Geräts**.

1. Wählen Sie aus der Dropdownliste Ihre Anwendung aus. Jetzt benötigen Sie eine ID des vorhandenen Geräts, das Sie aktualisieren möchten. Sie können die ID des IoT Central-Geräts über die Browser-URL abrufen.

    ![Geräte-ID im Device Explorer von IoT Central](./media/howto-add-microsoft-flow/iotcdeviceid.PNG)

1. Sie können den Gerätenamen aktualisieren. Um beliebige Geräteeigenschaften und -einstellungen zu aktualisieren, müssen Sie in der Dropdownliste **Gerätevorlage** die Gerätevorlage des Geräts auswählen, das Sie aktualisieren möchten. Die Aktionskachel wird erweitert, um alle Eigenschaften und Einstellungen anzuzeigen, die Sie aktualisieren können.

    ![Flow-Workflow zum Aktualisieren eines Geräts](./media/howto-add-microsoft-flow/flowupdatedevice.PNG)

1. Wählen Sie die Eigenschaften und Einstellungen aus, die Sie aktualisieren möchten. Wählen Sie im dynamischen Inhaltsbereich die entsprechende Eingabe vom Auslöser aus. In diesem Beispiel wird der Wert „Speicherort“ nach unten weitergegeben, um die Eigenschaft „Speicherort“ des Geräts zu aktualisieren.

1. Speichern Sie abschließend Ihren Workflow.

1. Probieren Sie den Workflow in der mobilen Microsoft Flow-App aus. Wechseln Sie in der App zur Registerkarte **Schaltflächen**. Ihr Workflow „Schaltfläche -> Aktualisieren eines Geräts“ sollte angezeigt werden. Nehmen Sie die Eingaben vor, und beobachten Sie, wie das Gerät in IoT Central aktualisiert wird!

## <a name="delete-a-device-in-a-workflow"></a>Löschen eines Geräts in einem Workflow

Sie können ein Gerät mit der Aktion **Azure IoT Central – Löschen eines Geräts** mithilfe seiner Geräte-ID löschen. Hier ist ein Beispielworkflow, der ein Gerät auf Schaltflächendruck in der mobilen Microsoft Flow-App löscht.

   ![Flow-Workflow zum Löschen eines Geräts](./media/howto-add-microsoft-flow/flowdeletedevice.PNG)
    
## <a name="troubleshooting"></a>Problembehandlung

Die folgenden Tipps sollen Ihnen helfen, wenn beim Erstellen einer Verbindung mit dem Azure IoT Central-Connector Probleme auftreten.

1. Persönliche Microsoft-Konten (z.B. die Domänen @hotmail.com, @live.com, @outlook.com) werden derzeit nicht unterstützt. Sie müssen ein AAD-Geschäfts-, -Schul- oder -Unikonto verwenden.

2. Sie müssen sich mindestens einmal an der IoT Central-Anwendung angemeldet haben, um den IoT Central-Connector in Microsoft Flow verwenden zu können. Andernfalls wird die Anwendung in der Dropdownliste der Anwendungen nicht angezeigt.

3. Wenn Sie bei der Verwendung eines AAD-Kontos eine Fehlermeldung erhalten, versuchen Sie, Windows PowerShell zu öffnen, und führen Sie die folgenden Cmdlets als Administrator aus.
    ``` PowerShell
    Install-Module AzureAD
    Connect-AzureAD
    New-AzureADServicePrincipal -AppId 9edfcdd9-0bc5-4bd4-b287-c3afc716aac7 -DisplayName "Azure IoT Central"
    ```
    
## <a name="next-steps"></a>Nächste Schritte
Da Sie nun gelernt haben, Microsoft Flow zum Erstellen von Workflows zu verwenden, sollten Sie im nächsten Schritt [Geräte verwalten](howto-manage-devices.md).

