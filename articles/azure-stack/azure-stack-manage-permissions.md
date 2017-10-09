---
title: "Verwalten von Berechtigungen für Ressourcen pro Benutzer in Azure Stack (Dienstadministrator und Mandant) | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie als Dienstadministrator oder Mandant RBAC-Berechtigungen verwalten.
services: azure-stack
documentationcenter: 
author: Heathl17
manager: byronr
editor: 
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: e558f9de9bc3182bbe20ceb9d8f3f96e47fa542c
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="manage-role-based-access-control"></a>Verwalten der rollenbasierten Zugriffssteuerung

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Ein Benutzer in Azure Stack kann ein Leser, ein Besitzer oder ein Mitwirkender an jeder Instanz eines Abonnements, einer Ressourcengruppe oder eines Diensts sein. Beispielsweise kann Benutzer A Leseberechtigungen für Abonnement 1 besitzen, aber auch die Besitzberechtigungen für Virtueller Computer 7.

* Leser: Der Benutzer kann alles anzeigen, jedoch keine Änderungen vornehmen.
* Mitwirkende: Der Benutzer kann alles mit Ausnahme des Zugriffs auf Ressourcen verwalten.
* Besitzer: Der Benutzer kann alles verwalten, einschließlich des Zugriffs auf Ressourcen.

## <a name="set-access-permissions-for-a-user"></a>Festlegen von Zugriffsberechtigungen für Benutzer
1. Melden Sie sich mit einem Konto an, das Besitzerberechtigungen für die Ressource hat, die Sie verwalten möchten.
2. Klicken Sie auf dem Blatt für die Ressource auf das Symbol **Zugriff** ![](media/azure-stack-manage-permissions/image1.png).
3. Klicken Sie auf dem Blatt **Benutzer** auf **Rollen**.
4. Klicken Sie auf dem Blatt **Rollen** auf **Hinzufügen**, um Berechtigungen für den Benutzer hinzuzufügen.

## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen von Azure Stack-Mandanten](azure-stack-add-new-user-aad.md)


