---
title: "Rufen Sie Werte für die Authentifizierung der app - Azure SQL-Datenbank | Microsoft Docs"
description: "Erstellen Sie einen Dienstprinzipal für den Zugriff auf SQL-Datenbank aus Code ein."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
tags: 
ms.assetid: b43e43bb-6660-49e6-b069-abde97eb5770
ms.service: sql-database
ms.custom: develop apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/30/2016
ms.author: sstein
ms.openlocfilehash: ec6256e9c5bb0d9c8d15d0f673cea70b3915eb34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Rufen Sie die erforderlichen Werte zum Authentifizieren von einer Anwendung Code SQL-Datenbank zugreifen
Zum Erstellen und Verwalten von SQL-Datenbank aus Code müssen Sie Ihre app in der Azure Active Directory (AAD)-Domäne im Abonnement registrieren, auf dem Azure-Ressourcen erstellt wurden.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Ein Dienstprinzipal erstellt den Zugriff auf Ressourcen von einer Anwendung
Sie müssen das aktuellste [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) installiert ist und ausgeführt. Ausführliche Informationen finden Sie unter [zum Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

Das folgende PowerShell-Skript erstellt die Active Directory (AD)-Anwendung und der Dienstprinzipal, den wir unsere C#-app authentifizieren müssen. Das Skript gibt die Werte für das vorhergehende Beispiel c# benötigten. Ausführliche Informationen finden Sie unter [Verwenden von Azure PowerShell zum Erstellen eines dienstprinzipals den Zugriff auf Ressourcen](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    # Sign in to Azure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Weitere Informationen:
* [Erstellen Sie eine SQL-Datenbank mit c#](sql-database-get-started-csharp.md)
* [Über Azure Active Directory-Authentifizierung eine Verbindung mit SQL-Datenbank](sql-database-aad-authentication.md)

