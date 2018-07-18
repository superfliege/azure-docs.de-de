---
title: Verwaltungslösungen in Azure | Microsoft-Dokumentation
description: Bei Verwaltungslösungen in Azure handelt es sich um eine Sammlung von Logik-, Visualisierungs- und Datenerfassungsregeln, die Metriken zu einem bestimmten Problembereich bereitstellen.  Dieser Artikel enthält Informationen zur Installation und Verwendung von Verwaltungslösungen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: 3377a0b4e6440d83962d103b3e1770ccf43bd785
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752920"
---
# <a name="management-solutions-in-azure"></a>Verwaltungslösungen in Azure
Verwaltungslösungen nutzen Dienste in Azure, um zusätzliche Erkenntnisse zum Betrieb einer bestimmten Anwendung oder eines bestimmten Diensts zu liefern. Dieser Artikel enthält eine kurze Übersicht über Verwaltungslösungen in Azure sowie Details zu deren Verwendung und Installation.

Verwaltungslösungen sammeln in der Regel Informationen in Log Analytics und bieten Protokollsuchen und Ansichten für die Analyse der gesammelten Daten. Die Lösungen können auch andere Dienste wie etwa Azure Automation nutzen, um Aktionen im Zusammenhang mit der Anwendung oder dem Dienst auszuführen.

Verwaltungslösungen können Ihrem Azure-Abonnement für beliebige Anwendungen und Dienste hinzugefügt werden, die Sie verwenden. Die Lösungen sind in der Regel kostenlos, durch die Datensammlung können jedoch Kosten entstehen. Zusätzlich zu den [von Microsoft bereitgestellten Lösungen](../monitoring/monitoring-solutions-creating.md) können Partner und Kunden Verwaltungslösungen erstellen und in ihrer eigenen Umgebung verwenden oder über die Community für Kunden verfügbar machen.

## <a name="using-management-solutions"></a>Verwenden von Verwaltungslösungen
Auf der **Übersichtsseite** jedes Log Analytics-Arbeitsbereichs wird für jede im Workspace installierte Lösung jeweils eine Kachel angezeigt. Klicken Sie auf die Kachel für die Lösung, um die dazugehörige Ansicht mit einer ausführlicheren Analyse der gesammelten Daten anzuzeigen.

![Übersicht](media/monitoring-solutions/overview.png)

Verwaltungslösungen können mehrere Arten von Azure-Ressourcen enthalten, und Sie können beliebige, in einer Lösung enthaltene Ressourcen anzeigen (genau wie bei anderen Ressourcen). So ist beispielsweise jede in der Lösung enthaltene Protokollsuche in **Gespeicherte Suchvorgänge** im Arbeitsbereich zu finden. Diese Suchvorgänge können bei der Ad-hoc-Analyse in Log Analytics verwendet werden.

## <a name="list-installed-management-solutions"></a>Auflisten der installierten Verwaltungslösungen 
Gehen Sie wie folgt vor, um die in Ihrem Abonnement installierten Verwaltungslösungen aufzulisten:

1. Melden Sie sich beim Azure-Portal an.
2. Wählen Sie im linken Bereich **Alle Dienste** aus.
3. Scrollen Sie entweder hinunter zu **Lösungen**, oder tippen Sie *Lösungen* in das Dialogfeld **Filter** ein.
4. Alle in Ihren Arbeitsbereichen installierten Lösungen werden aufgelistet. Auf den Namen der Lösung folgt der Name des Log Analytics-Arbeitsbereichs, in dem sie installiert ist.
1. Mit den Dropdownfeldern am oberen Bildschirmrand können Sie die Anzeige nach Abonnement oder Ressourcengruppe filtern.


![Auflisten aller Lösungen](media/monitoring-solutions/list-solutions-all.png)

Klicken Sie auf den Namen einer Lösung, um die dazugehörige Zusammenfassungsseite zu öffnen. Auf dieser Seite werden alle in der Lösung enthaltenen Log Analytics-Ansichten angezeigt, und es stehen verschiedene Optionen für die Lösung und den Arbeitsbereich zur Verfügung. Zeigen Sie die Zusammenfassungsseite für eine Lösung an. Verwenden Sie hierzu eines der obigen Verfahren zum Auflisten der Lösungen, und klicken Sie anschließend auf den Namen der Lösung.

![Lösungseigenschaften](media/monitoring-solutions/solution-properties.png)



