---
title: Anmelden bei einem klassischen virtuellen Azure-Computer | Microsoft Docs
description: Melden Sie sich im Azure-Portal bei einem virtuellen Windows-Computer an, der mit dem klassischen Bereitstellungsmodell erstellt wurde.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 87ecc65d2d4802ae826f3260b66b26e0bbe414e6
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012358"
---
# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-portal"></a>Anmelden bei einem virtuellen Windows-Computer über das Azure-Portal
Im Azure-Portal können Sie über die Schaltfläche **Verbinden** eine Remotedesktopsitzung starten und sich bei einem virtuellen Windows-Computer anmelden.

Möchten Sie eine Verbindung mit einem virtuellen Linux-Computer herstellen? Siehe [Anmelden bei einem virtuellen Computer, auf dem Linux ausgeführt wird](../../linux/mac-create-ssh-keys.md).

<!--
Deleting, but not 100% sure
Learn how to [perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Informationen zum Anmelden bei einem virtuellen Computer im Resource Manager-Modell finden Sie [hier](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung mit dem virtuellen Computer
1. Melden Sie sich beim Azure-Portal an.
2. Klicken Sie auf den virtuellen Computer, auf den Sie zugreifen möchten. Der Name ist im Bereich **Alle Ressourcen** aufgeführt.

    ![Virtual-machine-locations](./media/connect-logon/azureportaldashboard.png)

1. Klicken Sie auf der Seite mit den Eigenschaften des virtuellen Computers auf die Schaltfläche **Verbinden**. 
2. Wählen Sie auf der Seite **Connect to virtual machine** (Verbindung mit virtuellem Computer herstellen) die passenden Optionen aus, und klicken Sie anschließend auf **RDP-Datei herunterladen**.
2. Öffnen Sie die heruntergeladene RDP-Datei, und klicken Sie auf **Verbinden**, wenn Sie dazu aufgefordert werden. 
2. Es wird eine Warnung angezeigt mit dem Hinweis, dass die `.rdp`-Datei von einem unbekannten Herausgeber stammt. Dies ist normal. Klicken Sie im Fenster "Remotedesktop" auf **Verbinden** , um den Vorgang fortzusetzen.
   
    ![Screenshot einer Warnung zu einem unbekannten Herausgeber](./media/connect-logon/rdp-warn.png)
3. Wählen Sie im Fenster **Windows-Sicherheit** die Option **Weitere Optionen** und dann **Anderes Konto verwenden** aus. Geben Sie die Anmeldeinformationen eines Kontos auf dem virtuellen Computer ein, und klicken Sie anschließend auf **OK**.
   
     **Lokales Konto**: Hierbei handelt es sich in der Regel um den Benutzernamen und das Kennwort des lokalen Kontos, die Sie beim Erstellen des virtuellen Computers angegeben haben. In diesem Fall ist die Domäne der Name des virtuellen Computers. Das Eingabeformat lautet *VM-Name*&#92;*Benutzername*.  
   
    **In die Domäne eingebundener virtueller Computer**: Falls der virtuelle Computer in eine Domäne eingebunden ist, geben Sie den Benutzernamen im Format *Domäne*&amp;#92;*Benutzername* ein. Das Konto muss außerdem entweder zur Gruppe „Administratoren“ gehören oder über Remotezugriffsrechte für den virtuellen Computer verfügen.
   
    **Domänencontroller** – Wenn der virtuelle Computer ein Domänencontroller ist, geben Sie den Benutzernamen und das Kennwort eines Domänenadministratorkontos für diese Domäne an.
4. Klicken Sie auf **Ja** , um die Identität des virtuellen Computers zu bestätigen und das Anmelden zu beenden.
   
   ![Screenshot mit einer Meldung zur Überprüfung der Identität des virtuellen Computers](./media/connect-logon/cert-warning.png)

## <a name="next-steps"></a>Nächste Schritte
* Wenn die Schaltfläche **Verbinden** inaktiv ist oder andere Probleme mit der Remotedesktopverbindung auftreten, versuchen Sie, die Konfiguration zurückzusetzen. Klicken Sie im Dashboard des virtuellen Computers auf **Remotezugriff zurücksetzen**.

    ![Reset-remote-access](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* Wenn Sie Probleme mit Ihrem Kennwort haben, setzen Sie es zurück. Klicken Sie links im Dashboard des virtuellen Computers unter **Support und Problembehandlung** auf **Kennwort zurücksetzen**.

    ![Reset-password](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

Wenn diese Tipps nicht funktionieren oder nicht das Gesuchte sind, finden Sie weitere Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Dieser Artikel führt Sie durch die Diagnose und Behebung von häufigen Problemen.
