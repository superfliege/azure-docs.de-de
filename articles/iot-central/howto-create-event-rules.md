---
title: Erstellen und Verwalten von Ereignisregeln in Ihre Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Azure IoT Central-Ereignisregeln ermöglichen Ihnen, Ihre Geräte nahezu in Echtzeit zu überwachen und Aktionen, wie das Senden einer E-Mail, durch Auslösen der Regel automatisch aufzurufen.
author: ankitgupta
ms.author: ankitgup
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 30223fdca9d848ddc407981bf4a3ca683a10575a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628367"
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Erstellen einer Ereignisregel und Einrichten von Benachrichtigungen in Ihrer Azure IoT Central-Anwendung

Mithilfe von Microsoft Azure IoT Central können Sie Ihre angeschlossenen Geräte remote überwachen. Azure IoT Central-Regeln ermöglichen Ihnen, Ihre Geräte nahezu in Echtzeit zu überwachen und Aktionen, wie das Senden einer E-Mail, durch Auslösen der Regel automatisch aufzurufen. In nur wenigen Klicks können Sie die Bedingung definieren, um Ihre Gerätedaten zu überwachen und die aufzurufende Aktion zu konfigurieren. Dieser Artikel beschreibt die Ereignisüberwachungsregel im Detail.

Azure IoT Central nutzt die [Ereignismessung](howto-set-up-template.md) zur Erfassung von Gerätedaten. Jeder Typ von Messung weist Schlüsselattribute auf, die die Messung definieren. Sie können Regeln erstellen, um jeden Typ von Gerätemessung zu überwachen und Warnungen zu generieren, wenn die Regel ausgelöst wird. Eine Ereignisregel wird ausgelöst, wenn das ausgewählte Geräteereignis vom Gerät gemeldet wird.

## <a name="create-an-event-rule"></a>Erstellen einer Ereignisregel

In diesem Abschnitt erfahren Sie, wie Sie eine Ereignisregel erstellen. In diesem Beispiel wird ein Kühlgerät verwendet, das den Fehler des Lüftermotors meldet. Die Regel überwacht das vom Gerät gemeldete Ereignis und sendet eine E-Mail, sobald das Ereignis gemeldet wird.

1. Navigieren Sie zur Gerätedetailseite für das Gerät, dem Sie die Regel hinzufügen möchten.

1. Wenn Sie noch keine Regeln erstellt haben, gelangen Sie auf folgenden Bildschirm:

    ![Noch keine Regeln](media\howto-create-event-rules\image1.png)

1. Wählen Sie auf der Registerkarte **Regeln** die Option **+ Neue Regel**, um die Typen von Regeln anzuzeigen, die Sie erstellen können.

    ![Regeltypen](media\howto-create-event-rules\image2.png)

1. Klicken Sie auf **Ereignis**, um das Formular zum Erstellen der Regel zu öffnen.

    ![Ereignisregel](media\howto-create-event-rules\image3.png)

1. Wählen Sie einen aussagekräftigen Namen, damit Sie die Regel in dieser Gerätevorlage einfach identifizieren können.

1. Um die Regel sofort für alle Geräte zu aktivieren, die mit dieser Vorlage erstellt wurden, schalten Sie die Option **Regel aktivieren** ein.

### <a name="configure-the-rule-condition"></a>Konfiguration der Regelbedingung

In diesem Abschnitt erfahren Sie, wie Sie eine Bedingung zum Überwachen der Messung von Fehlerereignissen am Lüftermotor hinzufügen können.

1. Wählen Sie **+** neben der Option **Bedingung**.

1. Wählen Sie in der Dropdownliste die Ereignismessung aus, die Sie überwachen möchten. In diesem Beispiel wurde das Ereignis **Fan Motor Error** (Lüftermotorfehler) ausgewählt.

