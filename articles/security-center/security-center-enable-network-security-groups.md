---
title: Aktivieren der Netzwerk-Sicherheitsgruppen in Azure Security Center | Microsoft Docs
description: Dieses Dokument veranschaulicht das Implementieren der Empfehlung Azure Security Center ** aktivieren Netzwerk Sicherheit Gruppen **.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 1e034d59d8847f237fa0d4c772344d45cd618576
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Aktivieren der Netzwerk-Sicherheitsgruppen in Azure Security Center
Azure Security Center empfiehlt, dass Sie eine Netzwerksicherheitsgruppe (NSG) aktivieren, wenn nicht bereits aktiviert ist. NSGs enthalten eine Liste der Zugriffssteuerungsliste (ACL)-Regeln, die zulassen oder verweigern den Netzwerkdatenverkehr zu Ihrem VM-Instanzen in einem virtuellen Netzwerk. NSGs können Subnetzen oder einzelnen VM-Instanzen in diesem Subnetz zugeordnet werden. Wenn eine NSG einem Subnetz zugeordnet ist, gelten die ACL-Regeln für alle VM-Instanzen in diesem Subnetz. Darüber hinaus kann Datenverkehr an einen einzelnen virtuellen Computer eingeschränkt werden durch das Zuordnen einer NSG direkt an den virtuellen Computer weiter. Um zu erfahren, weitere finden Sie unter [neuerungen eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)

Wenn Sie keine NSGs aktiviert Security Center stellt zwei Empfehlungen: Aktivieren von Netzwerksicherheitsgruppen für Subnetze und Netzwerksicherheitsgruppen auf virtuellen Computern zu aktivieren. Sie wählen Sie die Ebene, Subnetz oder den virtuellen Computer NSGs anwenden.

> [!NOTE]
> Dieses Dokument wird den Dienst mithilfe einer beispielbereitstellung eingeführt.  Dies ist keiner detaillierten Anleitung erfahren.
>
>

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
1. In der **Empfehlungen** Blatt select **Netzwerksicherheitsgruppen aktivieren** in Subnetzen oder auf virtuellen Computern.
   ![Aktivieren von Netzwerksicherheitsgruppen][1]
2. Daraufhin wird das Blatt **fehlende Netzwerksicherheitsgruppen konfigurieren** für ein Subnetz oder für virtuelle Computer, abhängig von der Empfehlung, die Sie ausgewählt haben. Wählen Sie ein Subnetz oder eine virtuelle Maschine so konfigurieren Sie eine NSG auf.

   ![Konfigurieren der NSG für Subnetz][2]

   ![Konfigurieren der NSG für virtuellen Computer][3]
3. Auf der **Netzwerksicherheitsgruppe auswählen** Blatt, wählen Sie eine vorhandene NSG oder **neu erstellen** eine NSG zu erstellen.

   ![Netzwerksicherheitsgruppe auswählen][4]

Wenn Sie eine NSG erstellen, führen Sie die Schritte in [NSGs mit der Azure-Portal verwalten](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) eine NSG zu erstellen und Festlegen von Sicherheitsregeln.

## <a name="see-also"></a>Weitere Informationen:
In diesem Artikel wurde gezeigt, wie zur Umsetzung der Empfehlung Sicherheitscenter "Netzwerksicherheitsgruppen aktivieren", für die Subnetze oder virtuellen Computern. Weitere Informationen zum Aktivieren von Netzwerksicherheitsgruppen finden Sie unter den folgenden:

* [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [NSGs mit der Azure-Portal verwalten](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Weitere Informationen zum Security Center finden Sie in der folgenden:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren der Sicherheitsrichtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
* [Verwalten von Empfehlungen zur Sicherheit in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen Ihrer Azure-Ressourcen zu schützen.
* [Sicherheit-Systemüberwachung in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie die Integrität der Azure-Ressourcen zu überwachen.
* [Verwalten von und reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten von und reagieren auf Sicherheitswarnungen.
* [Überwachung mit Azure Security Center partnerlösungen](security-center-partner-solutions.md) – erfahren Sie, wie Überwachung des Integritätsstatus von partnerlösungen.
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Diensts suchen.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) --Neuigkeiten zu Azure-Sicherheit und Informationen.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
