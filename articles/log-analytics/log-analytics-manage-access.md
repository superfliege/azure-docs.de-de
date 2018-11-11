---
title: Verwalten von Arbeitsbereichen in Azure Log Analytics und im OMS-Portal | Microsoft-Dokumentation
description: Arbeitsbereiche können in Azure Log Analytics und im OMS-Portal mithilfe verschiedener Verwaltungsaufgaben für Benutzer, Konten, Arbeitsbereiche und Azure-Konten verwaltet werden.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 796e10053df79f8f7106d98dd9c9be6083d9f719
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50964151"
---
# <a name="manage-workspaces"></a>Verwalten von Arbeitsbereichen

Zum Verwalten des Zugriffs auf Log Analytics führen Sie verschiedene Verwaltungsaufgaben für Arbeitsbereiche durch. Dieser Artikel enthält Tipps und Verfahren für die Arbeitsbereichsverwaltung. Ein Arbeitsbereich ist im Wesentlichen ein Container, der Kontoinformationen und einfache Konfigurationsinformationen für das Konto enthält. Sie oder andere Mitglieder Ihrer Organisation können mehrere Arbeitsbereiche nutzen, um unterschiedliche Mengen von Daten zu verwalten, die in Ihrer gesamten IT-Infrastruktur oder Teilen davon erfasst werden.

Sie benötigen Folgendes, um einen Arbeitsbereich zu erstellen:

1. Ein Azure-Abonnement
2. Einen Namen für den Arbeitsbereich
3. Zuordnung des Arbeitsbereichs zu einem Ihrer Abonnements und einer Ihrer Ressourcengruppen
4. Ausgewählten geografischen Standort

## <a name="determine-the-number-of-workspaces-you-need"></a>Bestimmen der benötigten Anzahl von Arbeitsbereichen
Ein Arbeitsbereich ist eine Azure-Ressource. Es handelt sich hierbei um einen Container, in dem Daten gesammelt, aggregiert, analysiert und im Azure-Portal angezeigt werden.

Sie können mehrere Arbeitsbereiche pro Azure-Abonnement verwenden und über den Zugriff auf mehr als einen Arbeitsbereich mit einfacher Abfragemöglichkeit verfügen. In diesem Abschnitt wird beschrieben, wann es hilfreich sein kann, mehr als einen Arbeitsbereich zu erstellen.

Ein Arbeitsbereich bietet jetzt Folgendes:

* Einen geografischen Standort für die Speicherung von Daten
* Datenisolation zum Definieren unterschiedlicher Benutzerzugriffsrechte
* Bereich für die Konfiguration von Einstellungen wie Aufbewahrung und Datenobergrenzen

Im Hinblick auf die Nutzung wird empfohlen, möglichst wenige Arbeitsbereiche zu erstellen. Dadurch werden Verwaltung und Abfragen einfacher und schneller. Auf der Grundlage der obigen Merkmale können Sie jedoch in folgenden Szenarien mehrere Arbeitsbereiche erstellen:

* Sie sind ein globales Unternehmen und müssen Daten aus Gründen der Datensouveränität bzw. aus Compliancegründen in bestimmten Regionen speichern.
* Sie nutzen Azure und möchten Gebühren für ausgehende Datenübertragungen vermeiden, indem Sie einen Arbeitsbereich in derselben Region wie die verwalteten Azure-Ressourcen nutzen.
* Sie möchten Gebühren basierend auf der Nutzung unterschiedlichen Abteilungen bzw. Geschäftseinheiten zuordnen, indem Sie einen Arbeitsbereich für jede Abteilung oder Geschäftseinheit im eigenen Azure-Abonnement erstellen.
* Sie sind ein Dienstanbieter mit Verwaltung und müssen die Log Analytics-Daten für jeden Kunden, den Sie verwalten, von den Daten der anderen Kunden isolieren.
* Sie verwalten mehrere Kunden und möchten, dass den einzelnen Kunden/Abteilungen/Geschäftseinheiten jeweils nur die eigenen Daten angezeigt werden.

Wenn Sie Windows-Agents zum Sammeln von Daten verwenden, können Sie [jeden Agent so konfigurieren, dass er Informationen zu mindestens einem Arbeitsbereich meldet](log-analytics-agent-windows.md).

Bei Verwendung von System Center Operations Manager kann jede Operations Manager-Verwaltungsgruppe mit nur einem Arbeitsbereich verbunden werden. Sie können den Microsoft Monitoring Agent auf Computern installieren, die mit Operations Manager verwaltet werden, und den Agent so einrichten, dass er sowohl Daten an Operations Manager als auch an einen anderen Log Analytics-Arbeitsbereich liefert.

