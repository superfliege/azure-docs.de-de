---
title: Behebung unerwarteter Neustarts von VMs mit angefügten VHDs auf virtuellen Linux-Computern in Azure | Microsoft-Dokumentation
description: So beheben Sie unerwartete Neustarts virtueller Linux-Computer.
keywords: SSH-Verbindung abgelehnt, SSH-Fehler, Azure SSH, SSH-Verbindungsfehler
services: virtual-machines-linux
author: genlin
manager: cshepard
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 29aecf748ddda280e6f42be0bb3c986d90a3d7f9
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34364419"
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Behebung unerwarteter Neustarts von virtuellen Computern mit angefügten VHDs

Wenn ein virtueller Azure-Computer über eine große Anzahl von angefügten VHDs verfügt, die sich im gleichen Speicherkonto befinden, können die Skalierbarkeitsziele für ein einzelnes Speicherkonto überschritten werden. Dies führt zu einem unerwarteten Neustart der VM (virtual machine, virtueller Computer). Überprüfen Sie die Minutenmetriken für das Speicherkonto (**TotalRequests**/**TotalIngress**/**TotalEgress**) auf Spitzen, die die Skalierbarkeitsziele für ein Speicherkonto überschreiten. Unter [Metriken zeigen einen Anstieg bei PercentThrottlingError an](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) finden Sie Unterstützung bei der Ermittlung, ob in Ihrem Speicherkonto eine Drosselung stattgefunden hat.

In der Regel wird jeder einzelne Eingabe- oder - oder Ausgabevorgang auf einer VHD von einem virtuellen Computer in **Get Page**- oder **Put Page**-Vorgänge des zugrunde liegenden Seitenblobs übersetzt. Daher können Sie die geschätzten IOPS für Ihre Umgebung verwenden, um zu bestimmen, über wie viele virtuelle Festplatten Sie in einem einzigen Speicherkonto auf Grundlage des spezifischen Anwendungsverhaltens verfügen können. Microsoft empfiehlt höchstens 40 Datenträger in einem Speicherkonto. Unter [Skalierbarkeits- und Leistungsziele für Azure Storage](../../storage/common/storage-scalability-targets.md) finden Sie nähere Informationen über Skalierbarkeitsziele für Speicherkonten, insbesondere die Gesamtanforderungsrate und die Gesamtbandbreite für den Speicherkontotyp, den Sie verwenden.

Wenn Sie die Skalierbarkeitsziele für Ihr Speicherkonto überschreiten, platzieren Sie die virtuellen Festplatten in mehreren Speicherkonten, um die Aktivität in den einzelnen Konten zu reduzieren.
