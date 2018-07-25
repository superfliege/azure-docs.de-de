---
title: Leitfaden für die Veröffentlichung von VM-Angeboten für Azure Marketplace
description: Dieser Artikel beschreibt die Anforderungen zum Veröffentlichen einer VM und einer kostenlosen Softwaretestversion, die auf dem Marketplace bereitgestellt werden.
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
ms.openlocfilehash: 5508b5943e116545297d91e85621d2a11a635299
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39058072"
---
# <a name="virtual-machine-offer-publishing-guide"></a>Leitfaden für die Veröffentlichung von VM-Angeboten

VM-Images zählen zu den wichtigsten Verfahren, eine Lösung im Azure Marketplace zu veröffentlichen. Informieren Sie sich in diesem Handbuch über die Anforderungen für dieses Angebot. 

Hierbei handelt es sich um Transaktionsangebote, die über den Marketplace bereitgestellt und abgerechnet werden. Die Benutzer sehen hier den Aktionsaufruf „Jetzt kaufen“.

## <a name="free-trial"></a>Kostenlose Testversion 

Bei Verwendung des Abrechnungsmodells Bring-Your-Own-License (BYOL) können Sie arrangieren, dass Benutzer zum Testen Ihres Angebots Zugriff mittels zeitlich begrenzter Softwarelizenzen erhalten. Im Folgenden sind die Anforderungen zum Bereitstellen dieses Angebots aufgeführt. 

|Requirements (Anforderungen)  |Details  |
|---------|---------|
|Kostenloser Testzeitraum und Testversion     |   Ihr Kunde kann Ihre App für begrenzte Zeit kostenlos ausprobieren. Ihr Kunde muss keine Lizenz- oder Abonnementgebühren für Ihr Angebot bezahlen. Ihr Kunde muss nicht für das zugrunde liegende Erstanbieterprodukt bzw. den zugrunde liegenden Erstanbieterdienst von Microsoft bezahlen. Alle Testoptionen werden in Ihrem Azure-Abonnement bereitgestellt. Sie haben alleinige Kontrolle über die Kostenoptimierung und -verwaltung. Sie können eine kostenlose Testversion oder eine interaktive Demo auswählen. Unabhängig von Ihrer Wahl muss Ihre kostenlose Testversion dem Kunden einen vorab festgelegten Zeitraum bereitstellen, in dem er Ihr Angebot ohne zusätzliche Kosten testen kann.|
|Einfach konfigurierbare und einsatzbereite Lösungen    |  Ihre App muss sich einfach und schnell konfigurieren und einrichten lassen.       |
|Verfügbarkeit/Betriebszeit    |    Ihre SaaS-App oder -Plattform benötigt eine Betriebszeit von mindestens 99,9 %.     |
|Azure Active Directory     |    Ihr Angebot muss die einmalige Azure Active Directory-Verbundanmeldung (SSO) mit aktivierter Zustimmung zulassen.     |

## <a name="test-drive"></a>Testversion

Sie stellen eine oder mehrere VMs über IaaS- (Infrastructure-As-A-Service) oder SaaS-Apps bereit. Ein Vorteil der Testversion-Veröffentlichungsoption ist die automatische Bereitstellung einer VM oder einer gesamten Lösung mit einer von einem Partner gehosteten geführten Tour. Eine Testversion stellt Ihrem Kunden eine Bewertung ohne Zusatzkosten zur Verfügung. Ihr Kunde muss nicht bereits Azure-Kunde sein, um die Testversion zu nutzen. 

Senden Sie eine E-Mail an amp-testdrive@microsoft.com, um zu beginnen. 

|Requirements (Anforderungen)  |Details |
|---------|---------|
| Sie verfügen über eine Marketplace-App   |    Mindestens eine VM über IaaS oder SaaS.      |

## <a name="interactive-demo"></a>Interaktive Demo

Mit einer interaktiven Demo bieten Sie Ihren Kunden eine geführte Einführungen in Ihr Produkt. Der Vorteil der Veröffentlichungsoption der interaktiven Demo ist, dass Sie ohne komplizierte Bereitstellung für komplexe Lösungen eine Testversion bieten können. 

## <a name="virtual-machine-offer"></a>VM-Angebot

Verwenden Sie den VM-Angebotstyp beim Bereitstellen einer virtuellen Appliance für das Abonnement Ihres Kunden. VMs können vollständig kommerziell genutzt werden und verwenden nutzungsbasierte oder Bring-Your-Own-Licence-Lizenzierungsmodelle (BYOL). Microsoft hostet die kommerzielle Transaktion und führt für Sie die Abrechnung mit Ihrem Kunden durch. Sie haben den Vorteil, dass Sie die bevorzugte Zahlungsmethoden zwischen Ihrem Kunden und Microsoft, einschließlich des Enterprise Agreements, nutzen können.

>[!NOTE]
>Zu diesem Zeitpunkt können die finanziellen Verpflichtungen des Enterprise Agreements für die Azure-Nutzung Ihrer VM, nicht aber für Ihre Softwarelizenzgebühren verwendet werden.  

>[!NOTE]
>Sie können die Ermittlung und Bereitstellung Ihrer VM auf eine bestimmte Gruppe von Kunden beschränken, indem Sie das Image und die Preise als privates Angebot veröffentlichen. Durch private Angebote haben Sie die Möglichkeit, exklusive Angebote für Ihre besten Kunden zu erstellen und benutzerdefinierte Software und Bestimmungen für sie anzubieten. Benutzerdefinierte Bestimmungen ermöglichen Ihnen die Schaffung vielfältiger Szenarios, wie etwa vom Vertrieb erzielte Geschäftsabschlüsse mit Sonderpreisen und -bestimmungen sowie ein frühzeitiger Zugang zu Software, die in limitierter Anzahl veröffentlicht wird. Durch private Angebote können Sie Sonderpreise oder -produkte für eine begrenzte Anzahl von Kunden anbieten, indem Sie eine neue SKU mit diesen Konditionen erstellen.  
*   Weitere Informationen zu privaten Angeboten finden Sie auf der Seite für private Angebote in Azure Marketplace unter [azure.microsoft.com/blog/private-offers-on-azure-marketplace](https://azure.microsoft.com/blog/private-offers-on-azure-marketplace).  

| Anforderung | Details |  
|:--- |:--- | 
| Abrechnung und Messung | Ihre VM muss entweder BYOL oder die monatliche nutzungsbasierte Bezahlung unterstützen. |  
| Azure-kompatible virtuelle Festplatte (VHD) | VMs müssen unter Windows oder Linux erstellt werden.<ul> <li>Weitere Informationen zum Erstellen einer Linux-VHD finden Sie unter „Erstellen einer Azure-kompatiblen VHD (Linux-basiert)“ unter [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2-create-an-azure-compatible-vhd-linux-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2-create-an-azure-compatible-vhd-linux-based).</li> <li>Weitere Informationen zum Erstellen einer Windows-VHD finden Sie unter „Erstellen einer Azure-kompatiblen VHD (Windows-basiert)“ unter [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3-create-an-azure-compatible-vhd-windows-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3-create-an-azure-compatible-vhd-windows-based).</li> </ul> |  

## <a name="next-steps"></a>Nächste Schritte

Falls Sie dies noch nicht getan haben, 

- [registrieren](https://azuremarketplace.microsoft.com/sell) Sie sich im Marketplace.

Wenn Sie registriert sind und ein neues Angebot erstellen oder an einem vorhandenen arbeiten,

- [melden Sie sich beim Cloud-Partnerportal an](https://cloudpartner.azure.com), um Ihr Angebot zu erstellen oder zu vervollständigen.
