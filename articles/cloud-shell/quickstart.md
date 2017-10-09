---
title: "Schnellstart für Bash in Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation"
description: "Schnellstart für Bash in Cloud Shell"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: juluk
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: bbad2735acf06ebefda020410731fb307c4ec614
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Schnellstart für Bash in Azure Cloud Shell

Dieses Dokument erläutert die Verwendung von Bash in Azure Cloud Shell im [Azure-Portal](https://ms.portal.azure.com/).

> [!NOTE]
> Ein Schnellstart zu [PowerShell in Azure Cloud Shell](quickstart-powershell.md) ist ebenfalls verfügbar.

## <a name="start-cloud-shell"></a>Starten von Cloud Shell
1. Starten Sie **Cloud Shell** aus dem oberen Navigationsbereich im Azure-Portal. <br>
![](media/quickstart/shell-icon.png)
2. Wählen Sie ein Abonnement aus, in dem ein Speicherkonto und eine Azure-Dateifreigabe erstellt werden sollen.
3. Wählen Sie „Speicher erstellen“ aus.

> [!TIP]
> Sie werden in jeder Sitzung automatisch bei der Azure CLI 2.0 authentifiziert.

### <a name="select-the-bash-environment"></a>Auswählen der Bash-Umgebung
1. Wählen Sie auf der linken Seite des Shell-Fensters im Dropdownmenü die Umgebung aus. <br>
![](media/quickstart/env-selector.png)
2. Wählen Sie „Bash“ aus.

### <a name="set-your-subscription"></a>Festlegen des Abonnements
1. Führen Sie die Abonnements auf, auf die Sie Zugriff haben: <br>
`az account list`
2. Legen Sie Ihr bevorzugtes Abonnement fest: <br>
`az account set --subscription my-subscription-name`

> [!TIP]
> Ihr Abonnement wird mithilfe von `/home/<user>/.azure/azureProfile.json` für künftige Sitzungen gespeichert.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie eine neue Ressourcengruppe in „USA, Westen“ namens „MyRG“: <br>
`az group create -l westus -n MyRG` <br>

### <a name="create-a-linux-vm"></a>Erstellen eines virtuellen Linux-Computers
Erstellen Sie einen virtuellen Ubuntu-Computer in Ihrer neuen Ressourcengruppe. Die Azure CLI 2.0 erstellt SSH-Schlüssel und richtet den virtuellen Computer damit ein. <br>
`az vm create -n my_vm_name -g MyRG --image UbuntuLTS --generate-ssh-keys`

> [!NOTE]
> Die öffentlichen und privaten Schlüssel, mit denen Ihr virtueller Computer authentifiziert wird, werden von der Azure CLI 2.0 standardmäßig in `/User/.ssh/id_rsa` und `/User/.ssh/id_rsa.pub` gespeichert. Der SSH-Ordner wird im 5 GB großen Image der angefügten Azure-Dateifreigabe beibehalten.

Ihr Benutzername auf diesem virtuellen Computer ist der Benutzername, der in Cloud Shell verwendet wird ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>Herstellen einer SSH-Verbindung mit Ihrem virtuellen Linux-Computer
1. Suchen Sie über die Suchleiste des Azure-Portals nach dem Namen Ihres virtuellen Computers.
2. Klicken Sie auf „Verbinden“ und führen Sie `ssh username@ipaddress` aus.

![](media/quickstart/sshcmd-copy.png)

Nach dem Herstellen der SSH-Verbindung sollte der Willkommensbildschirm von Ubuntu angezeigt werden. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Bereinigen 
Löschen Sie Ihre Ressourcengruppe und alle darin befindlichen Ressourcen: <br>
Führen Sie `az group delete -n MyRG` aus.

## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie mehr über das Beibehalten von Dateien für Bash in Cloud Shell](persisting-shell-storage.md) <br>
[Erfahren Sie mehr über Azure CLI 2.0](https://docs.microsoft.com/cli/azure/). <br>
[Erfahren Sie mehr über Azure File Storage](../storage/files/storage-files-introduction.md). <br>
