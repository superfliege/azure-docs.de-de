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
ms.topic: conceptual
ms.date: 02/14/2019
ms.author: b-juche
ms.openlocfilehash: c2086eb0c5529d8e570a545e35fc716f70c7541f
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2019
ms.locfileid: "56668343"
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
