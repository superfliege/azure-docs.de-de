---
title: Konfigurieren der Homepage Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Ersteller die Homepage Ihrer Azure IoT Central-Anwendung konfigurieren.
author: tbhagwat3
ms.author: tanmayb
ms.date: 07/10/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 93f406a1d5e4a8c2ce5ad1db0c3936dd3ad2bfb9
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992356"
---
## <a name="configuring-homepage"></a>Konfigurieren der Homepage

Die Homepage ist die Seite, die geladen wird, wenn zugriffsberechtigte Benutzer zur URL der Anwendung navigieren. Wenn Sie beim Erstellen Ihrer Anwendung die Anwendungsvorlagen „Beispiel-Contoso“ oder „Beispiel-Entwickler-Kits“ ausgewählt haben, verfügt Ihre Anwendung über vordefinierte Homepages. Dagegen ist die Homepage bei Auswahl der Anwendungsvorlage „Benutzerdefinierte Anwendung“ leer.

In diesem Beispiel ist die Homepage für Anwendungen zu sehen, die auf der „Beispiel-Contoso“-Vorlage basieren. Um die Homepage für Ihre Anwendung anzupassen, schalten Sie zunächst oben rechts den **Entwurfsmodus** ein. 

![Homepage für Anwendungen auf Basis der „Beispiel-Contoso“-Vorlage](media\howto-configure-homepage\image1.png)

Durch Aktivieren des **Entwurfsmodus** wird die Dashboardbibliothek links in einem Fenster geöffnet. Es gibt viele Arten von Kacheln und Dashboardprimitiven, die Sie zum Anpassen Ihrer Homepage hinzufügen können.

![Dashboardbibliothek](media\howto-configure-homepage\image2.png)

Beispielsweise können Sie eine Kachel **Einstellungen und Eigenschaften** hinzufügen, um eine Auswahl der aktuellen Werte für Einstellungen und Eigenschaften anzuzeigen. Wählen Sie dazu zuerst eine **Gerätevorlage** und dann eine **Geräteinstanz** aus. Danach geben Sie der Kachel einen Titel und wählen eine anzuzeigende **Einstellung** oder **Eigenschaft** aus. In diesem Fall wurde **Lüfterdrehzahl** ausgewählt. Wenn Sie auf **Speichern** klicken, wird diese Kachel auf der Homepage angezeigt.

![Formular „Gerätedetails konfigurieren“ mit Details zu Einstellungen und Eigenschaften](media\howto-configure-homepage\image3.png)

Wenn ein Operator nun die Homepage aufruft, wird ihm diese Kachel mit den Eigenschaften bzw. Einstellungen des Geräts angezeigt:

![Registerkarte „Dashboard“ mit Anzeige der Einstellungen und Eigenschaften für die Kachel](media\howto-configure-homepage\image4.png)

Probieren Sie die verschiedenen anderen Kacheltypen in der Bibliothek einfach aus, um herauszufinden, wie Sie die Homepage Ihrer Anwendung noch besser anpassen können.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun gelernt haben, wie Sie Ihre Azure IoT Central-Homepage konfigurieren:

> [!div class="nextstepaction"]
> [Erfahren Sie, wie Sie Bilder vorbereiten und hochladen.](howto-prepare-images.md)
