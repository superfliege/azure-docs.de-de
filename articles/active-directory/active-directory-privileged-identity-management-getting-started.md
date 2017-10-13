---
title: Erste Schritte mit Azure AD Privileged Identity Management | Microsoft Docs
description: "Erfahren Sie, wie Sie mit der Anwendung Azure Active Directory Privileged Identity Management privilegierte Identitäten im Azure-Portal verwalten."
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: 0f9f09ca8fb30d494433ed8d26b808d1b5d4d0b1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Erste Schritte mit Azure AD Privileged Identity Management

Mit Azure Active Directory (AD) Privileged Identity Management können Sie den Zugriff innerhalb Ihrer Organisation verwalten, steuern und überwachen. Dazu zählt der Zugriff auf Azure-Ressourcen, Azure AD und andere Microsoft-Onlinedienste wie Office 365 oder Microsoft Intune.

Dieser Artikel beschreibt, wie Sie die Azure AD PIM-App zu Ihrem Azure-Portaldashboard hinzufügen.

## <a name="add-the-privileged-identity-management-application"></a>Hinzufügen der Anwendung Privileged Identity Management

Bevor Sie Azure AD Privileged Identity Management verwenden können, müssen Sie die Anwendung zum Dashboard in Ihrem Azure-Portal hinzufügen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Verzeichnisses an.
2. Wenn Ihre Organisation über mehr als ein Verzeichnis verfügt, klicken Sie in der oberen rechten Ecke des Azure-Portals auf Ihren Benutzernamen. Wählen Sie das Verzeichnis aus, in dem Sie PIM verwenden möchten.
3. Wählen Sie **Weitere Dienste** aus, und verwenden Sie das Textfeld „Filter“, um nach **Azure AD Privileged Identity Management** zu suchen.
4. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie dann auf **Erstellen**. Die Anwendung Privileged Identity Management wird geöffnet.

Wenn Sie die erste Person sind, die Azure AD Privileged Identity Management in Ihrem Verzeichnis verwendet, werden Ihnen automatisch die Rollen **Sicherheitsadministrator** und **Administrator für privilegierte Rollen** im Verzeichnis zugewiesen. Nur Administratoren für privilegierte Rollen können Azure AD-Verzeichnisrollenzuweisungen von Benutzern verwalten. Darüber hinaus können Sie den [Sicherheits-Assistenten](active-directory-privileged-identity-management-security-wizard.md) ausführen. Dieser Assistent führt Sie durch die Erstermittlung und -zuweisung.

## <a name="navigate-to-your-tasks"></a>Navigieren zu Ihren Aufgaben

Nachdem Azure AD Privileged Identity Management eingerichtet wurde, wird beim Öffnen der Anwendung immer das Blatt für die Navigation angezeigt. Verwenden Sie dieses Blatt für Ihre Aufgaben der Identitätsverwaltung.

![Aufgaben der obersten Ebene für PIM – Screenshot](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

- Unter **My Roles** (Meine Rollen) wird eine Liste mit berechtigten und aktiven Rollen angezeigt, die Ihnen zugewiesen sind. Hier können Sie zugewiesene berechtigte Rollen aktivieren.
- **Anforderungen genehmigen (Vorschauversion)** zeigt eine Liste mit Anforderungen zum Aktivieren berechtigter Azure AD-Verzeichnisrollen nach Benutzern in Ihrem Verzeichnis an, für deren Genehmigung Sie zuständig sind. [Weitere Informationen.](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
- **Ausstehende Anforderungen (Vorschauversion)** zeigt Ihre ausstehenden Anforderungen zum Aktivieren berechtigter Rollenzuweisungen an.
- Unter **Zugriff überprüfen** wird eine Liste mit aktiven Zugriffsüberprüfungen angezeigt, die Ihnen zugewiesen sind. Dabei spielt es keine Rolle, ob Sie den Zugriff für sich selbst oder für eine andere Person überprüfen.
- Im Verwaltungsabschnitt des linken Navigationsmenüs befindet sich unter **Azure AD-Verzeichnisrollen** das Dashboard für Administratoren für privilegierte Rollen. Es dient unter anderem zum Verwalten von Rollenzuweisungen, zum Ändern der Einstellungen für die Rollenaktivierung sowie zum Starten von Zugriffsüberprüfungen. Für Benutzer, die kein Administrator für privilegierte Rollen sind, ist dieses Dashboard deaktiviert. Diese Benutzer haben Zugriff auf ein spezielles Dashboard namens „Meine Ansicht“. Auf dem Dashboard „Meine Ansicht“ werden nur Informationen zu dem Benutzer angezeigt, der auf das Dashboard zugreift (also keine Informationen für den gesamten Mandanten).
- Im Verwaltungsabschnitt des linken Navigationsmenüs wird unter **Azure Resource roles** (Azure-Ressourcenrollen) (Vorschauversion) eine Liste mit Ihren ausgewählten Abonnementressourcen angezeigt. 

## <a name="next-steps"></a>Nächste Schritte
Die Übersicht über [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) enthält weitere Details zur Verwaltung des administrativen Zugriffs in Ihrer Organisation.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
