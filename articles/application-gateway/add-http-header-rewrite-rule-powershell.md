---
title: Erneutes Generieren von HTTP-Headern in Azure Application Gateway
description: Dieser Artikel enthält Informationen zum erneuten Generieren von HTTP-Headern in Azure Application Gateway mithilfe von Azure PowerShell
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/12/2019
ms.author: absha
ms.openlocfilehash: 405bc9aed4605e9728e112595f33c879bf55ec7f
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60005620"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-powershell"></a>Erneutes Generieren von HTTP-Anforderungs- und Antwortheadern mit Azure Application Gateway – Azure PowerShell

In diesem Artikel erfahren Sie, wie Sie mithilfe von Azure PowerShell eine [Application Gateway v2-SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) konfigurieren, um die HTTP-Header in den Anforderungen und Antworten erneut zu generieren.

> [!IMPORTANT]
> Die Application Gateway-SKU mit automatischer Skalierung und Zonenredundanz ist derzeit als öffentliche Vorschau verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- Für dieses Tutorial müssen Sie Azure PowerShell lokal ausführen. Sie müssen Version 1.0.0 oder höher des Az-Moduls installiert haben. Führen Sie `Import-Module Az` und dann `Get-Module Az` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps) Informationen dazu. Führen Sie nach dem Überprüfen der PowerShell-Version `Login-AzAccount` aus, um eine Verbindung mit Azure zu erstellen.
- Sie benötigen eine Application Gateway v2-SKU, da die Funktion zum erneuten Generieren von Headern für die v1-SKU nicht unterstützt wird. Wenn Sie nicht über die v2-SKU verfügen, erstellen Sie eine [Application Gateway v2-SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps), bevor Sie beginnen.

## <a name="what-is-required-to-rewrite-a-header"></a>Voraussetzungen zum erneuten Generieren eines Headers

Folgendes ist erforderlich, um das erneute Generieren eines HTTP-Headers zu konfigurieren:

1. Erstellen Sie die neuen Objekte, die benötigt werden, um die HTTP-Header neu zu generieren:

   - **RequestHeaderConfiguration:** Dieses Objekt wird dazu verwendet, die Anforderungsheaderfelder zu bestimmen, die Sie neu generieren möchten, sowie den neuen Wert, den die ursprünglichen Header benötigen, um neu generiert werden zu können.

   - **ResponseHeaderConfiguration:** Dieses Objekt wird dazu verwendet, die Antwortheaderfelder, die Sie neu generieren möchten, und den neuen Wert zu bestimmen, den die ursprünglichen Header benötigen, um neu generiert werden zu können.

   - **ActionSet:** Dieses Objekt beinhaltet die Konfigurationen der Anforderungs- und Antwortheader, die oben bestimmt wurden.

   - **Bedingung:** Dies ist eine optionale Konfiguration. Wenn eine Bedingung für das erneute Generieren hinzugefügt wird, wertet sie den Inhalt der HTTP(S)-Anforderungen und -Antworten aus. Die Entscheidung, die mit der Bedingung für das erneute Generieren verbundene Aktion zum erneuten Generieren auszuführen, hängt davon ab, ob die HTTP(S)-Anforderung oder -Antwort mit der Bedingung für das erneute Generieren übereinstimmt. 

     Wenn einer Aktion mehr als eine Bedingung zugeordnet ist, wird die Aktion nur ausgeführt, wenn alle Bedingungen erfüllt sind, d. h. es wird eine logische UND-Operation durchgeführt.

   - **RewriteRule**: enthält mehrere Kombinationen aus Aktion und Bedingung zum erneuten Generieren.

   - **RuleSequence**: Dies ist eine optionale Konfiguration. Sie hilft, die Reihenfolge zu bestimmen, in der die verschiedenen Regeln zum erneuten Generieren ausgeführt werden. Dies ist hilfreich, wenn es mehrere Regeln zum erneuten Generieren in einem Satz zum erneuten Generieren gibt. Die Regel zum erneuten Generieren mit einem niedrigeren Wert für die Regelreihenfolge wird zuerst ausgeführt. Wenn Sie dieselbe Regelreihenfolge für zwei Regeln zum erneuten Generieren festlegen, ist die Reihenfolge der Ausführung unbestimmt.

     Wenn Sie die RuleSequence nicht explizit angeben, wird der Standardwert 100 festgelegt.

   - **RewriteRuleSet**: Dieses Objekt enthält mehrere Regeln zum erneuten Generieren, die einer Anforderungsroutingregel zugeordnet werden.

