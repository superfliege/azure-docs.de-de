---
title: Konfigurieren von Azure AD Privileged Identity Management | Microsoft-Dokumentation
description: "Ein Thema, in dem erläutert wird, was Azure AD Privileged Identity Management ist und wie Sie damit die Sicherheit in Ihrer Cloud erhöhen."
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.assetid: c548ed2e-06e3-4eaf-a63d-0f02ee72da25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: barclayn
ms.custom: pim
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: e22d896b47588e867f4f4d2b07de3b86d5cd19e5
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Was ist Azure AD Privileged Identity Management?

Mit Azure Active Directory (AD) Privileged Identity Management können Sie den Zugriff innerhalb Ihrer Organisation verwalten, steuern und überwachen. Dazu zählt der Zugriff auf Ressourcen in Azure AD, in Azure-Ressourcen (Vorschauversion) und anderen Microsoft-Onlinediensten wie Office 365 oder Microsoft Intune.

> [!NOTE]
> Privileged Identity Management ist für Ihre gesamte Organisation verfügbar, wenn Sie Ihre Administratoren mit der Premium P2-Edition von Azure Active Directory lizenzieren. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

Organisationen möchten die Anzahl von Personen mit Zugriff auf sichere Informationen oder Ressourcen möglichst gering halten, da sich dadurch das Risiko verringert, dass ein böswilliger Benutzer Zugriff darauf erhält oder dass ein autorisierter Benutzer versehentlich eine sensible Ressource kompromittiert.  Benutzer müssen jedoch in Azure AD, Azure, Office 365 oder SaaS-Apps weiterhin privilegierte Vorgänge ausführen. Organisationen können Benutzern privilegierten Zugriff auf Azure-Ressourcen wie Abonnements und Azure AD gewähren. Dabei muss jedoch überwacht werden, wofür diese Benutzer ihre Administratorrechte nutzen. Azure AD Privileged Identity Management trägt dazu bei, das Risiko durch unverhältnismäßige oder unnötige Zugriffsrechte bzw. durch deren Missbrauch zu verringern.

Azure AD Privileged Identity Management ermöglicht Ihrer Organisation Folgendes:

- Ermitteln, welchen Benutzern privilegierte Rollen für die Verwaltung von Azure-Ressourcen (Vorschauversion) und welchen Benutzern Administratorrollen in Azure AD zugewiesen sind
- Aktivieren von bedarfsgesteuertem Just-In-Time-Administratorzugriff auf Microsoft-Onlinedienste wie Office 365 und Intune sowie auf Azure-Ressourcen (Vorschauversion) von Abonnements, Ressourcengruppen und Einzelressourcen wie virtuellen Computern 
-   Anzeigen eines Administratoraktivierungsverlaufs, der unter anderem Aufschluss über die von Administratoren vorgenommenen Änderungen an Azure-Ressourcen (Vorschauversion) gibt
- Erhalten von Benachrichtigungen bei geänderten Administratorzuweisungen
- Erzwingen einer Genehmigung für die Aktivierung privilegierter Azure AD-Administratorrollen (Vorschauversion) 
- Überprüfen der Mitgliedschaft von Administratorrollen und Erzwingen der Angabe einer Begründung bei längerer Mitgliedschaft

In Azure AD kann Azure AD Privileged Identity Management die Benutzer verwalten, die den integrierten Azure AD-Organisationsrollen zugewiesen sind (beispielsweise „Globaler Administrator“). In Azure kann Azure AD Privileged Identity Management die Benutzer und Gruppen verwalten, die über Azure RBAC-Rollen zugewiesen werden (einschließlich „Besitzer“ oder „Mitwirkender“).

## <a name="just-in-time-administrator-access"></a>Bedarfsabhängiger Administratorzugriff

In der Vergangenheit konnten Sie einen Benutzer über das Azure-Portal, über andere Portale von Microsoft-Onlinediensten oder über die Azure AD-Cmdlets in Windows PowerShell einer Administratorrolle zuweisen. Dadurch wird der Benutzer zum **permanenten Administrator**, der in seiner zugewiesenen Rolle stets aktiv bleibt. Mit Azure AD Privileged Identity Management wird das Konzept **berechtigter Administratoren** eingeführt. Berechtigte Administratoren sollten Benutzer sein, die von Zeit zu Zeit (aber nicht ständig und jeden Tag) Zugriff mit erhöhten Rechten benötigen. Die Rolle ist inaktiv, bis der Benutzer Zugriff benötigt. Dann wird eine Aktivierung ausgeführt, und der Benutzer wird für einen zuvor festgelegten Zeitraum zu einem aktiven Administrator. Immer mehr Unternehmen entscheiden sich für diesen Ansatz, um den ständigen Administratorzugriff auf privilegierte Rollen zu verringern oder ganz zu beseitigen.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Aktivieren von Privileged Identity Management für Ihr Verzeichnis

