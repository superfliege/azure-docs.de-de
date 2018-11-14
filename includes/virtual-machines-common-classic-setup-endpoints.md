---
title: Includedatei
description: Includedatei
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 10/23/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: e7dfd7d2a0363a95acb76a5dc214dbd4036de11d
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50973797"
---
Jeder Endpunkt verfügt über einen *öffentlichen* und einen *privaten* Port:

* Der öffentliche Port wird von Azure Load Balancer verwendet, um auf den Datenverkehr zu lauschen, der aus dem Internet auf dem virtuellen Computer eingeht.
* Der private Port wird vom virtuellen Computer zum Überwachen von eingehendem Datenverkehr verwendet, in der Regel für eine Anwendung oder ein Dienst, der auf dem virtuellen Computer ausgeführt wird.

Die Standardwerte für das IP-Protokoll und die TCP- oder UDP-Ports für bekannte Netzwerkprotokolle werden bereitgestellt, wenn Sie mit dem Azure-Portal Endpunkte erstellen. Geben Sie bei benutzerdefinierten Endpunkten das richtige Internetprotokoll (TCP oder UDP) und die öffentlichen und privaten Ports an. Um eingehenden Datenverkehr nach dem Zufallsprinzip auf mehrere virtuelle Computer zu verteilen, erstellen Sie eine Gruppe mit Lastenausgleich, die mehrere Endpunkte umfasst.

Nachdem Sie einen Endpunkt erstellt haben, können Sie eine Zugriffssteuerungsliste (ACL) zum Definieren von Regeln verwenden, die eingehenden Datenverkehr an den öffentlichen Port des Endpunkts basierend der Quell-IP-Adresse zulassen oder verweigern. Wenn sich der virtuelle Computer in einem virtuellen Azure-Netzwerk befindet, verwenden Sie stattdessen Netzwerksicherheitsgruppen. Weitere Informationen finden Sie unter [Informationen zu Netzwerksicherheitsgruppen](../articles/virtual-network/security-overview.md).

> [!NOTE]
> Firewallkonfiguration für virtuelle Computer in Azure erfolgt automatisch für Ports, die mit Remotekonnektivität-Endpunkten verknüpft sind, die Azure automatisch einrichtet. Für Ports, die für alle anderen Endpunkte angegeben werden, wird keine Konfiguration der Firewall des virtuellen Computers automatisch durchgeführt. Wenn Sie einen Endpunkt für den virtuellen Computer erstellen, stellen Sie außerdem sicher, dass die Firewall des virtuellen Computers den Datenverkehr für das Protokoll und den privaten Port gemäß der Endpunktkonfiguration ermöglicht. Informationen zum Konfigurieren der Firewall finden Sie in der Dokumentation oder der Onlinehilfe für das Betriebssystem, das auf dem virtuellen Computer ausgeführt wird.
>
>

## <a name="create-an-endpoint"></a>Erstellen eines Endpunkts
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie **Virtuelle Computer** und dann die VM aus, die Sie konfigurieren möchten.

3. Wählen Sie in der Gruppe **Einstellungen** **Endpunkte** aus. Daraufhin wird die Seite **Endpunkte** angezeigt, die alle aktuellen Endpunkte für den virtuellen Computer auflistet. (Dieses Beispiel bezieht sich auf eine Windows-VM. Eine Linux-VM weist standardmäßig einen Endpunkt für SSH auf.)

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![Endpunkte](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)


4. Wählen Sie in der Befehlsleiste über den Endpunkteinträgen **Hinzufügen** aus. Daraufhin wird die Seite **Endpunkt hinzufügen** angezeigt.

5. Geben Sie für **Name** einen Endpunktnamen ein.

6. Wählen Sie für **Protokoll** entweder **TCP** oder **UDP** aus.

7. Geben Sie für **Öffentlicher Port** die Portnummer für den eingehenden Datenverkehr aus dem Internet ein. 

8. Geben Sie unter **Privater Port** die Nummer des Ports an, an dem der virtuelle Computer lauscht. Die Nummern von öffentlichen und privaten Ports können sich unterscheiden. Stellen Sie sicher, dass die Firewall auf dem virtuellen Computer so konfiguriert wurde, dass der Datenverkehr dem Protokoll und dem privaten Port entspricht.

9. Klicken Sie auf **OK**.

Der neue Endpunkt wird auf der Seite **Endpunkte** aufgeführt.

![Endpunkt erfolgreich erstellt](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-the-acl-on-an-endpoint"></a>Verwaltung der ACL für einen Endpunkt
Zum Definieren der Computer, die Datenverkehr senden können, kann die ACL für einen Endpunkt Datenverkehr anhand der IP-Quelladresse einschränken. Gehen Sie folgendermaßen vor, um eine ACL an einem Endpunkt hinzuzufügen, zu modifizieren oder zu entfernen.

> [!NOTE]
> Wenn der Endpunkt Teil eines Satzes mit Lastenausgleich ist, werden alle Änderungen, die Sie an der ACL oder an einem Endpunkt vornehmen, auf alle Endpunkte in diesem Satz angewendet.
>
>

Wenn sich der virtuelle Computer in einem virtuellen Azure-Netzwerk befindet, verwenden Sie Netzwerksicherheitsgruppen statt ACLs. Weitere Informationen finden Sie unter [Informationen zu Netzwerksicherheitsgruppen](../articles/virtual-network/security-overview.md).

1. Melden Sie sich beim Azure-Portal an.

2. Wählen Sie **Virtuelle Computer** und dann den Namen der VM aus, die Sie konfigurieren möchten.

3. Wählen Sie **Endpunkte**. Wählen Sie in der Liste der Endpunkte den entsprechenden Endpunkt aus. Die ACL-Liste befindet sich unten auf der Seite.

   ![ACL-Details festlegen](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. Verwenden Sie die Zeilen in der Liste zum Hinzufügen, Löschen oder Bearbeiten von Regeln für eine ACL und Änderung ihrer Reihenfolge. Der Wert für **Remotesubnetz** ist ein IP-Adressbereich für eingehenden Datenverkehr aus dem Internet, den Azure Load Balancer verwendet, um Datenverkehr basierend auf der IP-Quelladresse zuzulassen oder zu verweigern. Geben Sie den IP-Adressbereich im CIDR-Format (klassenloses domänenübergreifendes Routing) an, auch als Adresspräfixformat bezeichnet. Beispiel: `10.1.0.0/8`.

 ![Neuer ACL-Eintrag](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)


Sie können Regeln verwenden, um nur Verkehr von bestimmten Computern zuzulassen, die Ihren Computern im Internet entsprechen, oder um Datenverkehr von bestimmten, bekannten Adressbereichen abzulehnen.

Die Regeln werden der Reihe nach, beginnend mit der ersten und endend mit der letzten Regel, bewertet. Deswegen müssen Regeln nach Einschränkungen geordnet werden – von der geringsten bis zur stärksten Einschränkung. Weitere Informationen finden Sie unter [Was ist eine Netzwerk-Zugriffssteuerungsliste?](../articles/virtual-network/virtual-networks-acl.md).
