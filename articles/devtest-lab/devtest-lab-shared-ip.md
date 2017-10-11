---
title: Verstehen Sie gemeinsam genutzte IP-Adressen in Azure DevTest Labs | Microsoft Docs
description: "Erfahren Sie, wie Azure DevTest Labs freigegebene IP-Adressen verwendet, minimieren Sie die öffentliche IP-Adressen erforderlich, um Ihre Lab-VMs zuzugreifen."
services: devtest-lab
documentationcenter: na
author: camsoper
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: casoper
ms.openlocfilehash: 9f6e1980bf5ea5b41da98a135d89f1c5159921a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Verstehen Sie gemeinsam genutzte IP-Adressen in Azure DevTest-Labs

Azure DevTest Labs können Lab-VMs, die gemeinsam dieselbe öffentliche IP-Adresse zum Minimieren der Anzahl der öffentlichen IP-Adressen erforderlich, um die einzelnen Lab VMs zuzugreifen.  In diesem Artikel wird beschrieben, wie freigegebene IP-Adressen arbeiten und ihren verwandten Konfigurationsoptionen.

## <a name="shared-ip-setting"></a>Freigegebene IP-Einstellung

Wenn Sie ein Labor erstellen, befindet sich in einem Subnetz eines virtuellen Netzwerks.  Standardmäßig wird dieses Subnetz mit erstellt **shared öffentliche IP-Adresse aktivieren** festgelegt *Ja*.  Diese Konfiguration wird eine öffentliche IP-Adresse für das gesamte Subnetz erstellt.  Weitere Informationen zum Konfigurieren virtueller Netzwerke und Subnetze finden Sie unter [Konfigurieren eines virtuellen Netzwerks in Azure DevTest Labs](devtest-lab-configure-vnet.md).

![Neue Lab-Subnetz](media/devtest-lab-shared-ip/lab-subnet.png)

Für vorhandene Labs, können Sie diese Option aktivieren, durch Auswahl **Konfigurations- und Richtlinien > virtuelle Netzwerke**. Wählen Sie ein virtuelles Netzwerk aus der Liste aus und wählen Sie **aktivieren FREIGEGEBENE öffentliche IP-Adresse** für eine ausgewählte Subnetz. Sie können mit dieser Option alle Lab auch deaktivieren, wenn Sie eine öffentliche IP-Adresse für Lab-VMs freigeben möchten.

Alle virtuellen Computer in dieser Übungseinheit wird standardmäßig unter Verwendung einer freigegebenen IP-Adresse erstellt.  Wenn Sie den virtuellen Computer zu erstellen, kann diese Einstellung beobachtet werden, der **Erweiterte Einstellungen** Blatt unter **IP-Adresskonfiguration**.

![Neue virtuelle Maschine](media/devtest-lab-shared-ip/new-vm.png)

- **Shared:** alle virtuellen Computer erstellt, die als **Shared** befinden sich in einer Ressourcengruppe (RG). Eine einzelne IP-Adresse wird zugewiesen, RG und alle virtuellen Computer in der Quellroutinggruppe dieser IP-Adresse verwendet werden.
- **Öffentlich:** jeder virtuelle Computer, die Sie erstellen, verfügt über eine eigene IP-Adresse und wird in einem eigenen Ressourcengruppe erstellt.
- **Private:** jeder virtuelle Computer, die Sie erstellen, wird eine private IP-Adresse verwendet. Sie werden nicht direkt aus dem Internet mit Remotedesktop eine Verbindung mit diesem virtuellen Computer sein.

Das Subnetz ein virtuellen Computers mit den freigegebenen IP aktiviert hinzugefügt wird, fügt den virtuellen Computer mit einem Lastenausgleichsmodul DevTest Labs automatisch und weist eine TCP-Portnummer an, auf die öffentliche IP-Adresse an den RDP-Port auf dem virtuellen Computer weitergeleitet werden.  

## <a name="using-the-shared-ip"></a>Verwenden die freigegebene IP-Adresse

- **Linux-Benutzer:** SSH eine Verbindung mit dem virtuellen Computer mithilfe der IP-Adresse oder den vollqualifizierten Domänennamen, gefolgt von einem Doppelpunkt, gefolgt von den Port. In der folgenden Abbildung wird die RDP-Adresse für die Verbindung mit dem virtuellen Computer z. B. `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.

  ![VM-Beispiel](media/devtest-lab-shared-ip/vm-info.png)

- **Windows-Benutzer:** wählen Sie die **verbinden** auf Azure-Portal zum Herunterladen einer vorkonfigurierten RDP-Datei und Zugriff auf den virtuellen Computer auf die Schaltfläche.

## <a name="next-steps"></a>Nächste Schritte

* [Definieren von Lab-Richtlinien in Azure DevTest-Labs](devtest-lab-set-lab-policy.md)
* [Konfigurieren eines virtuellen Netzwerks in Azure DevTest-Labs](devtest-lab-configure-vnet.md)





