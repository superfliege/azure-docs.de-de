---
title: "Anzeigen der Nutzung öffentlicher IP-Adressen in Azure Stack | Microsoft-Dokumentation"
description: "Administratoren können die Nutzung öffentlicher IP-Adressen in einer Region anzeigen."
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 0f77be49-eafe-4886-8c58-a17061e8120f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: scottnap
ms.openlocfilehash: 7651565eebf6272f307a4ce4790ca19b41bfa826
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>Anzeigen der Nutzung öffentlicher IP-Adressen in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Als Cloudadministrator können Sie die Anzahl der öffentlichen IP-Adressen anzeigen, die Mandanten zugeordnet wurden, die Anzahl der öffentlichen IP-Adressen, die weiterhin für die Zuordnung verfügbar sind, sowie den Prozentsatz der öffentlichen IP-Adressen, die an diesem Speicherort zugewiesen wurden.

Die Kachel **Nutzung öffentlicher IP-Pools** zeigt die Gesamtanzahl der öffentlichen IP-Adressen, die in allen öffentlichen IP-Adresspools im Fabric verwendet wurden, und ob sie für IaaS-VM-Instanzen von Mandanten, Fabricinfrastrukturdienste oder öffentliche IP-Adressressourcen verwendet wurden, die von den Mandanten explizit erstellt wurden.

Diese Kachel soll Azure Stack-Administratoren einen Eindruck von der Gesamtanzahl öffentlicher IP-Adressen vermitteln, die an diesem Speicherort verwendet wurden. So können Administratoren feststellen, ob diese Ressource knapp wird.

Auf dem Blatt **Ressourcenanbieter**, **Netzwerk** listet das Menüelement **Öffentliche IP-Adressen** unter **Mandantenressourcen** nur die öffentlichen IP-Adressen auf, die *explizit von den Mandanten erstellt* wurden. Die Anzahl der **Verwendeten** öffentlichen IP-Adressen auf der Kachel **Nutzung öffentlicher IP-Pools** unterscheidet sich immer von der Anzahl auf der Kachel **Öffentliche IP-Adressen** unter **Mandantenressourcen** (ist größer).

## <a name="view-the-public-ip-address-usage-information"></a>Anzeigen der Informationen zur Nutzung öffentlicher IP-Adressen
So zeigen Sie die Gesamtanzahl der öffentlichen IP-Adressen an, die in der Region verwendet wurden:

1. Klicken Sie im Azure Stack-Administratorportal auf **Weitere Dienste**, und klicken Sie unter **Verwaltungsressourcen** auf **Ressourcenanbieter**.
2. Wählen Sie aus der Liste **Ressourcenanbieter** die Option **Netzwerk**.
3. Das Blatt **Netzwerk** zeigt die Kachel **Nutzung öffentlicher IP-Pools** im Abschnitt **Übersicht** an.

![Blatt „Netzwerkressourcenanbieter“](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

Beachten Sie, dass der Wert von **Verwendet** die Anzahl der öffentlichen IP-Adressen aus allen öffentlichen IP-Adresspools an diesem Speicherort angibt, die zugewiesen sind. Der Wert von **Frei** gibt die Anzahl der öffentlichen IP-Adressen von allen öffentlichen IP-Adresspools an, die nicht zugewiesen wurden und weiterhin verfügbar sind. Der Wert von **% verwendet** gibt die Anzahl der verwendeten oder zugewiesenen Adressen als Prozentsatz der Gesamtzahl der öffentlichen IP-Adressen in allen öffentlichen IP-Adresspools an diesem Speicherort an.

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>Anzeigen der öffentlichen IP-Adressen, die von Mandantenabonnements erstellt wurden
Um eine Liste der öffentlichen IP-Adressen anzuzeigen, die explizit von Mandantenabonnements in einer bestimmten Region erstellt wurden, klicken Sie auf **Öffentliche IP-Adressen** unter **Mandantenressourcen**.

![Öffentliche IP-Adressen von Mandanten](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

Sie stellen möglicherweise fest, dass einige öffentliche IP-Adressen, die dynamisch zugewiesen wurden, in der Liste angezeigt werden, ihnen jedoch keine Adresse zugeordnet ist. Der Grund ist, dass die Adressressource im Netzwerkressourcenanbieter erstellt wurde, jedoch noch nicht im Netzwerkcontroller.

Der Netzwerkcontroller weist dieser Ressource erst dann eine Adresse zu, wenn sie tatsächlich an eine Schnittstelle, einen Netzwerkadapter, ein Lastenausgleichsmodul oder ein Gateway für virtuelle Netzwerke gebunden ist. Wenn die öffentliche IP-Adresse an eine Schnittstelle gebunden ist, weist der Netzwerkcontroller ihr eine IP-Adresse zu, und sie wird im Feld **Adresse** angezeigt.

## <a name="view-the-public-ip-address-information-summary-table"></a>Übersichtstabelle zum Anzeigen der Informationen zu öffentlichen IP-Adressen
Es gibt eine Reihe verschiedener Fälle, in denen öffentliche IP-Adressen zugewiesen werden, die bestimmen, ob die Adresse in der einen oder anderen Liste angezeigt wird.

| **Zuweisungsfall der öffentlichen IP-Adresse** | **Wird in Nutzungsübersicht angezeigt** | **Wird in Liste öffentlicher IP-Adressen von Mandanten angezeigt** |
| --- | --- | --- |
| Dynamische öffentliche IP-Adresse, die noch nicht einem Netzwerkadapter oder Lastenausgleichsmodul zugewiesen ist (temporär) |Nein |Ja |
| Dynamische öffentliche IP-Adresse, die einem Netzwerkadapter oder Lastenausgleichsmodul zugewiesen ist. |Ja |Ja |
| Statische öffentliche IP-Adresse, die einem Mandanten-Netzwerkadapter oder Lastenausgleichsmodul zugewiesen ist. |Ja |Ja |
| Statische öffentliche IP-Adresse, die einem Fabricinfrastruktur-Dienstendpunkt zugewiesen ist. |Ja |Nein |
| Öffentliche IP-Adresse, die implizit für IaaS-VM-Instanzen erstellt wird und für ausgehende NAT im virtuellen Netzwerk verwendet wird. Diese werden im Hintergrund erstellt, wenn ein Mandant eine VM-Instanz erstellt, sodass virtuelle Computer Informationen an das Internet senden können. |Ja |Nein |

## <a name="next-steps"></a>Nächste Schritte
[Verwalten von Speicherkonten in Azure Stack](azure-stack-manage-storage-accounts.md)