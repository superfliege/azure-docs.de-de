---
title: Konfigurieren der Homepage Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Ersteller die Homepage Ihrer Azure IoT Central-Anwendung konfigurieren.
author: dominicbetts
ms.author: dobett
ms.date: 07/10/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a03ac0ef66f4ffdce53d0bd2a35839bbe1615d0b
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54199084"
---
# <a name="configuring-homepage"></a>Konfigurieren der Homepage

Die Homepage ist die Seite, die geladen wird, wenn zugriffsberechtigte Benutzer zur URL der Anwendung navigieren. Wenn Sie beim Erstellen Ihrer Anwendung die Anwendungsvorlagen „Beispiel-Contoso“ oder „Beispiel-Entwickler-Kits“ ausgewählt haben, verfügt Ihre Anwendung über vordefinierte Homepages. Dagegen ist die Homepage bei Auswahl der Anwendungsvorlage „Benutzerdefinierte Anwendung“ leer.

In diesem Beispiel ist die Homepage für Anwendungen zu sehen, die auf der „Beispiel-Contoso“-Vorlage basieren. Um die Homepage für Ihre Anwendung anzupassen, wählen Sie zunächst oben rechts **Bearbeiten** aus. 

![Homepage für Anwendungen auf Basis der „Beispiel-Contoso“-Vorlage](media/howto-configure-homepage/image1.png)

Wenn Sie **Bearbeiten** auswählen, wird die Dashboardbibliothek links in einem Fenster geöffnet. Es gibt viele Arten von Kacheln und Dashboardprimitiven, die Sie zum Anpassen Ihrer Homepage hinzufügen können.

![Dashboardbibliothek](media/howto-configure-homepage/image2.png)

Beispielsweise können Sie eine Kachel **Einstellungen und Eigenschaften** hinzufügen, um eine Auswahl der aktuellen Werte für Einstellungen und Eigenschaften anzuzeigen. Wählen Sie dazu zuerst eine **Gerätevorlage** und dann eine **Geräteinstanz** aus. Danach geben Sie der Kachel einen Titel und wählen eine anzuzeigende **Einstellung** oder **Eigenschaft** aus. In diesem Fall wurde **Temperatur festlegen** ausgewählt. Wenn Sie auf **Speichern** klicken, wird diese Kachel auf der Homepage angezeigt.

![Formular „Gerätedetails konfigurieren“ mit Details zu Einstellungen und Eigenschaften](media/howto-configure-homepage/image3.png)

Wenn ein Operator nun die Homepage aufruft, wird ihm diese Kachel mit den Eigenschaften bzw. Einstellungen des Geräts angezeigt:

![Registerkarte „Dashboard“ mit Anzeige der Einstellungen und Eigenschaften für die Kachel](media/howto-configure-homepage/image4.png)

Probieren Sie die verschiedenen anderen Kacheltypen in der Bibliothek einfach aus, um herauszufinden, wie Sie die Homepage Ihrer Anwendung noch besser anpassen können.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun gelernt haben, wie Sie Ihre Azure IoT Central-Homepage konfigurieren:

> [!div class="nextstepaction"]
> [Erfahren Sie, wie Sie Bilder vorbereiten und hochladen.](howto-prepare-images.md)
