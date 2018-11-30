---
title: Einrichten der Zielumgebung für die Notfallwiederherstellung von lokalen physischen Servern in Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie die Azure-Zielumgebung für die Notfallwiederherstellung von physischen Servern mithilfe von Azure Site Recovery einrichten.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: ramamill
ms.openlocfilehash: a45e8c7bdb616eb389d95be8421bea7d31eafe29
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2018
ms.locfileid: "51974168"
---
# <a name="prepare-target-vmware-to-azure"></a>Vorbereiten des Ziels (VMware nach Azure)

Dieser Artikel beschreibt, wie Sie Ihre Azure-Umgebung vorbereiten, um mit dem Replizieren von physischen Servern (x64) unter Windows oder Linux nach Azure zu beginnen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird Folgendes vorausgesetzt:
- Sie haben einen Recovery Services-Tresor zum Schutz Ihrer physischen Server erstellt. Sie können einen Recovery Services-Tresor im [Azure-Portal](http://portal.azure.com "Azure-Portal") erstellen.
- Sie haben [Ihre lokale Umgebung eingerichtet](physical-azure-disaster-recovery.md), um physische Server nach Azure zu replizieren.

## <a name="prepare-target"></a>Vorbereiten des Ziels

Nach Abschluss von **Schritt 1: Auswählen des Schutzziels** und **Schritt 2: Vorbereiten der Quelle** gelangen Sie zu **Schritt 3: Ziel**

![Vorbereiten des Ziels](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **Abonnement:** Wählen Sie aus dem Dropdownmenü das Abonnement aus, in das Sie die physischen Server replizieren möchten.
2. **Bereitstellungsmodell:** Wählen Sie das Bereitstellungsmodell aus (klassisch oder Resource Manager).

Basierend auf dem ausgewählten Bereitstellungsmodell wird eine Überprüfung vorgenommen, um sicherzustellen, dass Sie über mindestens ein kompatibles Speicherkonto und ein virtuelles Netzwerk im Zielabonnement verfügen, um die Replikation und das Failover Ihrer physischen Servern auszuführen.

Wenn die Überprüfungen erfolgreich abgeschlossen sind, klicken Sie auf „OK“, um mit dem nächsten Schritt fortzufahren.

Falls Sie über kein kompatibles Resource Manager-Speicherkonto oder kein virtuelles Netzwerk verfügen, können Sie eins erstellen, indem Sie oben auf der Seite auf die Schaltflächen **+ Speicherkonto** oder **+ Netzwerk** klicken.

## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren der Replikationseinstellungen](vmware-azure-set-up-replication.md).
