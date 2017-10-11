---
title: "Einrichten von Quelle und Ziel für die physischen Server-Replikation in Azure mit Azure Site Recovery | Microsoft Docs"
description: "Fasst die Schritte zum Einrichten von Quell- und Einstellungen für die Replikation von physischen Servern in den Azure-Speicher mit dem Azure Site Recovery-Dienst"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 2e3d03bc-4e53-4590-8a01-f702d3cc9500
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e89bbf5a2c1d71852e49da43d3106a05ebfc28a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-the-source-and-target-for-physical-server-replication-to-azure"></a>Schritt 7: Richten Sie die Quell-als auch für physische Serverreplikation in Azure

In diesem Artikel wird beschrieben, wie so konfigurieren Sie Quell-und zieleinstellungen beim Replizieren von lokalen physischen Server auf Azure, mit der [Azure Site Recovery](site-recovery-overview.md) Dienst im Azure-Portal.

Senden Sie Fragen und Kommentare am Ende dieses Artikels oder auf die [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-the-source-environment"></a>Die Source-Umgebung einrichten

Richten Sie den Konfigurationsserver im Tresor zu registrieren und Ermitteln von Computern.

1. Klicken Sie auf **Standortwiederherstellung** > **Schritt 1: Vorbereiten der Infrastruktur** > **Quelle**.
2. Wenn Sie mit einem Konfigurationsserver haben, klicken Sie auf **+ Konfigurationsserver**.
3. In **Server hinzufügen**, überprüfen Sie, ob **Konfigurationsserver** wird im **Servertyp**.
4. Laden Sie Setup Unified für Site Recovery-Installationsdatei herunter.
5. Tresorregistrierungsschlüssel herunterladen Sie benötigen diese beim Ausführen des Setups Unified. Der Schlüssel ist fünf Tage nach der Generierung gültig.

   ![Einrichten von Datenquellen](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a>Den Konfigurationsserver im Tresor zu registrieren.

Gehen Sie folgendermaßen vor, bevor Sie beginnen, und führen Sie Setup zum Installieren der Konfigurationsserver, Prozessserver und masterzielserver Unified. Das Video beschreibt das Einrichten der Komponenten für VMware-VM zur Replikation in Azure. Allerdings ist der gleiche Prozess für physischen Servern Replikation in Azure gültig.

- Auf dem Konfigurationsserver VM, stellen Sie sicher, dass die Systemuhr mit synchronisiert ist ein [Zeitserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Sie sollte übereinstimmen. Wenn es 15 Minuten im Vordergrund oder nach hinten, Setup schlägt möglicherweise fehl.
- Führen Sie Setup als lokaler Administrator auf dem Servercomputer Konfiguration.
- Stellen Sie sicher, dass TLS 1.0 auf dem virtuellen Computer aktiviert ist.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Der Konfigurationsserver kann auch installiert [über die Befehlszeile](http://aka.ms/installconfigsrv).




## <a name="set-up-the-target-environment"></a>Einrichten der zielumgebung

Bevor Sie die zielumgebung einrichten, stellen Sie sicher, dass Sie ein Azure Storage-Konto und ein virtuelles Netzwerk eingerichtet haben.

1. Klicken Sie auf **Vorbereiten der Infrastruktur** > **Ziel**, und wählen Sie das Azure-Abonnement verwenden möchten.
2. Geben Sie, ob Ziel Bereitstellungsmodell Ressourcen-Manager-basierte oder klassische ist.
3. Site Recovery überprüft, dass Sie eine oder mehrere Azure-Speicherkonten kompatibel und Netzwerke verfügen.

   ![Ziel](./media/physical-walkthrough-source-target/gs-target.png)

4. Wenn Sie ein Speicherkonto oder Netzwerk erstellt haben, klicken Sie auf **+ Speicherkonto** oder **+ Netzwerk**, um ein Ressourcen-Manager-Konto erstellen oder Inline-Netzwerk.

## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Schritt 8: Richten Sie eine Replikationsrichtlinie](physical-walkthrough-replication.md)
