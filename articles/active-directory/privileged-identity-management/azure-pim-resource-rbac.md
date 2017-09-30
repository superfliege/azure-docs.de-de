---
title: "Übersicht über RBAC in Azure PIM | Microsoft-Dokumentation"
description: "Verschaffen Sie sich einen Überblick über das RBAC-Feature in PIM (einschließlich Terminologie und Benachrichtigungen)."
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: barclayn
ms.translationtype: HT
ms.sourcegitcommit: a29f1e7b39b7f35073aa5aa6c6bd964ffaa6ffd0
ms.openlocfilehash: e80fe891be661acef5bb30dbf9af023a79ace191
ms.contentlocale: de-de
ms.lasthandoff: 09/21/2017

---
# <a name="pim-for-azure-resources-preview"></a>PIM für Azure-Ressourcen (Vorschauversion)

Mit Azure Active Directory Privileged Identity Management (PIM) können Sie nun den Zugriff auf Azure-Ressourcen (Vorschauversion) innerhalb Ihrer Organisation verwalten, steuern und überwachen. Dies schließt Abonnements, Ressourcengruppen und sogar virtuelle Computer ein. Alle Ressourcen im Azure-Portal, die die Azure-Funktion für die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) nutzen, können sowohl von den großartigen Sicherheits- und Lebenszyklusverwaltungsfunktionen profitieren, die Azure AD PIM zu bieten hat, als auch von einigen neuen Features, die in Kürze für Azure AD-Rollen eingeführt werden. 

## <a name="pim-for-azure-resources-preview-helps-resource-administrators"></a>Unterstützung von Ressourcenadministratoren durch PIM für Azure-Ressourcen (Vorschauversion)

- Sehen Sie, welchen Benutzern und Gruppen Rollen für die von Ihnen verwalteten Azure-Ressourcen zugewiesen sind.
- Aktivieren Sie bedarfsgesteuerten Just-In-Time-Zugriff zur Verwaltung von Ressourcen wie Abonnements und Ressourcengruppen
- Lassen Sie mithilfe neuer, zeitgebundener Zuweisungseinstellungen den Ressourcenzugriff zugewiesener Benutzer/Gruppen automatisch ablaufen.
- Weisen Sie temporären Ressourcenzugriff für kurze Aufgaben oder Bereitschaftszeitpläne zu.
- Erzwingen Sie die Verwendung der mehrstufigen Authentifizierung für den Ressourcenzugriff jeder beliebigen integrierten oder benutzerdefinierten Rolle. 
- Rufen Sie Berichte zu ressourcenzugriffsbezogenen Ressourcenaktivitäten während der aktiven Sitzung eines Benutzers ab.
- Erhalten Sie Benachrichtigungen, wenn neuen Benutzern oder Gruppen Ressourcenzugriff zugewiesen wird und sie geeignete Zuweisungen aktivieren.

Azure AD PIM kann sowohl die integrierten Azure-Ressourcenrollen als auch benutzerdefinierte (RBAC-)Rollen verwalten. Beispiele:

- Besitzer
- Benutzerzugriffsadministrator
- Mitwirkender
- Sicherheitsadministrator
- Sicherheits-Manager

>[!NOTE]
Benutzer oder Mitglieder von Gruppen, die der Rolle „Besitzer“ oder „Benutzerzugriffsadministrator“ zugewiesen sind, sowie globale Administratoren, die die Abonnementverwaltung in Azure AD ermöglichen, sind Ressourcenadministratoren. Diese Administratoren können Rollen zuweisen, Rolleneinstellungen konfigurieren und den Zugriff mithilfe von PIM für Azure-Ressourcen prüfen. Eine Liste mit den integrierten Rollen für Azure-Ressourcen finden Sie [hier](../role-based-access-built-in-roles.md).

## <a name="tasks"></a>Aufgaben

Mit PIM können Sie über den Aufgabenabschnitt des linken Navigationsmenüs komfortabel Rollen aktivieren sowie ausstehende Aktivierungen/Anforderungen, ausstehende Genehmigungen (für [Azure AD-Verzeichnisrollen](azure-ad-pim-approval-workflow.md)) und Überprüfungen mit ausstehender Antwort anzeigen.
Wenn Sie von der Übersicht aus auf eines der Elemente im Aufgabenmenü zugreifen, enthält die daraufhin angezeigte Ansicht sowohl Ergebnisse für Azure AD-Verzeichnisrollen als auch Ergebnisse für Azure-Ressourcenrollen (Vorschauversion). 

