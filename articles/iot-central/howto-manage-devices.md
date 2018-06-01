---
title: Verwalten der Geräte in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Operator Gerätegruppen in der Azure IoT Central-Anwendung verwalten.
services: iot-central
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 75472d701160e7cfd331d01efcdc1a19ae20fb2d
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
ms.locfileid: "34303578"
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Verwalten von Geräten in Ihrer Azure IoT Central-Anwendung

In diesem Artikel wird beschrieben, wie Sie als Operator Gerätegruppen in Ihrer Microsoft Azure IoT Central-Anwendung verwalten. Als Operator können Sie folgende Aktionen ausführen:

- Mit der **Explorer**-Seite Geräte anzeigen, hinzufügen und löschen, die mit Ihrer Azure IoT Central-Anwendung verbunden sind.
- Verwalten Sie ein aktuelles Inventar Ihrer Geräte.
- Halten Sie Ihre Gerätemetadaten durch Ändern der in den Geräteeigenschaften gespeicherten Werte auf dem neuesten Stand.
- Steuern Sie das Verhalten Ihrer Geräte, indem Sie eine Einstellung auf einem bestimmten Gerät auf der Seite **Einstellungen** aktualisieren.

## <a name="view-your-devices"></a>Anzeigen von Geräten

So zeigen Sie ein einzelnes Gerät an:

1. Wählen Sie im Navigationsmenü auf der linken Seite die Option **Explorer** aus. Hier sehen Sie eine Liste Ihrer [Gerätevorlagen](howto-set-up-template.md).

1. Wählen Sie eine **Gerätevorlage** im linken Bereich aus.

