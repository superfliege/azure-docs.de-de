---
title: Erstellen und Ausführen von Aufträgen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Im Rahmen von Azure IoT Central-Aufträgen können Funktionen zur Verwaltung von mehreren Geräten gleichzeitig ausgeführt werden, wie das Aktualisieren einer Geräteeigenschaft oder -einstellung oder das Ausführen eines Befehls.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 02/04/2019
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: e418ec7d22622c341abd972763d78ac2f0df46d9
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55772341"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Erstellen und Ausführen von Aufträgen in Ihrer Azure IoT Central-Anwendung

Sie können Microsoft Azure IoT Central verwenden, um Ihre angeschlossenen Geräte bedarfsgerecht mithilfe von Aufträgen zu überwachen. Dank der Auftragsfunktionalität können Sie Massenaktualisierungen für Geräteeigenschaften, Einstellungen und Befehle durchführen. In diesem Artikel wird der Einstieg in die Verwendung von Aufträgen in der eigenen Anwendung beschrieben.

## <a name="create-and-run-a-job"></a>Erstellen und Ausführen eines Auftrags

In diesem Abschnitt erfahren Sie, wie Sie einen Auftrag erstellen und ausführen. Bei jedem Schritt wird anhand eines Beispiels erläutert, wie ein Auftrag für Kühlgeräte für Automaten ausgeführt wird, bei denen die Lüfterdrehzahl erhöht werden muss.

1. Navigieren Sie im Navigationsbereich zu „Aufträge“.

1. Klicken Sie auf **+ Neu**, um mit dem Erstellen eines neuen Auftrags zu beginnen.

    ![Neuen Auftrag erstellen](./media/howto-run-a-job-experimental/createnewjob.png)

1. Geben Sie einen Namen und eine Beschreibung ein, damit Sie den Auftrag, den Sie erstellen, wiederfinden zu können.

1. Wählen Sie die Gerätegruppe aus, auf die Ihr Auftrag angewendet werden soll. Nachdem Sie die Gerätegruppe ausgewählt haben, werden auf der rechten Seite die in der ausgewählten Gerätegruppe enthaltenen Geräte eingetragen. Wenn Sie eine fehlerhafte Gerätegruppe auswählen, werden keine Geräte, stattdessen jedoch eine Meldung angezeigt, dass Ihre Gerätegruppe fehlerhaft ist.

1. Wählen Sie anschließend den zu definierenden Auftragstyp (Einstellung, Eigenschaft oder Befehl) aus. Klicken Sie neben dem ausgewählten Auftragstyp auf **+**, und fügen Sie die gewünschten Vorgänge hinzu.

    ![Auftrag konfigurieren](./media/howto-run-a-job-experimental/configurejob.png)

1. Wählen Sie auf der rechten Seite die Geräte aus, für die der Auftrag ausgeführt werden soll. Wenn Sie auf das obere Kontrollkästchen klicken, werden alle Geräte in der gesamten Gerätegruppe ausgewählt. Wenn Sie auf das Kontrollkästchen neben „Name“ klicken, werden alle Geräte auf der aktuellen Seite ausgewählt.

1. Nachdem Sie Ihre gewünschten Geräte ausgewählt haben, wählen Sie **Ausführen** aus. Der Auftrag wird nun auf der Hauptseite **Aufträge** angezeigt. In dieser Ansicht können Sie Ihre derzeit ausgeführten Aufträge sowie den Verlauf aller bisher ausgeführten Aufträge sehen. Der aktuell ausgeführte Auftrag wird immer oben in der Liste angezeigt.

    ![Auftrag anzeigen](./media/howto-run-a-job-experimental/viewjob.png)

    > [!NOTE]
    > Der Verlauf von bisher ausgeführten Aufträgen wird bis zu 30 Tage lang angezeigt.

1. Um eine Übersicht über Ihren Auftrag anzuzeigen, klicken Sie in der Liste auf den Namen des Auftrags, den Sie anzeigen möchten. Diese Übersicht enthält die Auftragsdetails, Geräte und Gerätestatus.

    ![Anzeigen des Gerätestatus](./media/howto-run-a-job-experimental/viewdevicestatus.png)

