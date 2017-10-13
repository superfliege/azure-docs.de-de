---
title: Erstellen eines virtuellen Testcomputers in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie als Cloudbetreiber einen virtuellen Testcomputer in Azure Stack bereitstellen.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: c86646e1-a12e-493f-b396-f17bfacd60c2
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: erikje
ms.openlocfilehash: 233cf4df53af6a49e5fe4c5d51e112d8196a7530
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-test-virtual-machine-in-azure-stack"></a>Erstellen eines virtuellen Testcomputers in Azure Stack

*Gilt für: Azure Stack Development Kit*

Als Azure Stack-Bediener können Sie einen virtuellen Testcomputer zum Überprüfen Ihrer [Azure Stack](azure-stack-poc.md) Developer Kit-Bereitstellung erstellen.

> [!NOTE]
> Bevor Sie virtuelle Computer bereitstellen können, müssen Sie dem [Azure Stack-Marketplace das Windows Server 2016 Evaluation-Image hinzufügen](azure-stack-add-default-image.md).
> 
> 

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers
1. Sie müssen sich auf dem Azure Stack Development Kit-Host beim Administratorportal (`https://adminportal.local.azurestack.external`) [anmelden](azure-stack-connect-azure-stack.md). Klicken Sie dann auf **Neu** > **Compute** > **Windows Server 2016 Datacenter Eval** > **Erstellen**.  
2. Geben Sie auf dem Blatt **Grundlagen** Werte für **Name**, **Benutzername** und **Kennwort** ein. Wählen Sie ein **Abonnement**aus. Erstellen Sie eine **Ressourcengruppe**, oder wählen Sie eine vorhandene aus, und klicken Sie anschließend auf **OK**.  
3. Klicken Sie auf dem Blatt **Größe auswählen** auf **A1 Standard** und anschließend auf **Auswählen**.  
4. Behalten Sie auf dem Blatt **Einstellungen** die Standardwerte bei, und klicken Sie auf **OK**.
5. Klicken Sie auf dem Blatt **Zusammenfassung** auf **OK**, um den virtuellen Computer zu erstellen.  
6. Klicken Sie zum Anzeigen Ihres neuen virtuellen Computers auf **Alle Ressourcen**, und suchen Sie dann nach dem virtuellen Computer. Klicken Sie auf seinen Namen.
    ![](media/azure-stack-provision-vm/image06.png)


## <a name="next-steps"></a>Nächste Schritte
[Verwenden des Administrator- und des Benutzerportals in Azure Stack](azure-stack-manage-portals.md)
