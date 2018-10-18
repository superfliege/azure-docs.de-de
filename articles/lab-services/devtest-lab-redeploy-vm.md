---
title: Erneute Bereitstellung eines virtuellen Computers in einem Lab in Azure DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen virtuellen Computer in Azure DevTest Labs erneut bereitstellen (von einem Azure-Knoten auf einen anderen verschieben).
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
ms.openlocfilehash: 273b0f1105d8b71b90a06e2627e201b97f12a754
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47095394"
---
# <a name="redeploy-a-vm-in-a-lab-in-azure-devtest-labs"></a>Erneute Bereitstellung eines virtuellen Computers in einem Lab in Azure DevTest Labs
Wenn Sie keine Verbindung zu einem virtuellen Computer (VM) in einem Lab über eine Remotedesktopverbindung herstellen können, stellen Sie die VM erneut bereit, und versuchen Sie nochmals, eine Verbindung herzustellen. Wenn Sie eine VM erneut bereitstellen, verschiebt DevTest Labs die VM von dem Knoten, auf dem sie ausgeführt wird, auf einen neuen Knoten innerhalb der Azure-Infrastruktur. Dann wird die VM gestartet, wobei alle Konfigurationsoptionen und zugehörigen Ressourcen beibehalten werden. Mit diesem Feature sparen Sie die Zeit, die Sie sonst für die Fehlerbehebung bei Ihrer Remotedesktopverbindung oder beim Anwendungszugriff auf Windows-basierte VMs im Lab benötigen. 

## <a name="steps-to-redeploy-a-vm-in-a-lab"></a>Schritte zum erneuten Bereitstellen einer VM in einem Lab 
Führen Sie die folgenden Schritte aus, um eine VM in einem Lab in Azure DevTest Labs erneut bereitzustellen: 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Alle Dienste** und dann in der Liste die Option **DevTest Labs**.
3. Wählen Sie in der Liste der Labs das Lab aus, das die erneut bereitzustellende VM enthält.  
4. Wählen Sie im linken Bereich die Option **Meine virtuellen Computer** aus. 
5. Wählen Sie in der Liste der VMs eine VM aus.
6. Wählen Sie auf der VM-Seite für Ihre VM im linken Menü unter **Vorgänge** **Erneut bereitstellen** aus.

    ![Erneute Bereitstellung](media/devtest-lab-redeploy-vm/redeploy.png)
7. Lesen Sie sich die Informationen auf der Seite durch, und klicken Sie auf die Schaltfläche **Erneut bereitstellen**. 9. Überprüfen Sie den Status der erneuten Bereitstellung im Fenster **Benachrichtigungen**.

    ![Status der erneuten Bereitstellung](media/devtest-lab-redeploy-vm/redeploy-status.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Ändern der Größe einer VM in Azure DevTest Labs finden Sie unter [Resize a VM (Ändern der Größe einer VM)](devtest-lab-resize-vm.md).


