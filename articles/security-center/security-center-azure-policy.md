---
title: Integration von Azure Security Center-Sicherheitsrichtlinien in Azure Policy | Microsoft-Dokumentation
description: In diesem Dokument erfahren Sie, wie Sie die Integration von Azure Security Center-Sicherheitsrichtlinien in Azure Policy konfigurieren.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 5e07cd6891a5ab04012f819b5f6b9379312e530d
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="set-security-policies-in-security-center-powered-by-azure-policy"></a>Festlegen von Sicherheitsrichtlinien in Security Center mit Unterstützung von Azure Policy
Dieses Dokument hilft Ihnen dabei, von Azure Policy unterstützte Sicherheitsrichtlinien in Security Center zu konfigurieren, und beschreibt die dazu erforderlichen Schritte. 


## <a name="how-security-policies-work"></a>Funktionsweise von Sicherheitsrichtlinien
Security Center erstellt für jedes Ihrer Azure-Abonnements automatisch eine Standardsicherheitsrichtlinie. Sie können die Richtlinie in Security Center bearbeiten oder mithilfe von [Azure Policy](http://docs.microsoft.com/azure/azure-policy/azure-policy-introduction) neue Richtliniendefinitionen erstellen, verwaltungsgruppenübergreifende Richtlinien zuweisen (die die gesamte Organisation, eine darin enthaltene Unternehmenseinheit und Ähnliches darstellen können) sowie die Einhaltung dieser Richtlinien bereichsübergreifend überwachen.

> [!NOTE]
> Azure Policy ist als eingeschränkte Vorschauversion verfügbar. Klicken Sie [hier](https://aka.ms/getpolicy), um beizutreten. Weitere Informationen zu Azure-Richtlinien finden Sie unter [Create and manage policies to enforce compliance](http://docs.microsoft.com/en-us/azure/azure-policy/create-manage-policy) (Erstellen und Verwalten von Richtlinien zur Complianceerzwingung).

## <a name="edit-security-policies"></a>Bearbeiten von Sicherheitsrichtlinien
Sie können die Standardsicherheitsrichtlinie für jedes Ihrer Azure-Abonnements in Security Center bearbeiten. Eine Sicherheitsrichtlinie kann nur von einem Besitzer, Mitwirkenden oder Systemadministrator des Abonnements oder der enthaltenden Verwaltungsgruppe geändert werden. Melden Sie sich beim Azure-Portal an, und führen Sie die folgenden Schritte aus, um Ihre Sicherheitsrichtlinien in Security Center anzuzeigen:

1. Klicken Sie auf dem Dashboard **Security Center** unter **Allgemein** auf **Sicherheitsrichtlinie**.
2. Wählen Sie das Abonnement aus, für das Sie die Sicherheitsrichtlinie aktivieren möchten.

    ![Richtlinienverwaltung](./media/security-center-policies/security-center-policies-fig10.png)

3. Klicken Sie im Abschnitt **RICHTLINIENKOMPONENTEN** auf **Sicherheitsrichtlinie**.

    ![Richtlinienkomponenten](./media/security-center-policies/security-center-policies-fig12.png)

4. Dies ist die Standardrichtlinie, die Security Center über Azure Policy zugewiesen ist. Sie können Elemente unter **POLICIES AND PARAMETERS** (RICHTLINIEN UND PARAMETER) löschen oder unter **VERFÜGBARE OPTIONEN** weitere Richtliniendefinitionen hinzufügen. Klicken Sie hierzu einfach neben dem Definitionsnamen auf das Pluszeichen.

    ![Richtliniendefinitionen](./media/security-center-policies/security-center-policies-fig11.png)

5. Wenn Sie eine ausführlichere Erläuterung für die Richtlinie anzeigen möchten, klicken Sie auf die Richtlinie. Daraufhin erscheint eine weitere Seite mit den Details und dem JSON-Code, der die Struktur der [Richtliniendefinition](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy/#policy-definition-structure) enthält:

    ![JSON](./media/security-center-policies/security-center-policies-fig14.png)

6. Klicken Sie nach Abschluss der Bearbeitung auf **Speichern**.


## <a name="available-security-policy-definitions"></a>Verfügbare Sicherheitsrichtliniendefinitionen

Die folgende Tabelle gibt Aufschluss über die verfügbaren Richtliniendefinitionen in der Standardsicherheitsrichtlinie: 

| Richtlinie | Zustand „Ein“ |
| --- | --- |
| Systemupdates |Ruft eine tägliche Liste mit verfügbaren Sicherheitsupdates und kritischen Updates von Windows Update oder Windows Server Update Services ab. Die abgerufene Liste richtet sich nach dem Dienst, der für den virtuellen Computer konfiguriert wurde. Darin wird empfohlen, die fehlenden Updates anzuwenden. Bei Linux-Systemen wird für die Richtlinie das von der Distribution bereitgestellte Paketverwaltungssystem genutzt, um Pakete mit verfügbaren Updates zu ermitteln. Außerdem werden Sicherheitsupdates und kritische Updates für [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md)-VMs ermittelt. |
| Sicherheitsrisiken des Betriebssystems |Führt eine tägliche Analyse von Betriebssystemkonfigurationen durch, um Probleme zu ermitteln, die mit einer Anfälligkeit des virtuellen Computers für Angriffe verbunden sind. Außerdem werden in der Richtlinie Konfigurationsänderungen empfohlen, um diesen Sicherheitsrisiken zu begegnen. Weitere Informationen zu den speziellen Konfigurationen, die überwacht werden, finden Sie in der [Liste mit den empfohlenen Basisregeln](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) . (Zu diesem Zeitpunkt wird Windows Server 2016 nicht vollständig unterstützt.) |
| Endpoint Protection |Empfiehlt die Bereitstellung von Endpoint Protection für alle virtuellen Windows-Computer, um Viren, Spyware und andere Schadsoftware zu erkennen und zu entfernen. |
| Datenträgerverschlüsselung |Empfiehlt die Aktivierung der Datenträgerverschlüsselung auf allen virtuellen Computern, um den Datenschutz im Ruhezustand zu optimieren. |
| Netzwerksicherheitsgruppen |Empfiehlt die Konfiguration von [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) , um den eingehenden und ausgehenden Datenverkehr für VMs mit öffentlichen Endpunkten zu steuern. Für ein Subnetz konfigurierte Netzwerksicherheitsgruppen werden für alle Netzwerkschnittstellen der virtuellen Computer übernommen, sofern nichts anderes angegeben ist. Zusätzlich zur Überprüfung, ob eine Netzwerksicherheitsgruppe konfiguriert wurde, bewertet diese Richtlinie Sicherheitsregeln für eingehende Daten, um Regeln zu identifizieren, die eingehenden Datenverkehr zulassen. |
| Web Application Firewall |Empfiehlt die Bereitstellung einer Web Application Firewall auf virtuellen Computern, wenn eine der folgenden Bedingungen erfüllt ist: </br></br>[Öffentliche IP-Adresse auf Instanzebene](../virtual-network/virtual-networks-instance-level-public-ip.md) (Instance-level Public IP, ILPIP) wird verwendet, und die Eingangssicherheitsregeln für die zugeordnete Netzwerksicherheitsgruppe sind so konfiguriert, dass sie den Zugriff auf Port 80/443 ermöglichen.</br></br>Es wird eine IP-Adresse mit Lastenausgleich verwendet, und die zugehörigen Regeln für Lastenausgleich und eingehenden NAT-Datenverkehr (Netzwerkadressübersetzung) sind so konfiguriert, dass Zugriff auf den Port 80/443 zugelassen wird. Weitere Informationen finden Sie unter [Unterstützung des Azure-Ressourcen-Managers für Load Balancer](../load-balancer/load-balancer-arm.md). |
| Firewall der nächsten Generation |Erweitert den Schutz von Netzwerken über die in Azure integrierten Netzwerksicherheitsgruppen hinaus. Security Center erkennt Bereitstellungen, für die eine Firewall der nächsten Generation empfohlen wird, und ermöglicht Ihnen die Bereitstellung eines virtuellen Geräts. |
| SQL-Überwachung und -Bedrohungserkennung |Empfiehlt die Aktivierung der Zugriffsüberwachung für Azure-Datenbanken zur Erfüllung von Complianceanforderungen sowie die Aktivierung der erweiterten Bedrohungserkennung zu Untersuchungszwecken. |
| SQL-Verschlüsselung |Empfiehlt, dass die Verschlüsselung im Ruhezustand für Ihre Azure SQL-Datenbank, die zugehörigen Sicherungen und die Transaktionsprotokolldateien aktiviert wird. So können Ihre Daten auch dann nicht gelesen werden, wenn unberechtigt darauf zugegriffen wird. |
| Sicherheitsrisikobewertung |Empfiehlt die Installation einer Lösung zur Sicherheitsrisikobewertung auf dem virtuellen Computer. |
| Speicherverschlüsselung |Dieses Feature ist derzeit für Azure-Blobs und -Dateien verfügbar. Nach dem Aktivieren der Speicherdienstverschlüsselung werden nur neue Daten verschlüsselt. Bereits vorhandene Dateien in diesem Speicherkonto bleiben unverschlüsselt. |
| JIT-Netzwerkzugriff |Wenn Just-In-Time aktiviert ist, sperrt das Security Center durch das Erstellen einer NSG-Regel eingehenden Datenverkehr auf den Azure-VMs. Sie wählen die Ports auf dem virtuellen Computer aus, die für eingehenden Datenverkehr gesperrt werden sollen. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf virtuelle Computer mithilfe des Just-In-Time-Features](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |


## <a name="next-step"></a>Nächster Schritt
In diesem Dokument haben Sie erfahren, wie Sie Sicherheitsrichtlinien in Security Center konfigurieren können. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md): Hier erfahren Sie, wie Sie die Entwurfsaspekte in Bezug auf die Einführung von Azure Security Center planen und verstehen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md). Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md). Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md). Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md)festgelegt ist. Enthält häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/). Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.
