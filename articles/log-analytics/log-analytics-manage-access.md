---
title: Verwalten von Arbeitsbereichen in Azure Log Analytics | Microsoft-Dokumentation
description: "Sie können Arbeitsbereiche in Azure Log Analytics verwalten, indem Sie verschiedene Verwaltungsaufgaben für Benutzer, Konten, Arbeitsbereiche und Azure-Konten verwenden."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2017
ms.author: magoedte
ms.openlocfilehash: d9f86ac19044fd13e77d35d6c3dd9964c3852001
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-workspaces"></a>Verwalten von Arbeitsbereichen

Zum Verwalten des Zugriffs auf Log Analytics führen Sie verschiedene Verwaltungsaufgaben für Arbeitsbereiche durch. Dieser Artikel enthält Informationen zu den bewährten Methoden und Verfahren für die Arbeitsbereichsverwaltung. Ein Arbeitsbereich ist im Wesentlichen ein Container, der Kontoinformationen und einfache Konfigurationsinformationen für das Konto enthält. Sie oder andere Mitglieder Ihrer Organisation können mehrere Arbeitsbereiche nutzen, um unterschiedliche Mengen von Daten zu verwalten, die in Ihrer gesamten IT-Infrastruktur oder Teilen davon erfasst werden.

Sie benötigen Folgendes, um einen Arbeitsbereich zu erstellen:

1. Ein Azure-Abonnement
2. Einen Namen für den Arbeitsbereich
3. Zuordnung des Arbeitsbereichs zum Abonnement
4. Ausgewählten geografischen Standort

## <a name="determine-the-number-of-workspaces-you-need"></a>Bestimmen der benötigten Anzahl von Arbeitsbereichen
Ein Arbeitsbereich ist eine Azure-Ressource. Es handelt sich hierbei um einen Container, in dem Daten gesammelt, aggregiert, analysiert und im Azure-Portal angezeigt werden.

Sie können mehrere Arbeitsbereiche pro Azure-Abonnement verwenden und über den Zugriff auf mehr als einen Arbeitsbereich verfügen. Eine Verringerung der Anzahl von Arbeitsbereichen ermöglicht Ihnen das Abfragen und Korrelieren der größten Datenmenge, da es nicht möglich ist, mehrere Arbeitsbereiche auf einmal abzufragen. In diesem Abschnitt wird beschrieben, wann es hilfreich sein kann, mehr als einen Arbeitsbereich zu erstellen.

Ein Arbeitsbereich bietet jetzt Folgendes:

* Einen geografischen Standort für die Speicherung von Daten
* Granularität für die Abrechnung
* Datenisolation
* Bereich für die Konfiguration

Auf der Grundlage der obigen Merkmale können Sie in folgenden Szenarien mehrere Arbeitsbereiche erstellen:

* Sie sind ein globales Unternehmen und müssen Daten aus Gründen der Datensouveränität bzw. aus Compliancegründen in bestimmten Regionen speichern.
* Sie nutzen Azure und möchten Gebühren für ausgehende Datenübertragungen vermeiden, indem Sie einen Arbeitsbereich in derselben Region wie die verwalteten Azure-Ressourcen nutzen.
* Sie möchten Gebühren basierend auf der Nutzung unterschiedlichen Abteilungen bzw. Geschäftseinheiten zuordnen. Wenn Sie einen Arbeitsbereich für jede Abteilung bzw. Geschäftseinheit erstellen, werden die Gebühren in Ihrer Azure-Rechnung und Nutzungsaufstellung für jeden Arbeitsbereich separat aufgeführt.
* Sie sind ein Anbieter von verwalteten Diensten und müssen Log Analytics-Daten für jeden verwalteten Kunden von den Daten der anderen Kunden isolieren.
* Sie verwalten mehrere Kunden und möchten, dass den einzelnen Kunden/Abteilungen/Geschäftseinheiten jeweils nur die eigenen Daten angezeigt werden.

Wenn Sie Agents zum Sammeln von Daten verwenden, können Sie [jeden Agent so konfigurieren, dass er Informationen zu mindestens einem Arbeitsbereich meldet](log-analytics-windows-agents.md).

