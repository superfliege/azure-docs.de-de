---
title: Includedatei
description: Includedatei
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: eadff6f61bd9e5979ae835230de53562ff8542d1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003900"
---
### <a name="virtual-machines"></a>Virtual Machines

|  |  |
|---------|---------|
| [Zulassen eines benutzerdefinierten VM-Images aus einer Ressourcengruppe](../articles/governance/policy/samples/allow-custom-vm-image.md) |  Verlangt, dass benutzerdefinierte Images aus einer genehmigten Ressourcengruppe stammen. Sie geben den Namen der genehmigten Ressourcengruppe an. |
| [Zulässige SKUs für Speicherkonten und virtuelle Computer](../articles/governance/policy/samples/allowed-skus-storage.md) | Erfordert, dass Speicherkonten und virtuelle Computer genehmigte SKUs verwenden. Verwendet integrierte Richtlinien, um die Verwendung von genehmigten SKUs zu gewährleisten. Sie geben ein Array von genehmigten SKUs für virtuelle Computer und ein Array von genehmigten SKUs für Speicherkonten an. |
| [Genehmigte VM-Images](../articles/governance/policy/samples/allowed-custom-images.md) | Verlangt, dass nur genehmigte benutzerdefinierte Images in Ihrer Umgebung bereitgestellt werden. Sie geben ein Array mit genehmigten Image-IDs an. |
| [Überwachung, wenn keine Erweiterung vorhanden ist](../articles/governance/policy/samples/audit-ext-not-exist.md) | Überwacht, ob eine Erweiterung nicht mit einem virtuellen Computer bereitgestellt wird. Sie geben den Herausgeber und den Typ der Erweiterung an, um zu überprüfen, ob diese bereitgestellt wurde. |
| [Nicht zulässige VM-Erweiterungen](../articles/governance/policy/samples/not-allowed-vm-ext.md) | Untersagt die Verwendung bestimmter Erweiterungen. Sie geben ein Array an, das die unzulässigen Erweiterungstypen enthält. |