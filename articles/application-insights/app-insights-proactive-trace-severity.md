---
title: Intelligente Erkennung – Verschlechterung des Schweregrads der Ablaufverfolgung – in Azure Application Insights | Microsoft Docs
description: Überwachen Sie Anwendungsablaufverfolgungen mit Azure Application Insights auf ungewöhnliche Muster in der Ablaufverfolgungstelemetrie.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: mbullwin
ms.openlocfilehash: a593e42ed87ab985d4f4584ed6b73109716120bf
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52741806"
---
# <a name="degradation-in-trace-severity-ratio-preview"></a>Verschlechterung des Schweregrads der Ablaufverfolgung (Vorschau)

Ablaufverfolgungen werden häufig in Anwendungen verwendet, weil sie dabei helfen, die Abläufe im Hintergrund nachzuvollziehen. Wenn etwas schief geht, bieten Ablaufverfolgungen entscheidende Einblicke in die Abfolge der Ereignisse, die zu dem unerwünschten Zustand geführt haben. Ablaufverfolgungen sind zwar in der Regel unstrukturiert, aber dennoch kann man von ihnen konkret etwas ableiten – ihren Schweregrad. Im stabilen Zustand einer Anwendung erwarten wir, dass das Verhältnis zwischen „guten“ Ablaufverfolgungen (*Info* und *Ausführlich*) und „schlechten“ Ablaufverfolgungen (*Warnung*, *Fehler* und *Kritisch*) stabil bleibt. Es wird davon ausgegangen, dass „schlechte“ Ablaufverfolgungen in gewissem Umfang aus einer Vielzahl von Gründen (z. B. aufgrund vorübergehender Netzwerkprobleme) regelmäßig auftreten können. Aber wenn ein echtes Problem wächst, manifestiert es sich in der Regel als eine Zunahme des relativen Anteils von „schlechten“ und „guten“ Ablaufverfolgungen. Die intelligente Erkennung von Application Insights analysiert die von Ihrer Anwendung protokollierten Ablaufverfolgungen automatisch und kann Sie vor ungewöhnlichen Mustern im Schweregrad der Ablaufverfolgungstelemetrie warnen.

Diese Funktion erfordert keine spezielle Einrichtung, abgesehen von der Konfiguration der Ablaufverfolgungsprotokollierung für Ihre App. (Lesen Sie die Informationen zum Konfigurieren eines Ablaufverfolgungs-Protokolllisteners für [.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-trace-logs) oder [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-trace-logs).) Sie ist aktiv, wenn Ihre App genügend Ausnahmetelemetriedaten generiert.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wann erhalte ich diese Art von Benachrichtigung der intelligenten Erkennung?
Sie können diese Art von Benachrichtigung erhalten, wenn das Verhältnis zwischen „guten“ (mit dem Status *Info* oder *Ausführlich* protokollierten) Ablaufverfolgungen und „schlechten“ (mit dem Status *Warnung*, „*Fehler“ oder *Schwerwiegend* protokollierten) Ablaufverfolgungen an einem bestimmten Tag abnimmt, im Vergleich zu einer Baseline, die über die letzten sieben Tage berechnet wurde.

## <a name="does-my-app-definitely-have-a-problem"></a>Liegt wirklich ein Problem mit meiner App vor?
Nein, eine Benachrichtigung bedeutet nicht zwingend, dass ein Problem mit Ihrer App vorliegt. Obwohl eine Verschlechterung des Verhältnisses zwischen „guten“ und „schlechten“ Ablaufverfolgungen auf ein Anwendungsproblem hindeuten könnte, hat diese Änderung des Verhältnisses möglicherweise keine Auswirkungen. Der Anstieg könnte z. B. auf einen neuen Flow in der Anwendung zurückzuführen sein, der mehr „schlechte“ Ablaufverfolgungen ausgibt als bestehende Flows.

## <a name="how-do-i-fix-it"></a>Wie behebe ich das Problem?
Die Benachrichtigungen umfassen Diagnoseinformationen zur Unterstützung des Diagnoseprozesses:
1. **Selektierung.** Die Benachrichtigung zeigt an, wie viele Vorgänge betroffen sind. Dadurch können Sie dem Problem eine Priorität zuweisen.
2. **Umfang.** Betrifft das Problem den gesamten Datenverkehr oder nur bestimmte Vorgänge? Diese Informationen können Sie der Benachrichtigung entnehmen.
3. **Diagnose.** Zur weiteren Diagnose des Problems können Sie die entsprechenden Elemente und Berichte verwenden, die mit weiterführenden Informationen verknüpft sind.


