---
title: Verkehrsinformationen in Azure Maps | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Verkehrsinformationen in Azure Maps.
services: azure-maps
keywords: ''
author: kgremban
ms.author: kgremban
ms.date: 11/28/2017
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: ''
ms.openlocfilehash: 49e91287d23c35d8cfea858d41c47ba15efd820c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
---
# <a name="azure-maps-traffic-coverage"></a>Verkehrsinformationen in Azure Maps

Azure Maps bietet umfangreiche Verkehrsinformationen in Form von **Verkehrsfluss** und **Verkehrsvorfällen**. Diese Daten können auf Karten visualisiert oder zum Erstellen besserer Routen verwendet werden, die reale Fahrbedingungen berücksichtigen. 

Maps bietet allerdings nicht für alle Regionen das gleiche Maß an Informationen und Genauigkeit. Die folgende Tabelle enthält Informationen dazu, welche Art von Verkehrsinformationen in den jeweiligen Regionen angefordert werden kann: 

|Region  |Vorfälle  |Flow  |
|---------|:---------:|:---------:|
|Argentinien      |         |✓         |
|Australien     |✓         |✓        |
|Österreich     |✓         |✓         |
|Bahrain     |         |✓         |
|Belgien     |✓         |✓         |
|Brasilien     |✓         |✓         |
|Bulgarien     |✓         |✓         |
|Kanada     |✓         |✓         |
|Chile     |✓         |✓         |
|Kolumbien      |         |✓         |
|Kroatien     |         |✓         |
|Tschechische Republik     |✓         |✓         |
|Dänemark     |✓         |✓         |
|Ägypten     |         |✓         |
|Estland     |         | ✓        |
|Finnland     |✓         |✓         |
|+Åland-Inseln      |✓         |✓         |
|Frankreich     |✓         |✓         |
|+Monaco     |✓         |✓         |
|Deutschland     |✓         |✓         |
|Griechenland     |✓         |✓         |
|Hongkong (SAR)     |✓         |✓         |
|Ungarn     |✓         |✓         |
|Island     |         |✓         |
|Indonesien     |✓         |✓         |
|Irland     |✓         |✓         |
|Israel     |         |✓         |
|Italien     |✓         |✓        |
|+San Marino     |✓         |✓         |
|Kuwait     |✓         |✓         |
|Lettland     |         |✓         |
|Lesotho     |✓         |✓         |
|Litauen     |         |✓         |
|Luxemburg     |✓         |✓         |
|Macau (SAR)     |         |✓         |
|Malaysia     |✓         |✓         |
|Malta     |✓         |✓         |
|Mexiko     |✓         |✓         |
|Marokko     |         |✓         |
|Niederlande     |✓         |✓         |
|Neuseeland     |✓         |✓         |
|Norwegen     |✓         |✓         |
|Oman     |         |✓         |
|Polen     |✓         |✓         |
|Portugal     |✓         |✓         |
|+Azoren und Madeira     |         |✓         |
|Katar     |         |✓         |
|Rumänien     |         |✓         |
|Russische Föderation     |✓         |✓         |
|Saudi-Arabien     |✓         |✓         |
|Singapur     |✓         |✓         |
|Slowakei     |✓         |✓         |
|Slowenien     |✓         |✓         |
|Südafrika     |✓         |✓         |
|Spanien     |✓         |✓         |
|+Andorra     |✓         |✓         |
|+Balearische Inseln     |✓         |✓         |
|+Kanarische Inseln     |✓         |✓         |
|+Gibraltar     |✓         |✓         |
|Schweden     |✓         |✓         |
|Schweiz     |✓         |✓        |
|+Liechtenstein      |✓         |✓         |
|Taiwan     |✓         |✓        |
|Thailand     |✓         |✓        |
|Türkei     |✓         |✓         |
|Ukraine     |✓         |✓         |
|Vereinigte Arabische Emirate     |✓         |✓         |
|Vereinigtes Königreich     |✓         |✓         |
|(Guernsey & Jersey)     |✓         |✓         |
|Insel Man     |✓         |✓         |
|USA     |✓         |✓        |
|+Puerto Rico     |✓         |✓         |

Weitere Informationen zur Azure Maps-Verkehrsinformationen finden Sie auf den Referenzseiten zum [Verkehr](https://docs.microsoft.com/rest/api/maps/traffic).