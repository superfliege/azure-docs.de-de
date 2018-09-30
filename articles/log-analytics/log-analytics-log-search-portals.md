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
ms.date: 09/17/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 4677b8d5601dcafbf4760e6f185ef70393229b1a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961058"
---
# <a name="viewing-and-analyzing-data-in-log-analytics"></a>Anzeigen und Analysieren von Daten in Log Analytics
Im Azure-Portal gibt es zwei Optionen zum Analysieren der in Log Analytics gespeicherten Daten und zum Erstellen von Abfragen für die Ad-hoc-Analyse. Die mit diesen Portalen erstellten Abfragen können für weitere Funktionen wie Warnungen und Dashboards verwendet werden.

## <a name="log-analytics-page"></a>Log Analytics-Seite
Öffnen Sie über **Protokolle** im Log Analytics-Menü die Seite „Log Analytics“. Dies ist eine neue Benutzeroberfläche für die Arbeit mit Protokolldaten und das Erstellen von Abfragen. Unter [Erste Schritte mit der Seite „Log Analytics“ im Azure-Portal](query-language/get-started-analytics-portal.md) finden Sie eine Einführung in dieses Portal und können die entsprechenden Funktionen überprüfen.

Auf der Seite „Log Analytics“ finden Sie die folgenden Verbesserungen gegenüber der Benutzeroberfläche für die [Protokollsuche (klassisch)](#log-search-classic).

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

### <a name="resource-logs"></a>Ressourcenprotokolle
Die neue Log Analytics-Benutzererfahrung ist in verschiedene Azure-Ressourcen, wie etwa VMs, integriert. Das bedeutet, dass Sie die Log Analytics-Seite direkt über das Überwachungsmenü der Ressource öffnen können, ohne zu Azure Monitor oder Log Analytics zu wechseln und den Ressourcenkontext zu verlieren. **Protokolle** wurde noch nicht für alle Azure-Ressourcen aktiviert, es taucht jedoch nach und nach in den Portalmenüs verschiedener Ressourcentypen auf.

Beim Öffnen von Log Analytics aus einer bestimmten Ressource ist der Umfang automatisch so festgelegt, dass nur Datensätze der betreffenden Ressource protokolliert werden.   Wenn Sie eine Abfrage schreiben möchten, die andere Datensätze enthält, müssen Sie es über das Log Analytics- oder das Azure Monitor-Menü öffnen.

Die folgenden Optionen stehen noch nicht über die Ressourcenansicht von Log Analytics zur Verfügung:

- Speichern
- Warnung festlegen
- Abfrage-Explorer
- Wechseln zu einem anderen Arbeitsbereich/einer anderen Ressource (derzeit nicht geplant)


### <a name="firewall-requirements"></a>Firewallanforderungen
Ihr Browser benötigt Zugriff auf die folgenden Adressen, um auf die Seite „Log Analytics“ und das Advanced Analytics-Portal zuzugreifen.  Wenn der Browser über eine Firewall auf das Azure-Portal zugreift, müssen Sie den Zugriff auf diese Adressen aktivieren.

| Uri | IP | Ports |
|:---|:---|:---|
| portal.loganalytics.io | Dynamisch | 80, 443 |
| api.loganalytics.io    | Dynamisch | 80, 443 |
| docs.loganalytics.io   | Dynamisch | 80, 443 |


## <a name="log-search-classic"></a>Protokollsuche (klassisch)
Öffnen Sie die Seite „Protokollsuche“ über **Protokolle (klassisch)** im Log Analytics-Menü oder über **Log Analytics** im Azure Monitor-Menü. Dies ist die klassische Seite, die für die Arbeit mit Log Analytics-Abfragen verwendet wird, und der die oben aufgeführten zusätzlichen Features der [Log Analytics-Seite](#log-analytics-page) fehlen.



![Seite „Protokollsuche“](media/log-analytics-log-search-portals/log-search-portal.png)


## <a name="next-steps"></a>Nächste Schritte

- Absolvieren Sie ein [Tutorial zur Verwendung der Protokollsuche](log-analytics-tutorial-viewdata.md), um zu erfahren, wie Abfragen mithilfe der Abfragesprache erstellt werden.
- Absolvieren Sie eine [Lektion zur Verwendung des Advanced Analytics-Portal](query-language/get-started-analytics-portal.md), welches die gleiche Funktionalität wie die Seite „Log Analytics“ bietet.

