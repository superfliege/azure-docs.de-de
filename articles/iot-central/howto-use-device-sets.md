---
title: Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Operator Gerätegruppen in der Azure IoT Central-Anwendung verwenden.
services: iot-central
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 932c8315b5af22c3adf18de50cf03deaf6b2a53e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34201148"
---
# <a name="use-device-sets-in-your-azure-iot-central-application"></a>Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung

In diesem Artikel wird beschrieben, wie Sie als Operator Gerätegruppen in Ihrer Microsoft Azure IoT Central-Anwendung verwenden.

Eine Gerätegruppe ist eine Liste der Geräte, die zusammen gruppiert werden, da sie alle einigen der angegebenen Kriterien entsprechen. Gerätegruppen helfen Ihnen bei der Verwaltung, Visualisierung und Analyse aller im Umfang enthaltenen Geräte, indem sie Geräte in kleinere, logische Gruppen zusammenfassen. Beispielsweise erstellen Sie eine Liste aller Klimageräte in Seattle, damit der Techniker in Seattle alle Geräte finden kann, für die er verantwortlich ist. In diesem Artikel erfahren Sie, wie Sie Gerätegruppen erstellen und konfigurieren.

## <a name="create-a-device-set"></a>Erstellen einer Gerätegruppe

So erstellen Sie eine Gerätegruppe

1. Wählen Sie links im Navigationsmenü die Option **Gerätegruppen** aus.

1. Klicken Sie auf **+ NEU**.

    ![Neue Gerätegruppe](media/howto-use-device-sets/image1.png)

1. Geben Sie Ihrer Gerätegruppe einen Namen, der in der gesamten Anwendung eindeutig ist. Sie können auch eine Beschreibung hinzufügen. Eine Gerätegruppe kann nur Geräte aus einer einzigen Gerätevorlage enthalten. Wählen Sie die Gerätevorlage, die für diese Gruppe verwendet werden soll.

1. Erstellen Sie die Abfrage, um die Geräte für die Gerätegruppe zu identifizieren, indem Sie eine Eigenschaft, einen Vergleichsoperator und einen Wert auswählen. Sie können mehrere Abfragen hinzufügen und Geräte, die **alle** Kriterien erfüllen, werden in die Gerätegruppe aufgenommen. Die von Ihnen erstellte Gerätegruppe ist für jeden zugänglich, der Zugriff auf die Anwendung hat, sodass jeder die Gerätegruppe anzeigen, ändern oder löschen kann.

    ![Abfrage der Gerätegruppe](media/howto-use-device-sets/image2.png)

    > [!NOTE]
    > Die Gerätegruppe ist eine dynamische Abfrage. Jedes Mal, wenn Sie die Liste der Geräte anzeigen, werden möglicherweise andere Geräte in der Liste angezeigt. Die Liste hängt davon ab, welche Geräte derzeit die Kriterien der Abfrage erfüllen.

1. Wählen Sie **Speichern** aus.

## <a name="configure-the-dashboard-for-your-device-set"></a>Konfigurieren des Dashboards für Ihre Gerätegruppe

Nachdem Sie Ihre Gerätegruppe erstellt haben, können Sie deren **Dashboard** konfigurieren. Das **Dashboard** ist die Homepage, auf der Sie Bilder und Links platzieren können. Sie können auch Raster hinzufügen, die die Geräte in der Gerätegruppe auflisten.

1. Wählen Sie links im Navigationsmenü die Option **Gerätegruppen** aus.

1. Wählen Sie die Registerkarte **Dashboard**.

1. Aktivieren Sie **Entwurfsmodus**.

    ![Entwurfsmodus aktiviert](media/howto-use-device-sets/image3.png)

1. Informationen zum Hinzufügen eines Bilds finden Sie unter [Vorbereiten und Hochladen von Bildern in eine Azure IoT Central-Anwendung](howto-prepare-images.md).

1. So fügen Sie eine Kachel mit einem Link hinzu
    1. Wählen Sie im rechten Bereich **Link** aus.

        ![Auswählen von „Link“](media/howto-use-device-sets/image6.png)

    1. Geben Sie für Ihren Link einen **Titel** ein.
    1. Wählen Sie eine URL, die beim Klicken auf den Link geöffnet werden soll.
    1. Geben Sie für den Link eine Beschreibung ein, die unter dem **Titel** angezeigt wird.
    1. Wählen Sie **Speichern** aus.

        ![Speichern des Links](media/howto-use-device-sets/image7.png)

    1. Sie können die Kachel mit dem Link auf dem **Dashboard** verschieben und dessen Größe verändern.

1. Fügen Sie ein Raster hinzu. Ein Raster ist eine Tabelle von Geräten in der Gerätegruppe mit den von Ihnen gewählten Spalten.
    1. Wählen Sie im rechten Fensterbereich **Raster** aus.

        ![Auswählen von „Raster“](media/howto-use-device-sets/image8.png)

    1. Geben Sie für Ihr Raster einen **Titel** ein.
    1. Wählen Sie über die Schaltfläche „Einstellungen“ die Spalten aus, die angezeigt werden sollen. Im eingeblendeten Bereich wählen Sie nun die gewünschte Spalte und anschließend den Pfeil nach rechts, um sie auszuwählen.
    1. Wählen Sie **OK**.
    1. Wählen Sie **Speichern** aus.

        ![Speichern des Rasters](media/howto-use-device-sets/image9.png)

    1. Ziehen Sie das Raster per Drag & Drop auf das **Dashboard**.

    > [!NOTE]
    > Sie können mehrere Bilder, Links und Raster hinzufügen.

1. Deaktivieren Sie **Entwurfsmodus**.

    ![Entwurfsmodus ausschalten](media/howto-use-device-sets/image10.png)

## <a name="configure-the-list-for-your-device-set"></a>Konfigurieren der Liste für Ihre Gerätegruppe

Nachdem Sie Ihre Gerätegruppe erstellt haben, können Sie die **Liste** konfigurieren. Die **Liste** zeigt alle Geräte der Gerätegruppe in einer Tabelle mit den von Ihnen gewählten Spalten.

1. Wählen Sie links im Navigationsmenü die Option **Gerätegruppen** aus.

1. Wählen Sie die Registerkarte **Liste**.

1. Wählen Sie **Spaltenoptionen**.

    ![Spaltenoptionen](media/howto-use-device-sets/image11.png)

1. Wählen Sie die anzuzeigenden Spalten aus, indem Sie die gewünschte Spalte markieren und den Pfeil nach rechts wählen.

    ![Auswählen von Spalten](media/howto-use-device-sets/image12.png)

1. Wählen Sie **OK**.

## <a name="analytics"></a>Analytics

Die Analyse in Gerätegruppen entspricht der Hauptregisterkarte für die Analyse im linken Navigationsmenü. Mehr über Analysen erfahren Sie im Artikel [Erstellen von Analysen](howto-create-analytics.md).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Gerätegruppen in Ihrer Azure IoT Central-Anwendung verwendet werden, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Erstellen von Telemetrieregeln](howto-create-telemetry-rules.md)
