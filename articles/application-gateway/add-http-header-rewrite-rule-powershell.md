---
title: Erneutes Generieren von HTTP-Headern in einem vorhandenen Azure Application Gateway
description: Dieser Artikel enthält Informationen zum erneuten Generieren von HTTP-Headern in einem vorhandenen Azure Application Gateway mithilfe von Azure PowerShell.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: 68da63bcad3c670c5e8bda62dda656e29c41f899
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55692917"
---
# <a name="rewrite-http-headers-in-an-existing-application-gateway"></a>Erneutes Generieren von HTTP-Headern in einem vorhandenen Application Gateway

Sie können Azure PowerShell verwenden, um [Regeln zum erneuten Generieren von HTTP-Anforderungs- und -Antwort-Headern](rewrite-http-headers.md) in einer vorhandenen [zonenredundante Application Gateway-SKU mit automatischer Skalierung](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) zu konfigurieren.

> [!IMPORTANT]
> Die Application Gateway-SKU mit automatischer Skalierung und Zonenredundanz ist derzeit als öffentliche Vorschau verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Abrufen der Konfiguration eines vorhandenen Application Gateway
> * Angeben Ihrer Regelkonfiguration für das erneute Generieren eines HTTP-Headers
> * Aktualisieren des Application Gateway mit der oben stehenden Konfiguration zum erneuten Generieren von HTTP-Headern

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial müssen Sie Azure PowerShell lokal ausführen. Sie müssen Version 1.0.0 oder höher des Az-Moduls installiert haben. Führen Sie `Import-Module Az` und dann `Get-Module Az` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) Informationen dazu. Führen Sie nach dem Überprüfen der PowerShell-Version `Login-AzAccount` aus, um eine Verbindung mit Azure zu erstellen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>**Angeben Ihrer Regelkonfiguration für das erneute Generieren eines HTTP-Headers**

Konfigurieren Sie die neuen Objekte, die benötigt werden, um die HTTP-Header neu zu generieren:

- **RequestHeaderConfiguration:** Dieses Objekt wird dazu verwendet, die Anforderungsheaderfelder zu bestimmen, die Sie neu generieren möchten, sowie den neuen Wert, den die ursprünglichen Header benötigen, um neu generiert werden zu können.
- **ResponseHeaderConfiguration:** Dieses Objekt wird dazu verwendet, die Antwortheaderfelder, die Sie neu generieren möchten, und den neuen Wert zu bestimmen, den die ursprünglichen Header benötigen, um neu generiert werden zu können.
- **ActionSet:** Dieses Objekt beinhaltet die Konfigurationen der Anforderungs- und Antwortheader, die oben bestimmt wurden.
- **RewriteRule:** Dieses Objekt beinhaltet alle *actionSets*, die oben bestimmt wurden.
- **RewriteRuleSet:** Dieses Objekt beinhaltet alle *RewriteRules* und muss an die Routingregel einer Anforderung angefügt werden („Basic“ oder „pfadbasiert“).

```azurepowershell
$requestHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-isThroughProxy" -HeaderValue "True"
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Strict-Transport-Security" -HeaderValue "max-age=31536000"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name rewriteRule1 -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name rewriteRuleSet1 -RewriteRule $rewriteRule
```

## <a name="retrieve-configuration-of-your-existing-application-gateway"></a>Abrufen der Konfiguration Ihres vorhandenen Application Gateway

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-configuration-of-your-existing-request-routing-rule"></a>Abrufen der Konfiguration Ihrer vorhandenen Routingregel für Anforderungen

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name Rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Aktualisieren des Application Gateway mit der Konfiguration zum erneuten Generieren von HTTP-Headern

```azurepowershell
Add-AzApplicationGatewayRewriteRuleSet -ApplicationGateway $appgw -Name rewriteRuleSet1 -RewriteRule $rewriteRuleSet.RewriteRules
Set-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appgw -Name rule1 -RuleType $reqRoutingRule.RuleType -BackendHttpSettingsId $reqRoutingRule.BackendHttpSettings.Id -HttpListenerId $reqRoutingRule.HttpListener.Id -BackendAddressPoolId $reqRoutingRule.BackendAddressPool.Id -RewriteRuleSetId $rewriteRuleSet.Id
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="delete-a-rewrite-rule"></a>Löschen einer Rewriteregel

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
Remove-AzApplicationGatewayRewriteRuleSet -Name "rewriteRuleSet1" -ApplicationGateway $appgw
$requestroutingrule= Get-AzApplicationGatewayRequestRoutingRule -Name "rule1" -ApplicationGateway $appgw
$requestroutingrule.RewriteRuleSet= $null
set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen eines Anwendungsgateways mit Routingregeln auf URL-Pfadbasis](./tutorial-url-route-powershell.md)
