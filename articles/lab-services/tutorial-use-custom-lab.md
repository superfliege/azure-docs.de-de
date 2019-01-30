---
title: Zugreifen auf ein Lab in Azure DevTest Labs | Microsoft-Dokumentation
description: In diesem Tutorial greifen Sie auf das Lab zu, das unter Verwendung von Azure DevTest Labs erstellt wird, beanspruchen virtuelle Computer, verwenden diese, und heben den Anspruch wieder auf.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/18/2019
ms.author: spelluru
ms.openlocfilehash: b5abb8d4aad7c58bf673aa578255efe12d32ad4b
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54422897"
---
# <a name="tutorial-access-a-lab-in-azure-devtest-labs"></a>Tutorial: Zugreifen auf ein Lab in Azure DevTest Labs
In diesem Tutorial verwenden Sie das Lab, das in [Tutorial: Erstellen eines Labs mithilfe von Azure DevTest Labs](tutorial-create-custom-lab.md) erstellt wurde.

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Beanspruchen eines virtuellen Computers (virtual machine, VM) im Lab
> * Herstellen der Verbindung zur VM
> * Aufheben des Anspruchs auf den virtuellen Computer

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="access-the-lab"></a>Zugreifen auf das Lab

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im linken Menü die Option **Alle Ressourcen**. 
3. Wählen Sie **DevTest Labs** als Ressourcentyp aus. 
4. Wählen Sie das Lab aus. 

    ![Auswählen des Labs](./media/tutorial-use-custom-lab/search-for-select-custom-lab.png)

## <a name="claim-a-vm"></a>Beanspruchen eines virtuellen Computers

1. Klicken Sie in der Liste **Abrufbare virtuelle Computer** auf die Schaltfläche mit den drei Auslassungspunkten (**...**), und wählen Sie **Computer beanspruchen** aus.

    ![Beanspruchen eines virtuellen Computers](./media/tutorial-use-custom-lab/claim-virtual-machine.png)
1. Vergewissern Sie sich, dass der virtuelle Computer in der Liste **Meine virtuellen Computer** angezeigt wird.

    ![Mein virtueller Computer](./media/tutorial-use-custom-lab/my-virtual-machines.png)

## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

1. Wählen Sie Ihren virtuellen Computer aus der Liste aus. Die Seite **Virtueller Computer** wird angezeigt. Klicken Sie in der Symbolleiste auf **Verbinden**.

    ![Herstellen der Verbindung mit dem virtuellen Computer](./media/tutorial-use-custom-lab/connect-button.png)
2. Speichern Sie die heruntergeladene **RDP**-Datei auf Ihrer Festplatte, und verwenden Sie sie, um eine Verbindung mit dem virtuellen Computer herzustellen. Geben Sie den Benutzernamen und das Kennwort an, den bzw. das Sie beim Erstellen des virtuellen Computers im vorigen Abschnitt angegeben haben. 

    > [!NOTE] 
    > Für die VM muss SSH- bzw. RDP-Zugriff aktiviert sein, um eine Verbindung mit einer Linux-VM herzustellen. Schritte zum Herstellen einer Verbindung mit einer Linux-VM per RDP finden Sie unter [Installieren und Konfigurieren von Remotedesktop zum Herstellen einer Verbindung mit einem virtuellen Linux-Computer in Azure](../virtual-machines/linux/use-remote-desktop.md). 


## <a name="unclaim-the-vm"></a>Aufheben des Anspruchs auf den virtuellen Computer
Wenn Sie den virtuellen Computer nicht benötigen, führen Sie folgende Schritte aus, um den Anspruch auf den virtuellen Computer aufzuheben: 

1. Wählen Sie auf der Seite des virtuellen Computers auf der Symbolleiste die Option **Anspruch aufheben** aus. 

    ![Aufheben des Anspruchs auf den virtuellen Computer](./media/tutorial-use-custom-lab/unclaim-vm-menu.png)
1. Der virtuelle Computer wird heruntergefahren, bevor der Anspruch aufgehoben wird. Der Status dieses Vorgangs wird in den Benachrichtigungen angezeigt.  
3. Navigieren Sie wieder zur DevTest Lab-Seite, indem Sie im Breadcrumb-Menü im oberen Bereich auf den Namen Ihres Labs klicken. 
    
    ![Navigieren zum Lab](./media/tutorial-use-custom-lab/breadcrumb-to-lab.png)
1. Vergewissern Sie sich im unteren Bereich, dass der virtuelle Computer in der Liste **Abrufbare virtuelle Computer** enthalten ist.

    
## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurde gezeigt, wie Sie auf ein Lab zugreifen, das unter Verwendung von Azure DevTest Labs erstellt wurde, und wie Sie dieses Lab verwenden. Weitere Informationen zum Zugriff auf virtuelle Computer in einem Lab sowie zu deren Verwendung finden Sie hier: 

> [!div class="nextstepaction"]
> [Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs](devtest-lab-add-vm.md)

