---
title: Vorbereiten von Ziel (Physical-to Azure) | Microsoft Docs
description: Dieser Artikel beschreibt, wie der Azure-Umgebung starten, Replizieren von physischen Servern, die mit Windows oder Linux in Azure vorbereiten.
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 5/31/2017
ms.author: bsiva
ms.openlocfilehash: aa7a32ace8354f615a8b8cc137f6bdf48fbadf48
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-target-vmware-to-azure"></a>Vorbereiten des Ziels (VMware in Azure)
> [!div class="op_single_selector"]
> * [VMware in Azure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Physische in Azure](./site-recovery-prepare-target-physical-to-azure.md)

Dieser Artikel beschreibt, wie der Azure-Umgebung starten, Replizieren von physischen Servern (x 64), die mit Windows oder Linux in Azure vorbereiten.

## <a name="prerequisites"></a>Voraussetzungen

Der Artikel wird Folgendes vorausgesetzt:
- Sie haben eine Recovery Services-Tresor zum Schutz Ihrer physischen Server erstellt. Sie können aus einer Recovery Services-Tresor erstellen die [Azure-Portal](http://portal.azure.com "Azure-Portal").
- Sie haben [Einrichten Ihrer lokalen Umgebung](./site-recovery-set-up-physical-to-azure.md) auf physische Servern in Azure zu replizieren.

## <a name="prepare-target"></a>Vorbereiten des Ziels

Nach Abschluss der **Schritt 1:Select Schutzziel** und **Schritt2: Vorbereiten der Quelle**, gelangen Sie auf **Schritt 3: Ziel**

![Vorbereiten des Ziels](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. **Abonnement:** aus dem Dropdownmenü aus, wählen Sie das Abonnement, das die physischen Servern repliziert werden sollen.
2. **Bereitstellungsmodell:** auswählen des Bereitstellungsmodells (Classic oder Ressourcen-Manager)

Basierend auf dem ausgewählten Bereitstellungsmodell, wird eine Überprüfung ausgeführt, um sicherzustellen, dass Sie über mindestens ein kompatibles Speicherkonto und virtuelles Netzwerk in der Ziel-Abonnements zu replizieren und das Failover Ihren physischen Servern verfügen.

Wenn die Überprüfungen erfolgreich abgeschlossen ist, klicken Sie auf OK, um mit dem nächsten Schritt fortfahren.

Falls Sie nicht, eine kompatible Ressourcen-Manager-Storage-Konto oder ein virtuelles Netzwerk besitzen, oder weitere Domänen hinzufügen möchten, Sie können dazu durch Klicken auf die **+ Speicherkonto** oder **+ Netzwerk** Schaltflächen oben auf dem Blatt ".

## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren Sie replikationseinstellungen](./site-recovery-setup-replication-settings-vmware.md).
