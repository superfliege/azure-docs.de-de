---
title: Problembehandlung bei an virtuelle Azure-Computer angeschlossenen Datenträgern | Microsoft-Dokumentation
description: Azure Blob Storage dient zum Speichern großer Mengen unstrukturierter Objektdaten wie etwa Text- und Binärdaten. Ihre Anwendungen können über PowerShell oder über die Azure-Befehlszeilenschnittstelle, über Code mittels Azure Storage-Clientbibliotheken oder über REST auf Objekte in Blob Storage zugreifen.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/01/2018
ms.author: tamram
ms.openlocfilehash: ae309efb95e2f633effcfb5723d8377f5e94d406
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32779676"
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
