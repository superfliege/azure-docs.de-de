---
title: Azure Security Center-Richtlinien können entweder individuell oder als Teil von Azure-Richtlinien festgelegt werden | Microsoft-Dokumentation
description: Dieses Dokument enthält Informationen darüber, wie Sie Richtlinien in Azure Security Center oder Azure Policy festlegen können.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/5/2018
ms.author: terrylan
ms.openlocfilehash: 0b38c6895421b43d6f80e0c34cc23b379a673559
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261943"
---
# <a name="setting-security-policies-in-security-center-or-in-azure-policy"></a>Festlegen von Sicherheitsrichtlinien in Security Center oder Azure Policy

Dieser Artikel enthält Informationen darüber, wie Sie Sicherheitsrichtlinien in Azure Security Center konfigurieren. Azure Security Center-Richtlinien können in Azure-Richtlinien integriert werden. Sie können sie entweder in Security Center für ein bestimmtes Abonnement oder in [Azure Policy](../azure-policy/azure-policy-introduction.md) festlegen, was Ihnen ermöglicht, Richtlinien für mehrere Verwaltungsgruppen und Abonnements festzulegen.

## <a name="what-are-security-policies"></a>Was sind Sicherheitsrichtlinien?
Eine Sicherheitsrichtlinie definiert die gewünschte Konfiguration Ihrer Workloads und trägt zur Erfüllung unternehmensbezogener oder gesetzlicher Sicherheitsanforderungen bei. In Azure Security Center können Sie Richtlinien für Ihre Azure-Abonnements definieren und auf die Art der Workload oder auf die Vertraulichkeit der Daten abstimmen. So kann etwa für Anwendungen mit regulierten Daten (beispielsweise personenbezogene Informationen) eine höhere Sicherheitsstufe erforderlich sein als für andere Workloads. Um eine Richtlinie mehreren Abonnements oder Verwaltungsgruppen zuzuweisen, legen Sie diese in [Azure Policy](../azure-policy/azure-policy-introduction.md) fest.

> [!NOTE]
> Falls Sie zuvor Sicherheitsrichtlinien für ein Abonnement konfiguriert haben, das zu einer Verwaltungsgruppe gehört, oder dem bereits mehrere Richtlinien zugewiesen sind, werden diese Richtlinien in Security Center ausgegraut angezeigt, damit Sie die Richtlinie auf der Verwaltungsgruppenebene über die Azure Policy-Seite verwalten können. 

## <a name="how-security-policies-work"></a>Funktionsweise von Sicherheitsrichtlinien
Security Center erstellt für jedes Ihrer Azure-Abonnements automatisch eine Standardsicherheitsrichtlinie. Sie können die Richtlinien in Security Center bearbeiten oder Azure Policy verwenden, um Folgendes zu erreichen:
- Erstellen von neuen Richtliniendefinitionen
- Übergreifendes Zuweisen von Richtlinien für Verwaltungsgruppen und Abonnements, die für eine gesamte Organisation oder eine Geschäftseinheit innerhalb der Organisation stehen können
- Überwachen der Richtlinienkonformität

Weitere Informationen zu Azure Policy finden Sie unter [Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../azure-policy/create-manage-policy.md).

Eine Azure-Richtlinie umfasst die folgenden Komponenten:

- Eine **Richtlinie** ist eine Regel.
- Eine **Initiative** ist eine Sammlung mit Richtlinien.
- Eine **Zuweisung** ist eine Anwendung mit einer Initiative oder Richtlinie für einen bestimmten Bereich (Verwaltungsgruppe, Abonnement oder Ressourcengruppe).

Eine Ressource wird basierend auf den Richtlinien ausgewertet, die ihr zugewiesen sind, und erhält gemäß der Anzahl von Richtlinien, mit denen die Ressource konform ist, einen Konformitätsverhältniswert.

## <a name="who-can-edit-security-policies"></a>Wer kann Sicherheitsrichtlinien bearbeiten?
Security Center verwendet die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC). Dabei werden integrierte Rollen bereitgestellt, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können. In Security Center werden den Benutzern nur Informationen zu den Ressourcen angezeigt, auf die sie Zugriff haben. Das bedeutet, dass Benutzern die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ für das Abonnement oder die Ressourcengruppe einer Ressource zugewiesen wird. Neben diesen Rollen gibt es zwei spezifische Security Center-Rollen:

