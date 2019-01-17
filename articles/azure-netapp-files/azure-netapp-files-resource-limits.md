---
title: Ressourceneinschränkungen für Azure NetApp Files | Microsoft-Dokumentation
description: Hier werden Grenzwerte für Azure NetApp Files-Ressourcen beschrieben, einschließlich der Grenzwerte für Kapazitätspools, Volumes und das delegierte Subnetz.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 01/03/2019
ms.author: b-juche
ms.openlocfilehash: f34afb1df2ae38353f29a80bfb6798c16856dbeb
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54056518"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Ressourcenlimits für Azure NetApp Files
Grundlegende Informationen zu Ressourceneinschränkungen für Azure NetApp Files unterstützen Sie bei der Verwaltung Ihrer Volumes.

## <a name="capacity_pools"></a>Kapazitätspools

- Die Mindestgröße für einen einzelnen Kapazitätspool beträgt 4 TiB, die Maximalgröße 500 TiB. 
- Jeder einzelne Kapazitätspool kann nur zu einem einzelnen NetApp-Konto gehören. Innerhalb eines NetApp-Kontos können jedoch mehrere Kapazitätspools vorhanden sein.  

## <a name="volumes"></a>Volumes

- Die Mindestgröße für ein einzelnes Volume beträgt 100 GiB, die Maximalgröße 92 TiB.
- Sie können maximal 100 Volumes pro Azure-Abonnement und Region einrichten.  

## <a name="delegated_subnet"></a>Delegiertes Subnetz 

In jedem virtuellen Azure-Netzwerk (VNET) kann nur ein Subnetz an Azure NetApp Files delegiert werden.

## <a name="next-steps"></a>Nächste Schritte

[Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
