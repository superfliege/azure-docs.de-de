---
title: Einrichten eines Prozessservers in Azure für das Failback während der Notfallwiederherstellung von VMware-VMs und physischen Servern mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie einen Prozessserver in Azure einrichten, um während der Notfallwiederherstellung von VMware-VMs und physischen Servern das Failback von Azure in die lokale Umgebung durchzuführen.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: raynew
ms.openlocfilehash: 330f0197b8a7735043e93f00dc4baa5578f50228
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212232"
---
# <a name="scale-for-failback-with-additional-process-servers"></a>Skalieren für das Failback mit zusätzlichen Prozessservern

Wenn Sie VMware-VMs oder physische Server mithilfe von [Site Recovery](site-recovery-overview.md) in Azure replizieren, wird standardmäßig ein Prozessserver auf dem Konfigurationsservercomputer installiert und zum Koordinieren der Datenübertragung zwischen Site Recovery und Ihrer lokalen Infrastruktur verwendet. Um die Kapazität zu erhöhen und die Replikationsbereitstellung horizontal hochzuskalieren, können Sie zusätzliche eigenständige Prozessserver hinzufügen. Dieser Artikel beschreibt die entsprechende Vorgehensweise.

## <a name="before-you-start"></a>Vorbereitung

### <a name="capacity-planning"></a>Kapazitätsplanung

Stellen Sie sicher, dass Sie eine [Kapazitätsplanung](site-recovery-plan-capacity-vmware.md) für die VMware-Replikation durchgeführt haben. Dadurch können Sie feststellen, wie und wann Sie zusätzliche Prozessserver bereitstellen sollten.

### <a name="sizing-requirements"></a>Größenanforderungen 

Prüfen Sie die in der Tabelle zusammengefassten Größenanforderungen. Allgemein gilt: Wenn Sie Ihre Bereitstellung auf mehr als 200 Quellcomputer hochskalieren müssen oder Ihre gesamte tägliche Änderungsrate 2 TB übersteigt, benötigen Sie zusätzliche Prozessserver zur Bewältigung des Datenverkehrsaufkommens.

| **Zusätzlicher Prozessserver** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer** |
| --- | --- | --- | --- |
|4 vCPUs (2 Sockets * 2 Kerne \@ 2,5 GHz), 8 GB Arbeitsspeicher |300 GB |250 GB oder weniger |Bis zu 85 Computer replizieren. |
|8 vCPUs (2 Sockets * 4 Kerne \@ 2,5 GHz), 12 GB Arbeitsspeicher |600 GB |250 GB bis 1 TB |Zwischen 85 und 150 Computer replizieren. |
|12 vCPUs (2 Sockets * 6 Kerne \@ 2,5 GHz), 24 GB Arbeitsspeicher |1 TB |1 TB bis 2 TB |Zwischen 150 und 225 Computer replizieren. |

Jeder geschützte Quellcomputer ist mit drei Datenträgern von jeweils 100 GB konfiguriert.

### <a name="prerequisites"></a>Voraussetzungen

In der folgenden Tabelle sind die Voraussetzungen für den zusätzlichen Prozessserver zusammengefasst.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]


## <a name="download-installation-file"></a>Herunterladen der Installationsdatei

Laden Sie die Installationsdatei für den Prozessserver wie folgt herunter:

1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu Ihrem Recovery Services-Tresor.
2. Öffnen Sie **Site Recovery-Infrastruktur** > **VMware und physische Computer** > **Konfigurationsserver** (unter „Für VMware und physische Computer“).
3. Wählen Sie den Konfigurationsserver aus, um die Detailsseite des Servers aufzurufen. Klicken Sie dann auf **+ Prozessserver**.
4. Wählen Sie unter **Prozessserver hinzufügen** >  **Geben Sie an, wo Sie Ihren Prozessserver bereitstellen möchten** die Option **Prozessserver für horizontales Hochskalieren lokal bereitstellen** aus.

  ![Seite „Server hinzufügen“](./media/vmware-azure-set-up-process-server-scale/add-process-server.png)
1. Klicken Sie auf **Einheitliches Setup von Microsoft Azure Site Recovery herunterladen**. Dadurch wird die neueste Version der Installationsdatei heruntergeladen.

  > [!WARNING]
  Bei der Installationsversion des Prozessservers sollte es sich um die gleiche oder eine frühere Version wie die von Ihnen ausgeführte Version des Konfigurationsservers handeln. Eine einfache Möglichkeit, die Versionskompatibilität sicherzustellen, ist die Verwendung des gleichen Installers, den Sie zuletzt zum Installieren oder Aktualisieren Ihres Konfigurationsservers verwendet haben.

## <a name="install-from-the-ui"></a>Installieren über die Benutzeroberfläche

Führen Sie die Installation auf folgende Weise aus. Nachdem Sie den Prozessserver eingerichtet haben, migrieren Sie Quellcomputer, damit diese ihn verwenden können.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="install-from-the-command-line"></a>Installieren über die Befehlszeile

Die Installation erfolgt, indem Sie den folgenden Befehl ausführen:

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

Die Befehlszeilenparameter lauten wie folgt:

[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]

Beispiel: 

```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```
### <a name="create-a-proxy-settings-file"></a>Erstellen einer Datei mit Proxyeinstellungen

Wenn Sie einen Proxy einrichten müssen, nimmt der ProxySettingsFilePath-Parameter eine Datei als Eingabe an. Sie können die Datei auf folgende Weise erstellen und als Eingabe an den ProxySettingsFilePath-Parameter übergeben.

```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```

## <a name="next-steps"></a>Nächste Schritte
Informationen zum [Verwalten von Prozessservereinstellungen](vmware-azure-manage-process-server.md)
