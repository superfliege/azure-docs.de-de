---
title: 'Tutorial: Konfigurieren der Geofilterung in einer Domäne für Azure Front Door Service | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine einfache Richtlinie für die Geofilterung erstellen und sie Ihrem vorhandenen Front Door-Front-End-Host zuordnen.
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2018
ms.author: sharadag
ms.openlocfilehash: c4032f7c33cec7b7a7864ccff07a05b87c945949
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988588"
---
# <a name="how-to-set-up-a-geo-filtering-policy-for-your-front-door"></a>Einrichten einer Geofilterungsrichtlinie für Ihre Front Door-Instanz
In diesem Tutorial wird veranschaulicht, wie Sie mit Azure PowerShell eine Beispielrichtlinie für die Geofilterung erstellen und sie Ihrem vorhandenen Front Door-Front-End-Host zuordnen. Diese Beispielrichtlinie für die Geofilterung blockiert Anforderungen aus allen Ländern, mit Ausnahme der USA.

## <a name="1-set-up-your-powershell-environment"></a>1. Einrichten Ihrer PowerShell-Umgebung
Azure PowerShell bietet eine Reihe von Cmdlets, die das [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)-Modell für die Verwaltung von Azure-Ressourcen verwenden. 

Sie können [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) auf Ihrem lokalen Computer installieren und in einer beliebigen PowerShell-Sitzung nutzen. Befolgen Sie die Anweisungen auf der Seite, um sich mit Ihren Azure-Anmeldeinformationen anzumelden, und installieren Sie AzureRM.
```
# Connect to Azure with an interactive dialog for sign-in
Connect-AzureRmAccount
Install-Module -Name AzureRM
```
> [!NOTE]
>  Support für die [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) wird in Kürze verfügbar sein.

Stellen Sie vor dem Installieren des Front Door-Moduls sicher, dass Sie die aktuelle Version von PowerShellGet installiert haben. Führen Sie den folgenden Befehl aus, und öffnen Sie PowerShell erneut.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

Installieren Sie das AzureRM.FrontDoor-Modul. 

```
Install-Module -Name AzureRM.FrontDoor -AllowPrerelease
```

## <a name="2-define-geo-filtering-match-conditions"></a>2. Definieren der Übereinstimmungsbedingung(en) für die Geofilterung
Erstellen Sie zuerst eine Übereinstimmungsbedingung, mit der Anforderungen ausgewählt werden, die nicht von „US“ stammen. Informationen zu Parametern beim Erstellen einer Übereinstimmungsbedingung finden Sie im PowerShell-[Leitfaden](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoormatchconditionobject). Den zweistelligen Ländercode für die Zuordnung des Lands finden Sie [hier](/Protection/GeoFiltering).

```
$nonUSGeoMatchCondition = New-AzureRmFrontDoorMatchConditionObject -MatchVariable RemoteAddr -OperatorProperty GeoMatch -NegateCondition $true -MatchValue "US"
```
 
## <a name="3-add-geo-filtering-match-condition-to-a-rule-with-action-and-priority"></a>3. Hinzufügen der Übereinstimmungsbedingung für die Geofilterung zu einer Regel mit Aktion und Priorität

Erstellen Sie anschließend das CustomRule-Objekt `nonUSBlockRule` basierend auf der Übereinstimmungsbedingung, einer Aktion und einer Priorität.  Ein CustomRule-Objekt kann über mehrere MatchCondition-Elemente verfügen.  In diesem Beispiel ist „Action“ auf „Block“ und „Priority“ auf „1“ (höchste Priorität) festgelegt.

```
$nonUSBlockRule = New-AzureRmFrontDoorCustomRuleObject -Name "geoFilterRule" -RuleType MatchRule -MatchCondition $nonUSGeoMatchCondition -Action Block -Priority 1
```

Informationen zu Parametern beim Erstellen eines CustomRule-Objekts finden Sie im PowerShell-[Leitfaden](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoorcustomruleobject).

## <a name="4-add-rules-to-a-policy"></a>4. Hinzufügen von Regeln zu einer Richtlinie
In diesem Schritt wird ein `geoPolicy`-Richtlinienobjekt mit `nonUSBlockRule` aus dem vorherigen Schritt in der angegebenen Ressourcengruppe erstellt. Verwenden Sie `Get-AzureRmResourceGroup`, um nach „ResourceGroupName $resourceGroup“ zu suchen.

```
$geoPolicy = New-AzureRmFrontDoorFireWallPolicy -Name "geoPolicyAllowUSOnly" -resourceGroupName $resourceGroup -Customrule $nonUSBlockRule  -Mode Prevention -EnabledState Enabled
```

Informationen zu Parametern beim Erstellen einer Richtlinie finden Sie im PowerShell-[Leitfaden](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoorfirewallpolicy).

## <a name="5-link-policy-to-a-front-door-frontend-host"></a>5. Verknüpfen der Richtlinie mit einem Front Door-Front-End-Host
Die letzten Schritte umfassen das Verknüpfen des Schutzrichtlinienobjekts mit einem vorhandenen Front Door-Front-End-Host und das Aktualisieren der Front Door-Eigenschaften. Sie rufen zuerst Ihr Front Door-Objekt ab, indem Sie [Get-AzureRmFrontDoor](https://docs.microsoft.com/azure/frontdoor/get-azurermfrontdoor) verwenden, gefolgt vom Festlegen der WebApplicationFirewallPolicyLink-Eigenschaft für das Front-End auf die resourceId von `geoPolicy`.

```
$geoFrontDoorObjectExample = Get-AzureRmFrontDoor -ResourceGroupName $resourceGroup
$geoFrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $geoPolicy.Id
```

Verwenden Sie den folgenden [Befehl](https://docs.microsoft.com/azure/frontdoor/set-azurermfrontdoor), um Ihr Front Door-Objekt zu aktualisieren.

```
Set-AzureRmFrontDoor -InputObject $geoFrontDoorObjectExample[0]
```

> [!NOTE] 
> Sie müssen die WebApplicationFirewallPolicyLink-Eigenschaft nur einmal festlegen, um eine Schutzrichtlinie mit einem Front Door-Front-End-Host zu verknüpfen. Nachfolgende Richtlinienaktualisierungen werden automatisch auf den Front-End-Host angewendet.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Sicherheit für die Anwendungsschicht mit Front Door](front-door-application-security.md).
- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](quickstart-create-front-door.md).
