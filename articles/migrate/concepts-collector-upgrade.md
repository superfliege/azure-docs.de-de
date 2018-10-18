---
title: Upgrades für den Azure Migrate-Collector | Microsoft-Dokumentation
description: Informationen zu Upgrades für den Azure Migrate-Collector
author: musa-57
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/28/2018
ms.author: hamusa
services: azure-migrate
ms.openlocfilehash: 6080096194ec5b11f1d5593fcbb6732d1551667f
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47431391"
---
# <a name="collector-update-release-history"></a>Updatereleases für den Collector

In diesem Artikel werden Informationen zu Upgrades für den [Azure Migrate-Collector](migrate-overview.md) zusammengefasst.

Der Azure Migrate-Collector ist eine einfache Appliance, die zum Entdecken einer lokalen vCenter-Umgebung verwendet wird, damit vor der Migration zu Azure eine Überprüfung durchgeführt werden kann. [Weitere Informationen](concepts-collector.md)


## <a name="one-time-discovery-upgrade-versions"></a>Einmalige Ermittlung: Upgradeversionen

### <a name="version-10914"></a>Version 1.0.9.14

Hashwerte für das [Upgradepaket 1.0.9.14](https://aka.ms/migrate/col/upgrade_9_14)

**Algorithmus** | **Hashwert**
--- | ---
MD5 | c5bf029e9fac682c6b85078a61c5c79c
SHA1 | af66656951105e42680dfcc3ec3abd3f4da8fdec
SHA256 | 58b685b2707f273aa76f2e1d45f97b0543a8c4d017cd27f0bdb220e6984cc90e

### <a name="version-10913"></a>Version 1.0.9.13

Hashwerte für das [Upgradepaket 1.0.9.13](https://aka.ms/migrate/col/upgrade_9_13)

**Algorithmus** | **Hashwert**
--- | ---
MD5 | 739f588fe7fb95ce2a9b6b4d0bf9917e
SHA1 | 9b3365acad038eb1c62ca2b2de1467cb8eed37f6
SHA256 | 7a49fb8286595f39a29085534f29a623ec2edb12a3d76f90c9654b2f69eef87e

### <a name="version-10911"></a>Version 1.0.9.11

Hashwerte für das [Upgradepaket 1.0.9.11](https://aka.ms/migrate/col/upgrade_9_11)

**Algorithmus** | **Hashwert**
--- | ---
MD5 | 0e36129ac5383b204720df7a56b95a60
SHA1 | aa422ef6aa6b6f8bc88f27727e80272241de1bdf
SHA256 | 5f76dbbe40c5ccab3502cc1c5f074e4b4bcbf356d3721fd52fb7ff583ff2b68f

### <a name="version-1097"></a>Version 1.0.9.7

Hashwerte für das [Upgradepaket 1.0.9.7](https://aka.ms/migrate/col/upgrade_9_7)

**Algorithmus** | **Hashwert**
--- | ---
MD5 | 01ccd6bc0281f63f2a672952a2a25363
SHA1 | 3e6c57523a30d5610acdaa14b833c070bffddbff
SHA256 | e3ee031fb2d47b7881cc5b13750fc7df541028e0a1cc038c796789139aa8e1e6

## <a name="continuous-discovery-upgrade-versions"></a>Kontinuierliche Ermittlung: Upgradeversionen

Es ist bisher noch kein Upgrade der Appliance für die kontinuierliche Ermittlung verfügbar.

## <a name="run-an-upgrade"></a>Ausführen eines Upgrades

Sie können den Collector auf die neuste Version aktualisieren, ohne dafür die OVA-Datei erneut herunterladen zu müssen.

1. Laden Sie das aktuelle Upgradepaket aus der nachfolgenden Liste herunter.
2. Öffnen Sie das Administratorbefehlsfenster, und führen Sie den folgenden Befehl zum Generieren des Hash für die ZIP-Datei aus, um sicherzustellen, dass der heruntergeladene Hotfix sicher ist. Der generierte Hash sollte mit dem für die betreffende Version genannten Hash übereinstimmen:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    Beispiel: **C:\>CertUtil -HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)**
3. Kopieren Sie die ZIP-Datei in die VM des Collectors.
4. Klicken Sie erst mit der rechten Maustaste auf die ZIP-Datei und anschließend mit der linken auf **Extract All** (Alle extrahieren).
5. Klicken Sie mit der rechten Maustaste auf **Setup.ps1** > **Run with PowerShell** (Mit PowerShell ausführen), und führen Sie die Installationsanweisungen aus.


## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen](concepts-collector.md) zum Collector.
- [Führen Sie eine Überprüfung](tutorial-assessment-vmware.md) von VMware-VMs durch.
