---
title: Vorbereiten der Zielumgebung (VMware-/physische Server nach Azure) | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie Ihre Azure-Zielumgebung für das Replizieren von virtuellen VMware-Computern bzw. physischen Servern nach Azure vorbereiten.
services: site-recovery
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 09/28/2018
ms.author: bsiva
ms.openlocfilehash: 948812f05697362978ad041566d22977efec92a1
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434635"
---
# <a name="prepare-the-target-environment-vmwarephysical-to-azure"></a>Vorbereiten der Zielumgebung (VMware-/physische Server nach Azure)

Dieser Artikel beschreibt, wie Sie Ihre Azure-Zielumgebung vorbereiten, um mit dem Replizieren von virtuellen VMware-Computern bzw. physischen Servern nach Azure zu beginnen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird Folgendes vorausgesetzt:
- Sie haben einen Recovery Services-Tresor im [Azure-Portal](http://portal.azure.com "Azure-Portal") erstellt, um Ihre Quellcomputer zu schützen.
- Sie haben Ihre lokale Umgebung eingerichtet, um [virtuelle VMware-Quellcomputer](vmware-azure-set-up-source.md) oder [physische Server](physical-azure-set-up-source.md) nach Azure zu replizieren.

## <a name="prepare-target"></a>Vorbereiten des Ziels

Nach Abschluss von **Schritt 1: Auswählen des Schutzziels** und **Schritt 2: Vorbereiten der Quelle** gelangen Sie zu **Schritt 3: Ziel**.

![Vorbereiten des Ziels](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Abonnement:** Wählen Sie aus dem Dropdownmenü das Abonnement aus, in das Sie die virtuellen Computer bzw. physischen Server replizieren möchten.
2. **Bereitstellungsmodell:** Wählen Sie das Bereitstellungsmodell aus (klassisch oder Resource Manager).

Basierend auf dem ausgewählten Bereitstellungsmodell wird eine Überprüfung vorgenommen, um sicherzustellen, dass Sie über mindestens ein kompatibles Speicherkonto und ein virtuelles Netzwerk im Zielabonnement verfügen, um die Replikation und das Failover Ihres virtuellen Computers bzw. physischen Servers auszuführen.

Wenn die Überprüfungen erfolgreich abgeschlossen sind, klicken Sie auf „OK“, um mit dem nächsten Schritt fortzufahren.

Falls Sie über kein kompatibles Resource Manager-Speicherkonto oder kein virtuelles Netzwerk verfügen, können Sie eins erstellen, indem Sie oben auf der Seite auf die Schaltflächen **+ Speicherkonto** oder **+ Netzwerk** klicken.

## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren der Replikationseinstellungen](vmware-azure-set-up-replication.md).
