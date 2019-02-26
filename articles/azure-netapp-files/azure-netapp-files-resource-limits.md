---
title: Ressourceneinschränkungen für Azure NetApp Files | Microsoft-Dokumentation
description: Hier werden Grenzwerte für Azure NetApp Files-Ressourcen beschrieben, einschließlich der Grenzwerte für NetApp-Konten, Kapazitätspools, Volumes, Momentaufnahmen und das delegierte Subnetz.
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
ms.topic: concepts
ms.date: 02/14/2019
ms.author: b-juche
ms.openlocfilehash: 196d85917e0a9900e141d58bff171beeb8540409
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430012"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Ressourcenlimits für Azure NetApp Files

Grundlegende Informationen zu Ressourceneinschränkungen für Azure NetApp Files unterstützen Sie bei der Verwaltung Ihrer Volumes.

- Jedes Azure-Abonnement kann über maximal 10 NetApp-Konten verfügen.
- Jedes NetApp-Konto kann maximal 25 Kapazitätspools besitzen.
- Jeder einzelne Kapazitätspool kann nur zu einem einzelnen NetApp-Konto gehören.  
- Die Mindestgröße für einen einzelnen Kapazitätspool beträgt 4 TiB, die Maximalgröße 500 TiB. 
- Jeder Kapazitätspool kann maximal 500 Volumes enthalten.
- Die Mindestgröße für ein einzelnes Volume beträgt 100 GiB, die Maximalgröße 92 TiB.
- Jedes Volume kann maximal 255 Momentaufnahmen besitzen.
- Jedes virtuelle Azure-Netzwerk (VNET) kann nur über ein an Azure NetApp Files delegiertes Subnetz verfügen.

**Nächste Schritte**

[Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
