---
title: Azure Security Center-Sicherheitsrichtlinien können im Rahmen von Azure-Richtlinien festgelegt und in Security Center angezeigt werden | Microsoft-Dokumentation
description: Dieses Dokument enthält Informationen dazu, wie Sie Richtlinien in Azure Policy festlegen oder in Azure Security Center anzeigen.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/25/2018
ms.author: rkarlin
ms.openlocfilehash: 330b66e64484417e50f39c35cf90a6fd62b1e888
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334668"
---
# <a name="view-security-policies"></a>Anzeigen von Sicherheitsrichtlinien

In diesem Artikel wird beschrieben, wie Sicherheitsrichtlinien konfiguriert werden und wie Sie sie in Security Center anzeigen. Azure Security Center weist seine [integrierten Sicherheitsrichtlinien](security-center-policy-definitions.md) automatisch für jedes Abonnement zu, für das das Onboarding durchgeführt wird. Sie können sie in [Azure Policy](../azure-policy/azure-policy-introduction.md) konfigurieren und haben hierbei auch die Möglichkeit, Richtlinien übergreifend für Verwaltungsgruppen und mehrere Abonnements festzulegen.

Anweisungen zum Einrichten von Richtlinien mit PowerShell finden Sie unter [Schnellstartanleitung: Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen mithilfe des Azure RM-PowerShell-Moduls](../azure-policy/assign-policy-definition-ps.md).



## <a name="what-are-security-policies"></a>Was sind Sicherheitsrichtlinien?
Eine Sicherheitsrichtlinie definiert die gewünschte Konfiguration Ihrer Workloads und trägt zur Erfüllung unternehmensbezogener oder gesetzlicher Sicherheitsanforderungen bei. In Azure Policy können Sie Richtlinien für Ihre Azure-Abonnements definieren und auf die Art der Workload oder auf die Vertraulichkeit der Daten abstimmen. So kann etwa für Anwendungen mit regulierten Daten (beispielsweise personenbezogene Informationen) eine höhere Sicherheitsstufe erforderlich sein als für andere Workloads. Um eine Richtlinie mehreren Abonnements oder Verwaltungsgruppen zuzuweisen, legen Sie diese in [Azure Policy](../azure-policy/azure-policy-introduction.md) fest.



Ihre Sicherheitsrichtlinien sind die Grundlage der Sicherheitsempfehlungen, die Sie in Azure Security Center erhalten. Sie können ihre Einhaltung überwachen, damit Sie potenzielle Sicherheitsrisiken identifizieren und Bedrohungen eindämmen können. Weitere Informationen zur Ermittlung der Option, die für Sie geeignet ist, finden Sie in der Liste mit den [integrierten Sicherheitsrichtlinien](security-center-policy-definitions.md).

### <a name="management-groups"></a>Verwaltungsgruppen
Wenn Ihre Organisation über viele Abonnements verfügt, benötigen Sie möglicherweise eine Möglichkeit zur effizienten Verwaltung von Zugriff, Richtlinien und Konformität für diese Abonnements. Azure-Verwaltungsgruppen stellen einen abonnementübergreifenden Bereich dar. Sie organisieren Abonnements in Containern, die als „Verwaltungsgruppen“ bezeichnet werden, und wenden Ihre Governancerichtlinien auf die Verwaltungsgruppen an. Alle Abonnements in einer Verwaltungsgruppe erben automatisch die auf die Verwaltungsgruppe angewendeten Richtlinien. Jedes Verzeichnis erhält eine einzelne Verwaltungsgruppe auf oberster Ebene, die als Stammverwaltungsgruppe (Root) bezeichnet wird. Die Stammverwaltungsgruppe ist in die Hierarchie integriert, sodass ihr alle Verwaltungsgruppen und Abonnements untergeordnet sind. Diese Stammverwaltungsgruppe ermöglicht das Anwenden von globalen Richtlinien und RBAC-Zuweisungen auf Verzeichnisebene. Befolgen Sie die Anleitung unter [Erzielen der mandantenweiten Sichtbarkeit für Azure Security Center](security-center-management-groups.md), um Verwaltungsgruppen für die Verwendung mit Azure Security Center einzurichten.

