---
title: Erstellen, Anzeigen und Verwalten von Aktivitätsprotokollwarnungen in Azure Monitor
description: Vorgehensweise zum Erstellen von Aktivitätsprotokollwarnungen über Azure-Portal, Ressourcenvorlage und PowerShell.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: a95cdbb48371cf960211f55bf077cea9db783db5
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248328"
---
# <a name="create-view-and-manage-activity-log-alerts-using-azure-monitor"></a>Erstellen, Anzeigen und Verwalten von Aktivitätsprotokollwarnungen mit Azure Monitor  

## <a name="overview"></a>Übersicht
Aktivitätsprotokollwarnungen werden aktiviert, wenn ein neues Aktivitätsprotokollereignis eintritt, das die in der Warnung angegebenen Bedingungen erfüllt.

Diese Warnungen gelten für Azure-Ressourcen und können mit einer Azure Resource Manager-Vorlage erstellt werden. Sie können auch im Azure-Portal erstellt, aktualisiert oder gelöscht werden. Normalerweise erstellen Sie Aktivitätsprotokollwarnungen, um Benachrichtigungen zu erhalten, wenn es für Ressourcen in Ihrem Azure-Abonnement zu spezifischen Änderungen kommt, die häufig für bestimmte Ressourcengruppen oder Ressourcen gelten. Es kann beispielsweise sein, dass Sie benachrichtigt werden möchten, wenn ein virtueller Computer in **myProductionResourceGroup** (Beispielressourcengruppe) gelöscht wird oder wenn einem Benutzer Ihres Abonnements neue Rollen zugewiesen werden.

> [!IMPORTANT]
> Warnungen zur Service Health-Benachrichtigung können nicht über die Schnittstelle zur Erstellung von Aktivitätsprotokollwarnungen erstellt werden. Weitere Informationen zum Erstellen und Verwenden von Dienstintegritätsbenachrichtigungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="azure-portal"></a>Azure-Portal

> [!NOTE]

>  Stellen Sie beim Erstellen der Warnungsregeln Folgendes sicher:

> - Das Abonnement im Bereich unterscheidet sich nicht von dem Abonnement, unter dem die Warnung erstellt wird.
- Folgende Kriterien müssen verwendet werden: Ebene/Status/Aufrufer/Ressourcengruppe/Ressourcen-ID/Ressourcentyp/Ereigniskategorie, unter der die Warnung konfiguriert wird.
- Der JSON-Code der Warnungskonfiguration enthält keine „anyOf“-Bedingung oder geschachtelten Bedingungen (nur ein „allOf“-Element ohne weitere „allOf“/„anyOf“-Elemente ist zulässig).
- Wenn die Kategorie „Administration“ lautet. Sie müssen mindestens eine der oben genannten Kriterien in der Warnung angeben. Eine Warnung, die jedes Mal aktiviert wird, wenn ein Ereignis in den Aktivitätsprotokollen erstellt wird, kann nicht erstellt werden.

### <a name="create-with-azure-portal"></a>Erstellen mit dem Azure-Portal

Gehen Sie dazu wie folgt vor:

1. Wählen Sie im Azure-Portal die Option **Überwachen** > **Warnungen**.
2. Klicken Sie oben im Fenster **Warnungen** auf **Neue Warnungsregel**.

     ![Neue Warnungsregel](./media/monitor-alerts-unified/AlertsPreviewOption.png)

     Das Fenster **Regel erstellen** wird angezeigt.

      ![Optionen für neue Warnungsregel](./media/monitoring-activity-log-alerts-new-experience/create-new-alert-rule-options.png)

