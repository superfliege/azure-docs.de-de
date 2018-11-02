---
title: Lösung für die Updateverwaltung in Azure
description: In diesem Artikel soll vermittelt werden, wie Sie die Lösung für die Azure-Updateverwaltung zum Verwalten von Updates für Ihre Windows- und Linux-Computer verwenden können.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 10/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6d2076a91bc7e7c0e2ca9d2fe6899cddec2f8d0b
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024493"
---
# <a name="update-management-solution-in-azure"></a>Lösung für die Updateverwaltung in Azure

Sie können die Lösung für die Updateverwaltung in Azure Automation für Betriebssystemupdates für Ihre Windows- und Linux-Computer verwalten, die in Azure, in lokalen Umgebungen oder bei anderen Cloudanbietern bereitgestellt werden. Sie können den Status der verfügbaren Updates auf allen Agent-Computern schnell auswerten und die Installation der für den Server erforderlichen Updates initiieren.

Die Updateverwaltung für virtuelle Computer kann direkt in Ihrem Azure Automation-Konto aktiviert werden. Informationen zum Aktivieren der Updateverwaltung für virtuelle Computer über das Automation-Konto finden Sie unter [Verwalten von Updates für mehrere virtuelle Azure-Computer](manage-update-multi.md). Sie können die Updateverwaltung im Azure-Portal im Bereich des virtuellen Computers auch für einen einzelnen virtuellen Computer aktivieren. Dieses Szenario ist für virtuelle [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management)- und [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management)-Computer verfügbar.

## <a name="solution-overview"></a>Lösungsübersicht

Verwenden Sie für Computer, die mit der Updateverwaltung verwaltet werden, die folgenden Konfigurationen, um Bewertungen und Updatebereitstellungen durchzuführen:

* Microsoft Monitoring Agent (MMA) für Windows oder Linux
* PowerShell Desired State Configuration (DSC) für Linux
* Automation Hybrid Runbook Worker
* Microsoft Update oder Windows Server Update Services (WSUS) für Windows-Computer

Das folgende Diagramm enthält eine konzeptionelle Darstellung des Verhaltens und Datenflusses und zeigt, wie die Lösung alle verbundenen Windows Server- und Linux-Computer eines Arbeitsbereichs bewertet und Sicherheitsupdates darauf anwendet:

![Ablauf des Updateverwaltungsprozesses](media/automation-update-management/update-mgmt-updateworkflow.png)

