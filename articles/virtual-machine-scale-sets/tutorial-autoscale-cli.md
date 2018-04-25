---
title: Tutorial – Automatisches Skalieren einer Skalierungsgruppe mit Azure CLI 2.0 | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine VM-Skalierungsgruppe mit Azure CLI 2.0 automatisch skalieren, wenn sich die CPU-Anforderungen erhöhen oder verringern.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6f184ac0b2af3a66affecd1a3a9c247a96e616f8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-automatically-scale-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Tutorial: Automatisches Skalieren einer VM-Skalierungsgruppe mit Azure CLI 2.0
Beim Erstellen einer Skalierungsgruppe definieren Sie die Anzahl von VM-Instanzen, die Sie ausführen möchten. Wenn sich die Nachfrage nach Ihrer Anwendung ändert, können Sie die Anzahl von VM-Instanzen automatisch erhöhen oder verringern lassen. Dank der Möglichkeit zum automatischen Skalieren können Sie über den gesamten Lebenszyklus Ihrer App die Kundennachfrage decken oder auf Änderungen der Anwendungsleistung reagieren. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verwenden der automatischen Skalierung mit einer Skalierungsgruppe
> * Erstellen und Verwenden von Regeln für die automatische Skalierung
> * Durchführen eines Belastungstests für VM-Instanzen und Auslösen von Regeln für die automatische Skalierung
> * Zurückfahren der automatischen Skalierung bei einer Reduzierung des Bedarfs

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens die Azure CLI-Version 2.0.29 ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 


## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe
Definieren Sie einige Parameter für Ihre Abonnement-ID, die Ressourcengruppe, den Namen der Skalierungsgruppe und den Standort, um die Erstellung von Regeln für die automatische Skalierung zu unterstützen:

```azurecli-interactive
sub=$(az account show --query id -o tsv)
resourcegroup_name="myResourceGroup"
scaleset_name="myScaleSet"
location_name="eastus"
```

