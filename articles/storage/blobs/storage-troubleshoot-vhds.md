---
title: Problembehandlung bei an virtuelle Azure-Computer angeschlossenen Datenträgern | Microsoft-Dokumentation
description: Enthält Links zu Problembehandlungsressourcen für virtuelle Festplatten (VHDs) virtueller Azure-Computer.
services: storage
author: genlin
ms.service: storage
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 81db3a819c28aab8f4b644a940eeb5e6c5ecf3d6
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307171"
---
# <a name="troubleshoot-disks-attached-to-azure-vms"></a>Problembehandlung bei an virtuelle Azure-Computer angeschlossenen Datenträgern 

Virtuelle Azure-Computer (VMs) nutzen virtuelle Festplatten (VHDs) als Betriebssystemdatenträger und angefügte Datenträger. VHDs werden als Seitenblobs in einem oder mehreren Azure Storage-Konten gespeichert. Dieser Artikel verweist auf Inhalte zur Problembehandlung bei allgemeinen Problemen, die bei VHDs auftreten können. 

## <a name="troubleshoot-storage-deletion-errors-for-a-vm"></a>Problembehandlung bei Speicherlöschfehlern für eine VM

In bestimmten Fällen kann ein Fehler beim Löschen einer Speicherressource auftreten, wenn ein virtueller Computer in einer Resource Manager-Bereitstellung angefügte VHDs enthält. Hilfe zum Beheben dieses Problems finden Sie in den folgenden Artikeln: 

  * Bei Linux-VMs: [Speicherbezogene Fehler beim Löschen in der Resource Manager-Bereitstellung](../../virtual-machines/linux/storage-resource-deletion-errors.md)  
  * Bei Windows-VMs: [Speicherbezogene Fehler beim Löschen in der Resource Manager-Bereitstellung](../../virtual-machines/windows/storage-resource-deletion-errors.md)  

## <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Problembehandlung bei unerwarteten Neustarts von virtuellen Computern mit angefügten VHDs

Wenn unerwartete Neustarts eines virtuellen Computers mit einer großen Anzahl angefügter VHDs auftreten, lesen Sie folgende Artikel:

  * Bei Linux-VMs: [Unerwartete Neustarts von virtuellen Computern mit angefügten VHDs](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
  * Bei Windows-VMs: [Unerwartete Neustarts von virtuellen Computern mit angefügten VHDs](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
