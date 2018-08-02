---
title: Erstellen und Verwalten von Telemetrieregeln in Ihre Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Azure IoT Central-Telemetrieregeln ermöglichen Ihnen, Ihre Geräte nahezu in Echtzeit zu überwachen und Aktionen, wie das Senden einer E-Mail, durch Auslösen der Regel automatisch aufzurufen.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 42516e4dd6a85e0d07d4a8e70e958b2ec6e84aad
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225199"
---
# <a name="create-a-telemetry-rule-and-set-up-an-action-in-your-azure-iot-central-application"></a>Erstellen einer Telemetrieregel und Einrichten einer Aktion in Ihrer Azure IoT Central-Anwendung

Mithilfe von Microsoft Azure IoT Central können Sie Ihre angeschlossenen Geräte remote überwachen. Azure IoT Central-Regeln ermöglichen Ihnen, Ihre Geräte nahezu in Echtzeit zu überwachen und Aktionen, wie das Senden einer E-Mail oder Auslösen von Workflows in Microsoft Flow automatisch aufzurufen, sofern die Regelbedingungen erfüllt werden. In nur wenigen Klicks können Sie die Bedingung definieren, um Ihre Gerätedaten zu überwachen und die aufzurufende Aktion zu konfigurieren. In diesem Artikel wird die Telemetrieregel im Detail beschrieben.

Azure IoT Central nutzt [Telemetriedatenmessungen](howto-set-up-template.md) zur Erfassung von Gerätedaten. Jeder Typ von Messung weist Schlüsselattribute auf, die die Messung definieren. Sie können Regeln erstellen, um jeden Typ von Gerätemessung zu überwachen und Warnungen zu generieren, wenn die Regel ausgelöst wird. Eine Telemetrieregel wird ausgelöst, wenn die ausgewählten Gerätetelemetriedaten einen angegebenen Schwellenwert überschreiten.

## <a name="create-a-telemetry-rule"></a>Erstellen einer Telemetrieregel

In diesem Abschnitt erfahren Sie, wie Sie eine Telemetrieregel erstellen. In diesem Beispiel wird ein angeschlossenes Klimagerät verwendet, das Temperatur- und Feuchtetelemetriedaten sendet. Die Regel überwacht die vom Gerät gemeldete Temperatur und sendet eine E-Mail, sobald diese 80 Grad übersteigt.

1. Navigieren Sie zur Gerätedetailseite für das Gerät, dem Sie die Regel hinzufügen möchten.

1. Wenn Sie noch keine Regeln erstellt haben, gelangen Sie auf folgenden Bildschirm:

    ![Noch keine Regeln](media/howto-create-telemetry-rules/image1.png)

1. Wählen Sie auf der Registerkarte **Regeln** die Option **+ Neue Regel**, um die Typen von Regeln anzuzeigen, die Sie erstellen können.

    ![Regeltypen](media/howto-create-telemetry-rules/image2.png)

1. Wählen Sie die Kachel **Telemetrie** aus, um das Formular zum Erstellen der Regel zu öffnen.

    ![Telemetrieregel](media/howto-create-telemetry-rules/image3.png)

1. Wählen Sie einen aussagekräftigen Namen, damit Sie die Regel in dieser Gerätevorlage einfach identifizieren können.

1. Um die Regel sofort für alle Geräte zu aktivieren, die mit dieser Vorlage erstellt wurden, schalten Sie die Option **Regel aktivieren** ein.

### <a name="configure-the-rule-condition"></a>Konfiguration der Regelbedingung

In diesem Abschnitt erfahren Sie, wie Sie eine Bedingung zum Überwachen der Temperaturtelemetriedaten hinzufügen können.

1. Wählen Sie **+** neben der Option **Bedingung**.

1. Wählen Sie in der Dropdownliste den Telemetrietyp **Temperatur** aus. Anschließend wählen Sie den Operator aus, und geben Sie einen Schwellenwert an. Sie können mehrere Telemetriebedingungen hinzufügen. Wenn mehrere Bedingungen angegeben werden, müssen alle Bedingungen erfüllt sein, damit die Regel ausgelöst wird.

    ![Hinzufügen einer Telemetriebedingung](media/howto-create-telemetry-rules/image4.png)

    > [!NOTE]
    > Wählen Sie mindestens eine Telemetriedatenmessung aus, wenn Sie eine Telemetrieregelbedingung definieren.

1. Klicken Sie auf **Speichern**, um Ihre Regel zu speichern. Die Regel geht innerhalb weniger Minuten live und beginnt mit der Überwachung der Telemetriedaten, die an Ihre Anwendung gesendet werden.