- Benutzer mit Leseberechtigung für Sicherheitsfunktionen: Kann Informationen in Security Center (wie etwa Empfehlungen, Warnungen, Richtlinien und die Integrität) anzeigen, aber keine Änderungen vornehmen.
- Sicherheitsadministrator: Verfügt über die gleichen Anzeigeberechtigungen wie ein Benutzer mit Leseberechtigung für Sicherheitsfunktionen, ist zusätzlich aber auch zum Aktualisieren der Sicherheitsrichtlinie und zum Verwerfen von Empfehlungen und Warnungen berechtigt.

## <a name="edit-security-policies"></a>Bearbeiten von Sicherheitsrichtlinien
Sie können die Standardsicherheitsrichtlinie für jedes Ihrer Azure-Abonnements und jede Verwaltungsgruppe in Security Center bearbeiten. Eine Sicherheitsrichtlinie kann nur von einem Besitzer, Mitwirkenden oder Systemadministrator des Abonnements oder der enthaltenden Verwaltungsgruppe geändert werden. Zeigen Sie Ihre Sicherheitsrichtlinien in Security Center wie folgt an:

> [!NOTE]
> Alle Richtlinien, die für ein Abonnement festgelegt sind, welches zu einer Verwaltungsgruppe gehört oder dem mehrere Richtlinien zugewiesen sind, werden in Security Center ausgegraut angezeigt. Sie können diese Richtlinien in [Azure Policy](../azure-policy/azure-policy-introduction.md) bearbeiten. 

1. Wählen Sie im Dashboard **Security Center** unter **RICHTLINIE UND KONFORMITÄT** die Option **Sicherheitsrichtlinie**. Der Bereich **Richtlinienverwaltung** wird geöffnet.

    ![Bereich „Richtlinienverwaltung“](./media/security-center-azure-policy/security-center-policies-fig10.png)

  Unter „Richtlinienverwaltung“ wird die Anzahl von Verwaltungsgruppen, Abonnements und Arbeitsbereichen sowie Ihre Verwaltungsgruppenstruktur angezeigt.

  > [!NOTE]
  > Im Security Center-Dashboard wird unter **Abonnementabdeckung** ggf. eine höhere Anzahl von Abonnements als unter **Richtlinienverwaltung** angezeigt. Unter der „Abonnementabdeckung“ wird die Anzahl von Abonnements vom Typ „Standard“, „Free“ und „Nicht abgedeckt“ angezeigt. Für Abonnements vom Typ „Nicht abgedeckt“ ist Security Center nicht aktiviert, und sie werden unter **Richtlinienverwaltung** nicht angezeigt.
  >
  >

  In den Spalten der Tabelle wird Folgendes angezeigt:

 - Zuweisung der Richtlinieninitiative: Integrierte Richtlinien und Initiativen von Security Center, die einem Abonnement oder einer Verwaltungsgruppe zugewiesen sind.
 - Konformität: Die Gesamtbewertung für die Compliance für eine Verwaltungsgruppe, ein Abonnement oder einen Arbeitsbereich. Die Bewertung drückt den gewichteten Mittelwert der Zuweisungen aus. In den gewichteten Mittelwert werden die Anzahl von Richtlinien einer einzelnen Zuweisung und die Anzahl von Ressourcen einbezogen, für die die Zuweisung gilt.

 Wenn Ihr Abonnement beispielsweise über zwei VMs und eine Initiative mit fünf zugewiesenen Richtlinien verfügt, weist Ihr Abonnement zehn Bewertungen auf. Falls eine der VMs die Anforderungen für zwei Richtlinien nicht erfüllt, hat die Gesamtbewertung für die Konformität der Abonnementzuweisung den Wert 80%.

 - Abdeckung: Identifiziert den Tarif („Free“ oder „Standard“), unter dem die Verwaltungsgruppe, das Abonnement oder der Arbeitsbereich ausgeführt werden.  Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md).
 - Einstellungen: Abonnements verfügen über den Link **Einstellungen bearbeiten**. Wenn Sie **Einstellungen bearbeiten** wählen, können Sie Ihre Abonnementeinstellungen aktualisieren, z.B. Datensammlung, Tarif und E-Mail-Benachrichtigungen.