1. Optional können Sie auch einen Wert angeben, wenn Sie einen bestimmten Wert des vom Gerät gemeldeten Ereignisses überwachen möchten. Meldet das Gerät beispielsweise das gleiche Ereignis mit unterschiedlichen Fehlercodes, wird durch die Angabe des Fehlercodes als Wert in der Bedingung der Regel sichergestellt, dass die Regel nur dann ausgelöst wird, wenn das Gerät diesen bestimmten Wert als Ereignisnutzlast sendet. Bleibt dieses Feld leer, wird die Regel immer dann ausgelöst, wenn das Gerät das Ereignis unabhängig vom Ereigniswert sendet.

    ![Hinzufügen einer Ereignisbedingung](media\howto-create-event-rules\image4.png)

    > [!NOTE]
    > Wenn Sie eine Ereignisregelbedingung definieren, müssen Sie mindestens eine Ereignismessung auswählen.

### <a name="configure-the-action"></a>Konfigurieren der Aktion

In diesem Abschnitt wird gezeigt, wie Sie mithilfe einer Aktion festlegen, was die Regel ausführt, wenn die Bedingung erfüllt ist.

1. Wählen Sie **+** neben der Option **Aktionen**. Hier sehen Sie die Liste der verfügbaren Aktionen. Während der öffentlichen Vorschau ist **E-Mail** die einzige unterstützte Aktion.

    ![Hinzufügen einer Aktion](media\howto-create-event-rules\image5.png)

1. Wählen Sie die Aktion **E-Mail**, geben Sie eine gültige E-Mail-Adresse in das Feld **An** ein, und geben Sie eine Notiz ein, die im Text der E-Mail erscheint, wenn die Regel ausgelöst wird.

    > [!NOTE]
    > E-Mails werden nur an die Benutzer gesendet, die der Anwendung hinzugefügt wurden und sich mindestens einmal angemeldet haben. Erfahren Sie mehr über die [Benutzerverwaltung](howto-administer.md) in Azure IoT Central.

   ![Konfigurieren einer Aktion](media\howto-create-event-rules\image6.png)

1. Um die neue Regel zu speichern, wählen Sie **Speichern** aus. Die Regel geht innerhalb weniger Minuten live und beginnt mit der Überwachung der Ereignisse, die an Ihre Anwendung gesendet werden. Wenn die in der Regel festgelegte Bedingung erfüllt ist, löst die Regel die konfigurierte E-Mail-Aktion aus.

## <a name="parameterize-the-rule"></a>Parametrisieren der Regel

Aktionen können auch über **Geräteeigenschaft** als Parameter konfiguriert werden. Wenn eine E-Mail-Adresse als Geräteeigenschaft gespeichert ist, kann sie bei der Definition der Empfängeradresse verwendet werden.

## <a name="delete-a-rule"></a>Löschen einer Regel

Wenn Sie eine Regel nicht mehr benötigen, löschen Sie sie, indem Sie die Regel öffnen und **Löschen** wählen. Das Löschen der Regel entfernt sie aus der Gerätevorlage und allen zugehörigen Geräten.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Aktivieren oder Deaktivieren einer Regel für eine Gerätevorlage

Navigieren Sie zu dem Gerät, und wählen Sie die Regel, die Sie aktivieren oder deaktivieren möchten. Wenn Sie in der Regel die Option **Regel für alle Geräte dieser Vorlage aktivieren** umschalten, wird die Regel für alle Geräte, die der Gerätevorlage zugeordnet sind, aktiviert bzw. deaktiviert.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Aktivieren oder Deaktivieren einer Regel für ein Gerät

Navigieren Sie zu dem Gerät, und wählen Sie die Regel, die Sie aktivieren oder deaktivieren möchten. Schalten Sie die Option **Regel für dieses Gerät aktivieren** ein bzw. aus, je nachdem, ob Sie die Regel für dieses Gerät aktivieren bzw. deaktivieren möchten.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Regeln in Ihrer Azure IoT Central-Anwendung erstellt werden, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten von Geräten](howto-manage-devices.md)