---
title: Ändern der Größe eines virtuellen Computers in einem Lab in Azure DevTest Labs | Microsoft-Dokumentation
description: Informationen zum Ändern der Größe eines virtuellen Computers in Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8460f09e-482f-48ba-a57a-c95fe8afa001
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: spelluru
ms.openlocfilehash: 9b9a1839bf4b028aec13b764b4de66385de4189e
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47090750"
---
# <a name="resize-a-vm-in-a-lab-in-azure-devtest-labs"></a>Ändern der Größe eines virtuellen Computers in einem Lab in Azure DevTest Labs
Eines der wichtigen Features von virtuellen Azure-Computern ist, dass Sie die Größe einer VM (virtueller Computer) basierend auf Ihren Anforderungen für CPU-, Netzwerk- oder Datenträgerleistung ändern können. Dieses Feature wird nun durch Azure DevTest Labs für VMs in einem Lab unterstützt. Das Feature zum Ändern der Größe entspricht der Labrichtlinie für zugelassene VM-Größen im Lab. Das heißt, Sie können die Größe einer VM nur in im Lab zugelassene Größen ändern. 


## <a name="steps-to-resize-a-vm-in-a-lab"></a>Vorgehensweise zum Ändern der Größe einer VM in einem Lab 
Führen Sie die folgenden Schritte aus, um die Größe einer VM in einem Lab in Azure DevTest Labs zu ändern: 

> [!NOTE]
> Wenn Sie über eine Remotedesktopsitzung (RDP) mit der VM verbunden sind, speichern Sie Ihre Arbeit, und trennen Sie die Verbindung zur VM, bevor Sie die Größe ändern.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Alle Dienste** und dann in der Liste die Option **DevTest Labs**.
3. Wählen Sie aus der Liste der Labs das Lab aus, das die VM enthält, deren Größe geändert werden soll.  
4. Wählen Sie im linken Bereich die Option **Meine virtuellen Computer** aus. 
5. Wählen Sie eine VM aus der Liste der VMs aus.
6. Klicken Sie auf der Symbolleiste auf **Anhalten**, wenn die VM ausgeführt wird. Überprüfen Sie den Status des Vorgangs im Fenster **Benachrichtigungen**. Warten Sie darauf, dass die VM angehalten wurde, und schließen Sie dann das Fenster **Benachrichtigungen**. 

    ![Beenden des virtuellen Computers](media/devtest-lab-resize-vm/stop-vm.png)
1. Klicken Sie im linken Menü auf der Seite Ihrer VM unter **EINSTELLUNGEN** auf **Größe**.

    ![Größenmenü](media/devtest-lab-resize-vm/size-menu.png)
1. Wählen Sie eine Größe für Ihre VM im Fenster **Größe auswählen** aus, und klicken Sie auf **Auswählen**.     
1. Überprüfen Sie den Status des Größenänderungsvorgangs im Fenster **Benachrichtigungen**.

    ![Status der Größenänderung](media/devtest-lab-resize-vm/resize-status.png)
10. Schließen Sie nach Abschluss des Größenänderungsvorgangs das **Benachrichtigungsfenster**. 
11. Klicken Sie im linken Menü auf **Übersicht**, und klicken Sie dann auf der Symbolleiste auf **Neustart**, um die VM neu zu starten. 

## <a name="next-steps"></a>Nächste Schritte
Ausführliche Informationen über das Feature zum Ändern der Größe, das von Azure-VMs unterstützt wird, finden Sie unter [Resize virtual machines (Ändern der Größe von virtuellen Computern)](https://azure.microsoft.com/blog/resize-virtual-machines/).


