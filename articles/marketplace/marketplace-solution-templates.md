---
title: Leitfaden für die Veröffentlichung von Lösungsvorlagenangeboten für Azure-Anwendungen
description: In diesem Artikel werden die Anforderungen für die Veröffentlichung von Lösungsvorlagen im Marketplace beschrieben.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 07/09/2018
ms.author: ellacroi
ms.openlocfilehash: 44d081a0666aa37ec0bf8eeac540b7a7f4f4f904
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39057882"
---
# <a name="azure-applications-solution-template-offer-publishing-guide"></a>Azure-Anwendungen: Leitfaden für die Veröffentlichung von Lösungsvorlagenangeboten

Lösungsvorlagen zählen zu den wichtigsten Verfahren, eine Lösung im Marketplace zu veröffentlichen. Informieren Sie sich in diesem Handbuch über die Anforderungen für dieses Angebot. 

Hierbei handelt es sich um Transaktionsangebote, die über den Marketplace bereitgestellt und abgerechnet werden. Die Benutzer sehen hier den Aktionsaufruf „Jetzt kaufen“.

Verwenden Sie eine Lösungsvorlage für Azure-Apps, wenn für eine Lösung neben der einfachen VM noch eine zusätzliche Bereitstellung und Konfigurationsautomatisierung erforderlich ist. Sie können die Bereitstellung einer oder mehrerer VMs mithilfe von Azure-Apps automatisieren: Lösungsvorlagen. Sie können auch Ressourcen für Netzwerke und Speicher zur Verfügung stellen. Azure-Apps: Angebotstyp der Lösungsvorlagen bietet Automatisierungsvorteile für Einzel-VMs und gesamte IaaS-basierte Lösungen.

## <a name="requirements-for-solution-templates"></a>Anforderungen für Lösungsvorlagen

|Requirements (Anforderungen) |Details  |
|---------|---------|
|Abrechnung und Messung    |  Die Ressourcen werden im Azure-Abonnement des Kunden bereitgestellt. Für VMs mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYGO) erfolgt die Transaktion mit dem Kunden über Microsoft und die Abrechnung über das Azure-Abonnement des Kunden (PAYGO). 
Im Fall von Bring-Your-Own-License rechnet Microsoft die angefallenen Infrastrukturkosten im Kundenabonnement ab, während Sie Ihre Softwarelizenzgebühren direkt mit dem Kunden abrechnen.        |
|Azure-kompatible virtuelle Festplatte (VHD)    |   VMs müssen unter Windows oder Linux erstellt werden.<ul> <li>Weitere Informationen zum Erstellen einer Linux-VHD finden Sie unter „Erstellen einer Azure-kompatiblen VHD (Linux-basiert)“ unter [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2-create-an-azure-compatible-vhd-linux-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2-create-an-azure-compatible-vhd-linux-based).</li> <li>Weitere Informationen zum Erstellen einer Windows-VHD finden Sie unter „Erstellen einer Azure-kompatiblen VHD (Windows-basiert)“ unter [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3-create-an-azure-compatible-vhd-windows-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3-create-an-azure-compatible-vhd-windows-based).</li> </ul>      |



## <a name="next-steps"></a>Nächste Schritte
Falls Sie dies noch nicht getan haben, 

- [registrieren](https://azuremarketplace.microsoft.com/sell) Sie sich im Marketplace.

Wenn Sie registriert sind und ein neues Angebot erstellen oder an einem vorhandenen arbeiten,

- [melden Sie sich beim Cloud-Partnerportal an](https://cloudpartner.azure.com), um Ihr Angebot zu erstellen oder zu vervollständigen.
