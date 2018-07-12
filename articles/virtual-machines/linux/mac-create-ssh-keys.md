---
title: Erstellen und Verwenden eines SSH-Schlüsselpaars für virtuelle Linux-Computer in Azure| Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie ein SSH-Schlüsselpaar (öffentlicher und privater Schlüssel) für virtuelle Linux-Computer in Azure erstellen und verwenden, um die Sicherheit des Authentifizierungsprozesses zu verbessern.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: cynthn
ms.openlocfilehash: 2e3d86d776f44c47a33bf075cf7f2140a3940e5e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928453"
---
# <a name="quick-steps-create-and-use-an-ssh-public-private-key-pair-for-linux-vms-in-azure"></a>Schnelle Schritte: Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure
Mit einem SSH-Schlüsselpaar (Secure Shell) können Sie virtuelle Computer (VMs) in Azure erstellen, bei deren Authentifizierung SSH-Schlüssel verwendet werden, sodass zum Anmelden keine Kennwörter mehr erforderlich sind. In diesem Artikel erfahren Sie, wie Sie für virtuelle Linux-Computer schnell eine Datei mit einem SSH-Schlüsselpaar (ein öffentlicher und ein privater Schlüssel) generieren. Sie können diese Schritte mit Azure Cloud Shell, einem macOS- oder Linux-Host, dem Windows-Subsystem für Linux und anderen Tools ausführen, die OpenSSH unterstützen. 

Weitere Hintergrundinformationen und Beispiele finden Sie unter [Ausführliche exemplarische Vorgehensweise zum Erstellen eines SSH-Schlüsselpaars mit zusätzlichen Zertifikaten für einen virtuellen Linux-Computer in Azure](create-ssh-keys-detailed.md).

