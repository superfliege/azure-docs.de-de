---
title: Lösung für die Updateverwaltung in Azure
description: In diesem Artikel soll vermittelt werden, wie Sie diese Lösung zum Verwalten von Updates für Ihre Windows- und Linux-Computer verwenden.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 04/23/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: c095576ccce7e32850c3fb2daf8303a0d6e957bc
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34070292"
---
# <a name="update-management-solution-in-azure"></a>Lösung für die Updateverwaltung in Azure

Mithilfe der Lösung für die Updateverwaltung in Azure Automation können Sie Betriebssystemupdates für Ihre Windows- und Linux-Computer verwalten, die in Azure, in lokalen Umgebungen oder bei anderen Cloudanbietern bereitgestellt wurden. Sie können den Status der verfügbaren Updates auf allen Agent-Computern schnell auswerten und die Installation der für den Server erforderlichen Updates initiieren.

Die Updateverwaltung für virtuelle Computer kann direkt in Ihrem Azure Automation-Konto aktiviert werden.
Informationen zum Aktivieren der Updateverwaltung für virtuelle Computer über das Automation-Konto finden Sie unter [Verwalten von Updates für mehrere virtuelle Azure-Computer](manage-update-multi.md). Sie können die Updateverwaltung im Azure-Portal auf der Seite des virtuellen Computers auch für einen einzelnen virtuellen Computer aktivieren. Dieses Szenario ist sowohl für virtuelle [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management)- als auch [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management)-Computer verfügbar.

## <a name="solution-overview"></a>Lösungsübersicht

Verwenden Sie für Computer, die mit der Updateverwaltung verwaltet werden, die folgenden Konfigurationen, um Bewertungen und Updatebereitstellungen durchzuführen:

* Microsoft Monitoring Agent für Windows oder Linux
* PowerShell Desired State Configuration (DSC) für Linux
* Automation Hybrid Runbook Worker
* Microsoft Update oder Windows Server Update Services für Windows-Computer

Das folgende Diagramm enthält eine konzeptionelle Darstellung des Verhaltens und Datenflusses, und zeigt, wie die Lösung alle verbundenen Windows Server- und Linux-Computer eines Arbeitsbereichs bewertet und Sicherheitsupdates darauf anwendet.

![Ablauf des Updateverwaltungsprozesses](media/automation-update-management/update-mgmt-updateworkflow.png)

Nachdem ein Computer einen Scanvorgang durchgeführt hat, um die Konformität für das Update zu überprüfen, leitet der Agent die Informationen gesammelt an Log Analytics weiter. Auf einem Windows-Computer wird der Konformitätsscan standardmäßig alle zwölf Stunden durchgeführt. Zusätzlich zum Scanzeitplan wird der Update-Konformitätsscan innerhalb von 15 Minuten initiiert, wenn der Microsoft Monitoring Agent (MMA) neu gestartet wird – jeweils vor und nach der Updateinstallation. Bei einem Linux-Computer wird der Konformitätsscan standardmäßig alle drei Stunden durchgeführt, und bei einem MMA-Neustart erfolgt innerhalb von 15 Minuten ein Konformitätsscan.

Die Lösung meldet basierend darauf, welche Quelle für die Synchronisierung konfiguriert ist, wie aktuell der Computer ist. Wenn der Windows-Computer für das Senden von Meldungen an WSUS konfiguriert ist, können sich die Ergebnisse von den angezeigten Microsoft Update-Ergebnissen unterscheiden. Dies hängt davon ab, wann WSUS zuletzt mit Microsoft Update synchronisiert wurde. Dasselbe gilt für Linux-Computer, die für das Melden an ein lokales Repository konfiguriert sind, anstatt an ein öffentliches Repository.

