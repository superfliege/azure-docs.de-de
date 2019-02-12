---
title: Anpassen der Ansichten für Bediener in Azure IoT Central | Microsoft-Dokumentation
description: Als Ersteller können Sie die Ansichten für Bediener in Ihrer Azure IoT Central-Anwendung anpassen.
author: sandeeppujar
ms.author: sandeepu
ms.date: 10/12/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: f65463b534988e0a721a1a5f816183f8dd8ebcaf
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2019
ms.locfileid: "55657653"
---
# <a name="tutorial-customize-the-azure-iot-central-operators-view"></a>Tutorial: Anpassen der Azure IoT Central-Ansicht für Bediener

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

* [Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung](tutorial-define-device-type.md)
* [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](tutorial-configure-rules.md)

## <a name="configure-your-device-dashboard"></a>Konfigurieren Ihres Gerätedashboards

Als Ersteller können Sie definieren, welche Informationen auf einem Gerätedashboard angezeigt werden sollen. Im Tutorial [Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung](tutorial-define-device-type.md) haben Sie dem Dashboard **Connected Air Conditioner-1** ein Liniendiagramm und andere Informationen hinzugefügt.

1. Klicken Sie zum Bearbeiten der Gerätevorlage **Connected Air Conditioner** (Verbundene Klimaanlage) im linken Navigationsmenü auf **Explorer**:

    ![Explorer-Seite](media/tutorial-customize-operator/explorer.png)

2. Klicken Sie auf die Gerätevorlage **Connected Air Conditioner (1.0.0)**, um mit der Anpassung des Dashboards für Ihre verbundene Klimaanlage zu beginnen. Wählen Sie das Gerät **Connected Air Conditioner-1** aus, das Sie im Tutorial [Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung](tutorial-define-device-type.md) erstellt haben:

    ![Auswählen der verbundenen Klimaanlage](media/tutorial-customize-operator/selectdevice.png)

    Innerhalb eines Geräts, z.B. **Connected Air Conditioner-1**, können Sie **Vorlage bearbeiten** auswählen, um eine Änderung an der zugrunde liegenden Vorlage vorzunehmen. Weitere Informationen finden Sie unter [Erstellen einer neuen Gerätevorlagenversion](howto-version-devicetemplate.md).

3. Wählen Sie zum Bearbeiten des Dashboards **Dashboard** und dann **Vorlage bearbeiten** aus:

    ![Dashboardseite der Gerätevorlage](media/tutorial-customize-operator/dashboard.png)

4. Klicken Sie auf **KPI**, um dem Dashboard eine KPI-Kachel (Key Performance Indicator) hinzuzufügen:

    ![Hinzufügen eines KPIs](media/tutorial-customize-operator/addkpi.png)

    Definieren Sie den KPI mit den Informationen aus der folgenden Tabelle:

    | Einstellung     | Wert |
    | ----------- | ----- |
    | NAME        | Maximum temperature |
    | Messung | Temperatur |
    | Aggregation | Maximum |
    | Zeitbereich  | Past 1 week (Letzte Woche) |

5. Wählen Sie **Speichern** aus. Auf dem Dashboard wird nun die KPI-Kachel angezeigt:

    ![KPI-Kachel](media/tutorial-customize-operator/temperaturekpi.png)

6. Wenn Sie eine Kachel verschieben oder ihre Größe ändern möchten, zeigen Sie mit dem Mauszeiger auf die Kachel. Sie können die Kachel an einen anderen Ort ziehen oder ihre Größe ändern:

    ![Bearbeiten des Dashboardlayouts](media/tutorial-customize-operator/dashboardlayout.png)

7. Wenn Sie alle gewünschten Änderungen vorgenommen haben, klicken Sie auf **Fertig**.

## <a name="configure-your-settings-layout"></a>Konfigurieren des Layouts Ihrer Einstellungen

Als Ersteller können Sie auch die Darstellung der Geräteeinstellungen für Bediener konfigurieren. Bediener verwenden die Seite mit den Geräteeinstellungen, um ein Gerät zu konfigurieren. So kann ein Bediener auf der Einstellungsseite beispielsweise die Zieltemperatur für die verbundene Klimaanlage festlegen.

1. Wählen Sie **Einstellungen** und dann **Vorlage bearbeiten** aus, um das Layout der Einstellungen für Ihre verbundene Klimaanlage zu bearbeiten:

    ![Seite "Einstellungen"](media/tutorial-customize-operator/settings.png)

2. Sie können die Einstellungskacheln verschieben und ihre Größe ändern:

    ![Bearbeiten des Layouts der Einstellungen](media/tutorial-customize-operator/settingslayout.png)

3. Wenn Sie alle gewünschten Änderungen vorgenommen haben, klicken Sie auf **Fertig**.

> [!NOTE]
> Im Modus **Vorlage bearbeiten** können die Werte der Einstellungen nicht bearbeitet werden.

## <a name="configure-your-properties-layout"></a>Konfigurieren des Layouts Ihrer Eigenschaften

Neben dem Dashboard und den Einstellungen können Sie auch die Darstellung der Geräteeigenschaften für Bediener konfigurieren. Bediener verwenden die Seite mit den Geräteeigenschaften zur Verwaltung von Gerätemetadaten. So kann ein Bediener über die Eigenschaftenseite beispielsweise die Seriennummer eines Geräts anzeigen oder die Kontaktdetails für den Hersteller aktualisieren.

