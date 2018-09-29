---
title: Intelligente Erkennung – Sicherheitserkennungspaket für Azure Application Insights | Microsoft Docs
description: Überwachen Sie Anwendungen mit Azure Application Insights auf potenzielle Sicherheitsprobleme.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: edfe2956da84347abf3e5da7a7d0fd448361c7f5
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47091340"
---
# <a name="application-security-detection-pack-preview"></a>Paket zur Anwendungsicherheitserkennung (Vorschau)

Application Insights analysiert die von Ihrer Anwendung generierten Telemetriedaten automatisch und erkennt potentielle Sicherheitsprobleme. Diese Funktion ermöglicht es Ihnen, potenzielle Sicherheitsprobleme zu identifizieren und diese durch Reparieren der Anwendung oder durch Ergreifen der erforderlichen Sicherheitsmaßnahmen zu beheben.

Diese Funktion erfordert keine spezielle Einrichtung, abgesehen von der [Konfiguration Ihrer App für das Senden von Telemetriedaten](https://docs.microsoft.com/azure/application-insights/app-insights-usage-overview).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wann erhalte ich diese Art von Benachrichtigung der intelligenten Erkennung?
Es gibt drei Arten von Sicherheitsproblemen, die erkannt werden:
1. Unsicherer URL-Zugriff: Auf eine URL in der Anwendung wird über HTTP und HTTPS zugegriffen. In der Regel sollte eine URL, die HTTPS-Anforderungen akzeptiert, keine HTTP-Anforderungen akzeptieren. Dies kann auf einen Fehler oder ein Sicherheitsproblem in Ihrer Anwendung hinweisen.
2. Unsicheres Formular: Ein Formular (oder eine andere „POST“-Anforderung) in der Anwendung verwendet HTTP statt HTTPS. Die Verwendung von HTTP kann die vom Formular gesendeten Benutzerdaten gefährden.
3. Verdächtige Benutzeraktivität: Der Zugriff auf die Anwendung erfolgt aus mehreren Ländern durch denselben Benutzer zum ungefähr gleichen Zeitpunkt. Beispiel: Derselbe Benutzer hat innerhalb einer Stunde von Spanien und den USA aus auf die Anwendung zugegriffen. Diese Erkennung gibt einen potenziell böswilligen Zugriffsversuch auf Ihre Anwendung an.

## <a name="does-my-app-definitely-have-a-security-issue"></a>Liegt in meiner App definitiv ein Sicherheitsproblem vor?
Nein, eine Benachrichtigung bedeutet nicht zwingend, dass in Ihrer App ein Sicherheitsproblem vorliegt. Eine Erkennung eines der oben genannten Szenarien kann in vielen Fällen auf ein Sicherheitsproblem hinweisen. Die Erkennung kann jedoch einen natürlichen geschäftlichen Grund haben und ignoriert werden.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>Wie behebe ich die Erkennung „Unsicherer URL-Zugriff“?
1. **Selektierung.** Die Benachrichtigung enthält die Anzahl von Benutzern, die auf unsichere URLs zugegriffen haben, und die URL, die am häufigsten von unsicheren Zugriffen betroffen war. Dadurch können Sie dem Problem eine Priorität zuweisen.
2. **Umfang.** Wie viel Prozent der Benutzer haben auf unsichere URLs zugegriffen? Wie viele URLs waren betroffen? Diese Informationen können Sie der Benachrichtigung entnehmen.
3. **Diagnose.** Die Erkennung enthält die Liste der unsicheren Anforderungen, die Liste der betroffenen URLs und die Liste der betroffenen Benutzer, sodass Sie das Problem anhand dieser Informationen weiter diagnostizieren können.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>Wie behebe ich die Erkennung „Unsicheres Formular“?
1. **Selektierung.** Die Benachrichtigung enthält die Anzahl der unsicheren Formulare und die Anzahl der Benutzer, deren Daten potenziell gefährdet sind. Dadurch können Sie dem Problem eine Priorität zuweisen.
2. **Umfang.** Welches Formular war an der größten Anzahl unsicherer Übertragungen beteiligt, und wie ist die zeitliche Verteilung unsicherer Übertragungen? Diese Informationen können Sie der Benachrichtigung entnehmen.
3. **Diagnose.** Die Erkennung enthält die Liste der unsicheren Formulare und eine Aufschlüsselung der Anzahl unsicherer Übertragungen für jedes Formular, sodass Sie das Problem anhand dieser Informationen weiter diagnostizieren können.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>Wie behebe ich die Erkennung „Verdächtige Benutzeraktivität“?
1. **Selektierung.** Die Benachrichtigung enthält die Anzahl der verschiedenen Benutzer, die das verdächtige Verhalten gezeigt haben. Dadurch können Sie dem Problem eine Priorität zuweisen.
2. **Umfang.** Aus welchen Ländern stammen die verdächtigen Anforderungen? Welcher Benutzer war am verdächtigsten? Diese Informationen können Sie der Benachrichtigung entnehmen.
3. **Diagnose.** Die Erkennung enthält die Liste der verdächtigen Benutzer und die Liste der Länder für jeden Benutzer, sodass Sie das Problem anhand dieser Informationen weiter diagnostizieren können.
