---
title: Erstellen, Ändern und Löschen von Azure-Netzwerkschnittstellen | Microsoft-Dokumentation
description: Erfahren Sie, was Netzwerkschnittstellen sind und wie Sie sie erstellen, ihre Einstellungen ändern und sie löschen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 49685d5b0e30356cab7f1f530bcc97e193d7fd90
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="create-change-or-delete-a-network-interface"></a>Erstellen, Ändern oder Löschen von Netzwerkschnittstellen

Erfahren Sie, wie Sie Netzwerkschnittstellenkarten erstellen, ihre Einstellungen ändern und sie löschen. Eine Netzwerkschnittstelle ermöglicht einem virtuellen Azure-Computer die Kommunikation mit Internet, Azure und lokalen Ressourcen. Wenn Sie im Azure-Portal eine VM erstellen, generiert das Portal für Sie eine Netzwerkschnittstelle mit Standardeinstellungen. Sie können stattdessen Netzwerkschnittstellen mit benutzerdefinierten Einstellungen erstellen und einem virtuellen Computer bei der Erstellung Netzwerkschnittstellen hinzufügen. Sie können auch die Standardeinstellungen einer vorhandenen Netzwerkschnittstelle ändern. In diesem Artikel wird erläutert, wie Sie Netzwerkschnittstellen mit benutzerdefinierten Einstellungen erstellen, vorhandene Netzwerkschnittstelleneinstellungen ändern, z.B. die Netzwerkfilterzuweisung (Netzwerksicherheitsgruppen), Subnetzzuweisung, DNS-Servereinstellungen und IP-Weiterleitung, und Netzwerkschnittstellen löschen.

Weitere Informationen zum Hinzufügen, Ändern oder Entfernen von IP-Adressen für eine Netzwerkschnittstelle finden Sie unter [Hinzufügen, Ändern oder Entfernen von IP-Adressen für Azure-Netzwerkschnittstellen](virtual-network-network-interface-addresses.md). Wenn Sie auf einem virtuellen Computer Netzwerkschnittstellen hinzufügen oder entfernen möchten, lesen Sie [Hinzufügen oder Entfernen von Netzwerkschnittstellen](virtual-network-network-interface-vm.md).


## <a name="before-you-begin"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Öffnen Sie bei Verwendung des Portals https://portal.azure.com, und melden Sie sich mit Ihrem Azure-Konto an.
- Wenn Sie PowerShell-Befehle zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/powershell) oder durch Ausführen von PowerShell auf Ihrem Computer aus. Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Für dieses Tutorial ist das Azure PowerShell-Modul Version 5.2.0 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/bash) oder durch Ausführen der CLI auf Ihrem Computer aus. Für dieses Tutorial ist mindestens Version 2.0.26 der Azure CLI erforderlich. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. Wenn Sie die Azure CLI lokal ausführen, müssen Sie auch `az login` ausführen, um eine Verbindung mit Azure herzustellen.

Dem Konto, mit dem Sie sich bei Azure anmelden, müssen mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ Ihres Abonnements zugewiesen sein. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).

## <a name="create-a-network-interface"></a>Erstellen einer Netzwerkschnittstelle

