---
title: Aktivieren von Netzwerksicherheitsgruppen in Azure Security Center | Microsoft Docs
description: In diesem Dokument erfahren Sie, wie Sie die Azure Security Center-Empfehlung **Netzwerksicherheitsgruppen aktivieren** implementieren.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 3c0ad4a0e1a5f4f2fd6def4f29599e2e55eb1a9d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34364453"
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Aktivieren von Netzwerksicherheitsgruppen in Azure Security Center
Azure Security Center empfiehlt die Aktivierung einer Netzwerksicherheitsgruppe (NSG), sofern noch keine aktiviert ist. NSGs enthalten eine Zugriffssteuerungsliste (Access Control List, ACL) zum Zulassen oder Verweigern von eingehendem Netzwerkdatenverkehr für Ihre VM-Instanzen in einem virtuellen Netzwerk. NSGs können Subnetzen oder einzelnen VM-Instanzen innerhalb dieses Subnetzes zugeordnet werden. Wenn eine NSG einem Subnetz zugeordnet ist, gelten die ACL-Regeln für alle VM-Instanzen in diesem Subnetz. Darüber hinaus kann Datenverkehr zu einem einzelnen virtuellen Computer weiter beschränkt werden, indem eine NSG direkt diesem virtuellen Computer zugewiesen wird. Weitere Informationen finden Sie unter [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/security-overview.md)

Sollten keine Netzwerksicherheitsgruppen aktiviert sein, stellt Security Center zwei Empfehlungen bereit: „Netzwerksicherheitsgruppen in Subnetzen aktivieren“ und „Netzwerksicherheitsgruppen auf virtuellen Computern aktivieren“. Sie haben die Wahl, auf welcher Ebene (Subnetz oder virtueller Computer) Sie NSGs anwenden möchten.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Es ist keine schrittweise Anleitung.
>
>

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
1. Wählen Sie auf dem Blatt **Empfehlungen** aus, ob **Netzwerksicherheitsgruppen** in Subnetzen oder auf virtuellen Computern aktiviert werden sollen.
   ![][1]
2. Daraufhin wird das Blatt zum **Konfigurieren fehlender Netzwerksicherheitsgruppen** für Subnetze oder virtuelle Computer geöffnet (je nachdem, welche Empfehlung Sie ausgewählt haben). Wählen Sie ein Subnetz oder einen virtuellen Computer aus, um eine Netzwerksicherheitsgruppe dafür zu konfigurieren.

   ![Konfigurieren der NSG (Subnetz)][2]

   ![Konfigurieren der NSG (virtueller Computer)][3]
3. Wählen Sie auf dem Blatt **Netzwerksicherheitsgruppe auswählen** eine vorhandene NSG aus, oder wählen Sie **Neu erstellen** aus, um eine NSG zu erstellen.

   ![Netzwerksicherheitsgruppe auswählen][4]

Falls Sie eine NSG erstellen möchten, führen Sie die Schritte unter [Verwalten von NSGs mithilfe des Azure-Portals](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) aus, um eine NSG zu erstellen und Sicherheitsregeln festzulegen.

## <a name="see-also"></a>Weitere Informationen
In diesem Artikel haben Sie erfahren, wie Sie die von Security Center empfohlene Aktivierung von Netzwerksicherheitsgruppen für Subnetze oder virtuelle Computer implementieren. Weitere Informationen zu NSGs finden Sie hier:

* [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/security-overview.md)
* [Verwalten einer Netzwerksicherheitsgruppe](../virtual-network/manage-network-security-group.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) : Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und auf diese reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
