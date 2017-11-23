---
title: "Installieren von Replikatdomänencontrollern für eine lokale Active Directory-Domäne auf virtuellen Azure-Computern | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Replikatdomänencontroller für eine lokale Active Directory-Domäne auf virtuellen Azure-Computern (VMs) in einem virtuellen Azure-Netzwerk installieren."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 60839f93954bfe38f0346b235259f68e479b8a00
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Installieren eines Active Directory-Replikatdomänencontrollers in einem virtuellen Azure-Netzwerk
In diesem Artikel wird beschrieben, wie zusätzliche Domänencontroller (DCs) als Replikatdomänencontroller für eine lokale Active Directory-Domäne auf virtuellen Azure-Computern in einem virtuellen Azure-Netzwerk installiert werden. Sie können auch [eine Windows Server Active Directory-Gesamtstruktur in einem virtuellen Azure-Netzwerk installieren](active-directory-new-forest-virtual-machine.md). Informationen zur Installation von Active Directory Domain Services (AD DS) in einem virtuellen Azure-Netzwerk finden Sie unter [Richtlinien für die Bereitstellung von Windows Server Active Directory auf virtuellen Computern in Microsoft Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Szenariodiagramm
In diesem Szenario müssen externe Benutzer auf Anwendungen zugreifen, die auf in die Domäne eingebundenen Servern ausgeführt werden. Die virtuellen Computer, auf denen die Anwendungsserver und die Replikatdomänencontroller ausgeführt werden, sind in einem virtuellen Azure-Netzwerk installiert. Das virtuelle Netzwerk kann per [ExpressRoute](../expressroute/expressroute-locations-providers.md) mit dem lokalen Netzwerk verbunden werden, oder Sie können eine [Site-to-Site-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)-Verbindung verwenden. Dies ist hier dargestellt: 

![Diagramm eines Active Directory-Replikatdomänencontrollers in einem virtuellen Azure-Netzwerk][1]

Die Anwendungsserver und Domänencontroller werden in separaten Clouddiensten bereitgestellt, um die Computeverarbeitung zu verteilen, sowie in Verfügbarkeitsgruppen, um eine bessere Fehlertoleranz zu erzielen.
Die Domänencontroller werden durch die Active Directory-Replikation untereinander und mit lokalen Domänencontroller repliziert. Es sind keine Synchronisierungstools erforderlich.

## <a name="create-an-on-premises-active-directory-site-for-the-azure-virtual-network"></a>Erstellen eines lokalen Active Directory-Standorts für das virtuelle Azure-Netzwerk
Sie können einen Standort in Active Directory erstellen, der die Netzwerkregion gemäß dem virtuellen Netzwerk darstellt. Durch diesen Standort können die Authentifizierung, die Replikation sowie andere Vorgänge am Standort des Domänencontrollers optimiert werden. In den folgenden Schritten wird erläutert, wie ein Standort erstellt wird. Weitere Hintergrundinformationen erhalten Sie unter [Hinzufügen eines neuen Standorts](https://technet.microsoft.com/library/cc781496.aspx).

1. Öffnen Sie „Active Directory-Standorte und -Dienste“: **Server Manager** > **Tools** > **Active Directory-Standorte und -Dienste**.
2. Erstellen Sie einen Standort, der die Region darstellt, in der Sie ein virtuelles Azure-Netzwerk erstellt haben: Klicken Sie auf **Standorte** > **Aktion** > **Neuer Standort** > geben Sie den Namen des neuen Standorts ein, z.B. Azure USA, Westen > wählen Sie eine Standortverknüpfung aus > **OK**.
3. Erstellen Sie ein Subnetz, und ordnen Sie es dem neuen Standort zu: Doppelklicken Sie auf **Standorte** > klicken Sie mit der rechten Maustaste auf **Subnetze** > **Neues Subnetz** > geben Sie den IP-Adressbereich des virtuellen Netzwerks ein (z.B. 10.1.0.0/16 im Szenariodiagramm) > wählen Sie den neuen Azure-Standort aus > **OK**.

## <a name="create-an-azure-virtual-network"></a>Erstellen eines virtuellen Azure-Netzwerks
Führen Sie die Schritte im Artikel [Erstellen einer Site-to-Site-Verbindung im Azure-Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) aus, um ein virtuelles Azure-Netzwerk zu erstellen und die Site-to-Site-VPN-Verbindung einzurichten. 

Sie können auch das Gateway des virtuellen Netzwerks konfigurieren, um eine sichere Site-to-Site-VPN-Verbindung zu erstellen. Erstellen Sie die Site-to-Site-VPN-Verbindung zwischen dem neuen virtuellen Netzwerk und einem lokalen VPN-Gerät. Anweisungen hierzu finden Sie unter [Konfigurieren des Gateways für das virtuelle Netzwerk](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="create-azure-vms-for-the-dc-roles"></a>Erstellen von virtuellen Azure-Computern für die DC-Rollen
Wiederholen Sie je nach Bedarf die Schritte unter [Erstellen einer Windows-VM im Azure-Portal](../virtual-machines/windows/quick-create-portal.md), um VMs zum Hosten der DC-Rolle zu erstellen. Stellen Sie mindestens zwei virtuelle DCs bereit, um Fehlertoleranz und Redundanz zu gewährleisten. Wenn im virtuellen Azure-Netzwerk mindestens zwei DCs enthalten sind, die ähnlich konfiguriert sind, können Sie die VMs für die Ausführung dieser DCs in einer Verfügbarkeitsgruppe anordnen, um die Fehlertoleranz zu verbessern.

Lesen Sie [Verwenden von Azure PowerShell zum Erstellen und Vorabkonfigurieren Windows-basierter virtueller Computer](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), wenn Sie die virtuellen Computer mithilfe von Windows PowerShell anstelle des Azure-Portals erstellen möchten.

Reservieren Sie eine statische IP-Adresse für virtuelle Computer, auf denen die DC-Rolle ausgeführt wird. Laden Sie zum Reservieren einer statischen IP-Adresse den Microsoft-Webplattform-Installer herunter, [installieren Sie Azure PowerShell](/powershell/azure/overview) , und führen Sie anschließend das Cmdlet "Set-AzureStaticVNetIP" aus. Beispiel:

````
Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM
````
Weitere Informationen zum Festlegen einer statischen IP-Adresse finden Sie unter [Konfigurieren einer statischen internen IP-Adresse für einen virtuellen Computer](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Installieren von AD DS auf virtuellen Azure-Computern
Melden Sie sich bei einem virtuellen Computer an, und stellen Sie sicher, dass Sie über die Site-to-Site-VPN-Verbindung bzw. die ExpressRoute-Verbindung mit Ressourcen im lokalen Netzwerk verbunden sind. Installieren Sie dann AD DS auf den virtuellen Azure-Computern. Sie können denselben Prozess wie bei der Installation eines zusätzlichen Domänencontrollers im lokalen Netzwerk verwenden (UI, Windows PowerShell oder eine Antwortdatei). Stellen Sie beim Installieren von AD DS sicher, dass Sie das neue Volume für den Standort der AD-Datenbank, der Protokolle und von SYSVOL angeben. Wenn Sie Ihre Kenntnisse über die AD DS-Installation auffrischen möchten, finden Sie unter [Installieren von Active Directory Domain Services (Stufe 100)](https://technet.microsoft.com/library/hh472162.aspx) oder [Installieren eines Windows Server 2012-Domänencontrollerreplikats in einer vorhandenen Domäne (Stufe 200)](https://technet.microsoft.com/library/jj574134.aspx) weitere Informationen.

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Erneutes Konfigurieren des DNS-Servers für das virtuelle Netzwerk
1. Suchen Sie zum Anzeigen einer Liste mit Namen von virtuellen Netzwerken im [Azure-Portal](https://portal.azure.com) nach *Virtuelle Netzwerke*, und wählen Sie anschließend **Virtuelle Netzwerke**. 
2. Öffnen Sie das zu verwaltende virtuelle Netzwerk, und [konfigurieren Sie dann die IP-Adressen der DNS-Server für das virtuelle Netzwerk neu](../virtual-network/virtual-network-manage-network.md#dns-servers), sodass anstelle der IP-Adressen für lokale DNS-Server die den Replikatdomänencontrollern zugewiesenen statischen IP-Adressen verwendet werden.
3. Stellen Sie wie folgt sicher, dass alle Replikat-DC-VMs im virtuellen Netzwerk für die Verwendung der entsprechenden DNS-Server konfiguriert sind:
  1. Wählen Sie **Virtuelle Computer**.
  2. Wählen Sie die VMs aus, und klicken Sie anschließend auf **Neu starten**. 
  3. Warten Sie, bis die VM wieder den Status **Wird ausgeführt** hat, und melden Sie sich dann an.

## <a name="create-vms-for-application-servers"></a>Erstellen von virtuellen Computern für Anwendungsserver

Wiederholen Sie je nach Bedarf die Schritte unter [Erstellen einer Windows-VM im Azure-Portal](../virtual-machines/windows/quick-create-portal.md), um VMs zum Hosten der Anwendungsserverrolle zu erstellen. Lesen Sie [Verwenden von Azure PowerShell zum Erstellen und Konfigurieren Windows-basierter virtueller Computer](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), um die virtuellen Computer mithilfe von Microsoft PowerShell anstelle der Benutzeroberfläche zu erstellen. Die folgende Tabelle enthält die empfohlenen Einstellungen.

| Einstellung | Werte |
| --- | --- |
|  **Image auswählen** | Windows Server 2012 R2 Datacenter |
|  **Konfiguration des virtuellen Computers** |<p>Name des virtuellen Computers: Geben Sie einen Namen mit einer einzelnen Bezeichnung ein (z. B. AppServer1).</p><p>Neuer Benutzername: Geben Sie den Namen eines Benutzers ein. Dieser Benutzer wird Mitglied der lokalen Administratorgruppe auf dem virtuellen Computer. Sie benötigen diesen Namen, um sich zum ersten Mal bei dem virtuellen Computer anzumelden. Das integrierte Konto mit dem Namen "Administrator" funktioniert nicht.</p><p>Neues Kennwort/Bestätigen: Geben Sie ein Kennwort ein.</p> |
|  **Konfiguration des virtuellen Computers** |<p>Clouddienst: Wählen Sie für den ersten virtuellen Computer **Einen neuen Cloud-Dienst erstellen** aus, und wählen Sie denselben Cloud-Dienstnamen aus, wenn Sie weitere virtuelle Computer erstellen, die die Anwendung hosten.</p><p>DNS-Name des Clouddiensts: Geben Sie einen global eindeutigen Namen ein.</p><p>Region/Affinitätsgruppe/Virtuelles Netzwerk: Geben Sie den Namen des virtuellen Netzwerks an (z. B. WestUSVNet).</p><p>Speicherkonto: Wählen Sie für den ersten virtuellen Computer **Ein automatisch generiertes Speicherkonto verwenden** aus, und wählen Sie dann denselben Speicherkontonamen aus, wenn Sie weitere virtuelle Computer erstellen, die die Anwendung hosten.</p><p>Verfügbarkeitsgruppe: Wählen Sie **Verfügbarkeitsgruppe erstellen** aus.</p><p>Name der Verfügbarkeitsgruppe: Geben Sie einen Namen für die Verfügbarkeitsgruppe ein, wenn Sie den ersten virtuellen Computer erstellen, und wählen Sie dann denselben Namen aus, wenn Sie weitere virtuelle Computer erstellen.</p> |
|  **Konfiguration des virtuellen Computers** |<p>Wählen Sie <b>VM-Agent installieren</b> und alle anderen erforderlichen Erweiterungen aus.</p> |
  
Nachdem alle virtuellen Computer bereitgestellt wurden, melden Sie sich an, und stellen eine Verbindung zur Domäne her. 
1. Wählen Sie unter **Server-Manager** &gt; **Lokaler Server** &gt; **ARBEITSGRUPPE** &gt; **Ändern…** die Option **Domäne**.
2. Geben Sie den Namen Ihrer lokalen Domäne ein. 
3. Geben Sie die Anmeldeinformationen eines Domänenbenutzers an.
4. Starten Sie den virtuellen Computer neu.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* Weitere Informationen zum Verwenden von Windows PowerShell finden Sie unter [Erste Schritte mit Azure-Cmdlets](/powershell/azure/overview) und [Azure-Cmdlet-Referenz](/powershell/azure/get-started-azureps).
* [Richtlinien für die Bereitstellung von Windows Server Active Directory auf virtuellen Azure-Computern](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Hochladen vorhandener lokaler Hyper-V-Domänencontroller in Azure mithilfe von Azure PowerShell](http://support.microsoft.com/kb/2904015)
* [Installieren einer neuen Active Directory-Gesamtstruktur auf einem virtuellen Azure-Netzwerk](active-directory-new-forest-virtual-machine.md)
* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Microsoft Azure IT Pro IaaS: (01) Grundlagen zu virtuellen Computern](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IT Pro IaaS: (05) Erstellen virtueller Netzwerke und Herstellen standortübergreifender Verbindungen](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure-Verwaltungs-Cmdlets](/powershell/module/azurerm.compute/#virtual_machines)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
