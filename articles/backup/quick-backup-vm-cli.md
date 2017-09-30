---
title: "Azure-Schnellstart – Sichern einer VM per Azure CLI | Microsoft-Dokumentation"
description: Es wird beschrieben, wie Sie Ihre virtuellen Computer mit der Azure CLI sichern.
services: virtual-machines-linux, azure-backup
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: virtual-machines-linux, azure-backup
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/18/2017
ms.author: iainfou
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: b01916516d15ffee46f135e175ee4136f79acbe5
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="back-up-a-virtual-machine-in-azure-with-the-cli"></a>Sichern eines virtuellen Computers in Azure per CLI
Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. Sie können Ihre Daten schützen, indem Sie in regelmäßigen Abständen Sicherungen erstellen. Azure Backup erstellt Wiederherstellungspunkte, die in georedundanten Recovery-Tresoren gespeichert werden können. In diesem Artikel wird ausführlich beschrieben, wie Sie einen virtuellen Computer (VM) in Azure mit der Azure CLI sichern. Sie können diese Schritte auch mit [Azure PowerShell](quick-backup-vm-powershell.md) oder im [Azure-Portal](quick-backup-vm-portal.md) ausführen.

Dieser Schnellstart ermöglicht die Sicherung auf einer vorhandenen Azure-VM. Wenn Sie eine VM erstellen müssen, können Sie die [Erstellung mit der Azure CLI durchführen](../virtual-machines/linux/quick-create-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.18 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 


## <a name="register-the-azure-backup-resource-provider"></a>Registrieren des Azure Backup-Ressourcenanbieters
Bei der ersten Verwendung von Azure Backup müssen Sie den Azure Recovery Service-Anbieter in Ihrem Abonnement mit [az provider register](/cli/azure/provider?view=azure-cli-latest#az_provider_register) registrieren.

```azurecli-interactive
az provider register --namespace Microsoft.RecoveryServices
```


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors
Ein Recovery Services-Tresor ist ein logischer Container, in dem die Sicherungsdaten für jede geschützte Ressource, z.B. Azure-VMs, gespeichert werden. Wenn der Sicherungsauftrag für eine geschützte Ressource ausgeführt wird, wird im Recovery Services-Tresor ein Wiederherstellungspunkt erstellt. Sie können einen dieser Wiederherstellungspunkte dann verwenden, um Daten für einen bestimmten Zeitpunkt wiederherzustellen.

Erstellen Sie einen Recovery Services-Tresor mit **az backup vault create**. Geben Sie dieselbe Ressourcengruppe und denselben Standort wie für die zu schützende VM an. Wenn Sie den [VM-Schnellstart](../virtual-machines/linux/quick-create-cli.md) verwendet haben, hat die Ressourcengruppe den Namen *myResourceGroup*, die VM heißt *myVM* und die Ressourcen befinden sich am Standort *eastus*.

```azurecli-interactive 
az backup vault create --resource-group myResourceGroup \
    --name myRecoveryServicesVault \
    --location eastus
```

Für den Tresor ist standardmäßig die georedundante Speicherung festgelegt. Als weiterer Schutz für Ihre Daten wird auf dieser Speicherredundanzebene sichergestellt, dass Ihre Sicherungsdaten in einer sekundären Azure-Region repliziert werden, die Hunderte Kilometer von der primären Region entfernt ist.


## <a name="enable-backup-for-an-azure-vm"></a>Aktivieren der Sicherung für eine Azure-VM
Sie erstellen und verwenden Richtlinien, um zu definieren, wann ein Sicherungsauftrag ausgeführt wird und wie lange die Wiederherstellungspunkte gespeichert werden. Bei der Standardschutzrichtlinie wird jeden Tag ein Sicherungsauftrag ausgeführt, und Wiederherstellungspunkte werden 30 Tage lang beibehalten. Sie können diese Standardrichtlinienwerte verwenden, um Ihre VM schnell zu schützen. Verwenden Sie **az backup protection enable-for-vm**, um den Sicherungsschutz für eine VM zu aktivieren. Geben Sie die Ressourcengruppe und die zu schützende VM und anschließend die Richtlinie an, die verwendet werden soll:

```azurecli-interactive 
az backup protection enable-for-vm \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --vm myVM \
    --policy-name DefaultPolicy
```


## <a name="start-a-backup-job"></a>Starten eines Sicherungsauftrags
Verwenden Sie **az backup protection backup-now**, um sofort einen Sicherungsvorgang zu starten, anstatt zu warten, bis die Standardrichtlinie den Auftrag zum geplanten Zeitpunkt ausführt. Bei diesem ersten Sicherungsauftrag wird ein vollständiger Wiederherstellungspunkt erstellt. Bei jedem Sicherungsauftrag nach diesem ersten Sicherungsvorgang werden dann inkrementelle Wiederherstellungspunkte erstellt. Inkrementelle Wiederherstellungspunkte sind in Bezug auf die Speicherung und die Dauer effizient, da nur Änderungen übertragen werden, die seit der letzten Sicherung vorgenommen wurden.

Die folgenden Parameter werden verwendet, um die VM zu sichern:

- `--container-name` ist der Name Ihrer VM.
- `--item-name` ist der Name Ihrer VM.
- Der Wert `--retain-until` sollte auf das letzte mögliche Datum festgelegt werden (im UTC-Zeitformat **tt-mm-jjjj**), bis zu dem der Wiederherstellungspunkt verfügbar sein soll.

Im folgenden Beispiel wird die VM mit dem Namen *myVM* gesichert und der Ablauf des Wiederherstellungspunkts auf den 18. Oktober 2017 festgelegt:

```azurecli-interactive 
az backup protection backup-now \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --retain-until 18-10-2017
```

Da bei diesem ersten Sicherungsauftrag ein vollständiger Wiederherstellungspunkt erstellt wird, kann der Vorgang bis zu 20 Minuten dauern.


## <a name="monitor-the-backup-job"></a>Überwachen des Sicherungsauftrags
Verwenden Sie **az backup job list**, um den Status von Sicherungsaufträgen zu überwachen:

```azurecli-interactive 
az backup job list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --output table
```

Die Ausgabe ähnelt dem folgenden Beispiel, bei dem für den Sicherungsauftrag der Status *InProgress* angezeigt wird:

```
Name      Operation        Status      Item Name    Start Time UTC       Duration
--------  ---------------  ----------  -----------  -------------------  --------------
a0a8e5e6  Backup           InProgress  myvm         2017-09-19T03:09:21  0:00:48.718366
fe5d0414  ConfigureBackup  Completed   myvm         2017-09-19T03:03:57  0:00:31.191807
```

Wenn der *Status* des Sicherungsauftrags als *Completed* gemeldet wird, ist Ihre VM durch Recovery Services geschützt und verfügt über einen gespeicherten vollständigen Wiederherstellungspunkt.


## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Wenn die Komponenten nicht mehr benötigt werden, können Sie den Schutz auf der VM deaktivieren, die Wiederherstellungspunkte und den Recovery Services-Tresor entfernen und anschließend die Ressourcengruppe und die dazugehörigen VM-Ressourcen löschen. Falls Sie eine vorhandene VM verwendet haben, können Sie den abschließenden Befehl [az group delete](/cli/azure/group?view=azure-cli-latest#az_group_delete) überspringen, damit die Ressourcengruppe und die VM erhalten bleiben.

Wenn Sie mit einem Backup-Tutorial fortfahren möchten, in dem die Wiederherstellung von Daten für Ihre VM beschrieben wird, können Sie die Schritte in diesem Abschnitt überspringen und mit [Nächste Schritte](#next-steps) fortfahren. 

```azurecli-interactive 
az backup protection disable \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --delete-backup-data true
az backup vault delete \
    --resource-group myResourceGroup \
    --name myRecoveryServicesVault \
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie einen Recovery Services-Tresor erstellt, den Schutz auf einer VM aktiviert und den ersten Wiederherstellungspunkt erstellt. Weitere Informationen zu Azure Backup und Recovery Services sind in den Tutorials enthalten.

> [!div class="nextstepaction"]
> [Sichern von mehreren Azure-VMs](./tutorial-backup-vm-at-scale.md)

