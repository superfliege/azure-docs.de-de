---
title: Schützen von Computern und Anwendungen in Azure Security Center | Microsoft-Dokumentation
description: In diesem Dokument werden Empfehlungen in Security Center erläutert, die zum Schutz Ihrer virtuellen und physischen Computer sowie Ihrer Web-Apps und App Service-Umgebungen beitragen.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 454478fa02b8f4e71bc4efb61e1b8c194b927bc6
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53340826"
---
# <a name="protecting-your-machines-and-applications-in-azure-security-center"></a>Schützen von Computern und Anwendungen in Azure Security Center
Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Werden potenzielle Sicherheitslücken erkannt, erstellt Security Center Empfehlungen, die Sie beim Konfigurieren der erforderlichen Steuerelemente unterstützen. Die Empfehlungen gelten für folgende Azure-Ressourcentypen: virtuelle Computer (VMs) und physische Computer, Anwendungen, Netzwerke, SQL sowie Identität und Zugriff.

In diesem Artikel werden Empfehlungen für Computer und Anwendungen behandelt.

## <a name="monitoring-security-health"></a>Überwachung der Sicherheitsintegrität
Auf dem Dashboard **Security Center – Übersicht** können Sie den Sicherheitsstatus Ihrer Ressourcen überwachen. Im Abschnitt **Ressourcen** sind die Anzahl der ermittelten Probleme und der Sicherheitsstatus für die einzelnen Ressourcentypen angegeben.

Sie können eine Liste aller Probleme anzeigen, indem Sie auf **Empfehlungen** klicken. Weitere Informationen zur Anwendung der Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).

Eine vollständige Liste der Empfehlungen für Compute Services und App Services finden Sie unter [Empfehlungen](security-center-virtual-machine-recommendations.md).

Wählen Sie zum Fortsetzen des Vorgangs unter **Ressourcen** oder im Security Center-Hauptmenü die Option **Compute und Apps** aus.
![Security Center-Dashboard][1]

## <a name="monitor-compute-and-app-services"></a>Überwachen von Compute Services und App Services
**Compute** umfasst vier Registerkarten:

- **Übersicht:** Überwachung und von Security Center identifizierte Empfehlungen
- **VMs und Computer:** Liste Ihrer virtuellen und physischen Computer sowie deren aktueller Sicherheitsstatus
- **Clouddienste:** Liste mit Ihren von Security Center überwachten Web- und Workerrollen
- **App Services (Vorschau):** Liste mit Ihren App Service-Umgebungen und deren jeweiliger aktueller Sicherheitsstatus.
Wählen Sie zum Fortsetzen des Vorgangs unter **Ressourcen** oder im Security Center-Hauptmenü die Option **Compute und Apps** aus.

![Compute][2]

Auf den einzelnen Registerkarten können sich jeweils mehrere Abschnitte befinden, und in jedem Abschnitt können Sie eine einzelne Option auswählen, um weitere Details zu den empfohlenen Problembehandlungsschritten für ein bestimmtes Problem anzuzeigen.

### <a name="monitoring-recommendations"></a>Überwachen der Empfehlungen
Dieser Abschnitt enthält die Gesamtanzahl und den jeweils aktuellen Status der virtuellen und physischen Computer, die für die automatische Bereitstellung initialisiert wurden. Dieses Beispiel enthält eine Empfehlung: **Integritätsprobleme des Überwachungs-Agents**. Wählen Sie diese Empfehlung aus.

![Integritätsprobleme des Überwachungs-Agents][3]

**Integritätsprobleme des Überwachungs-Agents** wird geöffnet. Virtuelle und physische Computer, die von Security Center nicht überwacht werden können, werden aufgeführt. Wählen Sie einen virtuellen oder physischen Computer aus, um ausführliche Informationen anzuzeigen. Unter **ZUSTANDSÜBERWACHUNG** wird ein Grund angegeben, warum die Überwachung mit Security Center nicht durchgeführt werden kann. Das [Handbuch zur Problembehandlung für Security Center](security-center-troubleshooting-guide.md) enthält eine Liste mit Werten, Beschreibungen und Lösungsschritten für die **ZUSTANDSÜBERWACHUNG**.

