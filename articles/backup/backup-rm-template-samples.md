---
title: Azure Resource Manager-Vorlagen für Azure Backup
description: Beispiele für Azure Backup PowerShell
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 04/18/2018
ms.author: markgal
ms.custom: mvc
ms.openlocfilehash: 941357d0a8b7fb46a5c2dcebb70681452118f23f
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406380"
---
# <a name="azure-resource-manager-templates-for-azure-backup"></a>Azure Resource Manager-Vorlagen für Azure Backup

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für die Verwendung mit Recovery Services-Tresoren und Azure Backup-Features.

|   |   |
|---|---|
|**Recovery Services-Tresor** | |
| [Erstellen eines Recovery Services-Tresors](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vault-create)| Erstellen Sie einen Recovery Services-Tresor. Der Tresor kann für Azure Backup und für Azure Site Recovery verwendet werden. |
|**Sichern virtueller Computer**| |
| [Sichern von Resource Manager-VMs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-backup-vms) | Verwenden Sie den vorhandenen Recovery Services-Tresor und die vorhandene Sicherungsrichtlinie, um Resource Manager-VMs in der gleichen Ressourcengruppe zu sichern.|
| [Sichern von IaaS-VMs im Recovery Services-Tresor](https://github.com/Azure/azure-quickstart-templates/tree/master/201-recovery-services-backup-classic-resource-manager-vms) | Vorlage zum Sichern klassischer und Resource Manager-basierter virtueller Computer. |
| [Erstellen einer wöchentlichen Sicherungsrichtlinie für IaaS-VMs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-weekly-backup-policy-create) | Vorlage zum Erstellen eines Recovery Services-Tresors und einer wöchentlichen Sicherungsrichtlinie zum Sichern klassischer und Resource Manager-basierter virtueller Computer.|
| [Erstellen einer täglichen Sicherungsrichtlinie für IaaS-VMs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-daily-backup-policy-create) | Vorlage zum Erstellen eines Recovery Services-Tresors und einer täglichen Sicherungsrichtlinie zum Sichern klassischer und Resource Manager-basierter virtueller Computer.|
| [Bereitstellen eines virtuellen Windows Server-Computers mit aktivierter Sicherung](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-create-vm-and-configure-backup) | Vorlage zum Erstellen eines virtuellen Windows Server-Computers und eines Recovery Services-Tresors mit aktivierter Standardsicherungsrichtlinie.|
|**Überwachen von Sicherungsaufträgen** |  |
| [OMS monitoring solution for Azure Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-backup-oms-monitoring) (OMS-Überwachungslösung für Azure Backup) | Vorlage zum Bereitstellen der Log Analytics-Überwachung für Azure Backup zur Überwachung von Sicherungs- und Wiederherstellungsaufträgen, Sicherungswarnungen und des Cloudspeichers, der in Ihren Recovery Services-Tresoren verwendet wird.|  
|   |   |