![](media/azure-pim-resource-rbac/role-settings-details.png)

Unter „My roles“ (Meine Rollen) befindet sich eine Liste mit Ihren aktiven und geeigneten Rollenzuweisungen für Azure AD-Verzeichnisrollen und Azure-Ressourcenrollen (Vorschauversion).

## <a name="activate-roles"></a>Aktivieren von Rollen

Zur Aktivierung von Rollen für Azure-Ressourcen (Vorschauversion) wird eine neue Umgebung eingeführt, in der geeignete Rollenmitglieder die Aktivierung für einen späteren Zeitpunkt planen und eine bestimmte Aktivierungsdauer auswählen können, die innerhalb des (von Administratoren konfigurierten) Bereichs liegt. Informationen zum Aktivieren von Azure AD-Rollen finden Sie [hier](../active-directory-privileged-identity-management-how-to-activate-role.md).

![](media/azure-pim-resource-rbac/contributor.png)

Geben Sie im Aktivierungsmenü den gewünschten Startzeitpunkt (Datum und Uhrzeit) für die Rollenaktivierung ein. Verringern Sie optional die Aktivierungsdauer (also den Zeitraum, für den die Rolle aktiv ist), geben Sie ggf. eine Begründung ein, und klicken Sie anschließend auf „Aktivieren“.

Wenn Startdatum und -uhrzeit nicht geändert werden, wird die Rolle innerhalb der nächsten Sekunden aktiviert. Auf der Seite mit Ihren Rollen wird eine Bannermeldung mit dem Hinweis angezeigt, dass die zu aktivierende Rolle der Warteschlange hinzugefügt wurde. Klicken Sie auf die Aktualisierungsschaltfläche, um diese Meldung zu löschen.

![](media/azure-pim-resource-rbac/my-roles.png)

Wenn die Aktivierung für einen späteren Zeitpunkt geplant wurde, wird die ausstehende Anforderung im linken Navigationsmenü auf der Registerkarte für ausstehende Anforderungen angezeigt. Sollte die Rollenaktivierung nicht mehr benötigt werden, kann der Benutzer die Anforderung rechts auf der Seite durch Klicken auf die Schaltfläche „Abbrechen“ abbrechen.

![](media/azure-pim-resource-rbac/pending-requests.png)

## <a name="discover-and-manage-azure-resources"></a>Ermitteln und Verwalten von Azure-Ressourcen

Wählen Sie auf der Verwaltungsregisterkarte des linken Navigationsmenüs die Option für Azure-Ressourcen (Vorschauversion) aus, um Rollen für eine Azure-Ressource zu suchen und zu verwalten. Suchen Sie mithilfe der Filter oder der Suchleiste am oberen Seitenrand nach einer Ressource.

![](media/azure-pim-resource-rbac/azure-resources.png)

## <a name="resource-dashboards"></a>Ressourcendashboards

Das Dashboard mit der Administratoransicht verfügt über vier Hauptkomponenten. Eine grafische Darstellung der Ressourcenrollenaktivierungen der letzten sieben Tage. Diese Daten beziehen sich auf die ausgewählte Ressource und geben Aufschluss über Aktivierungen für die gängigsten Rollen (Besitzer, Mitwirkender, Benutzerzugriffsadministrator) sowie für alle Rollen zusammen.

Rechts neben dem Aktivierungsdiagramm befinden sich zwei Diagramme mit der Verteilung von Rollenzuweisungen nach Zuweisungstyp (für Benutzer und Gruppen). Wenn Sie auf ein Segment des Diagramms klicken, ändert sich der Wert in einen Prozentsatz (oder umgekehrt).

![](media/azure-pim-resource-rbac/admin-view.png)

Unterhalb der Diagramme werden die Anzahl von Benutzern und Gruppen mit neuen Rollenzuweisungen in den letzten 30 Tagen (links) sowie eine absteigend nach Gesamtzuweisungen sortierte Liste mit Rollen angezeigt.

![](media/azure-pim-resource-rbac/role-settings.png)

## <a name="manage-role-assignments"></a>Verwalten von Rollenzuweisungen

Administratoren können zum Verwalten von Rollenzuweisungen im linken Navigationsbereich entweder auf „Rollen“ oder auf „Mitglieder“ klicken. Unter „Rollen“ können Administratoren Verwaltungsaufgaben für eine bestimmte Rolle durchführen. Unter „Mitglieder“ werden dagegen alle Benutzer- und Gruppenrollenzuweisungen für die Ressource angezeigt.

