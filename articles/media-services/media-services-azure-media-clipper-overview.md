---
title: Erstellen von Clips mit Azure Media Clipper | Microsoft-Dokumentation
description: "Übersicht über Azure Media Clipper, ein Tool zum Erstellen von Medienclips aus Medienobjekten"
services: media-services
keywords: Clip;Schneiden;Subclip;Codierung;Medien
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 6a5bb66db889dc1c52252773e3f129ba2c3d55f1
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="create-clips-with-azure-media-clipper"></a>Erstellen von Clips mit Azure Media Clipper
Azure Media Clipper ist eine kostenlose JavaScript-Bibliothek, mit deren Hilfe Webentwickler ihren Benutzern eine Schnittstelle zum Erstellen von Medienclips bieten können. Dieses Tool lässt sich in jede Webseite integrieren und stellt APIs zum Laden von Medienobjekten und Übermitteln von Schnittaufträgen bereit.

Azure Media Clipper ermöglicht Folgendes:
- Beschneiden der Medien vor und nach der Klappe in Livearchiven 
- Zusammenstellen von Videohighlights aus AMS-Liveveranstaltungen, Livearchiven oder FMP4-VOD-Dateien 
- Verketten von Videos aus mehreren Quellen 
- Erstellen von Übersichtsclips aus Ihren AMS-Medienobjekten 
- Schneiden von Videos mit framebasierter Genauigkeit 
- Generieren von dynamischen Manifestfiltern für vorhandene Live- und VOD-Medienobjekte mit auf Bildgruppen (Group of Pictures, GOP) basierender Genauigkeit 
- Erstellen von Codierungsaufträgen für die Medienobjekte in Ihrem Media Services-Konto

Bringen Sie sich mit Anfragen nach neuen Features, Ideen oder Feedback auf der [UserVoice-Seite für Azure Media Services](http://aka.ms/amsvoice/) ein. Wenn Sie ein spezielles Problem oder eine Frage haben oder einen Fehler finden, wenden Sie sich unter amcinfo@microsoft.com an das Media Services-Team.

## <a name="release-notes"></a>Versionshinweise
Unter den folgenden Punkten finden Sie Informationen zu verschiedenen bekannten Problemen und das Änderungsprotokoll für das neueste Release von Clipper:
- [Liste der bekannten Probleme](https://amp.azure.net/libs/amc/latest/docs/known_issues.html)
- [Änderungsprotokoll](https://amp.azure.net/libs/amc/latest/docs/changelog.html)

## <a name="browser-support"></a>Browserunterstützung
Azure Media Clipper basiert auf modernen HTML5-Technologien und unterstützt die folgenden Browser:

- Microsoft Edge 13+
- Internet Explorer 11+
- Chrome 54+
- Safari 10+
- Firefox 50+

> [!NOTE]
> Zurzeit wird nur die HTML5-Wiedergabe von Streams aus Azure Media Services unterstützt.

## <a name="language-support"></a>Sprachunterstützung
Das Clipper-Widget ist in den folgenden 18 Sprachen verfügbar:
- Chinesisch (vereinfacht)
- Chinesisch (traditionell)
- Tschechisch
- Niederländisch, Flämisch
- Englisch
- Französisch
- Deutsch
- Ungarisch
- Italienisch
- Japanisch
- Koreanisch
- Polnisch
- Portugiesisch (Brasilien)
- Portugiesisch (Portugal)
- Russisch
- Spanisch
- Schwedisch
- Türkisch

## <a name="next-steps"></a>Nächste Schritte
Um mit der Verwendung von Azure Media Clipper zu beginnen, lesen Sie den Artikel mit den [ersten Schritten](media-services-azure-media-clipper-getting-started.md), um Informationen zum Bereitstellen des Widgets zu erhalten.