> [!NOTE]
> Es ist wichtig, dass Sie die Hierarchie von Verwaltungsgruppen und Abonnements verstehen. Weitere Informationen zu Verwaltungsgruppen, zur Stammveraltung und zum Zugriff auf Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../governance/management-groups/index.md#root-management-group-for-each-directory).
>

## <a name="how-security-policies-work"></a>Funktionsweise von Sicherheitsrichtlinien
Security Center erstellt für jedes Ihrer Azure-Abonnements automatisch eine Standardsicherheitsrichtlinie. Sie können die Richtlinien in Azure Policy bearbeiten, um Folgendes zu erreichen:
- Erstellen von neuen Richtliniendefinitionen
- Übergreifendes Zuweisen von Richtlinien für Verwaltungsgruppen und Abonnements, die für eine gesamte Organisation oder eine Geschäftseinheit innerhalb der Organisation stehen können
- Überwachen der Richtlinienkonformität

Weitere Informationen zu Azure Policy finden Sie unter [Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../azure-policy/create-manage-policy.md).

Eine Azure-Richtlinie umfasst die folgenden Komponenten:

- Eine **Richtlinie** ist eine Regel.
- Eine **Initiative** ist eine Sammlung mit Richtlinien.
- Eine **Zuweisung** ist die Anwendung einer Initiative oder Richtlinie auf einen bestimmten Bereich (Verwaltungsgruppe, Abonnement oder Ressourcengruppe).

Eine Ressource wird basierend auf den Richtlinien ausgewertet, die ihr zugewiesen sind, und erhält gemäß der Anzahl von Richtlinien, mit denen die Ressource konform ist, einen Konformitätsverhältniswert.

## <a name="who-can-edit-security-policies"></a>Wer kann Sicherheitsrichtlinien bearbeiten?
Security Center verwendet die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC). Dabei werden integrierte Rollen bereitgestellt, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können. In Security Center werden den Benutzern nur Informationen zu den Ressourcen angezeigt, auf die sie Zugriff haben. Das bedeutet, dass Benutzern die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ für das Abonnement oder die Ressourcengruppe einer Ressource zugewiesen wird. Neben diesen Rollen gibt es zwei spezifische Security Center-Rollen:

- Benutzer mit Leseberechtigung für Sicherheitsfunktionen: Kann Informationen in Security Center (wie etwa Empfehlungen, Warnungen, Richtlinien und die Integrität) anzeigen, aber keine Änderungen vornehmen.
- Sicherheitsadministrator: Verfügt über die gleichen Anzeigeberechtigungen wie ein Benutzer mit Leseberechtigung für Sicherheitsfunktionen, ist zusätzlich aber auch zum Aktualisieren der Sicherheitsrichtlinie und zum Verwerfen von Empfehlungen und Warnungen berechtigt.

## <a name="edit-security-policies"></a>Bearbeiten von Sicherheitsrichtlinien
Sie können die Standardsicherheitsrichtlinie für jedes Ihrer Azure-Abonnements und jede Verwaltungsgruppe in [Azure Policy](../governance/policy/tutorials/create-and-manage.md) bearbeiten. Eine Sicherheitsrichtlinie kann nur von einem Besitzer, Mitwirkenden oder Systemadministrator des Abonnements oder der enthaltenden Verwaltungsgruppe geändert werden.

Eine Anleitung zur Bearbeitung einer Sicherheitsrichtlinie in Azure Policy finden Sie unter [Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../governance/policy/tutorials/create-and-manage.md).

## <a name="view-security-policies"></a>Anzeigen von Sicherheitsrichtlinien

Zeigen Sie Ihre Sicherheitsrichtlinien in Security Center wie folgt an:

