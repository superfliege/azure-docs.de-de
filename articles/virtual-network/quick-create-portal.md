---
title: "Erstellen eines virtuellen Netzwerks in Azure – Portal | Microsoft-Dokumentation"
description: "Erfahren Sie schnell, wie Sie ein virtuelles Netzwerk im Azure-Portal erstellen. In einem virtuellen Netzwerk können viele Arten von Azure-Ressourcen privat miteinander kommunizieren."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 264dc38383b9adad70325f7fb7802b1dcf2da1c0
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Erstellen eines virtuellen Netzwerks im Azure-Portal

In diesem Artikel erfahren Sie, wie Sie ein virtuelles Netzwerk erstellen. Nach dem Erstellen eines virtuellen Netzwerks stellen Sie zwei virtuelle Computer im virtuellen Netzwerk bereit, die privat untereinander kommunizieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf **+ Neu**.

2. Wählen Sie **Netzwerk** und anschließend **Virtuelles Netzwerk** aus.

3. Geben Sie wie in der folgenden Abbildung *myVirtualNetwork* für **Name** und *myResourceGroup* für **Ressourcengruppe** ein, wählen Sie einen  **Speicherort** und Ihr **Abonnement** aus, akzeptieren Sie die restlichen Standardeinstellungen, und klicken Sie dann auf **Erstellen**. 

    ![Eingeben grundlegender Informationen zu Ihrem virtuellen Netzwerk](./media/quick-create-portal/virtual-network.png)

    Der **Adressraum** wird in CIDR-Notation angegeben. Ein virtuelles Netzwerk enthält null oder mehr Subnetze. Das **Standardadressbereich** für Subnetze von 10.0.0.0/24 nutzt den gesamten Adressbereich des virtuellen Netzwerks, sodass innerhalb des virtuellen Netzwerks kein anderes Subnetz mit dem Standardadressraum und -bereich erstellt werden kann. Der angegebene Adressbereich enthält die IP-Adressen 10.0.0.0 bis 10.0.0.254. Es sind jedoch nur 10.0.0.4 bis 10.0.0.254 verfügbar, da Azure die ersten vier Adressen (0 bis 3) und die letzte Adresse in jedem Subnetz reserviert. Die verfügbaren IP-Adressen werden Ressourcen zugewiesen, die in einem virtuellen Netzwerk bereitgestellt werden.

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

In einem virtuellen Netzwerk können mehrere Arten von Azure-Ressourcen privat miteinander kommunizieren. Eine Art von Ressource, die Sie in einem virtuellen Netzwerk bereitstellen können, ist ein virtueller Computer. Erstellen Sie zwei virtuelle Computer in dem virtuellen Netzwerk, damit Sie die Funktionsweise der Kommunikation zwischen virtuellen Computern in einem virtuellen Netzwerk in einem späteren Schritt überprüfen und verstehen können.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**.

3. Geben Sie die Informationen zum virtuellen Computer gemäß der folgenden Abbildung ein. Der eingegebene **Benutzername** und das **Kennwort** werden zum Anmelden beim virtuellen Computer in einem späteren Schritt verwendet. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen. Wählen Sie Ihr **Abonnement** und die vorhandene Ressourcengruppe *myResourceGroup* aus, und vergewissern Sie sich, dass der ausgewählte **Speicherort** sich am selben Standort befindet, an dem Sie das virtuelle Netzwerk erstellt haben. Klicken Sie zum Abschluss auf **OK**.

    ![Eingeben von grundlegenden Informationen zu einem virtuellen Computer](./media/quick-create-portal/virtual-machine-basics.png)

4. Wählen Sie eine Größe für den virtuellen Computer aus, und klicken Sie auf **Auswählen**. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. Die angezeigten Größen können sich von denen im folgenden Beispiel unterscheiden: 

    ![Auswählen einer Größe für einen virtuellen Computer](./media/quick-create-portal/virtual-machine-size.png)

5. Unter **Einstellungen** sollte bereits *myVirtualNetwork* als **Virtuelles Netzwerk** ausgewählt sein. Ist das nicht der Fall, klicken Sie auf **Virtuelles Netzwerk**, und wählen Sie dann *myVirtualNetwork* aus. Lassen Sie *default* für **Subnetz** ausgewählt, und klicken Sie auf **OK**.

    ![Auswählen eines virtuellen Netzwerks](./media/quick-create-portal/virtual-machine-network-settings.png)

6. Klicken Sie auf der Seite **Zusammenfassung** auf **Erstellen**, um die Bereitstellung des virtuellen Computers zu starten. 

