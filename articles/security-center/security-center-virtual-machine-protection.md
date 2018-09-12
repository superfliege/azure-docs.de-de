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
ms.date: 08/28/2018
ms.author: rkarlin
ms.openlocfilehash: 1692e111d48a6e4574b2b114c0de84d9bc9f3203
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299830"
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

Sie können die Erweiterung auf dem nicht überwachten virtuellen oder physischen Computer zusätzlich zum bereits installierten lokalen Agent installieren. Konfigurieren Sie beide Agents auf die gleiche Weise, und verbinden Sie sie mit demselben Arbeitsbereich. Security Center kann dann mit der Microsoft Monitoring Agent-Erweiterung interagieren und Daten sammeln. Eine Anleitung zur Installation der Microsoft Monitoring Agent-Erweiterung finden Sie unter [Aktivieren der VM-Erweiterung](../log-analytics/log-analytics-quick-collect-azurevm.md).

Weitere Informationen zu den Gründen, warum Security Center keine VMs und Computer erfolgreich überwachen kann, die für die automatische Bereitstellung initialisiert wurden, finden Sie unter [Integritätsprobleme des Überwachungs-Agents](security-center-troubleshooting-guide.md#mon-agent).

### <a name="recommendations"></a>Empfehlungen
Dieser Abschnitt enthält eine Reihe von Empfehlungen für die einzelnen virtuellen und physischen Computer, Web- und Workerrollen, Azure App Service-Web-Apps und Azure App Service-Umgebungen, die von Security Center überwacht werden. Die erste Spalte enthält die Empfehlung. Die zweite Spalte enthält die Gesamtanzahl der Ressourcen, die diese Empfehlung betrifft. Die dritte Spalte gibt Aufschluss über den Schweregrad des Problems, wie im folgenden Screenshot zu sehen:

![Empfehlungen][4]

Jeder Empfehlung ist eine Reihe von Aktionen zugeordnet, die ausgeführt werden können, nachdem Sie sie ausgewählt haben. Wenn Sie beispielsweise **Fehlende Systemupdates** auswählen, wird wie im folgenden Screenshot die Anzahl der virtuellen und physischen Computer, auf denen Patches fehlen, und der Schweregrad des fehlenden Updates angezeigt:

![Systemupdates anwenden][5]

Für **Systemupdates anwenden** werden zwei Diagramme mit einer Zusammenfassung kritischer Updates angezeigt (eines für Windows, eines für Linux). Der zweite Teil enthält eine Tabelle mit folgenden Informationen:

- **NAME**: Der Name des fehlenden Updates.
- **Anzahl von VMs und Computern**: Die Gesamtzahl virtueller und physischer Computer, auf denen dieses Update fehlt.
- **Updateschweregrad:** Beschreibt den Schweregrad der jeweiligen Empfehlung:

    - **Kritisch:** Ein Sicherheitsrisiko betrifft eine wichtige Ressource (Anwendung, virtueller Computer oder Netzwerksicherheitsgruppe) und erfordert eine Reaktion.
    - **Wichtig:** Es liegt ein nicht kritisches Problem vor, oder es sind zusätzliche Schritte erforderlich, um einen Prozess abzuschließen oder eine Sicherheitslücke zu schließen.
    - **Mittel:** Ein Sicherheitsrisiko, das behandelt werden sollte, jedoch keine unmittelbare Aufmerksamkeit erfordert. (Standardmäßig werden Empfehlungen mit dem Status „Niedrig“ nicht angezeigt, aber Sie können bei Bedarf nach diesen Empfehlungen filtern.)


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



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Themen:


* [Grundlegendes zu Azure Security Center-Empfehlungen für virtuelle Computer](security-center-virtual-machine-recommendations.md)
* [Überwachen von Identität und Zugriff in Azure Security Center](security-center-identity-access.md)
* [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md)
* [Schützen Ihres Azure SQL-Diensts in Azure Security Center](security-center-sql-service-recommendations.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
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
