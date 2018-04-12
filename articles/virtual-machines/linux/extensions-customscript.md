---
title: Ausführen von benutzerdefinierten Skripts auf Linux-VMs in Azure | Microsoft-Dokumentation
description: Automatisieren Sie Konfigurationsaufgaben für virtuelle Linux-Computer mithilfe der benutzerdefinierten Skripterweiterung.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: danis
ms.openlocfilehash: 88f337df14b7e2647a76cce7ef91ec6f8950bbc6
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Verwenden der Azure-Erweiterung für benutzerdefinierte Skripts mit virtuellen Linux-Computern
Die Erweiterung für benutzerdefinierte Skripts lädt Skripts auf virtuelle Azure-Computer herunter und führt sie aus. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Sie können Skripts von Azure Storage oder einem anderen zugänglichen Speicherort im Internet herunterladen oder sie für die Erweiterungsruntime bereitstellen. 

Die Erweiterung für benutzerdefinierte Skripts ist mit Azure Resource Manager-Vorlagen integriert. Sie können sie auch mithilfe der von PowerShell, der Azure-Befehlszeilenschnittstelle, dem Azure-Portal oder der Azure Virtual Machines-REST-API ausführen.

In diesem Artikel wird erläutert, wie Sie die Erweiterung für benutzerdefinierte Skripts über die Azure-Befehlszeilenschnittstelle verwenden und die Erweiterung mithilfe einer Azure Resource Manager-Vorlage ausführen. Außerdem enthält dieser Artikel Schritte zur Problembehandlung für Linux-Systeme.

## <a name="extension-configuration"></a>Konfiguration der Erweiterung
In der Konfiguration der benutzerdefinierten Skripterweiterung werden Aspekte wie der Skriptspeicherort und der auszuführende Befehl angegeben. Sie können diese Konfiguration in Konfigurationsdateien speichern oder sie in der Befehlszeile oder in einer Azure Resource Manager-Vorlage angeben. 

Sie können sensible Daten in einer geschützten Konfiguration speichern. Diese ist verschlüsselt und wird nur auf dem virtuellen Computer entschlüsselt. Die geschützte Konfiguration ist hilfreich, wenn der Ausführungsbefehl vertrauliche Informationen (beispielsweise ein Kennwort) enthält.

### <a name="public-configuration"></a>Öffentliche Konfiguration
Das Schema für die öffentliche Konfiguration lautet wie folgt.

>[!NOTE]
>Bei Eigenschaftennamen wird zwischen Groß- und Kleinschreibung unterschieden. Um Bereitstellungsprobleme zu vermeiden, verwenden Sie die Namen wie hier gezeigt.

* **commandToExecute** (erforderlich, Zeichenfolge): das auszuführende Skript für den Einstiegspunkt.
* **fileUris** (optional, Zeichenfolgenarray): die URLs für die herunterzuladenden Dateien.
* **Zeitstempel** (optional, Integer): der Zeitstempel des Skripts. Ändern Sie den Wert dieses Felds nur, wenn Sie eine erneute Ausführung des Skripts auslösen möchten.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Geschützte Konfiguration
Das Schema für die geschützte Konfiguration lautet wie folgt.

>[!NOTE]
>Bei Eigenschaftennamen wird zwischen Groß- und Kleinschreibung unterschieden. Um Bereitstellungsprobleme zu vermeiden, verwenden Sie die Namen wie hier gezeigt.

* **commandToExecute** (optional, Zeichenfolge): das auszuführende Einstiegspunktskript. Verwenden Sie dieses Feld, wenn Ihr Befehl Geheimnisse enthält, z.B. Kennwörter.
* **storageAccountName** (optional, Zeichenfolge): der Name des Speicherkontos. Wenn Sie Speicheranmeldeinformationen angeben, muss es sich bei allen Datei-URIs um URLs für Azure-Blobs handeln.
* **storageAccountKey** (optional, Zeichenfolge): der Zugriffsschlüssel des Speicherkontos.

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Wenn Sie die Azure-Befehlszeilenschnittstelle zum Ausführen der Erweiterung für benutzerdefinierte Skripts verwenden, erstellen Sie eine Konfigurationsdatei oder mehrere Konfigurationsdateien. Konfigurationsdateien enthalten mindestens den Datei-URI und den Befehl für die Skriptausführung.

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

Optional können Sie die Einstellungen im Befehl als JSON-formatierte Zeichenfolge angeben. Dadurch kann die Konfiguration während der Ausführung und ohne separate Konfigurationsdatei angegeben werden.

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Beispiele für die Azure-Befehlszeilenschnittstelle

#### <a name="public-configuration-with-script-file"></a>Öffentliche Konfiguration mit Skriptdatei

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],
  "commandToExecute": "./config-music.sh"
}
```

Befehl für die Azure-Befehlszeilenschnittstelle:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

#### <a name="public-configuration-with-no-script-file"></a>Öffentliche Konfiguration ohne Skriptdatei

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Befehl für die Azure-Befehlszeilenschnittstelle:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

#### <a name="public-and-protected-configuration-files"></a>Dateien für die öffentliche und geschützte Konfiguration

Dateien für die öffentliche Konfiguration werden zum Angeben des Skriptdatei-URI verwendet. Dateien für die geschützte Konfiguration werden zum Angeben des auszuführenden Befehls verwendet.

Datei für die öffentliche Konfiguration:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

Datei für die geschützte Konfiguration:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Befehl für die Azure-Befehlszeilenschnittstelle:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a>Resource Manager-Vorlage
Sie können die Erweiterung für benutzerdefinierte Skripts von Azure beim Bereitstellen virtueller Computer mithilfe einer Resource Manager-Vorlage ausführen. Zu diesem Zweck muss der Bereitstellungsvorlage ordnungsgemäß formatierter JSON-Code hinzugefügt werden.

### <a name="resource-manager-examples"></a>Resource Manager-Beispiele

#### <a name="public-configuration"></a>Öffentliche Konfiguration

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

#### <a name="execution-command-in-protected-configuration"></a>Ausführungsbefehl in der geschützten Konfiguration

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Ein vollständiges Beispiel finden Sie in der [.NET-Demo eines Musikgeschäfts](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="troubleshooting"></a>Problembehandlung
Beim Ausführen der Erweiterung für benutzerdefinierte Skripts wird das Skript erstellt oder wie im folgenden Beispiel in ein Verzeichnis heruntergeladen. Die Ausgabe des Befehls wird ebenfalls in diesem Verzeichnis gespeichert (in den Dateien `stdout` und `stderr`).

```bash
/var/lib/waagent/custom-script/download/0/
```

Das von der Azure-Skripterweiterung erzeugte Protokoll finden Sie hier:

```bash
/var/log/azure/custom-script/handler.log
```

Sie können den Ausführungsstatus der Erweiterung für benutzerdefinierte Skripts auch über die Azure-Befehlszeilenschnittstelle abrufen:

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

Die Ausgabe sieht in etwa wie folgt aus:

```azurecli
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Nächste Schritte
Informationen zu anderen Skripterweiterungen für virtuelle Computer finden Sie unter [Übersicht über die Azure-Skripterweiterung für Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

