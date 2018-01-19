---
title: Erstellen eines benutzerdefinierten Azure DevTest Labs-Images aus einer VHD-Datei | Microsoft Docs
description: Erfahren Sie, wie Sie mit dem Azure-Portal aus einer VHD-Datei ein benutzerdefiniertes Image in Azure DevTest Labs erstellen.
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: v-craic
ms.openlocfilehash: d1f1b9948fb591484c107818a01e141932effbba
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Erstellen eines benutzerdefinierten Images aus einer VHD-Datei

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Schrittweise Anweisungen

Die folgenden Schritte führen Sie durch die Erstellung eines benutzerdefinierten Images aus einer VHD-Datei mithilfe des Azure-Portals:

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.

1. Wählen Sie **Alle Dienste** und dann in der Liste die Option **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  

1. Wählen Sie im Hauptbereich des Labs die Option **Konfiguration und Richtlinien** aus. 

1. Wählen Sie im Bereich **Konfiguration und Richtlinien** die Option **Benutzerdefinierte Images**.

1. Wählen Sie im Bereich **Benutzerdefinierte Images** die Option **+ Hinzufügen**.

    ![Hinzufügen des benutzerdefinierten Images](./media/devtest-lab-create-template/add-custom-image.png)

1. Geben Sie den Namen des benutzerdefinierten Images ein. Dieser Name wird in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen.

1. Geben Sie die Beschreibung des benutzerdefinierten Images ein. Diese Beschreibung wird in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen.

1. Wählen Sie als **Betriebssystemtyp** entweder **Windows** oder **Linux**.

    - Wenn Sie **Windows** auswählen, können Sie über das Kontrollkästchen angeben, ob *sysprep* auf dem Computer ausgeführt wurde. 
    - Wenn Sie **Linux** auswählen, können Sie über das Kontrollkästchen angeben, ob *deprovision* auf dem Computer ausgeführt wurde. 

1. Wählen Sie im Dropdownmenü die Option **VHD**. Dies ist die virtuelle Festplatte (VHD), die zum Erstellen des neuen benutzerdefinierten Images verwendet werden soll. Aktivieren Sie bei Bedarf das Kontrollkästchen **VHD mithilfe von PowerShell hochladen**.

1. Sie können auch einen Plannamen, ein Planangebot und einen Planherausgeber eingeben, wenn das Image, das zum Erstellen des benutzerdefinierten Images verwendet wird, nicht von Microsoft veröffentlicht wird.

1. Wählen Sie **OK** , um das benutzerdefinierte Image zu erstellen.

Nach wenigen Minuten wird das benutzerdefinierte Image erstellt und im Speicherkonto des Labs gespeichert. Das Image ist in der Liste mit den Basisimages verfügbar, falls ein Lab-Benutzer einen neuen virtuellen Computer erstellen möchte.

![Verfügbares benutzerdefiniertes Image in der Liste mit den Basisimages](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge

- [Custom images or formulas? (Benutzerdefinierte Images oder Formeln?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Kopieren benutzerdefinierter Images zwischen Azure DevTest Labs)](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen eines virtuellen Computers zum Lab](./devtest-lab-add-vm.md)
