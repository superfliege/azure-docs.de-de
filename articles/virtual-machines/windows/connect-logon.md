---
title: Herstellen einer Verbindung mit einem virtuellen Windows Server-Computer | Microsoft Docs
description: Hier erfahren Sie, wie Sie unter Verwendung des Azure-Portals und des Resource Manager-Bereitstellungsmodells eine Verbindung mit einem virtuellen Windows-Computer herstellen und sich bei diesem Computer anmelden.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: cynthn
ms.openlocfilehash: 7c495a74ccbcad3ee18147726742ee59b65f1c0e
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012640"
---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer
Verwenden Sie die Schaltfläche **Verbinden** im Azure-Portal, um eine Remotedesktopsitzung (RDP) von einem Windows-Desktop zu starten. Zuerst stellen Sie eine Verbindung mit dem virtuellen Computer her, und dann melden Sie sich an.

Wenn Sie von einem Mac eine Verbindung mit einem virtuellen Windows-Computer herstellen möchten, müssen Sie einen RDP-Client für Mac installieren, etwa [Microsoft-Remotedesktop](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung mit dem virtuellen Computer
1. Melden Sie sich am [Azure-Portal](https://portal.azure.com/)an, falls Sie dies noch nicht getan haben.
2. Klicken Sie im linken Menü auf **Virtual Machines**.
3. Wählen Sie den gewünschten virtuellen Computer aus der Liste aus.
4. Klicken Sie oben auf der Seite für den virtuellen Computer auf die Schaltfläche ![Abbildung der Schaltfläche „Verbinden“.](./media/connect-logon/connect.png) .
2. Wählen Sie auf der Seite **Connect to virtual machine** (Mit virtuellem Computer verbinden) die passenden Optionen aus, und klicken Sie anschließend auf **RDP-Datei herunterladen**.
2. Öffnen Sie die heruntergeladene RDP-Datei, und klicken Sie auf **Verbinden**, wenn Sie dazu aufgefordert werden. 
2. Es wird eine Warnung angezeigt mit dem Hinweis, dass die `.rdp`-Datei von einem unbekannten Herausgeber stammt. Dies ist normal. Klicken Sie im Fenster "Remotedesktop" auf **Verbinden** , um den Vorgang fortzusetzen.
   
    ![Screenshot einer Warnung zu einem unbekannten Herausgeber](./media/connect-logon/rdp-warn.png)
3. Wählen Sie im Fenster **Windows-Sicherheit** die Option **Weitere Optionen** und dann **Anderes Konto verwenden** aus. Geben Sie die Anmeldeinformationen eines Kontos auf dem virtuellen Computer ein, und klicken Sie anschließend auf **OK**.
   
     **Lokales Konto**: Hierbei handelt es sich in der Regel um den Benutzernamen und das Kennwort des lokalen Kontos, die Sie beim Erstellen des virtuellen Computers angegeben haben. In diesem Fall ist die Domäne der Name des virtuellen Computers. Das Eingabeformat lautet *VM-Name*&#92;*Benutzername*.  
   
    **In die Domäne eingebundener virtueller Computer**: Falls der virtuelle Computer in eine Domäne eingebunden ist, geben Sie den Benutzernamen im Format *Domäne*&amp;#92;*Benutzername* ein. Das Konto muss außerdem entweder zur Gruppe „Administratoren“ gehören oder über Remotezugriffsrechte für den virtuellen Computer verfügen.
   
    **Domänencontroller** – Wenn der virtuelle Computer ein Domänencontroller ist, geben Sie den Benutzernamen und das Kennwort eines Domänenadministratorkontos für diese Domäne an.
4. Klicken Sie auf **Ja** , um die Identität des virtuellen Computers zu bestätigen und das Anmelden zu beenden.
   
   ![Screenshot mit einer Meldung zur Überprüfung der Identität des virtuellen Computers](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Wenn die Schaltfläche **Verbinden** im Portal ausgeblendet ist und keine [ExpressRoute](../../expressroute/expressroute-introduction.md)- oder [Site-to-Site-VPN-Verbindung](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) mit Azure besteht, müssen Sie eine öffentliche IP-Adresse erstellen und dem virtuellen Computer zuweisen, damit Sie RDP nutzen können. Lesen Sie mehr über [öffentliche IP-Adressen in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 


## <a name="next-steps"></a>Nächste Schritte
Falls beim Herstellen der Verbindung Probleme auftreten, helfen Ihnen die Informationen unter [Problembehandlung bei Remotedesktopverbindungen](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)weiter. Dieser Artikel führt Sie durch die Diagnose und Behebung von häufigen Problemen.