Im [Azure-Portal](https://portal.azure.com/)können Sie mit der Nutzung von Azure AD Privileged Identity Management beginnen.

> [!NOTE]
> Sie müssen ein globaler Administrator mit einem Geschäftskonto (wie @yourdomain.com) und keinem Microsoft-Konto (wie @outlook.com) sein, um Azure AD Privileged Identity Management für ein Verzeichnis zu aktivieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Verzeichnisses an.
2. Wenn Ihre Organisation über mehr als ein Verzeichnis verfügt, klicken Sie in der oberen rechten Ecke des Azure-Portals auf Ihren Benutzernamen. Wählen Sie das Verzeichnis aus, in dem Sie Azure AD Privileged Identity Management verwenden möchten.
3. Wählen Sie **Weitere Dienste** aus, und verwenden Sie das Textfeld „Filter“, um nach **Azure AD Privileged Identity Management** zu suchen.
4. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie dann auf **Erstellen**. Die Anwendung Privileged Identity Management wird geöffnet.

Wenn Sie die erste Person sind, die Azure AD Privileged Identity Management in Ihrem Verzeichnis verwendet, und Sie zu Azure AD-Verzeichnisrollen navigieren, führt Sie der [Sicherheits-Assistent](active-directory-privileged-identity-management-security-wizard.md) durch die erste Zuweisung. Danach sind Sie automatisch der erste **Sicherheitsadministrator** und der **Administrator für privilegierte Rollen** des Verzeichnisses.

Für Azure AD-Rollen gilt: Nur ein Benutzer mit der Rolle eines Administrators für privilegierte Rollen kann Zuweisungen für andere Administratoren in Azure AD PIM verwalten. Sie können [anderen Benutzern die Verwaltung von Verzeichnisrollen in PIM ermöglichen](active-directory-privileged-identity-management-how-to-give-access-to-pim.md). Globale Administratoren, Sicherheitsadministratoren und Benutzer mit Leseberechtigung für Sicherheitsfunktionen können Zuweisungen zu Azure AD-Rollen in Azure AD PIM anzeigen.
Bei Azure RBAC-Rollen können nur Abonnementadministratoren, Ressourcenbesitzer und Ressourcen-Benutzerzugriffsadministratoren Zuweisungen für andere Administratoren in Azure AD PIM verwalten.  Administratoren für privilegierte Rollen, Sicherheitsadministratoren oder Benutzer mit Leseberechtigung für Sicherheitsfunktionen haben nicht standardmäßig Zugriff auf Zuweisungen zu Azure RBAC-Rollen in Azure AD PIM.

## <a name="privileged-identity-management-overview-entry-point"></a>Übersicht über Privileged Identity Management (Einstiegspunkt)

Azure AD Privileged Identity Management unterstützt die Verwaltung von Azure AD-Verzeichnisrollen sowie von Rollen für Azure-Ressourcen (Vorschauversion). Die Funktion von Rollen für Azure-Ressourcen unterscheidet sich von Administratorrollen in Azure AD. Azure-Ressourcenrollen bieten präzise Berechtigungen für die Ressource, für die sie zugewiesen sind, sowie für alle untergeordneten Ressourcen in der Ressourcenhierarchie (Vererbung). Weitere Informationen zu RBAC, Ressourcenhierarchie und Vererbung finden Sie [hier](role-based-access-control-configure.md). PIM für Azure AD-Verzeichnisrollen und Azure-Ressourcen (Vorschauversion) kann über den entsprechenden Link im Abschnitt „Verwalten“ verwaltet werden. Zu diesem Abschnitt gelangen Sie über den Einstiegspunkt der PIM-Übersicht im linken Navigationsmenü.

Mit PIM können Sie über den Aufgabenabschnitt des linken Navigationsmenüs komfortabel Rollen aktivieren sowie ausstehende Aktivierungen/Anforderungen, ausstehende Genehmigungen (für Azure AD-Verzeichnisrollen) und Überprüfungen mit ausstehender Antwort anzeigen.

Wenn Sie von der Übersicht aus auf eines der Elemente im Aufgabenmenü zugreifen, enthält die daraufhin angezeigte Ansicht sowohl Ergebnisse für Azure AD-Verzeichnisrollen als auch Ergebnisse für Azure-Ressourcenrollen (Vorschauversion).

![Schnellstart](./media/active-directory-privileged-identity-management-configure/quick-start.png)

Unter „My roles“ (Meine Rollen) befindet sich eine Liste mit aktiven und berechtigten Rollenzuweisungen für Azure AD-Verzeichnisrollen und Azure-Ressourcenrollen (Vorschauversion). Weitere Informationen zum Aktivieren berechtigter Rollenzuweisungen finden Sie [hier](active-directory-privileged-identity-management-how-to-activate-role.md).

Zur Aktivierung von Rollen für Azure-Ressourcen (Vorschauversion) wird eine neue Umgebung eingeführt, in der berechtigte Mitglieder einer Rolle die Aktivierung für einen späteren Zeitpunkt planen und eine bestimmte Aktivierungsdauer auswählen können, die innerhalb des von Administratoren festgelegten Bereichs liegt.

![](./media/active-directory-privileged-identity-management-configure/activations.png)

Falls eine geplante Aktivierung nicht mehr benötigt wird, können Benutzer die ausstehende Anforderung abbrechen, indem sie über das linke Navigationsmenü zu den ausstehenden Anforderungen navigieren und für die entsprechende Anforderung auf die Schaltfläche „Abbrechen“ klicken.

![Ausstehende Anforderungen](./media/active-directory-privileged-identity-management-configure/pending-requests.png)

## <a name="privileged-identity-management-admin-dashboard"></a>Privileged Identity Management-Administator-Dashboard

Azure AD Privileged Identity Manager bietet ein Administratoren-Dashboard, das wichtige Informationen enthält, wie etwa:

* Warnungen, die auf Möglichkeiten zur Erhöhung der Sicherheit hinweisen
* Anzahl der Benutzer, die jeder privilegierten Rolle zugewiesen sind  
* Anzahl berechtigter und permanenter Administratoren
* Ein Diagramm der Aktivierungen privilegierter Rollen in Ihrem Verzeichnis
*   Die Anzahl von Just-In-Time-Zuweisungen, zeitgebundenen Zuweisungen und permanenten Zuweisungen für Azure-Ressourcenrollen (Vorschauversion)
*   Benutzer und Gruppen mit neuen Rollenzuweisungen in den letzten 30 Tagen (Azure-Ressourcenrollen)


![PIM-Dashboard – Screenshot][2]

## <a name="privileged-role-management"></a>Verwaltung privilegierter Rollen

Mit Azure AD Privileged Identity Management können Sie die Administratoren verwalten, indem Sie den einzelnen Rollen permanente oder berechtigte Administratoren für Azure AD-Verzeichnisrollen hinzufügen oder selbige entfernen. Mit PIM für Azure-Ressourcen (Vorschauversionen) können Besitzer, Benutzerzugriffsadministratoren und globale Administratoren, die die Verwaltung von Abonnements in ihrem Mandanten ermöglichen, Azure-Ressourcenrollen Benutzer oder Gruppen als berechtigt (Just-In-Time-Zugriff) oder zeitgebunden (keine Aktivierung erforderlich) zuweisen – entweder mit einem Start- oder Endzeitpunkt (Datum und Uhrzeit) oder dauerhaft (sofern in den Rolleneinstellungen aktiviert).

![Hinzufügen/Entfernen von PIM-Administratoren – Screenshot][3]

## <a name="configure-the-role-activation-settings"></a>Konfigurieren der Rollenaktivierungseinstellungen

Mithilfe der [Rolleneinstellungen](active-directory-privileged-identity-management-how-to-change-default-settings.md) können Sie die Eigenschaften der Aktivierung der berechtigten Rolle für Azure AD-Verzeichnisrollen konfigurieren:

* Dauer des Rollenaktivierungszeitraums
* Benachrichtigung zur Rollenaktivierung
* Informationen, die ein Benutzers während des Rollenaktivierungsprozesses bereitstellen muss
* Dienstticket oder Vorfallnummer
* [Anforderungen an den Genehmigungsworkflow – Vorschauversion](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![PIM-Einstellungen – Administratoraktivierung – Screenshot][4]

Beachten Sie, dass in der Abbildung die Schaltflächen für **Multi-Factor Authentication** deaktiviert sind. Für bestimmte, sehr privilegierte Rollen ist MFA für erhöhten Schutz erforderlich.

Mit Rolleneinstellungen für Azure-Ressourcenrollen (Vorschauversion) können Administratoren Einstellungen für Just-In-Time- und Direktzuweisungen konfigurieren:

- Die Möglichkeit, Benutzer oder Gruppen Rollen ohne Enddatum/-uhrzeit zuzuweisen (permanente Zuweisung)
- Die Standarddauer einer Zuweisung (sofern nicht permanent)
- Die maximale Aktivierungsdauer (Aktivierung eines berechtigten Rollenmitglieds)
- Die Informationen, die ein Benutzer im Rahmen der Rollenaktivierung (Just-In-Time-Zuweisungen) oder im Rahmen des Zuweisungsprozesses (Direktzuweisungen) angeben muss

![](./media/active-directory-privileged-identity-management-configure/role-settings-details.png)

## <a name="role-activation"></a>Rollenaktivierung

Um eine [Rolle zu aktivieren](active-directory-privileged-identity-management-how-to-activate-role.md), fordert ein berechtigter Administrator eine zeitgebundene „Aktivierung“ für die Rolle an. Die Aktivierung kann über die Option **Meine Rolle aktivieren** in Azure AD Privileged Identity Management angefordert werden.

Ein Administrator, der eine Rolle aktivieren möchte, muss Azure AD Privileged Identity Management im Azure-Portal initialisieren.

Die Rollenaktivierung ist anpassbar. In den PIM-Einstellungen können Sie die Dauer der Aktivierung sowie die Informationen festlegen, die der Administrator angeben muss, um die Rolle zu aktivieren.

![Anforderung der Rollenaktivierung durch PIM-Administrator – Screenshot][5]

## <a name="review-role-activity"></a>Überprüfen der Rollenaktivität

Ihnen stehen zwei Möglichkeiten zur Verfügung, um nachzuverfolgen, wie Ihre Mitarbeiter und Administratoren privilegierte Rollen nutzen. Die erste Option verwendet den [Verlauf der Verzeichnisrollenüberwachung](active-directory-privileged-identity-management-how-to-use-audit-log.md). Im Überwachungsverlauf werden Änderungen an privilegierten Rollenzuweisungen, der Rollenaktivierungsverlauf sowie Änderungen an Einstellungen für Azure-Ressourcenrollen (Vorschauversion) nachverfolgt. 

![PIM-Aktivierungsverlauf – Screenshot][6]

Die zweite Option besteht darin, regelmäßige [Zugriffsüberprüfungen](active-directory-privileged-identity-management-how-to-start-security-review.md)einzurichten. Diese Zugriffsüberprüfungen können von einem zugewiesenen Prüfer (wie etwa einem Teamleiter) durchgeführt werden, oder die Mitarbeiter führen die Überprüfung selbst durch. Dies ist die beste Möglichkeit, um zu prüfen, wer noch Zugriff benötigt und wer nicht mehr.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Azure AD PIM bei Abonnementablauf

Zur Verwendung von Azure AD PIM muss ein Mandant in seinem Mandanten über ein Testabonnement oder kostenpflichtiges Abonnement vom Typ „Azure AD Premium P2“ (oder EMS E5) verfügen.  Darüber hinaus müssen den Administratoren des Mandanten Lizenzen zugewiesen werden.  Das gilt insbesondere für Administratoren in Azure AD-Rollen, die über Azure AD PIM verwaltet werden, für Administratoren in Azure RBAC-Rollen, die über Azure AD PIM verwaltet werden, und für Benutzer ohne Administratorrechte, die Zugriffsüberprüfungen durchführen.
Wenn Ihre Organisation Azure AD Premium P2 nicht verlängert oder der Testzeitraum abläuft, stehen die Azure AD PIM-Features in Ihrem Mandanten nicht mehr zur Verfügung, berechtigte Rollenzuweisungen werden entfernt, und Benutzer können keine Rollen mehr aktivieren. Weitere Informationen finden Sie in den [Abonnementanforderungen von Azure AD PIM](./privileged-identity-management/subscription-requirements.md).

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png