3. Geben Sie unter **Warnungsbedingung definieren** die folgenden Informationen an, und klicken Sie auf **Fertig**.

    - **Warnungsziel**: Verwenden Sie zum Anzeigen und Auswählen des Ziels für die neue Warnung die Option **Nach Abonnement filtern** / **Nach Ressourcentyp filtern**, und wählen Sie die Ressource oder Ressourcengruppe aus der angegebenen Liste aus.

    > [!NOTE]

    > Sie können eine Ressource, eine Ressourcengruppe oder ein gesamtes Abonnement für das Aktivitätsprotokollsignal auswählen.

    **Beispielansicht für Warnungsziel** ![Ziel auswählen](./media/monitoring-activity-log-alerts-new-experience/select-target.png)

    - Klicken Sie unter **Zielkriterien** auf **Kriterien hinzufügen**. Daraufhin werden alle verfügbaren Signale für das Ziel angezeigt, einschließlich der Signale aus verschiedenen Kategorien von **Aktivitätsprotokoll**, wobei der Kategoriename an den Namen für den **Monitordienst** angehängt wird.

    - Wählen Sie das Signal aus der Liste der verschiedenen möglichen Operationen für den Typ **Aktivitätsprotokoll** aus.

    Sie können die Zeitachse für den Protokollverlauf und die entsprechende Warnungslogik für dieses Zielsignal auswählen:

    **Anzeige „Kriterien hinzufügen“**

    ![Kriterien hinzufügen](./media/monitoring-activity-log-alerts-new-experience/add-criteria.png)

    **Verlaufszeit**: Die für den ausgewählten Vorgang verfügbaren Ereignisse können über die letzten 6/12/24 Stunden oder über die letzte Woche aufgezeichnet werden.

    **Warnungslogik**:

     - **Ereignisstufe**: Der Schweregrad des Ereignisses. _Ausführlich_, _Information_, _Warnung_, _Fehler_ oder _Kritisch_.
     - **Status**: Der Status des Ereignisses. _Gestartet_, _Fehlgeschlagen_ oder _Erfolgreich_.
     - **Ereignis initiiert von**: Auch als Aufrufer bekannt; die E-Mail-Adresse oder der Azure Active Directory-Bezeichner des Benutzers, der den Vorgang durchgeführt hat.

        Beispielsignaldiagramm mit angewandter Warnungslogik:

        ![ ausgewählten Kriterien](./media/monitoring-activity-log-alerts-new-experience/criteria-selected.png)

4. Geben Sie unter **define alert rules details** (Informationen zu Warnungsregeln definieren) die folgenden Details an:

    - **Name der Warnungsregel**: Der Name für die neue Warnungsregel.
    - **Beschreibung**: Die Beschreibung für die neue Warnungsregel.
    - **Warnung in Ressourcengruppe speichern**: Wählen Sie die Ressourcengruppe aus, in der Sie die neue Regel speichern möchten.

5. Geben Sie unter **Aktionsgruppe** im Dropdownmenü die Aktionsgruppe an, der Sie die neue Warnungsregel zuweisen möchten. Alternativ hierzu können Sie auch [eine neue Aktionsgruppe erstellen](monitoring-action-groups.md) und der neuen Regel zuweisen. Klicken Sie zum Erstellen einer neuen Gruppe auf **+ Neue Gruppe**.