7. Das Erstellen des virtuellen Computers dauert einige Minuten. Nach dem Erstellen wird der virtuelle Computer an das Dashboard des Azure-Portals angeheftet, und die Zusammenfassung des virtuellen Computers wird automatisch geöffnet. Klicken Sie auf **Netzwerk**.

    ![Netzwerkinformationen für den virtuellen Computer](./media/quick-create-portal/virtual-machine-networking.png)

    Sie können sehen, dass die **Private IP**-Adresse *10.0.0.4* lautet. In Schritt 5 haben Sie unter **Einstellungen** das virtuelle Netzwerk *myVirtualNetwork* ausgewählt und das Subnetz *default* für **Subnetz** akzeptiert. Beim [Erstellen des virtuellen Netzwerks](#create-a-virtual-network) haben Sie als Standardwert für das Subnetz den **Adressbereich** 10.0.0.0/24 akzeptiert. Der DHCP-Server von Azure weist die erste verfügbare Adresse für das ausgewählte Subnetz dem virtuellen Computer zu. Da Azure die ersten vier Adressen (0 bis 3) für jedes Subnetz reserviert, ist 10.0.0.4 die erste verfügbare IP-Adresse für das Subnetz.

    Die **Öffentliche IP**-Adresse unterscheidet sich von der dem virtuellen Computer zugewiesenen Adresse. Azure weist in der Standardeinstellung jedem virtuellen Computer eine öffentliche, im Internet routingfähige IP-Adresse zu. Die öffentliche IP-Adresse wird dem virtuellen Computer aus einem [Pool von jeder Azure-Region zugewiesenen Adressen](https://www.microsoft.com/download/details.aspx?id=41653) zugewiesen. Auch wenn Azure bekannt ist, welche öffentliche IP-Adresse einem virtuellen Computer zugewiesen ist, hat das auf einem virtuellen Computer ausgeführte Betriebssystem keine Informationen über die ihm zugewiesene öffentliche IP-Adresse.

8. Führen Sie die Schritte 1–7 erneut aus, geben Sie jedoch in Schritt 3 dem virtuellen Computer den Namen *myVm2*. 

9. Klicken Sie nach dem Erstellen des virtuellen Computers auf **Netzwerk**, wie in Schritt 7. Sie können sehen, dass die **Private IP**-Adresse *10.0.0.5* lautet. Da Azure zuvor die erste verwendbare Adresse *10.0.0.4* im Subnetz dem virtuellen Computer *myVm1* zugewiesen hat, wurde nun dem virtuellen Computer *myVm2* die Adresse *10.0.0.5* zugewiesen, da es sich um die nächste verfügbare Adresse im Subnetz handelte.

## <a name="connect-to-a-virtual-machine"></a>Herstellen einer Verbindung mit einem virtuellen Computer

1. Stellen Sie eine Remoteverbindung mit dem virtuellen Computer *myVm1* her. Geben Sie im oberen Bereich des Azure-Portals *myVm1* ein. Wenn **myVm1** in den Suchergebnissen angezeigt wird, klicken Sie darauf. Klicken Sie auf die Schaltfläche **Verbinden** .

    ![Übersicht über virtuelle Computer](./media/quick-create-portal/virtual-machine-overview.png)


2. Nachdem Sie auf die Schaltfläche **Verbinden** klicken, wird eine RDP-Datei (Remotedesktopprotokoll) erstellt und auf Ihren Computer heruntergeladen.  
3. Öffnen Sie die heruntergeladene RDP-Datei. Klicken Sie in der angezeigten Aufforderung auf **Verbinden**. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben, und klicken Sie dann auf **OK**. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.

## <a name="validate-communication"></a>Überprüfen der Kommunikation

Beim Versuch, einen virtuellen Windows-Computer mit Ping zu erreichen, tritt ein Fehler auf, da Ping über die Windows-Firewall standardmäßig nicht zugelassen wird. Um ein Ping an *myVm1* zu ermöglichen, geben Sie an einer Eingabeaufforderung den folgenden Befehl ein:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Um die Kommunikation mit *myVm2* zu überprüfen, geben Sie an einer Eingabeaufforderung auf dem virtuellen Computer *myVm1* den folgenden Befehl ein. Geben Sie die beim Erstellen des virtuellen Computers verwendeten Anmeldeinformationen an, und vervollständigen Sie die Verbindung:

```
mstsc /v:myVm2
```

Die Remotedesktopverbindung ist erfolgreich, da beide virtuelle Computer über vom Subnetz *default* zugewiesene private IP-Adressen verfügen und Remotedesktop standardmäßig über die Windows-Firewall geöffnet ist. Sie können eine Verbindung mit *myVm2* anhand des Hostnamens herstellen, da Azure automatisch DNS-Namensauflösung für alle Hosts in einem virtuellen Netzwerk bereitstellt. Führen Sie an einer Eingabeaufforderung ein Ping von *myVm2* zu *myVm1* durch.

```
ping myvm1
```

Das Ping ist erfolgreich, da Sie den Befehl in einem vorherigen Schritt durch die Windows-Firewall auf dem virtuellen Computer *myVm1* zugelassen haben. Um die ausgehende Kommunikation mit dem Internet zu bestätigen, geben Sie den folgenden Befehl ein:

```
ping bing.com
```

Sie erhalten vier Antworten von bing.com. Standardmäßig können alle Ressourcen in einem virtuellen Netzwerk in ausgehender Richtung mit dem Internet kommunizieren.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit ihrem gesamten Inhalt, wenn Sie sie nicht mehr benötigen. Geben Sie im oberen Bereich des Azure-Portals *myResourceGroup* ein. Wenn **myResourceGroup** in den Suchergebnissen angezeigt wird, klicken Sie darauf. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie ein virtuelles Standardnetzwerk mit einem Subnetz und zwei virtuellen Computern bereitgestellt. Um zu erfahren, wie Sie ein benutzerdefiniertes virtuelles Netzwerk mit mehreren Subnetzen erstellen und grundlegende Verwaltungsaufgaben ausführen, fahren Sie mit dem Tutorial zum Erstellen und Verwalten eines benutzerdefinierten virtuellen Netzwerks fort.


> [!div class="nextstepaction"]
> [Erstellen und Verwalten eines benutzerdefinierten virtuellen Netzwerks](virtual-networks-create-vnet-arm-pportal.md#portal)