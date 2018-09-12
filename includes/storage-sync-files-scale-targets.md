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
ms.openlocfilehash: 3f70a8cf2df25f487de7cd1a8c8cbdf9431839f0
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43283080"
---
| Ressource | Ziel | Harte Grenze |
|----------|--------------|------------|
| Speichersynchronisierungsdienste pro Abonnement | 15 Speichersynchronisierungsdienste | Nein  |
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
