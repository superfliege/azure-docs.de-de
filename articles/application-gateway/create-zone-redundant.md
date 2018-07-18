---
title: Erstellen eines zonenredundanten Azure-Anwendungsgateways
description: Hier erfahren Sie, wie Sie ein zonenredundantes Anwendungsgateway erstellen, das kein separates Gateway in jeder Zone erfordert.
services: application-gateway
author: amsriva
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 5/8/2018
ms.author: victorh
ms.openlocfilehash: 03bc58db813534fdd17c9567f6425ab7357ed901
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33937703"
---
# <a name="create-a-zone-redundant-azure-application-gateway---private-preview"></a>Erstellen eines zonenredundanten Azure-Anwendungsgateways (private Vorschau)

Die zonenredundante Anwendungsgatewayplattform ist eine neue SKU, die gegenüber der bereits vorhandenen Anwendungsgateway-SKU unter anderem folgende Vorteile bietet:
- **Zonenresilienz:** Eine Anwendungsgatewaybereitstellung kann sich jetzt über mehrere Verfügbarkeitszonen erstrecken, sodass nicht mehr in jeder Zone mit einem Datenverkehrs-Manager separate Anwendungsgatewayinstanzen bereitgestellt und fixiert werden müssen. Sie können eine einzelne Zone oder mehrere Zonen mit bereitgestellten Anwendungsgatewayinstanzen auswählen, um die Resilienz bei einem Zonenausfall zu gewährleisten. Der Back-End-Pool für Anwendungen kann auf ähnliche Weise auf Verfügbarkeitszonen verteilt werden.
- **Leistungsverbesserungen**
- **Statische VIP:** Die Anwendungsgateway-VIP unterstützt nun exklusiv den statischen VIP-Typ. Dadurch wird sichergestellt, dass die dem Anwendungsgateway zugeordnete VIP nach einem Neustart unverändert bleibt.
- **Key Vault-Integration für SSL-Kundenzertifikate**
- **Schnellere Bereitstellung und Aktualisierung**

> [!NOTE]
> Das zonenredundante Anwendungsgateway ist momentan als private Vorschau verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="supported-regions"></a>Unterstützte Regionen

Zonenredundante Anwendungsgateways werden derzeit in der Region „USA, Osten 2“ unterstützt. Weitere Regionen kommen in Kürze hinzu.

## <a name="topologies"></a>Topologien

Mit dem aktuellen Release müssen Sie keine Anwendungsgateways mit Zonenfixierung mehr erstellen, um Zonenredundanz zu erhalten. Eine Anwendungsgatewaybereitstellung kann sich über mehrere Zonen erstrecken.

Für die Verteilung auf alle drei Zonen sind mindestens drei Instanzen erforderlich. Das Anwendungsgateway verteilt Instanzen auf Zonen, wenn weitere Instanzen hinzugefügt werden.

Bislang sah eine zonenredundante Topologie wie im folgenden Diagramm aus:

![Alte redundante Topologie](media/create-zone-redundant/old-redundant.png)

Die neue zonenredundante Topologie sieht wie im folgenden Diagramm aus:

![Neue zonenredundante Topologie](media/create-zone-redundant/new-redundant.png)

## <a name="deployment"></a>Bereitstellung

### <a name="prerequisites"></a>Voraussetzungen

Die zonenredundante Funktion ist derzeit nur als private Vorschau verfügbar. Senden Sie eine E-Mail an appgwxzone@microsoft.com, um in die Whitelist aufgenommen zu werden. Nach Erhalt einer Bestätigung können Sie mit den nächsten Schritten fortfahren. Geben Sie in Ihrer E-Mail zur Aufnahme in die Whitelist Folgendes an:

- Abonnement-ID
- Regionsname
- Ungefähre Anzahl der erforderlichen Anwendungsgateways

### <a name="resource-manager-template-deployment"></a>Resource Manager-Vorlagenbereitstellung