### Nicht überwachte virtuelle und physische Computer <a name="unmonitored-vms-and-computers"></a>
Ein virtueller oder physischer Computer gilt als nicht von Security Center überwacht, wenn auf dem Computer die Microsoft Monitoring Agent-Erweiterung nicht ausgeführt wird. Auf einem Computer kann bereits ein lokaler Agent installiert sein, z.B. der OMS-Direkt-Agent oder der SCOM-Agent. Computer mit diesen Agents werden als nicht überwacht angesehen, da diese Agents in Security Center nicht vollständig unterstützt werden. Die Microsoft Monitoring Agent-Erweiterung ist erforderlich, um von allen Security Center-Vorteilen vollständig profitieren zu können.

Sie können die Erweiterung auf dem nicht überwachten virtuellen oder physischen Computer zusätzlich zum bereits installierten lokalen Agent installieren. Konfigurieren Sie beide Agents auf die gleiche Weise, und verbinden Sie sie mit demselben Arbeitsbereich. Security Center kann dann mit der Microsoft Monitoring Agent-Erweiterung interagieren und Daten sammeln. Eine Anleitung zur Installation der Microsoft Monitoring Agent-Erweiterung finden Sie unter [Aktivieren der VM-Erweiterung](../azure-monitor/learn/quick-collect-azurevm.md).

Weitere Informationen zu den Gründen, warum Security Center keine VMs und Computer erfolgreich überwachen kann, die für die automatische Bereitstellung initialisiert wurden, finden Sie unter [Integritätsprobleme des Überwachungs-Agents](security-center-troubleshooting-guide.md#mon-agent).

### <a name="recommendations"></a>Empfehlungen
Dieser Abschnitt enthält eine Reihe von Empfehlungen für die einzelnen virtuellen und physischen Computer, Web- und Workerrollen, Azure App Service-Web-Apps und Azure App Service-Umgebungen, die von Security Center überwacht werden. Die erste Spalte enthält die Empfehlung. Die zweite Spalte enthält die Gesamtanzahl der Ressourcen, die diese Empfehlung betrifft. Die dritte Spalte gibt Aufschluss über den Schweregrad des Problems, wie im folgenden Screenshot zu sehen:

![Empfehlungen][4]

Jeder Empfehlung ist eine Reihe von Aktionen zugeordnet, die ausgeführt werden können, nachdem Sie sie ausgewählt haben. Wenn Sie beispielsweise **Fehlende Systemupdates** auswählen, wird wie im folgenden Screenshot die Anzahl der virtuellen und physischen Computer, auf denen Patches fehlen, und der Schweregrad des fehlenden Updates angezeigt:

![Systemupdates anwenden][5]

Für **Systemupdates anwenden** werden zwei Diagramme mit einer Zusammenfassung kritischer Updates angezeigt (eines für Windows, eines für Linux). Der zweite Teil enthält eine Tabelle mit folgenden Informationen:

- **NAME**: Der Name des fehlenden Updates.
- **Anzahl VON VMs UND COMPUTERN**: Die Gesamtzahl von VMs und Computern, auf denen dieses Update fehlt.
- **UPDATESCHWEREGRAD**: Beschreibt den Schweregrad der jeweiligen Empfehlung:

    - **Kritisch**: Ein Sicherheitsrisiko betrifft eine wichtige Ressource (Anwendung, VM oder Netzwerksicherheitsgruppe) und erfordert einen Eingriff.
    - **Wichtig**: Es liegt ein nicht kritisches Problem vor, oder es sind zusätzliche Schritte erforderlich, um einen Prozess abzuschließen oder ein Sicherheitsrisiko zu beseitigen.
    - **Mittel**: Ein Sicherheitsrisiko, das behandelt werden sollte, jedoch keinen unmittelbaren Eingriff erfordert. (Standardmäßig werden Empfehlungen mit dem Status „Niedrig“ nicht angezeigt, aber Sie können bei Bedarf nach diesen Empfehlungen filtern.)


- **STATUS**: Der aktuelle Status der Empfehlung:

    - **Offen**: Die Empfehlung wurde noch nicht umgesetzt.
    - **In Bearbeitung**: Die Empfehlung wird derzeit auf die Ressourcen angewendet, und es ist keine Aktion Ihrerseits erforderlich.
    - **Behoben**: Die Empfehlung wurde bereits umgesetzt. (Wenn das Problem behoben wurde, wird der Eintrag abgeblendet dargestellt.)

Klicken Sie zum Anzeigen der Empfehlungsdetails in der Liste auf den Namen des fehlenden Updates.

![Empfehlungsdetails][6]

> [!NOTE]
> Hier werden die gleichen Sicherheitsempfehlungen aufgeführt wie unter der Kachel **Empfehlungen**. Weitere Informationen zur Anwendung von Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).
>
>

