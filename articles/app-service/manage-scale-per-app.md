---
title: High Density-Hosting mit Skalierung pro App – Azure App Service | Microsoft-Dokumentation
description: High Density-Hosting in Azure App Service
author: btardif
manager: erikre
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/22/2018
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: 08d6d0c31e1cff799e952c50bae3446e41477aba
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56104568"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>High Density-Hosting in Azure App Service mit Skalierung pro App

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Standardmäßig skalieren Sie App Service-Apps durch Skalierung des [App Service-Plans](overview-hosting-plans.md), in dem sie ausgeführt werden. Wenn sich mehrere Apps im gleichen App Service-Plan befinden, führt jede horizontal hochskalierte Instanz alle Apps im Plan aus.

Sie können die *Skalierung pro App* auf Ebene des App Service-Plans aktivieren. Hierbei wird eine App unabhängig von dem App Service-Plan skaliert, in dem sie gehostet wird. Auf diese Weise kann ein App Service-Plan auf 10 Instanzen skaliert werden. Jedoch kann eine App nur auf das Verwenden von fünf Instanzen festgelegt werden.

> [!NOTE]
> Die Skalierung pro App steht nur für die Tarife **Standard**, **Premium**, **Premium V2** und **Isolated** zur Verfügung.
>

## <a name="per-app-scaling-using-powershell"></a>Skalierung pro App mit PowerShell

Erstellen Sie einen Plan, der für die Skalierung pro App konfiguriert ist, indem Sie den ```-PerSiteScaling $true```-Parameter an das ```New-AzAppServicePlan```-Cmdlet übergeben.

```powershell
New-AzAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Aktivieren Sie die Skalierung pro App bei vorhandenem App Service-Plan, indem Sie den `-PerSiteScaling $true`-Parameter an das ```Set-AzAppServicePlan```-Cmdlet übergeben.

```powershell
# Enable per-app scaling for the App Service Plan using the "PerSiteScaling" parameter.
Set-AzAppServicePlan -ResourceGroupName $ResourceGroup `
   -Name $AppServicePlan -PerSiteScaling $true
```

Konfigurieren Sie auf App-Ebene die Anzahl der Instanzen, die von der App im App Service-Plan genutzt werden können.

Im folgenden Beispiel ist die App auf zwei Instanzen beschränkt, und zwar unabhängig davon, auf wie viele Instanzen der zugrunde liegenden App Service-Plan horizontal hochskaliert wird.

```powershell
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzWebApp $newapp
```

> [!IMPORTANT]
> `$newapp.SiteConfig.NumberOfWorkers` funktioniert anders als `$newapp.MaxNumberOfWorkers`. Die Skalierung pro App verwendet `$newapp.SiteConfig.NumberOfWorkers`, um die Skalierungsmerkmale der App zu ermitteln.

## <a name="per-app-scaling-using-azure-resource-manager"></a>Skalierung pro App mit Azure Resource Manager

Diese Azure Resource Manager-Vorlage erstellt Folgendes:

- Einen App Service-Plan, der bis zu 10 Instanzen horizontal hochskaliert wird
- Eine App, die für die Skalierung auf maximal fünf Instanzen konfiguriert wird

Im App Service-Plan wird die **PerSiteScaling**-Eigenschaft auf TRUE festgelegt (`"perSiteScaling": true`). Für die App wird die **Anzahl der Worker** auf 5 festgelegt (`"properties": { "numberOfWorkers": "5" }`).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "West US",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "West US",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "West US",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a>Empfohlene Konfiguration für High Density-Hosting
Bei der Skalierung pro App handelt es sich um ein Feature, das sowohl in globalen Azure-Regionen als auch in [App Service-Umgebungen](environment/app-service-app-service-environment-intro.md) aktiviert ist. Es wird jedoch empfohlen, App Service-Umgebungen mit ihren hoch entwickelten Features und größeren Kapazitätspools zu nutzen.  

Führen Sie zum Konfigurieren des High Density-Hosting für Ihre Apps die folgenden Schritte aus:

1. Konfigurieren Sie die App Service-Umgebung, und wählen Sie einen Workerpool aus, der ausschließlich für das Szenario mit High Density-Hosting genutzt wird.
2. Erstellen Sie einen einzelnen App Service-Plan, und skalieren Sie ihn so, dass die gesamte verfügbare Kapazität für den Workerpool verwendet wird.
3. Legen Sie im App Service-Plan das Flag `PerSiteScaling` auf TRUE fest.
4. Neue Apps werden erstellt und diesem App Service-Plan zugewiesen, wobei die **numberOfWorkers**-Eigenschaft auf **1** festgelegt wird. Durch die Konfiguration ergibt sich die höchstmögliche Dichte für diesen Workerpool.
5. Die Anzahl der Worker kann pro App unabhängig konfiguriert werden, um nach Bedarf zusätzliche Ressourcen zur Verfügung zu stellen. Beispiel: 
    - Bei einer stark ausgelasteten App kann **numberOfWorkers** auf **3** festgelegt werden, damit diese App über mehr Verarbeitungskapazität verfügt. 
    - Bei selten verwendeten Apps kann **numberOfWorkers** auf **1** festgelegt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Azure App Service-Pläne – Detaillierte Übersicht](overview-hosting-plans.md)
- [Einführung in die App Service-Umgebung](environment/app-service-app-service-environment-intro.md)
