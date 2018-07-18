---
title: Includedatei
description: Includedatei
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 04/05/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: 6b6e4afa7c8b18c8ce9af8c6abd371b4321e3343
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34852040"
---
| Ressource | Maximales Limit |Notizen|
| --- | --- |---|
| Max. Anzahl von neuen Aufträgen, die alle 30 Sekunden pro Automation-Konto (nicht geplante Aufträge) übermittelt werden können |100 |Wenn dieser Grenzwert erreicht wird, treten bei nachfolgenden Anforderungen zum Erstellen eines Auftrags Fehler auf. Der Client empfängt eine Fehlerantwort.|
| Max. Anzahl gleichzeitig ausgeführter Aufträge pro Automation-Konto (nicht geplante Aufträge) |200 |Wenn dieser Grenzwert erreicht wird, treten bei nachfolgenden Anforderungen zum Erstellen eines Auftrags Fehler auf. Der Client empfängt eine Fehlerantwort.|
| Max. Anzahl von Modulen, die alle 30 Sekunden pro Automation-Konto importiert werden können |5 ||
| Max. Größe eines Moduls |100 MB ||
| Auftrag-Laufzeit – Free-Tarif |500 Minuten pro Abonnement pro Kalendermonat ||
| Maximal zulässiger Speicherplatz pro Sandbox**<sup>1</sup>** |1 GB |Betrifft nur Azure-Sandboxen|
| Maximaler Arbeitsspeicher pro Sandbox**<sup>1</sup>** |400 MB |Betrifft nur Azure-Sandboxen|
| Maximale zulässige Anzahl von Netzwerksockets pro Sandbox**<sup>1</sup>** |1000 |Betrifft nur Azure-Sandboxen|
| Maximale Anzahl von Automation-Konten in einem Abonnement |Keine Begrenzung ||
|Maximale Anzahl gleichzeitiger Aufträge, die auf einem einzelnen Hybrid Runbook Worker ausgeführt werden können|50 ||

**<sup>1</sup>** Eine Sandbox ist eine gemeinsam verwendete Umgebung, die von mehreren Aufträgen verwendet werden kann. Aufträge, die eine gemeinsame Sandbox verwenden, sind durch die Ressourceneinschränkungen der Sandbox gebunden.
