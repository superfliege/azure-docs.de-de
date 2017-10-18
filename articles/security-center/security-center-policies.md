---
title: Festlegen von Sicherheitsrichtlinien in Azure Security Center | Microsoft Docs
description: "Dieses Dokument enthält Informationen dazu, wie Sie Sicherheitsrichtlinien in Azure Security Center konfigurieren können."
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
ms.date: 09/26/2017
ms.author: yurid
ms.openlocfilehash: 67564e930310433bf4d51f7642bdd7ebf7e8e600
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="set-security-policies-in-azure-security-center"></a>Festlegen von Sicherheitsrichtlinien in Azure Security Center
Dieses Dokument hilft Ihnen dabei, Sicherheitsrichtlinien in Security Center zu konfigurieren. Es werden die erforderlichen Schritte zum Ausführen dieser Aufgabe beschrieben.


## <a name="how-security-policies-work"></a>Funktionsweise von Sicherheitsrichtlinien
Security Center erstellt für jedes Ihrer Azure-Abonnements automatisch eine Standardsicherheitsrichtlinie. Sie können die Richtlinie in Security Center bearbeiten oder mithilfe von [Azure Policy](http://docs.microsoft.com/azure/azure-policy/azure-policy-introduction) neue Definitionen erstellen, zusätzliche Richtlinien definieren, verwaltungsgruppenübergreifende Richtlinien zuweisen (die die gesamte Organisation, eine darin enthaltene Unternehmenseinheit und Ähnliches darstellen können) sowie die Einhaltung dieser Richtlinien bereichsübergreifend überwachen.

> [!NOTE]
> Azure Policy ist als eingeschränkte Vorschauversion verfügbar. Klicken Sie [hier](https://aka.ms/getpolicy), um beizutreten. Weitere Informationen zu Azure-Richtlinien finden Sie unter [Create and manage policies to enforce compliance](http://docs.microsoft.com/en-us/azure/azure-policy/create-manage-policy) (Erstellen und Verwalten von Richtlinien zur Complianceerzwingung).

## <a name="how-to-change-security-policies-in-security-center"></a>Ändern von Sicherheitsrichtlinien in Security Center
Sie können die Standardsicherheitsrichtlinie für jedes Ihrer Azure-Abonnements in Security Center bearbeiten. Eine Sicherheitsrichtlinie kann nur von einem Besitzer, Mitwirkenden oder Systemadministrator des Abonnements oder der enthaltenden Verwaltungsgruppe geändert werden. Melden Sie sich beim Azure-Portal an, und führen Sie die folgenden Schritte aus, um Ihre Sicherheitsrichtlinien in Security Center anzuzeigen:

1. Klicken Sie auf dem Dashboard **Security Center** unter **Allgemein** auf **Sicherheitsrichtlinie**.
2. Wählen Sie das Abonnement aus, für das Sie die Sicherheitsrichtlinie aktivieren möchten.

    ![Richtlinienverwaltung](./media/security-center-policies/security-center-policies-fig10.png)

3. Klicken Sie im Abschnitt **RICHTLINIENKOMPONENTEN** auf **Sicherheitsrichtlinie**.

    ![Richtlinienkomponenten](./media/security-center-policies/security-center-policies-fig12.png)

4. Dies ist die Standardrichtlinie, die Security Center über Azure Policy zugewiesen ist. Sie können Elemente unter **POLICIES AND PARAMETERS** (RICHTLINIEN UND PARAMETER) löschen oder unter **VERFÜGBARE OPTIONEN** weitere Richtliniendefinitionen hinzufügen. Klicken Sie hierzu einfach neben dem Definitionsnamen auf das Pluszeichen.

    ![Richtliniendefinitionen](./media/security-center-policies/security-center-policies-fig11.png)

5. Wenn Sie eine ausführlichere Erläuterung für die Richtlinie anzeigen möchten, klicken Sie auf die Richtlinie. Daraufhin erscheint eine weitere Seite mit den Details und dem JSON-Code, der die Struktur der [Richtliniendefinition(https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy#policy-definition-structure) enthält:

    ![JSON](./media/security-center-policies/security-center-policies-fig13.png)

6. Klicken Sie nach Abschluss der Bearbeitung auf **Speichern**.

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument haben Sie erfahren, wie Sie Sicherheitsrichtlinien in Azure Security Center konfigurieren können. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Azure Security Center planning and operations guide](security-center-planning-and-operations-guide.md)festgelegt ist. Hier erfahren Sie, wie Sie die Entwurfsaspekte in Bezug auf die Einführung von Azure Security Center planen und verstehen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md). Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md). Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md). Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md)festgelegt ist. Enthält häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/). Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.
