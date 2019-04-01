---
title: Interoperabilität von Azure Site Recovery und Azure Backup | Microsoft-Dokumentation
description: Bietet einen Überblick darüber, wie Azure Site Recovery und Azure Backup zusammen verwendet werden können.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/26/2019
ms.author: sideeksh
ms.openlocfilehash: 6658ab8c967c70ac1deaeba3d1dfeac602515591
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2019
ms.locfileid: "57731865"
---
# <a name="about-site-recovery-and-backup-interoperability"></a>Informationen zur Interoperabilität von Azure Site Recovery und Azure Backup

Dieser Artikel enthält eine Anleitung für die erfolgreiche Verwendung von Azure IaaS-VM-Sicherung und Azure VM-Notfallwiederherstellung.

## <a name="azure-backup"></a>Azure Backup

Azure Backup hilft beim Schutz der Daten für lokale Server, virtuelle Computer, virtualisierte Workloads, SQL-Server, SharePoint-Server usw. Azure Site Recovery orchestriert und verwaltet die Wiederherstellung im Notfall für Azure-VMs, lokale virtuelle Computer und physische Server.

## <a name="azure-site-recovery"></a>Azure Site Recovery

Es ist möglich, sowohl Azure Backup als auch Azure Site Recovery auf einer VM oder einer Gruppe von VMs zu konfigurieren. Beide Produkte sind interoperabel. Einige Szenarien, in denen die Interoperabilität zwischen Azure Backup und Azure Site Recovery wichtig wird, sind die folgenden:

### <a name="file-backuprestore"></a>Datensicherung/-wiederherstellung

Wenn Sicherung und Wiederherstellung aktiviert sind, und eine Sicherung erstellt wird, gibt es kein Problem mit der Wiederherstellung von Dateien auf dem virtuellen Quellcomputer oder der Gruppe virtuellen Quellcomputer. Die Replikation wird wie gewohnt fortgesetzt, ohne dass sich der Zustand der Replikation ändert.

### <a name="disk-backuprestore"></a>Datenträgersicherung/-wiederherstellung

Wenn Sie einen Datenträger aus der Sicherung wiederherstellen, muss der Schutz des virtuellen Computers wieder aktiviert werden.

### <a name="vm-backuprestore"></a>V;-Sicherung/-Wiederherstellung

Das Sichern und Wiederherstellen einer VM oder einer Gruppe von VMs wird nicht unterstützt. Damit dies funktioniert, muss der Schutz erneut aktiviert werden.

**Szenario** | **Unterstützt von Azure Site Recovery?** | **Problemumgehung, falls vorhanden**  
--- | --- | ---
Sicherung von Dateien/Ordnern | Ja | Nicht zutreffend
Datenträgersicherung | Derzeit nicht | Deaktivieren und Aktivieren des Schutzes
VM-Sicherung | Nein  | Deaktivieren und Aktivieren des Schutzes
