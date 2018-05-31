---
title: Hochladen von Bildern in eine Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Ersteller Bilder vorbereiten und in Ihrer Azure IoT Central-Anwendung hochladen.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: a43f2dd780604235ada1d8e3ae8a20563042fbaa
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34200230"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Vorbereiten und Hochladen von Bildern in eine Azure IoT Central-Anwendung

In diesem Artikel wird beschrieben, wie Sie als Ersteller Ihre Microsoft Azure IoT Central-Anwendung durch Hochladen benutzerdefinierter Bilder anpassen können. Beispielsweise können Sie ein Gerätedashboard durch ein Bild des Geräts benutzerdefiniert anpassen.

## <a name="before-you-begin"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

1. Eine Azure IoT Central-Anwendung. Weitere Informationen finden Sie unter [Erstellen Ihrer Azure IoT Central-Anwendung](howto-create-application.md).
1. Ein Tool zum Ändern der Größe von Bilddateien.

## <a name="choose-where-to-use-custom-images"></a>Auswählen der Anzeigepositon benutzerdefinierter Bilder

An folgenden Stellen in einer Azure IoT Central-Anwendung können Sie benutzerdefinierte Bilder verwenden:

* Auf der Seite **Anwendungs-Manager**

    ![Bild auf der Seite „Anwendungs-Manager“](media/howto-prepare-images/applicationmanager.png)

* Auf der Startseite

    ![Bild auf der Startseite](media/howto-prepare-images/homepage.png)

* In einer Gerätevorlage

    ![Bild in einer Gerätevorlage](media/howto-prepare-images/devicetemplate.png)

* Als Kachel auf einem Gerätedashboard

    ![Bild als Gerätekachel](media/howto-prepare-images/devicetile.png)

* Als Kachel auf dem Dashboard einer Gerätegruppe

    ![Bild als Kachel einer Gerätegruppe](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>Vorbereiten der Bilder

An allen vier Stellen können Sie entweder PNG-, GIF- oder JPEG-Bilder verwenden.

Die Bildgrößen, die Sie verwenden können, werden in der folgenden Tabelle angegeben:

| Speicherort | Größen |
| -------- | ------ |
| **Anwendungs-Manager** | 268 x 160 px |
| Gerätevorlage | 64 x 64 px |
| Startseite und Dashboardkacheln | Die kleinste Kachelgröße beträgt 200 x 200 px. Größere Kacheln können aus quadratischen oder rechteckigen kleinen Kacheln bestehen. Beispiel: 200 x 400 px, 400 x 200 px oder 400 x 400 px. |

Zur optimalen Darstellung in der Anwendung sollten Sie Bilder erstellen, die die in der obigen Tabelle aufgeführten Maße aufweisen.

## <a name="upload-the-images"></a>Hochladen der Bilder

In den folgenden Abschnitten wird beschrieben, wie die Bilder für die Verwendung an verschiedenen Positionen hochgeladen werden:

### <a name="application-manager"></a>Anwendungs-Manager

Um ein Bild für die Verwendung im **Anwendungs-Manager** hochzuladen, navigieren Sie im Abschnitt **Verwaltung** zur Seite **Anwendungseinstellungen**. Sie müssen ein Administrator sein, um diese Aufgabe durchzuführen:

![Hochladen eines Anwendungsbilds](media/howto-prepare-images/uploadapplicationmanager.png)

Klicken Sie auf die Schaltfläche zum Hochladen des Bilds, und wählen Sie dann die hochzuladende Datei auf Ihrem lokalen Computer aus.

### <a name="home-page"></a>Startseite

Um ein Bild für die Verwendung auf der Startseite hochzuladen, navigieren Sie zur **Startseite** Ihrer Anwendung und aktivieren den Entwurfsmodus. Sie müssen ein Ersteller sein, um diese Aufgabe durchzuführen:

![Hochladen eines Bilds für die Startseite](media/howto-prepare-images/uploadhomepage.png)

Klicken Sie auf die Schaltfläche zum Hochladen des Bilds, und wählen Sie dann die hochzuladende Datei auf Ihrem lokalen Computer aus.

Nachdem das Bild hochgeladen wurde, können Sie bei aktiviertem Entwurfsmodus dessen Größe ändern.

### <a name="device-template"></a>Gerätevorlage

Um ein Bild für die Verwendung in einer Gerätevorlage hochzuladen, navigieren Sie zum **Device Explorer**, wählen die Gerätevorlage und ein Gerät aus und aktivieren dann den Entwurfsmodus. Sie müssen ein Ersteller sein, um diese Aufgabe durchzuführen:

![Hochladen eines Bilds für die Gerätevorlage](media/howto-prepare-images/uploaddevicetemplate.png)

Klicken Sie auf die Schaltfläche zum Hochladen des Bilds, und wählen Sie dann die hochzuladende Datei auf Ihrem lokalen Computer aus.

### <a name="device-dashboard"></a>Gerätedashboard

Um ein Bild für die Verwendung auf einem Gerätedashboard hochzuladen, navigieren Sie zum **Device Explorer** und wählen die Gerätevorlage und ein Gerät aus. Wählen Sie dann die Seite **Dashboard**, und aktivieren Sie den Entwurfsmodus. Sie müssen ein Ersteller sein, um diese Aufgabe durchzuführen:

![Hochladen eines Bilds für das Gerätedashboard](media/howto-prepare-images/uploaddevicedashboard.png)

Klicken Sie auf die Schaltfläche zum Hochladen des Bilds, und wählen Sie dann die hochzuladende Datei auf Ihrem lokalen Computer aus.

Nachdem das Bild hochgeladen wurde, können Sie bei aktiviertem **Entwurfsmodus** dessen Größe und Position ändern.

### <a name="device-set-dashboard"></a>Dashboard einer Gerätegruppe

Um ein Bild für die Verwendung auf dem Dashboard einer Gerätegruppe hochzuladen, navigieren Sie zu **Gerätegruppen** und wählen die Gerätegruppe und ein Gerät aus. Wählen Sie dann die Seite **Dashboard**, und aktivieren Sie den **Entwurfsmodus**:

![Hochladen eines Bilds für das Dashboard einer Gerätegruppe](media/howto-prepare-images/uploaddevicesetdashboard.png)

Klicken Sie auf die Schaltfläche zum Hochladen des Bilds, und wählen Sie dann die hochzuladende Datei auf Ihrem lokalen Computer aus.

Nachdem das Bild hochgeladen wurde, können Sie bei aktiviertem Entwurfsmodus dessen Größe und Position ändern.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Bilder in Ihrer Azure IoT Central-Anwendung vorbereitet und hochgeladen werden, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten von Geräten in Ihrer Azure IoT Central-Anwendung](howto-manage-devices.md)