### <a name="vms-and-computers"></a>VMs und Computer
Der Abschnitt „VMs und Computer“ enthält eine Übersicht über alle Empfehlungen für virtuelle und physische Computer. Jede Spalte steht für eine Gruppe von Empfehlungen, wie im folgenden Screenshot zu sehen:

![Empfehlungen für virtuelle und physische Computer][7]

Die Liste enthält vier Arten von Symbolen:

![Nicht-Azure-Computer][8] Azure-fremder Computer

![Virtueller Azure Resource Manager-Computer][9] Virtueller Azure Resource Manager-Computer

![Klassischer virtueller Azure-Computer][10] Klassischer virtueller Azure-Computer

![Virtuelle Computer, die anhand des Arbeitsbereichs identifiziert werden][11] Virtuelle Computer, die nur anhand des Arbeitsbereichs identifiziert werden, der dem angezeigten Abonnement angehört. Dazu zählen virtuelle Computer aus anderen Abonnements, die dem Arbeitsbereich in diesem Abonnement unterstellt sind, sowie virtuelle Computer, die mit dem direkten SCOM-Agent installiert wurden und über keine Ressourcen-ID verfügen.

Anhand des Symbols unter der jeweiligen Empfehlung sehen Sie sofort, bei welchem virtuellen oder physischen Computer eine Aktion erforderlich ist und um welche Art von Empfehlung es sich handelt. Mithilfe der Option „Filter“ können Sie zudem auswählen, welche Optionen auf diesem Bildschirm angezeigt werden sollen.

![Filter][12]

Im vorherigen Beispiel wird für einen virtuellen Computer eine dringende Empfehlung in Bezug auf den Endpunktschutz angezeigt. Wählen Sie den virtuellen Computer aus, um weitere Informationen anzuzeigen:

![Kritische Empfehlung][13]

Hier finden Sie die Sicherheitsdetails für den virtuellen oder physischen Computer. Im unteren Bereich des Blatts werden die empfohlene Aktion und der Schweregrad des jeweiligen Problems angezeigt.

### <a name="cloud-services"></a>Clouddienste
Für Clouddienste wird eine Empfehlung erstellt, wenn die Betriebssystemversion nicht mehr aktuell ist, wie im folgenden Screenshot zu sehen:

![Clouddienste][14]

In einem Szenario, in dem eine Empfehlung vorliegt (was im vorherigen Beispiel nicht der Fall ist), muss die Betriebssystemversion mithilfe der Schritte aus der Empfehlung aktualisiert werden. Ist ein Update verfügbar, erhalten Sie eine Warnung (rot oder orange, je nach Schweregrad des Problems). Wenn Sie diese Warnung in „WebRole1“ (Ausführung von Windows Server mit automatischer Bereitstellung Ihrer Web-App für IIS) oder „WorkerRole1“ (Ausführung von Windows Server mit automatischer Bereitstellung Ihrer Web-App für IIS) auswählen, werden weitere Details zur Empfehlung angezeigt, wie im folgenden Screenshot zu sehen:

![WorkerRole1][15]

Eine Erläuterung der Empfehlung erhalten Sie, wenn Sie in der Spalte **BESCHREIBUNG** auf **Betriebssystemversion aktualisieren** klicken.

![Betriebssystemversion aktualisieren][16]

### <a name="app-services-preview"></a>App Services (Vorschau)

> [!NOTE]
> Die Überwachung von App Services ist als Vorschauversion und im Standard-Tarif von Security Center verfügbar. Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md).
>
>

Unter **App Services** wird eine Liste Ihrer App Service-Umgebungen und die Integritätszusammenfassung basierend auf der von Security Center ausgeführten Bewertung angezeigt.

