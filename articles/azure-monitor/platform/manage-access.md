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
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 6c8f48ce71e11d1de0c28b4dab5327ab03e54f28
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2019
ms.locfileid: "54231783"
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
* Bereich für die Konfiguration von Einstellungen wie [Tarif](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier), [Aufbewahrung](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period) und [Datenobergrenzen](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#daily-cap) 

Im Hinblick auf die Nutzung wird empfohlen, möglichst wenige Arbeitsbereiche zu erstellen. Dadurch werden Verwaltung und Abfragen einfacher und schneller. Auf der Grundlage der obigen Merkmale können Sie jedoch in folgenden Szenarien mehrere Arbeitsbereiche erstellen:

* Sie sind ein globales Unternehmen und müssen Daten aus Gründen der Datensouveränität bzw. aus Compliancegründen in bestimmten Regionen speichern.
* Sie nutzen Azure und möchten Gebühren für ausgehende Datenübertragungen vermeiden, indem Sie einen Arbeitsbereich in derselben Region wie die verwalteten Azure-Ressourcen nutzen.
* Sie möchten Gebühren basierend auf der Nutzung unterschiedlichen Abteilungen bzw. Geschäftseinheiten zuordnen, indem Sie einen Arbeitsbereich für jede Abteilung oder Geschäftseinheit im eigenen Azure-Abonnement erstellen.
* Sie sind ein Dienstanbieter mit Verwaltung und müssen die Log Analytics-Daten für jeden Kunden, den Sie verwalten, von den Daten der anderen Kunden isolieren.
* Sie verwalten mehrere Kunden und möchten, dass den einzelnen Kunden/Abteilungen/Geschäftseinheiten jeweils nur die eigenen Daten angezeigt werden.

Wenn Sie Windows-Agents zum Sammeln von Daten verwenden, können Sie [jeden Agent so konfigurieren, dass er Informationen zu mindestens einem Arbeitsbereich meldet](../../azure-monitor/platform/agent-windows.md).

Bei Verwendung von System Center Operations Manager kann jede Operations Manager-Verwaltungsgruppe mit nur einem Arbeitsbereich verbunden werden. Sie können den Microsoft Monitoring Agent auf Computern installieren, die mit Operations Manager verwaltet werden, und den Agent so einrichten, dass er sowohl Daten an Operations Manager als auch an einen anderen Log Analytics-Arbeitsbereich liefert.

## <a name="workspace-information"></a>Informationen zum Arbeitsbereich

Sie können im Azure-Portal die Details zu Ihrem Arbeitsbereich anzeigen. 

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com)an, falls Sie dies noch nicht getan haben.

2. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.  

    ![Azure-Portal](media/manage-access/azure-portal-01.png)  

3. Wählen Sie im Bereich mit den Log Analytics-Abonnements einen Arbeitsbereich aus.

4. Auf der Seite für den Arbeitsbereich werden Details zu den ersten Schritten und zur Konfiguration sowie Links zu weiteren Informationen angezeigt.  

    ![Informationen zum Arbeitsbereich](./media/manage-access/workspace-overview-page.png)  

## <a name="manage-accounts-and-users"></a>Verwalten von Konten und Benutzern
Jedem Arbeitsbereich können mehrere Konten zugeordnet werden, und jedes Konto kann Zugriff auf mehrere Arbeitsbereiche haben. Der Zugriff wird über die [rollenbasierte Zugriffssteuerung in Azure](../../role-based-access-control/role-assignments-portal.md) verwaltet. Diese Zugriffsrechte gelten für das Azure-Portal sowie für den API-Zugriff.


Für die folgenden Aktivitäten sind ebenfalls Azure-Berechtigungen erforderlich:

| Aktion                                                          | Azure-Berechtigungen erforderlich | Notizen |
|-----------------------------------------------------------------|--------------------------|-------|
| Hinzufügen und Entfernen von Verwaltungslösungen                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Diese Berechtigungen müssen auf der Ressourcengruppen- oder Abonnementebene gewährt werden. |
| Ändern des Tarifs                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Anzeigen von Daten auf den Kacheln der *Backup*- und *Site Recovery*-Lösungen | Administrator/Co-Administrator | Zugriff auf Ressourcen, die mit dem klassischen Bereitstellungsmodell bereitgestellt werden |
| Erstellen eines Arbeitsbereichs im Azure-Portal                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Verwalten des Zugriffs auf Log Analytics mit Azure-Berechtigungen
Führen Sie die Schritte unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](../../role-based-access-control/role-assignments-portal.md) aus, um den Zugriff auf den Log Analytics-Arbeitsbereich mit Azure-Berechtigungen zu gewähren.

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

Es empfiehlt sich, Zuweisungen auf der Ressourcenebene (Arbeitsbereich) vorzunehmen, um eine korrekte Zugriffssteuerung zu gewährleisten.  Verwenden Sie [benutzerdefinierte Rollen](../../role-based-access-control/custom-roles.md), um Rollen mit spezifischen Berechtigungen zu erstellen.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Erfassen der Daten von Computern in Ihrem Datencenter oder einer anderen Cloudumgebung finden Sie unter [Collect log data with the Azure Log Analytics agent](../../azure-monitor/platform/log-analytics-agent.md) (Sammeln von Protokolldaten mit dem Azure Log Analytics-Agent).
* Informationen zum Konfigurieren der Datensammlung von virtuellen Azure-Computern finden Sie unter [Sammeln von Daten über virtuelle Azure-Computer](../../azure-monitor/learn/quick-collect-azurevm.md).  
* [Add Log Analytics solutions from the Solutions Gallery](../../azure-monitor/insights/solutions.md) (Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog) beschreibt das Hinzufügen von Funktionen und das Sammeln von Daten.

