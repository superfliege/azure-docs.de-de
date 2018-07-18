---
title: Verwalten der Geräte in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Operator Gerätegruppen in der Azure IoT Central-Anwendung verwalten.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: cf803c03d266f2a400e47fc551dea62936456177
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937617"
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


## <a name="import-devices"></a>Importieren von Geräten

Um eine Verbindung einer großen Anzahl von Geräten mit Ihrer Anwendung herzustellen, ermöglicht Azure IoT Central das Massenimportieren von Geräten über eine CSV-Datei. 

Anforderungen an die CSV-Datei:
1. Die CSV-Datei sollte nur eine Spalte aufweisen, die Geräte-IDs enthält.

1. Die Datei sollte keine Kopfzeile enthalten.


So führen Sie die Massenregistrierung von Geräten in Ihrer Anwendung durch:

1. Wählen Sie im Navigationsmenü auf der linken Seite die Option **Explorer** aus.

1. Wählen Sie im linken Bereich die Gerätevorlage aus, für die Sie das Massenerstellen der Geräte ausführen möchten.

 >   [!NOTE] 
    Wenn Sie noch keine Gerätevorlage haben, können Sie Geräte unter **Nicht zugeordnete Geräte** importieren und ohne Vorlage registrieren. Nachdem die Geräte importiert wurden, können Sie sie als nächsten Schritt einer Vorlage zuordnen.

1. Klicken Sie auf **Importieren**.

    [![Importaktion](./media/howto-manage-devices/BulkImport1.png)](./media/howto-manage-devices/BulkImport1.png#lightbox)

1. Wählen Sie die CSV-Datei aus, die die Liste der zu importierenden Geräte-IDs enthält.

1. Der Geräteimport beginnt, sobald die Datei hochgeladen wurde. Sie können den Status des Importvorgangs am Kopf des Geräterasters verfolgen.

1. Nach Abschluss des Imports wird eine Erfolgsmeldung auf dem Geräteraster angezeigt.

    [![Import erfolgreich](./media/howto-manage-devices/BulkImport3.png)](./media/howto-manage-devices/BulkImport3.png#lightbox)

Wenn beim Geräteimport ein Fehler auftritt, wird auf dem Geräteraster eine Fehlermeldung angezeigt. Eine Protokolldatei, die alle Fehler erfasst, wird generiert und kann durch Klicken auf die Fehlermeldung heruntergeladen werden.


**Verknüpfen von Geräten mit einer Vorlage**

Wenn Sie Geräte registrieren, indem Sie den Import unter **Nicht zugeordnete Geräte** starten, dann werden die Geräte ohne Gerätevorlagenzuordnung erstellt. Das Gerät muss mit einer Vorlage verknüpft sein, um damit ein Zugriff auf Daten und andere Details zum Gerät möglich ist. Führen Sie die folgenden Schritte aus, um Geräte mit einer Vorlage zu verknüpfen:
1. Wählen Sie im Navigationsmenü auf der linken Seite die Option **Explorer** aus.
1. Wählen Sie im linken Bereich **Nicht zugeordnete Geräte**.
    [![Nicht zugeordnete Geräte](./media/howto-manage-devices/UnassociatedDevices1.png)](./media/howto-manage-devices/UnassociatedDevices1.png#lightbox)
1. Wählen Sie die Geräte aus, die Sie mit einer Vorlage verknüpfen möchten.
1. Klicken Sie auf die Option **Zuordnen**.
    [![Zuordnen von Geräten](./media/howto-manage-devices/UnassociatedDevices2.png)](./media/howto-manage-devices/UnassociatedDevices2.png#lightbox)
1. Wählen Sie eine verfügbare Vorlage in der Liste aus, und klicken Sie auf die Schaltfläche **Zuordnen**.
1. Die ausgewählten Geräte werden unter die entsprechende Gerätevorlage verschoben.

 >   [!NOTE] 
    Sobald ein Gerät mit einer Vorlage verknüpft wurde, kann dessen Zuordnung nicht mehr aufgehoben werden und es kann auch keiner anderen Vorlage zugeordnet werden.

## <a name="export-devices"></a>Exportieren von Geräten

Um Geräte für die Verbindung mit IoT Central bereitzustellen, benötigen Sie die Verbindungszeichenfolge des Geräts, die von IoT Central generiert wird. Sie können das Feature „Export“ verwenden, um einen Massenkopiervorgang der Verbindungszeichenfolgen und anderen Eigenschaften der Geräte aus Ihrer Anwendung vorzunehmen. Durch „Export“ wird eine CSV-Datei mit der Geräteidentität, dem Gerätenamen sowie eine primäre Verbindungszeichenfolge für alle ausgewählten Geräte erstellt.

So führen Sie den Massenexport von Geräten aus Ihrer Anwendung durch:
1. Wählen Sie im Navigationsmenü auf der linken Seite die Option **Explorer** aus.

1. Wählen Sie im linken Bereich die Gerätevorlage aus, für die Sie die Geräte exportieren möchten.

1. Wählen Sie die Geräte aus, die Sie exportieren möchten, und klicken Sie dann auf die Aktion **Exportieren**.

    [![Export](./media/howto-manage-devices/Export1.png)](./media/howto-manage-devices/Export1.png#lightbox)

1. Der Exportprozess wird gestartet, und Sie können den Status oben im Raster nachverfolgen. 

1. Sobald der Exportvorgang abgeschlossen ist, wird eine Erfolgsmeldung zusammen mit einem Link zum Download der generierten Datei angezeigt.

1. Klicken Sie auf die **Erfolgsmeldung**, um die Datei in einen lokalen Ordner auf dem Datenträger herunterzuladen.

    [![Export erfolgreich](./media/howto-manage-devices/Export2.png)](./media/howto-manage-devices/Export2.png#lightbox)

1. Die exportierte CSV-Datei verfügt über folgende Informationen:
    1. NAME
    1. Geräte-ID
    1. Primäre Verbindungszeichenfolge


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