![App Services][17]

Die Liste enthält drei Arten von Symbolen:

![App Service-Umgebung][18] App Service-Umgebung

![Webanwendung][19] Webanwendung

![Funktionsanwendung][24] Funktionsanwendung

1. Wählen Sie eine Webanwendung aus. Eine zusammenfassende Darstellung mit drei Registerkarten wird geöffnet:

  - **Empfehlungen**: Basiert auf vom Security Center ausgeführten Bewertungen, bei denen ein Fehler aufgetreten ist.
  - **Bestandene Bewertungen**: Liste der vom Security Center ausgeführten Bewertungen, die bestanden wurden.
  - **Nicht verfügbare Bewertungen:** Liste der Bewertungen, die aufgrund eines Fehlers nicht ausgeführt wurden, oder bei denen die Empfehlung für den jeweiligen Service nicht relevant ist.

  Unter **Empfehlungen** finden Sie eine Liste der Empfehlungen für die ausgewählte Webanwendung und den Schweregrad der einzelnen Empfehlungen.

  ![Zusammenfassung][20]

2. Wählen Sie eine Empfehlung aus, um eine Beschreibung der Empfehlung und eine Liste von fehlerhaften und fehlerfreien sowie nicht überprüften Ressourcen anzuzeigen.

  ![Empfehlungsbeschreibung][21]

  Unter **Bestandene Bewertungen** finden Sie eine Liste der bestandenen Bewertungen.  Der Schweregrad dieser Bewertungen wird immer in Grün angezeigt.

  ![Bestandene Bewertungen][22]

3. Wählen Sie eine bestandene Bewertung in der Liste aus, um eine Beschreibung der Bewertung, eine Liste von fehlerhaften und fehlerfreien Ressourcen und eine Liste von nicht überprüften Ressourcen anzuzeigen. Zwar ist eine Registerkarte für fehlerhafte Ressourcen vorhanden, diese Liste ist aber immer leer, da die Bewertung bestanden wurde.

    ![Fehlerfreie Ressourcen][23]

