---
title: Zugreifen auf ein benutzerdefiniertes Lab in Azure DevTest Labs | Microsoft-Dokumentation
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
ms.date: 04/09/2018
ms.author: spelluru
ms.openlocfilehash: ce4522673bac56f73944413d102b7cb36cf93f30
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-access-a-custom-lab-in-azure-devtest-labs"></a>Tutorial: Zugreifen auf ein benutzerdefiniertes Lab in Azure DevTest Labs
In diesem Tutorial verwenden Sie das benutzerdefinierte Lab, das im [Tutorial: Erstellen eines benutzerdefinierten Labs](tutorial-create-custom-lab.md) erstellt wurde.

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Beanspruchen eines virtuellen Computers in einem benutzerdefinierten Lab
> * Herstellen der Verbindung zur VM
> * Aufheben des Anspruchs auf den virtuellen Computer

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="access-the-lab"></a>Zugreifen auf das Lab

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im linken Menü die Option **Alle Ressourcen** aus. 
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

## <a name="unclaim-the-vm"></a>Aufheben des Anspruchs auf den virtuellen Computer
Wenn Sie den virtuellen Computer nicht benötigen, führen Sie folgende Schritte aus, um den Anspruch auf den virtuellen Computer aufzuheben: 

1. Wählen Sie auf der Seite des virtuellen Computers auf der Symbolleiste die Option **Anspruch aufheben** aus. 

    ![Aufheben des Anspruchs auf den virtuellen Computer](./media/tutorial-use-custom-lab/unclaim-vm-menu.png)
1. Der virtuelle Computer wird heruntergefahren, bevor der Anspruch aufgehoben wird. 

    ![Status der Anspruchsaufhebung](./media/tutorial-use-custom-lab/unclaim-status.png) 
1. Nachdem der Anspruch aufgehoben wurde, wird der virtuelle Computer unten in der Liste **Abrufbare virtuelle Computer** angezeigt. 
    
## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurde gezeigt, wie Sie auf ein benutzerdefiniertes Lab zugreifen und dieses verwenden, das unter Verwendung von Azure DevTest Labs erstellt wurde. Weitere Informationen zum Zugreifen auf und Verwenden von virtuellen Computern in einem benutzerdefinierten Lab finden Sie hier: 

> [!div class="nextstepaction"]
> [Verwenden von virtuellen Computern in einem benutzerdefinierten Lab](devtest-lab-add-vm.md)

