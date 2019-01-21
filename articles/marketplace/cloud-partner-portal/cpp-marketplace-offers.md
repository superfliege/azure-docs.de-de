---
title: Angebote für Azure Marketplace und Microsoft AppSource | Microsoft-Dokumentation
description: Erstellen und Verwalten von Angeboten für Azure Marketplace und Microsoft AppSource
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: pbutlerm
ms.openlocfilehash: 0d6879c6b9be06af4bb289761cc8f26b7e56d18e
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2019
ms.locfileid: "54355127"
---
# <a name="azure-and-appsource-marketplace-offers"></a>Angebote für Azure Marketplace und Microsoft AppSource

Der erste Teil dieses Abschnitts stellt die allgemeinen Abläufe zum Erstellen und Verwalten von Angeboten für Azure Marketplace und Microsoft AppSource vor.  Dieser Teil bietet den Hintergrund, den Sie verstehen müssen, um bestimmte Angebotstypen zu verwalten, sowie technische Informationen, die allen Angebotstypen gemeinsam sind.  Der Großteil dieses Abschnitts enthält ausführliche Anweisungen zum Erstellen und Verwalten bestimmter Angebotstypen.  

Das folgende Video stellt die verschiedenen Funktionen und Angebotstypen in Azure Marketplace oder AppSource vor.  Darüber hinaus werden wichtige technische und wirtschaftliche Aspekte der Veröffentlichung einer Anwendung oder eines Diensts in diesen Marketplaces behandelt.

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK2513/player]

**Erstellen von Apps und Beratungsdiensten für Azure Marketplace und AppSource – Build 2018**

Weitere Informationen zu diesen Marketplaces finden Sie unter [Veröffentlichungsleitfaden für Azure Marketplace und AppSource](../marketplace-publishers-guide.md).


## <a name="azure-marketplace-and-appsource-offer-types"></a>Azure Marketplace- und AppSource-Angebotstypen

Die folgende Tabelle enthält die aktuellen Angebotstypen, die vom [Cloud-Partnerportal](https://cloudpartner.azure.com) unterstützt werden.  Für jeden Angebotstyp sind die Marketplaces, in denen das Angebot aufgeführt werden kann, sowie eine allgemeine Beschreibung der Angebotslösungstechnologie aufgeführt.

|                Angebotstyp                |  Marketplace  |   BESCHREIBUNG                                                           |
|                ----------                |  -----------  |   -----------                                                           |
| [Azure-Anwendung](./azure-applications/cpp-azure-app-offer.md) | Azure | Die Lösung besteht aus einer oder mehreren virtuellen Computern (VMs), optionalem benutzerdefiniertem Azure-Code, der über eine Azure Resource Manager-Vorlage bereitgestellt wird.  Die Bereitstellung kann entweder durch den Kunden über eine Lösungsvorlage oder durch den Herausgeber erfolgen. Dieser Typ wird verwendet, um mehr Flexibilität zu bieten als der VM-Angebotstyp.  |
| [Beratungsdienst](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md) | beide | Microsoft-qualifizierte Berater können ihre domänenspezifischen Dienste entweder in Azure Marketplace oder AppSource aufführen.  Ihre Expertise unterstützt Kunden dabei, ihre Probleme zu beurteilen sowie die richtigen Lösungen zu entwickeln und bereitzustellen, um ihre Geschäftsziele zu erreichen.  |
| [Container](./containers/cpp-containers-offer.md)  | Azure | Die Lösung ist ein Docker-Containerimage, das entweder als Kubernetes-basierter Dienst oder in Form von Azure Containerinstanzen bereitgestellt wird. |
| [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md) | AppSource | Ein Paket, das dieses Enterprise Resource Planning- (ERP) und Business Management-System erweitert. |
| [Dynamics 365 for Customer Engagement](./dyn365ce/cpp-customer-engagement-offer.md) | AppSource | Ein Paket, das dieses Customer Resource Management-System (CRM) um die Module „Vertrieb“, „Service“, „Project Service“ und „Außendienst“ erweitert.  |
| [Dynamics 365 for Finance and Operations](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md) | AppSource | Ein Paket, das diesen Enterprise Resource Planning-Dienst (ERP) erweitert, der erweiterte Features für Finanzierung, operative Vorgänge, Fertigung und Lieferkettenmanagement unterstützt. |
| [IoT Edge-Modul](./iot-edge-module/cpp-offer-process-parts.md) | Azure | Ein Docker-kompatibler Container, der auf einem IoT Edge-Gerät ausgeführt wird.  Er enthält kleine Berechnungsmodule, die eine Kombination aus benutzerdefiniertem Code, anderen Azure-Diensten und Diensten von Drittanbietern verwenden. |
| [SaaS-App](./saas-app/cpp-saas-offer.md) | Azure | Die Lösung ist ein vom Herausgeber verwaltetes Software-as-a-Service-Abonnement, an dem sich die Benutzer über eine benutzerdefinierte Schnittstelle anmelden, und das das Azure Active Directory nutzt. |
| [Virtueller Computer](./virtual-machine/cpp-virtual-machine-offer.md)  | Azure  | Die Lösung ist auf einem einzigen virtuellen Computer enthalten, der im Abonnement des Kunden bereitgestellt wird.  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |   |   |

Weitere Informationen finden Sie im [Leitfaden zur Veröffentlichung nach Angebotstyp](../publisher-guide-by-offer-type.md).


## <a name="next-steps"></a>Nächste Schritte

Welche allgemeinen Vorgänge Sie im Rahmen der Marketplace-Angeboten durchführen können und deren gemeinsame technische Eigenschaften und Objekt erfahren Sie im Thema [Verwalten von Angeboten](./manage-offers/cpp-manage-offers.md).