Wenn Sie im Azure-Portal eine VM erstellen, generiert das Portal für Sie eine Netzwerkschnittstelle mit Standardeinstellungen. Wenn Sie lieber alle Netzwerkschnittstelleneinstellungen angeben möchten, können Sie eine Netzwerkschnittstelle mit benutzerdefinierten Einstellungen erstellen und beim Erstellen eines virtuellen Computers an diesen anfügen (mithilfe von PowerShell oder der Azure-Befehlszeilenschnittstelle). Sie können auch eine Netzwerkschnittstelle erstellen und an einen vorhandenen virtuellen Computer anfügen (mithilfe von PowerShell oder der Azure-Befehlszeilenschnittstelle). Weitere Informationen zum Erstellen eines virtuellen Computers mit einer vorhandenen Netzwerkschnittstelle oder zum Hinzufügen und Entfernen von Netzwerkschnittstellen bei vorhandenen virtuellen Computern finden Sie unter [Hinzufügen oder Entfernen von Netzwerkschnittstellen](virtual-network-network-interface-vm.md). Vor dem Erstellen einer Netzwerkschnittstelle müssen Sie über ein [virtuelles Netzwerk](manage-virtual-network.md#create-a-virtual-network) am selben Standort und im selben Abonnement verfügen, in dem Sie eine Netzwerkschnittstelle erstellen.

1. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, wählen Sie dies aus.
2. Wählen Sie **+ Hinzufügen** unter **Netzwerkschnittstellen** aus.
3. Geben Sie Werte für folgende Einstellungen ein, oder wählen Sie sie aus, und wählen Sie dann **Erstellen**:

    |Einstellung|Erforderlich?|Details|
    |---|---|---|
    |NAME|Ja|Der Name muss innerhalb der ausgewählten Ressourcengruppe eindeutig sein. Im Lauf der Zeit verfügen Sie wahrscheinlich über mehrere Netzwerkschnittstellen in Ihrem Azure-Abonnement. Vorschläge zum Festlegen einer Benennungskonvention zum Vereinfachen der Verwaltung mehrerer Netzwerkschnittstellen finden Sie unter [Benennungskonventionen](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). Nach dem Erstellen kann der Name der Netzwerkschnittstelle nicht mehr geändert werden.|
    |Virtuelles Netzwerk|Ja|Wählen Sie das virtuelle Netzwerk für die Netzwerkschnittstelle aus. Sie können eine Netzwerkschnittstelle nur einem virtuellen Netzwerk zuweisen, das dasselbe Abonnement und denselben Standort wie die Netzwerkschnittstelle hat. Nachdem eine Netzwerkschnittstelle erstellt wurde, können Sie das virtuelle Netzwerk, dem sie zugewiesen ist, nicht mehr ändern. Der virtuelle Computer, dem Sie die Netzwerkschnittstelle hinzufügen, muss auch denselben Standort und dasselbe Abonnement wie die Netzwerkschnittstelle aufweisen.|
    |Subnetz|Ja|Wählen Sie ein Subnetz innerhalb des ausgewählten virtuellen Netzwerks aus. Das Subnetz, dem die Netzwerkschnittstelle zugewiesen ist, kann nach der Erstellung geändert werden.|
    |Zuweisung der privaten IP-Adresse|Ja| Mit dieser Einstellung wählen Sie die Zuweisungsmethode für die IPv4-Adresse aus. Wählen Sie aus den folgenden Methoden für die Zuweisung: **Dynamisch:** Bei Wahl dieser Option weist Azure automatisch die nächste verfügbare Adresse aus dem Adressraum des Subnetzes zu, das Sie ausgewählt haben. **Statisch:** Bei Wahl dieser Option müssen Sie eine verfügbare IP-Adresse innerhalb des Adressraums des ausgewählten Subnetzes manuell zuweisen. Statische und dynamische Adressen bleiben unverändert, bis Sie sie ändern oder die Netzwerkschnittstelle gelöscht wird. Die Zuweisungsmethode kann nach Erstellung der Netzwerkschnittstelle geändert werden. Der DHCP-Server von Azure weist diese Adresse der Netzwerkschnittstelle innerhalb des Betriebssystems des virtuellen Computers zu.|
    |Netzwerksicherheitsgruppe|Nein | Belassen Sie diese Einstellung auf **Keine** festgelegt, wählen Sie eine vorhandene [Netzwerksicherheitsgruppe](virtual-networks-nsg.md) aus, oder [erstellen Sie eine Netzwerksicherheitsgruppe](virtual-networks-create-nsg-arm-pportal.md). Mithilfe von Netzwerksicherheitsgruppen können Sie den ein- und ausgehenden Netzwerkdatenverkehr für eine Netzwerkschnittstelle steuern. Auf eine Netzwerkschnittstelle kann maximal eine Netzwerksicherheitsgruppe angewendet werden. Auch auf das Subnetz, dem die Netzwerkschnittstelle zugewiesen ist, kann maximal eine Netzwerksicherheitsgruppe angewendet werden. Wenn eine Netzwerksicherheitsgruppe auf eine Netzwerkschnittstelle und auf das Subnetz, dem die Netzwerkschnittstelle zugewiesen ist, angewendet wird, kommt es in einigen Fällen zu unerwarteten Ergebnissen. Informationen zum Beheben von Problemen mit Netzwerksicherheitsgruppen, die auf Netzwerkschnittstellen und Subnetze angewendet werden, finden Sie unter [Beheben von Problemen bei Netzwerksicherheitsgruppen](virtual-network-nsg-troubleshoot-portal.md#nsg).|
    |Abonnement|Ja|Wählen Sie eines Ihrer Azure-[Abonnements](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) aus. Der virtuelle Computer, an den Sie eine Netzwerkschnittstelle anfügen, und das virtuelle Netzwerk, mit dem Sie eine Verbindung herstellen, müssen zu demselben Abonnement gehören.|
    |Private IP-Adresse (IPv6)|Nein | Wenn Sie dieses Kontrollkästchen aktivieren, wird der Netzwerkschnittstelle zusätzlich zu der IPv4-Adresse der Netzwerkschnittstelle eine IPv6-Adresse zugewiesen. Im Abschnitt [IPv6](#IPv6) dieses Artikels finden Sie wichtige Informationen zur Verwendung von IPv6 bei Netzwerkschnittstellen. Eine Auswahl der Zuweisungsmethode für die IPv6-Adresse ist nicht möglich. Wenn Sie eine IPv6-Adresse zuweisen, erfolgt die Zuordnung mit der dynamischen Methode.
    |IPv6-Name (wird nur angezeigt, wenn das Kontrollkästchen **Private IP-Adresse (IPv6)** aktiviert ist) |Ja, wenn das Kontrollkästchen **Private IP-Adresse (IPv6)** aktiviert ist.| Dieser Name wird einer sekundären IP-Konfiguration für die Netzwerkschnittstelle zugewiesen. Weitere Informationen zu IP-Konfigurationen finden Sie unter [Anzeigen der Einstellungen von Netzwerkschnittstellen](#view-network-interface-settings).|
    |Ressourcengruppe|Ja|Wählen Sie eine vorhandene [Ressourcengruppe](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) aus, oder erstellen Sie eine. Eine Netzwerkschnittstelle kann sich in der gleichen (oder in einer anderen) Ressourcengruppe befinden wie der virtuelle Computer, an den Sie sie anfügen, oder wie das virtuelle Netzwerk, mit dem Sie eine Verbindung herstellen.|
    |Speicherort|Ja|Der virtuelle Computer, an den Sie eine Netzwerkschnittstelle anfügen, und das virtuelle Netzwerk, mit dem Sie eine Verbindung herstellen, müssen sich an demselben [Standort](https://azure.microsoft.com/regions) befinden. Dieser wird auch als Region bezeichnet.|

Das Portal stellt keine Option bereit, mit der Sie der Netzwerkschnittstelle beim Erstellen eine öffentliche IP-Adresse zuweisen können. Sie können allerdings im Portal eine öffentliche IP-Adresse erstellen und diese einer Netzwerkschnittstelle zuweisen, wenn Sie im Portal einen virtuellen Computer erstellen. Informationen zum Hinzufügen einer öffentlichen IP-Adresse zur Netzwerkschnittstelle nach deren Erstellung finden Sie unter [Verwalten von IP-Adressen](virtual-network-network-interface-addresses.md). Wenn Sie eine Netzwerkschnittstelle mit einer öffentlichen IP-Adresse erstellen möchten, müssen Sie zum Erstellen der Netzwerkschnittstelle die Befehlszeilenschnittstelle oder PowerShell verwenden.

>[!Note]
> Azure weist der Netzwerkschnittstelle erst dann eine MAC-Adresse zu, wenn die Netzwerkschnittstelle an einen virtuellen Computer angefügt und der virtuelle Computer erstmalig gestartet wurde. Die MAC-Adresse, die Azure der Netzwerkschnittstelle zuweist, kann nicht angegeben werden. Die MAC-Adresse bleibt der Netzwerkschnittstelle zugewiesen, bis die Netzwerkschnittstelle gelöscht oder die private IP-Adresse, die der primären IP-Konfiguration der primären Netzwerkschnittstelle zugewiesen ist, geändert wird. Weitere Informationen zu IP-Adressen und IP-Konfigurationen finden Sie unter [Verwalten von IP-Adressen](virtual-network-network-interface-addresses.md).

**Befehle**

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic create](/cli/azure/network/nic#az_network_nic_create)|
|PowerShell|[New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface#create)|

## <a name="view-network-interface-settings"></a>Anzeigen der Einstellungen von Netzwerkschnittstellen

Sie können die meisten Einstellungen für eine Netzwerkschnittstelle anzeigen und ändern, nachdem diese erstellt wurde. Das Portal zeigt kein DNS-Suffix für die Netzwerkschnittstelle an. Sie können mithilfe der PowerShell- oder Azure CLI-[Befehle](#view-settings-commands) das DNS-Suffix anzeigen.

1. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, wählen Sie dies aus.
2. Wählen Sie in der Liste die Netzwerkschnittstelle aus, die Sie anzeigen oder deren Einstellungen Sie ändern möchten.
3. Die folgenden Elemente werden für die Netzwerkschnittstelle aufgelistet, die Sie ausgewählt haben:
    - **Übersicht:** enthält Informationen über die Netzwerkschnittstelle, z.B. die zugewiesenen IP-Adressen, das virtuelle Netzwerk/Subnetz, dem die Netzwerkschnittstelle zugewiesen ist, und der virtuelle Computer, an den die Netzwerkschnittstelle angefügt ist (sofern zutreffend). Die folgende Abbildung zeigt die Übersicht über die Einstellungen für eine Netzwerkschnittstelle mit dem Namen **mywebserver256**: ![Übersicht über die Netzwerkschnittstelle](./media/virtual-network-network-interface/nic-overview.png) Sie können eine Netzwerkschnittstelle in eine andere Ressourcengruppe oder ein anderes Abonnement verschieben, indem Sie neben der **Ressourcengruppe** oder dem **Abonnementnamen** (**ändern**) auswählen. Wenn Sie die Netzwerkschnittstelle verschieben, müssen Sie alle Ressourcen für die Netzwerkschnittstelle ebenfalls verschieben. Wenn die Netzwerkschnittstelle an einen virtuellen Computer angefügt ist, müssen Sie z.B. auch den virtuellen Computer und andere mit dem virtuellen Computer verknüpfte Ressourcen verschieben. Informationen zum Verschieben von Netzwerkschnittstellen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal). Hier erfahren Sie, welche Voraussetzungen erfüllt sein müssen und wie Sie Ressourcen über das Azure-Portal, mithilfe von PowerShell oder unter Verwendung der Azure CLI verschieben.
    - **IP-Konfigurationen:** Hier werden die öffentlichen und privaten IPv4- und IPv6-Adressen aufgeführt, die IP-Konfigurationen zugewiesen sind. Wenn einer IP-Konfiguration eine IPv6-Adresse zugewiesen ist, wird die Adresse nicht angezeigt. Weitere Informationen zu IP-Konfigurationen und zum Hinzufügen und Entfernen von IP-Adressen finden Sie unter [Konfigurieren von IP-Adressen für eine Azure-Netzwerkschnittstelle](virtual-network-network-interface-addresses.md). Die IP-Weiterleitung und Subnetzzuweisung werden auch in diesem Abschnitt konfiguriert. Weitere Informationen zu diesen Einstellungen finden Sie unter [Aktivieren oder Deaktivieren der IP-Weiterleitung](#enable-or-disable-ip-forwarding) und [Ändern der Subnetzzuweisung](#change-subnet-assignment).
    - **DNS-Server:** Sie können angeben, welcher DNS-Server einer Netzwerkschnittstelle durch die Azure-DHCP-Server zugewiesen wird. Die Netzwerkschnittstelle kann die Einstellung vom virtuellen Netzwerk, dem die Netzwerkschnittstelle zugewiesen ist, erben. Sie können aber auch benutzerdefinierte Einstellungen festlegen, die die entsprechenden Einstellungen für das virtuelle Netzwerk, dem sie zugewiesen ist, überschreiben. Um zu ändern, was angezeigt wird, lesen Sie [Ändern von DNS-Servern](#change-dns-servers).
    - **Netzwerksicherheitsgruppe (NSG):** zeigt an, welche Netzwerksicherheitsgruppe (NSG) der Netzwerkschnittstelle zugeordnet ist (falls vorhanden). Eine NSG enthält ein- und ausgehende Regeln zum Filtern von Netzwerkdatenverkehr für die Netzwerkschnittstelle. Falls der Netzwerkschnittstelle eine NSG zugeordnet ist, wird der Name der zugeordneten NSG angezeigt. Zum Ändern der angezeigten Informationen führen Sie die Schritte unter [Verwalten der Zuordnungen von Netzwerksicherheitsgruppen](virtual-network-manage-nsg-arm-portal.md#manage-associations) durch.
    - **Eigenschaften:** zeigt wichtige Einstellungen für die Netzwerkschnittstelle an, z.B. die MAC-Adresse (leer, wenn die Netzwerkschnittstelle an keine VM angefügt ist) und das Abonnement, dem sie angehört.
    - **Effektive Sicherheitsregeln:** Sicherheitsregeln werden aufgelistet, wenn die Netzwerkschnittstelle an einen aktiven virtuellen Computer angefügt und der Netzwerkschnittstelle oder dem Subnetz, mit dem sie verbunden ist, eine NSG zugeordnet ist. Weitere Informationen zu den angezeigten Informationen finden Sie unter [Problembehandlung bei Netzwerksicherheitsgruppen über das Azure-Portal](virtual-network-nsg-troubleshoot-portal.md#nsg). Weitere Informationen zu NSGs finden Sie unter [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](virtual-networks-nsg.md).
    - **Effektive Routen:** Routen werden aufgelistet, wenn die Netzwerkschnittstelle an einen aktiven virtuellen Computer angefügt ist. Bei den Routen handelt es sich um eine Kombination aus den Azure-Standardrouten, benutzerdefinierten Routen und BGP-Routen, die ggf. für das Subnetz vorhanden sind, dem die Netzwerkschnittstelle zugewiesen ist. Weitere Informationen zur Anzeige finden Sie unter [Problembehandlung bei Routen über das Azure-Portal](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-network-interface). Weitere Informationen zu Azure-Standardeinstellungen und benutzerdefinierten Routen finden Sie unter [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).
    - **Allgemeine Azure Resource Manager-Einstellungen:** Weitere Informationen zu allgemeinen Azure Resource Manager-Einstellungen finden Sie unter [Aktivitätsprotokolle](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [Zugriffssteuerung (IAM, Identity and Access Management)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [Tags](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Sperren von Ressourcen, um unerwartete Änderungen zu verhindern](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und [Exportieren der Vorlage aus der Ressourcengruppe](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group).

<a name="view-settings-commands"></a>**Befehle**

Wenn eine IPv6-Adresse einer Netzwerkschnittstelle zugewiesen ist, gibt die PowerShell-Ausgabe zurück, dass die Adresse zugewiesen ist, aber nicht die zugewiesene Adresse selbst. Auf ähnliche Weise gibt die Befehlszeilenschnittstelle zurück, dass die Adresse zugewiesen ist, als Ausgabe für die Adresse jedoch lediglich *NULL*.

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic list](/cli/azure/network/nic#az_network_nic_list): Anzeigen von im Abonnement enthaltenen Netzwerkschnittstellen; [az network nic show](/cli/azure/network/nic#az_network_nic_show): Anzeigen der Einstellungen für eine Netzwerkschnittstelle|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface): Anzeigen von im Abonnement enthaltenen Netzwerkschnittstellen oder der Einstellungen für eine Netzwerkschnittstelle|

## <a name="change-dns-servers"></a>Ändern von DNS-Servern

Der DHCP-Server von Azure weist den DNS-Server der Netzwerkschnittstelle innerhalb des Betriebssystems des virtuellen Computers zu. Der zugewiesene DNS-Server entspricht der geltenden DNS-Servereinstellung für eine Netzwerkschnittstelle. Weitere Informationen zu Namensauflösungseinstellungen für eine Netzwerkschnittstelle finden Sie im Artikel [Namensauflösung für virtuelle Computer](virtual-networks-name-resolution-for-vms-and-role-instances.md). Die Netzwerkschnittstelle kann die Einstellungen des virtuellen Netzwerks übernehmen oder eigene eindeutige Einstellungen verwenden, die die Einstellung für das virtuelle Netzwerk überschreiben.

1. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, wählen Sie dies aus.
2. Wählen Sie in der Liste die Netzwerkschnittstelle aus, für die Sie einen DNS-Server ändern möchten.
3. Wählen Sie unter **EINSTELLUNGEN** die Option **DNS-Server** aus.
4. Wählen Sie eins von beiden:
    - **Von virtuellem Netzwerk erben**: Wählen Sie diese Option aus, wenn die DNS-Servereinstellung des virtuellen Netzwerks übernommen werden soll, dem die Netzwerkschnittstelle zugewiesen ist. Auf Ebene des virtuellen Netzwerks ist entweder ein benutzerdefinierter DNS-Server oder der von Azure bereitgestellte DNS-Server definiert. Der von Azure bereitgestellte DNS-Server kann Hostnamen für Ressourcen auflösen, die demselben virtuellen Netzwerk zugewiesen sind. Zum Auflösen von Ressourcen, die anderen virtuellen Netzwerken zugewiesen sind, müssen vollqualifizierte Domänennamen (FQDNs) verwendet werden.
    - **Benutzerdefiniert:** Sie können einen eigenen DNS-Server für eine die virtuellen Netzwerke übergreifende Namensauflösung konfigurieren. Geben Sie die IP-Adresse des Servers ein, den Sie als DNS-Server verwenden möchten. Die von Ihnen angegebene DNS-Serveradresse wird nur dieser Netzwerkschnittstelle zugewiesen. Sie überschreibt alle DNS-Einstellungen für das virtuelle Netzwerk, dem die Netzwerkschnittstelle zugewiesen ist.
5. Wählen Sie **Speichern**aus.

**Befehle**

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic update](/cli/azure/network/nic#az_network_nic_update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="enable-or-disable-ip-forwarding"></a>Aktivieren oder Deaktivieren der IP-Weiterleitung

Die IP-Weiterleitung ermöglicht virtuellen Computern mit einer zugewiesenen Netzwerkschnittstelle Folgendes:
- Empfangen von Netzwerkdatenverkehr, der nicht für eine der IP-Adressen bestimmt ist, die einer der IP-Konfigurationen für die Netzwerkschnittstelle zugewiesen ist
- Senden von Netzwerkdatenverkehr mit einer IP-Quelladresse, die nicht der Adresse entspricht, die den IP-Konfigurationen einer Netzwerkschnittstelle zugewiesen ist

Die Einstellung muss für jede Netzwerkschnittstelle aktiviert werden, die an den virtuellen Computer angefügt ist, der Datenverkehr empfängt und weiterleiten muss. Ein virtueller Computer kann Datenverkehr unabhängig davon weiterleiten, ob er über mehrere Netzwerkschnittstellen verfügt oder nur eine einzelne Netzwerkschnittstelle angefügt ist. Bei der IP-Weiterleitung handelt es sich zwar um eine Azure-Einstellung, auf dem virtuellen Computer muss aber auch eine Anwendung ausgeführt werden, die den Datenverkehr weiterleiten kann – beispielsweise eine Firewall, ein WAN-Optimierer oder ein Lastenausgleich. Wenn ein virtueller Computer Netzwerkanwendungen ausführt, wird er häufig als virtuelles Netzwerkgerät (Network Virtual Appliance) bezeichnet. Eine Liste mit für die Bereitstellung vorbereiteten virtuellen Netzwerkgeräten finden Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Die IP-Weiterleitung wird in der Regel mit benutzerdefinierten Routen verwendet. Weitere Informationen zu benutzerdefinierten Routen finden Sie in [Benutzerdefinierte Routen](virtual-networks-udr-overview.md).

1. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, wählen Sie dies aus.
2. Wählen Sie die Netzwerkschnittstelle aus, für die Sie die IP-Weiterleitung aktivieren oder deaktivieren möchten.
3. Wählen Sie **IP-Konfigurationen** im Abschnitt **EINSTELLUNGEN** aus.
4. Wählen Sie **Aktiviert** oder **Deaktiviert** (Standardeinstellung) aus, um die Einstellung zu ändern.
5. Wählen Sie **Speichern**aus.

**Befehle**

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic update](/cli/azure/network/nic#az_network_nic_update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="change-subnet-assignment"></a>Ändern der Subnetzzuweisung

Sie können das Subnetz, aber nicht das virtuelle Netzwerk ändern, dem eine Netzwerkschnittstelle zugewiesen ist.

1. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, wählen Sie dies aus.
2. Wählen Sie die Netzwerkschnittstelle aus, für die Sie die Subnetzzuweisung ändern möchten.
3. Wählen Sie **IP-Konfigurationen** unter **EINSTELLUNGEN** aus. Falls neben privaten IP-Adressen für aufgeführte IP-Konfigurationen **(Statisch)** angegeben ist, müssen Sie die IP-Adresszuweisungsmethode in „Dynamisch“ ändern, indem Sie die folgenden Schritte ausführen. Alle privaten IP-Adressen müssen mit der dynamischen Zuweisungsmethode zugewiesen werden, um die Subnetzzuweisung für die Netzwerkschnittstelle zu ändern. Falls die Adressen mithilfe der dynamischen Methode zugewiesen wurden, fahren Sie mit Schritt 5 fort. Falls IPv4-Adressen mit der statischen Zuweisungsmethode zugewiesen wurden, führen Sie die folgenden Schritte aus, um die Zuweisungsmethode in „Dynamisch“ zu ändern:
    - Wählen Sie in der Liste mit den IP-Konfigurationen die IP-Konfiguration aus, deren IPv4-Adresszuweisungsmethode Sie ändern möchten.
    - Wählen Sie **Dynamisch** für die Methode der **Zuweisung** der privaten IP-Adresse aus. Sie können IPv6-Adressen nicht mit der statischen Zuweisungsmethode zuweisen.
    - Wählen Sie **Speichern**aus.
4. Wählen Sie in der Dropdownliste **Subnetz** das Subnetz aus, zu dem Sie die Netzwerkschnittstelle verschieben möchten.
5. Wählen Sie **Speichern**aus. Neue dynamische Adressen werden aus dem Subnetzadressbereich des neuen Subnetzes zugewiesen. Nach dem Zuweisen der Netzwerkschnittstelle zu einem neuen Subnetz können Sie nach Wunsch eine statische IPv4-Adresse aus dem Adressbereich des neuen Subnetzes zuweisen. Weitere Informationen zum Hinzufügen, Ändern und Entfernen von IP-Adressen für eine Netzwerkschnittstelle finden Sie unter [Verwalten von IP-Adressen](virtual-network-network-interface-addresses.md).

**Befehle**

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic ip-config update](/cli/azure/network/nic/ip-config#az_network_nic_ip_config_update)|
|PowerShell|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig)|


## <a name="delete-a-network-interface"></a>Löschen einer Netzwerkschnittstelle

Sie können eine Netzwerkschnittstelle löschen, solange diese nicht an einen virtuellen Computer angefügt ist. Wenn sie an einen virtuellen Computer angefügt ist, müssen Sie den virtuellen Computer zunächst in den Status „Beendet (Zuordnung aufgehoben)“ versetzen und dann die Netzwerkschnittstelle vom virtuellen Computer trennen, bevor Sie die Netzwerkschnittstelle löschen können. Die Schritte zum Trennen einer Netzwerkschnittstelle von einem virtuellen Computer finden Sie unter [Trennen einer Netzwerkschnittstelle von einem virtuellen Computer](virtual-network-network-interface-vm.md#remove-a-network-interface-from-a-vm). Beim Löschen eines virtuellen Computers werden alle angefügten Netzwerkschnittstellen getrennt, die Netzwerkschnittstellen werden aber nicht gelöscht.

1. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, wählen Sie dies aus.
2. Wählen Sie **...** auf der rechten Seite der Netzwerkschnittstelle aus, die Sie aus der Liste der Netzwerkschnittstellen löschen möchten.
3. Klicken Sie auf **Löschen**.
4. Wählen Sie **Ja** aus, um das Löschen der Netzwerkschnittstelle zu bestätigen.

Wenn Sie eine Netzwerkschnittstelle löschen, werden alle ihr zugewiesenen MAC- und IP-Adressen freigegeben.

**Befehle**

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic delete](/cli/azure/network/nic#az_network_nic_delete)|
|PowerShell|[Remove-AzureRmNetworkInterface](/powershell/module/azurerm.network/remove-azurermnetworkinterface)|

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Sie virtuelle Computer mit mehreren Netzwerkschnittstellen oder IP-Adressen erstellen:

**Befehle**

|Aufgabe|Tool|
|---|---|
|Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Erstellen eines virtuellen Computers mit einer NIC und mehreren IPv4-Adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Erstellen eines virtuellen Computers mit einer NIC und einer IPv6-Adresse (mit Azure Load Balancer)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-Vorlage](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