![](media/azure-pim-resource-rbac/roles.png)

![](media/azure-pim-resource-rbac/members.png)

>[!NOTE]
Ist eine Rolle mit ausstehender Aktivierung vorhanden, wird beim Anzeigen der Mitgliedschaft am oberen Seitenrand ein Benachrichtigungsbanner angezeigt.

## <a name="assign-roles"></a>Zuweisen von Rollen

Wenn Sie einen Benutzer oder eine Gruppe einer Rolle zuweisen möchten, wählen Sie die Rolle aus (Rollenansicht), bzw. klicken Sie auf der Aktionsleiste auf „Hinzufügen“ (Mitgliederansicht).

![](media/azure-pim-resource-rbac/members2.png)

>[!NOTE]
Wenn Sie einen Benutzer oder eine Gruppe über die Registerkarte „Mitglieder“ hinzufügen, müssen Sie im Menü „Hinzufügen“ zuerst eine Rolle auswählen, um einen Benutzer oder eine Gruppe auswählen zu können.

![](media/azure-pim-resource-rbac/select-role.png)

Wählen Sie einen Benutzer oder eine Gruppe aus dem Verzeichnis aus.

![](media/azure-pim-resource-rbac/choose.png)

Wählen Sie im Dropdownmenü den entsprechenden Zuweisungstyp aus. 

**Just In Time Assignment** (Just-In-Time-Zuweisung): Gewährt dem Benutzer oder den Gruppenmitgliedern geeigneten, aber nicht beständigen Zugriff auf die Rolle – entweder für einen bestimmten Zeitraum oder unbegrenzt (sofern in den Rolleneinstellungen konfiguriert). 

**Direct Assignment** (Direkte Zuweisung): Bei dieser Option ist keine Aktivierung der Rollenzuweisung durch den Benutzer oder die Gruppenmitglieder erforderlich. (Wird auch als beständiger Zugriff bezeichnet.) Microsoft empfiehlt die Verwendung der direkten Zuweisung für den kurzfristigen Einsatz (etwa bei Bereitschaftsschichten) oder für zeitkritische Aktivitäten, bei denen der Zugriff nach Abschluss der Aufgabe nicht mehr benötigt wird.

![](media/azure-pim-resource-rbac/membership-settings.png)

Mithilfe des Kontrollkästchens unterhalb der Dropdownliste für den Zuweisungstyp können Sie angeben, ob die Zuweisung dauerhaft sein soll (dauerhaft zur Aktivierung der Just-In-Time Zuweisung berechtigt/dauerhaft aktiv für die direkte Zuweisung). Wenn Sie einen bestimmten Zuweisungszeitraum angeben möchten, deaktivieren Sie dieses Kontrollkästchen, und ändern Sie nach Bedarf den Start- und/oder den Endzeitpunkt (Datum und Uhrzeit).

>[!NOTE]
Das Kontrollkästchen kann unter Umständen nicht bearbeitet werden, wenn ein anderer Administrator in den Rolleneinstellungen die maximale Zuweisungsdauer für die einzelnen Zuweisungstypen angegeben hat.

![](media/azure-pim-resource-rbac/calendar.png)

## <a name="view-activation-and-azure-resource-activity"></a>Anzeigen der Aktivierung und der Azure-Ressourcenaktivität

Falls Sie ermitteln möchten, welche Aktionen ein bestimmter Benutzer für verschiedene Ressourcen ausgeführt hat, können Sie die Azure-Ressourcenaktivität für einen bestimmten Aktivierungszeitraum prüfen (für geeignete Benutzer). Wählen Sie in der Mitgliederansicht zunächst einen Benutzer aus der Liste mit den Mitgliedern in einer bestimmten Rolle aus. Daraufhin erscheint eine grafische Darstellung der Aktionen (nach Datum), die der Benutzer für Azure-Ressourcen ausgeführt hat, und der kürzlich erfolgten Rollenaktivierungen im gleichen Zeitraum.

![](media/azure-pim-resource-rbac/user-details.png)

Wenn Sie auf eine bestimmte Rollenaktivierung klicken, werden Details zu der Rollenaktivierung sowie die entsprechende Azure-Ressourcenaktivität angezeigt, die erfolgt ist, während der Benutzer aktiv war.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="modify-existing-assignments"></a>Ändern vorhandener Zuweisungen

