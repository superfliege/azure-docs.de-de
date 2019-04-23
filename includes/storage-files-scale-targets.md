---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 99314538f90404d7c2b72da0dd2da2d8ac60a08a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59804099"
---
| Ressource | Standarddateifreigaben | Premium-Dateifreigaben (Vorschau) |
|----------|---------------|------------------------------------------|
| Mindestgröße einer Dateifreigabe | Keine Mindestgröße, nutzungsbasierte Bezahlung | 100 GiB; bereitgestellt |
| Maximale Größe einer Dateifreigabe | 5 TiB | 5 TiB (öffentliche Vorschau), 100 TiB (eingeschränkte öffentliche Vorschau) |
| Maximale Größe einer Datei in einer Dateifreigabe | 1 TiB | 1 TiB |
| Maximale Anzahl an Dateien in einer Dateifreigabe | Keine Begrenzung | Keine Begrenzung |
| Maximale Anzahl IOPS pro Freigabe | 1.000IOPS | 5.120 Basis-IOPS mit Burstlimit 15.360 (öffentliche Vorschau), 100.000 IOPS (eingeschränkte öffentliche Vorschau)|
| Maximale Anzahl gespeicherter Zugriffsrichtlinien pro Dateifreigabe | 5 | 5 |
| Zieldurchsatz für eine einzelne Dateifreigabe | Bis zu 60 MiB/s | Siehe: Ein- und Ausgangswerte für Premium-Dateifreigabe|
| Maximaler Ausgang für eine einzelne Dateifreigabe | Siehe: Zieldurchsatz für Standard-Dateifreigabe | Bis zu 368 MiB/s (öffentliche Vorschau), bis zu 6.204 MiB/s (eingeschränkte öffentliche Vorschau) |
| Maximaler Eingang für eine einzelne Dateifreigabe | Siehe: Zieldurchsatz für Standard-Dateifreigabe | Bis zu 245 MiB/s (öffentliche Vorschau), bis zu 4.136 MiB/s (eingeschränkte öffentliche Vorschau) |
| Maximale Anzahl geöffneter Handles pro Datei | 2.000 geöffnete Handles | 2.000 geöffnete Handles |
| Maximale Anzahl von Freigabemomentaufnahmen | 200 Freigabemomentaufnahmen | 200 Freigabemomentaufnahmen |
| Maximale Objektnamenlänge (Verzeichnisse und Dateien) | 2.048 Zeichen | 2.048 Zeichen |
| Maximale Pfadnamenkomponente (im Pfad \A\B\C\D ist jeder Buchstabe eine Komponente) | 255 Zeichen | 255 Zeichen |