---
title: Dienstebenen für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt die Durchsatzleistung für die Dienstebenen von Azure NetApp Files.
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
ms.openlocfilehash: a10ac319fe362011531e00f832bb8e471fd56fdf
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56431074"
---
# <a name="service-levels-for-azure-netapp-files"></a>Dienstebenen für Azure NetApp Files
Azure NetApp Files unterstützt zwei Dienstebenen: „Premium“ und „Standard“. 

## <a name="Premium"></a>Storage Premium

Der *Premium*-Speicher bietet bis zu 64 MiB/Sek. pro TiB des Durchsatzes. Die Durchsatzleistung wird für ein Volumekontingent indiziert. Beispiel: Ein Volume im Premium-Speicher mit 2 TiB des bereitgestellten Kontingents (unabhängig vom tatsächlichen Verbrauch) einen Durchsatz von 128 MiB/Sek.

## <a name="Standard"></a>Storage Standard

Der *Standard*-Speicher bietet bis zu 16 MiB/Sek. pro TiB des Durchsatzes. Die Durchsatzleistung wird für ein Volumekontingent indiziert. Beispiel: Ein Volume im Standard-Speicher mit 2 TiB des bereitgestellten Kontingents (unabhängig vom tatsächlichen Verbrauch) einen Durchsatz von 32 MiB/Sek.

## <a name="next-steps"></a>Nächste Schritte

- Die Preise der verschiedenen Dienstebenen finden Sie unter [Preise für Azure NetApp Files (Vorschauversion)](https://azure.microsoft.com/pricing/details/storage/netapp/).
- [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)
