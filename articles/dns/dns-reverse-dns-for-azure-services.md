---
title: "Reverse-DNS für Azure-Dienste | Microsoft Docs"
description: "Informationen Sie zum Konfigurieren von reverse-DNS-Lookups für in Azure gehosteten Dienste"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 63701e1ce0c1c6dcf2ce02ebce272b8280395e7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a>Konfigurieren von reverse-DNS für in Azure gehosteten Dienste

In diesem Artikel erläutert, wie reverse-DNS-Lookups für in Azure gehosteten Dienste zu konfigurieren.

Dienste in Azure verwenden, IP-Adressen von Azure zugewiesen und im Besitz von Microsoft. Diese reverse DNS-Einträge (PTR-Einträge) müssen in den entsprechenden Microsoft gehörende reverse DNS-Lookupzonen erstellt werden. Dieser Artikel beschreibt die Vorgehensweise.

Dieses Szenario dürfen nicht mit der Fähigkeit zum verwechselt werden [reverse DNS-Lookupzonen für Ihre zugewiesenen IP-Adressbereiche in Azure DNS-host](dns-reverse-dns-hosting.md). In diesem Fall müssen die IP-Adressbereiche, dargestellt durch die reverse-Lookupzone für Ihre Organisation, in der Regel werden von Ihrem Internetdienstanbieter zugewiesen.

Bevor Sie diesen Artikel lesen, werden Sie unsere Zuschauer schon [Überblick über das reverse DNS- und -Unterstützung in Azure](dns-reverse-dns-overview.md).

Azure gibt es zwei verschiedene Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Ressourcen-Manager und klassischen](../azure-resource-manager/resource-manager-deployment-model.md).
* Im Ressourcen-Manager-Bereitstellungsmodell werden über eine Ressource PublicIpAddress Serverressourcen (z. B. virtuelle Computer, VM-skalierungsgruppen oder Service Fabric-Cluster) verfügbar gemacht. DNS-Reverse-Lookups werden die Verwendung der die PublicIpAddress-Eigenschaft "ReverseFqdn" konfiguriert.
* In der klassischen Bereitstellungsmodell compute-Ressourcen mithilfe von Cloud-Dienste zur Verfügung gestellt werden. DNS-Reverse-Lookups werden mithilfe der 'ReverseDnsFqdn'-Eigenschaft des Cloud-Diensts konfiguriert.

Reverse-DNS-wird für den Azure-App-Dienst derzeit nicht unterstützt.

## <a name="validation-of-reverse-dns-records"></a>Überprüfung der reverse-DNS-Einträge

Ein Drittanbieter sollten nicht reverse DNS-Einträge für ihre Azure-Dienst-Zuordnung für Ihre DNS-Domänen erstellen können. Um dies zu verhindern, Azure nur ermöglicht die Erstellung eines reverse DNS-Eintrags, in dem im reverse DNS-Eintrag angegebenen Domänennamen identisch ist, oder aufgelöst wird, die DNS-Namen oder die IP-Adresse eines PublicIpAddress oder Cloud-Dienst im gleichen Azure-Abonnement.

Diese Überprüfung wird nur ausgeführt, wenn der reverse-DNS-Eintrag festgelegt oder geändert wird. Regelmäßige erneute Überprüfung wird nicht ausgeführt.

Zum Beispiel: Angenommen, die PublicIpAddress-Ressource, die DNS-Namen contosoapp1.northus.cloudapp.azure.com und IP-Adresse 23.96.52.53 hat. ReverseFqdn für die Adresse kann als angegeben werden:
* Der DNS-Name für die PublicIpAddress contosoapp1.northus.cloudapp.azure.com
* Der DNS-Name für eine andere Adresse im selben Abonnement, z. B. contosoapp2.westus.cloudapp.azure.com
* Eine Vanity DNS-Server ein, z. B. app1.contoso.com, solange dieser Name ist *erste* als einen CNAME-Eintrag contosoapp1.northus.cloudapp.azure.com oder eine andere Adresse im selben Abonnement konfiguriert.
* Eine Vanity DNS-Server ein, z. B. app1.contoso.com, solange dieser Name ist *erste* als einen A-Eintrag, um die IP-Adresse 23.96.52.53 oder der IP-Adresse von einer anderen Adresse im selben Abonnement konfiguriert.

Die gleichen Einschränkungen gelten für die DNS-reverse-für Cloud-Dienste.


## <a name="reverse-dns-for-publicipaddress-resources"></a>Reverse-DNS-PublicIpAddress-Ressourcen

Dieser Abschnitt enthält detaillierte Anweisungen zum Konfigurieren von DNS-reverse-PublicIpAddress-Ressourcen im Ressourcen-Manager-Bereitstellungsmodell mit Azure PowerShell, Azure-CLI 1.0 oder 2.0 für Azure-CLI. Konfigurieren von DNS-reverse-PublicIpAddress Ressourcen wird derzeit über das Azure Portal nicht unterstützt.

Azure umkehren derzeit unterstützt DNS nur für IPv4-Adresse Ressourcen. Es wird für IPv6 nicht unterstützt.

### <a name="add-reverse-dns-to-an-existing-publicipaddresses"></a>Fügen Sie reverse-DNS-hinzu, um einen vorhandenen Load Balancer

#### <a name="powershell"></a>PowerShell

So fügen Sie DNS-reverse-ein vorhandenes PublicIpAddress hinzu

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

Um eine vorhandene PublicIpAddress reverse-DNS-hinzuzufügen, die noch nicht über einen DNS-Namen verfügt, müssen Sie auch einen DNS-Namen angeben:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure-CLI 1.0