Erstellen Sie mit [az group create](/cli/azure/group#create) wie folgt eine Ressourcengruppe:

```azurecli-interactive
az group create --name $resourcegroup_name --location $location_name
```

Erstellen Sie dann mit [az vmss create](/cli/azure/vmss#create) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine Skalierungsgruppe mit der Instanzanzahl *2* erstellt, und es werden SSH-Schlüssel generiert, falls sie noch nicht vorhanden sind:

```azurecli-interactive
az vmss create \
  --resource-group $resourcegroup_name \
  --name $scaleset_name \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --instance-count 2 \
  --admin-username azureuser \
  --generate-ssh-keys
```


## <a name="define-an-autoscale-profile"></a>Definieren eines Profils für die automatische Skalierung
Die Regeln für die automatische Skalierung werden im JSON-Format (JavaScript Object Notation) per Azure CLI 2.0 bereitgestellt. Wir sehen uns die einzelnen Teile dieses Profils für die automatische Skalierung an und erstellen dann ein vollständiges Beispiel.

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


## <a name="create-a-rule-to-autoscale-out"></a>Erstellen einer Regel zum automatischen horizontalen Hochskalieren
Wenn sich die Nachfrage für Ihre Anwendung erhöht, erhöht sich auch die Last für die VM-Instanzen in Ihrer Skalierungsgruppe. Falls es sich um eine dauerhafte Last und nicht nur um eine kurzzeitige höhere Nachfrage handelt, können Sie die Regeln für die automatische Skalierung konfigurieren, um die Anzahl von VM-Instanzen in der Skalierungsgruppe zu erhöhen. Nachdem diese VM-Instanzen erstellt und Ihre Anwendungen bereitgestellt wurden, beginnt die Skalierungsgruppe damit, über das Lastenausgleichsmodul Datenverkehr darauf zu verteilen. Sie steuern, welche Metriken überwacht werden, z.B. CPU oder Datenträger, wie lange die Anwendungslast einen bestimmten Schwellenwert einhalten muss und wie viele VM-Instanzen der Skalierungsgruppe hinzugefügt werden sollen.

Wir erstellen eine Regel, mit der die Anzahl von VM-Instanzen in einer Skalierungsgruppe erhöht wird, wenn die durchschnittliche CPU-Last fünf Minuten lang über 70% liegt. Wenn die Regel ausgelöst wird, wird die Anzahl von VM-Instanzen um 3 erhöht.

Für diese Regel werden die folgenden Parameter verwendet:

| Parameter         | Erklärung                                                                                                         | Wert           |
|-------------------|---------------------------------------------------------------------------------------------------------------------|-----------------|
| *metricName*      | Die Leistungsmetrik zum Überwachen und Anwenden von Skalierungsgruppenaktionen.                                                   | CPU in Prozent  |
| *timeGrain*       | Gibt an, wie häufig Metriken zu Analysezwecken gesammelt werden.                                                                   | 1 Minute        |
| *timeAggregation* | Definiert, wie die gesammelten Metriken zu Analysezwecken aggregiert werden sollen.                                                | Durchschnitt         |
| *timeWindow*      | Der überwachte Zeitraum, bevor die Metrik und Schwellenwerte verglichen werden.                                   | 5 Minuten       |
| *operator*        | Operator zum Vergleichen der Metrikdaten mit dem Schwellenwert.                                                     | Größer als    |
| *threshold*       | Der Wert, der für die Regel für die automatische Skalierung das Auslösen einer Aktion bewirkt.                                                      | 70 %             |
| *direction*       | Definiert, ob die Skalierungsgruppe horizontal hoch- oder herunterskaliert werden soll, wenn die Regel zutrifft.                                              | Erhöhung        |
| *type*            | Gibt an, dass die Anzahl von VM-Instanzen um einen bestimmten Wert geändert werden soll.                                    | Änderungsanzahl    |
| *value*           | Gibt an, wie viele VM-Instanzen horizontal hoch- oder herunterskaliert werden sollen, wenn die Regel zutrifft.                                             | 3               |
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
    "timeWindow": "PT5M",
    "timeAggregation": "Average",
    "operator": "GreaterThan",
    "threshold": 70
  },
  "scaleAction": {
    "direction": "Increase",
    "type": "ChangeCount",
    "value": "3",
    "cooldown": "PT5M"
  }
}
```


## <a name="create-a-rule-to-autoscale-in"></a>Erstellen einer Regel zum automatischen horizontalen Herunterskalieren
Es kann sein, dass die Nachfrage nach Ihrer Anwendung abends oder am Wochenende abnimmt. Wenn diese Verringerung der Last für einen bestimmten Zeitraum anhält, können Sie Regeln zur automatischen Skalierung konfigurieren, um die Anzahl von VM-Instanzen in der Skalierungsgruppe zu reduzieren. Mit dieser Aktion zum horizontalen Herunterskalieren werden die Kosten für die Ausführung Ihrer Skalierungsgruppe gesenkt, da Sie nur so viele Instanzen ausführen, wie für die Erfüllung der derzeitigen Nachfrage erforderlich sind.

Erstellen Sie eine weitere Regel, mit der die Anzahl von VM-Instanzen in einer Skalierungsgruppe verringert wird, wenn die durchschnittliche CPU-Last fünf Minuten lang unter 30% fällt. Im folgenden Beispiel wird die Regel zum horizontalen Herunterskalieren der Anzahl von VM-Instanzen um 1 definiert. Für das *metricResourceUri*-Element werden die Variablen verwendet, die zuvor für die Abonnement-ID, den Namen der Ressourcengruppe und den Namen der Skalierungsgruppe definiert wurden:

```json
{
  "metricTrigger": {
    "metricName": "Percentage CPU",
    "metricNamespace": "",
    "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/'$resourcegroup_name'/providers/Microsoft.Compute/virtualMachineScaleSets/'$scaleset_name'",
    "metricResourceLocation": "'$location_name'",
    "timeGrain": "PT1M",
    "statistic": "Average",
    "timeWindow": "PT5M",
    "timeAggregation": "Average",
    "operator": "LessThan",
    "threshold": 30
  },
  "scaleAction": {
    "direction": "Decrease",
    "type": "ChangeCount",
    "value": "1",
    "cooldown": "PT5M"
  }
}
```


## <a name="apply-autoscale-rules-to-a-scale-set"></a>Anwenden von Regeln für die automatische Skalierung auf eine Skalierungsgruppe
Der letzte Schritt ist das Anwenden des Profils und der Regeln für die automatische Skalierung auf Ihre Skalierungsgruppe. Ihre Skalierungsgruppe kann dann automatisch je nach Anwendungsnachfrage das horizontale Herunter- oder Hochskalieren durchführen. Wenden Sie das Profil für die automatische Skalierung wie folgt mit [az monitor autoscale-settings create](/cli/azure/monitor/autoscale-settings#az_monitor_autoscale_settings_create) an. Im folgenden vollständigen JSON-Code werden das Profil und die Regeln verwendet, die in den vorherigen Abschnitten beschrieben wurden:

```azurecli-interactive
az monitor autoscale-settings create \
    --resource-group $resourcegroup_name \
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
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "GreaterThan",
                "threshold": 70
              },
              "scaleAction": {
                "direction": "Increase",
                "type": "ChangeCount",
                "value": "3",
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
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "LessThan",
                "threshold": 30
              },
              "scaleAction": {
                "direction": "Decrease",
                "type": "ChangeCount",
                "value": "1",
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


## <a name="generate-cpu-load-on-scale-set"></a>Generieren einer CPU-Last in einer Skalierungsgruppe
Generieren Sie zum Testen der Regeln für die automatische Skalierung eine CPU-Last auf den VM-Instanzen in der Skalierungsgruppe. Diese simulierte CPU-Last bewirkt über die automatische Skalierung das horizontale Hochskalieren und eine Erhöhung der Anzahl von VM-Instanzen. Wenn die simulierte CPU-Last dann verringert wird, wird über die Regeln für das automatische Skalieren das horizontale Herunterskalieren durchgeführt und die Anzahl von VM-Instanzen reduziert.

Listen Sie mit [az vmss list-instance-connection-info](/cli/azure/vmss#az_vmss_list_instance_connection_info) zuerst die Adresse und die Ports für die Verbindungsherstellung mit VM-Instanzen in einer Skalierungsgruppe auf:

```azurecli-interactive
az vmss list-instance-connection-info \
  --resource-group $resourcegroup_name \
  --name $scaleset_name
```

Die folgende Beispielausgabe enthält den Instanznamen, die öffentliche IP-Adresse des Lastenausgleichs und die Portnummer, an die Datenverkehr durch die Regeln für die Netzwerkadressübersetzung (Network Address Translation, NAT) weitergeleitet wird:

```json
{
  "instance 1": "13.92.224.66:50001",
  "instance 3": "13.92.224.66:50003"
}
```

Stellen Sie eine SSH-Verbindung mit Ihrer ersten VM-Instanz her. Geben Sie mithilfe des Parameters `-p` Ihre eigene öffentliche IP-Adresse und die Portnummer an, wie im vorherigen Befehl gezeigt:

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50001
```

Installieren Sie nach dem Anmelden das Hilfsprogramm **stress**. Starten Sie *10* **stress**-Worker zum Generieren der CPU-Last. Diese Worker werden *420* Sekunden lang ausgeführt. Dieser Zeitraum reicht aus, um über die Regeln der automatischen Skalierung die gewünschte Aktion zu implementieren.

```azurecli-interactive
sudo apt-get -y install stress
sudo stress --cpu 10 --timeout 420 &
```

Wenn für **stress** eine Ausgabe der Art *stress: info: [2688] dispatching hogs: 10 cpu, 0 io, 0 vm, 0 hdd* angezeigt wird, können Sie die *EINGABETASTE* drücken, um zur Eingabeaufforderung zurückzukehren.

Um zu bestätigen, dass **stress** CPU-Last generiert, können Sie die aktive Systemauslastung mit dem Hilfsprogramm **top** untersuchen:

```azuecli-interactive
top
```

Beenden Sie **top**, und schließen Sie dann Ihre Verbindung mit der VM-Instanz. **stress** wird auf der VM-Instanz weiterhin ausgeführt.

```azurecli-interactive
Ctrl-c
exit
```

Stellen Sie über die Portnummer, die mit dem obigen Befehl [az vmss list-instance-connection-info](/cli/azure/vmss#az_vmss_list_instance_connection_info) aufgeführt wurde, eine Verbindung mit der zweiten VM-Instanz her:

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50003
```

Installieren Sie **stress**, und führen Sie das Hilfsprogramm aus. Starten Sie auf dieser zweiten VM-Instanz anschließend zehn Worker.

```azurecli-interactive
sudo apt-get -y install stress
sudo stress --cpu 10 --timeout 420 &
```

Wenn für **stress** eine Ausgabe der Art *stress: info: [2713] dispatching hogs: 10 cpu, 0 io, 0 vm, 0 hdd* angezeigt wird, können Sie wieder die *EINGABETASTE* drücken, um zur Eingabeaufforderung zurückzukehren.

Schließen Sie die Verbindung mit der zweiten VM-Instanz. **stress** wird auf der VM-Instanz weiterhin ausgeführt.

```azurecli-interactive
exit
```

## <a name="monitor-the-active-autoscale-rules"></a>Überwachen der aktiven Regeln für die automatische Skalierung
Verwenden Sie **watch**, um die Anzahl von VM-Instanzen in Ihrer Skalierungsgruppe zu überwachen. Es dauert fünf Minuten, bis die Regeln für die automatische Skalierung als Reaktion auf die CPU-Last, die mit **stress** auf den einzelnen VM-Instanzen generiert wurde, mit dem horizontalen Hochskalieren beginnen:

```azurecli-interactive
watch az vmss list-instances \
  --resource-group $resourcegroup_name \
  --name $scaleset_name \
  --output table
```

Nachdem der CPU-Schwellenwert erreicht wurde, wird mit den Regeln für die automatische Skalierung die Anzahl von VM-Instanzen in der Skalierungsgruppe erhöht. In der folgenden Ausgabe werden drei VMs angezeigt, die erstellt werden, wenn für die Skalierungsgruppe das automatische horizontale Hochskalieren durchgeführt wird:

```bash
Every 2.0s: az vmss list-instances --resource-group myResourceGroup --name myScaleSet --output table

  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup    VmId
------------  --------------------  ----------  ------------  -------------------  ---------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUP  4f92f350-2b68-464f-8a01-e5e590557955
           2  True                  eastus      myScaleSet_2  Succeeded            MYRESOURCEGROUP  d734cd3d-fb38-4302-817c-cfe35655d48e
           4  True                  eastus      myScaleSet_4  Creating             MYRESOURCEGROUP  061b4c90-0d73-49fc-a066-19eab0b3d95c
           5  True                  eastus      myScaleSet_5  Creating             MYRESOURCEGROUP  4beff8b9-4e65-40cb-9652-43899309da27
           6  True                  eastus      myScaleSet_6  Creating             MYRESOURCEGROUP  9e4133dd-2c57-490e-ae45-90513ce3b336
```

Nachdem die Ausführung von **stress** auf den ersten VM-Instanzen angehalten wurde, sinkt die CPU-Last wieder auf normale Werte. Nach weiteren fünf Minuten wird über die Regeln für die automatische Skalierung dann das horizontale Herunterskalieren für die Anzahl von VM-Instanzen durchgeführt. Bei Aktionen zum horizontalen Herunterskalieren werden die VM-Instanzen mit den höchsten IDs zuerst entfernt. Wenn eine Skalierungsgruppe Verfügbarkeitsgruppen oder Verfügbarkeitszonen verwendet, werden Vorgänge zum horizontalen Herunterskalieren gleichmäßig auf diese VM-Instanzen verteilt. In der folgenden Beispielausgabe ist zu sehen, dass beim automatischen horizontalen Herunterskalieren der Skalierungsgruppe eine VM-Instanz gelöscht wurde:

```bash
           6  True                  eastus      myScaleSet_6  Deleting             MYRESOURCEGROUP  9e4133dd-2c57-490e-ae45-90513ce3b336
```

Beenden Sie *watch* mit `Ctrl-c`. Für die Skalierungsgruppe wird alle fünf Minuten das horizontale Herunterskalieren durchgeführt und eine VM-Instanz entfernt, bis die Mindestanzahl von zwei Instanzen erreicht ist.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die Ressourcengruppe und alle dazugehörigen Ressourcen mit [az group delete](/cli/azure/group#az_group_delete), um Ihre Skalierungsgruppe und die weiteren Ressourcen zu entfernen. Der Parameter `--no-wait` gibt die Steuerung an die Eingabeaufforderung zurück, ohne zu warten, bis der Vorgang abgeschlossen ist. Der Parameter `--yes` bestätigt ohne eine zusätzliche Aufforderung, dass Sie die Ressourcen löschen möchten.

```azurecli-interactive
az group delete --name $resourcegroup_name --yes --no-wait
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurde beschrieben, wie Sie für eine Skalierungsgruppe mit Azure CLI 2.0 das automatische horizontale Herunter- oder Hochskalieren durchführen:

> [!div class="checklist"]
> * Verwenden der automatischen Skalierung mit einer Skalierungsgruppe
> * Erstellen und Verwenden von Regeln für die automatische Skalierung
> * Durchführen eines Belastungstests für VM-Instanzen und Auslösen von Regeln für die automatische Skalierung
> * Zurückfahren der automatischen Skalierung bei einer Reduzierung des Bedarfs

Weitere Beispiele für VM-Skalierungsgruppen in Aktion finden Sie in den folgenden Azure CLI 2.0-Beispielskripts:

> [!div class="nextstepaction"]
> [Skalierungsgruppen-Skriptbeispiele für Azure CLI 2.0](cli-samples.md)