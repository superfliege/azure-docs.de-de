---
title: Anpassen der Einstellungen einer Azure Migrate-Bewertung | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie Sie eine Bewertung für die Migration von virtuellen VMware-Computern mithilfe von Azure Migration Planner einrichten und ausführen."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a068b9c7-5f87-4fe1-90b9-3be48d91aa3f
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 20aeb2073ad307952f92c8377bc9d78169f1756c
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="customize-an-assessment"></a>Anpassen einer Bewertung

[Azure Migrate](migrate-overview.md) erstellt Bewertungen mit Standardeinstellungen. Nach dem Erstellen einer Bewertung können Sie diese Standardeinstellungen entsprechend den Anweisungen in diesem Artikel ändern.


## <a name="edit-assessment-values"></a>Bearbeiten von Bewertungswerten

1. Wählen Sie im Azure Migrate-Projekt auf der Seite **Bewertungen** die Bewertung aus, und klicken Sie auf **Eigenschaften bearbeiten**.
2. Ändern Sie die Einstellungen entsprechend den Angaben in der folgenden Tabelle.

    **Einstellung** | **Details** | **Standard**
    --- | --- | ---
    **Zielstandort** | Der Azure-Standort, zu dem die Migration durchgeführt werden soll. |  „USA, Westen 2“ ist der Standardstandort.
    **Speicherredundanz** | Der Speichertyp, der von den Azure-VMs nach der Migration verwendet wird. | Derzeit wird nur die Replikation vom Typ [Lokal redundanter Speicher (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) unterstützt.
    **Komfortfaktor** | Der Komfortfaktor ist ein Puffer, der während der Bewertung verwendet wird. Verwenden Sie ihn, um Elemente wie z.B. saisonbedingte Nutzung, kurzer Leistungsverlauf, wahrscheinliche Zunahme der zukünftigen Verwendung zu berücksichtigen. | Die Standardeinstellung ist 1.3x.
    **Perfomance history** (Leistungsverlauf) | Zeitraum für die Auswertung des Leistungsverlaufs. | Standardmäßig ist ein Monat festgelegt.
    **Perzentilwert der Nutzung** | Der für den Leistungsverlauf zu berücksichtigende Perzentilwert. | Der Standardwert ist 95 %.
    **Preisstufe** | Sie können den [Tarif](https://azure.microsoft.com/blog/basic-tier-virtual-machines-2/) für einen virtuellen Computer angeben.  | Standardmäßig wird der Tarif [Standard](../virtual-machines/windows/sizes-general.md) verwendet.
    **Angebot** | Geltende [Azure-Angebote](https://azure.microsoft.com/support/legal/offer-details/). | [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) ist die Standardeinstellung.
    **Währung** | Rechnungswährung. | Der Standardwert ist US-Dollar.
    **Rabatt (%)** | Alle abonnementspezifischen Rabatte, die Sie zusätzlich zu allen Angeboten erhalten. | Die Standardeinstellung ist 0 %.
    **Azure-Hybridnutzungsvorteil** | Gibt an, ob Sie für den [Azure-Hybridnutzungsvorteil](https://azure.microsoft.com/pricing/hybrid-use-benefit/) registriert sind. Wenn diese Einstellung auf „Ja“ festgelegt ist, werden für virtuelle Windows-Computer Azure-Preise veranschlagt. | Die Standardeinstellung ist „Ja“.

3. Klicken Sie auf **Speichern**, um die Bewertung zu aktualisieren.


## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
