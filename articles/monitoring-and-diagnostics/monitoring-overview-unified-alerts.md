---
title: Einheitliche Oberfläche für Warnungen in Azure Monitor
description: Beschreibung der einheitlichen Oberfläche für Warnungen in Azure, über die Sie Warnungen und Warnungsregeln für alle Azure-Dienste verwalten können.
author: manishsm-msft
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: mamit
ms.component: alerts
ms.openlocfilehash: c4c8279a1d4638a1c5d889b53e2d9e89e458cc37
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117170"
---
# <a name="unified-alerts-in-azure-monitor"></a>Einheitliche Oberfläche für Warnungen in Azure Monitor

## <a name="overview"></a>Übersicht

> [!NOTE]
>  Eine neue einheitliche Oberfläche für Warnungen, über die Sie Warnungen aus mehreren Abonnements verwalten können und auf der Warnungsstatus und intelligente Gruppen eingeführt werden, ist derzeit in Public Preview verfügbar. Eine Beschreibung dieser verbesserten Oberfläche und ihrer Aktivierung finden Sie im letzten Abschnitt dieses Artikels.


Dieser Artikel beschreibt die einheitliche Oberfläche für Warnungen in Azure Monitor. Die [bisherige Oberfläche für Warnungen](monitoring-overview-alerts.md) steht über die Option **Warnungen (klassisch)** im Azure Monitor-Menü zur Verfügung. 

## <a name="features-of-the-unified-alert-experience"></a>Features der einheitlichen Oberfläche für Warnungen

Die einheitliche Oberfläche bietet folgende Vorteile gegenüber der klassischen Oberfläche:

-   **Besseres Benachrichtigungssystem**: [Aktionsgruppen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) sind benannte Gruppen von Benachrichtigungen und Aktionen, die in mehreren Warnungen wiederverwendet werden können. 
- **Einheitliche Oberfläche für die Erstellung**: Warnungen und Warnungsregeln für Metriken, Protokolle und Aktivitätsprotokolle in Azure Monitor, Log Analytics und Application Insights können an einem zentralen Ort verwaltet werden. 
- **Anzeige von ausgelösten Log Analytics-Warnungen im Azure-Portal**: Sie können Warnungen von Log Analytics nun mit Warnungen aus anderen Quellen im Azure-Portal anzeigen. Zuvor wurden Warnungen aus anderen Quellen in einem separaten Portal gezeigt.
- **Trennung von ausgelösten Warnungen und Warnungsregeln**: Es wird nun zwischen Warnungsregeln und Warnungen unterschieden. Eine Warnungsregel ist die Definition einer Bedingung, die eine Warnung auslöst. Eine Warnung ist eine Instanz der Auslösung einer Warnungsregel.
- **Verbesserter Workflow**: Die einheitliche Oberfläche für die Erstellung von Warnungen führt Sie durch den Prozess zum Konfigurieren einer Warnungsregel.
 
Metrikwarnungen weisen die folgenden Verbesserungen gegenüber klassischen Metrikwarnungen auf:

-   **Kürzere Wartezeit**: Metrikwarnungen können im Minutentakt ausgeführt werden. Klassische Metrikwarnungen werden immer nur alle fünf Minuten ausgeführt. Aufgrund der Zeit, die für die Erfassung der Protokolle benötigt wird, beträgt die Verzögerung bei Protokollwarnungen weiterhin mehr als eine Minute. 
-   **Unterstützung für mehrdimensionale Metriken**: Es können für dimensionale Metriken Warnungen ausgegeben werden, was heißt, dass Sie eine bestimmte Instanz der Metrik überwachen können.
-   **Mehr Kontrolle über Metrikbedingungen**: Sie können umfangreichere Warnungsregeln definieren. Diese unterstützen die Überwachung der maximalen, minimalen und durchschnittlichen Werte sowie der Gesamtwerte der Metriken.
-   **Kombinierte Überwachung mehrerer Metriken**: Sie können bis zu zwei Metriken mit einer einzelnen Regel überwachen. Eine Warnung wird ausgelöst, wenn beide Metriken ihre jeweiligen Schwellenwerte für den angegebenen Zeitraum überschreiten.
-   **Metriken aus Protokollen** (eingeschränkte Public Preview): Einige der bei Log Analytics eingehenden Protokolldaten können jetzt extrahiert und in Azure Monitor-Metriken konvertiert werden. Dadurch können sie genau wie andere Metriken in Warnungen genutzt werden. 


