---
title: "Übersicht über die Operations Management Suite (OMS) | Microsoft Docs"
description: "Microsoft Operations Management Suite (OMS) ist die Microsoft Cloud-basierten IT-verwaltungslösung, mit denen die verwalten und Schützen Ihrer lokalen und cloud-Infrastruktur.  Dieser Artikel beschreibt den Wert der OMS, identifiziert die verschiedene Dienste und Angebote, die in der OMS enthalten und enthält Links zu ihren detaillierten Inhalt."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: bwren
ms.openlocfilehash: 452dd602387db6db04ca87f6834c9e8606185484
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-operations-management-suite-oms"></a>Was ist die Operations Management Suite (OMS)?
Dieser Artikel bietet eine Einführung zum Operations Management Suite (OMS) einschließlich einen kurzen Überblick über den Geschäftswert bietet, die Dienste und Lösungen für die darin enthaltenen und den angeboten, die verschiedene Dienste und -Lösungen zusammen zu verpacken.  Links werden eingeschlossen, um die ausführliche Dokumentation zur Bereitstellung und alle Dienste und -Lösung verwenden.

## <a name="from-on-premises-to-the-cloud"></a>Lokal in der cloud
Microsoft hat lange Produkte bereitgestellt wurden für die Verwaltung von Enterprise-Umgebungen.  Mehrere Produkte wurden in 2007 zu System Center-Suite von Verwaltungsprodukten zusammengefasst.  Diese enthalten die Configuration Manager bereit, die mit Funktionen wie die Verteilung von Software und Inventar, Operations Manager die proaktive Überwachung von Systemen und Anwendungen, Orchestrator die Runbooks zur Automatisierung von manueller Prozessen enthält, und die Data Protection Manager zur Sicherung und Wiederherstellung wichtiger Daten bereitstellt.

Mit mehr Computerressourcen in der Cloud erworben System Center-Produkten wie Operations Manager und Verwalten von Ressourcen in Azure Orchestrator weitere Cloud-Funktionen.  Sie immer noch im Grunde entworfen wurden, wie Lösungen über lokale und erfordern eine erhebliche Investition in bereitstellen und Verwalten von lokalen Management-Umgebung.  Um vollständig nutzen der Cloud, und zukünftige Anwendungen zu unterstützen, musste ein neuer Ansatz für die Verwaltung.

## <a name="introducing-operations-management-suite"></a>Einführung in die Operations Management Suite
Operations Management Suite (auch bekannt als OMS) ist eine Auflistung der Management-Dienste, die in der Cloud vom Beginn entwickelt wurden.  Anstatt bereitstellen und Verwalten von lokalen Ressourcen, werden die OMS-Komponenten vollständig in Azure gehostet.  Konfiguration ist minimal, und Sie können ausgeführt werden und als solches in wenigen Minuten.  

- **Minimalen Kosten und Komplexität der Bereitstellung.**  Da alle Komponenten und Daten für die OMS in Azure gespeichert werden, können Sie einrichten und ausführen, die innerhalb kurzer Zeit ohne die Komplexität und die Investitionen in lokale Komponenten.
- **Dezimalstellen in der cloud Ebenen.**  Sie müssen die Sorge über für Serverressourcen, die Sie benötigen nicht bezahlen oder über unzureichenden Speicherplatz, da die Cloud können Sie Zahlen nur für was Sie tatsächlich verwenden, und werden ohne weiteres skalieren, um alle Lasten, die Sie benötigen.  Sie können durch die Verwaltung von ein paar Thematik einsteigen, und klicken Sie dann skaliert und an der gesamten Umgebung starten.
- **Nutzen Sie die neuesten Funktionen.**  Funktionen in OMS-Dienste werden fortlaufend hinzugefügt und aktualisiert.  Ständig haben Sie Zugriff auf die neuesten Funktionen ohne eine Anforderung zum Bereitstellen von Updates.
- **Integrierte Dienste.**  Während die OMS-Dienste bieten erheblichem Wert auf ihre eigenen, können sie zusammenarbeiten, um komplexe Szenarien zu lösen.  Beispielsweise kann ein Runbook in Azure Automation Laufwerk einen Failovervorgang mit Azure Site Recovery und dann Protokollinformationen mit Log Analytics zum Generieren von Warnungen.
- **Globale wissen.**  Lösungen in OMS haben kontinuierlich Zugriff auf die neuesten Informationen.  Die Lösung für Sicherheit und Überwachung kann z. B. eine Bedrohungsanalyse, die mit den neuesten Bedrohungen erkannt wird, auf der ganzen Welt ausführen.
- **Zugriff von überall aus.**  Zugriff auf Ihre verwaltungsumgebung von überall aus haben Sie einen Browser.  Installieren Sie die OMS-app auf Ihrem Smartphone für sofortigen Zugriff auf die Überwachungsdaten.