Bei Verwendung von System Center Operations Manager kann jede Operations Manager-Verwaltungsgruppe mit nur einem Arbeitsbereich verbunden werden. Der Microsoft Monitoring Agent auf dem Computer kann aber so konfiguriert werden, dass Berichte sowohl an Operations Manager als auch an einen anderen Log Analytics-Arbeitsbereich gesendet werden.  

### <a name="workspace-information"></a>Informationen zum Arbeitsbereich

Sie können im Azure-Portal die Details zu Ihrem Arbeitsbereich anzeigen. 

#### <a name="view-workspace-information-in-the-azure-portal"></a>Anzeigen von Arbeitsbereichsinformationen im Azure-Portal

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im Azure-Portal unten links auf **Weitere Dienste**.  Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Klicken Sie auf **Log Analytics**.  
    ![Azure-Hub](./media/log-analytics-manage-access/hub.png)  
3. Wählen Sie auf dem Blatt mit den Log Analytics-Abonnements einen Arbeitsbereich aus.
4. Auf dem Blatt zum Arbeitsbereich werden Details zum Arbeitsbereich und Links zu weiteren Informationen angezeigt.  
    ![Informationen zum Arbeitsbereich](./media/log-analytics-manage-access/workspace-details.png)  


## <a name="manage-accounts-and-users"></a>Verwalten von Konten und Benutzern
Jedem Arbeitsbereich können mehrere Konten zugeordnet sein, wobei jedes Konto (Microsoft- oder Organisationskonto) Zugriff auf mehrere Arbeitsbereiche hat.

Standardmäßig wird der Besitzer des Microsoft- oder Organisationskontos, mit dem der Arbeitsbereich erstellt wird, zum Administrator des Arbeitsbereichs.

Es gibt zwei Berechtigungsmodelle, mit denen der Zugriff auf einen Log Analytics-Arbeitsbereich gesteuert wird:

1. Ältere Log Analytics-Benutzerrollen
2. [Rollenbasierter Zugriff in Azure](../active-directory/role-based-access-control-configure.md)

In der folgenden Tabelle sind die Zugriffsmöglichkeiten aufgeführt, die für die einzelnen Berechtigungsmodelle festgelegt werden können:

|                          | Log Analytics-Portal | Azure-Portal | API (einschließlich PowerShell) |
|--------------------------|----------------------|--------------|----------------------------|
| Log Analytics-Benutzerrollen | Ja                  | Nein           | Nein                         |
| Rollenbasierter Zugriff in Azure  | Ja                  | Ja          | Ja                        |

> [!NOTE]
> Für Log Analytics wird eine Umstellung auf die Verwendung des rollenbasierten Zugriffs in Azure als Berechtigungsmodell durchgeführt, und die Log Analytics-Benutzerrollen werden hierdurch ersetzt.
>
>

Mit den älteren Log Analytics-Benutzerrollen wird nur der Zugriff auf Aktivitäten gesteuert, die im [Log Analytics-Portal](https://mms.microsoft.com) durchgeführt werden.

Für die folgenden Aktivitäten sind ebenfalls Azure-Berechtigungen erforderlich:

| Aktion                                                          | Azure-Berechtigungen erforderlich | Hinweise |
|-----------------------------------------------------------------|--------------------------|-------|
| Hinzufügen und Entfernen von Verwaltungslösungen                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | |
| Ändern des Tarifs                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Anzeigen von Daten auf den Kacheln der *Backup*- und *Site Recovery*-Lösungen | Administrator/Co-Administrator | Zugriff auf Ressourcen, die mit dem klassischen Bereitstellungsmodell bereitgestellt werden |
| Erstellen eines Arbeitsbereichs im Azure-Portal                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Verwalten des Zugriffs auf Log Analytics mit Azure-Berechtigungen
Führen Sie die Schritte unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](../active-directory/role-based-access-control-configure.md) aus, um den Zugriff auf den Log Analytics-Arbeitsbereich mit Azure-Berechtigungen zu gewähren.

Azure verfügt über zwei integrierte Benutzerrollen für Log Analytics:
- Log Analytics-Leser
- Log Analytics-Mitwirkender