## <a name="compute-and-app-recommendations"></a>Compute- und App-Empfehlungen
|Ressourcentyp|Sicherheitsbewertung|Empfehlung|BESCHREIBUNG|
|----|----|----|----|
|Computer|50|Monitoring Agent auf Ihren Computern installieren|Installieren Sie den Überwachungs-Agent, um die Datensammlung, das Prüfen auf Updates, die Baselineüberprüfung und Endpoint Protection auf jedem Computer zu aktivieren.|
|Computer|50|Automatische Bereitstellung und Datensammlung für Ihre Abonnements aktivieren |Aktivieren Sie die automatische Bereitstellung und Datensammlung für Computer in Ihren Abonnements, um die Datensammlung, das Prüfen auf Updates, die Baselineüberprüfung und Endpoint Protection auf jedem Computer zu aktivieren, der Ihren Abonnements hinzugefügt wird.|
|Computer|40|Monitoring Agent-Integritätsprobleme auf Ihren Computern beheben|Beheben Sie Monitoring Agent-Probleme auf Ihren Computern mithilfe der Anweisungen im Handbuch zur Problembehandlung, um den vollständigen Security Center-Schutz zu erhalten.| 
|Computer|40|Endpoint Protection-Integritätsprobleme auf Ihren Computern beheben|Beheben Sie Monitoring Agent-Probleme auf Ihren Computern mithilfe der Anweisungen im Handbuch zur Problembehandlung, um den vollständigen Security Center-Schutz zu erhalten.|
|Computer|40|Problembehandlung für fehlende Überprüfungsdaten auf Ihren Computern durchführen|Führen Sie eine Problembehandlung für fehlende Überprüfungsdaten auf virtuellen und physischen Computern durch. Fehlende Überprüfungsdaten auf Ihren Computern führen dazu, dass Sicherheitsbewertungen wie das Prüfen auf Updates, die Baselineüberprüfung und die Überprüfung der Endpoint Protection-Lösung nicht durchgeführt werden.|
|Computer|40|Systemupdates auf Ihren Computern installieren|Installieren Sie fehlende Systemsicherheitsupdates und kritische Updates zum Schutz Ihrer virtuellen und physischen Windows- und Linux-Computer.
|Computer|40|Betriebssystemversion für Ihre Clouddienstrollen aktualisieren|Aktualisieren Sie die Betriebssystemversion für Ihre Clouddienstrollen auf die aktuelle Version für Ihre Betriebssystemfamilie.|
|Computer|35|Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer beseitigen|Beseitigen Sie Sicherheitsrisiken in der Sicherheitskonfiguration Ihrer Computer, um sie vor Angriffen zu schützen. |
|Computer|35|Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Container beseitigen|Beseitigen Sie Sicherheitsrisiken in der Sicherheitskonfiguration von Computern, auf denen Docker installiert ist, um sie vor Angriffen zu schützen.|
|Computer|25|Adaptive Anwendungssteuerung aktivieren|Aktivieren Sie Anwendungssteuerung, um zu steuern, welche Anwendungen auf Ihren virtuellen Computern in Azure ausgeführt werden können. Dadurch sind Ihre virtuellen Computer besser gegen Schadsoftware abgesichert. Security Center nutzt maschinelles Lernen, um die auf den einzelnen virtuellen Computern ausgeführten Anwendungen zu analysieren, und unterstützt Sie beim Anwenden von Zulassungsregeln, die auf diesen Daten basieren. Dadurch können Sie Zulassungsregeln für Anwendungen einfacher konfigurieren und verwalten.|
|Computer|20|Endpoint Protection-Lösung auf Ihren Computern installieren|Installieren Sie eine Endpoint Protection-Lösung auf Ihren virtuellen Computern, um sie vor Bedrohungen und Sicherheitsrisiken zu schützen.|
|Computer|20|Computer zum Anwenden von Systemupdates neu starten|Starten Sie Ihre Computer neu, um die Systemupdates anzuwenden und Ihre Computer vor Sicherheitsrisiken zu schützen.|
|App Service|20|Zugriff auf Webanwendung nur über HTTPS gestatten|Beschränken Sie den Zugriff von Webanwendungen nur auf HTTPS.|
|App Service|20|Zugriff auf Funktions-App nur über HTTPS gestatten|Beschränken Sie den Zugriff von Funktionen-Apps nur auf HTTPS.|
|Computer|15|Datenträgerverschlüsselung auf Ihre virtuellen Computer anwenden|Verschlüsseln Sie die Datenträger Ihrer virtuellen Computer mit Azure Disk Encryption für virtuelle Windows- und Linux-Computer. Azure Disk Encryption (ADE) verwendet das BitLocker-Feature (Branchenstandard) von Windows und das DM-Crypt-Feature von Linux, um das Betriebssystem und den Datenträger zu verschlüsseln. Dies trägt zum Schutz Ihrer Daten und zur Erfüllung der Sicherheits- und Complianceanforderungen im Azure-Schlüsseltresor des Kunden bei. Wenn Ihre Compliance- und Sicherheitsauflagen eine End-to-End-Datenverschlüsselung mithilfe von Verschlüsselungsschlüsseln – einschließlich Verschlüsselung der kurzlebigen (lokal angeschlossenen temporären) Festplatte – vorsehen, nutzen Sie Azure Disk Encryption. Alternativ werden verwaltete Datenträger im Ruhezustand standardmäßig mit der Azure Speicherdienstverschlüsselung verschlüsselt. Hierbei werden von Microsoft verwaltete Schlüssel in Azure als Verschlüsselungsschlüssel verwendet. Wenn dieses Szenario Ihren Compliance- und Sicherheitsanforderungen entspricht, können Sie die standardmäßige Verschlüsselung für verwaltete Datenträger nutzen, um Ihre Anforderungen zu erfüllen.|
|Computeressourcen (Service Fabric)|10|Azure Active Directory für Clientauthentifizierung in Service Fabric verwenden|Führen Sie die Clientauthentifizierung in Service Fabric ausschließlich über Azure Active Directory durch.|
|Computeressourcen (Automation-Konto)|5| Verschlüsselung des Automation-Kontos aktivieren|Aktivieren Sie die Verschlüsselung für Variablenobjekte von Automation-Konten, wenn vertrauliche Daten gespeichert werden.|
|App Service|5|Diagnoseprotokolle in App-Dienst aktivieren|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Lastenausgleich)|5|Diagnoseprotokolle in Load Balancer aktivieren|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Suche)|5|Diagnoseprotokolle in Suchdienst aktivieren|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Service Bus)|5|Diagnoseprotokolle in Service Bus aktivieren|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Stream Analytics)|5|Diagnoseprotokolle in Azure Stream Analytics aktivieren|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Service Fabric)|5|Diagnoseprotokolle in Service Fabric aktivieren|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Batch)|5|Diagnoseprotokolle in Batch-Konten aktivieren|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Event Hub)|5|Diagnoseprotokolle in Event Hub aktivieren|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Logik-Apps)|5|Diagnoseprotokolle in Logic Apps aktivieren|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computer|30|Lösung zur Sicherheitsrisikobewertung auf Ihren virtuellen Computern installieren|Lösung zur Sicherheitsrisikobewertung auf Ihren virtuellen Computern installieren|
|Computer|15|Web Application Firewall hinzufügen| Stellen Sie eine Web Application Firewall (WAF) bereit, um Ihre Webanwendungen zu schützen. |
|Computer|30|Sicherheitsrisiken beseitigen – durch eine Lösung zur Sicherheitsrisikobewertung|Virtuelle Computer, für die eine Drittanbieterlösung zur Sicherheitsrisikobewertung bereitgestellt ist, werden kontinuierlich auf Schwachstellen in Anwendungen und im Betriebssystem überprüft. Wenn solche Sicherheitsrisiken gefunden werden, stehen diese Informationen im Rahmen der Empfehlung zur Verfügung.|
|Computeressourcen (Service Fabric)|15|Eigenschaft „ClusterProtectionLevel“ in Service Fabric auf „EncryptAndSign“ festlegen (Vorschauversion)|Service Fabric bietet drei Schutzebenen („None“, „Sign“ und „EncryptAndSign“) für die Kommunikation zwischen zwei Knoten unter Verwendung eines primären Clusterzertifikats.  Legen Sie die Schutzebene fest, um sicherzustellen, dass alle zwischen zwei Knoten übertragenen Nachrichten verschlüsselt und digital signiert werden. |
|App Service|10|Remotedebuggen muss für Webanwendung deaktiviert werden|Deaktivieren Sie das Debuggen für eine Webanwendung, wenn Sie sie nicht mehr benötigen. Für das Remotedebuggen müssen die eingehenden Ports für eine Funktions-App geöffnet sein.|
|App Service|10|Remotedebuggen muss für Funktionsanwendung deaktiviert werden|Deaktivieren Sie das Debuggen für eine Funktionen-App, wenn Sie sie nicht mehr benötigen. Für das Remotedebuggen müssen die eingehenden Ports für eine Funktions-App geöffnet sein.|
|App Service|10|IP-Einschränkungen für Webanwendung konfigurieren|Definieren Sie eine Liste mit IP-Adressen, die auf Ihre Anwendung zugreifen dürfen. Mithilfe von IP-Einschränkungen kann eine Webanwendung vor gängigen Angriffen geschützt werden.|
|App Service|10|IP-Einschränkungen für Funktions-App konfigurieren| Definieren Sie eine Liste mit IP-Adressen, die auf Ihre Anwendung zugreifen dürfen. Mithilfe von IP-Einschränkungen kann eine Funktions-App vor gängigen Angriffen geschützt werden.|
|App Service|10|Nicht allen („*“) Ressourcen Zugriff auf Ihre Anwendung erteilen| Lassen Sie nicht zu, dass der Parameter „WEBSITE_LOAD_CERTIFICATES“ auf "" gesetzt wird. Durch die Einstellung des Parameters auf ‘’ werden alle Zertifikate in den persönlichen Zertifikatspeicher Ihrer Webanwendungen geladen. Dies kann zum Missbrauch des Prinzips der „geringsten Rechte“ führen, da es unwahrscheinlich ist, dass die Website Zugriff auf sämtliche Zertifikate zur Laufzeit benötigt.|
|App Service|5|WebSockets für Webanwendung deaktivieren|Prüfen Sie die Verwendung von Websockets innerhalb von Webanwendungen. Das WebSockets-Protokoll ist für verschiedene Arten von Sicherheitsrisiken anfällig.|
|App Service|5|WebSockets für Funktions-App deaktivieren|Prüfen Sie die Verwendung von Websockets innerhalb von Funktionen-Apps. Das WebSockets-Protokoll ist für verschiedene Arten von Sicherheitsrisiken anfällig.|
|App Service|5|Benutzerdefinierte Domänen für Webanwendung verwenden|Verwenden Sie benutzerdefinierte Domänen, damit eine Webanwendung vor gängigen Angriffen wie Phishing und anderen DNS-bezogenen Angriffen geschützt wird.|
|App Service|5|Benutzerdefinierte Domänen für Funktions-App verwenden|Verwenden Sie benutzerdefinierte Domänen, damit eine Funktions-App vor gängigen Angriffen wie Phishing und anderen DNS-bezogenen Angriffen geschützt wird.|
|Computeressourcen (Batch)|1|Metrikwarnungsregeln für Batch-Konto konfigurieren|Konfigurieren Sie Metrikwarnungsregeln für das Batch-Konto, und aktivieren Sie die Metriken „Abgeschlossene Ereignisse zum Löschen von Pools“ und „Startereignisse zum Löschen von Pools“.|
|Computeressourcen (Service Bus)|1|Alle Autorisierungsregeln außer RootManageSharedAccessKey aus Service Bus-Namespace entfernen |Service Bus-Clients dürfen keine Zugriffsrichtlinie auf Namespace-Ebene verwenden, die Zugriff auf alle Warteschlangen und Themen in einem Namespace bereitstellt. Um dem Sicherheitsmodell der geringsten Rechte zu entsprechen, müssen Sie Zugriffsrichtlinien auf Entitätsebene erstellen, damit nur der jeweiligen Entität Zugriff auf Warteschlangen und Themen gewährt wird.|
|Computeressourcen (Event Hub)|1|Alle Autorisierungsregeln außer RootManageSharedAccessKey aus Event Hub-Namespace entfernen |Event Hub-Clients dürfen keine Zugriffsrichtlinie auf Namespace-Ebene verwenden, die Zugriff auf alle Warteschlangen und Themen in einem Namespace bereitstellt. Um dem Sicherheitsmodell der geringsten Rechte zu entsprechen, müssen Sie Zugriffsrichtlinien auf Entitätsebene erstellen, damit nur der jeweiligen Entität Zugriff auf Warteschlangen und Themen gewährt wird.|
|Computeressourcen (Event Hub)|5|Autorisierungsregeln für die Event Hub-Entität definieren|Überwachen Sie Autorisierungsregeln in der Event Hub-Entität, um die geringstmöglichen Zugriffsberechtigungen zu erteilen.|
|Computer|30|Lösung zur Sicherheitsrisikobewertung auf Ihren virtuellen Computern installieren|Lösung zur Sicherheitsrisikobewertung auf Ihren virtuellen Computern installieren|
|App Service|20|Nicht jeder Ressource den Zugriff auf Ihre Webanwendungen über CORS gestatten|Ermöglichen Sie nur erforderlichen Domänen die Interaktion mit Ihrer Webanwendung. Bei der Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) sollte nicht allen Domänen gestattet werden, auf Ihre Webanwendung zuzugreifen.|
|App Service|20|Nicht jeder Ressource den Zugriff auf Ihre Funktions-App über CORS gestatten| Ermöglichen Sie nur erforderlichen Domänen die Interaktion mit Ihrer Funktionsanwendung. Bei der Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) sollte nicht allen Domänen gestattet werden, auf Ihre Funktionsanwendung zuzugreifen.|
|Computer|15|Web Application Firewall hinzufügen| Stellen Sie eine Web Application Firewall (WAF) bereit, um Ihre Webanwendungen zu schützen. |
|App Service|10|Die neueste unterstützte .NET Framework-Version für Webanwendungen verwenden|Verwenden Sie die neueste .NET Framework-Version für die neuesten Sicherheitsklassen. Die Verwendung von älteren Klassen und Typen kann die Anfälligkeit Ihrer Anwendung für Sicherheitsrisiken erhöhen.|
|App Service|10|Die neueste unterstützte Java-Version für Webanwendungen verwenden|Verwenden Sie die neueste Java-Version für die neuesten Sicherheitsklassen. Die Verwendung von älteren Klassen und Typen kann die Anfälligkeit Ihrer Anwendung für Sicherheitsrisiken erhöhen.|
|App Service|10|Die neueste unterstützte PHP-Version für Webanwendungen verwenden|Verwenden Sie die neueste PHP-Version für die neuesten Sicherheitsklassen. Die Verwendung von älteren Klassen und Typen kann die Anfälligkeit Ihrer Anwendung für Sicherheitsrisiken erhöhen.|
|App Service|10|Die neueste unterstützte Node.js-Version für Webanwendungen verwenden|Verwenden Sie die neueste Node.js-Version für die neuesten Sicherheitsklassen. Die Verwendung von älteren Klassen und Typen kann die Anfälligkeit Ihrer Anwendung für Sicherheitsrisiken erhöhen.|
|App Service|10|Die neueste unterstützte Python-Version für Webanwendungen verwenden|Verwenden Sie die neueste Python-Version für die neuesten Sicherheitsklassen. Die Verwendung von älteren Klassen und Typen kann die Anfälligkeit Ihrer Anwendung für Sicherheitsrisiken erhöhen.|
|VMs und Computer|1|Virtuelle Computer zu neuen AzureRM-Ressourcen migrieren|Verwenden Sie den neuen Azure Resource Manager v2 für Ihre virtuellen Computer, um von den folgenden Sicherheitsverbesserungen zu profitieren: strengere Zugriffssteuerung (RBAC), bessere Überwachung, ARM-basierte Bereitstellung und Governance, Zugriff auf verwaltete Identitäten, Zugriff auf Schlüsseltresore für Geheimnisse, Azure AD-basierte Authentifizierung und Unterstützung für Markierungen und Ressourcengruppen für eine einfachere Sicherheitsverwaltung. |
|Computer|30|Sicherheitsrisiken beseitigen – durch eine Lösung zur Sicherheitsrisikobewertung|Virtuelle Computer, für die eine Drittanbieterlösung zur Sicherheitsrisikobewertung bereitgestellt ist, werden kontinuierlich auf Schwachstellen in Anwendungen und im Betriebssystem überprüft. Wenn solche Sicherheitsrisiken gefunden werden, stehen diese Informationen im Rahmen der Empfehlung zur Verfügung.|




## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Themen:


* [Grundlegendes zu Azure Security Center-Empfehlungen für virtuelle Computer](security-center-virtual-machine-recommendations.md)
* [Überwachen von Identität und Zugriff in Azure Security Center](security-center-identity-access.md)
* [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md)
* [Schützen Ihres Azure SQL-Diensts in Azure Security Center](security-center-sql-service-recommendations.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.

<!--Image references-->
[1]: ./media/security-center-virtual-machine-recommendations/overview.png
[2]: ./media/security-center-virtual-machine-recommendations/compute.png
[3]: ./media/security-center-virtual-machine-recommendations/monitoring-agent-health-issues.png
[4]: ./media/security-center-virtual-machine-recommendations/compute-recommendations.png
[5]: ./media/security-center-virtual-machine-recommendations/apply-system-updates.png
[6]: ./media/security-center-virtual-machine-recommendations/missing-update-details.png
[7]: ./media/security-center-virtual-machine-recommendations/vm-computers.png
[8]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png
[9]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png
[10]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png
[11]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png
[12]: ./media/security-center-virtual-machine-recommendations/filter.png
[13]: ./media/security-center-virtual-machine-recommendations/vm-detail.png
[14]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png
[15]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new3.png
[16]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png
[17]: ./media/security-center-virtual-machine-recommendations/app-services.png
[18]: ./media/security-center-virtual-machine-recommendations/ase.png
[19]: ./media/security-center-virtual-machine-recommendations/web-app.png
[20]: ./media/security-center-virtual-machine-recommendations/recommendation.png
[21]: ./media/security-center-virtual-machine-recommendations/recommendation-desc.png
[22]: ./media/security-center-virtual-machine-recommendations/passed-assessment.png
[23]: ./media/security-center-virtual-machine-recommendations/healthy-resources.png
[24]: ./media/security-center-virtual-machine-recommendations/function-app.png