Die Updateverwaltung kann für das systeminterne Integrieren von Computern in mehrere Abonnements im selben Mandanten verwendet werden. Zum Verwalten von Computern in einem anderen Mandanten müssen Sie diese als [Nicht-Azure-Computer](automation-onboard-solutions-from-automation-account.md#onboard-a-non-azure-machine) integrieren.

Nachdem ein CVE-Release veröffentlicht wurde, dauert es 2 bis 3 Stunden, bis der Patch für Linux-Computer für die Bewertung angezeigt wird.  Bei Windows-Computern dauert es 12 bis 15 Stunden, bis der Patch nach der Veröffentlichung für die Bewertung angezeigt wird.

Nachdem ein Computer einen Scanvorgang abgeschlossen hat, um die Konformität für das Update zu überprüfen, leitet der Agent die Informationen gesammelt an Azure Log Analytics weiter. Auf einem Windows-Computer wird der Konformitätsscan standardmäßig alle 12 Stunden ausgeführt.

Zusätzlich zum Scanzeitplan wird der Update-Konformitätsscan innerhalb von 15 Minuten initiiert, wenn der MMA neu gestartet wird (jeweils vor und nach der Updateinstallation).

Für einen Linux-Computer wird der Konformitätsscan standardmäßig alle drei Stunden ausgeführt. Wenn der MMA-Agent neu gestartet wird, wird ein Konformitätsscan innerhalb von 15 Minuten eingeleitet.

Die Lösung meldet basierend auf der für die Synchronisierung konfigurierten Quelle, wie aktuell der Computer ist. Wenn der Windows-Computer für das Senden von Meldungen an WSUS konfiguriert ist, können sich die Ergebnisse von den angezeigten Microsoft Update-Ergebnissen unterscheiden. Dies hängt davon ab, wann WSUS zuletzt mit Microsoft Update synchronisiert wurde. Dasselbe gilt für Linux-Computer, die für Meldungen an ein lokales Repository konfiguriert sind (anstatt an ein öffentliches Repository).

> [!NOTE]
> Damit Meldungen an den Dienst ordnungsgemäß erfolgen können, erfordert die Updateverwaltung, dass bestimmte URLs und Ports aktiviert werden. Weitere Informationen zu diesen Anforderungen finden Sie unter [Netzwerkplanung für Hybrid Worker](automation-hybrid-runbook-worker.md#network-planning).

Sie können Softwareupdates auf Computern bereitstellen und installieren, für die die Updates erforderlich sind, indem Sie einen geplante Bereitstellung erstellen. Updates, die als *Optional* klassifiziert sind, sind im Bereitstellungsumfang von Windows-Computern nicht enthalten. Nur erforderliche Updates sind im Bereitstellungsumfang enthalten. 

Bei der geplanten Bereitstellung wird definiert, welche Zielcomputer die jeweiligen Updates erhalten: entweder durch das explizite Angeben von Computern oder das Auswählen einer [Computergruppe](../log-analytics/log-analytics-computer-groups.md), die auf Protokollsuchen einer bestimmten Gruppe von Computern basiert. Außerdem geben Sie einen Zeitplan an, um einen Zeitraum zu genehmigen und festzulegen, in dem Updates installiert werden dürfen.

Updates werden mit Runbooks in Azure Automation installiert. Sie können diese Runbooks nicht anzeigen, und für die Runbooks ist keine Konfiguration erforderlich. Wenn eine Updatebereitstellung erstellt wird, erstellt die Updatebereitstellung einen Zeitplan, nach dem für die einbezogenen Computer zur angegebenen Zeit ein Masterrunbook für das Update gestartet wird. Das Masterrunbook startet ein untergeordnetes Runbook für jeden Agent, um die erforderlichen Updates zu installieren.

Wenn die Datums- bzw. Uhrzeitangabe der Updatebereitstellung erreicht ist, führen die Zielcomputer die Bereitstellung parallel aus. Vor der Installation wird ein Scan ausgeführt, um sicherzustellen, dass die Updates weiterhin erforderlich sind. Für WSUS-Clientcomputer tritt ein Fehler bei der Updatebereitstellung auf, wenn die Updates in WSUS nicht genehmigt sind.

Das Registrieren eines Computers für die Updateverwaltung in mehreren Log Analytics-Arbeitsbereichen (Multihosting) wird nicht unterstützt.

## <a name="clients"></a>Clients

### <a name="supported-client-types"></a>Unterstützte Clienttypen

In der folgenden Tabelle sind die unterstützten Betriebssysteme aufgeführt:

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Unterstützt nur Updatebewertungen.         |
|Windows Server 2008 R2 SP1 und höher     |.NET Framework 4.5.1 oder höher ist erforderlich. ([.NET Framework herunterladen](/dotnet/framework/install/guide-for-developers))<br/> WindowsPowerShell 4.0 oder höher ist erforderlich. ([WMF 4.0 herunterladen](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Für eine höhere Zuverlässigkeit wird Windows PowerShell 5.1 empfohlen.  ([WMF 5.1 herunterladen](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) und 7 (x64)      | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen. Für klassifizierungsbasiertes Patchen muss yum Sicherheitsdaten zurückgeben, über die CentOS nicht standardmäßig verfügt.         |
|Red Hat Enterprise 6 (x86/x64) und 7 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|SUSE Linux Enterprise Server 11 (x86/x64) und 12 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|Ubuntu 14.04 LTS und 16.04 LTS (x86/x64)      |Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.         |

### <a name="unsupported-client-types"></a>Nicht unterstützte Clienttypen

In der folgenden Tabelle werden die Betriebssysteme aufgelistet, die nicht unterstützt werden:

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows-Client     | Clientbetriebssysteme (z.B. Windows 7 und Windows 10) werden nicht unterstützt.        |
|Windows Server 2016 Nano Server     | Nicht unterstützt.       |

### <a name="client-requirements"></a>Clientanforderungen

#### <a name="windows"></a>Windows

Windows-Agents müssen für die Kommunikation mit einem WSUS-Server konfiguriert sein oder über Zugriff auf Microsoft Update verfügen. Sie können die Updateverwaltung mit System Center Configuration Manager verwenden. Weitere Informationen zu den Integrationsszenarien finden Sie unter [Integrieren von System Center Configuration Manager in die Updateverwaltung](oms-solution-updatemgmt-sccmintegration.md#configuration). Der [Windows-Agent](../log-analytics/log-analytics-agent-windows.md) ist erforderlich. Dieser Agent wird automatisch installiert, wenn Sie das Onboarding eines virtuellen Azure-Computer ausführen.

#### <a name="linux"></a>Linux

Für Linux muss der Computer über Zugriff auf ein Updaterepository verfügen. Das Updaterepository kann privat oder öffentlich sein. Für die Interaktion mit der Updateverwaltung ist TLS 1.1 oder TLS 1.2 erforderlich. Diese Lösung unterstützt keinen Log Analytics-Agent für Linux, der für die Berichterstattung an mehrere Log Analytics-Arbeitsbereiche konfiguriert ist.

Weitere Informationen zum Installieren des Log Analytics-Agents für Linux und zum Herunterladen der aktuellen Version finden Sie unter [Operations Management Suite-Agent für Linux](https://github.com/microsoft/oms-agent-for-linux). Informationen zur Installation des Log Analytics-Agents für Windows finden Sie unter [Operations Management Suite-Agent für Windows](../log-analytics/log-analytics-windows-agent.md).

## <a name="permissions"></a>Berechtigungen

Zum Erstellen und Verwalten von Updatebereitstellungen benötigen Sie bestimmte Berechtigungen. Weitere Informationen zu diesen Berechtigungen finden Sie unter [Rollenbasierter Zugriff: Updateverwaltung](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Lösungskomponenten

Die Lösung besteht aus den folgenden Ressourcen. Die Ressourcen werden Ihrem Automation-Konto hinzugefügt. Es handelt sich um direkt verbundene Agents oder um Agents in einer durch Operations Manager verbundenen Verwaltungsgruppe.

### <a name="hybrid-worker-groups"></a>Hybrid Worker-Gruppen

Nachdem Sie diese Lösung aktiviert haben, werden alle direkt mit dem Log Analytics-Arbeitsbereich verbundenen Windows-Computer automatisch als Hybrid Runbook Worker konfiguriert, um die in dieser Lösung enthaltenen Runbooks zu unterstützen.

Jeder von der Lösung verwaltete Windows-Computer wird im Bereich **Hybrid Worker-Gruppen** als **Hybrid Worker-Systemgruppe** für das Automation-Konto aufgeführt. Die Lösungen verwenden die Benennungskonvention *Hostname FQDN_GUID*. Es ist nicht möglich, diese Gruppen mit Runbooks in Ihrem Konto zu erreichen. Dieser Versuch führt zu einem Fehler. Diese Gruppen sind nur für die Unterstützung der Verwaltungslösung bestimmt.

Sie können die Windows-Computer einer Hybrid Runbook Worker-Gruppe in Ihrem Automation-Konto hinzufügen, um Automation-Runbooks zu unterstützen, wenn Sie für die Lösung und die Mitgliedschaft in der Hybrid Runbook Worker-Gruppe dasselbe Konto verwenden. Diese Funktionalität wurde in Version 7.2.12024.0 des Hybrid Runbook Worker hinzugefügt.

### <a name="management-packs"></a>Management Packs

Wenn Ihre System Center Operations Manager-Verwaltungsgruppe mit einem Log Analytics-Arbeitsbereich verbunden ist, werden in Operations Manager die folgenden Management Packs installiert. Diese Management Packs werden nach dem Hinzufügen dieser Lösung auch auf direkt verbundenen Windows-Computern installiert. Für diese Management Packs fällt kein Konfigurations- oder Verwaltungsaufwand an.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Update Deployment MP

Weitere Informationen zur Aktualisierung von Management Packs finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](../log-analytics/log-analytics-om-agents.md).

> [!NOTE]
> Damit Systeme mit dem Operations Manager-Agent vollständig durch die Updateverwaltung verwaltet werden können, muss der Agent auf den Microsoft Monitoring Agent aktualisiert werden. Informationen zum Aktualisieren des Agents finden Sie unter [Aktualisieren eines Operations Manager-Agents](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents).

### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Bestätigen der Integration von Nicht-Azure-Computern

Nach einigen Minuten können Sie eine der folgenden Protokollsuchen ausführen, um zu bestätigen, dass direkt verbundene Computer mit Log Analytics kommunizieren.

#### <a name="linux"></a>Linux

```
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

Auf einem Windows-Computer können Sie die folgenden Informationen überprüfen, um für den Agent die Log Analytics-Konnektivität zu bestätigen:

1. Öffnen Sie in der Systemsteuerung **Microsoft Monitoring Agent**. Auf der Registerkarte **Azure Log Analytics** wird vom Agent die folgende Meldung angezeigt: **The Microsoft Monitoring Agent has successfully connected to Log Analytics** (Für den Microsoft Monitoring Agent wurde die Verbindung mit Log Analytics erfolgreich hergestellt).
2. Öffnen Sie das Windows-Ereignisprotokoll. Navigieren Sie zu **Anwendungs- und Dienstprotokolle\Operations Manager**, und suchen Sie nach der Ereignis-ID 3000 und der Ereignis-ID 5002 aus der Quelle **Service Connector**. Mit diesen Ereignissen wird angegeben, dass für den Computer die Registrierung beim Log Analytics-Arbeitsbereich und die Konfiguration ausgeführt wurden.

Falls der Agent nicht mit dem OMS-Dienst kommunizieren kann und für die Kommunikation mit dem Internet über eine Firewall oder einen Proxyserver konfiguriert ist, sollten Sie für die Firewall bzw. den Proxyserver die Richtigkeit der Konfiguration sicherstellen. Weitere Informationen dazu, wie sie sicherstellen können, dass die Firewall oder der Proxyserver ordnungsgemäß konfiguriert ist, finden Sie unter [Netzwerkkonfiguration für den Windows-Agent](../log-analytics/log-analytics-agent-windows.md) oder [Netzwerkkonfiguration für den Linux-Agent](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Wenn Ihre Linux-Systeme für die Kommunikation mit einem Proxy oder Log Analytics-Gateway konfiguriert sind und Sie diese Lösung integrieren, aktualisieren Sie die Berechtigungen für *proxy.conf*, um der Gruppe „omiuser“ Leseberechtigungen für die Datei zu erteilen. Führen Sie zu diesem Zweck die folgenden Befehle aus:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Für neu hinzugefügte Linux-Agents wird der Status **Aktualisiert** angezeigt, nachdem eine Bewertung ausgeführt wurde. Dieser Vorgang kann bis zu sechs Stunden dauern.

Wenn Sie bestätigen möchten, dass eine Operations Manager-Verwaltungsgruppe mit Log Analytics kommuniziert, helfen Ihnen die Informationen unter [Überprüfen der Operations Manager-Integration für Log Analytics](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-log-analytics) weiter.

## <a name="data-collection"></a>Datensammlung

### <a name="supported-agents"></a>Unterstützte Agents

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden:

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
| --- | --- | --- |
| Windows-Agents |JA |Die Lösung sammelt Informationen zu Systemupdates aus Windows-Agents und initiiert dann die Installation von erforderlichen Updates. |
| Linux-Agents |JA |Die Lösung sammelt Informationen zu Systemupdates von Linux-Agents und initiiert dann die Installation von erforderlichen Updates für unterstützte Distributionen. |
| Operations Manager-Verwaltungsgruppe |JA |Die Lösung sammelt Informationen zu Systemupdates von Agents in einer verbundenen Verwaltungsgruppe.<br/>Es ist keine direkte Verbindung zwischen dem Operations Manager-Agent und Log Analytics erforderlich. Daten werden von der Verwaltungsgruppe an den Log Analytics-Arbeitsbereich weitergeleitet. |

### <a name="collection-frequency"></a>Sammlungshäufigkeit

Für jeden verwalteten Windows-Computer wird zwei Mal pro Tag ein Scanvorgang ausgeführt. Alle 15 Minuten wird die Windows-API aufgerufen, um den letzten Updatezeitpunkt abzufragen und zu ermitteln, ob sich der Status geändert hat. Wenn sich der Status geändert hat, wird eine Konformitätsprüfung eingeleitet. 

Für jeden verwalteten Linux-Computer wird alle drei Stunden ein Scanvorgang ausgeführt.

Es kann zwischen 30 Minuten und sechs Stunden dauern, bis im Dashboard aktualisierte Daten von verwalteten Computern angezeigt werden.

## <a name="viewing-update-assessments"></a>Anzeigen der Updatebewertungen

Wählen Sie in Ihrem Automation-Konto **Updateverwaltung** aus, um den Status Ihrer Computer anzuzeigen.

Diese Ansicht enthält Informationen zu Ihren Computern, zu fehlenden Updates, zu Updatebereitstellungen und geplanten Updatebereitstellungen. In der Spalte **KONFORMITÄT** können Sie sehen, wann der Computer zuletzt bewertet wurde. In der Spalte **UPDATE-AGENT-BEREITSCHAFT** können Sie die Integrität des Update-Agents anzeigen. Wenn ein Problem vorliegt, wählen Sie den Link aus, um zur Dokumentation für die Problembehandlung zu navigieren und zu erfahren, welche Schritte zum Beheben des Problems erforderlich sind.

Um eine Protokollsuche auszuführen, die Informationen zum Computer, zu Updates oder zu Bereitstellungen zurückgibt, wählen Sie das Element in der Liste aus. Der Bereich **Protokollsuche** wird mit einer Abfrage für das ausgewählte Element geöffnet:

![Standardansicht der Updateverwaltung](media/automation-update-management/update-management-view.png)

## <a name="install-updates"></a>Installieren von Updates

Nachdem die Updates für alle Linux- und Windows-Computer des Arbeitsbereichs bewertet wurden, können Sie die erforderlichen Updates installieren, indem Sie eine *Updatebereitstellung* erstellen. Eine Updatebereitstellung ist eine geplante Installation von erforderlichen Updates für mindestens einen Computer. Sie geben das Datum und die Uhrzeit für die Bereitstellung und einen Computer bzw. eine Gruppe von Computern an, die in den Umfang der Bereitstellung einbezogen werden sollen. Weitere Informationen zu Computergruppen finden Sie unter [Computergruppen in Log Analytics](../log-analytics/log-analytics-computer-groups.md).

 Wenn Sie Computergruppen in Ihre Updatebereitstellung einbinden, wird die Gruppenmitgliedschaft nur ein Mal beim Erstellen des Zeitplans ausgewertet. Nachfolgende Änderungen einer Gruppe werden nicht widergespiegelt. Um diese Art der Verwendung von [dynamischen Gruppen](#using-dynamic-groups) zu umgehen, werden diese Gruppen zum Zeitpunkt der Bereitstellung aufgelöst und durch eine Abfrage definiert.

> [!NOTE]
> Über Azure Marketplace bereitgestellte virtuelle Windows-Computer sind standardmäßig so konfiguriert, dass sie automatisch Updates von Windows Update Service erhalten. Dieses Verhalten ändert sich nicht, wenn Sie diese Lösung hinzufügen oder Ihrem Arbeitsbereich virtuelle Windows-Computer hinzufügen. Wenn Sie Updates mithilfe dieser Lösung nicht aktiv verwalten, gilt das Standardverhalten (Updates werden automatisch angewendet).

Damit unter Ubuntu keine Updates außerhalb der Wartungsfenster angewendet werden, konfigurieren Sie das „Unattended-Upgrade“-Paket erneut, um automatische Updates zu deaktivieren. Informationen zur Konfiguration dieses Pakets finden Sie im [Thema zu automatischen Updates im Ubuntu-Serverhandbuch](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Virtuelle Computer, die auf der Grundlage der über Azure Marketplace erhältlichen On-Demand-RHEL-Images (Red Hat Enterprise Linux) erstellt werden, werden für den Zugriff auf die in Azure bereitgestellte [Red Hat-Updateinfrastruktur (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) registriert. Alle anderen Linux-Distributionen müssen über das Onlinedateirepository der Distributionen gemäß den unterstützten Methoden der jeweiligen Distribution aktualisiert werden.

Wählen Sie zum Erstellen einer neuen Updatebereitstellung **Updatebereitstellung planen** aus. Der Bereich **Neue Updatebereitstellung** wird geöffnet. Geben Sie Werte für die Eigenschaften ein, die in der folgenden Tabelle beschrieben werden, und klicken Sie auf **Erstellen**:

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| NAME |Eindeutiger Name zum Identifizieren der Updatebereitstellung |
|Betriebssystem| Linux oder Windows|
| Zu aktualisierende Gruppen (Vorschau)|Definieren Sie eine Abfrage basierend auf einer Kombination aus Abonnement, Ressourcengruppen, Standorten und Tags, um eine dynamische Gruppe von Azure-VMs zu erstellen, die in Ihre Bereitstellung eingeschlossen werden sollen. Weitere Informationen finden Sie unter [Dynamische Gruppen](automation-update-management.md#using-dynamic-groups).|
| Zu aktualisierende Computer |Wählen Sie eine gespeicherte Suche oder eine importierte Gruppe aus, oder wählen Sie im Dropdownmenü „Computer“ und dann einzelne Computer aus. Bei Auswahl von **Computer** wird die Bereitschaft des Computers in der Spalte **BEREITSCHAFT DES UPDATE-AGENTS** angezeigt.</br> Weitere Informationen zu den verschiedenen Methoden zum Erstellen von Computergruppen in Log Analytics finden Sie unter [Computergruppen in Log Analytics](../log-analytics/log-analytics-computer-groups.md). |
|Updateklassifizierungen|Wählen Sie alle benötigten Updateklassifizierungen aus|
|Einschließen/Ausschließen von Updates|Daraufhin wird die Seite **Einschließen/ausschließen** geöffnet. Updates, die eingeschlossen oder ausgeschlossen werden sollen, befinden sich auf verschiedenen Registerkarten. Weitere Informationen zur Vorgehensweise beim Einschließen finden Sie unter [Verhalten beim Einschließen](automation-update-management.md#inclusion-behavior). |
|Zeitplaneinstellungen|Wählen Sie den Startzeitpunkt aus, und geben Sie unter „Wiederholung“ entweder „Einmal“ oder „Serie“ an|
| Vor und nach dem Vorgang auszuführende Skripts|Wählen Sie die Skripts aus, die vor und nach Ihrer Bereitstellung ausgeführt werden sollen.|
| Wartungsfenster |Festgelegte Minutenanzahl für Updates Der Wert darf nicht unter 30 Minuten und nicht über 6 Stunden betragen. |
| Neustartsteuerung| Legt fest, wie Neustarts behandelt werden sollen. Die verfügbaren Optionen lauten wie folgt:</br>Neu starten bei Bedarf (Standard)</br>Immer neu starten</br>Nie neu starten</br>Nur neu starten – Updates werden nicht installiert|

Updatebereitstellungen können ebenfalls programmgesteuert erstellt werden. Weitere Informationen zum Erstellen einer Updatebereitstellung mit der REST-API finden Sie unter [Softwareupdatekonfigurationen – Erstellen](/rest/api/automation/softwareupdateconfigurations/create). Es gibt auch ein Beispielrunbook, das zum Erstellen einer wöchentlichen Updatebereitstellung verwendet werden kann. Weitere Informationen zu diesem Runbook finden Sie unter [Erstellen einer wöchentlichen Updatebereitstellung für einen oder mehrere virtuelle Computer in einer Ressourcengruppe](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

## <a name="view-missing-updates"></a>Anzeigen fehlender Updates

Wählen Sie **Fehlende Updates** aus, um eine Liste der Updates anzuzeigen, die auf Ihrem Computer nicht vorhanden sind. Jedes Update wird aufgeführt und kann ausgewählt werden. Es werden Informationen zur Anzahl der Computer, die das Update benötigen, und zum Betriebssystem sowie ein Link zu weiteren Informationen angezeigt. Der Bereich **Protokollsuche** zeigt weitere Details zu den Updates an.

## <a name="view-update-deployments"></a>Anzeigen von Updatebereitstellungen

Wählen Sie die Registerkarte **Bereitstellung aktualisieren** aus, um die Liste mit den vorhandenen Updatebereitstellungen anzuzeigen. Wenn Sie in der Tabelle eine Updatebereitstellung auswählen, wird der Bereich **Updatebereitstellungsausführung** für diese Updatebereitstellung geöffnet.

![Übersicht über Ergebnisse der Updatebereitstellung](./media/automation-update-management/update-deployment-run.png)

Weitere Informationen zum Anzeigen einer Updatebereitstellung mithilfe der REST-API finden Sie unter [Softwarekonfigurationskonfigurationen – Ausführungen](/rest/api/automation/softwareupdateconfigurationruns).

## <a name="update-classifications"></a>Updateklassifizierungen

Die folgenden Tabellen enthalten eine Liste der Updateklassifizierungen in der Updateverwaltung sowie eine Definition für jede Klassifikation.

### <a name="windows"></a>Windows

|Classification  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates     | Ein Update für ein bestimmtes Problem, mit dem ein entscheidender, nicht sicherheitsrelevanter Fehler behoben wird.        |
|Sicherheitsupdates     | Ein Update für ein produktspezifisches, sicherheitsrelevantes Problem.        |
|Updaterollups     | Eine kumulative Gruppe von Hotfixes, die zur Vereinfachung der Bereitstellung gebündelt sind.        |
|Feature Packs     | Neue Produktfeatures, die nicht im Rahmen eines Produktreleases verteilt werden.        |
|Service Packs     | Eine kumulative Gruppe von Hotfixes, die auf eine Anwendung angewendet werden.        |
|Definitionsupdates     | Ein Update für virenbehaftete oder andere Definitionsdateien.        |
|Tools     | Ein Hilfsprogramm oder Feature, mit dem mindestens eine Aufgabe ausgeführt werden kann.        |
|Aktualisierungen     | Ein Update für eine Anwendung oder Datei, die zurzeit installiert ist.        |

### <a name="linux"></a>Linux

|Classification  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates und Sicherheitsupdates     | Updates für ein spezielles oder produktspezifisches, sicherheitsrelevantes Problem.         |
|Andere Updates     | Alle anderen Updates, die nicht kritisch sind oder bei denen es sich nicht um Sicherheitsupdates handelt.        |

Für Linux kann die Updateverwaltung zwischen kritischen und Sicherheitsupdates in der Cloud unterscheiden. Sie zeigt Bewertungsdaten aufgrund der Datenanreicherung in der Cloud an. Für das Patchen verwendet die Updateverwaltung Klassifizierungsdaten, die auf dem Computer verfügbar sind. Im Gegensatz zu anderen Distributionen verfügt CentOS nicht standardmäßig über diese Informationen. Wenn Sie CentOS-Computer für das Zurückgeben von Sicherheitsdaten für den folgenden Befehl konfiguriert haben, kann die Updateverwaltung basierend auf Klassifizierungen Patchvorgänge ausführen.

```bash
sudo yum -q --security check-update
```

Es gibt zurzeit keine unterstützte Methode zum Aktivieren nativer Klassifizierungsdatenverfügbarkeit unter CentOS. Zu diesem Zeitpunkt wird nur Unterstützung nach bestem Wissen für Kunden bereitgestellt, die dies möglicherweise auf eigene Weise aktiviert haben.

## <a name="firstparty-predownload"></a>Erstanbieterpatches und Vorabdownloads

Die Updateverwaltung verlässt sich zum Herunterladen und Installieren von Windows-Updates auf Windows Update. Daher werden viele der Einstellungen von Windows Update berücksichtigt. Wenn Sie Einstellungen verwenden, um Nicht-Windows-Updates zu aktivieren, verwaltet die Updateverwaltung diese Updates ebenfalls. Wenn Sie das Herunterladen der Updates vor der Updatebereitstellung aktivieren, können Bereitstellungen schneller ausgeführt werden und damit eher das Wartungsfenster einhalten.

### <a name="pre-download-updates"></a>Vorabdownload von Updates

Um das automatische Herunterladen von Updates in einer Gruppenrichtlinie zu konfigurieren, können Sie die Einstellung [Automatische Updates konfigurieren](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#BKMK_comp5) auf **3** festlegen. Damit werden die erforderlichen Updates im Hintergrund heruntergeladen, aber nicht installiert. Auf diese Weise behält die Updateverwaltung die Kontrolle über die Zeitpläne, während die Updates gleichzeitig außerhalb des Wartungsfensters für die Updateverwaltung heruntergeladen werden können. Dies kann Fehler durch ein **Überschreiten des Wartungsfensters** bei der Updateverwaltung verhindern.

Sie können dies auch mit PowerShell festlegen. Führen Sie dazu den folgenden PowerShell-Befehl auf einem System aus, auf dem Sie Updates automatisch herunterladen möchten.

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

### <a name="enable-updates-for-other-microsoft-products"></a>Aktivieren von Updates für andere Microsoft-Produkte

Standardmäßig stellt Windows Update nur Updates für Windows bereit. Wenn Sie **Give me updates for other Microsoft products when I update Windows** (Updates für andere Microsoft-Produkte beim Update von Windows einschließen) aktivieren, erhalten Sie auch Updates für andere Produkte, einschließlich Sicherheitspatches für SQL Server und anderer Software von Erstanbietern. Diese Option kann nicht über Gruppenrichtlinien konfiguriert werden. Führen Sie den folgenden PowerShell-Befehl auf den Systemen aus, auf denen Sie Patches anderer Erstanbieter aktivieren möchten. Die Updateverwaltung berücksichtigt diese Einstellung.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="ports"></a>Netzwerkplanung

Die folgenden Adressen sind speziell für die Updateverwaltung erforderlich. Die Kommunikation mit diesen Adressen erfolgt über Port 443.

|Azure – Öffentlich  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|*.azure-automation.net|*.azure-automation.us|

Weitere Informationen zu Ports, die für den Hybrid Runbook Worker erforderlich sind, finden Sie unter [Ports für Hybrid Worker-Rollen](automation-hybrid-runbook-worker.md#hybrid-worker-role).

Es wird empfohlen, beim Definieren von Ausnahmen die aufgeführten Adressen zu verwenden. Für IP-Adressen können Sie die [IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) herunterladen. Diese Datei mit den jeweils aktuellen bereitgestellten Bereichen und allen anstehenden Änderungen an den IP-Adressbereichen wird wöchentlich veröffentlicht.

## <a name="search-logs"></a>Protokollsuche

Zusätzlich zu den Details, die im Azure-Portal bereitgestellt werden, können Sie auch die Protokolle durchsuchen. Wählen Sie auf den Lösungsseiten **Log Analytics** aus. Der Bereich **Protokollsuche** wird geöffnet.

Weitere Informationen (z.B. zum Anpassen der Abfragen oder Verwenden der Abfragen mit anderen Clients) finden Sie hier: [Dokumentation zur Such-API von Log Analytics](
https://dev.loganalytics.io/).

### <a name="sample-queries"></a>Beispielabfragen

Die folgenden Abschnitte enthalten Beispielprotokollabfragen für Updatedatensätze, die von dieser Lösung erfasst werden:

#### <a name="single-azure-vm-assessment-queries-windows"></a>Bewertungsabfragen für einzelne virtuelle Azure-Computer (Windows)

Ersetzen Sie den VMUUID-Wert durch die VM-GUID des virtuellen Computers, den Sie abfragen. Sie können die VMUUID ermitteln, die verwendet werden soll, indem Sie die folgende Abfrage in Log Analytics ausführen: `Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Zusammenfassung fehlender Updates

```
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"b08d5afa-1471-4b52-bd95-a44fea6e4ca8"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security"
```

##### <a name="missing-updates-list"></a>Liste fehlender Updates

```
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"8bf1ccc6-b6d3-4a0b-a643-23f346dfdf82"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

#### <a name="single-azure-vm-assessment-queries-linux"></a>Bewertungsabfragen für einzelne virtuelle Azure-Computer (Linux)

Für einige Linux-Distributionen besteht ein Konflikt der [Bytereihenfolge](https://en.wikipedia.org/wiki/Endianness) mit dem VMUUID-Wert, der auf Azure Resource Manager und die in Log Analytics gespeicherten Daten zurückzuführen ist. Die folgende Abfrage überprüft, ob eine Übereinstimmung für einen der Endianness-Werte vorliegt. Ersetzen Sie die VMUUID-Werte durch das big-endian- bzw. little-endian-Format der GUID, um die Ergebnisse ordnungsgemäß zurückzugeben. Sie können die VMUUID ermitteln, die verwendet werden soll, indem Sie die folgende Abfrage in Log Analytics ausführen: `Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Zusammenfassung fehlender Updates

```
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="missing-updates-list"></a>Liste fehlender Updates

```
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl

```

#### <a name="multi-vm-assessment-queries"></a>Bewertungsabfragen für mehrere virtuelle Computer

##### <a name="computers-summary"></a>Computerzusammenfassung

```
Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Approved, Optional, Classification) by SourceComputerId, UpdateID
    | distinct SourceComputerId, Classification, UpdateState, Approved, Optional
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed" and (Optional==false or Classification has "Critical" or Classification has "Security") and Approved!=false, iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity
| union (Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by SourceComputerId, Product, ProductArch
    | distinct SourceComputerId, Classification, UpdateState
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed", iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity)
| summarize assessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity>-1), notAssessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==-1), computersNeedCriticalUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==4), computersNeedSecurityUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==2), computersNeeedOtherUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==1), upToDateComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==0)
| summarize assessedComputersCount=sum(assessedComputersCount), computersNeedCriticalUpdatesCount=sum(computersNeedCriticalUpdatesCount),  computersNeedSecurityUpdatesCount=sum(computersNeedSecurityUpdatesCount), computersNeeedOtherUpdatesCount=sum(computersNeeedOtherUpdatesCount), upToDateComputersCount=sum(upToDateComputersCount), notAssessedComputersCount=sum(notAssessedComputersCount)
| extend allComputersCount=assessedComputersCount+notAssessedComputersCount


```

##### <a name="missing-updates-summary"></a>Zusammenfassung fehlender Updates

```
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| union (Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification )
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security"
```

##### <a name="computers-list"></a>Computerliste

```
Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Product, Computer, ComputerEnvironment) by SourceComputerId, Product, ProductArch
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed"), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed"), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed"), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2)
| union(Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, Optional, Approved, Computer, ComputerEnvironment) by Computer, SourceComputerId, UpdateID
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed" and Approved!=false), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed" and Approved!=false), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed" and Optional==false and Approved!=false), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2) )
| order by ComplianceOrder asc, missingCriticalUpdatesCount desc, missingSecurityUpdatesCount desc, missingOtherUpdatesCount desc, displayName asc
| project-away ComplianceOrder
```

##### <a name="missing-updates-list"></a>Liste fehlender Updates

```
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| union(Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2)
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

