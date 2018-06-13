---
title: 'App Service in Azure Stack: Update für Fehlerdomänen | Microsoft-Dokumentation'
description: Weiterverteilen von Azure App Service in Azure Stack in Fehlerdomänen
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2018
ms.author: anwestg
ms.openlocfilehash: 42adef66fb1b1141ab44aab3a1ccdaae022202b5
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32150973"
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Weiterverteilen von Azure App Service in Azure Stack in Fehlerdomänen

*Gilt für: Integrierte Azure Stack-Systeme*

Mit dem Update 1802 unterstützt Azure Stack ab sofort die Verteilung von Workloads an Fehlerdomänen, ein Feature, das für die Bereitstellung von Hochverfügbarkeit von entscheidender Bedeutung ist.

> [!IMPORTANT]
> Sie müssen Ihr in Azure Stack integriertes System auf 1802 aktualisiert haben, um von der Unterstützung für Fehlerdomänen profitieren zu können.  Dieses Dokument gilt nur für Bereitstellungen von App Service-Ressourcenanbietern, die vor dem Update 1802 durchgeführt wurden.  Wenn Sie App Service in Azure Stack bereitgestellt haben, nachdem das Update 1802 auf Azure Stack angewendet wurde, wurde der Ressourcenanbieter bereits an die Fehlerdomänen verteilt.
>
>

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Ausgleichen eines App Service-Ressourcenanbieters über mehrere Fehlerdomänen hinweg

Um die für den App Service-Ressourcenanbieter bereitgestellten Skalierungsgruppen neu zu verteilen, müssen Sie für jede Skalierungsgruppe die folgenden Schritte durchführen.  Standardmäßig lauten die Namen der Skalierungsgruppen wie folgt:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

> [!NOTE]
> Wenn keine Instanzen in den Skalierungsgruppen im Workertarif bereitgestellt wurden, müssen Sie diese Skalierungsgruppen nicht erneut ausgleichen.  Die Skalierungsgruppen werden ordnungsgemäß ausgeglichen, wenn Sie sie in Zukunft horizontal hoch skalieren.
>
>

1. Navigieren Sie im Azure Stack-Administratorportal zu „VM-Skalierungsgruppen“.  Vorhandene Skalierungsgruppen, die im Rahmen der App Service-Bereitstellung bereitgestellt wurden, werden unter Angabe der Anzahl der Instanzen aufgeführt.

    ![Auf der Benutzeroberfläche „VM-Skalierungsgruppen“ aufgeführte Azure App Service-Skalierungsgruppen][1]

2. Skalieren Sie anschließend jede Gruppe horizontal hoch.  Wenn beispielsweise drei Instanzen in der Skalierungsgruppe enthalten sind, müssen Sie sie auf 6 horizontal hochskalieren, damit die drei neuen Instanzen für die Fehlerdomänen bereitgestellt werden.
    a. [Einrichten der Azure Stack-Administratorumgebung in PowerShell](azure-stack-powershell-configure-admin.md) b. Verwenden Sie dieses Beispiel, um die Skalierungsgruppe horizontal hochzuskalieren:
        ```powershell
                Add-AzureRmAccount -EnvironmentName AzureStackAdmin 

                # Get current scale set
                $vmss = Get-AzureRmVmss -ResourceGroupName "AppService.local" -VMScaleSetName "SmallWorkerTierScaleSet"

                # Set and update the capacity of your scale set
                $vmss.sku.capacity = 6
                Update-AzureRmVmss -ResourceGroupName "AppService.local" -Name "SmallWorkerTierScaleSet" -VirtualMachineScaleSet $vmss
        '''
> [!NOTE]
> Es kann je nach Typ der Rolle und der Anzahl von Instanzen einigen Stunden dauern, bis dieser Schritt abgeschlossen ist.
>
>

3. Überwachen Sie auf dem Blatt „App Service-Verwaltungsrollen“ den Status der neuen Rolleninstanzen.  Überprüfen Sie den Status einer einzelnen Rolleninstanz, indem Sie in der Liste auf den jeweiligen Rollentyp klicken.

    ![Azure App Service in Azure Stack-Rollen][2]

4. Sobald neue Instanzen in den Status **Bereit** wechseln, kehren Sie zum Blatt „VM-Skalierungsgruppe“ zurück, und **löschen** Sie alte Instanzen.

5. Wiederholen Sie diese Schritte für **jede** VM-Skalierungsgruppe.

## <a name="next-steps"></a>Nächste Schritte

Sie können auch weitere [PaaS-Dienste (Platform-as-a-Service)](azure-stack-tools-paas-services.md) ausprobieren.

* [SQL Server-Ressourcenanbieter](azure-stack-sql-resource-provider-deploy.md)
* [MySQL-Ressourcenanbieter](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
