---
title: Portale zum Erstellen und Bearbeiten von Protokollabfragen in Azure Log Analytics | Microsoft-Dokumentation
description: "In diesem Artikel werden die Portale beschrieben, die Sie zum Erstellen und Bearbeiten von Protokollsuchen in Azure Log Analytics verwenden können."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: magoedte; bwren
ms.openlocfilehash: b205f226d95d94b938a70a834ac0147e76d459ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portale zum Erstellen und Bearbeiten von Protokollabfragen in Azure Log Analytics

Protokollsuchen werden in Log Analytics in zahlreichen Bereichen zum Abrufen von Daten aus dem Arbeitsbereich verwendet.  Um tatsächlich Abfragen zu erstellen und zu bearbeiten und gleichzeitig jedoch interaktiv mit zurückgegebenen Daten zu arbeiten, haben Sie zwei Optionen, die im Folgenden beschrieben werden.  

## <a name="log-search"></a>Protokollsuche 
Die Seite „Protokollsuche“ kann über das Azure-Portal aufgerufen werden.  Es dient zum Erstellen von grundlegenden Abfragen, die in einer einzelnen Zeile erstellt werden können.  Die Protokollsuche kann verwendet werden, ohne ein externes Portal starten zu müssen. Zudem können Sie damit eine Vielzahl von Funktionen mit Protokollsuchen durchführen, einschließlich der Erstellung von Warnungsregeln, der Erstellung von Computergruppen und des Exports der Abfrageergebnisse.  

Die Protokollsuche stellt verschiedene Funktionen für die Bearbeitung der Abfrage bereit – dabei sind keine umfassenden Kenntnisse bezüglich der Abfragesprache erforderlich.  Unter [Erstellen von Protokollsuchvorgängen in Azure Log Analytics mit dem Portal für die Protokollsuche](log-analytics-log-search-log-search-portal.md) finden Sie eine Zusammenfassung dieser neuen Features.


![Seite „Protokollsuche“](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Advanced Analytics-Portal
Das Advanced Analytics-Portal ist ein dediziertes Portal mit erweiterten Funktionen, die in der Protokollsuche im Azure-Portal nicht verfügbar sind.  Zu den Funktionen zählen die Möglichkeiten, eine Abfrage in mehreren Zeilen zu bearbeiten, Codes selektiv auszuführen, kontextabhängige IntelliSense-Funktionen auszuführen und intelligente Analysen durchzuführen.  Das Advanced Analytics-Portal ist ideal für die Entwicklung komplexer Abfragen geeignet, die entweder als Protokollsuche gespeichert oder kopiert und in andere Log Analytics-Elemente eingefügt werden.  Das Advanced Analytics-Portal wird über einen Link auf der Seite „Protokollsuche“ gestartet.

![Advanced Analytics-Portal](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


Aufgrund der erweiterten Funktionen wird das Advanced Analytics-Portal in der Regel als primäres Tool für die Erstellung und Bearbeitung von Abfragen verwendet.  Nachdem Sie sich vergewissert haben, dass die Abfrage erwartungsgemäß funktioniert, kopieren Sie sie, und fügen Sie sie an anderer Stelle ein, z.B. auf der Seite „Protokollsuche“ oder im Ansicht-Designer.  Da das Advanced Analytics-Portal jedoch Abfragen mit mehreren Zeilen unterstützt, müssen Sie beim Kopieren einer Abfrage aus diesem Portal Folgendes berücksichtigen.

- Kommentare müssen aus der Abfrage entfernt werden, bevor sie kopiert und an anderer Stelle eingefügt werden.  Sie können eine Zeile durch zwei vorangestellte Schrägstriche (//) kommentieren.  Wenn Sie eine Abfrage mit mehreren Zeilen in eine einzelne Zeile einfügen, werden Zeilenumbrüche entfernt.  Wenn Kommentare eingeschlossen werden, werden alle Zeichen nach dem ersten Kommentar als Bestandteil des Kommentars behandelt.


## <a name="next-steps"></a>Nächste Schritte

- Absolvieren Sie ein Tutorial zur Verwendung der [Protokollsuche](log-analytics-tutorial-viewdata.md), um zu erfahren, wie Abfragen mithilfe der Abfragesprache erstellt werden.
- Lernen Sie das [Advanced Analytics-Portal](https://go.microsoft.com/fwlink/?linkid=856587) kennen, um komplexe Abfragen zu erstellen und sie als eine Entwicklungsumgebung für Ihre Protokollsuchen zu verwenden.

