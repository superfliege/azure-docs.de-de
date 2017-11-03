---
title: Automatisches Skalieren von VM-Skalierungsgruppen mit der Azure CLI | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie Regeln für die automatische Skalierung für VM-Skalierungsgruppen mit Azure CLI 2.0 erstellen."
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 83e93d9c-cac0-41d3-8316-6016f5ed0ce4
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.openlocfilehash: 6e8fadd54a78d432ed802f4c4880c2f77bb28c37
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="automatically-scale-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Automatisches Skalieren einer VM-Skalierungsgruppe mit Azure CLI 2.0
Beim Erstellen einer Skalierungsgruppe definieren Sie die Anzahl von VM-Instanzen, die Sie ausführen möchten. Wenn sich die Nachfrage nach Ihrer Anwendung ändert, können Sie die Anzahl von VM-Instanzen automatisch erhöhen oder verringern lassen. Dank der Möglichkeit zum automatischen Skalieren können Sie über den gesamten Lebenszyklus Ihrer App die Kundennachfrage decken oder auf Änderungen der Anwendungsleistung reagieren.

In diesem Artikel wird veranschaulicht, wie Sie mit Azure CLI 2.0 Regeln für die automatische Skalierung erstellen, mit denen die Leistung der VM-Instanzen in Ihrer Skalierungsgruppe überwacht wird. Mit diesen Regeln für die automatische Skalierung wird die Anzahl von VM-Instanzen erhöht oder verringert, um auf die Werte dieser Leitungsmetriken zu reagieren. Sie können diese Schritte auch mit [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) oder im [Azure-Portal](virtual-machine-scale-sets-autoscale-portal.md) ausführen.


