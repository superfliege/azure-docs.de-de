---
title: Erstellen, Ändern oder Löschen des Präfix einer öffentlichen Azure-IP-Adresse | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Präfix einer öffentlichen IP-Adresse erstellen, ändern oder löschen.
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: anavin
ms.openlocfilehash: 71dee9f36a3e8e11cc23f966c9157f6409a3b5e0
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405732"
---
# <a name="create-change-or-delete-a-public-ip-address-prefix"></a>Erstellen, Ändern oder Löschen des Präfix einer öffentlichen IP-Adresse

Sie erhalten Informationen über Präfixe öffentlicher IP-Adressen und darüber, wie Sie diese erstellen, ändern und löschen können. Das Präfix einer öffentlichen IP-Adresse ist ein zusammenhängender Adressbereich, der auf der Anzahl der von Ihnen angegeben IP-Adressen basiert. Die Adressen sind Ihrem Abonnement zugewiesen. Wenn Sie eine öffentliche IP-Adressressource erstellen, können Sie eine statische öffentliche IP-Adresse aus dem Präfix zuweisen und virtuellen Computern, Lastenausgleichsmodulen oder anderen Ressourcen zuordnen, um Internetkonnektivität zu ermöglichen. Wenn Sie mit Präfixen öffentlicher IP-Adressen nicht vertraut sind, finden Sie weitere Informationen unter [Übersicht über Präfixe öffentlicher IP-Adressen](public-ip-address-prefix.md).

## <a name="before-you-begin"></a>Voraussetzungen

