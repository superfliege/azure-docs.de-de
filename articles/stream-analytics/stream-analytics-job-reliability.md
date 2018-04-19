---
title: Vermeiden von Dienstunterbrechungen bei Azure Stream Analytics-Aufträgen
description: Dieser Artikel bietet eine Anleitung für die Resilienz von Stream Analytics-Aufträgen bei Upgrades.
services: stream-analytics
author: jseb225
manager: kfile
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 47ccfe99d2ee6576dbb70324eb383f52d2a1b2e7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Gewährleisten der Zuverlässigkeit von Stream Analytics-Aufträgen während Dienstupdates

Ein vollständig verwalteter Dienst bietet unter anderem die Möglichkeit, dass er sich im Handumdrehen um neue Dienstfunktionen und Verbesserungen erweitern lässt. Somit kann für Stream Analytics wöchentlich (oder häufiger) eine Bereitstellung von Dienstupdates durchgeführt werden. Trotz vieler und gründlicher Tests lässt sich nicht gänzlich ausschließen, dass ein vorhandener, aktuell ausgeführter Auftrag aufgrund eines Fehlers unter Umständen unterbrochen wird. Für Kunden, die wichtige Streamingverarbeitungsaufträge ausführen, müssen derartige Risiken vermieden werden. Das Modell der **[Azure-Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** stellt eine Möglichkeit dar, mit der Kunden dieses Risiko verringern können. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Wie wirken Azure-Regionspaare diesem Risiko entgegen?

Mit Stream Analytics wird gewährleistet, dass Aufträge in Regionspaaren in separaten Batches aktualisiert werden. Dies führt zu einer ausreichenden Zeitspanne zwischen den Updates, sodass mögliche Unterbrechungsfehler ermittelt und behoben werden können.

_Mit Ausnahme der Region „Indien, Mitte“_ (deren gekoppelte Region „Indien, Süden“ für Stream Analytics nicht verfügbar ist), erfolgt die Bereitstellung eines Updates für Stream Analytics in einer Gruppe von Regionspaaren nicht gleichzeitig. Bereitstellungen in mehreren Regionen **in derselben Gruppe** können **gleichzeitig** erfolgen.

Der Artikel über **[Verfügbarkeit und Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** enthält die aktuellsten Informationen darüber, welche Regionen zugeordnet werden.

Für Kunden empfiehlt es sich, identische Aufträge für beide gekoppelte Regionen bereitzustellen. Neben den internen Überwachungsfunktionen von Stream Analytics wird Kunden zudem empfohlen, die Aufträge so zu überwachen, als handle es sich bei **beiden** um Produktionsaufträge. Wenn eine Unterbrechung als Folge des Stream Analytics-Dienstupdates ermittelt wird, werden die entsprechende Eskalation und ein Failover der Downstreamconsumer an die fehlerfreie Auftragsausgabe durchgeführt. Die Eskalation zum Support verhindert, dass die gekoppelte Region von der neuen Bereitstellung betroffen ist, und gewährleistet die Integrität der gekoppelten Aufträge.
