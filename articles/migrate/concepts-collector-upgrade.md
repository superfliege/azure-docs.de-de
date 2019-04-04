---
title: Upgrades für den Azure Migrate-Collector | Microsoft-Dokumentation
description: Informationen zu Upgrades für den Azure Migrate-Collector
author: musa-57
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: hamusa
services: azure-migrate
ms.openlocfilehash: d7a9a6cb8683116b9e0c432c2aede2b8df3ddc68
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57895617"
---
# <a name="collector-update-release-history"></a>Updatereleases für den Collector

In diesem Artikel werden Informationen zu Upgrades für den [Azure Migrate-Collector](migrate-overview.md) zusammengefasst.

Der Azure Migrate-Collector ist eine einfache Appliance, die zum Entdecken einer lokalen vCenter-Umgebung verwendet wird, damit vor der Migration zu Azure eine Überprüfung durchgeführt werden kann. [Weitere Informationen](concepts-collector.md)

## <a name="continuous-discovery-upgrade-versions"></a>Kontinuierliche Ermittlung: Upgradeversionen

### <a name="version-101012-released-on-03132019"></a>Version 1.0.10.12 (Veröffentlicht: 13.03.2019)

Enthält Korrekturen für Probleme bei der Auswahl der Azure-Cloud in der Appliance.

Hashwerte für das [Upgradepaket 1.0.10.12](https://aka.ms/migrate/col/upgrade_10_12)

**Algorithmus** | **Hashwert**
--- | ---
MD5 | 27704154082344c058238000dff9ae44
SHA1 | 41e9e2fb71a8dac14d64f91f0fd780e0d606785e
SHA256 | c6e7504fcda46908b636bfe25b8c73f067e3465b748f77e50027e66f2727c2a9

## <a name="one-time-discovery-deprecated-now-previous-upgrade-versions"></a>Einmalige Ermittlung (jetzt veraltet): Vorherige Upgradeversionen

> [!NOTE]
> Die Appliance zur einmaligen Ermittlung ist inzwischen veraltet, da diese Methode bei der Verfügbarkeit der Leistungsdatenpunkte von den Statistikeinstellungen von vCenter Server abhängig war und durchschnittliche Leistungsindikatoren erfasst hat, was zu unterdimensionierten virtuellen Computern für die Migration zu Azure führte.

### <a name="version-10916-released-on-10292018"></a>Version 1.0.9.16 (Release: 29.10.2018)

Enthält Korrekturen für PowerCLI-Probleme, die beim Einrichten der Appliance auftreten.

Hashwerte für das [Upgradepaket 1.0.9.16](https://aka.ms/migrate/col/upgrade_9_16)

**Algorithmus** | **Hashwert**
--- | ---
MD5 | d2c53f683b0ec7aaf5ba3d532a7382e1
SHA1 | e5f922a725d81026fa113b0c27da185911942a01
SHA256 | a159063ff508e86b4b3b7b9a42d724262ec0f2315bdba8418bce95d973f80cfc

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
