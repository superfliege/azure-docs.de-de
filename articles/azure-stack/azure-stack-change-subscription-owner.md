---
title: Aktualisieren des Besitzers des Azure Stack-Benutzerabonnements | Microsoft-Dokumentation
description: Ändern Sie den Abrechnungsbesitzer für Azure Stack-Benutzerabonnements.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 06/12/2018
ms.author: brenduns
ms.reviewer: shnatara
ms.openlocfilehash: de8610944e11d8cf106ac484a0c6634c8661b5a2
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011240"
---
# <a name="change-the-owner-for-an-azure-stack-user-subscription"></a>Ändern des Besitzers für ein Azure Stack-Benutzerabonnement

Azure Stack-Bediener können mithilfe von PowerShell den Abrechnungsbesitzer eines Benutzerabonnements ändern. Die Änderung des Besitzers kann beispielsweise erforderlich sein, wenn ein Benutzer aus Ihrer Organisation ausscheidet.   

Einem Abonnement sind zwei Arten von *Besitzern* zugewiesen:

- **Abrechnungsbesitzer:** Der Abrechnungsbesitzer ist standardmäßig das Benutzerkonto, das das Abonnement aus einem Angebot erhält und dann für die Abrechnungsbeziehung dieses Abonnements zuständig ist. Dieses Konto ist gleichzeitig ein Administrator des Abonnements.  Für ein Abonnement kann jeweils nur ein einzelnes Benutzerkonto als Abrechnungsbesitzer festgelegt sein. Bei einem Abrechnungsbesitzer handelt es sich häufig um eine Führungskraft der Organisation oder um einen Teamleiter. 

  Der Abrechnungsbesitzer wird mithilfe des PowerShell-Cmdlets **Set-AzsUserSubscription** geändert.  

- **Über RBAC-Rollen hinzugefügte Besitzer:** Die Besitzerrolle kann weiteren Benutzern über die [rollenbasierte Zugriffssteuerung](azure-stack-manage-permissions.md) (Role Based Access Control, RBAC) erteilt werden.  Zur Ergänzung des Abrechnungsbesitzers kann eine beliebige Anzahl zusätzlicher Benutzerkonten hinzugefügt werden. Zusätzliche Besitzer sind ebenfalls Administratoren des Abonnements und verfügen über sämtliche Berechtigungen für das Abonnement (außer zum Löschen des Abrechnungsbesitzers). 

  Die zusätzlichen Besitzer können über PowerShell verwaltet werden. Weitere Informationen finden Sie unter [Verwalten des Zugriffs mit der RBAC und Azure PowerShell]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell).


## <a name="change-the-billing-owner"></a>Ändern des Abrechnungsbesitzers
Führen Sie das folgende Skript aus, um den Abrechnungsbesitzer eines Benutzerabonnements zu ändern.  Der für die Skriptausführung verwendete Computer muss eine Verbindung mit Azure Stack herstellen und mindestens Version 1.3.0 des Azure Stack PowerShell-Moduls ausführen. Weitere Informationen finden Sie unter [Installieren von Azure Stack PowerShell](azure-stack-powershell-install.md). 

> [!Note]  
>  In einer mehrinstanzenfähigen Azure Stack-Instanz muss sich der neue Besitzer im gleichen Verzeichnis befinden wie der aktuelle Besitzer. Wenn Sie die Besitzerrolle für das Abonnement einem Benutzer zuweisen möchten, der sich in einem anderen Verzeichnis befindet, müssen Sie zuerst den [Benutzer als Gast in Ihr Verzeichnis einladen](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator). 


Ersetzen Sie vor der Skriptausführung die folgenden Werte im Skript: 
 
- **$ArmEndpoint**: Geben Sie den Resource Manager-Endpunkt für Ihre Umgebung an.  
- **$TenantId**: Geben Sie Ihre Mandanten-ID an. 
- **$SubscriptionId**: Geben Sie Ihre Abonnement-ID an.
- **$OwnerUpn**: Geben Sie ein Konto im Format *user@example.com* an, um es als neuen Abrechnungsbesitzer hinzuzufügen.  


```PowerSHell   
# Setup Azure Stack Admin Environment
Add-AzureRmEnvironment -ARMEndpoint $ArmEndpoint -Name AzureStack-admin
Add-AzureRmAccount -Environment AzureStack-admin -TenantId $TenantId

# Select Admin Subscriptionr
$providerSubscriptionId = (Get-AzureRmSubscription -SubscriptionName "Default Provider Subscription").Id
Write-Output "Setting context the Default Provider Subscription: $providerSubscriptionId" 
Set-AzureRmContext -Subscription $providerSubscriptionId

# Change User Subscription owner
$subscription = Get-AzsUserSubscription -SubscriptionId $SubscriptionId
$Subscription.Owner = $OwnerUpn
Set-AzsUserSubscription -InputObject $subscription
```


## <a name="next-steps"></a>Nächste Schritte
[Verwalten der rollenbasierten Zugriffssteuerung](azure-stack-manage-permissions.md)
