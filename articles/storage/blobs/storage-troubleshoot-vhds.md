---
title: Problembehandlung bei an virtuelle Azure-Computer angeschlossenen Datenträgern | Microsoft-Dokumentation
description: Azure Blob Storage dient zum Speichern großer Mengen unstrukturierter Objektdaten wie etwa Text- und Binärdaten. Ihre Anwendungen können über PowerShell oder über die Azure-Befehlszeilenschnittstelle, über Code mittels Azure Storage-Clientbibliotheken oder über REST auf Objekte in Blob Storage zugreifen.
services: storage
author: genlin
manager: cshepard
ms.service: storage
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 766062b085c359499046151f337921a51d948715
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34362705"
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
