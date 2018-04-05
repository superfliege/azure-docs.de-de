---
title: Erste Schritte mit privaten Azure DNS-Zonen mithilfe von PowerShell | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine private DNS-Zone und einen DNS-Eintrag in Azure DNS erstellen. Dies ist eine Schritt-für-Schritt-Anleitung zum Erstellen und Verwalten Ihrer ersten privaten DNS-Zone und Ihres ersten DNS-Eintrags mithilfe von Azure PowerShell.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2017
ms.author: kumud
ms.openlocfilehash: d9e5c9b8caa5349fbcda9b71f7524fb9e99b976c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="get-started-with-azure-dns-private-zones-using-powershell"></a>Erste Schritte mit privaten Azure DNS-Zonen mithilfe von PowerShell

In diesem Artikel wird Schritt für Schritt beschrieben, wie Sie Ihre erste private DNS-Zone und Ihren ersten DNS-Eintrag mithilfe von Azure PowerShell erstellen.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt. Um eine private DNS-Zone in Ihrem virtuellen Netzwerk zu veröffentlichen, geben Sie die Liste mit den virtuellen Netzwerken an, für die das Auflösen von Einträgen in der Zone zulässig ist.  Diese werden als „virtuelle Auflösungsnetzwerke“ bezeichnet.  Sie können auch ein virtuelles Netzwerk angeben, für das Azure DNS Hostnameneinträge verwaltet, wenn eine VM erstellt wird, sich ihre IP-Adresse ändert oder die VM zerstört wird.  Dies wird als „virtuelles Registrierungsnetzwerk“ bezeichnet.

# <a name="get-the-preview-powershell-modules"></a>Abrufen der Vorschauversionen der PowerShell-Module
In dieser Anleitung wird vorausgesetzt, dass Sie Azure PowerShell bereits installiert und sich daran angemeldet und außerdem sichergestellt haben, dass Sie über die erforderlichen Module für das Feature für private Zonen verfügen. 

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]

## <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Vor dem Erstellen der DNS-Zone wird eine Ressourcengruppe für die DNS-Zone erstellt. Der Befehl ist im folgenden Beispiel enthalten.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-private-zone"></a>Erstellen einer privaten DNS-Zone

Eine DNS-Zone wird erstellt, indem das `New-AzureRmDnsZone`-Cmdlet zusammen mit dem Wert „Private“ für den Parameter „ZoneType“ verwendet wird. Im folgenden Beispiel wird eine DNS-Zone mit dem Namen *contoso.local* in der Ressourcengruppe *MyResourceGroup* erstellt, und die DNS-Zone wird für das virtuelle Netzwerk *MyAzureVnet* verfügbar gemacht. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

Beachten Sie, dass die Zone bei Auslassung des Parameters „ZoneType“ als öffentliche Zone (Public) erstellt wird. Er ist also erforderlich, wenn Sie eine private Zone erstellen möchten. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyAzureVnet -ResourceGroupName VnetResourceGroup
New-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ZoneType Private -ResolutionVirtualNetworkId @($vnet.Id)
```

Wenn es für Sie erforderlich ist, dass Azure in der Zone automatisch Hostnameneinträge erstellt, sollten Sie anstelle von *ResolutionVirtualNetworkId* den Parameter *RegistrationVirtualNetworkId* verwenden.  Virtuelle Registrierungsnetzwerke sind automatisch für die Auflösung aktiviert.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyAzureVnet -ResourceGroupName VnetResourceGroup
New-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ZoneType Private -RegistrationVirtualNetworkId @($vnet.Id)
```

## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Sie erstellen Ressourceneintragssätze mit dem Cmdlet `New-AzureRmDnsRecordSet`. Im folgenden Beispiel wird ein Eintrag mit dem relativen Namen „db“ in der DNS-Zone „contoso.local“ unter der Ressourcengruppe „MyResourceGroup“ erstellt. Der vollqualifizierte Name des Ressourceneintragssatzes lautet „db.contoso.local“. Der Eintragstyp ist „A“, die IP-Adresse lautet 10.0.0.4, und die Gültigkeitsdauer beträgt 3.600 Sekunden.

```powershell
New-AzureRmDnsRecordSet -Name db -RecordType A -ZoneName contoso.local -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "10.0.0.4")
```

Informationen zu anderen Eintragstypen, zu Eintragssätzen mit mehr als einem Eintrag, zu alternativen TTL-Werten und zum Ändern vorhandener Einträge finden Sie unter [Verwalten von DNS-Einträgen und -Ressourceneintragssätzen in Azure DNS über Azure PowerShell](dns-operations-recordsets.md). 

## <a name="view-records"></a>Anzeigen von Datensätzen