2. Sie müssen das rewriteRuleSet an eine Routingregel anfügen. Dies liegt daran, dass die erstellte Konfiguration der erneuten Generierung anhand der Routingregel an den Quelllistener angefügt wird. Bei Verwendung einer einfachen Routingregel wird die Konfiguration der erneuten Generierung eines Headers einem Quelllistener zugeordnet und fungiert als erneute Generierung eines globalen Headers. Wenn eine pfadbasierte Routingregel verwendet wird, wird die Konfiguration der erneuten Generierung eines Headers in der URL-Pfadzuordnung definiert. Daher gilt sie nur für den bestimmten Pfadbereich einer Site.

Sie können mehrere solche Sätze für das erneute Generieren eines HTTP-Headers erstellen, und jeder dieser Sätze kann auf mehrere Listener angewendet werden. Allerdings können Sie auf einen bestimmten Listener nur einen Satz für das erneute Generieren anwenden.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>**Angeben Ihrer Regelkonfiguration für das erneute Generieren eines HTTP-Headers**

In diesem Beispiel werden wir die Umleitungs-URL ändern, indem der Adressheader in der HTTP-Antwort immer dann erneut generiert wird, wenn der Adressheader einen Verweis auf „azurewebsites.net“ enthält. Zu diesem Zweck fügen wir durch Verwendung des Musters `(https?):\/\/.*azurewebsites\.net(.*)$` eine Bedingung hinzu, um zu prüfen, ob der Adressheader in der Antwort „azurewebsites.net“ enthält. Wir verwenden `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` als Headerwert. Dadurch wird im Adressheader *azurewebsites.net* durch *contoso.com* ersetzt.

```azurepowershell
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Location" -HeaderValue "{http_resp_Location_1}://contoso.com{http_resp_Location_2}"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$condition = New-AzApplicationGatewayRewriteRuleCondition -Variable "http_resp_Location" -Pattern "(https?):\/\/.*azurewebsites\.net(.*)$" -IgnoreCase
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name LocationHeader -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name LocationHeaderRewrite -RewriteRule $rewriteRule
```

## <a name="retrieve-configuration-of-your-existing-application-gateway"></a>Abrufen der Konfiguration Ihres vorhandenen Application Gateway

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-configuration-of-your-existing-request-routing-rule"></a>Abrufen der Konfiguration Ihrer vorhandenen Routingregel für Anforderungen

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Aktualisieren des Application Gateway mit der Konfiguration zum erneuten Generieren von HTTP-Headern

```azurepowershell
Add-AzApplicationGatewayRewriteRuleSet -ApplicationGateway $appgw -Name LocationHeaderRewrite -RewriteRule $rewriteRuleSet.RewriteRules
Set-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appgw -Name rule1 -RuleType $reqRoutingRule.RuleType -BackendHttpSettingsId $reqRoutingRule.BackendHttpSettings.Id -HttpListenerId $reqRoutingRule.HttpListener.Id -BackendAddressPoolId $reqRoutingRule.BackendAddressPool.Id -RewriteRuleSetId $rewriteRuleSet.Id
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="delete-a-rewrite-rule"></a>Löschen einer Rewriteregel

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
Remove-AzApplicationGatewayRewriteRuleSet -Name "LocationHeaderRewrite" -ApplicationGateway $appgw
$requestroutingrule= Get-AzApplicationGatewayRequestRoutingRule -Name "rule1" -ApplicationGateway $appgw
$requestroutingrule.RewriteRuleSet= $null
set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Konfiguration, die erforderlich ist, um einige der häufigsten Anwendungsfälle durchzuführen, finden Sie unter [Allgemeine Szenarien zum erneuten Generieren von Headern](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).