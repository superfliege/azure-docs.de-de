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
ms.date: 06/29/2018
ms.author: anwestg
ms.openlocfilehash: 53766099f283f802482fe8e84144502d386b1d69
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440150"
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Weiterverteilen von Azure App Service in Azure Stack in Fehlerdomänen

*Gilt für: Integrierte Azure Stack-Systeme*

Mit dem Update 1802 unterstützt Azure Stack ab sofort die Verteilung von Arbeitsauslastungen an Fehlerdomänen, ein Feature, das für die Bereitstellung von Hochverfügbarkeit von entscheidender Bedeutung ist.

>[!IMPORTANT]
>Sie müssen Ihr in Azure Stack integriertes System auf 1802 aktualisieren, um von der Unterstützung für Fehlerdomänen profitieren zu können. Dieses Dokument gilt nur für Bereitstellungen von App Service-Ressourcenanbietern, die vor dem Update 1802 abgeschlossen wurden. Wenn Sie App Service in Azure Stack bereitgestellt haben, nachdem das Update 1802 auf Azure Stack angewendet wurde, wurde der Ressourcenanbieter bereits an die Fehlerdomänen verteilt.

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Ausgleichen eines App Service-Ressourcenanbieters über mehrere Fehlerdomänen hinweg

Für die Neuverteilung der für den App Service-Ressourcenanbieter bereitgestellten Skalierungsgruppen müssen Sie für jede Skalierungsgruppe die Schritte in diesem Artikel durchführen. Standardmäßig lauten die Namen der Skalierungsgruppen wie folgt:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

>[!NOTE]
> Wenn Sie keine Instanzen in den Skalierungsgruppen im Workertarif bereitgestellt haben, müssen Sie diese Skalierungsgruppen nicht erneut ausgleichen. Die Skalierungsgruppen werden ordnungsgemäß ausgeglichen, wenn Sie sie in Zukunft horizontal hoch skalieren.

Führen Sie die folgenden Schritte aus, um die Skalierungsgruppen horizontal hochzuskalieren:

1. Melden Sie sich beim Azure Stack-Administratorportal an.
1. Wählen Sie **Weitere Dienste**.
1. Wählen Sie unter „COMPUTE“ die Option **Skalierungsgruppen für virtuelle Computer** aus. Vorhandene Skalierungsgruppen, die im Rahmen der App Service-Bereitstellung bereitgestellt wurden, werden unter Angabe der Anzahl der Instanzen aufgeführt. Die folgende Bildschirmaufnahme zeigt ein Beispiel für Skalierungsgruppen.

      ![Auf der Benutzeroberfläche „VM-Skalierungsgruppen“ aufgeführte Azure App Service-Skalierungsgruppen][1]

1. Skalieren Sie die einzelnen Gruppen horizontal hoch. Wenn Sie beispielsweise über drei Instanzen in der Skalierungsgruppe verfügen, müssen Sie diese auf 6 horizontal hochskalieren, damit die drei neuen Instanzen für die Fehlerdomänen bereitgestellt werden. Im folgenden PowerShell-Beispiel wird veranschaulicht, wie die Skalierungsgruppe horizontal hochskaliert wird.

   ```powershell
   Add-AzureRmAccount -EnvironmentName AzureStackAdmin 

   # Get current scale set
   $vmss = Get-AzureRmVmss -ResourceGroupName "AppService.local" -VMScaleSetName "SmallWorkerTierScaleSet"

   # Set and update the capacity of your scale set
   $vmss.sku.capacity = 6
   Update-AzureRmVmss -ResourceGroupName AppService.local" -Name "SmallWorkerTierScaleSet" -VirtualMachineScaleSet $vmss
   ```

   >[!NOTE]
   >Es kann je nach Typ der Rolle und der Anzahl von Instanzen mehrere Stunden dauern, bis dieser Schritt abgeschlossen ist.

1. Überwachen Sie unter **App Service-Verwaltungsrollen** den Status der neuen Rolleninstanzen. Wählen Sie zur Überprüfung des Status einer Rolleninstanz den Rollentyp in der Liste aus

    ![Azure App Service in Azure Stack-Rollen][2]

1. Wenn der Status der neuen Rolleninstanzen **Bereit** lautet, wechseln Sie wieder zur **VM-Skalierungsgruppe** und **löschen** Sie die alten Rolleninstanzen.

1. Wiederholen Sie diese Schritte für **jede** VM-Skalierungsgruppe.

## <a name="next-steps"></a>Nächste Schritte

Sie können auch weitere [PaaS-Dienste (Platform-as-a-Service)](azure-stack-tools-paas-services.md) ausprobieren.

* [SQL Server-Ressourcenanbieter](azure-stack-sql-resource-provider-deploy.md)
* [MySQL-Ressourcenanbieter](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