1. Im rechten Bereich sehen Sie eine Liste von Geräten, die anhand dieser Gerätevorlage erstellt wurden. Wählen Sie ein einzelnes Gerät aus, um die Seite **Gerätedetails** für dieses Gerät anzuzeigen:

    [![Seite „Gerätedetails“](./media/howto-manage-devices/image1.png)](./media/howto-manage-devices/image1.png#lightbox)

## <a name="add-a-device"></a>Hinzufügen eines Geräts

So fügen Sie ein Gerät Ihrer Azure IoT Central-Anwendung hinzu:

1. Wählen Sie im Navigationsmenü auf der linken Seite die Option **Explorer** aus.

1. Wählen Sie die Gerätevorlage aus, von der Sie ein Gerät erstellen möchten.

1. Wählen Sie **+ Neu** aus.

1. Wählen Sie **Real** oder **Simuliert** aus. Ein reales Gerät steht für ein physisches Gerät, dessen Verbindung mit der Azure IoT Central-Anwendung Sie herstellen. Ein simuliertes Gerät enthält Beispieldaten, die von Azure IoT Central für Sie generiert werden. In diesem Beispiel wird ein reales Gerät verwendet. Wählen Sie **Real**, um zur Seite **Gerätedetails** für das neue Gerät zu navigieren.


## <a name="bulk-import-devices"></a>Massenimportieren von Geräten

Um eine Verbindung einer großen Anzahl von Geräten mit Ihrer Anwendung herzustellen, ermöglicht Azure IoT Central das Massenimportieren von Geräten über eine CSV-Datei. 

Anforderungen an die CSV-Datei:
1. Die CSV-Datei sollte nur eine Spalte mit den Geräte-IDs aufweisen.

1. Die Datei sollte keine Kopfzeile enthalten.


So führen Sie die Massenregistrierung von Geräten in Ihrer Anwendung durch:

1. Wählen Sie im Navigationsmenü auf der linken Seite die Option **Explorer** aus.

1. Wählen Sie im linken Bereich die Gerätevorlage aus, für die Sie das Massenerstellen der Geräte ausführen möchten.

1. Wählen Sie **Neu** und dann **Massenimport** aus.

    [![Massenimportaktion](./media/howto-manage-devices/BulkImport1.png)](./media/howto-manage-devices/BulkImport1.png#lightbox)

1. Wählen Sie die CSV-Datei aus, die die Liste der zu importierenden Geräte-IDs enthält.

1. Der Geräteimport beginnt, sobald die Datei hochgeladen wurde. Sie können den Status des Importvorgangs am Kopf des Geräterasters verfolgen.

1. Nach Abschluss des Imports wird eine Erfolgsmeldung auf dem Geräteraster angezeigt.

    [![Massenimport-Erfolgsmeldung](./media/howto-manage-devices/BulkImport3.png)](./media/howto-manage-devices/BulkImport3.png#lightbox)

Wenn beim Geräteimport ein Fehler auftritt, wird auf dem Geräteraster eine Fehlermeldung angezeigt. Eine Protokolldatei, die alle Fehler erfasst, wird generiert und kann durch Klicken auf die Fehlermeldung heruntergeladen werden.



## <a name="delete-a-device"></a>Gerät löschen

So löschen Sie entweder ein reales oder simuliertes Gerät aus Ihrer Azure IoT Central-Anwendung:

1. Wählen Sie im Navigationsmenü die Option **Explorer** aus.

1. Wählen Sie die Gerätevorlage des Geräts aus, das Sie löschen möchten.

1. Aktivieren Sie das Kontrollkästchen neben dem zu löschenden Gerät.

1. Wählen Sie **Löschen** aus.

## <a name="change-a-device-setting"></a>Ändern einer Geräteeinstellung

Einstellungen steuern das Verhalten eines Geräts. D.h., damit können Sie Eingaben für Ihr Gerät bereitstellen. Sie können Geräteeinstellungen auf der Seite **Gerätedetails** anzeigen und aktualisieren.

1. Wählen Sie im Navigationsmenü die Option **Explorer** aus.

1. Wählen Sie die Gerätevorlage des Geräts aus, dessen Einstellungen Sie ändern möchten.

1. Wählen Sie die Registerkarte **Einstellungen** aus. Hier sehen Sie alle Einstellungen Ihres Geräts und ihre aktuellen Werte. Für jede Einstellung können Sie sehen, ob das Gerät noch synchronisiert wird.

1. Ändern Sie die Einstellungen in Ihre gewünschten Werte. Sie können mehrere Einstellungen gleichzeitig ändern und aktualisieren.

1. Wählen Sie **Aktualisieren** aus. Die Werte werden an Ihr Gerät gesendet. Wenn das Gerät die Einstellungsänderung bestätigt, ändert sich der Status der Einstellung wieder in **Synchronisiert**.

## <a name="change-a-property"></a>Ändern einer Eigenschaft

Eigenschaften sind die dem Gerät zugeordneten Gerätemetadaten, z.B. der Ort und die Seriennummer. Sie können Eigenschaften auf der Seite **Gerätedetails** anzeigen und aktualisieren.

1. Wählen Sie im Navigationsmenü die Option **Explorer** aus.

1. Wählen Sie die Gerätevorlage des Geräts aus, dessen Eigenschaften Sie ändern möchten.

1. Wählen Sie die Registerkarte **Eigenschaften** aus, auf der alle Eigenschaften angezeigt werden.

1. Ändern Sie die Eigenschaften in Ihre gewünschten Werte. Sie können mehrere Eigenschaften auf einmal ändern und alle gleichzeitig aktualisieren.

1. Wählen Sie **Aktualisieren** aus.

> [!NOTE]
> Sie können den Wert der _Geräteeigenschaften_ nicht ändern. Geräteeigenschaften werden vom Gerät festgelegt und sind in der Azure IoT Central-Anwendung schreibgeschützt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie in Ihrer Azure IoT Central-Anwendung Geräte verwaltet werden, sollte dies der nächste Schritt sein:

> [!div class="nextstepaction"]
> [Verwenden von Gerätegruppen](howto-use-device-sets.md)

<!-- Next how-tos in the sequence -->
