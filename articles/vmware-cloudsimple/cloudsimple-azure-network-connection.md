---
title: VMware-Lösung von CloudSimple – Azure-Netzwerkverbindungen
description: Erfahren Sie mehr über das Verbinden Ihres virtuellen Azure-Netzwerks mit Ihrem CloudSimple-Regionsnetzwerk.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8ea98d6493b824bfa232ef8193388e93b97c506b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2019
ms.locfileid: "64576812"
---
# <a name="azure-network-connection-overview"></a>Übersicht über Azure-Netzwerkverbindungen

Wenn Sie einen CloudSimple-Dienst in einer Region erstellen, passiert Folgendes:

* Es wird eine Azure ExpressRoute-Verbindung erstellt, und diese wird dem Dienst in dieser Region zugeordnet.
* Es wird eine Verbindung Ihres CloudSimple-Regionsnetzwerks mit Ihrem virtuellen Azure-Netzwerk oder Ihrem lokalen Netzwerk über Azure ExpressRoute hergestellt.
* Es wird ermöglicht, dass Sie aus der Umgebung Ihrer privaten Cloud Zugriff auf die Dienste haben, die in Ihrem Azure-Abonnement oder lokalen Netzwerk ausgeführt werden.

Diese Verbindung hat eine hohe Bandbreite mit geringer Wartezeit (Latenz).

## <a name="benefits"></a>Vorteile

Eine Azure-Netzwerkverbindung ermöglicht Ihnen Folgendes:

* Verwenden von Azure als Sicherungsziel für virtuelle Computer in Ihrer privaten Cloud
* Bereitstellen von KMS-Servern in Ihrem Azure-Abonnement, um den vSAN-Datenspeicher Ihrer privaten Cloud zu verschlüsseln
* Verwenden von Hybridanwendungen, bei denen die Webebene der jeweiligen Anwendung in der öffentlichen Cloud ausgeführt wird, während die Anwendungs- und die Datenbankebene in Ihrer privaten Cloud ausgeführt werden

## <a name="expressroute-connection-to-on-premises-network"></a>ExpressRoute-Verbindung mit lokalem Netzwerk

Sie können Ihre vorhandene Azure ExpressRoute-Verbindung (Netzwerk) mit Ihrer CloudSimple-Region verbinden. Über den ExpressRoute-Dienst Global Reach werden die beiden Netzwerke miteinander verbunden.  Es wird eine Verbindung zwischen dem lokalen und dem CloudSimple-ExpressRoute-Netzwerk hergestellt.

Mit dieser Methode wird eine Verbindung zwischen den beiden Umgebungen hergestellt, die folgende Merkmale hat:

* Schützen
* Private
* Hohe Bandbreite
* Geringe Wartezeit

Um eine ExpressRoute-Verbindung mit einem lokalen Netzwerk herzustellen, [wenden Sie sich an den Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

* [Connect Azure virtual network to CloudSimple using ExpressRoute](https://docs.azure.cloudsimple.com/virtual-network-connection)