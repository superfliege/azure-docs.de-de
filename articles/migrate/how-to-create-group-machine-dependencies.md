---
title: Gruppieren von Computern mithilfe von Computerabhängigkeiten per Azure Migrate | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie mithilfe von Computerabhängigkeiten mit dem Azure Migrate-Dienst eine Bewertung erstellen.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/05/2018
ms.author: raynew
ms.openlocfilehash: fc74af2e7f19d05ff53925b2765c1f78fd0b30c1
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919708"
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Gruppieren von Computern per Mapping von Computerabhängigkeiten

In diesem Artikel wird beschrieben, wie Sie eine Gruppe mit Computern für eine [Azure Migrate](migrate-overview.md)-Bewertung erstellen, indem Sie die Computerabhängigkeiten visualisieren. Sie verwenden diese Methode normalerweise, wenn Sie Gruppen mit VMs mit höheren Zuverlässigkeitsgraden bewerten möchten, indem Sie vor dem Durchführen einer Bewertung die Computerabhängigkeiten überprüfen. Durch eine Visualisierung der Abhängigkeiten können Sie Ihre Migration zu Azure effizient planen. So können Sie sicherstellen, dass Sie nichts übersehen und dass es bei der Migration zu Azure nicht zu unvorhergesehenen Ausfällen kommt. Sie können alle voneinander abhängigen Systeme ermitteln, die gemeinsam migriert werden müssen, und ermitteln, ob ein ausgeführtes System noch benötigt wird oder eher außer Betrieb gesetzt als migriert werden sollte.


## <a name="prepare-machines-for-dependency-mapping"></a>Vorbereiten von Computern für das Mapping von Abhängigkeiten
Um Computerabhängigkeiten anzuzeigen, müssen Sie Agents auf allen lokalen Computern herunterladen und installieren, für die Sie eine Bewertung durchführen möchten. Falls Sie über Computer ohne Internetverbindung verfügen, ist es außerdem erforderlich, dafür das [OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md) herunterzuladen und zu installieren.

### <a name="download-and-install-the-vm-agents"></a>Herunterladen und Installieren der VM-Agents
1. Klicken Sie unter **Übersicht** auf **Verwalten** > **Computer**, und wählen Sie den gewünschten Computer aus.
2. Klicken Sie in der Spalte **Abhängigkeiten** auf **Agents installieren**.
3. Laden Sie auf der Seite **Abhängigkeiten** den Microsoft Monitoring Agent (MMA) herunter, und installieren Sie ihn – sowie den Abhängigkeits-Agent auf allen VMs, die Sie auswerten möchten.
4. Kopieren Sie die Arbeitsbereichs-ID und den dazugehörigen Schlüssel. Sie benötigen diese Angaben beim Installieren des MMA auf dem lokalen Computer.

> [!NOTE]
> Um die Installation von Agents zu automatisieren, können Sie Bereitstellungstools wie System Center Configuration Manager verwenden oder unser Partner-Tool [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration), das eine Agent-Bereitstellungslösung für Azure Migrate enthält.

### <a name="install-the-mma"></a>Installieren des MMA

Gehen Sie wie folgt vor, um den Agent auf einem Windows-Computer zu installieren:

1. Doppelklicken Sie auf den heruntergeladenen Agent.
2. Klicken Sie auf der Seite **Willkommen**auf **Weiter**. Klicken Sie auf der Seite **Lizenzbedingungen** auf **Ich stimme zu**, um die Lizenzbedingungen zu akzeptieren.
3. Behalten Sie unter **Zielordner** den Standardinstallationsordner bei, oder ändern Sie ihn, und klicken Sie anschließend auf **Weiter**.
4. Wählen Sie unter **Agent-Setupoptionen** die Optionen **Azure Log Analytics** > **Weiter**.
5. Klicken Sie auf **Hinzufügen**, um einen neuen Log Analytics-Arbeitsbereich hinzuzufügen. Fügen Sie die Arbeitsbereichs-ID und den dazugehörigen Schlüssel ein, die bzw. den Sie im Portal kopiert haben. Klicken Sie auf **Weiter**.


Gehen Sie wie folgt vor, um einen Agent auf einem Linux-Computer zu installieren:

1. Übertragen Sie das entsprechende Paket (x86 oder x64) mithilfe von scp/sftp auf Ihren Linux-Computer.
2. Installieren Sie das Bundle mit dem Argument „--install“.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>Installieren des Abhängigkeits-Agents
1. Doppelklicken Sie zum Installieren des Abhängigkeits-Agents auf einem Windows-Computer auf die Setupdatei, und befolgen Sie die Schritte im Assistenten.
2. Installieren Sie den Abhängigkeits-Agent auf einem Linux-Computer als „root“, indem Sie den folgenden Befehl verwenden:

    ```sh InstallDependencyAgent-Linux64.bin```

[Erfahren Sie mehr](../monitoring/monitoring-service-map-configure.md#supported-operating-systems) zu den Betriebssystemen, die vom Abhängigkeits-Agent unterstützt werden.

[Erfahren Sie mehr](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) darüber, wie Sie den Dependency-Agent mithilfe von Skripts installieren können.

## <a name="create-a-group"></a>Erstellen einer Gruppe

1. Navigieren Sie nach der Installation des Agents zum Portal, und klicken Sie auf **Verwalten** > **Computer**.
2. Suchen Sie nach dem Computer, auf dem Sie die Agents installiert haben.
3. Die Spalte **Abhängigkeiten** für den Computer sollte jetzt als **Abhängigkeiten anzeigen** erscheinen. Klicken Sie auf die Spalte, um die Abhängigkeiten des Computers anzuzeigen.
4. In der Abhängigkeitszuordnung für den Computer werden die folgenden Informationen angezeigt:
    - Eingehende (Clients) und ausgehende (Server) TCP-Verbindungen vom/zum Computer.
        - Die abhängigen Computer, auf denen keine MMA- und Abhängigkeits-Agents installiert sind, gruppiert nach Portnummern
        - Die abhängigen Computer, auf denen MMA- und Abhängigkeits-Agents installiert sind, als separate Felder
    - Innerhalb des Computers ausgeführte Prozesse können Sie einblenden, indem Sie das Feld für den jeweiligen Computer erweitern.
    - Eigenschaften wie vollqualifizierter Domänenname, Betriebssystem, MAC-Adresse usw. für jeden Computer können Sie anzeigen, indem Sie auf das Feld für den jeweiligen Computer klicken.

 ![Anzeigen von Computerabhängigkeiten](./media/how-to-create-group-machine-dependencies/machine-dependencies.png)

4. Sie können Abhängigkeiten für verschiedene Zeiträume anzeigen, indem Sie auf im Zeitbereich auf die Zeitdauer klicken. Standardmäßig ist ein Bereich von einer Stunde ausgewählt. Sie können den Zeitraum ändern oder das Start- und Enddatum und die Dauer angeben.
5. Nachdem Sie abhängige Computer identifiziert haben, die Sie gruppieren möchten, können Sie die gewünschten Computer mit Strg+Klick auswählen und auf **Computer gruppieren** klicken.
6. Geben Sie einen Gruppennamen an. Stellen Sie sicher, dass die abhängigen Computer von Azure Migrate ermittelt werden.

    > [!NOTE]
    > Wenn ein abhängiger Computer nicht von Azure Migrate ermittelt wurde, können Sie ihn nicht zur Gruppe hinzufügen. Um solche Computer zur Gruppe hinzuzufügen, müssen Sie den Ermittlungsprozess mit dem richtigen Bereich erneut in vCenter Server ausführen und sicherstellen, dass der Computer von Azure Migrate ermittelt wird.  

7. Wenn Sie eine Bewertung für diese Gruppe erstellen möchten, aktivieren Sie das Kontrollkästchen, um eine neue Bewertung für die Gruppe zu erstellen.
8. Klicken Sie auf **OK**, um die Gruppe zu speichern.

Nach der Erstellung der Gruppe wird empfohlen, alle Agents auf sämtlichen Computern in der Gruppe zu installieren und die Gruppe zu verfeinern, indem die Abhängigkeiten der gesamten Gruppe visualisiert werden.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [Vorgehensweise](how-to-create-group-dependencies.md) zum Verfeinern der Gruppe durch das Visualisieren der Gruppenabhängigkeiten.
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