> [!NOTE]
> Für die Updateverwaltung müssen bestimmte URLs und Ports aktiviert werden, damit der Dienst ordnungsgemäß Berichte erstellt. Informationen zu diesen Anforderungen finden Sie unter [Netzwerkplanung für Hybrid Worker](automation-hybrid-runbook-worker.md#network-planning).

Sie können Softwareupdates auf Computern bereitstellen und installieren, für die die Updates erforderlich sind, indem Sie einen geplante Bereitstellung erstellen. Updates, die als *Optional* klassifiziert sind, sind im Bereitstellungsumfang von Windows-Computern nicht enthalten, sondern nur erforderliche Updates. Bei der geplanten Bereitstellung wird definiert, welche Zielcomputer die jeweiligen Updates erhalten – entweder durch das explizite Angeben von Computern oder das Auswählen einer [Computergruppe](../log-analytics/log-analytics-computer-groups.md), die auf Protokollsuchen einer bestimmten Gruppe von Computern basiert. Außerdem geben Sie einen Zeitplan an, um einen Zeitraum zu genehmigen und festzulegen, in dem Updates installiert werden dürfen. Updates werden mit Runbooks in Azure Automation installiert. Sie können diese Runbooks nicht anzeigen, und für die Runbooks ist keine Konfiguration erforderlich. Bei der Erstellung einer Updatebereitstellung wird ein Zeitplan erstellt, nach dem für die einbezogenen Computer zur angegebenen Zeit ein Masterrunbook für das Update gestartet wird. Über dieses Masterrunbook wird ein untergeordnetes Runbook auf jedem Agent gestartet, mit dem die Installation von erforderlichen Updates durchgeführt wird.

Wenn die Datums- bzw. Uhrzeitangabe der Updatebereitstellung erreicht ist, führt der Zielcomputer die Bereitstellung parallel aus. Zuerst wird ein Scanvorgang durchgeführt, um sicherzustellen, dass die Updates weiterhin erforderlich sind, und anschließend werden sie installiert. Für WSUS-Clientcomputer schlägt die Updatebereitstellung fehl, wenn die Updates in WSUS nicht genehmigt sind.

## <a name="clients"></a>Clients

### <a name="supported-client-types"></a>Unterstützte Clienttypen

In der folgenden Tabelle sind die unterstützten Betriebssysteme aufgeführt:

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Unterstützt nur Updatebewertungen         |
|Windows Server 2008 R2 SP1 und höher     |Windows PowerShell 4.0 oder höher erforderlich ([WMF 4.0 herunterladen](https://www.microsoft.com/download/details.aspx?id=40855)).</br> Für eine höhere Zuverlässigkeit wird Windows PowerShell 5.1 ([WMF 5.1 herunterladen](https://www.microsoft.com/download/details.aspx?id=54616)) empfohlen.         |
|CentOS 6 (x86/x64) und 7 (x64)      | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|Red Hat Enterprise 6 (x86/x64) und 7 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|SUSE Linux Enterprise Server 11 (x86/x64) und 12 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)      |Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.         |

### <a name="unsupported-client-types"></a>Nicht unterstützte Clienttypen

In der folgenden Tabelle werden die Betriebssysteme aufgelistet, die nicht unterstützt werden:

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows-Client     | Clientbetriebssysteme (Windows 7, Windows 10 usw.) werden nicht unterstützt.        |
|Windows Server 2016 Nano Server     | Nicht unterstützt       |

### <a name="client-requirements"></a>Clientanforderungen

#### <a name="windows"></a>Windows

Windows-Agents müssen entweder für die Kommunikation mit einem WSUS-Server (Windows Server Update Services) konfiguriert sein oder über Zugriff auf Microsoft Update verfügen. Die Updateverwaltung kann mit System Center Configuration Manager verwendet werden. Weitere Informationen zu den Integrationsszenarien erhalten Sie unter [Integrieren von System Center Configuration Manager in die Updateverwaltung](oms-solution-updatemgmt-sccmintegration.md#configuration). Der [Windows-Agent](../log-analytics/log-analytics-agent-windows.md) ist erforderlich. Dieser Agent wird automatisch installiert, wenn Sie einen virtuellen Azure-Computer integrieren.

#### <a name="linux"></a>Linux

Bei Linux muss der Computer Zugriff auf ein Updaterepository haben, das entweder privat oder öffentlich sein kann. Ein OMS-Agent für Linux, der für die Berichterstattung an mehrere Log Analytics-Arbeitsbereiche konfiguriert ist, wird für diese Lösung nicht unterstützt.

Weitere Informationen zum Installieren des OMS-Agents für Linux und zum Herunterladen der aktuellen Version finden Sie unter [Operations Management Suite-Agent für Linux](https://github.com/microsoft/oms-agent-for-linux). Informationen zur Installation des OMS-Agents für Windows finden Sie unter [Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure](../log-analytics/log-analytics-windows-agent.md).

## <a name="permissions"></a>Berechtigungen

Zum Erstellen und Verwalten von Updatebereitstellungen benötigen Sie bestimmte Berechtigungen. Weitere Informationen zu diesen Berechtigungen finden Sie unter [Rollenbasierter Zugriff – Updateverwaltung](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Lösungskomponenten

Diese Lösung besteht aus den folgenden Ressourcen, die Ihrem Automation-Konto hinzugefügt werden, und direkt verbundenen Agents oder mit Operations Manager verbundenen Verwaltungsgruppen.

### <a name="hybrid-worker-groups"></a>Hybrid Worker-Gruppen

Nachdem Sie diese Lösung aktiviert haben, werden alle direkt mit dem Log Analytics-Arbeitsbereich verbundenen Windows-Computer automatisch als Hybrid Runbook Worker konfiguriert, um die in dieser Lösung enthaltenen Runbooks zu unterstützen. Jeder von der Lösung verwaltete Windows-Computer wird auf der Seite mit den Hybrid Worker-Gruppen als Hybrid Worker-Systemgruppe für das Automation-Konto aufgeführt. Die Benennungskonvention lautet *Hostname FQDN_GUID*. Es ist nicht möglich, diese Gruppen mit Runbooks in Ihrem Konto zu erreichen, da der Vorgang fehlschlägt. Diese Gruppen sind nur für die Unterstützung der Verwaltungslösung bestimmt.

Aber Sie können die Windows-Computer einer Hybrid Runbook Worker-Gruppe in Ihrem Automation-Konto hinzufügen, um Automation-Runbooks zu unterstützen, solange Sie sowohl für die Lösung als auch die Mitgliedschaft in der Hybrid Runbook Worker-Gruppe dasselbe Konto verwenden. Diese Funktionalität wurde Version 7.2.12024.0 des Hybrid Runbook Worker hinzugefügt.

### <a name="management-packs"></a>Management Packs

Wenn Ihre System Center Operations Manager-Verwaltungsgruppe mit einem Log Analytics-Arbeitsbereich verbunden ist, werden in Operations Manager die folgenden Management Packs installiert. Diese Management Packs werden nach dem Hinzufügen dieser Lösung auch auf direkt verbundenen Windows-Computern installiert. Für diese Management Packs fällt kein Konfigurations- oder Verwaltungsaufwand an.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Update Deployment MP

Weitere Informationen zur Aktualisierung von Management Packs finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](../log-analytics/log-analytics-om-agents.md).

### <a name="confirm-non-azure-machines-are-onboarded"></a>Bestätigen der Integration von Nicht-Azure-Computern

Nach einigen Minuten können Sie die folgende Protokollsuche durchführen, um zu bestätigen, dass direkt verbundene Computer mit Log Analytics kommunizieren:

#### <a name="linux"></a>Linux

```
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table`
```

Auf einem Windows-Computer können Sie Folgendes überprüfen, um für den Agent die Log Analytics-Konnektivität zu bestätigen:

1. Öffnen Sie in der Systemsteuerung den Microsoft Monitoring Agent. Auf der Registerkarte **Azure Log Analytics** wird vom Agent folgende Meldung angezeigt: **The Microsoft Monitoring Agent has successfully connected to Log Analytics** (Für den Microsoft Monitoring Agent wurde die Verbindung mit Log Analytics erfolgreich hergestellt).
2. Öffnen Sie das Windows-Ereignisprotokoll, navigieren Sie zu **Anwendungs- und Dienstprotokolle\Operations Manager**, und suchen Sie nach der Ereignis-ID 3000 und 5002 (Service Connector-Quellinstanz). Mit diesen Ereignissen wird angegeben, dass für den Computer die Registrierung beim Log Analytics-Arbeitsbereich und die Konfiguration durchgeführt wurden.

Falls der Agent nicht mit Log Analytics kommunizieren kann und für die Kommunikation mit dem Internet über eine Firewall oder einen Proxyserver konfiguriert ist, sollten Sie für die Firewall bzw. den Proxyserver die Richtigkeit der Konfiguration sicherstellen, indem Sie die Informationen zur [Netzwerkkonfiguration für Windows-Agent](../log-analytics/log-analytics-agent-windows.md) bzw. die Informationen zur [Netzwerkkonfiguration für Linux-Agent](../log-analytics/log-analytics-agent-linux.md) durchgehen.

> [!NOTE]
> Wenn Ihre Linux-Systeme für die Kommunikation mit einem Proxy oder OMS-Gateway konfiguriert sind und Sie diese Lösung integrieren, aktualisieren Sie die *proxy.conf*-Berechtigungen, um der Gruppe „omiuser“ Leseberechtigungen für die Datei zu erteilen. Führen Sie hierzu die folgenden Befehle aus: `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Für neu hinzugefügte Linux-Agents wird der Status **Aktualisiert** angezeigt, nachdem eine Bewertung durchgeführt wurde. Dieser Vorgang kann bis zu sechs Stunden dauern.

Wenn Sie bestätigen möchten, dass eine Operations Manager-Verwaltungsgruppe mit Log Analytics kommuniziert, helfen Ihnen die Informationen unter [Überprüfen der Operations Manager-Integration für Log Analytics](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-log-analytics) weiter.

## <a name="data-collection"></a>Datensammlung

### <a name="supported-agents"></a>Unterstützte Agents

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden.

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
| --- | --- | --- |
| Windows-Agents |Ja |Die Lösung sammelt Informationen zu Systemupdates aus Windows-Agents und initiiert die Installation von erforderlichen Updates. |
| Linux-Agents |Ja |Die Lösung sammelt Informationen zu Systemupdates von Linux-Agents und initiiert die Installation von erforderlichen Updates für unterstützte Distributionen. |
| Operations Manager-Verwaltungsgruppe |Ja |Die Lösung sammelt Informationen zu Systemupdates von Agents in einer verbundenen Verwaltungsgruppe.</br>Es ist keine direkte Verbindung von Operations Manager mit Log Analytics erforderlich. Daten werden von der Verwaltungsgruppe an den Log Analytics-Arbeitsbereich weitergeleitet. |

### <a name="collection-frequency"></a>Sammlungshäufigkeit

Für jeden verwalteten Windows-Computer wird zweimal pro Tag ein Scanvorgang durchgeführt. Alle 15 Minuten wird die Windows-API aufgerufen, um den letzten Updatezeitpunkt abzufragen und zu ermitteln, ob sich der Status geändert hat. Wenn ja, wird ein Konformitätsscan initiiert. Für jeden verwalteten Linux-Computer wird alle drei Stunden ein Scanvorgang durchgeführt.

Es kann zwischen 30 Minuten und sechs Stunden dauern, bis im Dashboard aktualisierte Daten von verwalteten Computern angezeigt werden.

## <a name="viewing-update-assessments"></a>Anzeigen von Updatebewertungen

Klicken Sie in Ihrem Automation-Konto auf **Updateverwaltung**, um den Status Ihrer Computer anzuzeigen.

Diese Ansicht enthält Informationen zu Ihren Computern, zu fehlenden Updates, zu Updatebereitstellungen und geplanten Updatebereitstellungen.

Sie können eine Protokollsuche ausführen, die Informationen zum Computer, zu Updates oder zu Bereitstellungen zurückgibt, indem Sie das Element in der Liste auswählen. Daraufhin wird die Seite **Protokollsuche** mit einer Abfrage für das ausgewählte Element geöffnet.

## <a name="installing-updates"></a>Installieren von Updates

Nachdem die Updates für alle Linux- und Windows-Computer des Arbeitsbereichs bewertet wurden, installieren Sie die erforderlichen Updates, indem Sie eine *Updatebereitstellung* erstellen. Eine Updatebereitstellung ist eine geplante Installation von erforderlichen Updates für mindestens einen Computer. Sie geben das Datum und die Uhrzeit für die Bereitstellung und einen Computer bzw. eine Gruppe von Computern an, die in den Umfang der Bereitstellung einbezogen werden sollen. Weitere Informationen zu Computergruppen finden Sie unter [Computergruppen in Log Analytics](../log-analytics/log-analytics-computer-groups.md). Wenn Sie Computergruppen in Ihre Updatebereitstellung einbinden, wird die Gruppenmitgliedschaft nur einmal beim Erstellen des Zeitplans ausgewertet. Nachfolgende Änderungen einer Gruppe werden nicht widergespiegelt. Löschen Sie die geplante Updatebereitstellung, und erstellen Sie sie neu, um dieses Problem zu umgehen.

> [!NOTE]
> Über den Azure Marketplace bereitgestellte Windows-VMs sind standardmäßig so konfiguriert, dass sie automatisch Updates von Windows Update Service erhalten. Dieses Verhalten ändert sich nicht, nachdem Sie Ihrem Arbeitsbereich diese Lösung oder Windows-VMs hinzugefügt haben. Wenn Sie Updates für diese Lösung nicht aktiv verwalten, gilt das Standardverhalten (Updates werden automatisch angewendet).

Damit unter Ubuntu keine Updates außerhalb der Wartungsfenster angewendet werden, konfigurieren Sie das „Unattended-Upgrade“-Paket erneut, um automatische Updates zu deaktivieren. Informationen zu dieser Konfiguration finden Sie im [Thema zu automatischen Updates im Ubuntu-Serverhandbuch](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Für virtuelle Computer, die basierend auf den über den Azure Marketplace erhältlichen On-Demand-RHEL-Images (Red Hat Enterprise Linux) erstellt werden, werden sie für den Zugriff auf die in Azure bereitgestellte [Red Hat-Updateinfrastruktur (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) registriert. Alle anderen Linux-Distributionen müssen über das Onlinedateirepository der Distributionen gemäß den unterstützten Methoden aktualisiert werden.

## <a name="viewing-missing-updates"></a>Anzeigen fehlender Updates

Klicken Sie auf **Fehlende Updates**, um eine Liste der Updates anzuzeigen, die auf Ihrem Computer nicht vorhanden sind. Jedes Update wird aufgeführt und enthält Informationen zur Anzahl der Computer, die das Update benötigen, und zum Betriebssystem sowie einen Link für weitere Informationen. Jedes Update kann ausgewählt werden. Daraufhin wird die Seite **Protokollsuche** angezeigt, die weitere Details zu den Updates enthält.

## <a name="viewing-update-deployments"></a>Anzeigen von Updatebereitstellungen

Klicken Sie auf die Registerkarte **Bereitstellung aktualisieren**, um die Liste mit den vorhandenen Updatebereitstellungen anzuzeigen. Wenn Sie in der Tabelle auf eine Updatebereitstellung klicken, wird die Seite **Updatebereitstellungsausführung** für diese Updatebereitstellung geöffnet.

![Übersicht über Ergebnisse der Updatebereitstellung](./media/automation-update-management/update-deployment-run.png)

## <a name="creating-an-update-deployment"></a>Erstellen einer Updatebereitstellung

Erstellen Sie eine neue Updatebereitstellung, indem Sie auf die Schaltfläche **Updatebereitstellung planen** oben im Bildschirm klicken, um die Seite **Neue Updatebereitstellung** zu öffnen. Für die Eigenschaften in der folgenden Tabelle müssen Sie Werte angeben:

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| NAME |Eindeutiger Name zum Identifizieren der Updatebereitstellung |
|Betriebssystem| Linux oder Windows|
| Zu aktualisierende Computer |Wählen Sie eine gespeicherte Suche aus, oder wählen Sie im Dropdownmenü „Computer“ und dann einzelne Computer aus |
|Updateklassifizierungen|Wählen Sie alle benötigten Updateklassifizierungen aus|
|Auszuschließende Updates|Geben Sie alle auszuschließenden KBs ohne das Präfix "KB" ein|
|Zeitplaneinstellungen|Wählen Sie den Startzeitpunkt aus, und geben Sie unter „Wiederholung“ entweder „Einmal“ oder „Serie“ an|
| Wartungsfenster |Festgelegte Minutenanzahl für Updates Der Wert darf nicht weniger als 30 Minuten und nicht mehr als 6 Stunden betragen |

## <a name="update-classifications"></a>Updateklassifizierungen

Die folgenden Tabellen enthalten eine Liste der Updateklassifizierungen in der Updateverwaltung, zusammen mit einer Definition für jede Klassifikation.

### <a name="windows"></a>Windows

|Classification  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates     | Ein Update für ein bestimmtes Problem, mit dem ein entscheidender, nicht sicherheitsrelevanter Fehler behoben wird.        |
|Sicherheitsupdates     | Ein Update für ein produktspezifisches, sicherheitsrelevantes Problem.        |
|Updaterollups     | Eine kumulative Gruppe von Hotfixes, die zur Vereinfachung der Bereitstellung gebündelt sind.        |
|Feature Packs     | Neue Produktfeatures, die nicht im Rahmen eines Produktreleases verteilt werden.        |
|Service Packs     | Eine kumulative Gruppe von Hotfixes, die auf eine Anwendung angewendet werden.        |
|Definitionsupdates     | Ein Update für virenbehaftete oder andere Definitionsdateien.        |
|Tools     | Ein Hilfsprogramm oder Feature, mit dem eine oder mehrere Aufgaben ausgeführt werden können.        |
|Aktualisierungen     | Ein Update für eine Anwendung oder Datei, die derzeit installiert ist.        |

### <a name="linux"></a>Linux

|Classification  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates und Sicherheitsupdates     | Updates für ein spezielles oder produktspezifisches, sicherheitsrelevantes Problem.         |
|Andere Updates     | Alle anderen Updates, die nicht von entscheidender Bedeutung sind oder bei denen es sich nicht um Sicherheitsupdates handelt.        |

## <a name="ports"></a>Ports

Die folgenden Adressen sind speziell für die Updateverwaltung erforderlich. Die Kommunikation mit diesen Adressen erfolgt über Port 443.

|Azure – Öffentlich  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|

Weitere Informationen zu Ports, die der Hybrid Runbook Worker benötigt, finden Sie unter [Ports für Hybrid Worker-Rollen](automation-hybrid-runbook-worker.md#hybrid-worker-role).

## <a name="search-logs"></a>Protokollsuche

Zusätzlich zu den Details, die im Portal bereitgestellt werden, können Sie auch die Protokolle durchsuchen. Öffnen Sie die Seite **Änderungsnachverfolgung**, und klicken Sie auf **Log Analytics**; daraufhin wird die Seite **Protokollsuche** geöffnet.

### <a name="sample-queries"></a>Beispielabfragen

Die folgende Tabelle enthält Beispiele für Protokollsuchen nach Updatedatensätzen, die mit dieser Lösung erfasst wurden:

| Abfragen | BESCHREIBUNG |
| --- | --- |
|Aktualisieren</br>&#124; where UpdateState == "Needed" and Optional == false</br>&#124; project Computer, Title, KBID, Classification, PublishedDate |Alle Computer, auf denen Updates fehlen</br>Fügen Sie eine der folgenden Angaben hinzu, um das Betriebssystem einzugrenzen:</br>OSType = "Windows"</br>OSType == "Linux" |
| Aktualisieren</br>&#124; where UpdateState == "Needed" and Optional == false</br>&#124; where Computer == "ContosoVM1.contoso.com"</br>&#124; project Computer, Title, KBID, Product, PublishedDate |Fehlende Updates für einen bestimmten Computer (Ersetzen Sie den Wert durch den Namen Ihres eigenen Computers.)|
| Ereignis</br>&#124; where EventLevelName == "error" and Computer in ((Update &#124; where (Classification == "Security Updates" or Classification == "Critical Updates")</br>&#124; where UpdateState == "Needed" and Optional == false </br>&#124; distinct Computer)) |Fehlerereignisse für Computer, auf denen erforderliche kritische oder Sicherheitsupdates fehlen |
| Aktualisieren</br>&#124; where UpdateState == "Needed" and Optional == false</br>&#124; distinct Title |Eindeutig identifizierbare fehlende Updates auf allen Computern |
| UpdateRunProgress</br>&#124; where InstallationStatus == "failed" </br>&#124; summarize AggregatedValue = count() by Computer, Title, UpdateRunName |Computer mit Updates, bei denen während einer Updateausführung ein Fehler aufgetreten ist</br>Fügen Sie eine der folgenden Angaben hinzu, um das Betriebssystem einzugrenzen:</br>OSType = "Windows"</br>OSType == "Linux" |
| Aktualisieren</br>&#124; where OSType == "Linux"</br>&#124; where UpdateState != "Not needed" and (Classification == "Critical Updates" or Classification == "Security Updates")</br>&#124; summarize AggregatedValue = count() by Computer |Liste aller Linux-Computer, für die ein Paketupdate zur Behebung von kritischen oder sicherheitsrelevanten Sicherheitsrisiken verfügbar ist |
| UpdateRunProgress</br>&#124; where UpdateRunName == "DeploymentName"</br>&#124; summarize AggregatedValue = count() by Computer|Computer, die bei dieser Updateausführung aktualisiert wurden (Ersetzen Sie den Wert durch den Namen Ihrer Updatebereitstellung.) |

## <a name="integrate-with-system-center-configuration-manager"></a>Integrieren in System Center Configuration Manager

Kunden, die in System Center Configuration Manager investiert haben, um PCs, Server und Mobilgeräte zu verwalten, profitieren auch von der Leistungsstärke und dem Funktionsumfang dieser Lösung bei der Verwaltung von Softwareupdates im Rahmen des Softwareupdateverwaltungs-Zyklus.

Wie Sie die Verwaltungslösung in System Center Configuration Manager integrieren, erfahren Sie unter [Integrieren von System Center Configuration Manager und Updateverwaltung](oms-solution-updatemgmt-sccmintegration.md).

## <a name="patching-linux-machines"></a>Patchen von Linux-Computern

In den folgenden Abschnitten werden mögliche Probleme beim Patchen von Linux-Computern erläutert.

### <a name="package-exclusion"></a>Paketausschluss

Bei einigen Linux-Varianten (z. B. Red Hat Enterprise Linux) können Upgrades auf Betriebssystemebene über Pakete erfolgen. Dies kann zur Ausführung der Updateverwaltung führen, wodurch sich die Versionsnummer des Betriebssystems ändert. Dieses Verhalten ist beabsichtigt, da die Updateverwaltung die gleichen Methoden für Updatepakete verwendet, die auch ein Administrator lokal auf dem Linux-Computer nutzt.

Um zu vermeiden, dass die Betriebssystemversion durch die Ausführung der Updateverwaltung aktualisiert wird, verwenden Sie die **Ausschluss**-Funktion.

In Red Hat Enterprise Linux würde der Name des auszuschließenden Pakets „redhat-release-server.x86_64“ lauten.

![Auszuschließende Pakete für Linux](./media/automation-update-management/linuxpatches.png)

### <a name="security-patches-not-being-applied"></a>Sicherheitspatches werden nicht angewendet

Beim Bereitstellen von Updates für einen Linux-Computer können Sie die Updateklassifizierungen auswählen. Dadurch werden die Updates gefiltert, die auf die Computer angewendet werden, welche die definierten Kriterien erfüllen. Dieser Filter wird lokal auf dem Computer angewendet, wenn das Update bereitgestellt wird. Da die Updateverwaltung die Updateergänzung in der Cloud ausführt, werden einige Updates in der Updateverwaltung möglicherweise als Update mit Sicherheitsauswirkung gekennzeichnet, obwohl der lokale Computer nicht über diese Informationen verfügt. Daher gibt es beim Anwenden kritischer Updates auf einem Linux-Computer möglicherweise Updates, die nicht als Update mit Sicherheitsauswirkung auf diesen Computer gekennzeichnet sind. Diese werden dann nicht angewendet. Allerdings wird die Updateverwaltung diesen Computer wahrscheinlich weiterhin als nicht kompatibel melden, da sie über zusätzliche Informationen zu dem relevanten Update verfügt.

Das Bereitstellen von Updates nach Updateklassifizierung funktioniert aufgrund des unterschiedlichen Patchmodells bei OpenSUSE Linux möglicherweise nicht.

## <a name="troubleshooting"></a>Problembehandlung

Dieser Abschnitt enthält Informationen zum Durchführen der Problembehandlung mit der Lösung für die Updateverwaltung.

Sollten Sie Probleme beim Onboarding der Lösung oder eines virtuellen Computers haben, suchen Sie im Ereignisprotokoll **Anwendungs- und Dienstprotokolle\Operations Manager** nach Ereignissen mit der Ereignis-ID 4502 und einer Ereignisnachricht, die **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** enthält. Die folgende Tabelle enthält spezifische Fehlermeldungen und passende Lösungsvorschläge.

| Message | Grund | Lösung |
|----------|----------|----------|
| Unable to Register Machine for Patch Management,</br>Registration Failed with Exception</br>System.InvalidOperationException: {"Message":"Machine is already</br>registered to a different account. "} (Der Computer konnte nicht für die Patchverwaltung registriert werden. Ausnahme "System.InvalidOperationException" bei der Registrierung: {"Meldung":"Der Computer ist bereits für ein anderes Konto registriert."} | Der Computer ist bereits in einen Arbeitsbereich für die Updateverwaltung integriert. | Bereinigen Sie alte Artefakte durch [Löschen der Hybrid-Runbook-Gruppe](automation-hybrid-runbook-worker.md#remove-hybrid-worker-groups).|
| Der Computer konnte nicht für die Patchverwaltung registriert werden. Fehler beim Registrieren. Ausnahme:</br>System.Net.Http.HttpRequestException: An error occurred while sending the request. ---></br>System.Net.WebException: The underlying connection</br>was closed: An unexpected error</br>occurred on a receive. ---> System.ComponentModel.Win32Exception:</br>The client and server cannot communicate,</br>because they do not possess a common algorithm (Der Computer konnte nicht für die Patchverwaltung registriert werden. Ausnahme "System.Net.Http.HttpRequestException" bei der Registrierung: Fehler beim Senden der Anforderung. System.Net.WebException: Die zugrunde liegende Verbindung wurde getrennt. Unerwarteter Fehler bei einem Empfangsvorgang. ---> System.ComponentModel.Win32Exception: Client und Server können nicht kommunizieren, da sie keinen gemeinsamen Algorithmus besitzen.) | Proxy/Gateway/Kommunikation durch Firewall blockiert | [Prüfen Sie die Netzwerkanforderungen.](automation-hybrid-runbook-worker.md#network-planning)|
| Unable to Register Machine for Patch Management,</br>Registration Failed with Exception</br>Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value. (Der Computer konnte nicht für die Patchverwaltung registriert werden. Ausnahme "Newtonsoft.Json.JsonReaderException" bei der Registrierung: Fehler beim Analysieren des positiven Unendlichkeitswerts.) | Proxy/Gateway/Kommunikation durch Firewall blockiert | [Prüfen Sie die Netzwerkanforderungen.](automation-hybrid-runbook-worker.md#network-planning)|
| Das durch den Dienst „\<wsid\>.oms.opinsights.azure.com“ vorgelegte Zertifikat</br>wurde nicht durch eine für Microsoft-Dienste verwendete</br>Zertifizierungsstelle ausgestellt. Kontakt</br>Ihren Netzwerkadministrator überprüfen, ob ein Proxy die</br>TLS/SSL-Kommunikation abfängt. |Proxy/Gateway/Kommunikation durch Firewall blockiert | [Prüfen Sie die Netzwerkanforderungen.](automation-hybrid-runbook-worker.md#network-planning)|
| Unable to Register Machine for Patch Management,</br>Registration Failed with Exception</br>AgentService.HybridRegistration.</br>PowerShell.Certificates.CertificateCreationException:</br>Failed to create a self-signed certificate. ---></br>System.UnauthorizedAccessException: Access is denied. (Der Computer konnte nicht für die Patchverwaltung registriert werden. Ausnahme "AgentService.HybridRegistration.PowerShell.Certificates.CertificateCreationException" bei der Registrierung: Fehler beim Erstellen eines selbstsignierten Zertifikats. System.UnauthorizedAccessException: Zugriff verweigert.) | Fehler beim Erstellen eines selbstsignierten Zertifikats | Vergewissern Sie sich, dass das Systemkonto</br>Lesezugriff auf den folgenden Ordner hat:</br>**C:\ProgramData\Microsoft\**</br>** Crypto\RSA**|

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem Tutorial fort, um zu erfahren, wie Updates für Ihre virtuellen Windows-Computer verwaltet werden.

> [!div class="nextstepaction"]
> [Verwalten von Updates und Patches für Ihre virtuellen Azure Windows-Computer](automation-tutorial-update-management.md)

* Verwenden Sie die Protokollsuche in [Log Analytics](../log-analytics/log-analytics-log-searches.md), um ausführliche Daten zu Updates anzuzeigen.
* [Erstellen Sie Warnungen](../log-analytics/log-analytics-alerts.md), wenn kritische Updates für Computer als fehlend erkannt werden oder für einen Computer die automatischen Updates deaktiviert sind.
