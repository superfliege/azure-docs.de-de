---
title: Bereitstellen eines virtuellen Linux-Computers mit der Azure-REST-API | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen virtuellen Linux-Computer in Azure erstellen, der verwaltete Datenträger und SSH-Authentifizierung mit der Azure-REST-API verwendet.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2018
ms.author: cynthn
ms.openlocfilehash: 0f77b46be0207b0ce96e6dc2562fb5298afbe36b
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928181"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>Erstellen eines virtuellen Linux-Computers zur Verwendung der SSH-Authentifizierung mit der REST-API

Ein virtueller Computer (VM) in Azure wird durch verschiedene Parameter wie Speicherort, Größe der Hardware, Betriebssystemimage und Anmeldeinformationen definiert. In diesem Artikel erfahren Sie, wie Sie mithilfe der REST-API einen virtuellen Linux-Computer erstellen, der SSH-Authentifizierung verwendet.

Verwenden Sie zum Erstellen oder Aktualisieren eines virtuellen Computers den folgenden *PUT*-Vorgang:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

## <a name="create-a-request"></a>Erstellen einer Anforderung

Zum Erstellen der *PUT*-Anforderung ist der `{subscription-id}`-Parameter erforderlich. Wenn Sie über mehrere Abonnements verfügen, lesen Sie [Verwenden mehrerer Abonnements](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions): Sie definieren zusammen mit dem `api-version`-Parameter einen `{resourceGroupName}` und `{vmName}` für Ihre Ressourcen. In diesem Artikel wird `api-version=2017-12-01` verwendet.

Die folgenden Header sind erforderlich:

| Anforderungsheader   | BESCHREIBUNG |
|------------------|-----------------|
| *Inhaltstyp*:  | Erforderlich. Legen Sie diese Option auf `application/json` fest. |
| *Autorisierung*: | Erforderlich. Legen Sie diese Option auf ein gültiges `Bearer` [Zugriffstoken](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients) fest. |

Weitere Informationen zum Erstellen der Anforderung finden Sie unter [Komponenten einer REST-API-Anforderung/Antwort](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>Erstellen des Anforderungstexts

Die folgenden allgemeinen Definitionen werden verwendet, um einen Anforderungstext zu erstellen:

| NAME                       | Erforderlich | Typ                                                                                | BESCHREIBUNG  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| location                   | True     | Zeichenfolge                                                                              | Ressourcenspeicherort |
| name                       |          | Zeichenfolge                                                                              | Name des virtuellen Computers |
| properties.hardwareProfile |          | [HardwareProfile](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Legt die Hardwareeinstellungen für den virtuellen Computer fest. |
| properties.storageProfile  |          | [StorageProfile](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Legt die Speichereinstellungen für die Datenträger des virtuellen Computers fest. |
| properties.osProfile       |          | [OSProfile](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Legt die Einstellungen des Betriebssystems für den virtuellen Computer fest. |
| properties.networkProfile  |          | [NetworkProfile](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Legt die Netzwerkschnittstellen des virtuellen Computers fest. |

Eine vollständige Liste der verfügbaren Definitionen im Anforderungstext finden Sie in den [Definitionen zum Erstellen oder Aktualisieren eines Anforderungstexts für virtuelle Computer](/rest/api/compute/virtualmachines/createorupdate#definitions).

### <a name="example-request-body"></a>Beispiel für Anforderungstext

Der folgende Beispielanforderungstext definiert ein Ubuntu 18.04-LTS-Image, das verwaltete Premium-Datenträger verwendet. Authentifizierung mit öffentlichem SSH-Schlüssel wird verwendet, und die VM nutzt eine vorhandene virtuelle Netzwerkschnittstelle, die Sie [zuvor erstellt](../../virtual-network/virtual-network-network-interface.md) haben. Geben Sie Ihren öffentlichen SSH-Schlüssel in das Feld *osProfile.linuxConfiguration.ssh.publicKeys.keyData* ein. Bei Bedarf können Sie ein [SSH-Schlüsselpaar generieren](mac-create-ssh-keys.md).

```json
{
  "location": "eastus",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_DS1_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "18.04-LTS",
        "publisher": "Canonical",
        "version": "latest",
        "offer": "UbuntuServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      }
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "myVM",
      "linuxConfiguration": {
        "ssh": {
          "publicKeys": [
            {
              "path": "/home/{your-username}/.ssh/authorized_keys",
              "keyData": "ssh-rsa AAAAB3NzaC1{snip}mf69/J1"
            }
          ]
        },
        "disablePasswordAuthentication": true
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/{existing-nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    }
  },
  "name": "myVM"
}
```

## <a name="responses"></a>Antworten

Es gibt zwei erfolgreiche Antworten für den Vorgang, um einen virtuellen Computer zu erstellen oder zu aktualisieren:

| NAME        | Typ                                                                              | BESCHREIBUNG |
|-------------|-----------------------------------------------------------------------------------|-------------|
| 200 – OK      | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | OK          |
| 201 – Erstellt | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Erstellt     |

Weitere Informationen zu REST-API-Antworten finden Sie unter [Verarbeiten der Antwortnachricht](/rest/api/azure/#process-the-response-message).

### <a name="example-response"></a>Beispielantwort

Eine komprimierte *201 – Erstellt*-Antwort aus dem vorherigen Beispielanforderungstext, der eine VM erstellt, zeigt, dass eine *vmId* zugewiesen wurde und *ProvisioningState* *Wird erstellt* ist:

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Azure-REST-APIs oder anderen Verwaltungstools, z.B. Azure CLI 2.0 oder Azure PowerShell, finden Sie hier:

- [Azure Compute-Anbieter-REST-API](/rest/api/compute/)
- [Erste Schritte mit der Azure-REST-API](/rest/api/azure/)
- [Azure CLI 2.0](/cli/azure/)
- [Azure PowerShell-Modul](/powershell/azure/overview)