### <a name="is-it-just-for-the-cloud"></a>Ist es nur für die Cloud?
Nur weil OMS-Dienste ausgeführt werden, die Cloud impliziert dies nicht, dass sie Ihre lokalen Umgebung nicht effektiv verwalten können.  Setzen einen Agent auf allen Windows oder Linux-Computer in Ihrem Rechenzentrum aus, und sendet Daten an die Protokollanalyse, in denen es zusammen mit anderen Daten, die in der Cloud oder lokal Dienste gesammelt analysiert werden können.  Verwenden Sie Azure Backup und Azure Site Recovery, um die Cloud für die Sicherung und hohe Verfügbarkeit für lokale Ressourcen nutzen.  
Runbooks in der Cloud kann nicht in der Regel auf lokale Ressourcen zugreifen, aber Sie können einen Agent installieren, auf einem oder mehreren Computern zu Runbooks in Ihrem Rechenzentrum gehostet wird.  Wenn Sie ein Runbook starten, geben Sie einfach an, ob Sie in der Cloud oder auf einem lokalen Worker ausgeführt werden soll.

## <a name="hybrid-management-with-system-center"></a>Hybridverwaltung mit System Center
Wenn Sie eine vorhandene Installation von System Center verfügen, können Sie diese Komponenten mit OMS-Dienste bieten eine hybridlösung für Ihre lokalen und cloud-Umgebungen, die Nutzung der relativen Spezialisierung jedes Produkts integrieren.  Verbinden Sie Ihre vorhandene Operations Manager-Verwaltungsgruppe mit Protokollanalyse verwaltete Agents in der Cloud zu analysieren.  Verwenden Sie Ihre vorhandenen Sicherungsprozess mit Data Protection Manager zur Sicherung Ihrer Daten in die Cloud.  


## <a name="oms-services"></a>OMS-Dienste
Die Kernfunktionen der OMS wird durch eine Reihe von Diensten bereitgestellt, die in Azure ausgeführt werden.  Jeder Dienst bietet eine bestimmte Verwaltungsfunktion und Dienste, um verschiedene Verwaltungsszenarien zu erreichen können kombiniert werden.

|| Dienst | Description |
|:--|:--|:--|
| ![Protokollanalysen](media/operations-management-suite-overview/icon-log-analytics.png) | Protokollanalysen | Überwachen Sie und analysieren Sie die Verfügbarkeit und Leistung von anderen Ressourcen, einschließlich der physischen und virtuellen Maschinen. |
| ![Azure Automation](media/operations-management-suite-overview/icon-automation.png) | Benutzeroberflächenautomatisierung | Manuelle Prozesse zu automatisieren und Konfigurationen für physische und virtuelle Computer zu erzwingen. |
| ![Azure-Sicherung](media/operations-management-suite-overview/icon-backup.png) | Sicherung | Sicherung und Wiederherstellung wichtiger Daten. |
| ![Azure Site Recovery](media/operations-management-suite-overview/icon-site-recovery.png) | Site Recovery | Geben Sie hohen Verfügbarkeit für kritische Anwendungen. |

