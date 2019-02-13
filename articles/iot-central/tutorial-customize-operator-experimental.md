---
title: Anpassen der Ansichten für Bediener in Azure IoT Central | Microsoft-Dokumentation
description: Als Ersteller können Sie die Ansichten für Bediener in Ihrer Azure IoT Central-Anwendung anpassen.
author: sandeeppujar
ms.author: sandeepu
ms.date: 01/29/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 267a619fe32a8d4af0ee9cc8a5001d7a321c3098
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55765148"
---
# <a name="tutorial-customize-the-azure-iot-central-operators-view-new-ui-design"></a>Tutorial: Anpassen der Azure IoT Central-Ansicht für Bediener (neues Benutzeroberflächendesign)

Dieses Tutorial richtet sich an Ersteller und erläutert, wie Sie die Darstellung Ihrer Anwendung für Bediener anpassen. Wenn Sie als Ersteller eine Änderung an der Anwendung vornehmen, können Sie in der Microsoft Azure IoT Central-Anwendung eine Vorschau der Ansicht für Bediener anzeigen.

In diesem Tutorial passen Sie die Anwendung an, damit einem Bediener relevante Informationen zur verbundenen Klimaanlage angezeigt werden. Ihre Anpassungen ermöglichen dem Bediener die Verwaltung der mit der Anwendung verbundenen Klimaanlagen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren Ihres Gerätedashboards
> * Konfigurieren des Layouts Ihrer Geräteeinstellungen
> * Konfigurieren des Layouts Ihrer Geräteeigenschaften
> * Anzeigen einer Vorschau des Geräts als Bediener
> * Konfigurieren Ihrer Standardstartseite
> * Anzeigen einer Vorschau der Standardstartseite als Bediener

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird vorausgesetzt, dass Sie bereits die beiden vorherigen Tutorials absolviert haben:

* [Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)

## <a name="configure-your-device-dashboard"></a>Konfigurieren Ihres Gerätedashboards

Als Ersteller können Sie definieren, welche Informationen auf einem Gerätedashboard angezeigt werden sollen. Unter [Tutorial: Define a new device type in your Azure IoT Central application (New UI design)](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) (Tutorial: Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung (neues Benutzeroberflächendesign)) haben Sie dem Dashboard für die verbundene Klimaanlage (**Connected Air Conditioner**) ein Liniendiagramm und andere Informationen hinzugefügt.

1. Klicken Sie zum Bearbeiten der Gerätevorlage **Connected Air Conditioner** (Verbundene Klimaanlage) im linken Navigationsmenü auf **Gerätevorlagen**:

    ![Seite „Gerätevorlagen“](media/tutorial-customize-operator-experimental/devicetemplates.png)

2. Klicken Sie zum Anpassen des Gerätedashboards auf das Gerätevorlagengerät **Connected Air Conditioner (1.0.0)** (Verbundene Klimaanlage (1.0.0)), das Sie unter [Tutorial: Define a new device type in your Azure IoT Central application (New UI design)](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) (Tutorial: Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung (neues Benutzeroberflächendesign)) erstellt haben.

3. Wählen Sie zum Bearbeiten des Dashboards die Registerkarte **Dashboard** aus.

4. Klicken Sie auf **KPI**, um dem Dashboard eine KPI-Kachel (Key Performance Indicator) hinzuzufügen:

    Definieren Sie den KPI mit den Informationen aus der folgenden Tabelle:

    | Einstellung     | Wert |
    | ----------- | ----- |
    | NAME        | Maximum temperature |
    | Zeitbereich  | Past 1 week (Letzte Woche) |
    | Messungstyp | Telemetrie |
    | Messung | Temperatur |
    | Aggregation | Maximum |
    | Sichtbarkeit  | Aktiviert |

    ![Hinzufügen eines KPIs](media/tutorial-customize-operator-experimental/addkpi.png)

5. Klicken Sie auf **Speichern**. Auf dem Dashboard wird nun die KPI-Kachel angezeigt:

    ![KPI-Kachel](media/tutorial-customize-operator-experimental/temperaturekpi.png)

6. Wenn Sie eine Kachel verschieben oder ihre Größe ändern möchten, zeigen Sie mit dem Mauszeiger auf die Kachel. Sie können die Kachel an eine andere Position ziehen oder ihre Größe ändern.

## <a name="configure-your-settings-layout"></a>Konfigurieren des Layouts Ihrer Einstellungen

Als Ersteller können Sie auch die Darstellung der Geräteeinstellungen für Bediener konfigurieren. Bediener verwenden die Registerkarte mit den Geräteeinstellungen, um ein Gerät zu konfigurieren. So kann ein Bediener auf der Einstellungsregisterkarte beispielsweise die Zieltemperatur für die verbundene Klimaanlage festlegen.

1. Wählen Sie **Einstellungen**, um das Layout der Einstellungen für Ihre verbundene Klimaanlage zu bearbeiten.

2. Sie können die Einstellungskacheln verschieben und ihre Größe ändern:

    ![Bearbeiten des Layouts der Einstellungen](media/tutorial-customize-operator-experimental/settingslayout.png)

## <a name="configure-your-properties-layout"></a>Konfigurieren des Layouts Ihrer Eigenschaften

Neben dem Dashboard und den Einstellungen können Sie auch die Darstellung der Geräteeigenschaften für Bediener konfigurieren. Bediener verwenden die Registerkarte mit den Geräteeigenschaften zur Verwaltung von Gerätemetadaten. So kann ein Bediener über die Eigenschaftenregisterkarte beispielsweise die Seriennummer eines Geräts anzeigen oder die Kontaktdetails für den Hersteller aktualisieren.

