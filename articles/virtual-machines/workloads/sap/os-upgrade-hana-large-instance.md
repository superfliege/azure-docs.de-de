---
title: Durchführen eines Betriebssystemupgrades für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Durchführen von Betriebssystemupgrades für SAP HANA in Azure (große Instanzen)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f6af1a6612360c2433c05a7add79d2e7b3b9d754
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34658259"
---
# <a name="operating-system-upgrade"></a>Betriebssystemupgrade
In diesem Dokument werden die Details zu Betriebssystemupgrades für SAP HANA (große Instanzen) beschrieben.

>[!NOTE]
>Das Betriebssystemupgrade ist Zuständigkeit des Kunden. Der Microsoft Operations-Support kann Sie dabei auf wichtige Punkte hinweisen, die beim Upgrade zu beachten sind. Bevor Sie Planungen für ein Upgrade treffen, sollten Sie auch den Hersteller Ihres Betriebssystems zurate ziehen.

Das Microsoft Operations-Team installiert das Betriebssystem bei der Bereitstellung der HLI-Einheiten. Im Laufe der Zeit müssen Sie Wartungsvorgänge für das Betriebssystem für die HLI-Einheit vornehmen (z.B. Patches erstellen, Optimierungen sowie Upgrades durchführen).

Bevor Sie wesentliche Änderungen am Betriebssystem (z.B. ein Betriebssystemupgrade) vornehmen, **müssen** Sie die folgende Kompatibilitätsmatrix berücksichtigen. Zudem **müssen** Sie sich durch Öffnen eines Supporttickets an das Microsoft Operations-Team wenden, bevor Sie mit wesentlichen Betriebssystemaktivitäten wie Upgrades beginnen.

Die Supportmatrix der anderen SAP HANA-Versionen mit den verschiedenen Linux-Versionen finden Sie im [SAP-Hinweis 2235581](https://launchpad.support.sap.com/#/notes/2235581).

Für die HLIs wurde die Kompatibilität mit folgenden Versionen getestet. Wenn Ihr HLI-Server nicht der Kompatibilitätsmatrix entspricht, wenden Sie sich an den Microsoft Operations-Support.

## <a name="for-type-i-class-sku-category"></a>Bei SKU-Kategorie von Typ I

| Konfiguration | SUSE 12 SP1 | SUSE 12 SP2 | RHEL 7.2 | RHEL 7.3|
| --- | --- | --- | --- | --- |
| Serverfirmware | 3.1(2b) | 3.1(2b) | 3.1(2b) | 3.1(2b) |
| ENIC-Version | 2.3.0.44 | 2.3.0.44 | 2.3.0.30 | 2.3.0.44 |
| FNIC-Version | 1.6.0.34 | 1.6.0.34 | 1.6.0.27 | 1.6.0.36 |
| EDAC | Deaktiviert | Deaktiviert | Deaktiviert | Deaktiviert |
| Kernelversion | 4.4.21-69-default | 3.12.49-11-default | 3.10.0-327.el7.x86_64 | 3.10.0-693.17.1 |


## <a name="for-type-ii-class-sku-category"></a>Bei SKU-Kategorie von Typ II

| Konfiguration | SUSE 12 SP1 | SUSE 12 SP2 | RHEL 7.2 | RHEL 7.3|
| --- | --- | --- | --- | --- |
| RMC-Firmwareversion | 1.1.121  | 1.1.121  | 1.1.121  | 1.1.121 |
| BMC-Firmwareversion | 1.0.43   | 1.0.43   | 1.0.43   | 1.0.43  |
| SFS-Version (Software Foundation Server) | 2.16    | 2.16    | 2.14/2.16   | 2.16   |
| BIOS | 5.2.6    | 5.2.6    | 5.2.6    | 5.2.6   |
| i40e-Version    | 2.0.19     | 2.0.19     | 1.5.10-k    | 1.5.10-k   |
| Kernelversion    | 3.12.49-11.1     | 4.4.21-69.1     | 3.10.0-327    | 3.10.0-693.17.1   |


## <a name="known-issues"></a>Bekannte Probleme

Im Folgenden werden einige bekannte Probleme bei Upgrades erläutert:
- Bei SKUs von SKU-Typ II wird Software Foundation Software (SFS) nach der Durchführung eines Betriebssystemupgrades entfernt. Nach dem Betriebssystemupgrade müssen Sie die kompatible SFS-Version neu installieren.
- Für Ethernet-Kartentreiber (ENIC und FNIC) wird ein Rollback auf die ältere Version ausgeführt. Nach dem Upgrade müssen Sie die kompatible Version der Treiber neu installieren.

## <a name="next-steps"></a>Nächste Schritte
- Informationen zur SKU von Typ I für Betriebssystemsicherungen finden Sie unter [Sicherung und Wiederherstellung](hana-overview-high-availability-disaster-recovery.md).
- Informationen zur SKU von Typ II finden Sie unter [Betriebssystemsicherung für SKUs vom Typ II](os-backup-type-ii-skus.md).