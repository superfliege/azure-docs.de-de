---
title: "Problembehandlung für Zuordnungsfehler bei virtuellen Linux-Computern | Microsoft Docs"
description: "Problembehandlung für Zuordnungsfehler beim Erstellen, Neustarten oder Ändern der Größe eines virtuellen Linux-Computers in Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resourece-manager,azure-service-management
ms.assetid: 1ef41144-6dd6-4a56-b180-9d8b3d05eae7
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2016
ms.author: cjiang
ms.openlocfilehash: 8cf421afe9b41af050152dc965d42e7809b5f2d9
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-linux-vms-in-azure"></a>Problembehandlung für Zuordnungsfehler beim Erstellen, Neustarten oder Ändern der Größe von virtuellen Linux-Computern in Azure
Wenn Sie einen virtuellen Computer erstellen, beendete virtuelle Computer (zuordnungsaufgehobene) neu starten oder die Größe eines virtuellen Computers ändern, weist Microsoft Azure Ihrem Abonnement Compute-Ressourcen zu. Unter Umständen erhalten Sie beim Ausführen dieser Schritte auch dann gelegentlich Fehler, wenn Sie die Grenzwerte des Azure-Abonnements noch nicht erreicht haben. In diesem Artikel werden die Ursachen einiger häufig auftretender Zuordnungsfehler erläutert und mögliche Abhilfemaßnahmen vorgeschlagen. Diese Informationen können auch hilfreich sein, wenn Sie die Bereitstellung Ihrer Dienste planen. Sie können auch die [Problembehandlung für Zuordnungsfehler beim Erstellen, Neustarten oder Ändern der Größe von virtuellen Windows-Computern in Azure](../windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)durchführen.

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