## <a name="install-a-management-solution"></a>Installieren einer Verwaltungslösung
Verwaltungslösungen von Microsoft und Partnern sind im [Azure Marketplace](https://azuremarketplace.microsoft.com) verfügbar. Sie können nach verfügbaren Lösungen suchen und sie mit folgendem Verfahren installieren.

1. Klicken Sie in der [Liste mit Lösungen für Ihr Abonnement](#list-installed-management-solutions) auf **Hinzufügen**. 
1. Klicken Sie rechts neben **Verwaltungslösungen** auf **More** (Mehr). 
1. Suchen Sie die gewünschte Verwaltungslösung, und lesen Sie ihre Beschreibung.
1. Klicken Sie auf **Erstellen**, um den Installationsprozess zu starten.
1. Zu Beginn des Installationsvorgangs werden Sie zur Angabe der erforderlichen Konfiguration aufgefordert. Diese unterscheidet sich von Lösung zu Lösung. Bei jeder Konfiguration müssen Sie jedoch einen Log Analytics-Arbeitsbereich für die Installation der Lösung sowie für die Speicherung der gesammelten Daten auswählen. 

![Installieren einer Lösung](media/monitoring-solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Installieren einer Lösung aus der Community
Mitglieder der Community können Verwaltungslösungen für Azure-Schnellstartvorlagen senden. Sie können diese Lösungen entweder direkt installieren oder Vorlagen für eine spätere Installation herunterladen.

1. Führen Sie den unter [Log Analytics-Arbeitsbereich und Automation-Konto](#log-analytics-workspace-and-automation-account) beschriebenen Prozess aus, um einen Arbeitsbereich und ein Konto zu verknüpfen.
2. Gehen Sie zu [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/). 
3. Suchen Sie nach einer Lösung, an der Sie interessiert sind.
4. Wählen Sie die Lösung aus den Ergebnissen aus, um ihre Details anzuzeigen.
5. Klicken Sie auf die Schaltfläche **Bereitstellen in Azure**.
6. Sie werden aufgefordert, Informationen wie die Ressourcengruppe und den Standort sowie Werte für Parameter in der Lösung anzugeben.
7. Klicken Sie auf **Kaufen**, um die Lösung zu installieren.


## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics-Arbeitsbereich und Automation-Konto
Für alle Verwaltungslösungen wird ein [Log Analytics-Arbeitsbereich](../log-analytics/log-analytics-manage-access.md) zum Speichern der von der Lösung gesammelten Daten sowie zum Hosten der Protokollsuchvorgänge und Ansichten benötigt. Einige Lösungen erfordern auch ein [Automation-Konto](../automation/automation-security-overview.md#automation-account-overview) für Runbooks und zugehörige Ressourcen. Der Arbeitsbereich und das Konto müssen den folgenden Anforderungen entsprechen.

* Eine Lösungsinstallation kann jeweils nur einen Log Analytics-Arbeitsbereich und ein einzelnes Automation-Konto verwenden. Sie können die Lösung separat in mehreren Arbeitsbereichen installieren.
* Sollte eine Lösung ein Automation-Konto benötigen, müssen der Log Analytics-Arbeitsbereich und das Automation-Konto miteinander verknüpft werden. Ein Log Analytics-Arbeitsbereich kann nur mit einem Automation-Konto verknüpft werden, und ein Automation-Konto kann nur mit einem Log Analytics-Arbeitsbereich verknüpft werden.
* Damit sie verknüpft werden können, müssen sich der Log Analytics-Arbeitsbereich und das Automation-Konto in der gleichen Ressourcengruppe und im gleichen Bereich befinden. (Ausnahme: Ein Arbeitsbereich befindet sich in der Region „USA, Osten“ und ein Automation-Konto in der Region „USA, Osten 2“.)

### <a name="creating-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Erstellen einer Verknüpfung zwischen einem Log Analytics-Arbeitsbereich und einem Automation-Konto
Wie Sie den Log Analytics-Arbeitsbereich und das Automation-Konto angeben, hängt von der Installationsmethode Ihrer Lösung ab.

* Wenn Sie die Lösung über den Azure Marketplace installieren, werden Sie zur Angabe eines Arbeitsbereichs und eines Automation-Kontos aufgefordert. Die Verknüpfung zwischen beiden wird für Sie erstellt, sofern sie noch nicht besteht.
* Für Lösungen außerhalb des Azure Marketplace müssen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto miteinander verknüpfen, bevor Sie die Lösung installieren. Hierzu wählen Sie eine Lösung im Azure Marketplace aus und dann den Log Analytics-Arbeitsbereich und das Automation-Konto. Sie müssen die Lösung nicht wirklich installieren, da die Verknüpfung erstellt wird, sobald der Log Analytics-Arbeitsbereich und das Automation-Konto ausgewählt werden. Nachdem die Verknüpfung erstellt wurde, können Sie diesen Log Analytics-Arbeitsbereich und dieses Automation-Konto für jede Lösung verwenden.

### <a name="verifying-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Überprüfen der Verknüpfung zwischen einem Log Analytics-Arbeitsbereich und einem Automation-Konto
Sie können die Verknüpfung zwischen einem Log Analytics-Arbeitsbereich und einem Automation-Konto mithilfe des folgenden Verfahrens überprüfen.

1. Wählen Sie im Azure-Portal das Automation-Konto aus.
1. Scrollen Sie im Menü zum Abschnitt **Zugehörige Ressourcen**.
1. Wenn die Einstellung **Arbeitsbereich** aktiviert ist, ist dieses Konto mit einem Log Analytics-Arbeitsbereich verknüpft. Wenn Sie die Details des Arbeitsbereichs anzeigen möchten, klicken Sie auf **Arbeitsbereich**.

## <a name="remove-a-management-solution"></a>Entfernen einer Verwaltungslösung
Wenn Sie eine installierte Lösung entfernen möchten, suchen Sie sie in der [Liste mit den installierten Lösungen](#list-installed-management-solutions). Klicken Sie auf den Namen der Lösung, um die dazugehörige Zusammenfassungsseite zu öffnen, und klicken Sie anschließend auf **Löschen**.




## <a name="next-steps"></a>Nächste Schritte
* Rufen Sie eine [Liste mit Verwaltungslösungen von Microsoft](monitoring-solutions-inventory.md) ab.
* Machen Sie sich mit dem [Erstellen von Abfragen](../log-analytics/log-analytics-log-searches.md) für die Analyse der von Verwaltungslösungen gesammelten Daten vertraut.

