---
title: "Konfigurieren des PremiumV2-Tarifs für Azure App Service | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie in Azure App Service durch zentrales Hochskalieren auf den neuen PremiumV2-Tarif die Leistung Ihrer mobilen App, Web-App und API-App optimieren.
keywords: App-Dienst, Azure App Service, Skalierung, skalierbar, App Services-Plan, App Service-Kosten
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: ff00902b-9858-4bee-ab95-d3406018c688
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: cephalin
ms.openlocfilehash: 92cc8d8b0f67dde95ea2e3fc2f0f083bd8ac8aab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="configure-premiumv2-tier-for-azure-app-service"></a>Konfigurieren des PremiumV2-Tarifs für Azure App Service

Der neue **PremiumV2**-Tarif bietet [VMs der Dv2-Serie](../virtual-machines/windows/sizes-general.md#dv2-series) mit schnelleren Prozessoren, SSD-Speicher und einem doppelt so großen Arbeitsspeicher-zu-Kern-Verhältnis gegenüber dem **Standard**-Tarif. In diesem Artikel erfahren Sie, wie Sie eine App im **PremiumV2**-Tarif erstellen oder eine App auf den **PremiumV2**-Tarif zentral hochskalieren.

## <a name="prerequisites"></a>Voraussetzungen

Zum zentralen Hochskalieren einer Web-App auf **PremiumV2** benötigen Sie eine Web-App in Azure App Service, die in einem niedrigeren Tarif als **PremiumV2** ausgeführt wird.

<a name="availability"></a>

## <a name="premiumv2-availability"></a>Verfügbarkeit von PremiumV2

Der PremiumV2-Tarif ist aktuell nur für App Service unter _Windows_ verfügbar. Linux-Container werden momentan noch nicht unterstützt.

PremiumV2 ist bereits in den meisten Azure-Regionen verfügbar und wird zukünftig in weiteren Azure-Regionen zur Verfügung stehen. Um festzustellen, ob der Tarif in Ihrer Azure-Region verfügbar ist, führen Sie den folgenden Azure CLI-Befehl in der [Azure Cloud Shell](../cloud-shell/overview.md) aus:

```azurecli-interactive
az appservice list-locations --sku P1V2
```

Wenn Sie während der Erstellung der App oder des App Service-Plans eine Fehlermeldung erhalten, ist **PremiumV2** wahrscheinlich für die ausgewählte Azure-Region nicht verfügbar.

<a name="create"></a>

## <a name="create-an-app-in-premiumv2-tier"></a>Erstellen einer Anwendung im PremiumV2-Tarif

Der Tarif einer App Service-App wird im [App Service-Plan](azure-web-sites-web-hosting-plans-in-depth-overview.md) definiert, in dem die App ausgeführt wird. Sie können einen eigenständigen App Service-Plan oder einen App Service-Plan im Rahmen der Erstellung der Web-App erstellen.

Wählen Sie beim Konfigurieren des App Service-Plans im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> die Option **Tarif** aus. 

Wählen Sie anschließend eine der **PremiumV2**-Optionen aus, und klicken Sie auf **Auswählen**.

![](media/app-service-configure-premium-tier/pick-premium-tier.png)

> [!IMPORTANT] 
> Wenn Ihnen die Optionen **P1V2**, **P2V2** und **P3V2** nicht angezeigt werden, ist **PremiumV2** entweder nicht in der ausgewählten Azure-Region verfügbar, oder Sie konfigurieren einen App Service-Plan für Linux, der **PremiumV2** nicht unterstützt.

## <a name="scale-up-an-existing-app-to-premiumv2-tier"></a>Zentrales Hochskalieren einer vorhandenen App auf PremiumV2-Tarif

Stellen Sie vor dem zentralen Hochskalieren einer vorhandenen App auf den **PremiumV2**-Tarif sicher, dass **PremiumV2** in Ihrer Azure-Region verfügbar ist. Informationen hierzu finden Sie unter [PremiumV2 availability (Verfügbarkeit von PremiumV2)](#availability). Wenn der Tarif in Ihrer Azure-Region nicht verfügbar ist, finden Sie unter [Scale up from an unsupported region (Zentrales Hochskalieren aus einer nicht unterstützten Azure-Region)](#unsupported) weitere Informationen.

Je nach Hostingumgebung können für das zentrale Hochskalieren zusätzliche Schritte erforderlich sein. 

Öffnen Sie im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> die Seite Ihrer App Service-App.

Wählen Sie im linken Navigationsbereich **Zentral hochskalieren (App Service-Plan)** aus.

![](media/app-service-configure-premium-tier/scale-up-tier-portal.png)

Wählen Sie für **PremiumV2** eine der angezeigten Größen aus, und klicken Sie anschließend auf **Auswählen**.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

Wenn der Vorgang erfolgreich abgeschlossen wurde, wird auf der Übersichtsseite Ihrer App anzeigt, dass sich diese im Tarif **PremiumV2** befindet.

![](media/app-service-configure-premium-tier/finished.png)

### <a name="if-you-get-an-error"></a>Hinweis zu möglicher Fehlermeldung

Einige App Service-Pläne können nicht auf den PremiumV2-Tarif zentral hochskaliert werden. Wenn Ihnen beim zentralen Hochskalieren ein Fehler angezeigt wird, benötigen Sie einen neuen App Service-Plan für Ihre App.

Erstellen Sie in der gleichen Azure-Region und Ressourcengruppe wie Ihrer vorhandenen App Service-App einen App Service-Plan für _Windows_. Führen Sie die Schritte im Abschnitt [Erstellen einer Anwendung im Tarif PremiumV2](#create) aus, um den Tarif **PremiumV2** für die App festzulegen. Sie können bei Bedarf dieselbe Konfiguration für horizontales Hochskalieren nutzen (Anzahl der Instanzen, automatische Skalierung etc.), die in Ihrem vorhandenen App Service-Plan verwendet wird.

Öffnen Sie erneut die Seite Ihrer App Service-App. Wählen Sie in App Service im linken Navigationsbereich **App Service-Plan ändern** aus.

![](media/app-service-configure-premium-tier/change-plan.png)

Wählen Sie den erstellten App Service-Plan aus.

![](media/app-service-configure-premium-tier/select-plan.png)

Nachdem der Vorgang abgeschlossen wurde, wird Ihre App im **PremiumV2**-Tarif ausgeführt.

<a name="unsupported"></a>

## <a name="scale-up-from-an-unsupported-region"></a>Zentrales Hochskalieren aus einer nicht unterstützten Azure-Region

Wenn Ihre App in einer Azure-Region ausgeführt wird, in der **PremiumV2** noch nicht verfügbar ist, können Sie Ihre App in eine andere Azure-Region verschieben, um **PremiumV2** zu nutzen. Sie haben zwei Möglichkeiten:

- Erstellen Sie eine App im neuen **PremiumV2**-Plan, und stellen Sie anschließend Ihren Anwendungscode erneut bereit. Führen Sie hierzu die Schritte im Abschnitt [Erstellen einer Anwendung im Tarif PremiumV2](#create) aus, um den **PremiumV2**-Tarif für die App festzulegen. Sie können bei Bedarf dieselbe Konfiguration für horizontales Hochskalieren nutzen (Anzahl der Instanzen, automatische Skalierung etc.), die in Ihrem vorhandenen App Service-Plan verwendet wird.
- Wenn Ihre App bereits in einem vorhandenen **Premium**-Tarif ausgeführt wird, können Sie Ihre App mit allen App-Einstellungen, Verbindungszeichenfolgen und der Bereitstellungskonfiguration klonen.

    ![](media/app-service-configure-premium-tier/clone-app.png)

    Auf der Seite **App klonen** können Sie einen neuen App Service-Plan in der gewünschten Azure-Region erstellen. Außerdem können Sie die Einstellungen festlegen, die geklont werden sollen.

## <a name="automate-with-scripts"></a>Automatisieren mit Skripts

Mit **Azure CLI** oder [Azure PowerShell](/cli/azure/install-azure-cli) können Sie die Erstellung von Apps im [PremiumV2](/powershell/azure/overview)-Tarif mithilfe von Skripts automatisieren.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Durch den folgenden Befehl wird ein App Service-Plan in _P1V2_ erstellt. Den Befehl können Sie in der Cloud Shell ausführen. Für `--sku` sind die Optionen P1V2, _P2V2_ und _P3V2_ verfügbar.

```azurecli-interactive
az appservice plan create \
    --resource-group <resource_group_name> \
    --name <app_service_plan_name> \
    --sku P1V2
```

### <a name="azure-powershell"></a>Azure PowerShell

Durch den folgenden Befehl wird ein App Service-Plan in _P1V2_ erstellt. Für `-WorkerSize` sind die Optionen _Small_, _Medium_ und _Large_ verfügbar.

```PowerShell
New-AzureRmAppServicePlan -ResourceGroupName <resource_group_name> `
    -Name <app_service_plan_name> `
    -Location <region_name> `
    -Tier "PremiumV2" `
    -WorkerSize "Small"
```
## <a name="more-resources"></a>Weitere Ressourcen

[Zentrales Hochskalieren einer App in Azure](web-sites-scale.md)  
[Manuelles oder automatisches Skalieren der Instanzenzahl](../monitoring-and-diagnostics/insights-how-to-scale.md)