---
title: "Anfügen eines Datenträgers an eine Linux-VM | Microsoft Docs"
description: "Verwenden Sie das Portal, um einen neuen oder vorhandenen Datenträger an einen virtuellen Linux-Computer anzufügen."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: cynthn
ms.openlocfilehash: 1a7c98207a5f19d514a0cd05b66898e7fb725944
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="use-the-portal-to-attach-a-data-disk-to-a-linux-vm"></a>Anfügen eines Datenträgers an einen virtuellen Linux-Computer mithilfe des Portals 
In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal neue und vorhandene Datenträger an einen virtuellen Linux-Computer anfügen können. Sie können auch [einen Datenträger an eine Windows-VM im Azure-Portal anfügen](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Bevor Sie Datenträger an Ihren virtuellen Computer anfügen, lesen Sie diese Tipps:

* Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Für die Verwendung von Premium-Speicher benötigen Sie einen virtuellen Computer der DS- oder GS-Serie. Sie können für diese virtuellen Computer sowohl Premium- als auch Standard-Datenträger verwenden. Premium-Speicher ist in bestimmten Regionen verfügbar. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../windows/premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Bei an virtuelle Computer angefügten Datenträgern handelt es sich um VHD-Dateien, die in Azure gespeichert sind. Weitere Informationen finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="find-the-virtual-machine"></a>Suchen des virtuellen Computers
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im linken Menü auf **Virtuelle Computer**.
3. Wählen Sie den gewünschten virtuellen Computer aus der Liste aus.
4. Klicken Sie auf der Seite „Virtuelle Computer“ unter **Zusammenfassung** auf **Datenträger**.
   
    ![Öffnen der Datenträgereinstellungen](./media/attach-disk-portal/find-disk-settings.png)


## <a name="attach-a-new-disk"></a>Anfügen eines neuen Datenträgers

1. Klicken Sie im Bereich **Datenträger** auf **+ Datenträger anfügen**.
2. Klicken Sie auf das Dropdownmenü **Name**, und wählen Sie **Datenträger erstellen** aus:

    ![Erstellen eines verwalteten Azure-Datenträgers](./media/attach-disk-portal/create-new-md.png)

3. Geben Sie einen Namen für Ihren verwalteten Datenträger ein. Überprüfen und aktualisieren Sie bei Bedarf die Standardeinstellungen, und klicken Sie dann auf **Erstellen**.
   
   ![Überprüfen der Datenträgereinstellungen](./media/attach-disk-portal/create-new-md-settings.png)

4. Klicken Sie auf **Speichern**, um den verwalteten Datenträger zu speichern und die Konfiguration des virtuellen Computers zu aktualisieren:

   ![Speichern des neuen verwalteten Azure-Datenträgers](./media/attach-disk-portal/confirm-create-new-md.png)

5. Nachdem der Datenträger von Azure erstellt und an den virtuellen Computer angefügt wurde, wird der neue Datenträger in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger**angezeigt. Da es sich bei verwalteten Datenträgern um Ressourcen der obersten Ebene handelt, werden sie im Stammverzeichnis der Ressourcengruppe angezeigt:

   ![Verwalteter Azure-Datenträger in Ressourcengruppe](./media/attach-disk-portal/view-md-resource-group.png)

## <a name="attach-an-existing-disk"></a>Anfügen eines vorhandenen Datenträgers
1. Klicken Sie im Bereich **Datenträger** auf **+ Datenträger anfügen**.
2. Klicken Sie auf das Dropdownmenü **Name**, um eine Liste der vorhandenen verwalteten Datenträger anzuzeigen, auf die von Ihrem Azure-Abonnement aus zugegriffen werden kann. Wählen Sie den verwalteten Datenträger aus, den Sie anfügen möchten:

   ![Anfügen eines vorhandenen verwalteten Azure-Datenträgers](./media/attach-disk-portal/select-existing-md.png)

3. Klicken Sie auf **Speichern**, um den vorhandenen verwalteten Datenträger anzufügen und die Konfiguration des virtuellen Computers zu aktualisieren:
   
   ![Speichern von Aktualisierungen verwalteter Azure-Datenträger](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. Nachdem der Datenträger von Azure an den virtuellen Computer angefügt wurde, wird er in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger**angezeigt.



## <a name="next-steps"></a>Nächste Schritte
Sie können auch [einen Datenträger mithilfe der Azure CLI anfügen](add-disk.md).