2. Wählen Sie das Abonnement oder die Verwaltungsgruppe aus, für das bzw. die Sie eine Sicherheitsrichtlinie aktivieren möchten. Der Bereich **Sicherheitsrichtlinie** wird geöffnet.

3.  Wählen Sie unter **Sicherheitsrichtlinie** die Steuerelemente aus, die Security Center überwachen und mit Empfehlungen versehen soll, indem Sie **Ein** wählen.  Wählen Sie **Aus**, wenn Sie nicht möchten, dass dieses Steuerelement von Security Center überwacht wird.

    ![Richtlinienkomponenten](./media/security-center-azure-policy/security-policy.png)

4. Wählen Sie **Speichern**aus.

## <a name="available-security-policy-definitions"></a>Verfügbare Sicherheitsrichtliniendefinitionen

Die folgende Tabelle gibt Aufschluss über die verfügbaren Richtliniendefinitionen in der Standardsicherheitsrichtlinie:

| Richtlinie | Zweck der aktivierten Richtlinie |
| --- | --- |
| Systemupdates |Ruft eine tägliche Liste mit verfügbaren Sicherheitsupdates und kritischen Updates von Windows Update oder Windows Server Update Services ab. Die abgerufene Liste richtet sich nach dem Dienst, der für Ihre virtuellen Computer konfiguriert wurde. Darin wird empfohlen, die fehlenden Updates anzuwenden. Bei Linux-Systemen wird für die Richtlinie das von der Distribution bereitgestellte Paketverwaltungssystem genutzt, um Pakete mit verfügbaren Updates zu ermitteln. Außerdem werden Sicherheitsupdates und kritische Updates für [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md)-VMs ermittelt. |
| Sicherheitskonfigurationen |Führt eine tägliche Analyse von Betriebssystemkonfigurationen durch, um Probleme zu ermitteln, die mit einer Anfälligkeit des virtuellen Computers für Angriffe verbunden sind. Außerdem werden in der Richtlinie Konfigurationsänderungen empfohlen, um diesen Sicherheitsrisiken zu begegnen. Weitere Informationen zu den spezifischen Konfigurationen, die überwacht werden, finden Sie in der [Liste mit den empfohlenen Basisregeln](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). (Derzeit wird Windows Server 2016 nicht vollständig unterstützt.) |
| Endpoint Protection |Empfiehlt die Einrichtung von Endpoint Protection für alle virtuellen Windows-Computer (VMs), um Viren, Spyware und andere Schadsoftware zu erkennen und zu entfernen. |
| Datenträgerverschlüsselung |Empfiehlt die Aktivierung der Datenträgerverschlüsselung auf allen virtuellen Computern, um den Datenschutz im Ruhezustand zu optimieren. |
| Netzwerksicherheitsgruppen |Empfiehlt die Konfiguration von [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md) , um den eingehenden und ausgehenden Datenverkehr für VMs mit öffentlichen Endpunkten zu steuern. Für ein Subnetz konfigurierte Netzwerksicherheitsgruppen werden für alle Netzwerkschnittstellen der virtuellen Computer vererbt, sofern nichts anderes angegeben ist. Zusätzlich zur Überprüfung, ob eine Netzwerksicherheitsgruppe konfiguriert wurde, bewertet diese Richtlinie Sicherheitsregeln für eingehende Daten, um Regeln zum Zulassen von eingehendem Datenverkehr zu identifizieren. |
| Web Application Firewall |Empfiehlt die Einrichtung einer Web Application Firewall auf virtuellen Computern, wenn eine der folgenden Bedingungen erfüllt ist: <ul><li>Eine [öffentliche IP-Adresse auf Instanzebene](../virtual-network/virtual-networks-instance-level-public-ip.md) wird verwendet, und die Eingangssicherheitsregeln für die zugeordnete Netzwerksicherheitsgruppe sind so konfiguriert, dass sie den Zugriff auf Port 80/443 ermöglichen.</li><li>Es wird eine IP-Adresse mit Lastenausgleich verwendet, und die zugehörigen Regeln für Lastenausgleich und eingehenden NAT-Datenverkehr (Netzwerkadressübersetzung) sind so konfiguriert, dass Zugriff auf den Port 80/443 zugelassen wird. Weitere Informationen finden Sie unter [Unterstützung des Azure Resource Managers für Load Balancer](../load-balancer/load-balancer-arm.md).</li> |
| Firewall der nächsten Generation |Erweitert den Schutz von Netzwerken über die in Azure integrierten Netzwerksicherheitsgruppen hinaus. Security Center ermittelt Bereitstellungen, für die eine Firewall der nächsten Generation empfohlen wird. Anschließend können Sie ein virtuelles Gerät einrichten. |
| SQL-Überwachung und Bedrohungserkennung |Empfiehlt die Aktivierung der Zugriffsüberwachung für Azure-Datenbanken zur Erfüllung von Konformitätsanforderungen sowie die Aktivierung der erweiterten Bedrohungserkennung zu Untersuchungszwecken. |
| SQL-Verschlüsselung |Empfiehlt, dass die Verschlüsselung im Ruhezustand für Ihre Azure SQL-Datenbank, die zugehörigen Sicherungen und die Transaktionsprotokolldateien aktiviert wird. Auch wenn Ihre Daten ausgespäht werden, sind sie nicht lesbar. |
| Sicherheitsrisikobewertung |Empfiehlt die Installation einer Lösung zur Sicherheitsrisikobewertung auf dem virtuellen Computer. |
| Speicherverschlüsselung |Dieses Feature ist derzeit für Azure Blob Storage und Azure Files verfügbar. Nach dem Aktivieren der Speicherdienstverschlüsselung werden nur neue Daten verschlüsselt. Bereits vorhandene Dateien in diesem Speicherkonto bleiben unverschlüsselt. |
| JIT-Netzwerkzugriff |Wenn der Just-In-Time-Netzwerkzugriff aktiviert ist, sperrt Security Center eingehenden Datenverkehr für Ihre Azure-VMs, indem eine Netzwerksicherheitsgruppen-Regel erstellt wird. Sie wählen die Ports auf dem virtuellen Computer aus, die für eingehenden Datenverkehr gesperrt werden sollen. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf virtuelle Computer mithilfe des Just-In-Time-Features](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |

## <a name="management-groups"></a>Verwaltungsgruppen
Wenn Ihre Organisation über viele Abonnements verfügt, benötigen Sie möglicherweise eine Möglichkeit zur effizienten Verwaltung von Zugriff, Richtlinien und Konformität für diese Abonnements. Azure-Verwaltungsgruppen stellen einen abonnementübergreifenden Bereich dar. Sie organisieren Abonnements in Containern, die als „Verwaltungsgruppen“ bezeichnet werden, und wenden Ihre Governancerichtlinien auf die Verwaltungsgruppen an. Alle Abonnements in einer Verwaltungsgruppe erben automatisch die auf die Verwaltungsgruppe angewendeten Richtlinien. Jedes Verzeichnis erhält eine einzelne Verwaltungsgruppe auf oberster Ebene, die als Stammverwaltungsgruppe (Root) bezeichnet wird. Die Stammverwaltungsgruppe ist in die Hierarchie integriert, sodass ihr alle Verwaltungsgruppen und Abonnements untergeordnet sind. Diese Stammverwaltungsgruppe ermöglicht das Anwenden von globalen Richtlinien und RBAC-Zuweisungen auf Verzeichnisebene. Befolgen Sie die Anleitung im Artikel [Gain tenant-wide visibility for Azure Security Center](security-center-management-groups.md) (Erzielen der mandantenweiten Sichtbarkeit für Azure Security Center), um Verwaltungsgruppen für die Verwendung mit Azure Security Center einzurichten. 

> [!NOTE]
> Es ist wichtig, dass Sie die Hierarchie von Verwaltungsgruppen und Abonnements verstehen. Weitere Informationen zu Verwaltungsgruppen, zur Stammveraltung und zum Zugriff auf Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../governance/management-groups/index.md#root-management-group-for-each-directory).
>
>


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie Sicherheitsrichtlinien in Security Center konfigurieren können. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md): Sie erfahren, wie Sie die Einführung von Azure Security Center planen, und erhalten grundlegende Informationen zu Entwurfsaspekten.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Gain tenant-wide visibility for Azure Security Center](security-center-management-groups.md) (Erzielen der mandantenweiten Sichtbarkeit für Azure Security Center): Es wird beschrieben, wie Sie Verwaltungsgruppen für Azure Security Center einrichten. 
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie Antworten auf häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.

Weitere Informationen zu Azure Policy finden Sie unter [Was ist Azure Policy?](../azure-policy/azure-policy-introduction.md).
