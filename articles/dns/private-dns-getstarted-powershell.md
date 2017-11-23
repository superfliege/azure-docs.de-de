---
title: Erste Schritte mit privaten Azure DNS-Zonen mithilfe von PowerShell | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie eine private DNS-Zone und einen DNS-Eintrag in Azure DNS erstellen. Dies ist eine Schritt-für-Schritt-Anleitung zum Erstellen und Verwalten Ihrer ersten privaten DNS-Zone und Ihres ersten DNS-Eintrags mithilfe von Azure PowerShell."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2017
ms.author: kumud
ms.openlocfilehash: d71e2391b6415b2403447479dea4fd0a3b818ed0
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2017
---
# <a name="get-started-with-azure-dns-private-zones-using-powershell"></a>Erste Schritte mit privaten Azure DNS-Zonen mithilfe von PowerShell

In diesem Artikel wird Schritt für Schritt beschrieben, wie Sie Ihre erste private DNS-Zone und Ihren ersten DNS-Eintrag mithilfe von Azure PowerShell erstellen.

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt. Um eine private DNS-Zone in Ihrem virtuellen Netzwerk zu veröffentlichen, geben Sie die Liste mit den virtuellen Netzwerken an, für die das Auflösen von Einträgen in der Zone zulässig ist.  Diese Netzwerke werden als „Auflösungsnetzwerke“ bezeichnet.  Sie können auch einen Satz mit virtuellen Netzwerken angeben, für die Azure DNS Hostnameneinträge verwaltet, wenn eine VM erstellt wird, sich die IP-Adresse ändert oder die VM zerstört wird.  Diese Netzwerke werden als „Registrierungsnetzwerke“ bezeichnet.

Da es sich bei diesem Feature derzeit um eine verwaltete Vorschauversion handelt, wird ein PowerShell-Vorschaumodul bereitgestellt.

[!INCLUDE [private-dns-preview-notice](../../includes/private-dns-preview-notice.md)]

## <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Vor dem Erstellen der DNS-Zone wird eine Ressourcengruppe für die DNS-Zone erstellt. Hier sehen Sie den Befehl:

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `New-AzureRmDnsZone` -Cmdlet erstellt. Im folgenden Beispiel wird eine DNS-Zone mit dem Namen *contoso.local* in der Ressourcengruppe *MyResourceGroup* erstellt, und die DNS-Zone wird für das virtuelle Netzwerk *MyAzureVnet* verfügbar gemacht. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

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

## <a name="delete-all-resources"></a>Löschen aller Ressourcen

Führen Sie die folgenden Schritte aus, um alle in diesem Artikel erstellten Ressourcen zu löschen:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu privaten DNS-Zonen finden Sie unter [Using Azure DNS for private domains](private-dns-overview.md) (Verwenden von Azure DNS für private Domänen).

Weitere Informationen zum Verwalten von DNS-Einträgen in Azure DNS finden Sie unter [Verwalten von DNS-Einträgen und -Ressourceneintragssätzen in Azure DNS über Azure PowerShell](dns-operations-recordsets.md).