## <a name="alert-rules"></a>Warnregeln
Die einheitliche Oberfläche für Warnungen verwendet die folgenden Begriffe, um zwischen Warnungsregeln und Warnungen zu unterscheiden, während die Oberfläche für die Erstellung für verschiedene Warnungstypen vereinheitlicht wird.

| Item | Definition |
|:---|:---|
| Warnungsregel | Definition der Bedingung zum Erstellen einer Warnung. Eine Warnungsregel setzt sich aus einer _Zielressource_, einem _Signal_, _Kriterien_ und _Logik_ zusammen. Eine Warnungsregel ist nur in einem _aktivierten_ Zustand aktiv.
| Zielressource | Definiert die jeweiligen Ressourcen und Signale, die für Warnungen verfügbar sind. Ein Ziel kann eine beliebige Azure-Ressource sein.<br><br>Beispiele: Virtuelle Computer, Speicherkonten, VM-Skalierungsgruppen, Log Analytics-Arbeitsbereiche, Application Insights-Ressourcen. |
| Signal | Quelle der Daten, die von der Zielressource ausgegeben werden. Unterstützte Signaltypen sind *Metrik*, *Aktivitätsprotokoll*, *Application Insights* und *Protokoll*. |
| Kriterien | Eine Kombination aus _Signal_ und _Logik_, die auf eine Zielressource angewendet wird.<br><br>Beispiele: CPU in Prozent > 70 %, Serverantwortzeit > 4 ms, Anzahl der Ergebnisse einer Protokollabfrage > 100 usw. |
| Logik | Benutzerdefinierte Logik, um zu überprüfen, ob das Signal den erwarteten Bereich bzw. die erwarteten Werte aufweist. |
| Aktion | Beim Auslösen der Warnung auszuführende Aktion. Bei Auslösen einer Warnung können mehrere Aktionen erfolgen. Diese Warnungen unterstützen Aktionsgruppen.<br><br>Beispiele: Senden einer E-Mail an eine E-Mail-Adresse oder Aufrufen einer Webhook-URL |
| Überwachungsbedingung | Gibt an, ob die Bedingung, durch die eine Metrikwarnung erstellt wurde, behoben wurde. Metrikwarnungsregeln nehmen in regelmäßigen Abständen Stichproben einer bestimmten Metrik. Wenn die Kriterien der Warnungsregel erfüllt sind, wird eine neue Warnung mit der Bedingung „Ausgelöst“ erstellt.  Wenn erneut eine Stichprobe der Metrik genommen wird und die Kriterien weiterhin erfüllt sind, geschieht nichts.  Sind die Kriterien jedoch nicht erfüllt, wird die Bedingung der Warnung in „Gelöst“ geändert. Wenn die Kriterien das nächste Mal erfüllt werden, wird eine weitere Warnung mit der Bedingung „Ausgelöst“ erstellt. |


## <a name="alert-pages"></a>Warnungsseiten
Die einheitliche Oberfläche für Warnungen bietet eine zentrale Stelle zum Anzeigen und Verwalten aller Azure-Warnungen. In den folgenden Abschnitten werden die Funktionen der einzelnen Seiten der einheitlichen Oberfläche für Warnungen beschrieben.

### <a name="alerts-overview-page"></a>Übersichtsseite für Warnungen
Die Übersichtsseite **Warnungen** enthält eine aggregierte Zusammenfassung aller ausgelösten Warnungen und die Gesamtanzahl aktivierter Warnungsregeln. Durch Ändern der Abonnements oder Filterparameter wird die Liste der Aggregate und ausgelösten Warnungen aktualisiert.

 ![Warnungen – Übersicht](./media/monitoring-overview-unified-alerts/alerts-preview-overview2.png) 

