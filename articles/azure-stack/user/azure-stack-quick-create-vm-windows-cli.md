---
title: Erstellen eines virtuellen Windows-Computers in Azure Stack mithilfe der Azure CLI | Microsoft-Dokumentation
description: Informationen zum Erstellen einer Windows-VM in Azure Stack mit der Azure CLI
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: E26B246E-811D-44C9-9BA6-2B3CE5B62E83
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/19/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 5665af14b9b0d0705b68c8a27c593b19c31b053e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="quickstart-create-a-windows-virtual-machine-in-azure-stack-using-azure-cli"></a>Schnellstart: Erstellen eines virtuellen Windows-Computers in Azure Stack mithilfe der Azure CLI

Die Azure CLI dient zum Erstellen und Verwalten von Azure Stack-Ressourcen über die Befehlszeile. In dieser Anleitung erfahren Sie, wie Sie mithilfe der Azure CLI einen virtuellen Windows Server 2016-Computer in Azure Stack erstellen und darauf zugreifen.

## <a name="prerequisites"></a>Voraussetzungen

* Stellen Sie sicher, dass Ihr Azure Stack-Bediener das Image „Windows Server 2016“ dem Azure Stack-Marketplace hinzugefügt hat.

* Azure Stack erfordert eine spezifische Version der Azure CLI, um die Ressourcen zu erstellen und zu verwalten. Wenn Sie die Azure CLI nicht für Azure Stack konfiguriert haben, führen Sie die Schritte zum [Installieren und Konfigurieren der Azure CLI](azure-stack-version-profiles-azurecli2.md) aus.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Ressourcengruppe ist ein logischer Container, in dem Azure Stack-Ressourcen bereitgestellt und verwaltet werden. Führen Sie im Development Kit oder im integrierten Azure Stack-System den Befehl [az group create](/cli/azure/group#az_group_create) aus, um eine Ressourcengruppe zu erstellen. Für alle Variablen in diesem Dokument werden Werte zugewiesen. Sie können entweder diese Werte verwenden oder neue Werte zuweisen. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen myResourceGroup im lokalen Speicherort erstellt.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie mit dem Befehl [az vm create](/cli/azure/vm#az_vm_create) eine VM. Im folgenden Beispiel wird ein virtueller Computer namens „myVM“ erstellt. Dieses Beispiel verwendet „Demouser“ als Administratorbenutzernamen und Demouser@123 als Kennwort. Aktualisieren Sie diese Werte auf andere, für Ihre Umgebung geeignete Werte. Diese Werte werden benötigt, wenn eine Verbindung mit dem virtuellen Computer hergestellt wird.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "Win2016Datacenter" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --use-unmanaged-disk \
  --location local
```

Bei der Erstellung des virtuellen Computers wird der Parameter *PublicIPAddress* ausgegeben. Notieren Sie sich diese Adresse. Sie wird für den Zugriff auf den virtuellen Computer benötigt.

## <a name="open-port-80-for-web-traffic"></a>Öffnen von Port 80 für Webdatenverkehr

Standardmäßig sind für in Azure Stack bereitgestellte virtuelle Windows-Computer nur RDP-Verbindungen zulässig. Wenn dieser virtuelle Computer als Webserver fungieren soll, muss Port 80 für Datenverkehr aus dem Internet geöffnet werden. Verwenden Sie zum Öffnen des gewünschten Ports den Befehl [az vm open-port](/cli/azure/vm#open-port).

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung mit dem virtuellen Computer

Erstellen Sie mit dem folgenden Befehl eine Remotedesktopsitzung mit dem virtuellen Computer. Ersetzen Sie die IP-Adresse mit der öffentlichen IP-Adresse des virtuellen Computers. Geben Sie nach Aufforderung die beim Erstellen des virtuellen Computers verwendeten Anmeldeinformationen ein.

```
mstsc /v <Public IP Address>
```

## <a name="install-iis-using-powershell"></a>Installieren von IIS mithilfe von PowerShell

Nach der Anmeldung bei dem virtuellen Computer können Sie mit einer einzelnen PowerShell-Codezeile IIS installieren und die lokale Firewallregel aktivieren, um Webdatenverkehr zuzulassen. Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie den folgenden Befehl aus:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>Anzeigen der IIS-Willkommensseite

Die Standardwillkommensseite von IIS kann in einem beliebigen Webbrowser angezeigt werden. Verwenden Sie die öffentliche IP-Adresse, die Sie sich im vorherigen Abschnitt notiert haben, um die Standardseite zu besuchen.

![IIS-Standardwebsite](./media/azure-stack-quick-create-vm-windows-cli/default-iis-website.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen nicht mehr benötigen, führen Sie den Befehl [az group delete](/cli/azure/group#az_group_delete) aus, um die Ressourcengruppe, den virtuellen Computer und alle dazugehörigen Ressourcen zu löschen.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen einfachen virtuellen Windows-Computer bereitgestellt. Um weitere Informationen zu virtuellen Computern unter Azure Stack zu erhalten, fahren Sie mit [Considerations for Virtual Machines in Azure Stack](azure-stack-vm-considerations.md) (Überlegungen zu virtuellen Computern in Azure Stack) fort.
