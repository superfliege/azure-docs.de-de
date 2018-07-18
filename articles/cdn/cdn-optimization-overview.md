---
title: Optimieren der Inhaltsübermittlung von Azure für Ihr Szenario
description: Optimieren der Übermittlung Ihrer Inhalte für bestimmte Szenarien
services: cdn
documentationcenter: ''
author: dksimpson
manager: ''
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: rli
ms.openlocfilehash: de748f7fa33b0250b1572dd5ae55cddf15003a0e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33765673"
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>Optimieren der Inhaltsübermittlung von Azure für Ihr Szenario

Wenn Sie Inhalte an eine große globale Zielgruppe übermitteln, muss die Übermittlung optimiert werden. Das Azure Content Delivery Network (CDN) kann die Übermittlung basierend auf der Art der zu übermittelnden Inhalte optimieren. Bei den Inhalten kann es sich um eine Website, einen Livestream, ein Video oder eine große Downloaddatei handeln. Beim Erstellen eines CDN-Endpunkts geben Sie in der Option **Optimiert für** ein Szenario an. Ihre Auswahl bestimmt, welche Optimierung auf die Inhalte angewendet wird, die von dem CDN-Endpunkt übermittelt werden.

Die Optimierungsoptionen basieren auf bewährten Maßnahmen zur Verbesserung der Übermittlungsleistung und zur Entlastung des Ursprungsservers. Die gewählten Szenariooptionen passen die Konfigurationen für teilweises Zwischenspeichern, Objektblockerstellung und die Wiederholungsrichtlinie für Ursprungsfehler an und beeinflussen dadurch die Leistung. 

Dieser Artikel enthält eine Übersicht über verschiedene Optimierungsfeatures sowie Informationen zu deren Verwendung. Weitere Informationen zu Features und Einschränkungen finden Sie in den entsprechenden Artikeln zum jeweiligen Optimierungstyp.

> [!NOTE]
> Die Optionen unter **Optimiert für** können sich abhängig vom gewählten Anbieter unterscheiden. CDN-Anbieter wenden Verbesserungen je nach Szenario auf unterschiedliche Weise an. 

## <a name="provider-options"></a>Anbieteroptionen

**Azure CDN Standard von Microsoft** unterstützt die folgenden Optimierungen:

* [Allgemeine Webbereitstellung](#general-web-delivery). Diese Optimierung wird auch für Medienstreaming und den Download großer Datei verwendet.


**Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon** unterstützen die folgenden Optimierungen:

* [Allgemeine Webbereitstellung](#general-web-delivery). Diese Optimierung wird auch für Medienstreaming und den Download großer Datei verwendet.

* [Beschleunigung dynamischer Websites](#dynamic-site-acceleration) 


**Azure CDN Standard von Akamai** unterstützt die folgenden Optimierungen:

* [Allgemeine Webbereitstellung](#general-web-delivery) 

* [Allgemeines Medienstreaming](#general-media-streaming)

* [Video on Demand-Medienstreaming](#video-on-demand-media-streaming)

* [Download großer Dateien](#large-file-download)

* [Beschleunigung dynamischer Websites](#dynamic-site-acceleration) 

Microsoft empfiehlt, die Leistungsunterschiede zwischen verschiedenen Anbietern zu testen, um den optimalen Anbieter für die Übermittlung zu finden.

## <a name="select-and-configure-optimization-types"></a>Auswählen und Konfigurieren von Optimierungstypen

Wählen Sie zum Erstellen eines neuen Endpunkts einen Optimierungstyp aus, der am besten zu Ihrem Szenario und zur Art der Inhalte passt, die Sie über den Endpunkt übermittelt möchten. Standardmäßig ist **Allgemeine Webbereitstellung** ausgewählt. Für vorhandene Endpunkte vom Typ **Azure Content Delivery Network von Akamai** können Sie die Optimierungsoption jederzeit aktualisieren. Die Übermittlung über Azure CDN wird durch diese Änderung nicht beeinträchtigt. 

1. Wählen Sie in einem Profil für **Azure CDN Standard von Akamai** einen Endpunkt aus.

    ![Endpunktauswahl ](./media/cdn-optimization-overview/01_Akamai.png)

2. Wählen Sie unter „EINSTELLUNGEN“ die Option **Optimierung** aus. Wählen Sie anschließend in der Dropdownliste **Optimiert für** einen Typ aus.

    ![Auswählen von Optimierung und Typ](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Optimierung für bestimmte Szenarien

Der CDN-Endpunkt kann für eines der folgenden Szenarien optimiert werden: 

### <a name="general-web-delivery"></a>Allgemeine Webbereitstellung

Allgemeine Webbereitstellung ist die am häufigsten verwendete Optimierungsoption. Sie wird für die Optimierung allgemeiner Webinhalte wie Webseiten und Webanwendungen verwendet. Die Optimierung eignet sich aber auch für Datei- und Videodownloads.

Eine typische Website enthält sowohl statische als auch dynamische Inhalte. Statische Inhalte sind unter anderem Bilder, JavaScript-Bibliotheken und Stylesheets, die zwischengespeichert und an verschiedene Benutzer übermittelt werden können. Dynamische Inhalte werden für den jeweiligen Benutzer personalisiert. Hierzu zählen beispielsweise speziell auf ein Benutzerprofil zugeschnittene Nachrichten. Dynamische Inhalte werden nicht zwischengespeichert, da es sich hierbei um benutzerspezifische Inhalte wie etwa den Inhalt eines Einkaufswagens handelt. Die allgemeine Webbereitstellung kann Ihre gesamte Website optimieren. 

> [!NOTE]
> Bei Verwendung von **Azure CDN Standard von Akamai** können Sie diese Optimierung verwenden, wenn ihre Dateien im Schnitt kleiner als 10 MB sind. Wenn Ihre Dateien im Schnitt größer als 10 MB sind, wählen Sie in der Dropdownliste **Optimiert für** die Option **Download großer Dateien** aus.

### <a name="general-media-streaming"></a>Allgemeines Medienstreaming

Wenn Sie den Endpunkt für Livestreaming und Video on Demand-Streaming verwenden möchten, empfehlen wir die Option „Allgemeines Medienstreaming“.

Medienstreaming ist ein zeitkritischer Prozess, da das verspätete Eintreffen von Paketen beim Client häufiges Puffern des Videos und damit eine Beeinträchtigung der Wiedergabe zur Folge haben kann. Die Medienstreaming-Optimierung verringert die Wartezeit bei der Übermittlung von Medieninhalten und bietet dem Benutzer reibungsloses Streaming. 

Hierbei handelt es sich um ein gängiges Szenario für Azure Media Services-Kunden. Wenn Sie Azure Media Services verwenden, erhalten Sie einen Streamingendpunkt, der für Live- und On-Demand-Streaming verwendet werden kann. Dadurch müssen Kunden keinen anderen Endpunkt verwenden, wenn sie von Livestreaming zu On-Demand-Streaming wechseln. Dieses Szenario wird von der Optimierungsoption „Allgemeines Medienstreaming“ unterstützt.

Verwenden Sie für **Azure CDN Standard von Microsoft**, **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon** den Optimierungstyp „Allgemeine Webbereitstellung“, um allgemeine Streamingmedieninhalte zu übermitteln.

Weitere Informationen zur Medienstreaming-Optimierung finden Sie unter [Medienstreaming-Optimierung über das Azure Content Delivery Network](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Video on Demand-Medienstreaming

Die Optimierung „Video on Demand-Medienstreaming“ verbessert das Streamen von Video on Demand-Inhalten. Diese Option empfiehlt sich, wenn Sie einen Endpunkt für Video on Demand-Streaming verwenden.

Verwenden Sie für **Azure CDN Standard von Microsoft**, **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon** den Optimierungstyp „Allgemeine Webbereitstellung“, um Inhalte von Video on Demand-Streamingmedien zu übermitteln.

Weitere Informationen zur Medienstreaming-Optimierung finden Sie unter [Medienstreaming-Optimierung über das Azure Content Delivery Network](cdn-media-streaming-optimization.md).

> [!NOTE]
> Verwenden Sie diesen Optimierungstyp, wenn der CDN-Endpunkt in erster Linie Video on Demand-Inhalte bereitstellt. Der Hauptunterschied zwischen dieser Optimierung und der Optimierung „Allgemeines Medienstreaming“ ist das Timeout für wiederholte Verbindungsversuche. Für Livestreamingszenarien wird ein erheblich kürzeres Timeout verwendet.

### <a name="large-file-download"></a>Download großer Dateien

Bei Verwendung von **Azure CDN Standard von Akamai** ist der Download großer Dateien für Inhalte mit einer Größe von mehr als 10 MB optimiert. Wenn Ihre Dateien im Schnitt kleiner als 10 MB sind, verwenden Sie die Optimierung „Allgemeine Webbereitstellung“. Wenn Ihre Dateien durchweg größer als 10 MB sind, ist es unter Umständen effizienter, einen separaten Endpunkt für große Dateien zu erstellen. Firmware- oder Softwareupdates sind beispielsweise in der Regel große Dateien. Für die Bereitstellung von Dateien, die größer als 1,8 GB sind, ist die Optimierung für den Download großer Dateien erforderlich.

Verwenden Sie für **Azure CDN Standard von Microsoft**, **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon** den Optimierungstyp „Allgemeine Webbereitstellung“, um Inhalte umfangreicher Dateidownloads zu übermitteln. Es gibt keine Einschränkung hinsichtlich der Dateidownloadgröße.

Weitere Informationen zur Optimierung für große Dateien finden Sie unter [Optimierung großer Dateidownloads über das Azure Content Delivery Network](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Beschleunigung dynamischer Websites

 Die Beschleunigung dynamischer Websites ist für Profile für **Azure CDN Standard von Akamai**, **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon** verfügbar. Die Verwendung dieser Optimierung ist mit zusätzlichen Kosten verbunden. Weitere Informationen finden Sie unter [Azure Content Delivery Network – Preise ](https://azure.microsoft.com/pricing/details/cdn/).

Die Beschleunigung dynamischer Websites umfasst verschiedene Maßnahmen zur Verbesserung der Wartezeit und der Leistung dynamischer Inhalte. Hierzu zählen unter anderem Routen- und Netzwerkoptimierungen sowie TCP-Optimierungen. 

Mit dieser Optimierung können Sie eine Web-App mit zahlreichen nicht zwischenspeicherbaren Antworten beschleunigen. Beispiele hierfür wären etwa Suchergebnisse, Auftragsabschlusstransaktionen oder Echtzeitdaten. Für statische Daten können Sie weiterhin die grundlegenden CDN-Cachefunktionen verwenden. 

Weitere Informationen zur Beschleunigung dynamischer Websites finden Sie unter [Beschleunigung dynamischer Websites durch Azure CDN](cdn-dynamic-site-acceleration.md).



