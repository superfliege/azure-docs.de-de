---
title: Erstellen einer Momentaufnahme einer VHD in Azure | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie eine Kopie einer VHD in Azure als Sicherung oder für die Behandlung von Problemen erstellen."
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/09/2017
ms.author: cynthn
ms.openlocfilehash: da00c48f7da5a9be146f4fdb626c93db746c0f9b
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="create-a-snapshot"></a>Erstellen einer Momentaufnahme 

Erstellen Sie eine Momentaufnahme eines Betriebssystem oder eines VHD-Datenträgers für die Sicherung oder zum Behandeln von VM-Problemen. Eine Momentaufnahme ist eine vollständige, schreibgeschützte Kopie einer virtuellen Festplatte. 

## <a name="use-azure-cli-20-to-take-a-snapshot"></a>Erstellen einer Momentaufnahme mithilfe von Azure CLI 2.0

Für das folgende Beispiel muss Azure CLI 2.0 installiert sein, und Sie müssen bei Ihrem Azure-Konto angemeldet sein. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

Die folgenden Schritte zeigen, wie Sie mithilfe des Befehls `az snapshot create` mit dem `--source-disk`-Parameter eine Momentaufnahme erstellen. Beim folgenden Beispiel wird davon ausgegangen, dass ein virtueller Computer namens `myVM` vorhanden ist, der mit einem verwalteten Betriebssystemdatenträger in der Ressourcengruppe `myResourceGroup` erstellt wurde.

```azure-cli
# take the disk id with which to create a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

Die Ausgabe sollte ungefähr wie folgt aussehen:

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "westus",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

## <a name="use-azure-portal-to-take-a-snapshot"></a>Erstellen einer Momentaufnahme im Azure-Portal 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie zunächst links oben auf **Neu**, und suchen Sie nach **Momentaufnahme**.
3. Klicken Sie auf dem Blatt „Momentaufnahme“ auf **Erstellen**.
4. Geben Sie einen **Namen** für die Momentaufnahme ein.
5. Wählen Sie eine vorhandene [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md#resource-groups) aus, oder geben Sie den Namen für eine neue Ressourcengruppe ein. 
6. Wählen Sie den Standort eines Azure-Rechenzentrums aus.  
7. Wählen Sie für **Quelldatenträger** den verwalteten Datenträger aus, für den eine Momentaufnahme erstellt werden soll.
8. Wählen Sie den **Kontotyp** aus, der zum Speichern der Momentaufnahme verwendet werden soll. Wir empfehlen **Standard_LRS**, es sei denn, Sie benötigen eine Speicherung auf einem Hochleistungsdatenträger.
9. Klicken Sie auf **Erstellen**.

Wenn Sie vorhaben, die Momentaufnahme zum Erstellen eines verwalteten Datenträgers zu nutzen und an eine VM anzufügen, die hohe Leistung benötigt, verwenden Sie den Parameter `--sku Premium_LRS` mit dem Cmdlet `az snapshot create`. Dadurch wird die Momentaufnahme so erstellt, dass sie als verwalteter Premium-Datenträger gespeichert wird. Verwaltete Premium-Datenträger bieten eine bessere Leistung, da es sich um SSDs (Solid-State Drives) handelt. Allerdings sind sie auch teurer als Standard-Datenträger (HDDs).


## <a name="next-steps"></a>Nächste Schritte

 Erstellen Sie einen virtuellen Computer aus einer Momentaufnahme, indem Sie aus der Momentaufnahme einen verwalteten Datenträger erstellen und diesen neuen verwalteten Datenträger anschließend als den Betriebssystemdatenträger anfügen. Weitere Informationen finden Sie im Skript [Erstellen eines virtuellen Computers aus einer Momentaufnahme](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json).