### <a name="stop-a-running-job"></a>Beenden eines ausgeführten Auftrags

Wenn Sie einen derzeit ausgeführten Auftrag beenden möchten, klicken Sie auf den Namen des ausgeführten Auftrags, den Sie beenden möchten. Klicken Sie in dem Bereich auf die Schaltfläche **Beenden**. Wie Sie sehen, hat sich der Auftragsstatus geändert und zeigt nun an, dass der Auftrag beendet wurde.

   ![Auftrag beenden](./media/howto-run-a-job-experimental/stopjob.png)

### <a name="run-a-stopped-job"></a>Ausführen eines angehaltenen Auftrags

Wenn Sie einen Auftrag ausführen möchten, der zurzeit angehalten ist, klicken Sie auf den Namen des angehaltenen Auftrags, den Sie ausführen möchten. Klicken Sie im Panel auf die Schaltfläche **Beenden**. Wie Sie sehen, hat sich der Auftragsstatus geändert und zeigt nun an, dass der Auftrag jetzt ausgeführt wird.

   ![Fortgesetzter Auftrag](./media/howto-run-a-job-experimental/resumejob.png)

## <a name="view-the-job-status"></a>Anzeigen des Auftragsstatus

Nachdem ein Auftrag erstellt wurde, wird die Spalte **Status** mit der neuesten Statusmeldung des Auftrags aktualisiert. Die Statusmeldungen haben folgende Bedeutung:

| Statusmeldung       | Bedeutung der Statusmeldung                                          |
| -------------------- | ------------------------------------------------------- |
| Abgeschlossen            | Dieser Auftrag wurde auf allen Geräten ausgeführt.              |
| Fehler               | Bei diesem Auftrag ist ein Fehler aufgetreten. Er wurde auf den Geräten nicht vollständig ausgeführt.  |
| Ausstehend              | Mit der Ausführung dieses Auftrags wurde auf den Geräten noch nicht begonnen.        |
| Wird ausgeführt              | Dieser Auftrag wird derzeit auf Geräten ausgeführt.             |
| Beendet              | Dieser Auftrag wurde von einem Benutzer manuell beendet.           |

Der Statusmeldung folgt eine Übersicht über die Geräte innerhalb des Auftrags. Diese Gerätestatus haben folgende Bedeutung:

| Statusmeldung       | Bedeutung der Statusmeldung                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Succeeded            | Anzahl der Geräte, für die der Auftrag erfolgreich ausgeführt wurde.  |
| Fehler               | Anzahl der Geräte, für die der Auftrag nicht erfolgreich ausgeführt wurde.      |

### <a name="view-the-device-status"></a>Anzeigen des Gerätestatus

Klicken Sie zum Anzeigen des Status der einzelnen Geräte im Auftrag auf den Auftragsnamen. Hier werden die Details zum Auftrag sowie alle Geräte angezeigt, die in diesem Auftrag enthalten waren. Neben den einzelnen Gerätenamen werden folgende Statusmeldungen angezeigt:

| Statusmeldung       | Bedeutung der Statusmeldung                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Abgeschlossen            | Der Auftrag wurde auf diesem Gerät ausgeführt.                                     |
| Fehler               | Beim Ausführen des Auftrags auf diesem Gerät ist ein Fehler aufgetreten. In der zugehörigen Fehlermeldung werden weitere Informationen angezeigt.  |
| Ausstehend              | Der Auftrag wurde auf diesem Gerät noch nicht ausgeführt.                                  |

> [!NOTE]
> Wenn ein Gerät gelöscht wurde, können Sie es nicht auswählen und mit der Geräte-ID wird angezeigt, dass es gelöscht wurde.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Aufträge in Ihrer Azure IoT Central-Anwendung erstellt werden, können Sie die nächsten Schritte ausführen:

- [Verwenden von Gerätegruppen](howto-use-device-sets-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Verwalten von Geräten](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Verwalten der Versionen Ihrer Gerätevorlage](howto-version-devicetemplate.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