## <a name="using-dynamic-groups"></a>Mithilfe von dynamischen Gruppen (Vorschau)

Die Updateverwaltung bietet die Möglichkeit, eine dynamische Gruppe von virtuellen Azure-Computern als Ziel für Updatebereitstellungen zu verwenden. Diese Gruppen werden durch eine Abfrage definiert. Wenn eine Updatebereitstellung beginnt, werden die Mitglieder dieser Gruppe ausgewertet. Wenn Sie Ihre Abfrage definieren, können die folgenden Elemente zusammen verwendet werden, um die dynamische Gruppe aufzufüllen

* Abonnement
* Ressourcengruppen
* Standorte
* Tags

![Auswählen von Gruppen](./media/automation-update-management/select-groups.png)

Um eine Vorschau der Ergebnisse einer dynamischen Gruppe anzuzeigen, klicken Sie auf die Schaltfläche **Vorschau**. Diese Vorschau zeigt die Gruppenmitgliedschaft zu diesem Zeitpunkt an. In diesem Beispiel suchen wir nach Computern, bei denen das Tag **Role** den Wert **BackendServer** aufweist. Wenn dieses Tag weiteren Computern hinzugefügt wird, werden sie bei zukünftigen Bereitstellungen dieser Gruppe hinzugefügt.

