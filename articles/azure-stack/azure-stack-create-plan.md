---
title: Erstellen von Plänen in Azure Stack | Microsoft-Dokumentation
description: Erstellen Sie als Cloudadministrator einen Plan, mit dem Abonnenten virtuelle Computer bereitstellen können.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: b1bfff16c4f51a9fa53204930df78cbd2cf19b8d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-plan-in-azure-stack"></a>Erstellen von Plänen in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

[Pläne](azure-stack-key-features.md) sind Gruppen mit mindestens einem Dienst. Als Anbieter können Sie Pläne erstellen und Ihren Benutzern anbieten. Im Gegenzug abonnieren Ihre Benutzer die Angebote, um die darin enthaltenen Pläne und Dienste zu verwenden. In diesem Beispiel wird veranschaulicht, wie Sie einen Plan erstellen, der die Compute-, Netzwerk- und Speicherressourcenanbieter enthält. Mit diesem Plan erhalten Abonnenten die Möglichkeit, virtuelle Computer bereitzustellen.

1. Melden Sie sich beim Azure Stack-Administratorportal an (https://adminportal.local.azurestack.external).

2. Wählen Sie zum Erstellen von Plänen und Angeboten, die Benutzer abonnieren können, die Option **Neu** > **Angebote + Pläne** > **Plan**.  
   ![Auswählen eines Plans](media/azure-stack-create-plan/select-plan.png)

3. Geben Sie im Bereich **Neuer Plan** Werte für die Optionen **Anzeigename** und **Ressourcenname** ein. Der Anzeigename ist der verständliche Name des Plans, der Benutzern angezeigt wird. Nur der Administrator kann den Ressourcennamen sehen. Dies ist der Name, der von Administratoren zum Verwenden des Plans als Azure Resource Manager-Ressource genutzt wird.  
   ![Angeben von Details](media/azure-stack-create-plan/plan-name.png)

4. Erstellen Sie eine neue **Ressourcengruppe**, oder wählen Sie eine vorhandene aus, die als Container für den Plan dient.  
   ![Angeben der Ressourcengruppe](media/azure-stack-create-plan/resource-group.png)

5. Wählen Sie **Dienste**, und aktivieren Sie anschließend jeweils das Kontrollkästchen für **Microsoft.Compute**, **Microsoft.Network** und **Microsoft.Storage**. Wählen Sie als Nächstes die Option **Auswählen**, um die Konfiguration zu speichern. Die Kontrollkästchen werden angezeigt, wenn Sie mit der Maus auf eine Option zeigen.  
   ![Auswählen von Diensten](media/azure-stack-create-plan/services.png)

6. Wählen Sie **Kontingente** und dann **Microsoft.Storage (local)**. Wählen Sie entweder das Standardkontingent aus, oder klicken Sie auf **Neues Kontingent erstellen**, um das Kontingent anzupassen.  
   ![Kontingente](media/azure-stack-create-plan/quotas.png)

7. Geben Sie bei der Erstellung eines neuen Kontingents einen **Namen** für das Kontingent ein, geben Sie die Kontingentwerte an, und wählen Sie **OK**. Der Bereich **Kontingent erstellen** wird geschlossen.
   ![Neues Kontingent](media/azure-stack-create-plan/new-quota.png)

   Anschließend wählen Sie das von Ihnen neu erstellte Kontingent aus. Wenn Sie das Kontingent auswählen, wird es zugewiesen, und der Auswahlbereich wird geschlossen.  
   ![Zuweisen des Kontingents](media/azure-stack-create-plan/assign-quota.png)

8. Wiederholen Sie die Schritte 6 und 7, um Kontingente für **Microsoft.Network (local)** und **Microsoft.Compute (local)** zu erstellen und zuzuweisen.  Falls allen drei Diensten Kontingente zugewiesen wurden, werden diese in etwa wie in der folgenden Abbildung angezeigt.  
   ![Durchführen der Kontingentzuweisungen](media/azure-stack-create-plan/all-quotas-assigned.png)

9. Wählen Sie im Bereich **Kontingente** die Option **OK** und dann im Bereich **Neuer Plan** die Option **Erstellen**, um den Plan zu erstellen.  
    ![Erstellen des Plans](media/azure-stack-create-plan/create.png)
10. Wählen Sie zum Anzeigen des neuen Plans die Option **Alle Ressourcen**, suchen Sie nach dem Plan, und wählen Sie seinen Namen aus. Wenn Ihre Liste mit den Ressourcen sehr lang ist, können Sie die Option **Suche** verwenden, um anhand des Namens nach Ihrem Plan zu suchen.  
   ![Überprüfen des Plans](media/azure-stack-create-plan/plan-overview.png)

### <a name="next-steps"></a>Nächste Schritte
[Erstellen von Angeboten](azure-stack-create-offer.md)