> [!IMPORTANT]
> Präfixe öffentlicher IP-Adressen befinden sich in einigen Regionen in der öffentlichen Vorschau. [Hier erfahren Sie mehr über die Nutzungsbedingungen der Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Präfixe öffentlicher IP-Adressen sind zurzeit in folgenden Regionen verfügbar: „USA, Westen-Mitte“, „USA, Westen“, „USA, Westen 2“, „USA, Mitte“, „Europa, Norden“, „Europa, Westen“ und „Asien, Südosten“. Eine aktualisierte Liste der Regionen finden Sie unter [Azure-Updates](https://azure.microsoft.com/updates/?product=virtual-network).

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Öffnen Sie bei Verwendung des Portals https://aka.ms/publicipprefixportal, und melden Sie sich mit Ihrem Azure-Konto an.
- Wenn Sie PowerShell-Befehle zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/powershell) oder durch Ausführen von PowerShell auf Ihrem Computer aus. Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Für dieses Tutorial ist das AzureRm.Network-PowerShell-Modul Version 6.3.1 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM.Network` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](https://github.com/Azure/azure-powershell/releases/tag/AzureRm.Network.6.3.1) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/bash) oder durch Ausführen der CLI auf Ihrem Computer aus. Für dieses Tutorial ist die Azure CLI-Version 2.0.41 oder höher erforderlich. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. Wenn Sie die Azure CLI lokal ausführen, müssen Sie auch `az login` ausführen, um eine Verbindung mit Azure herzustellen.

Das Konto, bei dem Sie sich anmelden oder das Sie zum Herstellen einer Verbindung mit Azure verwenden, muss der Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) oder einer [benutzerdefinierten Rolle](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) zugewiesen sein, der die entsprechenden, unter [Berechtigungen](#permissions) aufgeführten Aktionen zugewiesen wurden.

Für Präfixe öffentlicher IP-Adressen fällt eine Gebühr an. Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="create-a-public-ip-address-prefix"></a>Erstellen des Präfix einer öffentlichen IP-Adresse

1. Wählen Sie in der linken oberen Ecke des Portals die Option **+ Ressource erstellen** aus.
2. Geben Sie im Feld *Marketplace durchsuchen* den Begriff *Präfix einer öffentlichen IP-Adresse* ein. Wenn **Präfix einer öffentlichen IP-Adresse** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie unter **Präfix einer öffentlichen IP-Adresse** auf **Erstellen**.
4. Geben Sie unter **Präfix einer öffentlichen IP-Adresse erstellen** Werte für folgende Einstellungen ein, oder wählen Sie Werte aus, und klicken Sie dann auf **Erstellen**:

   |Einstellung|Erforderlich?|Details|
   |---|---|---|
   |Abonnement|Ja|Muss im selben [Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) wie die Ressource vorhanden sein, der Sie die öffentliche IP-Adresse zuordnen möchten|
   |Ressourcengruppe|Ja|Kann in derselben oder in einer anderen [Ressourcengruppe](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) wie die Ressource vorhanden sein, der Sie die öffentliche IP-Adresse zuordnen möchten|
   |Name|Ja|Der Name muss innerhalb der ausgewählten Ressourcengruppe eindeutig sein.|
   |Region|Ja|Muss in der gleichen [Region](https://azure.microsoft.com/regions) wie die öffentlichen IP-Adressen vorhanden sein, die Sie aus dem Bereich zuweisen. Das Präfix ist zurzeit in folgenden Regionen in der Vorschau verfügbar: „USA, Westen-Mitte“, „USA, Westen“, „USA, Westen 2“, „USA, Mitte“, „Europa, Norden“, „Europa, Westen“ und „Asien, Südosten“.|
   |Präfixgröße|Ja| Die benötigte Größe des Präfix. /28- oder 16-IP-Adressen sind der Standard. 

**Befehle**


|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network public-ip prefix create](/cli/azure/network/public-ip/prefix/az-network-public-ip-prefix-create)|
|PowerShell|[New-AzureRmPublicIpAddressPrefix](/powershell/module/azurerm.network/new-azurermpublicipaddressprefix)|

## <a name="create-a-static-public-ip-address-from-a-prefix"></a>Erstellen einer statischen öffentlichen IP-Adresse aus einem Präfix
Sobald Sie ein Präfix erstellt haben, müssen Sie statische IP-Adressen aus dem Präfix erstellen. Führen Sie dazu die folgenden Schritte aus.

1. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Präfix öffentlicher IP-Adressen* ein. Wenn **Präfix öffentlicher IP-Adressen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
2. Wählen Sie das Präfix aus, aus dem Sie öffentliche IPs erstellen möchten.
3. Wenn das Präfix in den Suchergebnissen angezeigt wird, wählen Sie es aus, und klicken Sie im Abschnitt „Übersicht“ auf **+ IP-Adresse hinzufügen**. Wenn es nicht angezeigt wird, stellen Sie sicher, dass Sie den richtigen Link für die Vorschau verwenden: https://aka.ms/publicipprefixportal
4. Geben Sie unter **Öffentliche IP-Adresse erstellen** Werte für folgende Einstellungen ein, oder wählen Sie Werte aus. Da ein Präfix sich für Standard-SKUs, IPv4 und statische Adressen eignet, müssen Sie nur die folgenden Informationen angeben:

   |Einstellung|Erforderlich?|Details|
    |---|---|---|
    |Name|Ja|Der Name der öffentlichen IP-Adresse muss innerhalb der ausgewählten Ressourcengruppe eindeutig sein.|
   |Leerlaufzeitüberschreitung (Minuten)|Nein |Gibt an, wie viele Minuten eine TCP- oder HTTP-Verbindung geöffnet bleiben soll, ohne dass Clients Keep-Alive-Meldungen senden müssen. |
   |DNS-Namensbezeichnung|Nein |Muss in der Azure-Region, in der Sie den Namen erstellen, eindeutig sein (über alle Abonnements und Kunden hinweg). Azure registriert den Namen und die IP-Adresse automatisch im DNS, sodass Sie über den Namen eine Verbindung mit der Ressource herstellen können. Azure fügt ein Standardsubnetz wie etwa *location.cloudapp.azure.com* (wobei „location“ der Standort ist, den Sie auswählen) an den von Ihnen bereitgestellten Namen an, um den vollqualifizierten DNS-Namen zu erstellen. Weitere Informationen finden Sie unter [Verwenden von Azure DNS mit einer öffentlichen Azure-IP-Adresse](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|

## <a name="view-or-delete-a-prefix"></a>Anzeigen oder Löschen eines Präfix

1. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Präfix öffentlicher IP-Adressen* ein. Wenn **Präfix öffentlicher IP-Adressen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
2. Wählen Sie den Namen des Präfix der öffentlichen IP-Adresse aus, das Sie anzeigen oder aus der Liste löschen oder dessen Einstellungen Sie ändern möchten.
3. Führen Sie eine der folgenden Aktionen aus, je nachdem, ob Sie das Präfix der öffentlichen IP-Adresse anzeigen, löschen oder ändern möchten.
    - **Anzeigen**: Der Abschnitt **Übersicht** zeigt wichtige Einstellungen für das Präfix der öffentlichen IP-Adresse.
    - **Löschen**: Um das Präfix der öffentlichen IP-Adresse zu löschen, klicken Sie im Abschnitt **Übersicht** auf **Löschen**. Wenn Adressen innerhalb des Präfix mit öffentlichen IP-Adressressourcen verknüpft sind, müssen Sie zuerst die öffentlichen IP-Adressressourcen löschen. Informationen dazu finden Sie unter [Löschen einer öffentlichen IP-Adresse](virtual-network-public-ip-address.md#view-change-settings-for-or-delete-a-public-ip-address).

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network public-ip prefix list](/cli/azure/network/public-ip/prefix/az-network-public-ip-prefix-list) zum Auflisten der öffentlichen IP-Adressen, [az network public-ip prefix show](/cli/azure/network/public-ip/prefix/az-network-public-ip-prefix-show) zum Anzeigen der Einstellungen, [az network public-ip prefix update](/cli/azure/network/public-ip-prefix#az-network-public-ip-prefix-update) zum Aktualisieren, [az network public-ip prefix delete](/cli/azure/network/public-ip/prefix/az-network-public-ip-prefix-delete) zum Löschen|
|PowerShell|[Get-AzureRmPublicIpAddressPrefix](/powershell/module/azurerm.network/get-azurermpublicipaddressprefix) zum Abrufen eines öffentlichen IP-Adressenobjekts und Anzeigen der zugehörigen Einstellungen, [Set-AzureRmPublicIpAddressPrefix](/powershell/module/azurerm.network/set-azurermpublicipaddressprefix) zum Aktualisieren der Einstellungen, [Remove-AzureRmPublicIpAddressPrefix](/powershell/module/azurerm.network/remove-azurermpublicipaddressprefix) zum Löschen|

## <a name="permissions"></a>Berechtigungen

Zum Durchführen von Aufgaben für Präfixe öffentlicher IP-Adressen muss Ihr Konto der Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) oder einer [benutzerdefinierten](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Rolle zugewiesen sein, der die entsprechenden, in der folgenden Tabelle aufgeführten Aktionen zugewiesen wurden:

| Aktion                                                                   | Name                                                           |
| ---------                                                                | -------------                                                  |
| Microsoft.Network/publicIPPrefixes/read                           | Lesen des Präfix einer öffentlichen IP-Adresse                                |
| Microsoft.Network/publicIPPrefixes/write                          | Erstellen oder Aktualisieren des Präfix einer öffentlichen IP-Adresse                    |
| Microsoft.Network/publicIPPrefixes/delete                         | Löschen des Präfix einer öffentlichen IP-Adresse                              |
|Microsoft.Network/publicIPPrefixes/join/action | Erstellen einer öffentlichen IP-Adresse aus einem Präfix |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über Szenarien und Vorteile der Verwendung des [Präfix einer öffentlichen IP](public-ip-address-prefix.md).