### <a name="log-analytics"></a>Protokollanalysen
[Protokollanalysen](http://azure.microsoft.com/documentation/services/log-analytics) Überwachungsdienste für die OMS durch das Sammeln von Daten in einem zentralen Repository verwalteten Ressourcen enthält.  Diese Daten können es sich um Ereignisse, Leistungsdaten oder benutzerdefinierte Daten zur Verfügung gestellt, über die API enthalten. Sobald erfasst, sind die Daten für Warnungen, Analyse und Export verfügbar.  Diese Methode ermöglicht Ihnen, Daten aus einer Vielzahl von Quellen zu konsolidieren, sodass Sie Daten aus Ihrem Azure-Dienste mit Ihrer vorhandenen lokalen Umgebung kombinieren können.  Es dient zur auch klaren Trennung der Erfassung der Daten aus der Aktion für diese Daten so, dass alle Aktionen für alle Arten von Daten verfügbar sind.  

![Melden Sie sich Analyse (Übersicht)](media/operations-management-suite-overview/overview-log-analytics.png)

#### <a name="collecting-data"></a>Das Sammeln von Daten
Es gibt eine Vielzahl von Methoden, mit denen Sie Daten in das Repository für die Protokollanalyse zu analysierenden abrufen können.

- **Windows- oder Linux-Computern und virtuellen Computern.**  Installieren Sie Microsoft Monitoring Agent auf [Windows](../log-analytics/log-analytics-windows-agents.md) und [Linux](../log-analytics/log-analytics-linux-agents.md) Computern oder virtuellen Computern, die Sie Daten sammeln möchten.  Der Agent wird automatisch von Protokollanalyse Konfiguration herunterladen, die Ereignisse und Leistungsdaten sammeln definiert.  Sie können problemlos den Agent auf virtuellen Computern in Azure mithilfe von Azure-Portal, installieren.  Wenn Sie eine vorhandene Operations Manager-Umgebung verfügen, können verbinden die Verwaltungsgruppe mit Protokollanalyse und automatisch starten Sie das Sammeln von Daten von allen vorhandenen Agents.
- **Azure-Dienste.**  Log Analytics erfasst Telemetriedaten von [Azure-Diagnose und Azure-Überwachung](../log-analytics/log-analytics-azure-storage.md) im Repository, damit Sie Azure-Ressourcen überwachen können.
- **Datensammler-API.**  Log Analytics ist ein [REST-API für das Auffüllen von Daten von beliebigen Clients](../log-analytics/log-analytics-data-collector-api.md).  Dadurch können Sie zum Sammeln von Daten aus Anwendungen von Drittanbietern oder benutzerdefinierte Verwaltungsszenarien zu implementieren.  Eine gängige Methode ist die Verwendung ein Runbooks in Azure Automation verwenden, Daten sammeln und verwenden dann die Datensammler-API, um es in das Repository schreiben.

#### <a name="reporting-and-analyzing-data"></a>Berichterstellung und Analyse von Daten
Log Analytics umfasst eine leistungsstarke Abfragesprache zum Extrahieren von Daten, die im Repository gespeichert.  Da Daten aus allen Quellen als Datensätze gespeichert werden, können Sie Daten aus mehreren Quellen in einer einzelnen Abfrage analysieren.
  
Zusätzlich zu den ad-hoc-Analysen bietet Protokollanalyse mehrere Möglichkeiten, Berichten und Analysieren von Daten aus einer Abfrage.

- **Ansichten und Dashboards.**  [Sichten](../log-analytics/log-analytics-view-designer.md) und [Dashboards](../log-analytics/log-analytics-dashboards.md) Visualisieren der Ergebnisse einer Abfrage im Portal.  Lösungen für die Verwaltung werden in der Regel Sichten enthalten, die Analyse der Daten aus der Projektmappe.  Sie können auch Ihre eigenen benutzerdefinierten Ansichten zum Analysieren von Daten und Zurverfügungstellen dieser sofort in Ihrem benutzerdefinierten-Portal erstellen.
- **Exportieren**  Sie haben die Möglichkeit zum Exportieren der Ergebnisse von Abfragen, sodass außerhalb der Protokollanalyse analysiert werden können.  Sie können auch planen, eine reguläre exportieren nach [Power BI](../log-analytics/log-analytics-powerbi.md) stellt und die datenvisualisierungs- und Fähigkeiten.
- **Protokollsuch-API.**  Log Analytics ist ein [REST-API für das Sammeln von Daten von jedem beliebigen Client](../log-analytics/log-analytics-log-search-api.md).  Dadurch können Sie programmgesteuert arbeiten mit Daten, die im Repository erfasst oder in einem anderen Überwachungstool darauf zugreifen.

#### <a name="alerting"></a>Warnungen
Log Analytics können [proaktiv Warnung](../log-analytics/log-analytics-alerts.md) Sie oder die entsprechenden Maßnahmen ergreifen, wenn ein Problem erkannt wird.  Wie alle anderen Analyse Protokollanalyse erfolgt dies mit einer protokollsuche.  Bei dieser Suche wird in regelmäßigen Abständen ausgeführt, und eine Warnung wird erstellt, wenn die Ergebnisse auf bestimmte Kriterien entsprechen.

![Log Analytics-Warnungen](media/operations-management-suite-overview/overview-alerts.png)

Warnungen können folgende Maßnahmen ergreifen, zusätzlich zum Erstellen eines Warnungen Datensatzes in die Log Analytics-Repository.

- **E-Mail.**  Senden Sie eine e-Mail an Sie ein erkanntes Problem proaktiv zu informieren.
- **Runbook.**  Eine Warnung in Protokollanalyse kann ein Runbook in Azure Automation starten.  Dies erfolgt in der Regel um zu versuchen, das erkannte Problem zu beheben.  Das Runbook gestartet werden kann, die Cloud im Falle eines Problems in Azure oder einem anderen Cloud, oder es auf einem lokalen Agent für ein Problem auf einem physischen oder virtuellen Computer gestartet werden konnte.
- **Webhook.**  Eine Warnung kann einen Webhook starten und Daten aus den Ergebnissen der protokollsuch übergeben.  Dies ermöglicht die Integration in externe Dienste wie z. B. eine alternative Warnungssystem oder möglicherweise versucht, die für eine externe Website korrekturmaßnahmen.

### <a name="azure-automation"></a>Azure Automation
[Azure Automation](http://azure.microsoft.com/documentation/services/automation) prozessverwaltung Automatisierung und die Konfiguration mit der OMS bietet.  Manuelle Prozesse automatisiert, und hilft dabei, um Konfigurationen für physische und virtuelle Computer zu erzwingen.  

#### <a name="process-automation"></a>Die Automatisierung von Geschäftsprozessen
Azure Automation automatisiert manuelle Prozesse, die mit [Runbooks](../automation/automation-runbook-types.md) die basieren auf der PowerShell-Skript oder PowerShell-Workflow.  Darüber hinaus Bestand, die Unterstützung von Runbooks, z. B. Variablen, die freigegeben werden kann, zwischen mehreren Runbooks und Anmeldeinformationen und Verbindungen, mit denen Sie zum Speichern von verschlüsselten Informationen, der für ein Runbook für die Authentifizierung erforderlich sind.
Runbooks bieten prozessautomatisierungen für die anderen Dienste in der Suite.  Da jedes der anderen Dienste mit PowerShell oder über eine REST-API zugegriffen werden kann, können Sie Runbooks zum Ausführen von Funktionen wie das Sammeln von Daten in die Log Analytics Management oder initiieren eine Sicherung mit Azure-Sicherung erstellen.

##### <a name="accessing-resources"></a>Zugreifen auf Ressourcen
Da Runbooks auf PowerShell basieren, können sie alle Ressourcen verwalten, die mit PowerShell-Cmdlets zugegriffen werden kann.  Wenn Sie [Laden ein Moduls](../automation/automation-integration-modules.md) in Ihr Automation-Konto wird für alle Runbooks in diesem Konto verfügbar. 
 
Bei der Ausführung von Runbooks in der Cloud können sie alle Ressourcen zugegriffen werden kann aus der Cloud zugreifen.  Dies ist möglicherweise Ressourcen in Ihrem Azure-Abonnement in eine andere Cloud aus, z. B. Amazon Web Services (AWS) oder einen Dienst über eine REST-API zugegriffen werden kann.  Runbooks in der Cloud keine Anmeldeinformationen ausgeführt, aber nutzen sie automatisierungsressourcen wie z. B. Anmeldeinformationen und Verbindungen, Zertifikate, die Zugriff auf Ressourcen zu authentifizieren.

Ressourcen in Ihrem Rechenzentrum können nicht aus einem Runbook in der Cloud ausgeführte wahrscheinlich zugegriffen werden.  Sie können eine oder mehrere installieren [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md) in Ihren Daten zu zentrieren jedoch zum Ausführen von Runbooks, die Zugriff auf lokale Ressourcen benötigen.  Wenn Sie ein Runbook starten, geben Sie an, ob in der Cloud oder auf einem bestimmten Worker ausgeführt werden soll.

![Azure Automation-runbooks](media/operations-management-suite-overview/overview-runbooks.png)

##### <a name="starting-a-runbook"></a>Starten eines Runbooks
Runbooks können werden [durch eine Reihe von Methoden gestartet](../automation/automation-starting-a-runbook.md) , damit sie in einer Vielzahl von Szenarien enthalten sein können.  

- **Azure-Portal.**  Wie andere Azure-Dienste können Azure Automation aus dem Azure-Portal verwaltet werden.  Zusätzlich zum Starten von Runbooks, können Sie importieren oder eigene erstellen.
- **Geplant.**  Sie können Runbooks in regelmäßigen Abständen zu planen.  Dadurch können Sie automatisch einen reguläre Verwaltungsprozess wiederholen oder das Sammeln von Daten mit Log Analytics.
- **PowerShell und -API.**  Sie können Runbooks starten und übergeben sie die erforderlichen Parameterinformationen aus einer PowerShell-Cmdlets oder REST-API von Azure Automation.  
- **Webhook.**  Ein Webhook kann für jedes Runbook erstellt werden, die aus externen Anwendungen oder Websites gestartet werden kann.
- **Log Analytics-Warnung.**  Eine Warnung in Protokollanalyse kann ein Runbook, das Sie zur Behebung des Problems, die von der Warnung identifiziert versuchen automatisch gestartet.

#### <a name="configuration-management"></a>Konfigurationsverwaltung
[PowerShell Desired State Configuration (DSC)](../automation/automation-dsc-overview.md) ist eine Verwaltungsplattform in Windows PowerShell, die Sie zum Bereitstellen und erzwingen die Konfiguration der physischen und virtuellen Computern ermöglicht.  Azure Automation DSC-Konfigurationen verwaltet und bietet einen Pull-Server in der Cloud, die Agents zum Abrufen der erforderlichen Konfigurationen zugreifen können.

![Azure Automation DSC](media/operations-management-suite-overview/overview-dsc.png)

### <a name="azure-backup-and-azure-site-recovery"></a>Azure Backup und Azure Site Recovery
Azure Backup und Azure Site Recovery tragen jeweils zur Geschäftskontinuität und notfallwiederherstellung.  Beide verfügen über Funktionen, mit deren Hilfe Sie sicherstellen, dass Anwendungen zur Verfügung, weiterhin Wenn Ausfälle auftreten wird und an normalen Betrieb zurückgeben, wenn Systeme wieder online ist.  Beide Dienste werden mit dem Recovery Point Objectives (RPO) und Recovery Time Objectives (RTO) für Ihr Unternehmen definierten beitragen. RPO definiert den zulässigen Grenzwert in der Daten ist nicht verfügbar bei einem Stromausfall, und die RTO schränkt den akzeptablen Zeitraum, in dem ein Dienst oder eine app ist nicht verfügbar bei einem Stromausfall.

#### <a name="azure-backup"></a>Azure-Sicherung
[Azure-Sicherung](http://azure.microsoft.com/documentation/services/backup) Daten sichern und Wiederherstellen von Diensten für die OMS enthält.  Es schützt Ihre Anwendungsdaten und bewahrt sie für Jahre ohne Kapitaleinsatz und mit minimalen Betriebskosten.  Es kann Daten von physischen und virtuellen Windows-Servern zusätzlich zu anwendungsauslastungen wie SQL Server und SharePoint sichern.  Sie können auch von System Center Data Protection Manager (DPM) verwendet werden um geschützte Daten hinsichtlich Redundanz und langfristige Speicherung in Azure zu replizieren.

Geschützte Daten in Azure Backup werden in einem sicherungstresor befindet sich in einer bestimmten geografischen Region gespeichert. Die Daten werden innerhalb der gleichen Region repliziert und je nach Art des Tresors, möglicherweise auch repliziert werden in einer anderen Region für weitere Stabilität.

Azure Backup verfügt über drei grundlegende Szenarien.

- **Windows-Computer mit Azure Backup-Agent.** Sichern von Dateien und Ordner von einem Windows-Server oder Client direkt an den Azure backup-Tresor.<br><br>![Windows-Computer mit Azure Backup-agent](media/operations-management-suite-overview/overview-backup-01.png)
- **System Center Data Protection Manager (DPM) oder Microsoft Azure Backup Server.** Nutzen Sie DPM oder Microsoft Azure Backup Server zum Sichern von Dateien und Ordnern, zusätzlich zu anwendungsauslastungen wie SQL und SharePoint in den lokalen Speicher, und klicken Sie dann auf den Azure backup-Tresor replizieren. Unterstützt Windows- und Linux-VMs unter Hyper-V oder VMware.<br><br>![System Center Data Protection Manager (DPM) oder Microsoft Azure Backup Server](media/operations-management-suite-overview/overview-backup-02.png)
- **Azure VM-Erweiterungen.** Windows- oder Linux virtuelle Computer sichern in Azure in Ihrem Azure backup-Tresor.<br><br>![Azure VM-Erweiterungen](media/operations-management-suite-overview/overview-backup-03.png)



#### <a name="azure-site-recovery"></a>Azure Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) bietet Geschäftskontinuität orchestriert die Replikation von einer lokalen virtuelle und physische Computern in Azure oder einem sekundären Standort. Wenn der primäre Standort nicht verfügbar ist, ein Sie Failover auf den sekundären Standort, damit Benutzer weiterarbeiten können, und schlagen fehl, zurück, wenn Systeme zur funktionsfähigen Zustand zurückzukehren. 

Azure Site Recovery bietet eine hohe Verfügbarkeit für Server und Anwendungen.  Er trägt, Geschäftskontinuität und einer Notfallwiederherstellungsstrategie (BCDR) bei, indem Replikation, Failover und Wiederherstellung von einer lokalen Hyper-V-Computer, virtuelle VMware-Maschinen und physischen Windows-/Linux-Servern orchestriert. Sie können Computer zu einem sekundären Rechenzentrum repliziert oder das Datencenter erweitern, indem sie in Azure repliziert werden. Site Recovery bietet zudem einfachen Failover und Wiederherstellung für arbeitsauslastungen. Es wird in Mechanismen zur notfallwiederherstellung z. B. SQL Server AlwaysOn integriert und bietet Wiederherstellungspläne für den einfachen Failover von arbeitsauslastungen, die auf mehrere Computer mehrstufig verteilt sind.

Azure Site Recovery verfügt über drei grundlegende Replikationsszenarien.

- **Die Replikation von Hyper-V-Computer.**  Wenn Hyper-V-Maschinen in VMM-Clouds verwaltet werden, können Sie ein sekundäres Datencenter oder in Azure-Speicher replizieren. Die Replikation in Azure erfolgt über eine sichere Internetverbindung. Die Replikation erfolgt in ein sekundäres Datencenter über das LAN.  Wenn Hyper-V-Maschinen nicht von VMM verwaltet werden, können Sie den Azure-Speicher replizieren. Die Replikation in Azure erfolgt über eine sichere Internetverbindung.<br><br>![Replikation von Hyper-V-Computer](media/operations-management-suite-overview/overview-siterecovery-hyperv.png)
- **Die Replikation der virtuellen VMware-Maschinen.**  Sie können virtuelle VMware-Maschinen in ein VMware ausführendes sekundäres Datencenter oder in Azure-Speicher replizieren. Replikation in Azure kann über eine Standort-zu-Standort-VPN oder Azure ExpressRoute bzw. über eine sichere Internetverbindung erfolgen. Die Replikation in ein sekundäres Datencenter erfolgt über den InMage Scout-Datenkanal.<br><br>![Replikation von virtuellen VMware-Maschinen](media/operations-management-suite-overview/overview-siterecovery-vmware.png)
- **Die Replikation von physischen Windows- und Linux-Servern.**  Sie können physische Server in ein sekundäres Datencenter oder in Azure-Speicher replizieren. Replikation in Azure kann über eine Standort-zu-Standort-VPN oder Azure ExpressRoute bzw. über eine sichere Internetverbindung erfolgen. Die Replikation in ein sekundäres Datencenter erfolgt über den InMage Scout-Datenkanal. Azure Site Recovery bietet eine OMS-Lösung, die einige Statistiken anzeigt, aber Sie müssen das Azure-Portal für beliebige Vorgänge verwenden.<br><br>![Replikation von physischen Windows- und Linux-Servern](media/operations-management-suite-overview/overview-siterecovery-physical.png)


Site Recovery speichert Metadaten in Tresoren, die sich in einer bestimmten geografischen Azure-Region befinden. Keine replizierten Daten vom Site Recovery-Dienst gespeichert.

## <a name="management-solutions"></a>Lösungen für die Verwaltung
[Verwaltungslösungen](operations-management-suite-solutions.md) vorgefertigte Sätze von Logik, die einen bestimmten Verwaltungsszenarien nutzen einen oder mehrere OMS-Dienste zu implementieren.  Verschiedene Lösungen sind verfügbar, von Microsoft und von Partnern, dass Sie problemlos mit Ihrem Azure-Abonnement erhöhen den Wert Ihrer Investitionen in OMS hinzufügen können.  Als Partner können Sie Ihren eigenen Lösungen zur Unterstützung von Anwendungen und Dienste, und ermöglichen sie Benutzern über den Azure Marketplace oder Schnellstart Vorlagen erstellen.

Ist ein gutes Beispiel einer Lösung, die mehrere Dienste, um zusätzliche Funktionalität bereitstellen, nutzt der [Update verwaltungslösung](oms-solution-update-management.md).  Diese Lösung verwendet den Protokollanalyse-Agent für Windows und Linux, um Informationen über die erforderlichen Updates auf den einzelnen Agents sammeln.  Diese Daten in die Log Analytics-Repository, in dem Sie es mit einem enthalten Dashboard analysieren können, geschrieben.  Wenn Sie eine Bereitstellung erstellen, werden Runbooks in Azure Automation zum erforderlichen Updates zu installieren.  Sie Verwalten der gesamte Prozess im Portal und die Details der zugrunde liegenden kümmern müssen.

![Lösung](media/operations-management-suite-overview/overview-solution.png)

Die meisten Lösungen können eine oder mehrere der folgenden Funktionen ausführen.

- Sammeln Sie zusätzliche Informationen.  Log Analytics erfasst eine Vielzahl von Daten von Clients und Diensten, z. B. Ereignisse und Leistungsdaten an.  Eine verwaltungslösung erfasst aus anderen Datenquellen, die häufig mit Azure Automation-Runbook möglicherweise zusätzliche Informationen sind nicht verfügbar.
- Geben Sie zusätzliche Analyse der gesammelten Informationen an.  Management-Lösungen umfassen die Dashboards und Ansichten, die Analyse und Visualisierung von Daten enthalten.  Diese verknüpfen wieder mit vordefinierten protokollsuchen, die Ihnen ermöglichen, führen Sie einen Drilldown auf die detaillierten Daten.  Sie können auch Analysis ausführen, die auf Daten, die bereits im Repository gesammelten ist, suchen z. B. über Sicherheitsereignisse für Muster, die eine Bedrohung angeben.
- Fügen Sie die Funktionalität hinzu.  Einige Lösungen, die von Microsoft bereitgestellten möglicherweise auf die Funktionen der Kerndienste, um zusätzliche Funktionalität bereitzustellen aufbauen.  Dienstzuordnung beispielsweise bietet eine eigenen Konsole, um zu ermitteln und ordnet Server und den Prozesstyp Abhängigkeiten in Echtzeit.
Lösungen werden von Microsoft und Partner, sodass Sie den Wert Ihrer Investitionen in fortlaufend zu erhöhen regelmäßig mit der OMS hinzugefügt wird.  Sie können durchsuchen und installieren Microsoft-Lösungen, über den Katalog Lösungen in OMS-Portal oder Durchsuchen klicken und Lösungen von Microsoft und Partner über den Azure Marketplace in das Azure-Portal installieren.  

![Solution Gallery](media/operations-management-suite-overview/solution-gallery.png)


## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Protokollanalysen](http://azure.microsoft.com/documentation/services/log-analytics).
* Erfahren Sie mehr über [Azure Automation](../automation/automation-intro.md).
* Erfahren Sie mehr über [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Erfahren Sie mehr über [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).
* Ermitteln der [verfügbaren Lösungen](../log-analytics/log-analytics-add-solutions.md) in den verschiedenen OMS-Angebote. 