### <a name="add-an-action"></a>Hinzufügen einer Aktion

In diesem Beispiel erfahren Sie, wie Sie eine Aktion zu einer Regel hinzufügen. Dabei wird veranschaulicht, wie die E-Mail-Aktion hinzugefügt wird, Sie können jedoch auch andere Aktionen hinzufügen:
-  [Microsoft Flow-Aktion](howto-add-microsoft-flow.md) zum Initiieren eines Workflows in Microsoft Flow bei Auslösung einer Regel
- [Webhookaktion](howto-create-webhooks.md) zum Benachrichtigen anderer Dienste bei Auslösung einer Regel

> [!NOTE]
> Gegenwärtig kann einer einzigen Regel nur eine Aktion zugeordnet werden.

1. Wählen Sie **+** neben der Option **Aktionen**. Hier sehen Sie die Liste der verfügbaren Aktionen.

    ![Hinzufügen einer Aktion](media/howto-create-telemetry-rules/image5.png)

1. Wählen Sie die Aktion **E-Mail**, geben Sie eine gültige E-Mail-Adresse in das Feld **An** ein, und geben Sie eine Notiz ein, die im Text der E-Mail erscheint, wenn die Regel ausgelöst wird.

    > [!NOTE]
    > E-Mails werden nur an die Benutzer gesendet, die der Anwendung hinzugefügt wurden und sich mindestens einmal angemeldet haben. Erfahren Sie mehr über die [Benutzerverwaltung](howto-administer.md) in Azure IoT Central.

   ![Konfigurieren einer Aktion](media/howto-create-telemetry-rules/image6.png)

1. Klicken Sie auf **Speichern**. Die Regel geht innerhalb weniger Minuten live und beginnt mit der Überwachung der Telemetriedaten, die an Ihre Anwendung gesendet werden. Wenn die in der Regel festgelegte Bedingung erfüllt ist, löst die Regel die konfigurierte E-Mail-Aktion aus.

## <a name="parameterize-the-rule"></a>Parametrisieren der Regel

Regeln können bestimmte Werte aus den **Geräteeigenschaften** als Parameter ableiten. Die Verwendung von Parametern ist in Szenarien hilfreich, in denen die Telemetrieschwellenwerte für verschiedene Geräte variieren. Wenn Sie die Regel erstellen, wählen Sie eine Geräteeigenschaft, die den Schwellenwert angibt, z.B. **Maximaler Idealschwellenwert**, anstatt einen absoluten Wert, z.B. 80 Grad. Wenn die Regel ausgeführt wird, passt sie die Gerätetelemetrie an den in der Geräteeigenschaft angegebenen Wert an.

Die Verwendung von Parametern ist eine effektive Möglichkeit, die Anzahl der zu verwaltenden Regeln pro Gerätevorlage zu reduzieren.

Aktionen können auch über **Geräteeigenschaft** als Parameter konfiguriert werden. Wenn eine E-Mail-Adresse als Geräteeigenschaft gespeichert ist, kann sie bei der Definition der Empfängeradresse verwendet werden.

## <a name="delete-a-rule"></a>Löschen einer Regel

Wenn Sie eine Regel nicht mehr benötigen, löschen Sie sie, indem Sie die Regel öffnen und **Löschen** wählen. Das Löschen der Regel entfernt sie aus der Gerätevorlage und allen zugehörigen Geräten.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Aktivieren oder Deaktivieren einer Regel für eine Gerätevorlage

Navigieren Sie zu dem Gerät, und wählen Sie die Regel, die Sie aktivieren oder deaktivieren möchten. Wenn Sie in der Regel die Option **Regel für alle Geräte dieser Vorlage aktivieren** umschalten, wird die Regel für alle Geräte, die der Gerätevorlage zugeordnet sind, aktiviert bzw. deaktiviert.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Aktivieren oder Deaktivieren einer Regel für ein Gerät

Navigieren Sie zu dem Gerät, und wählen Sie die Regel, die Sie aktivieren oder deaktivieren möchten. Schalten Sie die Option **Regel für dieses Gerät aktivieren** ein bzw. aus, je nachdem, ob Sie die Regel für dieses Gerät aktivieren bzw. deaktivieren möchten.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Regeln in Ihrer Azure IoT Central-Anwendung bearbeitet werden, werden als Nächstes die folgenden Schritte empfohlen:

> [!div class="nextstepaction"]
> [Eine Microsoft Flow-Aktion zu einer Regel hinzufügen](howto-add-microsoft-flow.md)
> [Geräte verwalten](howto-manage-devices.md)
