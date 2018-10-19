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
ms.openlocfilehash: 34cae9172d9b024bd6866742d39d82ad496bfc52
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45570459"
---
| Ressource | Maximales Limit |Notizen|
| --- | --- |---|
| Max. Anzahl von neuen Aufträgen, die alle 30 Sekunden pro Automation-Konto (nicht geplante Aufträge) übermittelt werden können |100 |Wenn dieser Grenzwert erreicht wird, treten bei nachfolgenden Anforderungen zum Erstellen eines Auftrags Fehler auf. Der Client empfängt eine Fehlerantwort.|
| Max. Anzahl gleichzeitig ausgeführter Aufträge pro Automation-Konto (nicht geplante Aufträge) |200 |Wenn dieser Grenzwert erreicht wird, treten bei nachfolgenden Anforderungen zum Erstellen eines Auftrags Fehler auf. Der Client empfängt eine Fehlerantwort.|
| Max. Anzahl von Modulen, die alle 30 Sekunden pro Automation-Konto importiert werden können |5 ||
| Max. Größe eines Moduls |100 MB ||
| Auftrag-Laufzeit – Free-Tarif |500 Minuten pro Abonnement pro Kalendermonat ||
| Maximal zulässiger Speicherplatz pro Sandbox **<sup>1</sup>** |1 GB |Betrifft nur Azure-Sandboxen|
| Maximaler Arbeitsspeicher pro Sandbox **<sup>1</sup>** |400 MB |Betrifft nur Azure-Sandboxen|
| Maximale zulässige Anzahl von Netzwerksockets pro Sandbox **<sup>1</sup>** |1000 |Betrifft nur Azure-Sandboxen|
| Maximale Laufzeit pro Runbook **<sup>1</sup>** |3 Stunden |Betrifft nur Azure-Sandboxen|
| Maximale Anzahl von Automation-Konten in einem Abonnement |Keine Begrenzung ||
|Maximale Anzahl gleichzeitiger Aufträge, die auf einem einzelnen Hybrid Runbook Worker ausgeführt werden können|50 ||
| Maximale Größe der Runbookauftragsparameter   | 512 KB||
| Maximale Anzahl von Runbookparametern   | 50|Sie können einen JSON- oder XML-Zeichenfolge an einen Parameter übergeben und ihn mit dem Runbook analysieren, wenn Sie das Limit von 50 Parametern erreichen.|
| Maximale Größe für Webhooknutzlast |  512 KB|

**<sup>1</sup>** Eine Sandbox ist eine gemeinsam verwendete Umgebung, die von mehreren Aufträgen verwendet werden kann. Aufträge, die eine gemeinsame Sandbox verwenden, sind durch die Ressourceneinschränkungen der Sandbox gebunden.
