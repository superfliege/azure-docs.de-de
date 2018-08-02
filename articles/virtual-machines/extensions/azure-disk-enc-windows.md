---
title: Azure Disk Encryption für Windows | Microsoft-Dokumentation
description: Stellen Sie Azure Disk Encryption mithilfe einer VM-Erweiterung auf einem virtuellen Windows-Computer bereit.
services: virtual-machines-windows
documentationcenter: ''
author: ejarvi
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: ejarvi
ms.openlocfilehash: 11ec26729b2239279dddc8cd62f6b658a4f7ed20
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413789"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>Azure Disk Encryption für Windows (Microsoft.Azure.Security.AzureDiskEncryption)

## <a name="overview"></a>Übersicht

Azure Disk Encryption nutzt BitLocker, um auf virtuellen Azure-Computern unter Windows eine vollständige Datenträgerverschlüsselung bereitzustellen.  Diese Lösung ist in Azure Key Vault integriert, um die Verschlüsselungsschlüssel und Geheimnisse für die Datenträgerverschlüsselung in Ihrem Key Vault-Abonnement zu verwalten. 

## <a name="prerequisites"></a>Voraussetzungen

Eine vollständige Liste der Voraussetzungen finden Sie unter [Voraussetzungen für Azure Disk Encryption](
../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="operating-system"></a>Betriebssystem

Eine Liste der derzeit unterstützten Windows-Versionen finden Sie unter [Voraussetzungen für Azure Disk Encryption](../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="internet-connectivity"></a>Internetkonnektivität

Für Azure Disk Encryption ist eine Internetverbindung für den Zugriff auf Active Directory, Key Vault, Storage und Paketverwaltungs-Endpunkte erforderlich.  Weitere Informationen zu Netzwerksicherheitseinstellungen finden Sie unter [Voraussetzungen für Azure Disk Encryption](
../../security/azure-security-disk-encryption-prerequisites.md).

## <a name="extension-schema"></a>Erweiterungsschema

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]",
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryption",
    "typeHandlerVersion": "[extensionVersion]"
  }
}
```

### <a name="property-values"></a>Eigenschaftswerte

| NAME | Wert/Beispiel | Datentyp |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| Herausgeber | Microsoft.Azure.Security | Zeichenfolge |
| type | AzureDiskEncryptionForWindows| Zeichenfolge |
| typeHandlerVersion | 1.0, 2.2 (VMSS) | int |
| (Optional:) AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| (Optional:) AADClientSecret | password | Zeichenfolge |
| (Optional:) AADClientCertificate | thumbprint | Zeichenfolge |
| EncryptionOperation | EnableEncryption | Zeichenfolge | 
| KeyEncryptionAlgorithm | RSA-OAEP | Zeichenfolge |
| KeyEncryptionKeyURL | URL | Zeichenfolge |
| KeyVaultURL | URL | Zeichenfolge |
| SequenceVersion | uniqueidentifier | Zeichenfolge |
| VolumeType | Betriebssystem, Daten, alle | Zeichenfolge |

## <a name="template-deployment"></a>Bereitstellung von Vorlagen
Ein Beispiel für eine Vorlagenbereitstellung finden Sie unter [Create a new encrypted Windows VM from gallery image](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image) (Erstellen einer neuen verschlüsselten Windows-VM aus einem Katalogimage).

## <a name="azure-cli-deployment"></a>Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle

Anweisungen finden Sie in der aktuellen [Dokumentation der Azure-Befehlszeilenschnittstelle](/cli/azure/vm/encryption?view=azure-cli-latest). 

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Lesen Sie den [Leitfaden zur Azure Disk Encryption-Problembehandlung](../../security/azure-security-disk-encryption-tsg.md).

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/community/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Erweiterungen finden Sie unter [Erweiterungen und Features für virtuelle Computer für Windows](features-windows.md).