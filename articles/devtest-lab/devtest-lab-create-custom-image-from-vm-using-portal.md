---
title: Erstellen eines benutzerdefinierten Azure DevTest Labs-Images aus einem virtuellen Computer | Microsoft Docs
description: Erfahren Sie, wie Sie mit dem Azure-Portal aus einem bereitgestellten virtuellen Computer ein benutzerdefiniertes Image in Azure DevTest Labs erstellen.
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: v-craic
ms.openlocfilehash: dc315bcc625ea98244bb5804ce6ff1c13d0ec7b1
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="create-a-custom-image-from-a-vm"></a>Erstellen eines benutzerdefinierten Images von einem virtuellen Computer

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Schrittweise Anweisungen

Sie können ein benutzerdefiniertes Image aus einem bereitgestellten virtuellen Computer erstellen und es anschließend zum Erstellen identischer virtuelle Computer verwenden. Die folgenden Schritte beschreiben, wie Sie ein benutzerdefiniertes Image von einem virtuellen Computer erstellen:

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.

1. Wählen Sie **Alle Dienste** und dann in der Liste die Option **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  

1. Wählen Sie im Hauptbereich des Labs die Option **Meine virtuellen Computer**.
 
1. Wählen Sie im Bereich **Meine virtuellen Computer** den virtuellen Computer aus, von dem Sie das benutzerdefinierte Image erstellen möchten.

1. Wählen Sie im Verwaltungsbereich des virtuellen Computers die Option **Benutzerdefiniertes Image erstellen (VHD)**.

    ![Menüelement: Benutzerdefiniertes Image erstellen](./media/devtest-lab-create-template/create-custom-image.png)

1. Geben Sie im Bereich **Benutzerdefiniertes Image** einen Namen und eine Beschreibung für Ihr benutzerdefiniertes Image ein. Diese Informationen werden in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen.

    ![Bereich „Benutzerdefiniertes Image erstellen“](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Wählen Sie aus, ob „sysprep“ auf dem virtuellen Computer ausgeführt wurde. Wenn „sysprep“ auf dem virtuellen Computer nicht ausgeführt wurde, geben Sie an, ob „sysprep“ ausgeführt werden soll, wenn Sie einen virtuellen Computer anhand dieses benutzerdefinierten Images erstellen.

1. Wählen Sie **OK** , wenn Sie mit der Erstellung des benutzerdefinierten Images fertig sind.

Nach wenigen Minuten wird das benutzerdefinierte Image erstellt und im Speicherkonto des Labs gespeichert. Das Image ist in der Liste mit den Basisimages verfügbar, falls ein Lab-Benutzer einen neuen virtuellen Computer erstellen möchte.

![Verfügbares benutzerdefiniertes Image in der Liste mit den Basisimages](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge

- [Custom images or formulas? (Benutzerdefinierte Images oder Formeln?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Kopieren benutzerdefinierter Images zwischen Azure DevTest Labs)](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen eines virtuellen Computers zum Lab](devtest-lab-add-vm.md)