![Anzeigen einer Vorschau von Gruppen](./media/automation-update-management/preview-groups.png)

## <a name="integrate-with-system-center-configuration-manager"></a>Integrieren in System Center Configuration Manager

Kunden, die in System Center Configuration Manager investiert haben, um PCs, Server und mobile Geräte zu verwalten, profitieren auch von der Leistungsstärke und dem Funktionsumfang von Configuration Manager bei der Verwaltung von Softwareupdates. Configuration Manager ist Teil ihres Softwareupdateverwaltungs-Zyklus.

Wie Sie die Verwaltungslösung in System Center Configuration Manager integrieren, erfahren Sie unter [Integrieren von System Center Configuration Manager und Updateverwaltung](oms-solution-updatemgmt-sccmintegration.md).

## <a name="inclusion-behavior"></a>Einschlussverhalten

Durch das Einschließen von Updates können Sie die Anwendung bestimmter Updates festlegen. Alle enthaltenen Patches oder Pakete werden installiert. Wenn Patches oder Pakete enthalten sind und auch eine Klassifizierung ausgewählt wurde, werden sowohl die eingeschlossenen Elemente als auch Elemente, die der Klassifizierung entsprechen, installiert.

Es ist wichtig zu wissen, dass Ausschlüsse eine höhere Priorität als Einschlüsse haben. Wenn Sie beispielsweise die Ausschlussregel `*` definieren, werden keine Patches oder Pakete installiert, da sie alle ausgeschlossen wurden. Wenn auf Linux-Computern ein Paket eingeschlossen wird, das jedoch eine Abhängigkeit zu einem ausgeschlossenen Paket aufweist, wird das Paket nicht installiert.