1. Wählen Sie die Registerkarte **Eigenschaften**, um das Layout der Eigenschaften für Ihre verbundene Klimaanlage zu bearbeiten.

2. Sie können die Eigenschaftenfelder verschieben und ihre Größe ändern:

    ![Bearbeiten des Layouts der Eigenschaften](media/tutorial-customize-operator-experimental/propertieslayout.png)

## <a name="preview-the-connected-air-conditioner-device-as-an-operator"></a>Anzeigen einer Vorschau der verbundenen Klimaanlage als Bediener

Auf der Seite **Gerätevorlagen** können Sie die Registerkarten „Dashboard“, „Einstellungen“ und „Eigenschaften“ für einen Bediener anpassen. Auf der Seite **Device Explorer** können Sie die Gerätevorlage anzeigen und verwenden.

1. Wenn Sie die Vorlage für die verbundene Klimaanlage als Bediener anzeigen und verwenden möchten, navigieren Sie zur Seite **Device Explorer**, und wählen Sie das simulierte Gerät aus, das von IoT Central basierend auf Ihrer Vorlage erstellt wurde:

    ![Anzeigen und Verwenden der Gerätevorlage](media/tutorial-customize-operator-experimental/usetemplate.png)

2. Wählen Sie zum Aktualisieren des Standorts dieses Geräts die Option **Eigenschaften**, und bearbeiten Sie den Wert auf der Kachel für den Standort. Klicken Sie anschließend auf **Speichern**:

    ![Eingeben eines Eigenschaftswerts](media/tutorial-customize-operator-experimental/editproperty.png)

3. Klicken Sie auf **Einstellungen**, ändern Sie einen Einstellungswert in einer Kachel, und klicken Sie anschließend auf **Aktualisieren**, um eine Einstellung an Ihre verbundene Klimaanlage zu senden:

    ![Senden einer Einstellung an das Gerät](media/tutorial-customize-operator-experimental/sendsetting.png)

    Wenn das Gerät den neuen Einstellungswert bestätigt, wird die Einstellung auf der Kachel als **synchronisiert** angezeigt.

4. Als Bediener können Sie das vom Ersteller konfigurierte Gerätedashboard anzeigen:

    ![Ansicht des Gerätedashboards für Bediener](media/tutorial-customize-operator-experimental/operatordashboard.png)

## <a name="configure-the-default-home-page"></a>Konfigurieren der Standardstartseite

Wenn sich ein Ersteller oder Bediener bei einer Azure IoT Central-Anwendung anmeldet, wird eine Startseite angezeigt. Ersteller können den Inhalt dieser Startseite anpassen, sodass einem Bediener möglichst hilfreiche und relevante Inhalte angezeigt werden.

1. Navigieren Sie zum Anpassen der Standardstartseite zur**Startseite**, und wählen Sie rechts oben auf der Seite **Bearbeiten** aus. Auf der linken Seite wird ein Bereich mit einer Liste von Objekten geöffnet, die Sie zur **Startseite** hinzufügen können:

    ![Seite „Application Builder“ (Anwendungs-Generator)](media/tutorial-customize-operator-experimental/builderhome.png)

2. Fügen Sie zum Anpassen der **Startseite** Kacheln aus der **Bibliothek** hinzu. Klicken Sie auf **Link**, und fügen Sie Details zur Website Ihrer Organisation hinzu. Klicken Sie anschließend auf **Speichern**:

    ![Hinzufügen eines Links zur Startseite](media/tutorial-customize-operator-experimental/addlink.png)

    > [!NOTE]
    > Sie können auch Links zu Seiten innerhalb Ihrer Azure IoT Central-Anwendung hinzufügen. So können Sie beispielsweise einen Link zu einem Gerätedashboard oder zu einer Einstellungsseite hinzufügen.

3. Klicken Sie optional auf **Bild**, und laden Sie ein Bild hoch, das auf Ihrer Startseite angezeigt werden soll. Ein Bild kann über eine URL verfügen, zu der Sie navigieren, wenn Sie darauf klicken:

    ![Hinzufügen eines Bilds zur Startseite](media/tutorial-customize-operator-experimental/addimage.png)

    Weitere Informationen finden Sie unter [Vorbereiten und Hochladen von Bildern in eine Azure IoT Central-Anwendung](howto-prepare-images-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="preview-the-default-home-page-as-an-operator"></a>Anzeigen einer Vorschau der Standardstartseite als Bediener

Klicken Sie rechts oben auf der Seite auf **Fertig**, um als Bediener eine Vorschau der Startseite anzuzeigen:

![Umschalten des Entwurfsmodus](media/tutorial-customize-operator-experimental/operatorviewhome.png)

Sie können auf die Link- und Bildkacheln klicken, um zu den URLs zu navigieren, die Sie als Ersteller festgelegt haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die Anwendungsdarstellung für Bediener anpassen.

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Konfigurieren Ihres Gerätedashboards
> * Konfigurieren des Layouts Ihrer Geräteeinstellungen
> * Konfigurieren des Layouts Ihrer Geräteeigenschaften
> * Anzeigen einer Vorschau des Geräts als Bediener
> * Konfigurieren Ihrer Standardstartseite
> * Anzeigen einer Vorschau der Standardstartseite als Bediener

Nachdem Sie nun wissen, wie Sie die Anwendungsdarstellung für Bediener anpassen, können Sie mit folgenden Schritten fortfahren:

* [überwachen Ihrer Geräte](tutorial-monitor-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) (als Bediener)
* [Hinzufügen eines neuen Geräts zu Ihrer Anwendung](tutorial-add-device-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) (als Bediener und Geräteentwickler)
