---
title: Hochladen von Bildern in eine Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Ersteller Bilder vorbereiten und in Ihrer Azure IoT Central-Anwendung hochladen.
author: dominicbetts
ms.author: dobett
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: b975d7dccc85973a42408d87e3c03a91aaf1c450
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55812751"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Vorbereiten und Hochladen von Bildern in eine Azure IoT Central-Anwendung

In diesem Artikel wird beschrieben, wie Sie als Ersteller Ihre Azure IoT Central-Anwendung durch Hochladen benutzerdefinierter Bilder anpassen können. Beispielsweise können Sie ein Gerätedashboard durch ein Bild des Geräts benutzerdefiniert anpassen.

## <a name="before-you-begin"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

1. Eine Azure IoT Central-Anwendung. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Anwendung](quick-deploy-iot-central-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
1. Ein Tool zum Ändern der Größe von Bilddateien.

## <a name="choose-where-to-use-custom-images"></a>Auswählen der Anzeigepositon benutzerdefinierter Bilder

An folgenden Stellen in einer Azure IoT Central-Anwendung können Sie benutzerdefinierte Bilder verwenden:

* Auf der Seite **Anwendungs-Manager**

    ![Bild auf der Seite „Anwendungs-Manager“](media/howto-prepare-images-experimental/applicationmanager.png)

* Auf der Startseite

    ![Bild auf der Startseite](media/howto-prepare-images-experimental/homepage.png)

* In einer Gerätevorlage

    ![Bild in einer Gerätevorlage](media/howto-prepare-images-experimental/devicetemplate.png)

* Als Kachel auf einem Gerätedashboard

    ![Bild als Gerätekachel](media/howto-prepare-images-experimental/devicetile.png)

* Als Kachel auf dem Dashboard einer Gerätegruppe

    ![Bild als Kachel einer Gerätegruppe](media/howto-prepare-images-experimental/devicesettile.png)

## <a name="prepare-the-images"></a>Vorbereiten der Bilder

An allen vier Stellen können Sie entweder PNG-, GIF- oder JPEG-Bilder verwenden.

Die Bildgrößen, die Sie verwenden können, werden in der folgenden Tabelle angegeben:

| Standort | Größen |
| -------- | ------ |
| Anwendungs-Manager | 268 x 160 px |
| Gerätevorlage | 64 x 64 px |
| Startseite und Dashboardkacheln | Die kleinste Kachelgröße beträgt 200 x 200 px. Größere Kacheln können aus quadratischen oder rechteckigen kleinen Kacheln bestehen. Beispiel: 200 x 400 px, 400 x 200 px oder 400 x 400 px. |

Zur optimalen Darstellung in der Anwendung sollten Sie Bilder erstellen, die die in der obigen Tabelle aufgeführten Maße aufweisen.

## <a name="upload-the-images"></a>Hochladen der Bilder

In den folgenden Abschnitten wird beschrieben, wie Bilder an verschiedenen Positionen hochgeladen werden:

### <a name="application-manager"></a>Anwendungs-Manager

Um ein Bild für die Verwendung im **Anwendungs-Manager** hochzuladen, navigieren Sie im Abschnitt **Verwaltung** zur Seite **Anwendungseinstellungen**. Sie müssen ein Administrator sein, um diese Aufgabe durchzuführen:

![Hochladen eines Anwendungsbilds](media/howto-prepare-images-experimental/uploadapplicationmanager.png)

Klicken Sie auf die Kachel des Anwendungsbilds, um das vorbereitete Bild (268 x 160 px) von Ihrem lokalen Computer hochzuladen.

### <a name="home-page"></a>Startseite

Um ein Bild auf der Startseite hochzuladen, navigieren Sie zur **Startseite** Ihrer Anwendung, und klicken Sie auf **Bearbeiten**. Sie müssen ein Ersteller sein, um diese Aufgabe durchzuführen:

![Hochladen eines Bilds für die Startseite](media/howto-prepare-images-experimental/uploadhomepage.png)

Klicken Sie unter „Bild konfigurieren“ auf die Bildkachel, um das vorbereitete Bild von Ihrem lokalen Computer hochzuladen. Die kleinste Kachelgröße beträgt 200 x 200 px. Größere Kacheln können aus quadratischen oder rechteckigen kleinen Kacheln bestehen. Beispiel: 200 x 400 px, 400 x 200 px oder 400 x 400 px.

**Speichern**  Sie das hochgeladene Bild. Im Bearbeitungsmodus können Sie die Größe ändern. Wenn Sie fertig sind, klicken Sie auf **Fertig**. 

### <a name="device-template"></a>Gerätevorlage

Um ein Bild auf einer Gerätevorlage hochzuladen, navigieren Sie zu **Gerätevorlagen**, und wählen Sie die Gerätevorlage aus. Sie müssen ein Ersteller sein, um diese Aufgabe durchzuführen:

![Hochladen eines Bilds für die Gerätevorlage](media/howto-prepare-images-experimental/uploaddevicetemplate.png)

Klicken Sie auf die Bildkachel, um das vorbereitete Bild (64 x 64 px) von Ihrem lokalen Computer hochzuladen. 

### <a name="device-dashboard"></a>Gerätedashboard

Um ein Bild auf einem Gerätedashboard hochzuladen, navigieren Sie zu **Gerätevorlagen**, und wählen Sie die Gerätevorlage aus. Wählen Sie anschließend die Registerkarte **Dashboard** aus. Sie müssen ein Ersteller sein, um diese Aufgabe durchzuführen:

![Hochladen eines Bilds für das Gerätedashboard](media/howto-prepare-images-experimental/uploaddevicedashboard.png)

Klicken Sie unter „Bild konfigurieren“ auf die Bildkachel, und wählen Sie die Datei, die von Ihrem lokalen Computer hochgeladen werden soll. Die kleinste Kachelgröße beträgt 200 x 200 px. Größere Kacheln können aus quadratischen oder rechteckigen kleinen Kacheln bestehen. Beispiel: 200 x 400 px, 400 x 200 px oder 400 x 400 px.

**Speichern**  Sie das hochgeladene Bild. Im Bearbeitungsmodus können Sie die Größe und die Position ändern. Wenn Sie fertig sind, klicken Sie auf **Fertig**.

### <a name="device-set-dashboard"></a>Dashboard einer Gerätegruppe

Um ein Bild auf dem Dashboard einer Gerätegruppe hochzuladen, navigieren Sie zu **Gerätegruppen**, und wählen die Gerätegruppe und ein Gerät aus. Wählen Sie dann die Seite **Dashboard**, und klicken Sie auf **Bearbeiten**:

![Hochladen eines Bilds für das Dashboard einer Gerätegruppe](media/howto-prepare-images-experimental/uploaddevicesetdashboard.png)

Klicken Sie unter „Bild konfigurieren“ auf die Bildkachel, um das vorbereitete Bild von Ihrem lokalen Computer hochzuladen. Die kleinste Kachelgröße beträgt 200 x 200 px. Größere Kacheln können aus quadratischen oder rechteckigen kleinen Kacheln bestehen. Beispiel: 200 x 400 px, 400 x 200 px oder 400 x 400 px.

**Speichern**  Sie das hochgeladene Bild. Im Bearbeitungsmodus können Sie die Größe und die Position ändern. Wenn Sie fertig sind, klicken Sie auf **Fertig**.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Bilder in Ihrer Azure IoT Central-Anwendung vorbereitet und hochgeladen werden, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten von Geräten in Ihrer Azure IoT Central-Anwendung](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)