## <a name="workspace-information"></a>Informationen zum Arbeitsbereich

Sie können im Azure-Portal die Details zu Ihrem Arbeitsbereich anzeigen. 

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com)an, falls Sie dies noch nicht getan haben.

2. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.  

    ![Azure-Portal](media/log-analytics-manage-access/azure-portal-01.png)  

3. Wählen Sie im Bereich mit den Log Analytics-Abonnements einen Arbeitsbereich aus.

4. Auf der Seite für den Arbeitsbereich werden Details zu den ersten Schritten und zur Konfiguration sowie Links zu weiteren Informationen angezeigt.  

    ![Informationen zum Arbeitsbereich](./media/log-analytics-manage-access/workspace-overview-page.png)  

## <a name="manage-accounts-and-users"></a>Verwalten von Konten und Benutzern
Jedem Arbeitsbereich können mehrere Konten zugeordnet werden, und jedes Konto kann Zugriff auf mehrere Arbeitsbereiche haben. Der Zugriff wird über die [rollenbasierte Zugriffssteuerung in Azure](../role-based-access-control/role-assignments-portal.md) verwaltet. Diese Zugriffsrechte gelten für das Azure-Portal sowie für den API-Zugriff.


Für die folgenden Aktivitäten sind ebenfalls Azure-Berechtigungen erforderlich:

| Aktion                                                          | Azure-Berechtigungen erforderlich | Notizen |
|-----------------------------------------------------------------|--------------------------|-------|
| Hinzufügen und Entfernen von Verwaltungslösungen                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Diese Berechtigungen müssen auf der Ressourcengruppen- oder Abonnementebene gewährt werden. |
| Ändern des Tarifs                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Anzeigen von Daten auf den Kacheln der *Backup*- und *Site Recovery*-Lösungen | Administrator/Co-Administrator | Zugriff auf Ressourcen, die mit dem klassischen Bereitstellungsmodell bereitgestellt werden |
| Erstellen eines Arbeitsbereichs im Azure-Portal                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Verwalten des Zugriffs auf Log Analytics mit Azure-Berechtigungen
Führen Sie die Schritte unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](../role-based-access-control/role-assignments-portal.md) aus, um den Zugriff auf den Log Analytics-Arbeitsbereich mit Azure-Berechtigungen zu gewähren.

Azure verfügt über zwei integrierte Benutzerrollen für Log Analytics:
- Log Analytics-Leser
- Log Analytics-Mitwirkender

Mitglieder der Rolle *Log Analytics-Leser* können folgende Aktionen ausführen:
- Anzeigen und Durchsuchen aller Überwachungsdaten 
- Anzeigen von Überwachungseinstellungen (einschließlich der Konfiguration von Azure-Diagnosen für alle Azure-Ressourcen)

Die Rolle „Log Analytics-Leser“ umfasst die folgenden Azure-Aktionen:

| Typ    | Berechtigung | BESCHREIBUNG |
| ------- | ---------- | ----------- |
| Aktion | `*/read`   | Anzeigen aller Azure-Ressourcen und der Ressourcenkonfiguration, einschließlich: <br> VM-Erweiterungsstatus <br> Konfiguration von Azure-Diagnosen für Ressourcen <br> Sämtliche Eigenschaften und Einstellungen aller Ressourcen |
| Aktion | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Ausführen von Protokollsuchabfragen (v2) |
| Aktion | `Microsoft.OperationalInsights/workspaces/search/action` | Ausführen von Protokollsuchabfragen (v1) |
| Aktion | `Microsoft.Support/*` | Öffnen von Supportfällen |
|Keine Aktion | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Verhindert das Lesen des Arbeitsbereichsschlüssels, der zum Verwenden der Datensammlungs-API und zum Installieren von Agents erforderlich ist. Dadurch wird verhindert, dass Benutzer dem Arbeitsbereich neue Ressourcen hinzufügen. |


Mitglieder der Rolle *Log Analytics-Mitwirkender* können folgende Aktionen ausführen:
- Lesen aller Überwachungsdaten (wie dies die Rolle „Log Analytics-Leser“ ermöglicht)  
- Erstellen und Konfigurieren von Automation-Konten  
- Hinzufügen und Entfernen von Verwaltungslösungen    
    > [!NOTE] 
    > Um die letzten beiden Aktionen erfolgreich ausführen zu können, muss diese Berechtigung auf der Ressourcengruppen- oder Abonnementebene gewährt werden.  

- Lesen von Speicherkontoschlüsseln   
- Konfigurieren der Sammlung von Protokollen aus Azure Storage  
- Bearbeiten von Überwachungseinstellungen für Azure-Ressourcen, einschließlich:
  - Hinzufügen der VM-Erweiterung zu virtuellen Computern
  - Konfigurieren von Azure-Diagnosen für alle Azure-Ressourcen