## <a name="patch-linux-machines"></a>Patchen von Linux-Computern

In den folgenden Abschnitten werden mögliche Probleme beim Patchen von Linux-Computern erläutert.

### <a name="unexpected-os-level-upgrades"></a>Unerwartete Upgrades auf Betriebssystemebene

Bei einigen Linux-Varianten (z.B. Red Hat Enterprise Linux) können Upgrades auf Betriebssystemebene über Pakete erfolgen. Dies kann ggf. zur Ausführung der Updateverwaltung führen, wodurch sich die Versionsnummer des Betriebssystems ändert. Dieses Verhalten ist beabsichtigt, da die Updateverwaltung die gleichen Methoden für Updatepakete verwendet, die auch ein Administrator lokal auf dem Linux-Computer nutzt.

Um zu vermeiden, dass die Betriebssystemversion durch die Ausführung der Updateverwaltung aktualisiert wird, verwenden Sie das **Ausschluss**-Feature.

In Red Hat Enterprise Linux lautet der Name des auszuschließenden Pakets „redhat-release-server.x86_64“.

![Auszuschließende Pakete für Linux](./media/automation-update-management/linuxpatches.png)

### <a name="critical--security-patches-arent-applied"></a>Kritische/Sicherheitspatches werden nicht angewendet

