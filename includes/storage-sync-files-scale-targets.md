---
title: Includedatei
description: Includedatei
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/18/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 00124562d3bc21f9139ae54f022d7893a4eaf11c
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53638862"
---
| Ressource | Ziel | Harte Grenze |
|----------|--------------|------------|
| Speichersynchronisierungsdienste pro Abonnement | 15 Speichersynchronisierungsdienste pro Region | Nein  |
| Synchronisierungsgruppen pro Speichersynchronisierungsdienst | 100 Synchronisierungsgruppen | JA |
| Registrierte Server pro Speichersynchronisierungsdienst | 99 Server | JA |
| Cloudendpunkte pro Synchronisierungsgruppe | 1 Cloudendpunkt | JA |
| Serverendpunkte pro Synchronisierungsgruppe | 50 Serverendpunkte | Nein  |
| Serverendpunkte pro Server | 30 Serverendpunkte | JA |
| Endpunktgröße | 4 TiB | Nein  |
| Dateisystemobjekte (Verzeichnisse und Dateien) pro Synchronisierungsgruppe | 25 Millionen Objekte | Nein  |
| Maximale Anzahl von Dateisystemobjekten (Verzeichnisse und Dateien) in einem Verzeichnis | 200.000 Objekte | JA |
| Maximale Objektnamenlänge (Verzeichnisse und Dateien) | 255 Zeichen | JA |
| Maximale Sicherheitsbeschreibung des Objekts (Verzeichnisse und Dateien) | 4 KiB | JA |
| Dateigröße | 100 GB | Nein  |
| Minimale Dateigröße für die Unterteilung einer Datei | 64 KiB | JA |
| Gleichzeitige Synchronisierungssitzungen | V4-Agent: Limit variiert basierend auf verfügbaren Systemressourcen. <BR> V3-Agent: 2 aktive Synchronisierungssitzungen pro Prozessor oder maximal 8 aktive Synchronisierungssitzungen pro Server | JA