Listen Sie die DNS-Einträge in Ihrer Zone mit diesem Befehl auf:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.local -ResourceGroupName MyResourceGroup
```

# <a name="list-dns-private-zones"></a>Auflisten von privaten DNS-Zonen

Durch Auslassen des Zonennamens in `Get-AzureRmDnsZone` können Sie alle Zonen in einer Ressourcengruppe auflisten: Dieser Vorgang gibt ein Array von Zonenobjekten zurück.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

Durch das Weglassen des Zonennamens und des Ressourcengruppennamens aus `Get-AzureRmDnsZone` können Sie alle Zonen im Azure-Abonnement auflisten.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-private-zone"></a>Aktualisieren einer privaten DNS-Zone

Änderungen an einer DNS-Zonenressource können mithilfe von `Set-AzureRmDnsZone`vorgenommen werden. Durch dieses Cmdlet wird keine der DNS-Datensatzgruppen in der Zone aktualisiert (siehe [Verwalten von DNS-Einträgen](dns-operations-recordsets.md)). Es wird nur verwendet, um Eigenschaften der Zonenressource selbst zu aktualisieren. Die schreibbaren Zoneneigenschaften sind derzeit auf [Azure Resource Manager-Tags für die Zonenressource](dns-zones-records.md#tags) und auf die Parameter „RegistrationVirtualNetworkId“ und „ResolutionVirtualNetworkId“ für private Zonen beschränkt.

Im Beispiel unten wird das mit einer Zone verknüpfte virtuelle Registrierungsnetzwerk durch das neue virtuelle Netzwerk „MyNewAzureVnet“ ersetzt.

Beachten Sie, dass Sie hierbei nicht wie bei der Erstellung den Parameter „ZoneType“ für die Aktualisierung angeben dürfen. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyNewAzureVnet -ResourceGroupName MyResourceGroup
Set-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -RegistrationVirtualNetworkId @($vnet.Id)
```

Im Beispiel unten wird das mit einer Zone verknüpfte virtuelle Auflösungsnetzwerk durch das neue virtuelle Netzwerk „MyNewAzureVnet“ ersetzt.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyNewAzureVnet -ResourceGroupName MyResourceGroup
Set-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ResolutionVirtualNetworkId @($vnet.Id)
```

## <a name="delete-a-dns-private-zone"></a>Löschen einer privaten DNS-Zone

Private DNS-Zonen können genauso wie öffentliche Zonen mit dem `Remove-AzureRmDnsZone`-Cmdlet gelöscht werden.

> [!NOTE]
> Durch das Löschen einer DNS-Zone werden auch alle DNS-Einträge in der Zone gelöscht. Dieser Vorgang kann nicht rückgängig gemacht werden. Wenn die DNS-Zone verwendet wird, tritt in Diensten, die die Zone verwenden, ein Fehler auf.
>
>Informationen dazu, wie Sie Zonen vor versehentlichem Löschen schützen, finden Sie unter [How to protect DNS zones and records](dns-protect-zones-recordsets.md) (Schützen von DNS-Zonen und -Einträgen).

Verwenden Sie eine der beiden folgenden Möglichkeiten, um eine DNS-Zone zu entfernen:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Geben Sie die Zone mithilfe des Zonennamens und des Ressourcengruppennamens an.

```powershell
Remove-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a>Angeben der Zone mithilfe eines $zone-Objekts

Sie können festlegen, dass die Zone mithilfe eines von `Get-AzureRmDnsZone` zurückgegebenen `$zone`-Objekts gelöscht werden soll.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

Das Zonenobjekt kann auch weitergeleitet werden, anstatt als Parameter übergeben zu werden:

```powershell
Get-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup | Remove-AzureRmDnsZone

```

## <a name="confirmation-prompts"></a>Bestätigungsaufforderungen

Die Cmdlets `New-AzureRmDnsZone`, `Set-AzureRmDnsZone` und `Remove-AzureRmDnsZone` unterstützen jeweils Bestätigungsaufforderungen.

Sowohl `New-AzureRmDnsZone` als auch `Set-AzureRmDnsZone` fordern eine Bestätigung an, wenn die PowerShell-Einstellungsvariable `$ConfirmPreference` einen Wert von `Medium` oder weniger hat. Aufgrund der potenziell wesentlichen Auswirkungen der Löschung einer DNS-Zone forder das `Remove-AzureRmDnsZone`-Cmdlet zur Bestätigung auf, wenn die PowerShell-Variable `$ConfirmPreference` einen anderen Wert als `None` aufweist.

Da `High` der Standardwert für `$ConfirmPreference` ist, fordert nur `Remove-AzureRmDnsZone` standardmäßig zur Bestätigung auf.

Die aktuelle Einstellung für `$ConfirmPreference` kann mithilfe des `-Confirm`-Parameters überschrieben werden. Bei Angabe von `-Confirm` oder `-Confirm:$True` fordert das Cmdlet vor der Ausführung eine Bestätigung an. Bei Verwendung von `-Confirm:$False` fordert das Cmdlet keine Bestätigung an.

Weitere Informationen zu `-Confirm` und `$ConfirmPreference` finden Sie unter [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables) (Informationen zu Einstellungsvariablen).


## <a name="delete-all-resources"></a>Löschen aller Ressourcen

Führen Sie die folgenden Schritte aus, um alle in diesem Artikel erstellten Ressourcen zu löschen:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu privaten DNS-Zonen finden Sie unter [Using Azure DNS for private domains](private-dns-overview.md) (Verwenden von Azure DNS für private Domänen).

Informieren Sie sich über einige häufige [Szenarien zu privaten Zonen](./private-dns-scenarios.md), die mit privaten Zonen in Azure DNS realisiert werden können.

Weitere Informationen zum Verwalten von DNS-Einträgen in Azure DNS finden Sie unter [Verwalten von DNS-Einträgen und -Ressourceneintragssätzen in Azure DNS über Azure PowerShell](dns-operations-recordsets.md).