### <a name="alert-rules-management"></a>Verwaltung von Warnungsregeln
**Regeln** ist eine einzelne Seite für die Verwaltung aller Warnungsregeln in Ihren Azure-Abonnements. Sie listet alle Warnungsregeln auf und kann basierend auf Zielressourcen, Ressourcengruppen, Regelnamen oder Status sortiert werden. Warnungsregeln können auf dieser Seite auch bearbeitet, aktiviert und deaktiviert werden.

 ![Warnungsregeln](./media/monitoring-overview-unified-alerts/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Erstellen einer Warnungsregel
Warnungen können unabhängig vom Überwachungsdienst oder Signaltyp auf konsistente Weise erstellt werden. Alle ausgelösten Warnungen und zugehörigen Details sind auf einer einzelnen Seite verfügbar.
 
Sie erstellen eine neue Warnungsregel anhand der folgenden drei Schritte:
1. Wählen Sie das _Ziel_ für die Warnung aus.
1. Wählen Sie das _Signal_ aus den verfügbaren Signalen für das Ziel aus.
1. Geben Sie die _Logik_ an, die auf Daten vom Signal angewendet werden soll.
 
Bei dieser vereinfachten Erstellung müssen Sie nicht mehr die Überwachungsquelle oder -signale kennen, die unterstützt werden, bevor eine Azure-Ressource ausgewählt wird. Die Liste verfügbarer Signale wird automatisch auf Grundlage der ausgewählten Zielressource gefiltert und führt Sie durch die Festlegung der Logik für die Warnungsregel.

Weitere Informationen zum Erstellen von Warnungsregeln finden Sie unter [Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor](monitor-alerts-unified-usage.md).

Warnungen sind übergreifend für mehrere Azure-Überwachungsdienste verfügbar. Informationen dazu, wie und wann Sie die jeweiligen Dienste verwenden, finden Sie unter [Überwachen von Azure-Anwendungen und -Ressourcen](./monitoring-overview.md). Die folgende Tabelle enthält eine Liste der in Azure verfügbaren Warnungsregeltypen. Sie gibt an, was derzeit von der einheitlichen Oberfläche für Warnungen unterstützt wird.

| **Überwachungsquelle** | **Signaltyp**  | **Beschreibung** | 
|-------------|----------------|-------------|
| Azure Monitor | Metrik  | Die auch als [Metrikwarnungen nahezu in Echtzeit](monitoring-near-real-time-metric-alerts.md) bezeichneten Metrikwarnungen unterstützen das Auswerten von Metrikbedingungen in Intervallen von bis zu einer Minute und ermöglichen Regeln mit mehreren Metriken sowie mehrdimensionale Metrikregeln. Eine Liste der unterstützten Ressourcentypen finden Sie unter [Neuere Metrikwarnungen für Azure-Dienste im Azure-Portal](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported).<br>[Klassische Metrikwarnungen](monitoring-overview-alerts.md) werden von der neuen Oberfläche für Warnungen nicht unterstützt. Sie finden diese im Azure-Portal unter „Warnungen (Klassisch)“. Die klassischen Warnungen unterstützen einige Metriktypen, die noch nicht zu den neueren Warnungen verschoben wurden. Eine vollständige Liste finden Sie unter [Unterstützte Metriken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics). |
| Log Analytics | Protokolle  | Empfangen Sie Benachrichtigungen, oder führen Sie automatisierte Aktionen aus, wenn eine Protokollsuchabfrage bestimmte Kriterien erfüllt. Warnungen in Log Analytics [werden in die neue Oberfläche kopiert](monitoring-alerts-extend.md). Es ist eine [Vorschau von *Log Analytics-Protokollen als Metriken*](monitoring-alerts-extend-tool.md) verfügbar. In der Vorschau können Sie einige Arten von Protokollen in Metriken konvertieren, zu denen Sie dann Warnungen auf der neuen Oberfläche für Warnungen erstellen können. Die Vorschauversion ist nützlich, wenn Sie Nicht-Azure-Protokolle parallel zu nativen Azure Monitor-Metriken abrufen möchten. |
| Aktivitätsprotokolle | Aktivitätsprotokoll | Enthält die Datensätze für alle Erstellungs-, Aktualisierungs- und Löschaktionen, die vom ausgewählten Ziel erstellt werden. |
| Dienstintegrität | Aktivitätsprotokoll  | Wird von der einheitlichen Oberfläche für Warnungen nicht unterstützt. Siehe [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Application Insights | Protokolle  | Enthält Protokolle mit den Leistungsdaten Ihrer Anwendung. Mithilfe von Analyseabfragen und basierend auf Anwendungsdaten können Sie die Bedingungen für die auszuführenden Aktionen definieren. |
| Application Insights | Metrik | Wird von der einheitlichen Oberfläche für Warnungen nicht unterstützt. Siehe [Metrikwarnungen](../application-insights/app-insights-alerts.md). |
| Application Insights | Webverfügbarkeitstests | Wird von der einheitlichen Oberfläche für Warnungen nicht unterstützt.  Siehe [Webtestwarnungen](../application-insights/app-insights-monitor-web-app-availability.md). Verfügbar für jede Website, die für das Senden von Daten an Application Insights instrumentiert ist. Sie erhalten eine Benachrichtigung, wenn die Verfügbarkeit oder Reaktionsfähigkeit einer Website nicht den Erwartungen entspricht. |

## <a name="enhanced-unified-alerts-public-preview"></a>Verbesserte einheitliche Oberfläche für Warnungen (Public Preview)

Am 1. Juni 2018 wurde eine verbesserte einheitliche Oberfläche für Warnungen in einer öffentlichen Vorschau für Azure Monitor veröffentlicht. Diese Oberfläche baut auf den Vorteilen der [einheitlichen Oberfläche für Warnungen](#overview) auf, die im März 2018 veröffentlicht wurde, und bietet die Möglichkeit, einzelne Warnungen zu verwalten und zu aggregieren sowie den Warnungsstatus zu ändern. Dieser Abschnitt beschreibt die neuen Funktionen und die Navigation durch die neuen Warnungsseiten im Azure-Portal.

### <a name="enhanced-unified-alerts"></a>Verbesserte einheitliche Oberfläche für Warnungen

Die neue Oberfläche bietet die folgenden Funktionen, die in der klassischen einheitlichen Oberfläche nicht verfügbar sind:

- **Anzeigen von Warnungen für mehrere Abonnements**: Sie können nun einzelne Instanzen von Warnungen für mehrere Abonnements in einer einzigen Ansicht anzeigen und verwalten.
- **Verwalten des Status von Warnungen**: Warnungen weisen jetzt einen Status auf, der angibt, ob sie als geschlossen bestätigt wurden.
- **Organisieren von Warnungen mit intelligenten Gruppen**: Intelligente Gruppen fassen automatisch zusammengehörige Warnungen zusammen, sodass Sie diese als eine Gruppe statt einzeln verwalten können.

### <a name="enable-enhanced-unified-alerts"></a>Aktivieren der verbesserten einheitlichen Oberfläche für Warnungen
Aktivieren Sie die neue einheitliche Oberfläche für Warnungen, indem Sie auf das Banner am oberen Rand der Seite „Warnungen“ klicken. Durch diesen Vorgang wird ein Warnungsspeicher erstellt, der die ausgelösten Warnungen für unterstützte Dienste der letzten 30 Tage enthält. Sobald die neue Oberfläche aktiviert ist, können Sie durch Klicken auf das Banner zwischen der alten und der neuen Oberfläche wechseln.

> [!NOTE]
>  Es dauert möglicherweise einige Minuten, bis die neue Oberfläche zum ersten Mal aktiviert ist.

![Banner](media/monitoring-overview-unified-alerts/opt-in-banner.png)

Beim Aktivieren der neuen Oberfläche werden alle Abonnements registriert, auf die Sie Zugriff haben. Obwohl das gesamte Abonnement aktiviert wird, können nur Benutzer, die die neue Oberfläche ausgewählt haben, es auch anzeigen. Andere Benutzer mit Zugriff auf das Abonnement müssen die Oberfläche separat aktivieren.

Das Aktivieren der neuen Oberfläche für Warnungen wirkt sich nicht auf die Konfiguration von Aktionsgruppen oder Benachrichtigungen in den Warnungsregeln aus. Es ändert sich lediglich die Art der Anzeige und Verwaltung ausgelöster Instanzen von Warnungen im Azure-Portal.

### <a name="smart-groups"></a>Intelligente Gruppen
Intelligente Gruppen reduzieren das Warnungsaufkommen, da Sie Ihnen die Verwaltung zusammengehöriger Warnungen als einzelne Einheit anstelle einzelner Warnungen ermöglichen. Sie können ähnlich wie bei Warnungen die Details der intelligenten Gruppen anzeigen und den Status festlegen. Jede Warnung ist nur Mitglied einer einzigen intelligenten Gruppe.

Intelligente Gruppen werden automatisch mithilfe von Machine Learning erstellt, um zusammengehörige Warnungen zu kombinieren, die ein einzelnes Problem darstellen. Wenn eine Warnung erstellt wird, fügt der Algorithmus diese basierend auf Informationen wie bisherigen Mustern, Ähnlichkeit von Eigenschaften und Ähnlichkeit der Struktur einer neuen intelligenten Gruppe oder einer vorhandenen intelligenten Gruppe hinzu. 

Derzeit berücksichtigt der Algorithmus nur Warnungen vom gleichen Überwachungsdienst innerhalb eines Abonnements. Intelligente Gruppen können aufgrund dieser Konsolidierung das Warnungsaufkommen um bis zu 99 % reduzieren. Der Grund für die Einbindung von Warnungen in eine Gruppe wird auf der Detailseite der intelligenten Gruppe angezeigt.

Der Name einer intelligenten Gruppe ist der Name der ersten Warnung. Sie können eine intelligente Gruppe weder erstellen noch umbenennen.


### <a name="alert-states"></a>Warnungsstatus
Mit der verbesserten einheitlichen Oberfläche für Warnungen wird das Konzept des Warnungsstatus eingeführt. Sie können den Status einer Warnung festlegen, um anzugeben, an welchem Punkt des Lösungsprozesses sie sich befindet. Beim Erstellen einer Warnung weist sie den Status *Neu* auf. Sie können den Status ändern, wenn Sie eine Warnung bestätigen oder schließen. Alle Statusänderungen werden im Warnungsverlauf gespeichert.

Die folgenden Warnungsstatus werden unterstützt.

| State (Zustand) | BESCHREIBUNG |
|:---|:---|
| Neu | Das Problem wurde gerade erkannt und noch nicht überprüft. |
| Bestätigt | Ein Administrator hat die Warnung überprüft und mit deren Bearbeitung begonnen. |
| Geschlossen | Das Problem wurde gelöst. Nachdem eine Warnung geschlossen wurde, können Sie diese erneut öffnen, indem Sie den Status ändern. |

Der Status einer Warnung unterscheidet sich von der Überwachungsbedingung. Mit Metrikwarnungsregeln kann eine Warnung auf _Gelöst_ festgelegt werden, wenn die Fehlerbedingung nicht mehr erfüllt ist. Der Warnungsstatus wird vom Benutzer festgelegt und ist unabhängig von der Überwachungsbedingung. Obwohl die Überwachungsbedingung vom System auf „Gelöst“ festgelegt werden kann, ändert sich der Warnungsstatus erst dann, wenn er vom Benutzer geändert wird.

#### <a name="change-the-state-of-an-alert-or-smart-group"></a>Ändern des Status einer Warnung oder intelligenten Gruppe
Sie können den Status einer einzelnen Warnung ändern oder mehrere Warnungen gemeinsam verwalten, indem Sie den Status einer intelligenten Gruppe festlegen.

Ändern Sie den Status einer Warnung, indem Sie in der Detailansicht der Warnung **Warnungsstatus ändern** auswählen. Oder ändern Sie den Status einer intelligenten Gruppe, indem Sie in ihrer Detailansicht **Zustand der intelligenten Gruppe ändern** auswählen. Sie können den Status mehrerer Elemente gleichzeitig ändern, indem Sie diese in einer Listenansicht auswählen und dann am oberen Rand der Seite auf **Status ändern** klicken. 

Wählen Sie in beiden Fällen einen neuen Status im Dropdownmenü aus. Geben Sie optional einen Kommentar ein. Wenn Sie ein einzelnes Element ändern, haben Sie auch die Möglichkeit, die gleichen Änderungen auf alle Warnungen in der intelligenten Gruppe anzuwenden.

![Ändern des Status](media/monitoring-overview-unified-alerts/change-tate.png)

### <a name="alerts-page"></a>Seite „Warnungen“
Die Standardseite „Warnungen“ enthält eine Zusammenfassung der Warnungen, die in einem bestimmten Zeitfenster erstellt werden. Sie zeigt die gesamten Warnungen für jeden Schweregrad mit Spalten an, in denen die Gesamtanzahl der Warnungen im jeweiligen Status für jeden Schweregrad angegeben ist. Klicken Sie auf einen der Schweregrade, um die Seite [Alle Warnungen](#all-alerts-page) gefiltert nach diesem Schweregrad zu öffnen.

![Seite „Warnungen“](media/monitoring-overview-unified-alerts/alerts-page.png)

Zum Filtern dieser Ansicht können Sie Werte in den Dropdownmenüs am oberen Rand der Seite auswählen.

| Spalte | Beschreibung |
|:---|:---|
| Abonnement | Wählen Sie bis zu fünf Azure-Abonnements aus. Nur Warnungen in den ausgewählten Abonnements sind in der Ansicht enthalten. |
| Ressourcengruppe | Wählen Sie eine einzelne Ressourcengruppe aus. Nur Warnungen mit Zielen in der ausgewählten Ressourcengruppe sind in der Ansicht enthalten. |
| Zeitbereich | Nur Warnungen, die innerhalb des ausgewählten Zeitfensters ausgelöst wurden, sind in der Ansicht enthalten. Unterstützte Werte sind die letzte Stunde, die letzten 24 Stunden, die letzten 7 Tage und die letzten 30 Tage. |

Klicken Sie auf die folgenden Werte am oberen Rand der Seite „Warnungen“, um eine andere Seite zu öffnen.

| Wert | BESCHREIBUNG |
|:---|:---|
| Warnungen gesamt | Die Gesamtanzahl der Warnungen, die den ausgewählten Kriterien entsprechen. Wählen Sie diesen Wert aus, um die Ansicht „Alle Warnungen“ ohne Filter zu öffnen. |
| Intelligente Gruppen | Die Gesamtanzahl anhand von Warnungen erstellter intelligenter Gruppen, die den ausgewählten Kriterien entsprechen. Wählen Sie diesen Wert aus, um die Liste „Intelligente Gruppen“ in der Ansicht „Alle Warnungen“ zu öffnen.
| Warnungsregeln gesamt | Die Gesamtanzahl der Warnungsregeln im ausgewählten Abonnement und der ausgewählten Ressourcengruppe. Wählen Sie diesen Wert aus, um die Ansicht „Regeln“ gefiltert nach ausgewählten Abonnements und ausgewählter Ressourcengruppe zu öffnen.


### <a name="all-alerts-page"></a>Seite „Alle Warnungen“ 
Auf der Seite „Alle Warnungen“ können Sie eine Liste der Warnungen anzeigen, die innerhalb des ausgewählten Zeitfensters erstellt wurden. Sie können entweder eine Liste der einzelnen Warnungen oder eine Liste der intelligenten Gruppen anzeigen, die diese Warnungen enthalten. Klicken Sie auf das Banner am oberen Rand der Seite, um zwischen den Ansichten zu wechseln.

![Seite „Alle Warnungen“](media/monitoring-overview-unified-alerts/all-alerts-page.png)

Zum Filtern der Ansicht können Sie die folgenden Werte in den Dropdownmenüs am oberen Rand der Seite auswählen.

| Spalte | BESCHREIBUNG |
|:---|:---|
| Abonnement | Wählen Sie bis zu fünf Azure-Abonnements aus. Nur Warnungen in den ausgewählten Abonnements sind in der Ansicht enthalten. |
| Ressourcengruppe | Wählen Sie eine einzelne Ressourcengruppe aus. Nur Warnungen mit Zielen in der ausgewählten Ressourcengruppe sind in der Ansicht enthalten. |
| Ressourcentyp | Wählen Sie mindestens einen Ressourcentyp aus. Nur Warnungen mit Zielen des ausgewählten Typs sind in der Ansicht enthalten. Diese Spalte ist nur verfügbar, nachdem eine Ressourcengruppe angegeben wurde. |
| Ressource | Wählen Sie eine Ressource aus. Nur Warnungen mit dieser Ressource als Ziel sind in der Ansicht enthalten. Diese Spalte ist nur verfügbar, nachdem ein Ressourcentyp angegeben wurde. |
| Schweregrad | Wählen Sie einen Warnungsschweregrad oder *Alle* aus, um Warnungen aller Schweregrade einzuschließen. |
| Überwachungsbedingung | Wählen Sie eine Überwachungsbedingung oder *Alle* aus, um Warnungen aller Bedingungen einzuschließen. |
| Warnungsstatus | Wählen Sie einen Warnungsstatus oder *Alle* aus, um Warnungen aller Statusarten einzuschließen. |
| Überwachungsdienst | Wählen Sie einen Dienst oder *Alle* aus, um alle Dienste einzuschließen. Nur Warnungen, die anhand von Regeln erstellt wurden, die diesen Dienst als Ziel verwenden, sind enthalten. |
| Zeitbereich | Nur Warnungen, die innerhalb des ausgewählten Zeitfensters ausgelöst wurden, sind in der Ansicht enthalten. Unterstützte Werte sind die letzte Stunde, die letzten 24 Stunden, die letzten 7 Tage und die letzten 30 Tage. |

Klicken Sie am oberen Rand der Seite auf **Spalten**, um die anzuzeigenden Spalten auszuwählen. 

### <a name="alert-detail-page"></a>Seite „Warnungsdetails“
Die Seite „Warnungsdetails“ wird angezeigt, wenn Sie auf eine Warnung klicken. Sie enthält Details der Warnung und ermöglicht Ihnen das Ändern des Status.

![Warnungsdetails](media/monitoring-overview-unified-alerts/alert-detail.png)

Die Seite „Warnungsdetails“ enthält die folgenden Abschnitte.

| Abschnitt | BESCHREIBUNG |
|:---|:---|
| Zusammenfassung | Zeigt die Eigenschaften und andere wichtige Informationen zur Warnung an. |
| Verlauf | Listet die einzelnen Aktionen, die von der Warnung ausgeführt wurden, und alle an der Warnung vorgenommenen Änderungen auf. Dies ist derzeit auf Statusänderungen beschränkt. |
| Intelligente Gruppe | Informationen zur intelligenten Gruppe, der die Warnung angehört. Die *Warnungsanzahl* bezieht sich auf die Anzahl der Warnungen, die in der intelligenten Gruppe enthalten sind. Dies schließt die anderen Warnungen in der gleichen intelligenten Gruppe ein, die in den letzten 30 Tagen erstellt wurden.  Dies ist unabhängig vom Zeitfilter auf der Listenseite für Warnungen. Klicken Sie auf eine Warnung, um deren Details anzuzeigen. |
| Weitere Informationen | Zeigt weitere kontextbezogene Informationen für die Warnung an. Diese sind normalerweise für den Typ der Quelle spezifisch, die die Warnung erstellt hat. |


### <a name="smart-group-detail-page"></a>Detailseite für intelligente Gruppe
Die Detailseite für die intelligente Gruppe wird angezeigt, wenn Sie auf eine intelligente Gruppe klicken. Sie enthält Details zur intelligenten Gruppe (einschließlich der Gründe ihre Erstellung) und ermöglicht Ihnen das Ändern des Status.
 
![Details der intelligenten Gruppe](media/monitoring-overview-unified-alerts/smart-group-detail.png)


Die Detailseite für die intelligente Gruppe enthält die folgenden Abschnitte.

| Abschnitt | BESCHREIBUNG |
|:---|:---|
| Alerts | Listet die einzelnen Warnungen auf, die in der intelligenten Gruppe enthalten sind. Klicken Sie auf eine Warnung, um deren Detailseite zu öffnen. |
| Verlauf | Listet die einzelnen Aktionen, die von der intelligenten Gruppe ausgeführt wurden, und alle daran vorgenommenen Änderungen auf. Dies ist derzeit auf Statusänderungen und Änderungen der Warnungsmitgliedschaft beschränkt. |

## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über das Erstellen, Anzeigen und Verwalten von Warnungen mithilfe der neuen Oberfläche „Warnungen“.](monitor-alerts-unified-usage.md)
- [Erfahren Sie mehr über Protokollwarnungen auf der Oberfläche „Warnungen“.](monitor-alerts-unified-log.md)
- [Erfahren Sie mehr über Metrikwarnungen auf der Oberfläche „Warnungen“.](monitoring-near-real-time-metric-alerts.md)
- [Erfahren Sie mehr über Aktivitätsprotokollwarnungen auf der Oberfläche „Warnungen“.](monitoring-activity-log-alerts-new-experience.md)