Wenn Sie vorhandene Zuweisungen über die Benutzer-/Gruppendetailansicht ändern möchten, klicken Sie auf der Aktionsleiste am oberen Seitenrand auf „Einstellungen ändern“. Ändern Sie den Zuweisungstyp in „Just In Time Assignment“ (Just-In-Time-Zuweisung) oder „Direct Assignment“ (Direkte Zuweisung).

![](media/azure-pim-resource-rbac/change-settings.png)

## <a name="review-who-has-access-in-a-subscription"></a>Überprüfen, wer in einem Abonnement Zugriff hat

Rufen Sie zum Überprüfen der Rollenzuweisungen in Ihrem Abonnement über die linke Navigationsleiste die Registerkarte „Mitglieder“ auf, oder klicken Sie auf „Rollen“, und wählen Sie eine bestimmte Rolle aus, deren Mitglieder Sie überprüfen möchten. 

Klicken Sie auf der Aktionsleiste auf „Überprüfen“, um vorhandene Zugriffsüberprüfungen anzuzeigen, und klicken Sie anschließend auf „Hinzufügen“, um eine neue Überprüfung hinzuzufügen.

![](media/azure-pim-resource-rbac/owner.png)

Weitere Informationen zu Zugriffsüberprüfungen finden Sie [hier](../active-directory-privileged-identity-management-how-to-perform-security-review.md).

>[!NOTE]
Überprüfungen werden derzeit nur für Abonnementressourcentypen unterstützt.

## <a name="configure-role-settings"></a>Konfigurieren von Rolleneinstellungen

Die Rolleneinstellungen dienen zum Definieren der Standardwerte für Zuweisungen in der PIM-Umgebung. Wenn Sie diese Einstellungen für Ihre Ressource konfigurieren möchten, klicken Sie im linken Navigationsmenü auf die Registerkarte „Rolleneinstellungen“ oder auf der Aktionsleiste einer beliebigen Rolle auf die Schaltfläche „Rolleneinstellungen“, um die aktuellen Optionen anzuzeigen.

Die einzelnen Einstellungen können Sie ändern, indem Sie auf der Aktionsleiste am oberen Seitenrand auf „Bearbeiten“ klicken.

![](media/azure-pim-resource-rbac/owner.png)

![](media/azure-pim-resource-rbac/owner02.png)

Änderungen an den Einstellungen werden auf der Seite mit den Rolleneinstellungen protokolliert – zusammen mit dem Zeitpunkt der letzten Aktualisierung (Datum und Uhrzeit) und dem Administrator, der die Einstellungen geändert hat.

![](media/azure-pim-resource-rbac/role-settings-02.png)

## <a name="resource-audit"></a>Ressourcenüberwachung

Die Ressourcenüberwachung liefert einen Überblick über alle Rollenaktivitäten für die Ressource. Die Informationen können nach einem vordefinierten oder benutzerdefinierten Datumsbereich gefiltert werden.
![](media/azure-pim-resource-rbac/last-day.png) Darüber hinaus können Sie über die Ressourcenüberwachung schnell auf die Aktivitätsdetails eines Benutzers zugreifen. In dieser Ansicht werden alle Rollenaktivierungsaktionen als Links zur Ressourcenaktivität des jeweiligen Anforderers bereitgestellt.
![](media/azure-pim-resource-rbac/resource-audit.png)

## <a name="just-enough-administration"></a>Minimale Administration

PIM für Azure-Ressourcen ermöglicht die problemlose Anwendung bewährter Methoden für minimale Administration (Just Enough Administration, JEA) in Verbindung mit Rollenzuweisungen. Benutzer und Gruppenmitglieder mit Zuweisungen in Azure-Abonnements oder Ressourcengruppen können ihre vorhandene Rollenzuweisung in einem eingeschränkten Bereich aktivieren. 

Suchen Sie über die Suchseite die untergeordnete Ressource, die Sie verwalten möchten.

![](media/azure-pim-resource-rbac/azure-resources-02.png)

Klicken Sie im linken Navigationsmenü auf „My roles“ (Meine Rollen), und wählen Sie die zu aktivierende Rolle aus. Beachten Sie, dass der Zuweisungstyp übernommen wird, da die Rolle nicht auf der Ressourcengruppenebene, sondern auf der Abonnementebene zugewiesen wurde, wie hier zu sehen:

![](media/azure-pim-resource-rbac/my-roles-02.png)

## <a name="next-steps"></a>Nächste Schritte

- [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../role-based-access-built-in-roles.md)
- [Aktivieren oder Deaktivieren von Rollen in Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-how-to-activate-role.md)
- [Genehmigungen (Vorschau)](azure-ad-pim-approval-workflow.md)
