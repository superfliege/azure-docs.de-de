---
title: Festlegen von Sicherheitsrichtlinien in Azure Security Center | Microsoft Docs
description: "Dieser Artikel enthält Informationen zur Konfiguration der Sicherheitsrichtlinien in Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: b4b33143ad6e24fc40acafbe4ad37b7152d2b8d4
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="set-security-policies-in-azure-security-center"></a>Festlegen von Sicherheitsrichtlinien in Azure Security Center
Dieser Artikel enthält Informationen zur Konfiguration der Sicherheitsrichtlinien in Security Center. 

## <a name="how-security-policies-work"></a>Funktionsweise von Sicherheitsrichtlinien
Security Center erstellt für jedes Ihrer Azure-Abonnements automatisch eine Standardsicherheitsrichtlinie. In Security Center können Sie die Richtlinien bearbeiten und die Einhaltung überwachen. 

> [!NOTE]
> Sie können Security Center-Richtlinien jetzt mithilfe der eingeschränkten Vorschauversion von Azure Policy erweitern. Navigieren Sie zur [Registrierungsseite für Azure Policy](https://aka.ms/getpolicy), um an der Nutzung der Vorschauversion teilzunehmen. Weitere Informationen finden Sie unter [Integrieren von Security Center-Sicherheitsrichtlinien in Azure Policy](security-center-azure-policy.md).

Die Sicherheitsanforderungen für Ressourcen, die zu Entwicklungs- oder Testzwecken verwendet werden, weichen unter Umständen von den Anforderungen der Ressourcen ab, die für Produktionsanwendungen verwendet werden. Für Anwendungen mit reglementierten Daten, z.B. personenbezogene Informationen (Personally Identifiable Information), kann ggf. eine höhere Sicherheitsstufe erforderlich sein. Die in Azure Security Center aktivierten Sicherheitsrichtlinien werden dazu verwendet, Sicherheitsempfehlungen und die Überwachung zu steuern, damit Sie potenzielle Sicherheitsrisiken identifizieren und Bedrohungen entschärfen können. Weitere Informationen dazu, wie Sie die am besten geeignete Option ermitteln können, finden Sie unter [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md).

## <a name="edit-security-policies"></a>Bearbeiten von Sicherheitsrichtlinien
Sie können die Standardsicherheitsrichtlinie für jedes Ihrer Azure-Abonnements in Security Center bearbeiten. Damit Sie eine Sicherheitsrichtlinie ändern können, müssen Sie ein Besitzer, Mitwirkender oder Sicherheitsadministrator des Abonnements sein. Gehen Sie wie folgt vor, um die Sicherheitsrichtlinien in Security Center zu konfigurieren:

1. Melden Sie sich beim Azure-Portal an.

2. Wählen Sie im Dashboard **Security Center** unter **Allgemein** die Option **Sicherheitsrichtlinie**.

3. Wählen Sie das Abonnement aus, für das Sie eine Sicherheitsrichtlinie aktivieren möchten.

4. Wählen Sie im Abschnitt **Richtlinienkomponenten** die Option **Sicherheitsrichtlinie**.  
    Dies ist die Standardrichtlinie, die von Security Center zugewiesen wird. Sie können die verfügbaren Sicherheitsempfehlungen aktivieren oder deaktivieren.

5. Wählen Sie **Speichern**, wenn Sie die Bearbeitung abgeschlossen haben.

## <a name="available-security-policy-definitions"></a>Verfügbare Sicherheitsrichtliniendefinitionen

Die folgende Tabelle gibt Aufschluss über die verfügbaren Richtliniendefinitionen in der Standardsicherheitsrichtlinie:

| Richtlinie | Zweck der Richtlinie |
| --- | --- |
| Systemupdates |Ruft eine tägliche Liste mit verfügbaren Sicherheitsupdates und kritischen Updates von Windows Update oder Windows Server Update Services ab. Die abgerufene Liste richtet sich nach dem Dienst, der für Ihre virtuellen Computer konfiguriert wurde. Darin wird empfohlen, die fehlenden Updates anzuwenden. Bei Linux-Systemen wird für die Richtlinie das von der Distribution bereitgestellte Paketverwaltungssystem genutzt, um Pakete mit verfügbaren Updates zu ermitteln. Außerdem werden Sicherheitsupdates und kritische Updates für [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md)-VMs ermittelt. |
| Sicherheitsrisiken des Betriebssystems |Führt eine tägliche Analyse von Betriebssystemkonfigurationen durch, um Probleme zu ermitteln, die mit einer Anfälligkeit des virtuellen Computers für Angriffe verbunden sind. Außerdem werden in der Richtlinie Konfigurationsänderungen empfohlen, um diesen Sicherheitsrisiken zu begegnen. Weitere Informationen zu den spezifischen Konfigurationen, die überwacht werden, finden Sie in der [Liste mit den empfohlenen Basisregeln](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). (Zu diesem Zeitpunkt wird Windows Server 2016 nicht vollständig unterstützt.) |
| Endpoint Protection |Empfiehlt die Einrichtung von Endpoint Protection für alle virtuellen Windows-Computer (VMs), um Viren, Spyware und andere Schadsoftware zu erkennen und zu entfernen. |
| Datenträgerverschlüsselung |Empfiehlt die Aktivierung der Datenträgerverschlüsselung auf allen virtuellen Computern, um den Datenschutz im Ruhezustand zu optimieren. |
| Netzwerksicherheitsgruppen |Empfiehlt die Konfiguration von [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) , um den eingehenden und ausgehenden Datenverkehr für VMs mit öffentlichen Endpunkten zu steuern. Für ein Subnetz konfigurierte Netzwerksicherheitsgruppen werden für alle Netzwerkschnittstellen der virtuellen Computer vererbt, sofern nichts anderes angegeben ist. Zusätzlich zur Überprüfung, ob eine Netzwerksicherheitsgruppe konfiguriert wurde, bewertet diese Richtlinie Sicherheitsregeln für eingehende Daten, um Regeln zum Zulassen von eingehendem Datenverkehr zu identifizieren. |
| Web Application Firewall |Empfiehlt die Einrichtung einer Web Application Firewall auf virtuellen Computern, wenn eine der folgenden Bedingungen erfüllt ist: <ul><li>Eine [öffentliche IP-Adresse auf Instanzebene](../virtual-network/virtual-networks-instance-level-public-ip.md) wird verwendet, und die Eingangssicherheitsregeln für die zugeordnete Netzwerksicherheitsgruppe sind so konfiguriert, dass sie den Zugriff auf Port 80/443 ermöglichen.</li><li>Es wird eine IP-Adresse mit Lastenausgleich verwendet, und die zugehörigen Regeln für Lastenausgleich und eingehenden NAT-Datenverkehr (Netzwerkadressübersetzung) sind so konfiguriert, dass Zugriff auf den Port 80/443 zugelassen wird. Weitere Informationen finden Sie unter [Unterstützung des Azure Resource Managers für Load Balancer](../load-balancer/load-balancer-arm.md).</li> |
| Firewall der nächsten Generation |Erweitert den Schutz von Netzwerken über die in Azure integrierten Netzwerksicherheitsgruppen hinaus. Security Center ermittelt Bereitstellungen, für die eine Firewall der nächsten Generation empfohlen wird. Anschließend können Sie ein virtuelles Gerät einrichten. |
| SQL-Überwachung und Bedrohungserkennung |Empfiehlt die Aktivierung der Zugriffsüberwachung für Ihre SQL-Datenbank zur Erfüllung von Konformitätsanforderungen sowie zur Aktivierung der erweiterten Bedrohungserkennung zu Untersuchungszwecken. |
| SQL-Verschlüsselung |Empfiehlt, dass die Verschlüsselung im Ruhezustand für Ihre SQL-Datenbank, die zugehörigen Sicherungen und die Transaktionsprotokolldateien aktiviert wird. Auch wenn Ihre Daten ausgespäht werden, sind sie nicht lesbar. |
| Sicherheitsrisikobewertung |Empfiehlt die Installation einer Lösung zur Sicherheitsrisikobewertung auf dem virtuellen Computer. |
| Speicherverschlüsselung |Dieses Feature ist derzeit für Blobs und Azure Files verfügbar. Nach dem Aktivieren der Speicherdienstverschlüsselung werden nur neue Daten verschlüsselt. Bereits vorhandene Dateien in diesem Speicherkonto bleiben unverschlüsselt. |
| JIT-Netzwerkzugriff |Wenn der Just-In-Time-Netzwerkzugriff aktiviert ist, sperrt Security Center eingehenden Datenverkehr für Ihre Azure-VMs, indem eine Netzwerksicherheitsgruppen-Regel erstellt wird. Sie wählen die Ports auf dem virtuellen Computer aus, die für eingehenden Datenverkehr gesperrt werden sollen. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf virtuelle Computer mithilfe des Just-In-Time-Features](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie Sicherheitsrichtlinien in Security Center konfigurieren können. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md): Sie erfahren, wie Sie die Einführung von Azure Security Center planen, und erhalten grundlegende Informationen zu Entwurfsaspekten.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie Antworten auf häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.