Mitglieder der Rolle *Log Analytics-Leser* können folgende Aktionen ausführen:
- Anzeigen und Durchsuchen aller Überwachungsdaten 
- Anzeigen von Überwachungseinstellungen (einschließlich der Konfiguration von Azure-Diagnosen für alle Azure-Ressourcen)

| Typ    | Berechtigung | Beschreibung |
| ------- | ---------- | ----------- |
| Aktion | `*/read`   | Anzeigen aller Ressourcen und der Ressourcenkonfiguration, einschließlich: <br> VM-Erweiterungsstatus <br> Konfiguration von Azure-Diagnosen für Ressourcen <br> Sämtliche Eigenschaften und Einstellungen aller Ressourcen |
| Aktion | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Ausführen von Protokollsuchabfragen (v2) |
| Aktion | `Microsoft.OperationalInsights/workspaces/search/action` | Ausführen von Protokollsuchabfragen (v1) |
| Aktion | `Microsoft.Support/*` | Öffnen von Supportfällen |
|Keine Aktion | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Verhindert das Lesen des Arbeitsbereichsschlüssels, der zum Verwenden der Datensammlungs-API und zum Installieren von Agents erforderlich ist |


Mitglieder der Rolle *Log Analytics-Mitwirkender* können folgende Aktionen ausführen:
- Lesen sämtlicher Überwachungsdaten 
- Erstellen und Konfigurieren von Automation-Konten
- Hinzufügen und Entfernen von Verwaltungslösungen
- Lesen von Speicherkontoschlüsseln 
- Konfigurieren der Sammlung von Protokollen aus Azure Storage
- Bearbeiten von Überwachungseinstellungen für Azure-Ressourcen, einschließlich:
  - Hinzufügen der VM-Erweiterung zu virtuellen Computern
  - Konfigurieren von Azure-Diagnosen für alle Azure-Ressourcen

> [!NOTE] 
> Durch Hinzufügen einer VM-Erweiterung zu einem virtuellen Computer können Sie die vollständige Kontrolle über einen virtuellen Computer erlangen.

| Berechtigung | Beschreibung |
| ---------- | ----------- |
| `*/read`     | Anzeigen aller Ressourcen und der Ressourcenkonfiguration, einschließlich: <br> VM-Erweiterungsstatus <br> Konfiguration von Azure-Diagnosen für Ressourcen <br> Sämtliche Eigenschaften und Einstellungen aller Ressourcen |
| `Microsoft.Automation/automationAccounts/*` | Erstellen und Konfigurieren von Azure Automation-Konten (einschließlich Hinzufügen und Bearbeiten von Runbooks) |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Hinzufügen, Aktualisieren und Entfernen von VM-Erweiterungen (einschließlich Microsoft Monitoring Agent und des OMS-Agents für Linux) |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Anzeigen des Speicherkontoschlüssels. Erforderlich, um Log Analytics für das Lesen von Protokollen aus Azure-Speicherkonten zu konfigurieren |
| `Microsoft.Insights/alertRules/*` | Hinzufügen, Aktualisieren und Entfernen von Warnungsregeln |
| `Microsoft.Insights/diagnosticSettings/*` | Hinzufügen, Aktualisieren und Entfernen von Diagnoseeinstellungen für Azure-Ressourcen |
| `Microsoft.OperationalInsights/*` | Hinzufügen, Aktualisieren und Entfernen der Konfiguration für Log Analytics-Arbeitsbereiche |
| `Microsoft.OperationsManagement/*` | Hinzufügen und Entfernen von Verwaltungslösungen |
| `Microsoft.Resources/deployments/*` | Erstellen und Löschen von Bereitstellungen. Erforderlich für das Hinzufügen und Entfernen von Lösungen, Arbeitsbereichen und Automation-Konten |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Erstellen und Löschen von Bereitstellungen. Erforderlich für das Hinzufügen und Entfernen von Lösungen, Arbeitsbereichen und Automation-Konten |

Wenn Sie Benutzer einer Benutzerrolle hinzufügen oder daraus entfernen möchten, müssen Sie über die Berechtigungen `Microsoft.Authorization/*/Delete` und `Microsoft.Authorization/*/Write` verfügen.

