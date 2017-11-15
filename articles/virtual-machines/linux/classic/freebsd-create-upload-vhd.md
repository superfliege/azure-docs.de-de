---
title: Erstellen und Hochladen eines FreeBSD-VM-Images | Microsoft Docs
description: "Erfahren Sie, wie Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD), die das FreeBSD-Betriebssystem enthält, erstellen und hochladen, um einen virtuelle Azure-Computer zu erstellen."
services: virtual-machines-linux
documentationcenter: 
author: thomas1206
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: huishao
ms.openlocfilehash: 7b41826f071174df8f00af56a228e0f31c3cfe2f
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2017
---
# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Erstellen und Hochladen einer FreeBSD-VHD in Azure
In diesem Artikel erfahren Sie, wie Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) mit FreeBSD-Betriebssystem erstellen und hochladen. Nach dem Hochladen können Sie sie als eigenes Image verwenden, um einen virtuellen Computer (Virtual Machine, VM) in Azure zu erstellen.

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Informationen zum Hochladen einer VHD mit dem Resource Manager-Modell finden Sie [hier](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie über die folgenden Elemente verfügen:

* **Azure-Abonnement**: Falls Sie noch nicht über ein Konto verfügen, können Sie in wenigen Minuten eines erstellen. MSDN-Abonnenten finden weitere Informationen unter [Monatliche Azure-Gutschrift für Visual Studio-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Alle anderen Benutzer können sich unter [Erstellen eines kostenlosen Testkontos](https://azure.microsoft.com/pricing/free-trial/)informieren.  
* **Azure PowerShell-Tools**: Das Azure PowerShell-Modul muss installiert und für die Verwendung Ihres Azure-Abonnements konfiguriert sein. Informationen zum Herunterladen dieses Moduls finden Sie unter [Azure-Downloads](https://azure.microsoft.com/downloads/). Ein Tutorial zum Installieren und Konfigurieren des Moduls ist ebenfalls verfügbar. Verwenden Sie das Cmdlet [Azure-Downloads](https://azure.microsoft.com/downloads/) , um die VHD hochzuladen.
* **In einer VHD-Datei installiertes FreeBSD-Betriebssystem**: Auf einer virtuellen Festplatte muss ein unterstütztes FreeBSD-Betriebssystem installiert sein. Zum Erstellen von VHD-Dateien stehen mehrere verschiedene Tools bereit. Sie können beispielsweise eine Virtualisierungslösung wie Hyper-V verwenden, um die VHD-Datei zu erstellen und das Betriebssystem zu installieren. Eine Anleitung zum Installieren und Verwenden von Hyper-V finden Sie unter [Installieren von Hyper-V und Erstellen eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Sie können den Datenträger mit dem Hyper-V-Manager oder dem Cmdlet [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx)in das VHD-Format konvertieren. Auf MSDN steht zudem ein [Tutorial zum Verwenden von FreeBSD mit Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx)zur Verfügung.
>
>

Dieser Vorgang umfasst die folgenden fünf Schritte:

## <a name="step-1-prepare-the-image-for-upload"></a>Schritt 1: Vorbereiten des hochzuladenden Images
Führen Sie auf dem virtuellen Computer, auf dem Sie das FreeBSD-Betriebssystem installiert haben, die folgenden Schritte aus:

1. Aktivieren Sie DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. Aktivieren Sie SSH.

    Stellen Sie sicher, dass der SSH-Server installiert und konfiguriert ist, damit er beim Booten hochfährt. Standardmäßig ist es nach der Installation von FreeBSD-Datenträger aktiviert. 
3. Richten Sie eine serielle Konsole ein.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. Installieren Sie sudo.

    Das Stammkonto ist in Azure deaktiviert. sudo muss daher mit einem nicht privilegierten Benutzer verwendet werden, um Befehle mit erhöhten Berechtigungen auszuführen.

        # pkg install sudo
   
5. Voraussetzungen für den Azure-Agent:

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. Installieren Sie den Azure-Agent.

    Die neueste Version des Azure-Agents finden Sie immer auf [github](https://github.com/Azure/WALinuxAgent/releases). Die Version 2.0.10 und höher unterstützt offiziell FreeBSD 10 und 10.1, und die Version 2.1.4 und höher (inklusive 2.2.x) unterstützt offiziell FreeBSD 10.2 und spätere Versionen.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    Für 2.0 verwenden wir 2.0.16 als Beispiel:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    Für 2.1 verwenden wir 2.1.4 als Beispiel:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > Vergewissern Sie sich nach der Installation des Azure-Agents, dass er ausgeführt wird:
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. Heben Sie die Bereitstellung des Systems auf.

    Heben Sie die Bereitstellung des Systems auf, um es zu bereinigen und für eine erneute Bereitstellung vorzubereiten. Mit dem folgenden Befehl werden auch das zuletzt bereitgestellte Benutzerkonto und die dazugehörigen Daten gelöscht:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Nun können Sie Ihren virtuellen Computer herunterfahren.

## <a name="step-2-prepare-the-connection-to-azure"></a>Schritt 2: Vorbereiten der Verbindung mit Azure
Stellen Sie sicher, dass Sie die Azure-Befehlszeilenschnittstelle im klassischen Bereitstellungsmodell (`azure config mode asm`) verwenden, und melden Sie sich anschließend bei Ihrem Konto an:

```azurecli
azure login
```


<a id="upload"></a>


## <a name="step-3-upload-the-vhd-file"></a>Schritt 3: Hochladen der VHD-Datei

Zum Hochladen der VHD-Datei benötigen Sie ein Speicherkonto. Sie können ein vorhandenes Speicherkonto auswählen oder [ein neues erstellen](../../../storage/common/storage-create-storage-account.md).

Wenn Sie die VHD-Datei hochladen, können Sie sie an einem beliebigen Speicherort innerhalb des Blobspeichers ablegen. Im Anschluss finden Sie einige Begriffe, die beim Hochladen der Datei verwendet werden:

* **BlobStorageURL** ist die URL für das in Schritt 2 erstellte Speicherkonto.
* **YourImagesFolder** handelt es sich um den Container innerhalb des Blobspeichers, in dem Sie Ihre Images speichern möchten.
* **VHDName** steht für die Bezeichnung, die im klassischen Azure-Portal zur Identifizierung der virtuellen Festplatte angezeigt wird.
* **PathToVHDFile** stellt den vollständigen Pfad und den Namen der .vhd-Datei dar.

Geben Sie über das Azure PowerShell-Fenster, welches Sie im vorherigen Schritt verwendet haben, Folgendes ein:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-4-create-a-vm-with-the-uploaded-vhd-file"></a>Schritt 4: Erstellen einer VM mit der hochgeladenen VHD-Datei
Nach dem Hochladen der VHD-Datei können Sie sie als Image zu der Ihrem Abonnement zugeordneten Liste benutzerdefinierter Images hinzufügen und mit diesem benutzerdefinierten Image einen virtuellen Computer erstellen.

1. Geben Sie über das Azure PowerShell-Fenster, welches Sie im vorherigen Schritt verwendet haben, Folgendes ein:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

   > [!NOTE]
   > Verwenden Sie Linux als Betriebssystemtyp. Die aktuelle Version von Azure PowerShell akzeptiert nur „Linux“ oder „Windows“ als Parameter.
   >
   >
2. Nachdem Sie die vorherigen Schritte abgeschlossen haben, wird das neue Image aufgelistet, sobald Sie die Registerkarte **Images** im klassischen Azure-Portal auswählen.  

    ![Wählen Sie ein Image aus](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. Erstellen Sie einen virtuellen Computer über den Katalog. Dieses neue Image steht unter **Eigene Images**zur Verfügung.
4. Wählen Sie das neue Image aus. Gehen Sie die Eingabeaufforderungen durch, um Hostname, Kennwort/SSH-Schlüssel usw. einzurichten.

    ![Benutzerdefiniertes Image](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. Nach Abschluss der Bereitstellung sehen Sie, dass Ihr virtueller FreeBSD-Computer in Azure ausgeführt wird.

    ![FreeBSD-Image in Azure](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