> [!NOTE] 
> Durch Hinzufügen einer VM-Erweiterung zu einem virtuellen Computer können Sie die vollständige Kontrolle über einen virtuellen Computer erlangen.

Die Rolle „Log Analytics-Mitwirkender“ umfasst die folgenden Azure-Aktionen:

| Berechtigung | BESCHREIBUNG |
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

Es empfiehlt sich, Zuweisungen auf der Ressourcenebene (Arbeitsbereich) vorzunehmen, um eine korrekte Zugriffssteuerung zu gewährleisten.  Verwenden Sie [benutzerdefinierte Rollen](../role-based-access-control/custom-roles.md), um Rollen mit spezifischen Berechtigungen zu erstellen.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Verknüpfen eines vorhandenen Arbeitsbereichs mit einem Azure-Abonnement
Ab dem 26. September 2016 müssen alle Arbeitsbereiche bei der Erstellung mit einem Azure-Abonnement verknüpft werden. Vor diesem Datum erstellte Arbeitsbereiche müssen bei der Anmeldung mit einem Abonnement verknüpft werden. Wenn Sie den Arbeitsbereich über das Azure-Portal erstellen oder den Arbeitsbereich mit einem Azure-Abonnement verknüpfen, wird Azure Active Directory als Organisationskonto verknüpft.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>So verknüpfen Sie einen Arbeitsbereich mit einem Azure-Abonnement im Azure-Portal
1. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.  

2. Klicken Sie im Bereich mit den Log Analytics-Abonnements auf **Hinzufügen**.  

    ![Liste der Arbeitsbereiche](./media/log-analytics-manage-access/workspace-link-existing-01.png)

3. Klicken Sie im Bereich **Log Analytics-Arbeitsbereich** auf **Vorhandene verknüpfen**.  

4. Klicken Sie auf **Erforderliche Einstellungen konfigurieren**.  

5. Daraufhin wird eine Liste mit Arbeitsbereichen angezeigt, die noch nicht mit Ihrem Azure-Konto verknüpft sind. Wählen Sie den Arbeitsbereich aus.  
   
6. Bei Bedarf können Sie die Werte für die folgenden Elemente ändern:
   * Abonnement
   * Ressourcengruppe
   * Standort
   * Tarif  

7. Klicken Sie auf **OK**. Der Arbeitsbereich ist jetzt mit Ihrem Azure-Konto verknüpft.

> [!NOTE]
> Wenn der Arbeitsbereich, den Sie verknüpfen möchten, hier nicht angezeigt wird, hat Ihr Azure-Abonnement keinen Zugriff auf den Arbeitsbereich, den Sie mit dem OMS-Portal erstellt haben.  Unter [Hinzufügen eines Benutzers zu einem vorhandenen Arbeitsbereich](#add-a-user-to-an-existing-workspace) erfahren Sie, wie Sie über das OMS-Portal Zugriff auf dieses Konto gewähren.
>
>

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Upgraden eines Arbeitsbereichs auf einen kostenpflichtigen Plan
Für OMS stehen drei Arten von Arbeitsbereichsplänen zur Verfügung: **Free**, **Eigenständig** und **OMS**.  Bei der Planoption *Free* ist das an Log Analytics gesendete Datenvolumen auf 500 MB pro Tag beschränkt.  Wenn Sie diese Menge überschreiten, müssen Sie Ihren Arbeitsbereich auf einen kostenpflichtigen Plan upgraden, um zu vermeiden, dass die über diesen Grenzwert hinausgehenden Daten nicht erfasst werden. Der Plantyp kann jederzeit geändert werden.  Weitere Informationen zu den Preisen für OMS finden Sie unter [Preise](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Verwenden von Berechtigungen aus einem OMS-Abonnement
Wenn Sie die Berechtigungen nutzen möchten, die Sie durch den Kauf von OMS E1, OMS E2 oder des OMS-Add-Ons für System Center erwerben, wählen Sie die Planoption *OMS* von OMS Log Analytics aus.

Beim Kauf eines OMS-Abonnements werden die Berechtigungen Ihrem Enterprise Agreement hinzugefügt. Die Berechtigungen stehen für alle Azure-Abonnements zur Verfügung, die im Rahmen dieses Vertrags erstellt werden. Verwenden Sie für alle Arbeitsbereiche in diesen Abonnements die OMS-Berechtigungen.

Gehen Sie wie folgt vor, um sicherzustellen, dass die Verwendung eines Arbeitsbereichs auf Ihre Berechtigungen des OMS-Abonnements angewendet wird:

1. Erstellen Sie Ihren Arbeitsbereich unter einem Azure-Abonnement, das zum Enterprise Agreement mit dem OMS-Abonnement gehört.

2. Wählen Sie für den Arbeitsbereich die Planoption *OMS* aus.

> [!NOTE]
> Wenn Ihr Arbeitsbereich vor dem 26. September 2016 erstellt wurde und Ihr Log Analytics-Preisplan *Premium* lautet, verwendet dieser Arbeitsbereich Berechtigungen aus dem OMS-Add-On für System Center. Die Berechtigungen können auch durch einen Wechsel zum Tarif *OMS* genutzt werden.
>
>

Die OMS-Abonnementberechtigungen werden im Azure-Portal nicht angezeigt. Die Berechtigungen und die Nutzung können im Enterprise Portal angezeigt werden.  

Wenn Sie das Azure-Abonnement ändern möchten, mit dem Ihr Arbeitsbereich verknüpft ist, können Sie das Azure PowerShell-Cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) verwenden.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Verwenden von Azure Commitment von einem Enterprise Agreement
Wenn Sie über kein OMS-Abonnement verfügen, wird jede OMS-Komponente separat abgerechnet, und die Nutzung wird auf Ihrer Azure-Rechnung ausgewiesen.

Wenn Sie über einen monetären Azure-Verpflichtungsbetrag für die Unternehmensanmeldung verfügen, mit der Ihre Azure-Abonnements verknüpft sind, wird die Nutzung von Log Analytics automatisch mit dem verbleibenden monetären Verpflichtungsbetrag verrechnet.

Zum Ändern des Azure-Abonnements, mit dem der Arbeitsbereich verknüpft ist, können Sie das Azure PowerShell-Cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) verwenden.  

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-azure-portal"></a>Umstellen eines Arbeitsbereichs auf einen kostenpflichtigen Tarif über das Azure-Portal
1. Wählen Sie im Azure-Portal im Bereich mit den Log Analytics-Abonnements einen Arbeitsbereich aus.

