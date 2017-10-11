---
title: Erste Schritte mit Azure-DNS-mithilfe von PowerShell | Microsoft Docs
description: Erfahren Sie, wie eine DNS-Zone und diesen Eintrag im DNS von Azure zu erstellen. Dies ist eine schrittweise Anleitung zum Erstellen und verwalten Ihre erste DNS-Zone und einen Datensatz mithilfe von PowerShell.
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 48f7ba325f61b4a91c0208b4c99058da801bee19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-dns-using-powershell"></a>Erste Schritte mit Azure-DNS-mithilfe von PowerShell

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure-CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure-CLI 2.0](dns-getstarted-cli.md)

Dieser Artikel führt Sie durch die Schritte zum Erstellen einer ersten DNS-Zone und einen Datensatz mithilfe von Azure PowerShell. Sie können auch die folgenden Schritte aus, die mithilfe von Azure-Portal oder die plattformübergreifende Azure-CLI ausführen.

Eine DNS-Zone wird verwendet, um die DNS-Einträge für einen bestimmten Domänennamen hosten. Um Ihre Domäne in Azure DNS-hosting zu starten, müssen Sie eine DNS-Zone für diesen Domänennamen zu erstellen. Jeder DNS-Eintrags für Ihre Domäne wird dann innerhalb dieser DNS-Zone erstellt. Um Ihre DNS-Zone mit dem Internet zu veröffentlichen, müssen Sie schließlich den Namenserver für die Domäne zu konfigurieren. Jeder dieser Schritte werden unten beschrieben.

Diese Anweisungen wird angenommen, Sie bereits installiert und bei Azure PowerShell angemeldet haben. Hilfe zu erhalten, finden Sie unter [zum Verwalten von DNS-Zonen, die mithilfe von PowerShell](dns-operations-dnszones.md).

## <a name="create-the-resource-group"></a>Die Ressourcengruppe erstellen

Vor dem Erstellen der DNS-Zone, wird eine Ressourcengruppe erstellt, um die DNS-Zone enthalten. Das folgende Beispiel zeigt den Befehl aus.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>Erstellen Sie eine DNS-zone

Eine DNS-Zone wird erstellt, indem Sie mit der `New-AzureRmDnsZone` Cmdlet. Das folgende Beispiel erstellt eine DNS-Zone aufgerufen *"contoso.com"* aufgerufen, in der Ressourcengruppe *MyResourceGroup*. Verwenden Sie das Beispiel zum Erstellen einer DNS-Zone, die Werte für Ihre eigene ersetzen.

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Erstellen Sie Datensatzgruppen mithilfe der `New-AzureRmDnsRecordSet` Cmdlet. Das folgende Beispiel erstellt einen Datensatz mit den relativen Namen der DNS-Zone "contoso.com" in Ressourcengruppe "MyResourceGroup" Zeichenfolge "Www" an. Der vollständig qualifizierte Name der Datensatzgruppe ist "www.contoso.com". Der Datensatztyp "A", mit der IP-Adresse "1.2.3.4" ist, und der Gültigkeitsdauer erfolgt ist 3600 Sekunden.

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

Für andere Datensatztypen für Datensatzgruppen mit mehr als ein Datensatz, und um vorhandene Datensätze zu bearbeiten, finden Sie unter [Verwalten von DNS-Einträge und Datensatzgruppen mit Azure PowerShell](dns-operations-recordsets.md). 


## <a name="view-records"></a>Anzeigen von Datensätzen

Um die DNS-Einträge in der Zone aufzulisten, verwenden Sie:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a>Aktualisieren von Namenservern

Wenn Sie zufrieden sind, dass Ihre DNS-Zone und Datensätze haben ordnungsgemäß eingerichtet wurde, müssen Sie Ihren Domänennamen zu verwenden, die Azure-DNS-Namenservern konfigurieren. Dadurch können andere Benutzer im Internet auf Ihre DNS-Einträge gefunden.

Der Namenserver für Ihre Zone anhand der `Get-AzureRmDnsZone` Cmdlet:

```powershell
Get-AzureRmDnsZone -ZoneName contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

Diese Namenserver sollte mit der Domänennamen-Registrierungsstelle konfiguriert werden (wobei Sie den Domänennamen erworben haben). Die Registrierungsstelle bietet die Option zum Einrichten der Namenserver für die Domäne. Weitere Informationen finden Sie unter [delegieren Sie Ihre Domäne in Azure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Löschen Sie alle Ressourcen

Um alle Ressourcen, die in diesem Artikel erstellt zu löschen, führen Sie den folgenden Schritt aus:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS (Übersicht)](dns-overview.md).

Weitere Informationen zum Verwalten von DNS-Zonen in Azure DNS finden Sie unter [Verwalten von DNS-Zonen im Azure-DNS-mithilfe von PowerShell](dns-operations-dnszones.md).

Weitere Informationen zum Verwalten von DNS-Einträge im DNS für Azure finden Sie unter [Verwalten von DNS-Einträge Datensatz wird im Azure-DNS mit PowerShell](dns-operations-recordsets.md).

