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
ms.openlocfilehash: 03fe587ede297ac7dea90b7a5fb2d5323f60659e
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628139"
---
| Ressource | Ziel | Harte Grenze |
|----------|--------------|------------|
| Speichersynchronisierungsdienste pro Abonnement | 15 Speichersynchronisierungsdienste pro Region | Nein  |
| Synchronisierungsgruppen pro Speichersynchronisierungsdienst | 100 Synchronisierungsgruppen | JA |
| Registrierte Server pro Speichersynchronisierungsdienst | 99 Server | JA |
| Cloudendpunkte pro Synchronisierungsgruppe | 1 Cloudendpunkt | JA |
| Serverendpunkte pro Synchronisierungsgruppe | 50 Serverendpunkte | Nein  |
| Serverendpunkte pro Server | 33–99 Serverendpunkte | Ja, variiert aber je nach Konfiguration (CPU, Arbeitsspeicher, Volumes, Änderungsrate der Dateien, Anzahl der Dateien usw.) |
| Endpunktgröße | 4 TiB | Nein  |
| Dateisystemobjekte (Verzeichnisse und Dateien) pro Synchronisierungsgruppe | 25 Millionen Objekte | Nein  |
| Maximale Anzahl von Dateisystemobjekten (Verzeichnisse und Dateien) in einem Verzeichnis | 200.000 Objekte | JA |
| Maximale Objektnamenlänge (Verzeichnisse und Dateien) | 255 Zeichen | JA |
| Maximale Sicherheitsbeschreibung des Objekts (Verzeichnisse und Dateien) | 4 KiB | JA |
| Dateigröße | 100 GB | Nein  |
| Minimale Dateigröße für die Unterteilung einer Datei | 64 KiB | JA |
| Gleichzeitige Synchronisierungssitzungen | V4-Agent: Limit variiert basierend auf verfügbaren Systemressourcen. <BR> V3-Agent: 2 aktive Synchronisierungssitzungen pro Prozessor oder maximal 8 aktive Synchronisierungssitzungen pro Server | JA
