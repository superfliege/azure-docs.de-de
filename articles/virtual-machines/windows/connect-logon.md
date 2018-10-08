---
title: Herstellen einer Verbindung mit einem virtuellen Windows Server-Computer | Microsoft-Dokumentation
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
ms.date: 09/13/2018
ms.author: cynthn
ms.openlocfilehash: b9cce5658b705e9d3255d2662b2a0157a2e548c3
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47409030"
---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer
Verwenden Sie die Schaltfläche **Verbinden** im Azure-Portal, um eine Remotedesktopsitzung (RDP) von einem Windows-Desktop zu starten. Zunächst stellen Sie eine Verbindung mit dem virtuellen Computer her, danach melden Sie sich an.

Um von einem Mac aus eine Verbindung mit einem virtuellen Windows-Computer herzustellen, müssen Sie einen RDP-Client für Mac installieren, z.B. [Microsoft-Remotedesktop](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung mit dem virtuellen Computer
1. Melden Sie sich am [Azure-Portal](https://portal.azure.com/)an, falls Sie dies noch nicht getan haben.
2. Wählen Sie im linken Menü **Virtual Machines** aus.
3. Wählen Sie den gewünschten virtuellen Computer aus der Liste aus.
4. Klicken Sie oben auf der Seite für den virtuellen Computer auf die Schaltfläche **Verbinden**.
2. Wählen Sie auf der Seite **Mit virtuellem Computer verbinden** die passenden Optionen aus, und klicken Sie anschließend auf **RDP-Datei herunterladen**.
2. Öffnen Sie die heruntergeladene RDP-Datei, und klicken Sie auf **Verbinden**, wenn Sie dazu aufgefordert werden. 
2. Es wird eine Warnung mit dem Hinweis angezeigt, dass die RDP-Datei von einem unbekannten Herausgeber stammt. Dies entspricht dem erwarteten Verhalten. Klicken Sie im Fenster **Remotedesktopverbindung** auf **Verbinden**, um fortzufahren.
   
    ![Screenshot einer Warnung zu einem unbekannten Herausgeber](./media/connect-logon/rdp-warn.png)
3. Wählen Sie im Fenster **Windows-Sicherheit** die Option **Weitere Optionen** und dann **Anderes Konto verwenden** aus. Geben Sie die Anmeldeinformationen eines Kontos auf dem virtuellen Computer ein, und klicken Sie anschließend auf **OK**.
   
     **Lokales Konto**: Hierbei handelt es sich in der Regel um den Benutzernamen und das Kennwort des lokalen Kontos, den bzw. das Sie beim Erstellen des virtuellen Computers angegeben haben. In diesem Fall ist die Domäne der Name des virtuellen Computers. Das Eingabeformat lautet *VM-Name*&#92;*Benutzername*.  
   
    **In die Domäne eingebundener virtueller Computer**: Wenn der virtuelle Computer zu einer Domäne gehört, geben Sie den Benutzernamen im Format *Domäne*&#92;*Benutzername* ein. Das Konto muss außerdem entweder zur Gruppe „Administratoren“ gehören oder über Remotezugriffsrechte für den virtuellen Computer verfügen.
   
    **Domänencontroller**: Wenn der virtuelle Computer ein Domänencontroller ist, geben Sie den Benutzernamen und das Kennwort eines Domänenadministratorkontos für diese Domäne an.
4. Klicken Sie auf **Ja**, um die Identität des virtuellen Computers zu bestätigen und den Anmeldevorgang abzuschließen.
   
   ![Screenshot mit einer Meldung zur Überprüfung der Identität des virtuellen Computers](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Wenn die Schaltfläche **Verbinden** im Portal ausgeblendet ist und keine [ExpressRoute](../../expressroute/expressroute-introduction.md)- oder [Site-to-Site-VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)-Verbindung mit Azure besteht, müssen Sie eine öffentliche IP-Adresse erstellen und dem virtuellen Computer zuweisen, damit Sie RDP nutzen können. Weitere Informationen finden Sie unter [Öffentliche IP-Adressen in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 


## <a name="next-steps"></a>Nächste Schritte
Wenn beim Herstellen einer Verbindung Probleme auftreten, lesen Sie den Artikel [Problembehandlung bei Remotedesktopverbindungen](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

