---
title: Vorbereiten des Ziels (physisch nach Azure) | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie Ihre Azure-Umgebung vorbereiten, um mit dem Replizieren von physischen Servern unter Windows oder Linux nach Azure zu beginnen.
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 02/22/2018
ms.author: bsiva
ms.openlocfilehash: 6704ddc24db8415051a6064bbde79a6fc527871a
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
ms.locfileid: "29767545"
---
# <a name="prepare-target-physical-to-azure"></a>Vorbereiten des Ziels (physisch nach Azure)
> [!div class="op_single_selector"]
> * [VMware zu Azure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Physisch nach Azure](./site-recovery-prepare-target-physical-to-azure.md)

Dieser Artikel beschreibt, wie Sie Ihre Azure-Umgebung vorbereiten, um mit dem Replizieren von physischen Servern (x64) unter Windows oder Linux nach Azure zu beginnen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird Folgendes vorausgesetzt:
- Sie haben einen Recovery Services-Tresor zum Schutz Ihrer physischen Server erstellt. Sie können einen Recovery Services-Tresor im [Azure-Portal](http://portal.azure.com "Azure-Portal") erstellen.
- Sie haben [Ihre lokale Umgebung eingerichtet](./site-recovery-set-up-physical-to-azure.md), um physische Server nach Azure zu replizieren.

## <a name="prepare-target"></a>Vorbereiten des Ziels

Nach Abschluss von **Schritt 1: Auswählen des Schutzziels** und **Schritt 2: Vorbereiten der Quelle** gelangen Sie zu **Schritt 3: Ziel**

![Vorbereiten des Ziels](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. **Abonnement:** Wählen Sie aus dem Dropdownmenü das Abonnement aus, in das Sie die physischen Server replizieren möchten.
2. **Bereitstellungsmodell:** Wählen Sie das Bereitstellungsmodell aus (klassisch oder Resource Manager).

Basierend auf dem ausgewählten Bereitstellungsmodell wird eine Überprüfung vorgenommen, um sicherzustellen, dass Sie über mindestens ein kompatibles Speicherkonto und ein virtuelles Netzwerk im Zielabonnement verfügen, um die Replikation und das Failover Ihrer physischen Servern auszuführen.

Wenn die Überprüfungen erfolgreich abgeschlossen sind, klicken Sie auf „OK“, um mit dem nächsten Schritt fortzufahren.

Falls Sie über kein kompatibles Resource Manager-Speicherkonto oder kein virtuelles Netzwerk verfügen, können Sie eins erstellen, indem Sie oben auf der Seite auf die Schaltflächen **+ Speicherkonto** oder **+ Netzwerk** klicken.

## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren der Replikationseinstellungen](./site-recovery-setup-replication-settings-vmware.md).