Weitere Möglichkeiten zum Generieren und Verwenden von SSH-Schlüsseln auf einem Windows-Computer finden Sie unter [Gewusst wie: Verwenden von SSH-Schlüsseln mit Windows in Azure](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="create-an-ssh-key-pair"></a>Erstellen eines SSH-Schlüsselpaars
Verwenden Sie den `ssh-keygen`-Befehl, um Dateien für öffentliche und private SSH-Schlüssel zu generieren, die standardmäßig im Verzeichnis `~/.ssh` erstellt werden. Sie können einen anderen Speicherort und eine zusätzliche Passphrase (ein Kennwort zum Zugriff auf die Datei des privaten Schlüssels) angeben, wenn Sie dazu aufgefordert werden. Wenn am aktuellen Speicherort bereits ein SSH-Schlüsselpaar vorhanden ist, werden diese Dateien überschrieben.

```bash
ssh-keygen -t rsa -b 2048
```

Bei Verwendung von [Azure CLI 2.0](/cli/azure) zum Erstellen Ihres virtuellen Computers können Sie optional durch Ausführen des [az vm create](/cli/azure/vm#az_vm_create)-Befehls mit der `--generate-ssh-keys`-Option Dateien für öffentliche und private SSH-Schlüssel generieren. Die Schlüssel werden im Verzeichnis „~/.ssh“ gespeichert. Beachten Sie, dass diese Befehlsoption keine Schlüssel überschreibt, wenn sie bereits an diesem Speicherort vorhanden sind.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Bereitstellen des öffentlichen SSH-Schlüssels beim Bereitstellen eines virtuellen Computers
Um eine Linux-VM zu erstellen, die SSH-Schlüssel zur Authentifizierung verwendet, geben Sie Ihren öffentlichen SSH-Schlüssel beim Erstellen des virtuellen Computers mit Azure-Portal, CLI, Resource Manager-Vorlagen oder anderen Methoden ein:

* [Erstellen Sie eines virtuellen Linux-Computers mit dem Azure-Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer Linux-VM mit der Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer Linux-VM mit einer Azure-Vorlage](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Falls Sie nicht mit dem Format öffentlicher SSH-Schlüssel vertraut sein sollten, können Sie Ihren öffentlichen Schlüssel anzeigen, indem Sie `cat` wie unten angegeben ausführen. Ersetzen Sie hierbei `~/.ssh/id_rsa.pub` durch den Speicherort Ihrer eigenen Datei mit dem öffentlichen Schlüssel:

```bash
cat ~/.ssh/id_rsa.pub
```

Stellen Sie sicher, dass Sie keine zusätzlichen Leerzeichen kopieren, wenn Sie den Inhalt der Datei mit dem öffentlichen Schlüssel für die Verwendung im Azure-Portal oder in einer Resource Manager-Vorlage kopieren und einfügen. Bei Nutzung von macOS können Sie die Datei mit dem öffentlichen Schlüssel (standardmäßig `~/.ssh/id_rsa.pub`) beispielsweise per Pipe mit **pbcopy** verknüpfen, um den Inhalt zu kopieren (für diesen Zweck gibt es auch andere Linux-Programme, z.B. **xclip**).

Der öffentliche Schlüssel, den Sie auf Ihrer Linux-VM in Azure platzieren, wird standardmäßig in `~/.ssh/id_rsa.pub` gespeichert, sofern Sie den Speicherort nicht bei der Erstellung der Schlüssel geändert haben. Bei Verwendung der [Azure CLI 2.0](/cli/azure) zum Erstellen Ihres virtuellen Computers mit einem vorhandenen öffentlichen Schlüssel geben Sie den Wert oder Speicherort dieses öffentlichen Schlüssels durch Ausführen des [az vm create](/cli/azure/vm#az_vm_create)-Befehls mit der `--ssh-key-value`-Option ein. 

## <a name="ssh-to-your-vm"></a>Herstellen der SSH-Verbindung mit Ihrer VM
Stellen Sie mit dem auf Ihrer Azure-VM bereitgestellten öffentlichen Schlüssel und dem privaten Schlüssel auf Ihrem lokalen System mithilfe der IP-Adresse oder dem DNS-Namen Ihres virtuellen Computers eine SSH-Verbindung mit Ihrem virtuellen Computer her. Ersetzen Sie *azureuser* und *myvm.westus.cloudapp.azure.com* im folgenden Befehl mit dem Benutzernamen des Administrators und dem vollständig qualifizierten Domänennamen (oder der IP-Adresse):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Falls Sie beim Erstellen des Schlüsselpaars eine Passphrase angegeben haben, müssen Sie die Passphrase eingeben, wenn Sie während des Anmeldeprozesses dazu aufgefordert werden. (Der Server wird dem Ordner `~/.ssh/known_hosts` hinzugefügt, und Sie werden erst wieder zum Herstellen der Verbindung aufgefordert, wenn sich der öffentliche Schlüssel auf Ihrer Azure-VM ändert oder der Servername unter `~/.ssh/known_hosts` entfernt wird.)

VMs, die mithilfe von SSH-Schlüsseln erstellt werden, werden standardmäßig mit deaktivierten Kennwörtern konfiguriert, um Brute-Force-Angriffe, bei denen Kennwörter erraten werden sollen, deutlich aufwändiger und somit schwieriger zu machen. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wird beschrieben, wie Sie schnell ein einfaches SSH-Schlüsselpaar erstellen können. 

* Wenn Sie weitere Hilfe zur Arbeit mit Ihrem SSH-Schlüsselpaar benötigen, finden Sie weitere Informationen unter [Ausführliche exemplarische Vorgehensweise zum Erstellen eines SSH-Schlüsselpaars mit zusätzlichen Zertifikaten für einen virtuellen Linux-Computer in Azure](create-ssh-keys-detailed.md).

* Wenn beim Herstellen von SSH-Verbindungen mit einer Azure-VM Probleme auftreten, finden Sie weitere Informationen unter [Behandeln von Problemen, Fehlern oder Ablehnungen im Zusammenhang mit der SSH-Verbindung mit einem virtuellen Azure Linux-Computer](troubleshoot-ssh-connection.md).