2. Klicken Sie im Bereich des Arbeitsbereichs unter **Allgemein** auf **Tarif**.  

3. Wählen Sie unter **Tarif** einen Tarif aus, und klicken Sie anschließend auf **Auswählen**.  
    ![Ausgewählter Tarif](./media/log-analytics-manage-access/workspace-pricing-tier-info.png)

> [!NOTE]
> Falls Ihr Arbeitsbereich mit einem Automation-Konto verknüpft ist und Sie den Tarif *Standalone (Per GB)* (Eigenständig (pro GB)) auswählen möchten, müssen Sie zuvor alle Lösungen vom Typ **Automation & Control** löschen und die Verknüpfung mit dem Automation-Konto aufheben. Klicken Sie auf dem Blatt für den Arbeitsbereich unter **Allgemein** auf **Lösungen**, um die Lösungen anzuzeigen und zu löschen. Klicken Sie zum Aufheben der Verknüpfung mit dem Automation-Konto auf dem Blatt **Tarif** auf den Namen des Automatisierungskontos.
>
>

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-oms-portal"></a>Umstellen eines Arbeitsbereichs auf einen kostenpflichtigen Tarif über das OMS-Portal

Sie müssen über ein Azure-Abonnement verfügen, um den Tarif mithilfe des OMS-Portals zu ändern.

1. Klicken Sie im OMS-Portal auf die Kachel **Einstellungen**.

2. Klicken Sie auf die Registerkarte **Konten** **Azure Subscription & Data Plan** (Azure-Abonnement und -Datentarif).

3. Klicken Sie auf den Tarif, den Sie verwenden möchten.

4. Klicken Sie auf **Speichern**.  

    ![Abonnement und Datentarife](./media/log-analytics-manage-access/subscription-tab.png)

Der neue Datentarif wird oben auf der Webseite im Menüband des OMS-Portals angezeigt.

![OMS-Menüband](./media/log-analytics-manage-access/data-plan-changed.png)

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Sammeln von Daten von Computern in Ihrem Datencenter oder einer anderen Cloudumgebung finden Sie unter [Sammeln von Daten von Computern in Ihrer Umgebung mit Log Analytics](log-analytics-concept-hybrid.md).
* Informationen zum Konfigurieren der Datensammlung von virtuellen Azure-Computern finden Sie unter [Sammeln von Daten über virtuelle Azure-Computer](log-analytics-quick-collect-azurevm.md).  
* [Add Log Analytics solutions from the Solutions Gallery](../monitoring/monitoring-solutions.md) (Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog) beschreibt das Hinzufügen von Funktionen und das Sammeln von Daten.

