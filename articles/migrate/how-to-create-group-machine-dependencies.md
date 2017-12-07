---
title: "Gruppieren von Computern mithilfe von Computerabhängigkeiten per Azure Migrate | Microsoft-Dokumentation"
description: "Hier wird beschrieben, wie Sie mithilfe von Computerabhängigkeiten mit dem Azure Migrate-Dienst eine Bewertung erstellen."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0527e34e-a078-405e-aeb9-c91a5808112a
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 5ed49f3dc71af6a8c1c7b6c9e38fd84452505aec
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2017
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Gruppieren von Computern per Mapping von Computerabhängigkeiten

In diesem Artikel wird beschrieben, wie Sie per Mapping von Computerabhängigkeiten eine Gruppe mit Computern für eine [Azure Migrate](migrate-overview.md)-Bewertung erstellen. Sie verwenden diese Methode normalerweise, wenn Sie Gruppen mit VMs mit höheren Zuverlässigkeitsgraden bewerten möchten, indem Sie vor dem Durchführen einer Bewertung die Computerabhängigkeiten überprüfen.



## <a name="prepare-machines-for-dependency-mapping"></a>Vorbereiten von Computern für das Mapping von Abhängigkeiten
Zum Einbinden von Computern in das Mapping von Abhängigkeiten müssen Sie Agents für alle lokalen Computer, die Sie auswerten möchten, herunterladen und installieren. Falls Sie über Computer ohne Internetverbindung verfügen, ist es außerdem erforderlich, dafür das [OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md) herunterzuladen und zu installieren.

### <a name="download-and-install-the-vm-agents"></a>Herunterladen und Installieren der VM-Agents
1. Klicken Sie unter **Übersicht** auf **Verwalten** > **Computer**, und wählen Sie den gewünschten Computer aus.
2. Klicken Sie in der Spalte **Abhängigkeiten** auf **Agents installieren**. 
3. Laden Sie auf der Seite **Abhängigkeiten** den Microsoft Monitoring Agent (MMA) herunter, und installieren Sie ihn – sowie den Abhängigkeits-Agent auf allen VMs, die Sie auswerten möchten.
4. Kopieren Sie die Arbeitsbereichs-ID und den dazugehörigen Schlüssel. Sie benötigen diese Angaben beim Installieren des MMA auf dem lokalen Computer.

### <a name="install-the-mma"></a>Installieren des MMA

Gehen Sie wie folgt vor, um den Agent auf einem Windows-Computer zu installieren:

1. Doppelklicken Sie auf den heruntergeladenen Agent.
2. Klicken Sie auf der Seite **Willkommen**auf **Weiter**. Klicken Sie auf der Seite **Lizenzbedingungen** auf **Ich stimme zu**, um die Lizenzbedingungen zu akzeptieren.
3. Behalten Sie unter **Zielordner** den Standardinstallationsordner bei, oder ändern Sie ihn, und klicken Sie anschließend auf **Weiter**. 
4. Wählen Sie unter **Agent-Setupoptionen** die Optionen **Azure Log Analytics (OMS)** > **Weiter**. 
5. Klicken Sie auf **Hinzufügen**, um einen neuen OMS-Arbeitsbereich hinzuzufügen. Fügen Sie die Arbeitsbereichs-ID und den dazugehörigen Schlüssel ein, die bzw. den Sie im Portal kopiert haben. Klicken Sie auf **Weiter**.



Gehen Sie wie folgt vor, um einen Agent auf einem Linux-Computer zu installieren:

1. Übertragen Sie das entsprechende Bundle (x86 oder x64) mithilfe von scp/sftp auf Ihren Linux-Computer.
2. Installieren Sie das Bundle mit dem Argument „--install“.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>Installieren des Abhängigkeits-Agents
1. Doppelklicken Sie zum Installieren des Abhängigkeits-Agents auf einem Windows-Computer auf die Setupdatei, und befolgen Sie die Schritte im Assistenten.
2. Installieren Sie den Abhängigkeits-Agent auf einem Linux-Computer als „root“, indem Sie den folgenden Befehl verwenden:

    ```sh InstallDependencyAgent-Linux64.bin```

[Erfahren Sie mehr](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) zu den Betriebssystemen, die vom Abhängigkeits-Agent unterstützt werden. 

## <a name="create-a-group"></a>Erstellen einer Gruppe

1. Navigieren Sie nach der Installation des Agents zum Portal, und klicken Sie auf **Verwalten** > **Computer**.
2. Die Spalte **Abhängigkeiten** sollte jetzt als **Abhängigkeiten anzeigen** erscheinen. Klicken Sie auf die Spalte, um die Abhängigkeiten anzuzeigen.
3. Für jeden Computer können Sie Folgendes überprüfen:
    - Ob der MMA und der Abhängigkeits-Agent installiert sind und ob der Computer erkannt wurde.
    - Das auf dem Computer ausgeführte Gastbetriebssystem.
    - Ein- und ausgehende IP-Verbindungen und -Ports.
    - Prozesse, die auf Computern ausgeführt werden.
    - Abhängigkeiten zwischen Computern.

4. Sie können auf den Zeitraum klicken und ihn ändern, um ausführlichere Informationen zu den Abhängigkeiten zu erhalten. Standardmäßig ist ein Bereich von einer Stunde ausgewählt. Sie können den Zeitraum ändern oder das Start- und Enddatum und die Dauer angeben.
5. Nachdem Sie abhängige Computer identifiziert haben, die Sie gruppieren möchten, können Sie die Computer in der Karte auswählen und auf **Computer gruppieren** klicken.
6. Geben Sie einen Gruppennamen an. Stellen Sie sicher, dass der Computer von Azure Migrate ermittelt wird. Führen Sie den Ermittlungsprozess lokal erneut aus, wenn dies nicht der Fall ist. Wenn Sie möchten, können Sie sofort eine Bewertung durchführen.
7. Klicken Sie auf **OK**, um die Gruppe zu speichern.

    ![Erstellen einer Gruppe mit Computerabhängigkeiten](./media/how-to-create-group-machine-dependencies/create-group.png)

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [Vorgehensweise](how-to-create-group-dependencies.md) zum Verfeinern der Gruppe durch das Überprüfen der Gruppenabhängigkeiten.
- Lesen Sie die [weiteren Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen.