1. Wählen Sie im **Security Center**-Dashboard die Option **Sicherheitsrichtlinie**.

    ![Bereich „Richtlinienverwaltung“](./media/security-center-policies/security-center-policy-mgt.png)

  Auf dem Bildschirm **Richtlinienverwaltung** können Sie die Anzahl von Verwaltungsgruppen, Abonnements und Arbeitsbereichen sowie Ihre Verwaltungsgruppenstruktur anzeigen.

  > [!NOTE]
  > - Im Security Center-Dashboard wird unter **Abonnementabdeckung** ggf. eine höhere Anzahl von Abonnements als unter **Richtlinienverwaltung** angezeigt. Unter der „Abonnementabdeckung“ wird die Anzahl von Abonnements vom Typ „Standard“, „Free“ und „Nicht abgedeckt“ angezeigt. Für Abonnements vom Typ „Nicht abgedeckt“ ist Security Center nicht aktiviert, und sie werden unter **Richtlinienverwaltung** nicht angezeigt.
  >

  In den Spalten der Tabelle wird Folgendes angezeigt:

 - **Zuweisung der Richtlinieninitiative**: [Integrierte Richtlinien](security-center-policy-definitions.md) und Initiativen von Security Center, die einem Abonnement oder einer Verwaltungsgruppe zugewiesen sind.
 - **Konformität**: Die Gesamtbewertung zur Konformität (Compliance) für eine Verwaltungsgruppe, ein Abonnement oder einen Arbeitsbereich. Die Bewertung drückt den gewichteten Mittelwert der Zuweisungen aus. In den gewichteten Mittelwert werden die Anzahl von Richtlinien einer einzelnen Zuweisung und die Anzahl von Ressourcen einbezogen, für die die Zuweisung gilt.

 Wenn Ihr Abonnement beispielsweise über zwei VMs und eine Initiative mit fünf zugewiesenen Richtlinien verfügt, weist Ihr Abonnement zehn Bewertungen auf. Falls eine der VMs die Anforderungen für zwei Richtlinien nicht erfüllt, hat die Gesamtbewertung für die Konformität der Abonnementzuweisung den Wert 80%.

 - **Abdeckung**: Identifiziert den Tarif („Free“ oder „Standard“), unter dem die Verwaltungsgruppe, das Abonnement oder der Arbeitsbereich ausgeführt werden.  Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md).
 - **Einstellungen**: Abonnements verfügen über den Link **Einstellungen bearbeiten**. Indem Sie **Einstellungen bearbeiten** wählen, können Sie Ihre [Security Center-Einstellungen](security-center-policies-overview.md) für jedes Abonnement bzw. jede Verwaltungsgruppe aktualisieren.

2. Wählen Sie das Abonnement oder die Verwaltungsgruppe aus, zu dem bzw. der Sie die Richtlinien anzeigen möchten.

  - Auf dem Bildschirm **Sicherheitsrichtlinie** wird die Aktion der Richtlinien widergespiegelt, die dem von Ihnen gewählten Abonnement oder der Verwaltungsgruppe zugewiesen sind.
  - Verwenden Sie oben die angegebenen Links, um die einzelnen **Richtlinienzuweisungen** zu öffnen, die für das Abonnement bzw. die Verwaltungsgruppe gelten. Sie können die Links nutzen, um auf die Zuweisung zuzugreifen und die Richtlinie zu bearbeiten oder zu deaktivieren. Wenn Sie beispielsweise feststellen, dass eine bestimmte Zuweisung den Endpunktschutz verhindert, können Sie über den Link auf die Richtlinie zugreifen und sie bearbeiten oder deaktivieren.
  - In der Liste mit den Richtlinien können Sie die aktive Anwendung der Richtlinie auf Ihr Abonnement oder die Verwaltungsgruppe anzeigen. Dies bedeutet Folgendes: Die Einstellungen der einzelnen Richtlinien, die für den Bereich gelten, werden berücksichtigt, und für Sie wird das kumulierte Ergebnis dazu angegeben, welche Aktion von der Richtlinie durchgeführt wird. Wenn die Richtlinie beispielsweise in einer Zuweisung deaktiviert wird, aber in einer anderen auf „AuditIfNotExist“ festgelegt ist, wird aufgrund der Kumulation „AuditIfNotExist“ angewendet. Die aktivere Auswirkung hat immer Vorrang.
  - Für die Richtlinien können sich die folgenden Auswirkungen ergeben: Append, Audit, AuditIfNotExists, Deny, DeployIfNotExists, Disabled. Weitere Informationen zur Anwendung von Auswirkungen finden Sie unter [Grundlegendes zu Richtlinienauswirkungen](../governance/policy/concepts/effects.md).

   ![Bildschirm mit Richtlinien](./media/security-center-policies/policy-screen.png)

> [!NOTE]
> - Beim Anzeigen von zugewiesenen Richtlinien können Sie mehrere Zuweisungen sehen und die jeweilige Konfiguration der Zuweisungen auch einzeln prüfen.

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
