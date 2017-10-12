---
title: "Suchen eines Azure-Abonnements oder einer Verwaltungsgruppe – Azure | Microsoft-Dokumentation"
description: 'Gewusst wie: Navigieren zwischen mehreren Verzeichnissen, um Ihre Verwaltungsgruppen und Abonnements anzuzeigen'
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: df4d85d0556a62311c112f24431b54d042333c7f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="find-an-azure-subscription-or-management-group"></a>Suchen nach einem Azure-Abonnement oder einer Verwaltungsgruppe

Wenn Sie ein Abonnement oder eine Verwaltungsgruppe nicht in Azure finden, suchen Sie möglicherweise im falschen Verzeichnis. Diese Situation kann auftreten, wenn Ihr Konto in mehreren Azure Active Directorys vorhanden ist. Jedes [Active Directory ist unabhängig](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-directory-independence), und Zugriffsberechtigungen werden nicht Verzeichnisse übergreifend vererbt.      

![Menü „Verzeichnis wechseln“](media/billing-enterprise-mgmt-groups/mgempty.png)



## <a name="switch-directories"></a>Wechseln von Verzeichnissen 
Sie können im Azure-Portal einfach Verzeichnisse wechseln.
1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2.  Wählen Sie Ihren Namen oben rechts im Bildschirm aus. 
3.  Am unteren Rand des Menüs werden alle Verzeichnisse aufgelistet, auf die Sie zugreifen können.
4.  Wählen Sie den Namen eines Verzeichnisses aus, um darauf zuzugreifen. 

![Menü „Verzeichnis wechseln“](media/billing-enterprise-mgmt-groups/switch-directory.png)

## <a name="asset-is-unavailable"></a>Asset ist nicht verfügbar? 
Wenn Sie beim Versuch, auf ein Abonnement oder eine Verwaltungsgruppe zuzugreifen, die Fehlermeldung „Dieses Asset ist nicht verfügbar“ erhalten, dann besitzen Sie nicht die richtige Rolle, um dieses Element anzuzeigen.  

![Asset nicht gefunden](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Wenden Sie sich an den Administrator des Abonnements oder der Verwaltungsgruppe, um Zugriff zu erhalten.  
* Nutzen Sie für Abonnements das Dokument [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure) als Referenz, um zu ermitteln, welche Rolle erforderlich ist.
* Für Verwaltungsgruppen ist RBAC-Zugriff noch nicht verfügbar, wird jedoch bald verfügbar sein. Bitten Sie Ihren Enterprise Portal-Administrator, Ihnen den Zugriff zuzuweisen.   

## <a name="improve-your-experience-with-management-groups-and-subscriptions-in-the-same-directory"></a>Verbessern Ihrer Erfahrung mit Verwaltungsgruppen und Abonnements im gleichen Verzeichnis 
Sie können Ihre Abonnements oder Verwaltungsgruppen in das Verzeichnis Ihrer Wahl übertragen, damit alles am gleichen Ort vorhanden ist.  Das Konsolidieren von Abonnements und Verwaltungsgruppen im selben Verzeichnis bewirkt, dass weniger Verzeichniswechsel notwendig sind und Richtlinien vererbt werden können.  


### <a name="transfer-your-subscriptions"></a>Übertragen Ihrer Abonnements 
Sie können ein Abonnement in das Verzeichnis verschieben, das Ihren Verwaltungsgruppen zugeordnet ist. Um diese Verschiebung durchzuführen, muss Ihr Registrierungsadministrator Ihr Abonnement in ein Konto im Zielverzeichnis übertragen. Wenn Sie mehr erfahren möchten, melden Sie sich beim [Enterprise Portal](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) an.


 