So fügen Sie DNS-reverse-ein vorhandenes PublicIpAddress hinzu

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

Um eine vorhandene PublicIpAddress reverse-DNS-hinzuzufügen, die noch nicht über einen DNS-Namen verfügt, müssen Sie auch einen DNS-Namen angeben:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure-CLI 2.0

So fügen Sie DNS-reverse-ein vorhandenes PublicIpAddress hinzu

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

Um eine vorhandene PublicIpAddress reverse-DNS-hinzuzufügen, die noch nicht über einen DNS-Namen verfügt, müssen Sie auch einen DNS-Namen angeben:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a>Erstellen Sie eine öffentliche IP-Adresse mit DNS-reverse-

So erstellen Sie eine neue Adresse mit der reverse DNS-Eigenschaft, die bereits angegeben

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a>Azure-CLI 1.0

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure-CLI 2.0

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a>Ansicht reverse DNS für eine vorhandene PublicIpAddress

Den konfigurierten Wert für eine vorhandene PublicIpAddress anzeigen:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a>Azure-CLI 1.0

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a>Azure-CLI 2.0

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Entfernen von reverse-DNS-aus vorhandenen öffentlichen IP-Adressen

So entfernen eine umgekehrte DNS-Eigenschaft aus einer vorhandenen PublicIpAddress:

#### <a name="powershell"></a>PowerShell

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure-CLI 1.0

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a>Azure-CLI 2.0

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a>Konfigurieren von DNS-reverse-für Cloud-Dienste

Dieser Abschnitt enthält detaillierte Anweisungen zum Konfigurieren von reverse-DNS-für Cloud-Dienste in der klassischen Bereitstellungsmodell mit Azure PowerShell. Konfigurieren von DNS-reverse-für Cloud-Dienste wird über das Azure-Portal, Azure-CLI 1.0 oder 2.0 für Azure-CLI nicht unterstützt.

### <a name="add-reverse-dns-to-existing-cloud-services"></a>Hinzufügen von DNS-reverse-vorhandenen Cloud-Diensten

So fügen Sie eine umgekehrte DNS-Eintrag zu einem vorhandenen Cloud-Dienst hinzu:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a>Erstellen Sie einen Cloud-Dienst mit DNS-reverse-

So erstellen Sie einen neuen Cloud-Dienst mit der reverse DNS-Eigenschaft, die bereits angegeben

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a>Ansicht reverse-DNS für vorhandene Cloud-Dienste

So zeigen Sie die umgekehrte DNS-Eigenschaft für einen vorhandenen Cloud-Dienst an:

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a>Entfernen Sie die DNS-reverse-aus vorhandenen Cloud-Diensten

So entfernen eine umgekehrte DNS-Eigenschaft aus einer vorhandenen Cloud-Dienst:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a>FAQ

### <a name="how-much-do-reverse-dns-records-cost"></a>Wie viel DNS-Datensätze Kosten umkehren?

Sie sind kostenlos!  Es ist ohne zusätzliche Kosten für reverse-DNS-Einträge oder Abfragen.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Werden meine reverse DNS-Einträge werden aus dem Internet aufgelöst?

Ja. Nach die reverse-DNS-Eigenschaft für den Azure-Dienst festlegen verwaltet Azure alle DNS-Delegierungen und DNS-Zonen, die erforderlich sind, um sicherzustellen, dass die reverse-DNS-Eintrag für alle Internet-Benutzer aufgelöst wird.

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a>Werden standardmäßig reverse-DNS-Einträge werden für mein Azure-Dienste erstellt?

Nein. Reverse-DNS-ist eine opt-in-Funktion. Keine Standardeinstellung reverse DNS-Einträge werden erstellt, wenn Sie nicht für deren Konfiguration auswählen.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Was ist das Format für den vollqualifizierten Domänennamen (FQDN)?

FQDNs in normaler Reihenfolge angegeben werden und müssen durch einen Punkt (z. B. "app1.contoso.com.") beendet werden.

### <a name="what-happens-if-the-validation-check-for-the-reverse-dns-ive-specified-fails"></a>Schlägt fehl, was geschieht, wenn die Überprüfung für die DNS-reverse-ich angegeben haben?

Sofern die umgekehrte DNS-Überprüfung ein Fehler auftritt, schlägt der Vorgang so konfigurieren Sie den reverse-DNS-Eintrag fehl. Korrigieren Sie den reverse-DNS-Wert nach Bedarf, und wiederholen.

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a>Kann ich für Azure App Service reverse-DNS-konfigurieren?

Nein. Reverse-DNS-wird für den Azure-App-Dienst nicht unterstützt.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a>Kann ich für mein Azure-Dienst mehrere reverse DNS-Einträge konfigurieren?

Nein. Azure unterstützt einen einzelnen umgekehrten DNS-Datensatz für jede Azure-Cloud-Dienst oder die Adresse an.

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a>Kann ich reverse-DNS für IPv6-Adresse Ressourcen konfigurieren?

Nein. Azure umkehren derzeit unterstützt DNS nur für IPv4-Adresse Ressourcen und Cloud-Dienste.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kann ich e-Mail-Nachrichten mit externen Domänen aus meinem Azure Compute-Dienste senden?

Nein. [Azure-Serverdienste unterstützt sendende e-Mail-Nachrichten mit externen Domänen nicht](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu DNS-reverse-, finden Sie unter [reverse-DNS-Lookup für Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Erfahren Sie, wie [der reverse-Lookupzone für Ihre ISP zugewiesene IP-Adressbereich in Azure DNS-host](dns-reverse-dns-for-azure-services.md).