1. Wählen Sie **Eigenschaften** und dann **Vorlage bearbeiten** aus, um das Layout der Eigenschaften für Ihre verbundene Klimaanlage zu bearbeiten:

    ![Eigenschaftenseite](media/tutorial-customize-operator/properties.png)

2. Sie können die Eigenschaftenfelder verschieben und ihre Größe ändern:

    ![Bearbeiten des Layouts der Eigenschaften](media/tutorial-customize-operator/propertieslayout.png)

3. Wenn Sie alle gewünschten Änderungen vorgenommen haben, klicken Sie auf **Fertig**.

> [!NOTE]
> Im Modus **Vorlage bearbeiten** können die Werte der Eigenschaften nicht bearbeitet werden.

## <a name="preview-the-connected-air-conditioner-device-as-an-operator"></a>Anzeigen einer Vorschau der verbundenen Klimaanlage als Bediener

Im Modus **Vorlage bearbeiten** können Sie die Seiten „Dashboard“, „Einstellungen“ und „Eigenschaften“ für einen Bediener anpassen. Wenn Sie nicht im **Vorlage bearbeiten**-Modus sind, können Sie die Anwendung als Bediener anzeigen.

1. Wenn Sie Ihre verbundene Klimaanlage als Bediener anzeigen möchten, klicken Sie auf **Fertig**, um die Bearbeitung der Vorlage zu beenden und zur Bedieneransicht des Geräts zurückzukehren.

2. Bearbeiten Sie zum Aktualisieren des Standorts dieses Geräts den Wert in der Kachel für den Standort, und klicken Sie anschließend auf **Speichern**:

    ![Eingeben eines Eigenschaftswerts](media/tutorial-customize-operator/editproperty.png)

3. Klicken Sie auf **Einstellungen**, ändern Sie einen Einstellungswert in einer Kachel, und klicken Sie anschließend auf **Aktualisieren**, um eine Einstellung an Ihre verbundene Klimaanlage zu senden:

    ![Senden einer Einstellung an das Gerät](media/tutorial-customize-operator/sendsetting.png)

    Wenn das Gerät den neuen Einstellungswert bestätigt, wird die Einstellung auf der Kachel als **synchronisiert** angezeigt.

4. Als Bediener können Sie das vom Ersteller konfigurierte Gerätedashboard anzeigen:

    ![Ansicht des Gerätedashboards für Bediener](media/tutorial-customize-operator/operatordashboard.png)

## <a name="configure-the-default-home-page"></a>Konfigurieren der Standardstartseite

Wenn sich ein Ersteller oder Bediener bei einer Azure IoT Central-Anwendung anmeldet, wird eine Startseite angezeigt. Ersteller können den Inhalt dieser Startseite anpassen, sodass einem Bediener möglichst hilfreiche und relevante Inhalte angezeigt werden.

1. Navigieren Sie zum Anpassen der Standardstartseite zur**Startseite**, und wählen Sie rechts oben auf der Seite **Bearbeiten** aus. Bei Auswahl von **Bearbeiten** wird von rechts ein Bereich mit einer Liste von Objekten eingeblendet, die Sie Ihrer Startseite hinzufügen können.

    ![Seite „Application Builder“ (Anwendungs-Generator)](media/tutorial-customize-operator/builderhome.png)

2. Fügen Sie zum Anpassen der Startseite Kacheln aus der **Bibliothek** hinzu. Klicken Sie auf **Link**, und fügen Sie Details zur Website Ihrer Organisation hinzu. Klicken Sie anschließend auf **Speichern**:

    ![Hinzufügen eines Links zur Startseite](media/tutorial-customize-operator/addlink.png)

    > [!NOTE]
    > Sie können auch Links zu Seiten innerhalb Ihrer Azure IoT Central-Anwendung hinzufügen. So können Sie beispielsweise einen Link zu einem Gerätedashboard oder zu einer Einstellungsseite hinzufügen.

3. Klicken Sie optional auf **Bild**, und laden Sie ein Bild hoch, das auf Ihrer Startseite angezeigt werden soll. Ein Bild kann über eine URL verfügen, zu der Sie navigieren, wenn Sie darauf klicken:

    ![Hinzufügen eines Bilds zur Startseite](media/tutorial-customize-operator/addimage.png)

    Weitere Informationen finden Sie unter [Vorbereiten und Hochladen von Bildern in eine Azure IoT Central-Anwendung](howto-prepare-images.md).

## <a name="preview-the-default-home-page-as-an-operator"></a>Anzeigen einer Vorschau der Standardstartseite als Bediener

Um als Bediener eine Vorschau der Startseite anzuzeigen und die Bearbeitung zu beenden, wählen Sie rechts oben auf der Seite **Fertig** aus.

![Umschalten des Entwurfsmodus](media/tutorial-customize-operator/operatorviewhome.png)

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

* [überwachen Ihrer Geräte](tutorial-monitor-devices.md) (als Bediener)
* [Hinzufügen eines neuen Geräts zu Ihrer Anwendung](tutorial-add-device.md) (als Bediener und Geräteentwickler)
