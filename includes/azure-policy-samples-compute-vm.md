---
title: Includedatei
description: Includedatei
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: eedce1ef3a4e7d56cfaf3142a72e370c96f9f575
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664525"
---
### <a name="virtual-machines"></a>Virtual Machines

|  |  |
|---------|---------|
| [Zulassen eines benutzerdefinierten VM-Images aus einer Ressourcengruppe](../articles/azure-policy/scripts/allow-custom-vm-image.md) |  Verlangt, dass benutzerdefinierte Images aus einer genehmigten Ressourcengruppe stammen. Sie geben den Namen der genehmigten Ressourcengruppe an. |
| [Zulässige SKUs für Speicherkonten und virtuelle Computer](../articles/azure-policy/scripts/allowed-skus-storage.md) | Erfordert, dass Speicherkonten und virtuelle Computer genehmigte SKUs verwenden. Verwendet integrierte Richtlinien, um die Verwendung von genehmigten SKUs zu gewährleisten. Sie geben ein Array von genehmigten SKUs für virtuelle Computer und ein Array von genehmigten SKUs für Speicherkonten an. |
| [Genehmigte VM-Images](../articles/azure-policy/scripts/allowed-custom-images.md) | Verlangt, dass nur genehmigte benutzerdefinierte Images in Ihrer Umgebung bereitgestellt werden. Sie geben ein Array mit genehmigten Image-IDs an. |
| [Überwachung, wenn keine Erweiterung vorhanden ist](../articles/azure-policy/scripts/audit-ext-not-exist.md) | Überwacht, ob eine Erweiterung nicht mit einem virtuellen Computer bereitgestellt wird. Sie geben den Herausgeber und den Typ der Erweiterung an, um zu überprüfen, ob diese bereitgestellt wurde. |
| [Nicht zulässige VM-Erweiterungen](../articles/azure-policy/scripts/not-allowed-vm-ext.md) | Untersagt die Verwendung bestimmter Erweiterungen. Sie geben ein Array an, das die unzulässigen Erweiterungstypen enthält. |