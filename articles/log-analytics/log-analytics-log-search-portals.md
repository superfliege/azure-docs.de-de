---
title: Anzeigen und Analysieren von Daten in Azure Log Analytics | Microsoft-Dokumentation
description: In diesem Artikel werden die Portale beschrieben, die Sie zum Erstellen und Bearbeiten von Protokollsuchen in Azure Log Analytics verwenden können.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: magoedte; bwren
ms.component: na
ms.openlocfilehash: 386aad94461fa3f2ceafb7564342797eefa2f086
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2018
ms.locfileid: "40246508"
---
# <a name="viewing-and-analyzing-data-in-log-analytics"></a>Anzeigen und Analysieren von Daten in Log Analytics
Im Azure-Portal gibt es zwei Optionen zum Analysieren der in Log Analytics gespeicherten Daten und zum Erstellen von Abfragen für die Ad-hoc-Analyse. Die mit diesen Portalen erstellten Abfragen können für weitere Funktionen wie Warnungen und Dashboards verwendet werden.

## <a name="log-analytics-page-preview"></a>Seite „Log Analytics“ (Vorschau)
Öffnen Sie unter **Protokolle (Vorschau)** im Log Analytics-Menü die Seite „Log Analytics“. Dies ist eine neue Benutzeroberfläche für die Arbeit mit Protokolldaten und das Erstellen von Abfragen. Unter [Erste Schritte mit der Seite „Log Analytics“ im Azure-Portal](query-language/get-started-analytics-portal.md) finden Sie eine Einführung in dieses Portal und können die entsprechenden Funktionen überprüfen.

Auf der Seite „Log Analytics“ finden Sie die folgenden Verbesserungen gegenüber der Benutzeroberfläche für die [Protokollsuche](#log-search).

* Mehrere Registerkarten: Erstellen Sie separate Registerkarten für die Arbeit mit mehreren Abfragen.
* Umfangreiche Visualisierungen dank vielfältiger Diagrammoptionen.
* Verbesserungen bei IntelliSense und automatische Sprachvervollständigung.
* Syntaxhervorhebung zur besseren Lesbarkeit von Abfragen. 
* Abfrage-Explorer: Zugriff auf gespeicherte Abfragen und Funktionen.
* Schemaansicht: Überprüfen Sie die Struktur Ihrer Daten, um das Schreiben von Abfragen zu unterstützen.
* Freigeben: Erstellen Sie Links zu Abfragen, oder heften Sie Abfragen an ein freigegebenes Azure-Dashboard.
* Intelligente Analysen: Identifizieren Sie Spitzen in den Diagrammen, und erhalten Sie eine schnelle Analyse der Fehlerursache.
* Spaltenauswahl: Sortieren und gruppieren Sie Spalten in den Abfrageergebnissen.

> [!NOTE]
> Die Seite „Log Analytics“ verfügt über die gleiche Funktionalität wie das Advanced Analytics-Portal, ein externes Tool außerhalb des Azure-Portals. Das Advanced Analytics-Portal ist weiterhin verfügbar, allerdings werden Links und andere Verweise darauf im Azure-Portal durch diese neue Seite ersetzt.

![Advanced Analytics-Portal](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


### <a name="firewall-requirements"></a>Firewallanforderungen
Ihr Browser benötigt Zugriff auf die folgenden Adressen, um auf die Seite „Log Analytics“ und das Advanced Analytics-Portal zuzugreifen.  Wenn der Browser über eine Firewall auf das Azure-Portal zugreift, müssen Sie den Zugriff auf diese Adressen aktivieren.

| Uri | IP | Ports |
|:---|:---|:---|
| portal.loganalytics.io | Dynamisch | 80, 443 |
| api.loganalytics.io    | Dynamisch | 80, 443 |
| docs.loganalytics.io   | Dynamisch | 80, 443 |


## <a name="log-search"></a>Protokollsuche
Öffnen Sie die Seite „Protokollsuche“ über **Protokolle** im Log Analytics-Menü oder über **Log Analytics** im Azure Monitor-Menü. Diese eignet sich für die Analyse von Protokolldaten mit grundlegenden Abfragen. Die Protokollsuche stellt verschiedene Funktionen für die Bearbeitung von Abfragen bereit, ohne dass Sie die Abfragesprache im Detail kennen müssen.  Unter [Erstellen von Protokollsuchvorgängen in Azure Log Analytics mit dem Portal für die Protokollsuche](log-analytics-log-search-log-search-portal.md) finden Sie eine Zusammenfassung dieser neuen Features. 


![Seite „Protokollsuche“](media/log-analytics-log-search-portals/log-search-portal.png)


## <a name="next-steps"></a>Nächste Schritte

- Absolvieren Sie ein [Tutorial zur Verwendung der Protokollsuche](log-analytics-tutorial-viewdata.md), um zu erfahren, wie Abfragen mithilfe der Abfragesprache erstellt werden.
- Absolvieren Sie eine [Lektion zur Verwendung des Advanced Analytics-Portal](query-language/get-started-analytics-portal.md), welches die gleiche Funktionalität wie die Seite „Log Analytics“ bietet.

