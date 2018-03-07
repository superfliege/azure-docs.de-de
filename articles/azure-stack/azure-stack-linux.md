---
title: "Hinzufügen von Linux-Images zu Azure Stack"
description: "Hier erfahren Sie, wie Sie Linux-Images zu Azure Stack hinzufügen."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 6e6f9ca3b314ee2f58d8007e7ddc93ddd213e361
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="add-linux-images-to-azure-stack"></a>Hinzufügen von Linux-Images zu Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können virtuelle Linux-Computer in Azure Stack bereitstellen, indem Sie ein Linux-basiertes Image im Azure Stack-Marketplace hinzufügen. Am einfachsten wird ein Linux-Image über die Marketplace-Verwaltung zu Azure Stack hinzugefügt. Diese Images wurden vorbereitet und auf Kompatibilität mit Azure Stack getestet.

## <a name="marketplace-management"></a>Marketplace-Verwaltung

Wenn Sie Linux-Images aus Azure Marketplace herunterladen möchten, verwenden Sie die Verfahren im folgenden Artikel. Wählen Sie die Linux-Images aus, die Sie Benutzern in Azure Stack zur Verfügung stellen möchten.

[Herunterladen von Marketplace-Elementen von Azure in Azure Stack](azure-stack-download-azure-marketplace-item.md)

## <a name="prepare-your-own-image"></a>Vorbereiten eines eigenen Image

Bereiten Sie gemäß den folgenden Anweisungen Ihr eigenes Linux-Image vor:

   * [CentOS-basierte Verteilungen](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [SLES &amp; openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

1. Laden Sie den [Azure Linux-Agent](https://github.com/Azure/WALinuxAgent/) herunter, und installieren Sie ihn.

    Für die Bereitstellung Ihres virtuellen Linux-Computers in Azure Stack ist Azure Linux-Agent Version 2.2.2 oder höher erforderlich. Einige Versionen können nicht verwendet werden (etwa 2.2.12 und 2.2.13). Viele der zuvor aufgeführten Distributionen enthalten bereits eine Version des Agents (in der Regel als `WALinuxAgent` oder `walinuxagent` bezeichnet). Wenn Sie ein benutzerdefiniertes Image erstellen, müssen Sie den Agent manuell installieren. Die installierte Version kann entweder anhand des Paketnamens oder durch Ausführen von `/usr/sbin/waagent -version` auf dem virtuellen Computer ermittelt werden.

    Befolgen Sie die nachstehenden Anweisungen zur manuellen Installation des Azure Linux-Agents:

   a. Laden Sie zuerst den neuesten Azure Linux-Agent von [GitHub](https://github.com/Azure/WALinuxAgent/releases) herunter. Beispiel:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.21.tar.gz
   b. Entpacken Sie den Azure-Agent:

            # tar -vzxf v2.2.21.tar.gz
   c. Installieren von Python-Setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools
   d. Installieren Sie den Azure-Agent:

            # cd WALinuxAgent-2.2.21
            # sudo python3 setup.py install --register-service

     Auf Systemen, auf denen sowohl Python 2.x als auch Python 3.x installiert sind, muss möglicherweise folgender Befehl ausgeführt werden:

         sudo python3 setup.py install --register-service
     Weitere Informationen finden Sie in der [Infodatei](https://github.com/Azure/WALinuxAgent/blob/master/README.md) des Azure Linux-Agents.
2. [Fügen Sie das Image zum Marketplace hinzu](azure-stack-add-vm-image.md). Stellen Sie sicher, dass der `OSType`-Parameter auf `Linux` festgelegt ist.
3. Nachdem Sie das Image zum Marketplace hinzugefügt haben, wird ein Marketplace-Element erstellt, und Benutzer können einen virtuellen Linux-Computer bereitstellen.

## <a name="next-steps"></a>Nächste Schritte
[Übersicht über das Anbieten von Diensten in Azure Stack](azure-stack-offer-services-overview.md)
