---
title: "Azure CLI-Skriptbeispiel: Erstellen eines virtuellen Computers durch Anfügen eines verwalteten Datenträgers als Betriebssystemdatenträger | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Erstellen eines virtuellen Computers durch Anfügen eines verwalteten Datenträgers als Betriebssystemdatenträger"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 2141ea4fd25dfc69ada02c54c4f6b6b717b8e7db
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a>Erstellen eines virtuellen Computers mit einem vorhandenen verwalteten Betriebssystemdatenträger über die Befehlszeilenschnittstelle

Dieses Skript erstellt einen virtuellen Computer durch Anfügen eines vorhandenen verwalteten Datenträgers als Betriebssystemdatenträger. Verwenden Sie dieses Skript in den vorherigen Szenarien:
* Erstellen eines virtuellen Computers aus einem vorhandenen verwalteten Betriebssystemdatenträger, der von einem verwalteten Datenträger in einem anderen Abonnement kopiert wurde
* Erstellen eines virtuellen Computers aus einem vorhandenen verwalteten Datenträger, der aus einer spezialisierten VHD-Datei erstellt wurde 
* Erstellen eines virtuellen Computers aus einem vorhandenen verwalteten Betriebssystemdatenträger, der aus einer Momentaufnahme erstellt wurde 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Create VM from a managed disk")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um die Eigenschaften eines verwalteten Datenträgers abzurufen, einen verwalteten Datenträger an einen neuen virtuellen Computer anzufügen und einen virtuellen Computer zu erstellen. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az disk show](https://docs.microsoft.com/cli/azure/disk#az_disk_show) | Ruft die Eigenschaften eines verwalteten Datenträgers unter Verwendung des Datenträger- und Ressourcengruppennamens ab. Die ID-Eigenschaft wird verwendet, um einen verwalteten Datenträger an einen neuen virtuellen Computer anzufügen. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Erstellt einen virtuellen Computer unter Verwendung eines verwalteten Betriebssystemdatenträgers. |
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
