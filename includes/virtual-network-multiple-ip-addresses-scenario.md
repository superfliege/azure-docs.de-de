---
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 11/09/2018
ms.author: genli
ms.openlocfilehash: 3df4108907a4e1e65a444faf1049163966b7accf
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52270337"
---
## <a name="scenario"></a>Szenario
Ein virtueller Computer mit einer einzelnen NIC wird erstellt und mit einem virtuellen Netzwerk verbunden. Der virtuelle Computer benötigt drei verschiedene *private* IP-Adressen und zwei *öffentliche* IP-Adressen. Die IP-Adressen werden den folgenden IP-Konfigurationen zugewiesen:

* **IPConfig-1:** Weist eine *statische* private IP-Adresse und eine *statische* öffentliche IP-Adresse zu.
* **IPConfig-2:** Weist eine *statische* private IP-Adresse und eine *statische* öffentliche IP-Adresse zu.
* **IPConfig-3:** Weist eine *statische* private IP-Adresse und keine öffentliche IP-Adresse zu.
  
    ![Mehrere IP-Adressen](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

Die IP-Konfigurationen werden der NIC zugeordnet, wenn diese erstellt wird, und die NIC wird an den virtuellen Computer angefügt, wenn dieser erstellt wird. Die für das Szenario verwendeten IP-Adresstypen dienen lediglich zur Veranschaulichung. Sie können beliebige IP-Adressen und Zuweisungstypen zuweisen.

> [!NOTE]
> In den Schritten dieses Artikels werden zwar alle IP-Konfigurationen einer einzelnen NIC zugewiesen, es ist jedoch auch möglich, jeder NIC in einem virtuellen Computer mit mehreren NICs mehrere IP-Konfigurationen zuzuweisen. Erfahren Sie im Artikel [Bereitstellen von Multi-NIC-VMs mit PowerShell](../articles/virtual-machines/windows/multiple-nics.md), wie Sie einen virtuellen Computer mit mehreren NICs erstellen.