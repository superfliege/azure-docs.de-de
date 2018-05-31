---
title: Öffnen von Ports für einen virtuellen Computer mithilfe des Azure-Portals | Microsoft Docs
description: Erfahren Sie, wie Sie mit dem Resource Manager-Bereitstellungsmodell im Azure-Portal für Ihren virtuellen Windows-Computer einen Port öffnen oder einen Endpunkt erstellen.
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: a64e2bbe1bb784f0b6032980d6f212470549cdf4
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34366918"
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Öffnen von Ports zu einem virtuellen Computer mit dem Azure-Portal
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Schnellbefehle
Sie können [diese Schritte auch über Azure PowerShell ausführen](nsg-quickstart-powershell.md).

Erstellen Sie zuerst Ihre Netzwerksicherheitsgruppe. Wählen Sie im Portal eine Ressourcengruppe aus, wählen Sie **Hinzufügen**, suchen Sie nach **Netzwerksicherheitsgruppe**, und wählen Sie diese Option aus:

![Hinzufügen einer Netzwerksicherheitsgruppe](./media/nsg-quickstart-portal/add-nsg.png)

Geben Sie einen Namen für die Netzwerksicherheitsgruppe ein, wählen oder erstellen Sie eine Ressourcengruppe, und wählen Sie einen Speicherort aus: Wenn Sie fertig sind, klicken Sie auf **Erstellen**:

![Erstellen einer Netzwerksicherheitsgruppe](./media/nsg-quickstart-portal/create-nsg.png)

Wählen Sie die neue Netzwerksicherheitsgruppe aus. Wählen Sie „Eingangssicherheitsregeln“, und wählen Sie dann zum Erstellen einer Regel die Schaltfläche **Hinzufügen** aus:

![Hinzufügen einer eingehenden Regel](./media/nsg-quickstart-portal/add-inbound-rule.png)

So erstellen Sie eine Regel, die Datenverkehr zulässt:

- Wählen Sie die Schaltfläche **Einfach** aus. Im Fenster **Erweitert** werden standardmäßig einige zusätzliche Konfigurationsoptionen bereitgestellt, z. B. für die Definition eines bestimmten Quell-IP-Blocks oder Portbereichs.
- Wählen Sie im Dropdownmenü einen allgemeinen **Dienst**, z.B. *HTTP*, aus. Sie können auch *Benutzerdefiniert* auswählen, um einen bestimmten zu verwendenden Port anzugeben. 
- Sie können die Priorität oder den Namen ggf. ändern. Die Priorität wirkt sich auf die Reihenfolge aus, in der Regeln angewendet werden – je niedriger der numerische Wert, desto früher wird die Regel angewendet.
- Wenn Sie fertig sind, wählen Sie **OK** aus, um die Regel zu erstellen:

![Erstellen einer eingehenden Regel](./media/nsg-quickstart-portal/create-inbound-rule.png)

Im letzten Schritt muss die Netzwerksicherheitsgruppe einem Subnetz oder einer bestimmten Netzwerkschnittstelle zugeordnet werden. So ordnen Sie die Netzwerksicherheitsgruppe einem Subnetz zu: Wählen Sie **Subnetze** und dann **Zuordnen** aus:

![Zuordnen einer Netzwerksicherheitsgruppe zu einem Subnetz](./media/nsg-quickstart-portal/associate-subnet.png)

Wählen Sie das virtuelle Netzwerk und dann das gewünschte Subnetz aus:

![Zuordnen einer Netzwerksicherheitsgruppe zu einem virtuellen Netzwerk](./media/nsg-quickstart-portal/select-vnet-subnet.png)

Sie haben nun eine Netzwerksicherheitsgruppe sowie eine eingehende Regel erstellt, die Datenverkehr über Port 80 ermöglicht, und haben diese einem Subnetz zugeordnet. Alle virtuellen Computer, die Sie mit diesem Subnetz verbinden, sind über Port 80 erreichbar.

## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zu Netzwerksicherheitsgruppen
Mit diesen Schnellbefehlen können Sie den Datenverkehr zu Ihrem virtuellen Computer einrichten. Netzwerksicherheitsgruppen bieten eine Vielzahl erstklassiger Funktionen sowie eine differenzierte Steuerung des Ressourcenzugriffs. Weitere Informationen über die [Erstellung von Netzwerksicherheitsgruppen und ACL-Regeln erhalten Sie hier](../../virtual-network/tutorial-filter-network-traffic.md).

Bei hoch verfügbaren Webanwendungen sollten Sie die virtuellen Computer hinter einem Azure Load Balancer anordnen. Der Load Balancer verteilt den Datenverkehr auf virtuelle Computer, mit einer Netzwerksicherheitsgruppe, die den Datenverkehr filtert. Weitere Informationen finden Sie unter [Lastenausgleich für virtuelle Linux-Computer in Azure zum Erstellen einer hoch verfügbaren Anwendung](tutorial-load-balancer.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel erstellt, die HTTP-Datenverkehr zulässt. Informationen zum Erstellen von detaillierteren Umgebungen finden Sie in den folgenden Artikeln:

* [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Was ist eine Netzwerksicherheitsgruppe?](../../virtual-network/security-overview.md)