Beim Bereitstellen von Updates für einen Linux-Computer können Sie Updateklassifizierungen auswählen. Dadurch werden die angewandten Updates auf den Computer gefiltert, der die definierten Kriterien erfüllt. Dieser Filter wird lokal auf dem Computer angewendet, wenn das Update bereitgestellt wird.

Da die Updateverwaltung die Updateergänzung in der Cloud ausführt, werden einige Updates in der Updateverwaltung möglicherweise als Update mit Sicherheitsauswirkung gekennzeichnet, obwohl der lokale Computer nicht über diese Informationen verfügt. Daher gibt es beim Anwenden kritischer Updates auf einem Linux-Computer möglicherweise Updates, die nicht als Update mit Sicherheitsauswirkung auf diesen Computer gekennzeichnet sind. Diese Updates werden dann nicht angewendet.

Allerdings wird die Updateverwaltung diesen Computer wahrscheinlich weiterhin als nicht kompatibel melden, da sie über zusätzliche Informationen zum relevanten Update verfügt.

Das Bereitstellen von Updates nach einer Updateklassifizierung funktioniert unter CentOS nicht standardmäßig. Für SUSE kann die Auswahl *nur* von „Other Updates“ als Klassifizierung möglicherweise dazu führen, dass einige Sicherheitsupdates installiert werden, wenn zuerst Sicherheitsupdates im Zusammenhang mit zypper (Paket-Manager) oder dessen Abhängigkeiten erforderlich sind. Dies ist eine Einschränkung von zypper. In einigen Fällen müssen Sie die Updatebereitstellung u.U. erneut ausführen. Überprüfen Sie das Updateprotokoll diesbezüglich.

## <a name="troubleshoot"></a>Problembehandlung

Informationen zur Problembehandlung der Updateverwaltung finden Sie unter [Problembehandlung der Updateverwaltung](troubleshoot/update-management.md).

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem Tutorial fort, um zu erfahren, wie Updates für Ihre virtuellen Windows-Computer verwaltet werden.

> [!div class="nextstepaction"]
> [Verwalten von Updates und Patches für Ihre virtuellen Azure Windows-Computer](automation-tutorial-update-management.md)

* Verwenden Sie die Protokollsuche in [Log Analytics](../log-analytics/log-analytics-log-searches.md), um ausführliche Daten zu Updates anzuzeigen.
* [Erstellen Sie Warnungen](../log-analytics/log-analytics-alerts.md), wenn kritische Updates für Computer als fehlend erkannt werden oder für einen Computer automatische Updates deaktiviert sind.

* Weitere Informationen zur Interaktion mit der Updateverwaltung über die REST-API finden Sie unter [Softwareupdatekonfigurationen](/rest/api/automation/softwareupdateconfigurations).