6. Klicken Sie zum Aktivieren der Regeln nach der Erstellung für **Regel beim Erstellen aktivieren** auf **Ja**.
7. Klicken Sie auf **Warnungsregel erstellen**.

    Die neue Warnungsregel für das Aktivitätsprotokoll wird erstellt, und oben rechts im Fenster wird eine Bestätigungsmeldung angezeigt.

    Sie können eine Regel aktivieren, deaktivieren, bearbeiten oder löschen. [Erfahren Sie mehr](#view-and-manage-activity-log-alert-rules-in-azure-portal) zur Verwaltung von Aktivitätsprotokollregeln.


Alternativ ist eine einfache Analogie zum Verständnis der Bedingungen, unter denen Warnungsregeln für das Aktivitätsprotokoll erstellt werden können, die Untersuchung oder Filterung von Ereignissen über das [Aktivitätsprotokoll im Azure-Portal](monitoring-overview-activity-logs.md#query-the-activity-log-in-the-azure-portal). In Azure Monitor – Aktivitätsprotokoll können Sie erforderliche Ereignisse filtern oder finden und dann eine Warnung mit der Schaltfläche **Aktivitätsprotokollwarnung hinzufügen** erstellen. Befolgen Sie dann ab Schritt 4 die erforderlichen Schritte wie im obigen Tutorial beschrieben.
    
 ![ Hinzufügen einer Warnung aus dem Aktivitätsprotokoll](./media/monitoring-activity-log-alerts-new-experience/add-activity-log.png)
    

### <a name="view-and-manage-in-azure-portal"></a>Anzeigen und Verwalten im Azure-Portal

1. Klicken Sie im Azure-Portal auf **Überwachen** > **Warnungen** und dann oben links im Fenster auf **Regeln verwalten**.

    ![ Verwalten von Warnungsregeln](./media/monitoring-activity-log-alerts-new-experience/manage-alert-rules.png)

    Die Liste mit den verfügbaren Regeln wird angezeigt.

2. Suchen Sie nach der zu ändernden Aktivitätsprotokollregel.

    ![ Suche nach Regel für Aktivitätsprotokollwarnungen](./media/monitoring-activity-log-alerts-new-experience/searth-activity-log-rule-to-edit.png)

    Sie können die verfügbaren Filter _Abonnement_, _Ressourcengruppe_, _Ressource_, _Signaltyp_ oder _Status_ verwenden, um die Aktivitätsregel zu ermitteln, die Sie bearbeiten möchten.

    > [!NOTE]

    > Sie können nur **Beschreibung**, **Zielkriterien** und **Aktionsgruppen** bearbeiten.

3.  Wählen Sie die Regel aus, und doppelklicken Sie darauf, um die Regeloptionen zu bearbeiten. Nehmen Sie die erforderlichen Änderungen vor, und klicken Sie dann auf **Speichern**.

    ![ Verwalten von Warnungsregeln](./media/monitoring-activity-log-alerts-new-experience/activity-log-rule-edit-page.png)

4.  Sie können eine Regel deaktivieren, aktivieren oder löschen. Wählen Sie oben im Fenster die entsprechende Option, nachdem Sie die Regel wie in Schritt 2 beschrieben ausgewählt haben.


## <a name="azure-resource-template"></a>Azure-Ressourcenvorlage
Um eine Aktivitätsprotokollwarnung mithilfe einer Resource Manager-Vorlage zu erstellen, müssen Sie eine Ressource des Typs `microsoft.insights/activityLogAlerts` erstellen. Anschließend tragen Sie alle zugehörigen Eigenschaften ein. Hier sehen Sie eine Vorlage, mit der eine Aktivitätsprotokollwarnung erstellt wird.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```
Die Json-Beispiel oben kann im Rahmen dieser exemplarischen Vorgehensweise z. B. als „sampleActivityLogAlert.json“ gespeichert und mithilfe von [Azure Resource Manager im Azure-Portal](../azure-resource-manager/resource-group-template-deploy-portal.md) bereitgestellt werden.

> [!NOTE]
> Es kann bis zu 5 Minuten dauern, bis eine neue Warnungsregel des Aktivitätsprotokolls aktiv wird.

## <a name="rest-api"></a>REST-API 
[Azure Monitor – API für Aktivitätsprotokollwarnungen](https://docs.microsoft.com/rest/api/monitor/activitylogalerts) ist eine REST-API und vollständig kompatibel mit der Azure Resource Manager-REST-API. Daher kann es über Powershell mit dem Resource Manager-Cmdlet und der Azure CLI verwendet werden.

## <a name="powershell"></a>PowerShell
Nachfolgend wird die Verwendung über das PowerShell-Cmdlet von Azure Resource Manager für die weiter oben im [Abschnitt „Ressourcenvorlage“](#manage-alert-rules-for-activity-log-using-azure-resource-template) dargestellte Ressourcenvorlage (sampleActivityLogAlert.json) veranschaulicht:
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```
Darin enthält die „sampleActivityLogAlert.parameters.json“ die Werte für die Parameter, die für die Erstellung der Warnungsregeln erforderlich sind.

## <a name="cli"></a>Befehlszeilenschnittstelle (CLI)
Nachfolgend wird die Verwendung über den Azure Resource Manager-Befehl in der Azure CLI für die weiter oben im [Abschnitt „Ressourcenvorlage“](#manage-alert-rules-for-activity-log-using-azure-resource-template) dargestellte Ressourcenvorlage (sampleActivityLogAlert.json) veranschaulicht:

```azurecli
az group deployment create --resource-group myRG --template-file sampleActivityLogAlert.json --parameters @sampleActivityLogAlert.parameters.json
```
Die Datei *sampleActivityLogAlert.parameters.json* enthält die Werte für die Parameter, die für die Erstellung der Warnungsregeln erforderlich sind.


## <a name="next-steps"></a>Nächste Schritte

- [Webhookschema für Aktivitätsprotokolle](monitoring-activity-log-alerts-webhook.md)
- [Übersicht über Aktivitätsprotokolle](monitoring-activity-log-alerts.md) 
- Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md).  
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](monitoring-service-notifications.md).
