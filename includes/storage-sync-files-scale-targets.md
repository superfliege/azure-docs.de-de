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
ms.openlocfilehash: c82717be3791a1bc397bbbdb369c55cd2ba04439
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56213050"
---
| Ressource | Ziel | Harte Grenze |
|----------|--------------|------------|
| Speichersynchronisierungsdienste pro Region | 15 Speichersynchronisierungsdienste | Ja |
| Synchronisierungsgruppen pro Speichersynchronisierungsdienst | 100 Synchronisierungsgruppen | Ja |
| Registrierte Server pro Speichersynchronisierungsdienst | 99 Server | Ja |
| Cloudendpunkte pro Synchronisierungsgruppe | 1 Cloudendpunkt | Ja |
| Serverendpunkte pro Synchronisierungsgruppe | 50 Serverendpunkte | Nein  |
| Serverendpunkte pro Server | 30 Serverendpunkte | Ja |
| Endpunktgröße | 4 TiB | Nein  |
| Dateisystemobjekte (Verzeichnisse und Dateien) pro Synchronisierungsgruppe | 25 Millionen Objekte | Nein  |
| Maximale Anzahl von Dateisystemobjekten (Verzeichnisse und Dateien) in einem Verzeichnis | 1.000.000 Objekte | Ja |
| Maximale Sicherheitsbeschreibung des Objekts (Verzeichnisse und Dateien) | 4 KiB | Ja |
| Dateigröße | 100 GB | Nein  |
| Minimale Dateigröße für die Unterteilung einer Datei | 64 KiB | Ja |
| Gleichzeitige Synchronisierungssitzungen | Mindestens V4-Agent: Limit variiert basierend auf verfügbaren Systemressourcen. <BR> V3-Agent: 2 aktive Synchronisierungssitzungen pro Prozessor oder maximal 8 aktive Synchronisierungssitzungen pro Server | Ja