## <a name="prerequisites"></a>Voraussetzungen
Zum Erstellen von Regeln für die automatische Skalierung benötigen Sie eine vorhandene VM-Skalierungsgruppe. Sie können eine Skalierungsgruppe mit dem [Azure-Portal](virtual-machine-scale-sets-portal-create.md) oder per [Azure CLI 2.0](virtual-machine-scale-sets-create.md#create-from-azure-cli) oder [Azure PowerShell](virtual-machine-scale-sets-create.md#create-from-powershell) erstellen.

Definieren Sie einige Variablen für Ihre Skalierungsgruppe, um die Erstellung der Regeln für die automatische Skalierung zu vereinfachen. Im folgenden Beispiel werden Variablen für die Skalierungsgruppe mit dem Namen *myScaleSet* in der Ressourcengruppe *myResourceGroup* in der Region *eastus* definiert. Sie können Ihre Abonnement-ID mit [az account show](/cli/azure/account#az_account_show) abrufen. Wenn Ihrem Konto mehrere Abonnements zugeordnet sind, wird nur das erste Abonnement zurückgegeben. Passen Sie die Namen und die Abonnement-ID wie folgt an:

```azurecli
sub=$(az account show --query id -o tsv)
resourcegroup_name="myResourceGroup"
scaleset_name="myScaleSet"
location_name="eastus"
```

## <a name="define-an-autoscale-profile"></a>Definieren eines Profils für die automatische Skalierung
Die Regeln für die automatische Skalierung werden im JSON-Format (JavaScript Object Notation) per Azure CLI 2.0 bereitgestellt. Der vollständige JSON-Code, mit dem die Regeln für die automatische Skalierung definiert und bereitgestellt werden, ist weiter unten in diesem Artikel angegeben. 

Zu Beginn des Profils für die automatische Skalierung werden die standardmäßige, minimale und maximale Kapazität der Skalierungsgruppe angegeben. Im folgenden Beispiel werden als Standard- und Minimalwert *2* und als Maximalwert *10* VM-Instanzen festgelegt:

```json
{
  "name": "autoscale rules",
  "capacity": {
    "minimum": "2",
    "maximum": "10",
    "default": "2"
  }
}
```


## <a name="create-a-rule-to-automatically-scale-out"></a>Erstellen einer Regel für das automatische horizontale Hochskalieren
Wenn sich die Nachfrage für Ihre Anwendung erhöht, erhöht sich auch die Last für die VM-Instanzen in Ihrer Skalierungsgruppe. Falls es sich um eine dauerhafte Last und nicht nur um eine kurzzeitige höhere Nachfrage handelt, können Sie die Regeln für die automatische Skalierung konfigurieren, um die Anzahl von VM-Instanzen in der Skalierungsgruppe zu erhöhen. Nachdem diese VM-Instanzen erstellt und Ihre Anwendungen bereitgestellt wurden, beginnt die Skalierungsgruppe damit, über das Lastenausgleichsmodul Datenverkehr darauf zu verteilen. Sie steuern, welche Metriken überwacht werden, z.B. CPU oder Datenträger, wie lange die Anwendungslast einen bestimmten Schwellenwert einhalten muss und wie viele VM-Instanzen der Skalierungsgruppe hinzugefügt werden sollen.

Wir erstellen eine Regel, mit der die Anzahl von VM-Instanzen in einer Skalierungsgruppe erhöht wird, wenn die durchschnittliche CPU-Last für einen Zeitraum von 10 Minuten über 70% liegt. Wenn die Regel ausgelöst wird, wird die Anzahl von VM-Instanzen um 20% erhöht. In Skalierungsgruppen mit einer kleinen Zahl von VM-Instanzen können Sie `type` auf *ChangeCount* festlegen und `value` um *1* oder *2* Instanzen erhöhen. In Skalierungsgruppen mit einer großen Zahl von VM-Instanzen ist eine Erhöhung um 10 oder 20 Prozent für die VM-Instanzen unter Umständen angemessener.

Für diese Regel werden die folgenden Parameter verwendet:

| Parameter         | Erklärung                                                                                                         | Wert           |
|-------------------|---------------------------------------------------------------------------------------------------------------------|-----------------|
| *metricName*      | Die Leistungsmetrik zum Überwachen und Anwenden von Skalierungsgruppenaktionen.                                                   | CPU in Prozent  |
| *timeGrain*       | Gibt an, wie häufig Metriken zu Analysezwecken gesammelt werden.                                                                   | 1 Minute        |
| *timeAggregation* | Definiert, wie die gesammelten Metriken zu Analysezwecken aggregiert werden sollen.                                                | Durchschnitt         |
| *timeWindow*      | Der überwachte Zeitraum, bevor die Metrik und die Schwellenwerte verglichen werden.                                   | 10 Minuten      |
| *operator*        | Operator zum Vergleichen der Metrikdaten mit dem Schwellenwert.                                                     | Größer als    |
| *threshold*       | Der Wert, der für die Regel für die automatische Skalierung das Auslösen einer Aktion bewirkt.                                                      | 70%             |
| *direction*       | Definiert, ob die Skalierungsgruppe zentral hoch- oder herunterskaliert werden soll, wenn die Regel zutrifft.                                             | Erhöhung        |
| *type*            | Gibt an, dass die Anzahl von VM-Instanzen um einen Prozentbetrag geändert werden soll.                                 | Prozentuale Änderung  |
| *value*           | Gibt an, wie viele VM-Instanzen zentral hoch- oder herunterskaliert werden sollen, wenn die Regel zutrifft.                                            | 20              |
| *cooldown*        | Gibt an, wie lange gewartet wird, bevor die Regel erneut angewendet wird, damit die Aktionen für die automatische Skalierung wirksam werden können. | 5 Minuten       |

Im folgenden Beispiel wird die Regel zum horizontalen Hochskalieren der Anzahl von VM-Instanzen definiert. Für das *metricResourceUri*-Element werden die Variablen verwendet, die zuvor für die Abonnement-ID, den Namen der Ressourcengruppe und den Namen der Skalierungsgruppe definiert wurden:

```json
{
  "metricTrigger": {
    "metricName": "Percentage CPU",
    "metricNamespace": "",
    "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
    "metricResourceLocation": "'$location_name'",
    "timeGrain": "PT1M",
    "statistic": "Average",
    "timeWindow": "PT10M",
    "timeAggregation": "Average",
    "operator": "GreaterThan",
    "threshold": 70
  },
  "scaleAction": {
    "direction": "Increase",
    "type": "PercentChangeCount",
    "value": "20",
    "cooldown": "PT5M"
  }
}
```


## <a name="create-a-rule-to-automatically-scale-in"></a>Erstellen einer Regel für das automatische horizontale Herunterskalieren
Es kann sein, dass die Nachfrage nach Ihrer Anwendung abends oder am Wochenende abnimmt. Wenn diese Verringerung der Last für einen bestimmten Zeitraum anhält, können Sie die Regeln der automatischen Skalierung konfigurieren, um die Anzahl von VM-Instanzen in der Skalierungsgruppe zu reduzieren. Mit dieser Aktion zum horizontalen Herunterskalieren werden die Kosten für die Ausführung Ihrer Skalierungsgruppe gesenkt, da Sie nur so viele Instanzen ausführen, wie für die Erfüllung der derzeitigen Nachfrage erforderlich sind.

Erstellen Sie eine weitere Regel, mit der die Anzahl von VM-Instanzen in einer Skalierungsgruppe verringert wird, wenn die durchschnittliche CPU-Last über einen Zeitraum von 10 Minuten unter 30% fällt. Im folgenden Beispiel wird die Regel zum horizontalen Hochskalieren der Anzahl von VM-Instanzen definiert. Für das *metricResourceUri*-Element werden die Variablen verwendet, die zuvor für die Abonnement-ID, den Namen der Ressourcengruppe und den Namen der Skalierungsgruppe definiert wurden:

```json
{
  "metricTrigger": {
    "metricName": "Percentage CPU",
    "metricNamespace": "",
    "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
    "metricResourceLocation": "'$location_name'",
    "timeGrain": "PT1M",
    "statistic": "Average",
    "timeWindow": "PT10M",
    "timeAggregation": "Average",
    "operator": "LessThan",
    "threshold": 30
  },
  "scaleAction": {
    "direction": "Decrease",
    "type": "PercentChangeCount",
    "value": "20",
    "cooldown": "PT5M"
  }
}
```


## <a name="apply-autoscale-rules-to-a-scale-set"></a>Anwenden von Regeln für die automatische Skalierung auf eine Skalierungsgruppe
Der letzte Schritt ist das Anwenden des Profils und der Regeln für die automatische Skalierung auf Ihre Skalierungsgruppe. Ihre Skalierung kann dann automatisch je nach Anwendungsnachfrage das horizontale Herunter- oder Hochskalieren durchführen. Wenden Sie das Profil für die automatische Skalierung wie folgt mit [az monitor autoscale-settings create](/cli/azure/monitor/autoscale-settings#az_monitor_autoscale_settings_create) an. Im vollständigen JSON-Code werden das Profil und die Regeln verwendet, die in den vorherigen Abschnitten beschrieben wurden.

```azurecli
az monitor autoscale-settings create \
    --resource-group myResourceGroup \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "'$location_name'",
      "notifications": [],
      "profiles": [
        {
          "name": "autoscale by percentage based on CPU usage",
          "capacity": {
            "minimum": "2",
            "maximum": "10",
            "default": "2"
          },
          "rules": [
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
                "metricResourceLocation": "'$location_name'",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT10M",
                "timeAggregation": "Average",
                "operator": "GreaterThan",
                "threshold": 70
              },
              "scaleAction": {
                "direction": "Increase",
                "type": "PercentChangeCount",
                "value": "20",
                "cooldown": "PT5M"
              }
            },
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
                "metricResourceLocation": "'$location_name'",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT10M",
                "timeAggregation": "Average",
                "operator": "LessThan",
                "threshold": 30
              },
              "scaleAction": {
                "direction": "Decrease",
                "type": "PercentChangeCount",
                "value": "20",
                "cooldown": "PT5M"
              }
            }
          ]
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'"
    }'
```


## <a name="monitor-number-of-instances-in-a-scale-set"></a>Überwachen der Anzahl von Instanzen in einer Skalierungsgruppe
Zum Prüfen der Anzahl und des Status von VM-Instanzen können Sie mit [az vmss list-instances](/cli/azure/vmss#az_vmss_list_instances) eine Liste mit den Instanzen Ihrer Skalierungsgruppe anzeigen. Der Status zeigt an, ob die VM-Instanz beim automatischen horizontalen Hochskalieren bereitgestellt wird oder ob die Bereitstellung beim automatischen horizontalen Herunterskalieren aufgehoben wird. Im folgenden Beispiel wird der VM-Instanzstatus für die Skalierungsgruppe *myScaleSet* in der Ressourcengruppe *myResourceGroup* angezeigt:

```azurecli
az vmss list-instances \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --output table
```


## <a name="autoscale-based-on-a-schedule"></a>Automatisches Skalieren nach einem Zeitplan
In den vorherigen Beispielen wurde eine Skalierungsgruppe anhand von einfachen Hostmetriken, z.B. der CPU-Auslastung, automatisch herunter- oder hochskaliert. Sie können Regeln für die automatische Skalierung auch anhand von Zeitplänen erstellen. Mit diesen zeitplanbasierten Regeln können Sie die Anzahl von VM-Instanzen vor einem erwarteten Anstieg der Anwendungsnachfrage, z.B. der Kernarbeitszeit, automatisch horizontal hochskalieren und dann für Zeiträume mit geringerer Nachfrage, z.B. am Wochenende, wieder automatisch zentral herunterskalieren.

Erstellen Sie zum Verwenden von zeitplanbasierten Regeln für die automatische Skalierung ein JSON-Profil, mit dem die Anzahl von VM-Instanzen definiert wird, die für ein Fenster mit fester Start- und Endzeit ausgeführt werden sollen. Im folgenden Beispiel wird eine Regel zum horizontalen Hochskalieren auf *10* Instanzen um *9* Uhr an jedem Werktag (Montag bis Freitag) definiert.

```json
{
  "name": "Scale out during each work day",
  "capacity": {
      "minimum": "10",
      "maximum": "10",
      "default": "10"
  },
  "rules": [],
  "recurrence": {
      "frequency": "Week",
      "schedule": {
          "timeZone": "Pacific Standard Time",
          "days": [
              "Monday",
              "Tuesday",
              "Wednesday",
              "Thursday",
              "Friday"
          ],
          "hours": [
              9
          ],
          "minutes": [
              0
          ]
      }
  }
}
```

Erstellen Sie eine weitere Regel für das horizontale Herunterskalieren am Abend, mit der eine niedrigere Anzahl von VM-Instanzen und eine entsprechende Startzeit angegeben wird.

Im folgenden vollständigen Beispiel werden die Regeln zum horizontalen Hoch- und Herunterskalieren definiert, und anschließend wird das Profil für die automatische Skalierung mit [az monitor autoscale-settings create](/cli/azure/monitor/autoscale-settings#az_monitor_autoscale_settings_create) angewendet. In diesem Beispiel werden die metrikbasierten Regeln für die automatische Skalierung überschrieben, die in den vorherigen Beispielen erstellt wurden. Für das *metricResourceUri*-Element werden die Variablen verwendet, die zuvor für die Abonnement-ID, den Namen der Ressourcengruppe und den Namen der Skalierungsgruppe definiert wurden:

```azurecli
az monitor autoscale-settings create \
    --resource-group myResourceGroup \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "'$location_name'",
      "notifications": [],
      "profiles": [
        {
          "name": "Scale out during each work day",
          "capacity": {
            "minimum": "10",
            "maximum": "10",
            "default": "10"
          },
          "rules": [],
          "recurrence": {
            "frequency": "Week",
            "schedule": {
              "timeZone": "Pacific Standard Time",
              "days": [
                "Monday",
                "Tuesday",
                "Wednesday",
                "Thursday",
                "Friday"
              ],
              "hours": [
                9
              ],
              "minutes": [
                0
              ]
            }
          }
        },
        {
          "name": "Scale in during the evening",
          "capacity": {
            "minimum": "3",
            "maximum": "3",
            "default": "3"
          },
          "rules": [],
          "recurrence": {
            "frequency": "Week",
            "schedule": {
              "timeZone": "Pacific Standard Time",
              "days": [
                "Monday",
                "Tuesday",
                "Wednesday",
                "Thursday",
                "Friday"
              ],
              "hours": [
                18
              ],
              "minutes": [
                0
              ]
            }
          }
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'"
    }'
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde beschrieben, wie Sie mit Regeln für die automatische Skalierung horizontal skalieren und die *Anzahl* von VM-Instanzen in Ihrer Skalierungsgruppe erhöhen oder verringern. Sie können auch vertikal skalieren, um die *Größe* der VM-Instanz zu erhöhen oder zu verringern. Weitere Informationen finden Sie unter [Vertikale automatische Skalierung mit VM-Skalierungsgruppen](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Informationen zur Verwaltung Ihrer VM-Instanzen finden Sie unter [Manage a virtual machine scale set with Azure PowerShell](virtual-machine-scale-sets-windows-manage.md) (Verwalten einer VM-Skalierungsgruppe mit Azure PowerShell).

Informationen zum Generieren von Warnungen bei der Auslösung von Regeln für die automatische Skalierung finden Sie unter [Verwenden von automatischen Skalierungsvorgängen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). Informieren Sie sich auch über das [Aufrufen eines Webhooks für Azure-Aktivitätsprotokollwarnungen](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