Mit diesen Rollen können Sie Benutzern Zugriff auf verschiedenen Ebenen gewähren:
- Abonnement: Zugriff auf alle Arbeitsbereiche im Abonnement
- Ressourcengruppe: Zugriff auf alle Arbeitsbereiche in der Ressourcengruppe
- Ressource: Nur Zugriff auf den angegebenen Arbeitsbereich

Verwenden Sie [benutzerdefinierte Rollen](../active-directory/role-based-access-control-custom-roles.md), um Rollen mit spezifischen Berechtigungen zu erstellen.

### <a name="azure-user-roles-and-log-analytics-portal-user-roles"></a>Azure-Benutzerrollen und Benutzerrollen des Log Analytics-Portals
Wenn Sie mindestens über die Azure-Leseberechtigung für den Log Analytics-Arbeitsbereich verfügen, können Sie das OMS-Portal öffnen, indem Sie beim Anzeigen des Log Analytics-Arbeitsbereichs auf die Aufgabe **OMS-Portal** klicken.

Beim Öffnen des OMS-Portals wechseln Sie zu den bisher verwendeten Log Analytics-Benutzerrollen. Falls Sie im Log Analytics-Portal nicht über eine Rollenzuweisung verfügen, [überprüft der Dienst Ihre Azure-Berechtigungen für den Arbeitsbereich](https://docs.microsoft.com/rest/api/authorization/permissions#Permissions_ListForResource).
Ihre Rollenzuweisung im OMS-Portal wird wie folgt ermittelt:

| Bedingungen                                                   | Zugewiesene Log Analytics-Benutzerrolle | Hinweise |
|--------------------------------------------------------------|----------------------------------|-------|
| Ihr Konto gehört zu einer älteren Log Analytics-Benutzerrolle.     | Angegebene Log Analytics-Benutzerrolle | |
| Ihr Konto gehört nicht zu einer älteren Log Analytics-Benutzerrolle. <br> Vollständige Azure-Berechtigungen für den Arbeitsbereich (Berechtigung `*` <sup>1</sup>) | Administrator ||
| Ihr Konto gehört nicht zu einer älteren Log Analytics-Benutzerrolle. <br> Vollständige Azure-Berechtigungen für den Arbeitsbereich (Berechtigung `*` <sup>1</sup>). <br> *Keine Aktionen* von `Microsoft.Authorization/*/Delete` und `Microsoft.Authorization/*/Write` | Mitwirkender ||
| Ihr Konto gehört nicht zu einer älteren Log Analytics-Benutzerrolle. <br> Azure-Leseberechtigung | Nur Leseberechtigung ||
| Ihr Konto gehört nicht zu einer älteren Log Analytics-Benutzerrolle. <br> Azure-Berechtigungen werden nicht verstanden. | Nur Leseberechtigung ||
| Für per Cloudlösungsanbieter (CSP) verwaltete Abonnements. <br> Das Konto, mit dem Sie angemeldet sind, befindet sich unter der Azure Active Directory-Instanz, die mit dem Arbeitsbereich verknüpft ist. | Administrator | Normalerweise der Kunde eines CSP |
| Für per Cloudlösungsanbieter (CSP) verwaltete Abonnements. <br> Das Konto, mit dem Sie angemeldet sind, befindet sich nicht unter der Azure Active Directory-Instanz, die mit dem Arbeitsbereich verknüpft ist. | Mitwirkender | Normalerweise der CSP |

<sup>1</sup> Weitere Informationen zu Rollendefinition finden Sie unter [Erstellen von benutzerdefinierten Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-control-custom-roles.md). Beim Auswerten von Rollen ist die Aktion `*` nicht äquivalent zu `Microsoft.OperationalInsights/workspaces/*`.

Wichtige Punkte zum Azure-Portal:

* Wenn Sie sich über „http://mms.microsoft.com“ beim OMS-Portal anmelden, wird die Liste **Arbeitsbereich auswählen** angezeigt. Diese Liste enthält nur Arbeitsbereiche, für die Sie über eine Log Analytics-Benutzerrolle verfügen. Zum Anzeigen der Arbeitsbereiche, auf die Sie mit Azure-Abonnements zugreifen können, müssen Sie als Teil der URL einen Mandanten angeben. Beispiel: `mms.microsoft.com/?tenant=contoso.com`. Die Mandanten-ID ist häufig dieser letzte Teil der E-Mail-Adresse, die Sie bei der Anmeldung verwenden.
* Wenn Sie direkt zu einem Portal navigieren möchten, auf das Sie mit Azure-Berechtigungen Zugriff haben, müssen Sie die Ressource als Teil der URL angeben. Es ist möglich, diese URL mit PowerShell abzurufen.

  Beispiel: `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  Die URL sieht wie folgt aus: `https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

### <a name="managing-users-in-the-oms-portal"></a>Verwalten von Benutzern im OMS-Portal
Sie verwalten Benutzer und Gruppen auf der Registerkarte **Benutzer verwalten** unter der Registerkarte **Konten** auf der Seite „Einstellungen“.   

![Verwalten von Benutzern](./media/log-analytics-manage-access/setup-workspace-manage-users.png)


#### <a name="add-a-user-to-an-existing-workspace"></a>Hinzufügen eines Benutzers zu einem vorhandenen Arbeitsbereich
Gehen Sie wie folgt vor, um einen Benutzer oder eine Gruppe einem Arbeitsbereich hinzuzufügen.

1. Klicken Sie im OMS-Portal auf die Kachel **Einstellungen**.
2. Klicken Sie auf die Registerkarte **Konten** und dann auf die Registerkarte **Benutzer verwalten**.
3. Wählen Sie im Abschnitt **Benutzer verwalten** den hinzuzufügenden Kontotyp aus: **Unternehmenskonto**, **Microsoft-Konto** oder **Microsoft-Support**.

   * Wenn Sie „Microsoft-Konto“ auswählen, geben Sie die E-Mail-Adresse des Benutzers ein, der dem Microsoft-Konto zugeordnet ist.
   * Geben Sie einen Teil des Benutzer- oder Gruppennamens bzw. des E-Mail-Alias ein, wenn Sie „Unternehmenskonto“ auswählen. Anschließend wird in einem Dropdownfeld eine Liste mit passenden Benutzern und Gruppen angezeigt. Wählen Sie einen Benutzer oder eine Gruppe aus.
   * Verwenden Sie „Microsoft-Support“, um einem Microsoft-Supporttechniker oder einem anderen Microsoft-Mitarbeiter vorübergehenden Zugriff auf Ihren Arbeitsbereich zu gewähren, damit er bei der Problembehandlung behilflich sein kann.

     > [!NOTE]
     > Damit Sie eine optimale Leistung erzielen, sollten Sie die Anzahl von Active Directory-Gruppen, die einem einzelnen OMS-Konto zugeordnet sind, auf drei begrenzen – eines für Administratoren, eines für Mitwirkende und eines für ReadOnly-Benutzer. Mehr Gruppen können sich auf die Leistung von Log Analytics auswirken.
     >
     >
4. Wählen Sie den hinzuzufügenden Benutzer- oder Gruppentyp aus: **Administrator**, **Mitwirkender** oder **ReadOnly-Benutzer**.  
5. Klicken Sie auf **Hinzufügen**.

   Wenn Sie ein Microsoft-Konto hinzufügen, wird eine Einladung zur Teilnahme am Arbeitsbereich an die angegebene E-Mail-Adresse gesendet. Nachdem der Benutzer die Anweisungen in der Einladung zur Teilnahme an OMS befolgt hat, kann er auf den Arbeitsbereich zugreifen.
   Wenn Sie ein Organisationskonto hinzufügen, kann der Benutzer sofort auf Log Analytics zugreifen.  

#### <a name="edit-an-existing-user-type"></a>Bearbeiten eines vorhandenen Benutzertyps
Sie können die Kontorolle eines Benutzers ändern, der Ihrem OMS-Konto zugeordnet ist. Sie haben die folgenden Rollenoptionen:

* *Administrator*: Kann Benutzer verwalten, alle Warnungen anzeigen und darauf reagieren sowie Server hinzufügen und entfernen
* *Mitwirkender*: Kann alle Warnungen anzeigen und darauf reagieren sowie Server hinzufügen und entfernen
* *ReadOnly-Benutzer*: Als schreibgeschützt gekennzeichnete Benutzer können folgende Aktionen nicht ausführen:

  1. Lösungen hinzufügen/entfernen Der Lösungskatalog ist ausgeblendet
  2. Kacheln unter **Mein Dashboard** hinzufügen/ändern/entfernen
  3. Seiten unter **Einstellungen** anzeigen Die Seiten sind ausgeblendet.
  4. In der Suchansicht: PowerBI-Konfiguration, gespeicherte Suchen und Warnungsaufgaben sind ausgeblendet

#### <a name="to-edit-an-account"></a>So bearbeiten Sie ein Konto
1. Klicken Sie im OMS-Portal auf die Kachel **Einstellungen**.
2. Klicken Sie auf die Registerkarte **Konten** und dann auf die Registerkarte **Benutzer verwalten**.
3. Wählen Sie die Rolle für den Benutzer, den Sie ändern möchten.
4. Klicken Sie im Bestätigungsdialogfeld auf **Ja**.

### <a name="remove-a-user-from-a-workspace"></a>Entfernen eines Benutzers aus einem Arbeitsbereich
Führen Sie die unten angegebenen Schritte aus, um einen Benutzer aus einem Arbeitsbereich zu entfernen. Durch Entfernen des Benutzers wird der Arbeitsbereich nicht geschlossen. Stattdessen wird die Zuordnung zwischen diesem Benutzer und dem Arbeitsbereich aufgehoben. Wenn ein Benutzer mehreren Arbeitsbereichen zugeordnet ist, kann er sich weiterhin bei OMS anmelden und seine anderen Arbeitsbereiche anzeigen.

1. Klicken Sie im OMS-Portal auf die Kachel **Einstellungen**.
2. Klicken Sie auf die Registerkarte **Konten** und dann auf die Registerkarte **Benutzer verwalten**.
3. Klicken Sie neben dem Namen des Benutzers, den Sie entfernen möchten, auf **Entfernen**.
4. Klicken Sie im Bestätigungsdialogfeld auf **Ja**.

### <a name="add-a-group-to-an-existing-workspace"></a>Hinzufügen einer Gruppe zu einem vorhandenen Arbeitsbereich
1. Führen Sie die Schritte 1 bis 4 des obigen Abschnitts „So fügen Sie einen Benutzer einem vorhandenen Arbeitsbereich hinzu“ aus.
2. Wählen Sie unter **Benutzer/Gruppe auswählen** die Option **Gruppe**.  
   ![add a group to an existing workspace](./media/log-analytics-manage-access/add-group.png)
3. Geben Sie den Anzeigenamen oder die E-Mail-Adresse für die Gruppe ein, die Sie hinzufügen möchten.
4. Wählen Sie in der Liste die gewünschte Gruppe aus, und klicken Sie auf **Hinzufügen**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Verknüpfen eines vorhandenen Arbeitsbereichs mit einem Azure-Abonnement
Ab dem 26. September 2016 müssen alle Arbeitsbereiche bei der Erstellung mit einem Azure-Abonnement verknüpft werden. Vor diesem Datum erstellte Arbeitsbereiche müssen bei der Anmeldung mit einem Abonnement verknüpft werden. Wenn Sie den Arbeitsbereich über das Azure-Portal erstellen oder den Arbeitsbereich mit einem Azure-Abonnement verknüpfen, wird Azure Active Directory als Organisationskonto verknüpft.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>So verknüpfen Sie einen Arbeitsbereich mit einem Azure-Abonnement im OMS-Portal

- Wenn Sie sich beim OMS-Portal anmelden, werden Sie aufgefordert, ein Azure-Abonnement auszuwählen. Wählen Sie das Abonnement aus, das Sie mit Ihrem Arbeitsbereich verknüpfen möchten, und klicken Sie anschließend auf **Verknüpfen**.  
    ![Verknüpfen des Azure-Abonnements](./media/log-analytics-manage-access/required-link.png)

    > [!IMPORTANT]
    > Damit Sie einen Arbeitsbereich verknüpfen können, muss Ihr Azure-Konto bereits Zugriff auf den zu verknüpfenden Arbeitsbereich haben.  Anders ausgedrückt: Das Konto, das Sie für den Zugriff auf das Azure-Portal verwenden, muss **identisch** mit dem Konto sein, mit dem Sie auf den Arbeitsbereich zugreifen. Ist dies nicht der Fall, lesen Sie unter [Hinzufügen eines Benutzers zu einem vorhandenen Arbeitsbereich](#add-a-user-to-an-existing-workspace) weiter.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>So verknüpfen Sie einen Arbeitsbereich mit einem Azure-Abonnement im Azure-Portal
1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.
2. Suchen Sie nach **Log Analytics**, und wählen Sie diese Option aus.
3. Ihre Liste mit den vorhandenen Arbeitsbereichen wird angezeigt. Klicken Sie auf **Hinzufügen**.  
   ![Liste der Arbeitsbereiche](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4. Klicken Sie unter **OMS Workspace** (OMS-Arbeitsbereich) auf **Or link existing** (Oder vorhandenen einfügen).  
   ![Vorhandene verknüpfen](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5. Klicken Sie auf **Erforderliche Einstellungen konfigurieren**.  
   ![configure required settings](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6. Daraufhin wird eine Liste mit Arbeitsbereichen angezeigt, die noch nicht mit Ihrem Azure-Konto verknüpft sind. Wählen Sie einen Arbeitsbereich aus.  
   ![Auswählen von Arbeitsbereichen](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7. Bei Bedarf können Sie die Werte für die folgenden Elemente ändern:
   * Abonnement
   * Ressourcengruppe
   * Ort
   * Tarif   
     ![Ändern von Werten](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8. Klicken Sie auf **OK**. Der Arbeitsbereich ist jetzt mit Ihrem Azure-Konto verknüpft.

> [!NOTE]
> Wenn der Arbeitsbereich, den Sie verknüpfen möchten, hier nicht angezeigt wird, hat Ihr Azure-Abonnement keinen Zugriff auf den Arbeitsbereich, den Sie mit dem OMS-Portal erstellt haben.  Unter [Hinzufügen eines Benutzers zu einem vorhandenen Arbeitsbereich](#add-a-user-to-an-existing-workspace) erfahren Sie, wie Sie über das OMS-Portal Zugriff auf dieses Konto gewähren.
>
>

## <a name="change-an-azure-active-directory-organization-for-a-workspace"></a>Ändern einer Azure Active Directory-Organisation für einen Arbeitsbereich

Sie können die Azure Active Directory-Organisation eines Arbeitsbereichs ändern. Wenn Sie die Azure Active Directory-Organisation ändern, können Sie dem Arbeitsbereich Benutzer und Gruppen aus diesem Verzeichnis hinzufügen.

### <a name="to-change-the-azure-active-directory-organization-for-a-workspace"></a>So ändern Sie eine Azure Active Directory-Organisation für einen Arbeitsbereich

1. Klicken Sie im OMS-Portal auf der Seite mit den Einstellungen auf **Konten** und anschließend auf die Registerkarte **Benutzer verwalten**.  
2. Überprüfen Sie die Informationen zu Organisationskonten, und klicken Sie dann auf **Organisation ändern**.  
    ![Organisation ändern](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Geben Sie die Identitätsinformationen des Administrators Ihrer Azure Active Directory-Domäne ein. Anschließend wird bestätigt, dass Ihr Arbeitsbereich mit Ihrer Azure Active Directory-Domäne verknüpft ist.  
    ![Verknüpfte Arbeitsbereichsbestätigung](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Analysieren des Volumes mit den Daten, die von Lösungen gesammelt und von Computern gesendet werden, finden Sie unter [Analysieren der Datennutzung in Log Analytics](log-analytics-usage.md).
* Führen Sie die Schritte unter [Hinzufügen von Azure Log Analytics-Verwaltungslösungen zu Ihrem Arbeitsbereich](log-analytics-add-solutions.md) aus, um Funktionalität hinzuzufügen und Daten zu sammeln.