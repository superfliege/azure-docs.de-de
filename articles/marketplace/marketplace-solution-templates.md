---
title: Leitfaden für die Veröffentlichung von Lösungsvorlagenangeboten für Azure-Anwendungen
description: In diesem Artikel werden die Anforderungen für die Veröffentlichung von Lösungsvorlagen im Azure Marketplace beschrieben.
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
ms.date: 11/15/2018
ms.author: ellacroi
ms.openlocfilehash: d955f5c11121e8d42bc4b02b75427ab07298e74b
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52264181"
---
# <a name="azure-applications-solution-template-offer-publishing-guide"></a>Azure-Anwendungen: Leitfaden für die Veröffentlichung von Lösungsvorlagenangeboten

Lösungsvorlagen zählen zu den wichtigsten Optionen für die Veröffentlichung von Lösungen im Marketplace. Informieren Sie sich in diesem Handbuch über die Anforderungen für dieses Angebot. 

Verwenden Sie eine Lösungsvorlage für Azure-Apps, wenn für eine Lösung neben einem einzelnen virtuellen Computer noch eine zusätzliche Bereitstellung und Konfigurationsautomatisierung erforderlich ist. Sie können die Bereitstellung einer oder mehrerer VMs mithilfe von Azure-Apps automatisieren: Lösungsvorlagen. Sie können auch Ressourcen für Netzwerke und Speicher zur Verfügung stellen. Azure-Apps: Angebotstyp der Lösungsvorlagen bietet Automatisierungsvorteile für Einzel-VMs und gesamte IaaS-basierte Lösungen.

Bei diesen Lösungsvorlagen handelt es sich um Transaktionsangebote, die über den Marketplace bereitgestellt und abgerechnet werden. Die Benutzer sehen hier den Aktionsaufruf „Jetzt kaufen“.


## <a name="requirements-for-solution-templates"></a>Anforderungen für Lösungsvorlagen

| **Anforderungen** | **Details**  |
| ---------------  | -----------  |
|Abrechnung und Messung    |  Die Ressourcen werden im Azure-Abonnement des Kunden bereitgestellt. Für virtuelle Computer mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYGO) wird die Transaktion mit dem Kunden über Microsoft und die Abrechnung über das Azure-Abonnement des Kunden (PAYGO) abgewickelt.  <br/> Im Falle von Bring-Your-Own-License (BYOL) rechnet Microsoft die im Kundenabonnement angefallenen Infrastrukturkosten ab, während Sie Ihre Softwarelizenzgebühren direkt mit dem Kunden abrechnen.   |
|Azure-kompatible virtuelle Festplatte (VHD)  |   VMs müssen unter Windows oder Linux erstellt werden.  Weitere Informationen finden Sie unter [Erstellen einer Azure-kompatiblen VHD](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md). |
| Zuordnen der Nutzung durch Kunden | Die Zuordnung der Nutzung durch Kunden muss für alle Lösungsvorlagen aktiviert werden, die über den Azure Marketplace veröffentlicht werden. Weitere Informationen zur Zuordnung der Nutzung durch Kunden sowie zu deren Aktivierung finden Sie unter [Zuordnen der Nutzung durch Kunden von Azure-Partnern](./azure-partner-customer-usage-attribution.md).  |
|  |  |


## <a name="next-steps"></a>Nächste Schritte
[Registrieren Sie sich](https://azuremarketplace.microsoft.com/sell) im Marketplace, sofern noch nicht geschehen.

Wenn Sie bereits registriert sind und ein neues Angebot erstellen oder ein bereits vorhandenes Angebot bearbeiten möchten, melden Sie sich beim [Cloud-Partnerportal](https://cloudpartner.azure.com) an, um Ihr Angebot zu erstellen oder fertigzustellen.