1. Vergewissern Sie sich, dass das verwendete Abonnement in der Whitelist enthalten ist, wie weiter oben erwähnt.
2. Melden Sie sich nach Erhalt einer Bestätigung beim Azure-Konto an, und wählen Sie bei Bedarf das passende Abonnement aus. Achten Sie darauf, das Abonnement auszuwählen, das in die Whitelist aufgenommen wurde.

   ```PowerShell
   Login-AzureRmAccount

   Select-AzureRmSubscription -Subscription "<whitelisted subscription name>”
   ```
3. Erstellen einer neuen Ressourcengruppe

   ``` PowerShell
   New-AzureRmResourceGroup -Name <resource group name> -Location "East US 2"
   ```
4. Laden Sie die Vorlagen von [GitHub](https://github.com/amitsriva/CrossZonePreview) herunter, und notieren Sie sich den Speicherort.
5. Erstellen Sie in der erstellten Ressourcengruppe eine neue Bereitstellung. Passen Sie die Vorlage und die Parameterdatei vor der Bereitstellung entsprechend an. 

   Das folgende Diagramm zeigt, wo Sie im Azure-Portal die Mandanten-ID ermitteln können:

   ![Mandanten-ID aus dem Portal](media/create-zone-redundant/tenant-id.png)

Die Vorlage erstellt die folgenden Ressourcen:

- **Vom Benutzer zugewiesene Identität:** Dient dazu, Anwendungsgatewayinstanzen den Zugriff auf den Schlüsseltresor sowie den Abruf der vom Benutzer gespeicherten Zertifikate zu ermöglichen.
- **KeyVault:** Die Key Vault-Instanz, in der das Zertifikat des Benutzers gespeichert ist. Hierbei kann es sich auch um eine bereits vorhandene Key Vault-Instanz handeln.
- **Geheimnis:** Der private Schlüssel, der in der Key Vault-Instanz gespeichert ist.
- **Zugriffsrichtlinie:** Eine auf die Key Vault-Instanz angewendete Zugriffsrichtlinie, die unter Verwendung der vom Benutzer zugewiesenen Identität eine Berechtigung erteilt, damit Anwendungsgatewaybereitstellungen Benutzerzertifikate abrufen können.
- **Öffentliche IP-Adresse:** Eine reservierte IP-Adresse für den Zugriff auf das Anwendungsgateway. Diese IP-Adresse bleibt während des Lebenszyklus des Anwendungsgateways unverändert.
- **Netzwerksicherheitsgruppen:** Eine automatisch erstellte NSG für das Subnetz des Anwendungsgateways, die Portdatenverkehr für den konfigurierten Listener zulässt. Diese NSG wird explizit in der neuen SKU erstellt und verwaltet. (In der vorherigen SKU war sie implizit.)
- **Virtuelles Netzwerk:** Das VNet, in dem das Anwendungsgateway und die Anwendungen bereitgestellt werden.
- **Anwendungsgateway:** Erstellt ein Anwendungsgateway mit Instanzen in den erforderlichen Zonen. Standardmäßig sind alle Zonen (1, 2, 3) ausgewählt. Der SKU-Name wird in *Standard_v2* geändert. Dieser SKU-Name ändert sich möglicherweise noch. Die Mindest- und Maximalwerte der Konfiguration der automatischen Skalierung sind aktuell auf die erforderliche Anzahl von Instanzen festgelegt. Dies kann nach der Aktivierung der automatischen Skalierung angepasst werden.

```PowerShell
New-AzureRmResourceGroupDeployment -Name Deployment1 -ResourceGroupName AmitVMSSLinuxTest9 -TemplateFile <complete path to template.json> -TemplateParameterFile <complete path to parameters.json>
```
### <a name="powershell-deployment"></a>PowerShell-Bereitstellung

1. Vergewissern Sie sich, dass das verwendete Abonnement in der Whitelist enthalten ist, wie weiter oben in den Voraussetzungen erwähnt.
2. Laden Sie die private PowerShell-MSI-Datei von [GitHub](https://github.com/amitsriva/CrossZonePreview/blob/master/Azure-Cmdlets-5.7.0.19009-x64.msi) herunter, und installieren Sie sie.
3. Laden Sie die private PowerShell-ZIP-Datei von dem Ort herunter, der in der Bestätigungs-E-Mail für die Vorschauregistrierung angegeben ist. Entzippen Sie die Datei auf Ihrem Laufwerk, und notieren Sie sich den Speicherort.
4. Laden Sie nach dem Aktivieren der Vorschau zunächst die Vorschaumodule, bevor Sie sich bei Ihrem Konto anmelden:

   ```PowerShell
   $azurePSPath = "<complete path to Azure-PowerShell folder>"
   import-module "$azurePSPath\AzureRM.Profile\AzureRM.Profile.psd1"
   import-module "$azurePSPath\Azure.Storage\Azure.Storage.psd1"
   import-module "$azurePSPath\AzureRM.Resources\AzureRM.Resources.psd1"
   import-module "$azurePSPath\AzureRM.Network\AzureRM.Network.psd1"
   import-module "$azurePSPath\AzureRM.KeyVault\AzureRM.KeyVault.psd1"
   ```

4. Melden Sie sich beim Azure-Konto an, und wählen Sie bei Bedarf das gewünschte Abonnement aus. Achten Sie darauf, das Abonnement auszuwählen, das in die Whitelist aufgenommen wurde.
5. Führen Sie die folgenden Befehle aus, um allgemeine Konstanten einzurichten, die Namen für die meisten der zu erstellenden Entitäten enthalten. 

   Passen Sie die Einträge ggf. an Ihre Benennungskonvention an.

   ```PowerShell
   $location = "eastus2"
   $version = "300"

   $rgname = "RG_A_$version"
   $appgwName = "AGW_A_$version"
   $vaultName = "KVA$version"
   $userAssignedIdentityName = "UI_A_$version"
   $certificateName = "KVCA$version"
   $nsgName = "NSG_A_$version"
   $vnetName = "VN_A_$version"
   $gwSubnetName = "SN_A_$version"
   $gipconfigname = "GC_A_$version"
   $publicIpName = "PIP_A_$version"
   $fipconfig01Name = "FC_A_$version"
   $poolName = "BP_A_$version"
   $frontendPort01Name = "FP1_A_$version"
   $frontendPort02Name = "FP2_A_$version"
   $poolSetting01Name = "BS_A_$version"
   $listener01Name = "HL1_A_$version"
   $listener02Name = "HL2_A_$version"
   $rule01Name = "RR1_A_$version"
   $rule02Name = "RR2_A_$version"
   $AddressPrefix = "111.111.222.0" 
   ```
6. Erstellen Sie die Ressourcengruppe:

   ```PowerShell
   $resourceGroup = New-AzureRmResourceGroup -Name $rgname -Location $location -Force
   ```
7. Erstellen Sie die vom Benutzer zugewiesene Identität, mit der dem Anwendungsgateway Zugriff für den Abruf von Zertifikaten aus der Key Vault-Instanz gewährt wird.

   ```PowerShell
   $userAssignedIdentity = New-AzureRmResource -ResourceGroupName $rgname -Location $location -ResourceName $userAssignedIdentityName -ResourceType Microsoft.ManagedIdentity/userAssignedIdentities -Force
   ```
8. Erstellen Sie die Key Vault-Instanz für die Speicherung Ihrer Zertifikate:

   ```PowerShell
   $keyVault = New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgname -Location $location -EnableSoftDelete
   ```
9. Laden Sie das Zertifikat als Geheimnis in Key Vault hoch:

   ```PowerShell
   $securepfxpwd = ConvertTo-SecureString -String "<password>" -AsPlainText -Force

   $cert = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name         $certificateName -FilePath ‘<path to pfx file>'  -Password $securepfxpwd
   ```
10. Weisen Sie der Key Vault-Instanz die Zugriffsrichtlinie unter Verwendung der vom Benutzer zugewiesenen Identität zu. Dadurch können die Anwendungsgatewayinstanzen auf das Key Vault-Geheimnis zugreifen:

   ```PowerShell
   Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $rgname -PermissionsToSecrets get -ObjectId $userAssignedIdentity.Properties.principalId
   ```
11. Erstellen Sie die Netzwerksicherheitsgruppe (NSG), um den Zugriff auf das Anwendungsgateway-Subnetz an Ports zuzulassen, an denen neue Listener erstellt werden.

    Beispiel: Für HTTP/HTTPS an Standardports lässt die NSG an 80, 443 sowie an 65200–65535 Zugriff in eingehender Richtung zu, um Verwaltungsvorgänge zu ermöglichen.

   ```PowerShell
   $srule01 = New-AzureRmNetworkSecurityRuleConfig -Name "listeners" -Direction Inbound -SourceAddressPrefix * -SourcePortRange * -Protocol * -DestinationAddressPrefix * -DestinationPortRange 22,80,443 -Access Allow -Priority 100

   $srule02 = New-AzureRmNetworkSecurityRuleConfig -Name "managementPorts" -Direction Inbound -SourceAddressPrefix * -SourcePortRange * -Protocol * -DestinationAddressPrefix * -DestinationPortRange "65200-65535" -Access Allow -Priority 101

   $nsg = New-AzureRmNetworkSecurityGroup -Name $nsgName -ResourceGroupName $rgname -Location $location -SecurityRules $srule01,$srule02 -Force
   ```

12. Erstellen Sie das VNet und die Subnetze:

   ```PowerShell
   $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 
   $gwSubnetName -AddressPrefix "$AddressPrefix/24" -NetworkSecurityGroup $nsg

   $vnet = New-AzureRmvirtualNetwork -Name $vnetName -ResourceGroupName $rgname -Location $location -AddressPrefix "$AddressPrefix/24" -Subnet $gwSubnet -Force
   ```
13. Erstellen Sie eine reservierte/statische öffentliche IP-Adresse:

   ```PowerShell
   $publicip = New-AzureRmPublicIpAddress -ResourceGroupName $rgname -name $publicIpName -location $location -AllocationMethod Static -Sku Standard -Force
   ```

14. Erstellen Sie das Anwendungsgateway:

   ```PowerShell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name $gipconfigname -Subnet $gwSubnet

   $fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name $fipconfig01Name -PublicIPAddress $publicip

   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name $poolName -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

   $fp01 = New-AzureRmApplicationGatewayFrontendPort -Name $frontendPort01Name -Port 443

   $fp02 = New-AzureRmApplicationGatewayFrontendPort -Name $frontendPort02Name -Port 80

   $sslCert01 = New-AzureRmApplicationGatewaySslCertificate -Name "SSLCert" -KeyVaultSecretId $secret.Id

   $listener01 = New-AzureRmApplicationGatewayHttpListener -Name $listener01Name -Protocol Https -FrontendIPConfiguration
 $fipconfig01 -FrontendPort $fp01 -SslCertificate $sslCert01

   $listener02 = New-AzureRmApplicationGatewayHttpListener -Name $listener02Name -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp02

   $poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name $poolSetting01Name -Port 80 -Protocol Http -CookieBasedAffinity Disabled

   $rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name $rule01Name -RuleType basic -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool

   $rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name $rule02Name -RuleType basic -BackendHttpSettings $poolSetting01 -HttpListener $listener02 -BackendAddressPool $pool

   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2

   $listeners = @($listener02)

   $fps = @($fp01, $fp02)

   $fipconfigs = @($fipconfig01)

   $sslCerts = @($sslCert01)

   $rules = @($rule01, $rule02)

   $listeners = @($listener01, $listener02)

   $appgw = New-AzureRmApplicationGateway -Name $appgwName -ResourceGroupName $rgname -Location $location -UserAssignedIdentityId $userAssignedIdentity.ResourceId -Probes $probeHttps -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfigs -FrontendPorts $fps -HttpListeners $listeners -RequestRoutingRules $rules -Sku $sku -SslPolicy $sslPolicy -sslCertificates $sslCerts -Force
   ```

15. Rufen Sie die öffentliche IP-Adresse des erstellten Anwendungsgateways ab:

   ```PowerShell
   $pip = Get-AzureRmPublicIpAddress -Name $publicIpName -ResourceGroupName $rgname $pip.IpAddress
   ```

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

-  Fallen während der Vorschauphase Kosten für das Anwendungsgateway an?

   Während der Vorschauphase fallen keine Kosten für das Anwendungsgateway an. Andere Ressourcen (Key Vault, virtuelle Computer und Ähnliches) werden Ihnen jedoch in Rechnung gestellt.
- In welchen Regionen ist die Vorschau verfügbar?

   Die Vorschau ist derzeit in der Region „USA, Osten 2“ verfügbar. Weitere Regionen kommen in Kürze hinzu.
- Wird in der Vorschau das Portal unterstützt?

   Nein. Während der privaten Vorschau beschränkt sich die Unterstützung auf ein privates PowerShell-Modul und auf eine Resource Manager-Vorlage.

- Werden während der privaten Vorschau Produktionsworkloads unterstützt?

   Nein. Während der privaten Vorschau wird keine SLA und kein Support bereitgestellt. Es wird davon abgeraten, während der Vorschau eine Produktionsworkload zu verwenden. Der Support ist auf die direkte Interaktion mit der Produktgruppe unter dem E-Mail-Alias für die Vorschau beschränkt.

- Wie kann ich Probleme melden?

   Die private Vorschau enthält möglicherweise Fehler und geht ggf. mit häufigen Codebereitstellungen einher. Verwenden Sie den Supportalias appgwxzone@microsoft.com, wenn Sie Probleme melden möchten oder Unterstützung benötigen.

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen


|Problem  |Details  |
|---------|---------|---------|
|Abrechnung     |Derzeit findet keine Abrechnung statt.|
|Diagnoseprotokolle (keine Metriken)     |Momentan werden keine Leistungs- und Anforderung/Antwort-Protokolle angezeigt.|
|Portal/CLI/SDK     |Keine Portal-, CLI- oder SDK-Unterstützung. Aktualisierungen für Vorschaugateways dürfen nicht über das Portal angewendet werden.|
|Bei der vorlagenbasierten Aktualisierung tritt bisweilen ein Fehler auf.     |Dies ist auf die Racebedingung mit KeyVault-Zugriffsrichtlinie zurückzuführen.|Nach der Erstellung der Key Vault-Instanz und der vom Benutzer zugewiesenen Identität kann sie aus der Vorlage entfernt werden. In der Vorlage sind dann nur noch Verweise auf das Geheimnis und auf die Identität erforderlich.|
|Automatische Skalierung     |Die automatische Skalierung wird derzeit nicht unterstützt.|
|WAF     |Die WAF wird derzeit nicht unterstützt.|
|Vom Benutzer bereitgestellte Zertifikate und dynamische VIPs     |Diese werden im neuen Modell nicht unterstützt. Speichern Sie Zertifikate und statische VIPs in Key Vault.|
|Gleiches Subnetz für die alte Version und die Vorschauversion des Anwendungsgateways     |Ein Subnetz mit einem vorhandenen Anwendungsgateway (alte Modell) kann nicht für die private Vorschauversion verwendet werden.|
|HTTP/2, FIPS-Modus, WebSocket, Azure Web Apps als Back-End     |Derzeit keine Unterstützung |


## <a name="support-and-feedback"></a>Support und Feedback

Wenn Sie Unterstützung benötigen oder Feedback senden möchten, wenden Sie sich an appgwxzone@microsoft.com. Die Produktgruppe für das Anwendungsgateway freut sich über Ihr Feedback zu Verbesserungen und hilft Ihnen bei Bedarf gerne weiter.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über weitere Features des Anwendungsgateways:

- [Was ist Azure Application Gateway?](overview.md)