---
title: Gruppieren von Computern mithilfe von Computerabhängigkeiten per Azure Migrate | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie mithilfe von Computerabhängigkeiten mit dem Azure Migrate-Dienst eine Bewertung erstellen.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/05/2018
ms.author: raynew
ms.openlocfilehash: 8756809de4ec1a8150610027a8197f1bcae213f0
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53252505"
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Gruppieren von Computern per Mapping von Computerabhängigkeiten

In diesem Artikel wird beschrieben, wie Sie eine Gruppe mit Computern für eine [Azure Migrate](migrate-overview.md)-Bewertung erstellen, indem Sie die Computerabhängigkeiten visualisieren. Sie verwenden diese Methode normalerweise, wenn Sie Gruppen mit VMs mit höheren Zuverlässigkeitsgraden bewerten möchten, indem Sie vor dem Durchführen einer Bewertung die Computerabhängigkeiten überprüfen. Durch eine Visualisierung der Abhängigkeiten können Sie Ihre Migration zu Azure effizient planen. So können Sie sicherstellen, dass Sie nichts übersehen und dass es bei der Migration zu Azure nicht zu unvorhergesehenen Ausfällen kommt. Sie können alle voneinander abhängigen Systeme ermitteln, die gemeinsam migriert werden müssen, und ermitteln, ob ein ausgeführtes System noch benötigt wird oder eher außer Betrieb gesetzt als migriert werden sollte.

> [!NOTE]
> Die Funktion zur Visualisierung von Abhängigkeiten ist in Azure Government nicht verfügbar.

## <a name="prepare-for-dependency-visualization"></a>Vorbereiten für die Visualisierung der Abhängigkeiten
Azure Migrate nutzt für die Visualisierung von Computerabhängigkeiten die Dienstzuordnungslösung in Log Analytics.

### <a name="associate-a-log-analytics-workspace"></a>Zuordnen eines Log Analytics-Arbeitsbereichs
Sie müssen jedem Azure Migrate-Projekt einen neuen oder vorhandenen Log Analytics-Arbeitsbereich zuordnen, um die Abhängigkeitsvisualisierung nutzen zu können. Sie können einen Arbeitsbereich nur in dem Abonnement erstellen oder anfügen, in dem das Migrationsprojekt erstellt wird.

- Um einem Projekt einen Log Analytics-Arbeitsbereich anzufügen, wechseln Sie unter **Overview** (Übersicht) zum Projektabschnitt **Essentials** (Zusammenfassung), und klicken Sie auf **Requires configuration** (Erfordert Konfiguration).

    ![Zuordnen von Log Analytics-Arbeitsbereichen](./media/concepts-dependency-visualization/associate-workspace.png)

- Beim Zuordnen eines Arbeitsbereichs erhalten Sie die Möglichkeit, einen neuen Arbeitsbereich zu erstellen oder einen vorhandenen anzufügen:
  - Wenn Sie einen neuen Arbeitsbereich erstellen, müssen Sie für diesen einen Namen angeben. Der Arbeitsbereich wird dann in einer Region in derselben [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/) erstellt, in der auch das Migrationsprojekt erstellt wurde.
  - Wenn Sie einen vorhandenen Arbeitsbereich anfügen, können Sie zwischen allen verfügbaren Arbeitsbereichen im selben Abonnement wie das Migrationsprojekt auswählen. Beachten Sie, dass nur die Arbeitsbereiche aufgeführt sind, die in einer Region erstellt wurden, in der die [Dienstzuordnung unterstützt wird](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-configure#supported-azure-regions). Um einen Arbeitsbereich anzufügen, stellen Sie sicher, dass Sie Lesezugriff auf den Arbeitsbereich besitzen.

> [!NOTE]
> Den einem Migrationsprojekt zugeordneten Arbeitsbereich können Sie nicht ändern.

### <a name="download-and-install-the-vm-agents"></a>Herunterladen und Installieren der VM-Agents
Beim Konfigurieren eines Arbeitsbereiches müssen Sie auf allen lokalen Computern, für die Sie eine Bewertung durchführen möchten, Agents herunterladen und installieren. Falls Sie über Computer ohne Internetverbindung verfügen, ist es außerdem erforderlich, dafür das [Log Analytics-Gateway](../azure-monitor/platform/gateway.md) herunterzuladen und zu installieren.

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

[Erfahren Sie mehr](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) über die Liste der Unterstützungen durch den MMA für Windows-Betriebssysteme.

Gehen Sie wie folgt vor, um einen Agent auf einem Linux-Computer zu installieren:

1. Übertragen Sie das entsprechende Paket (x86 oder x64) mithilfe von scp/sftp auf Ihren Linux-Computer.
2. Installieren Sie das Bundle mit dem Argument „--install“.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

[Erfahren Sie mehr](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) über die Liste der Unterstützungen durch den MMA für Linux-Betriebssysteme.

### <a name="install-the-dependency-agent"></a>Installieren des Abhängigkeits-Agents
1. Doppelklicken Sie zum Installieren des Abhängigkeits-Agents auf einem Windows-Computer auf die Setupdatei, und befolgen Sie die Schritte im Assistenten.
2. Installieren Sie den Abhängigkeits-Agent auf einem Linux-Computer als „root“, indem Sie den folgenden Befehl verwenden:

    ```sh InstallDependencyAgent-Linux64.bin```

Erfahren Sie mehr zur Unterstützung des Dependency-Agents für die Betriebssysteme [Windows](../azure-monitor/insights/service-map-configure.md#supported-windows-operating-systems) und [Linux](../azure-monitor/insights/service-map-configure.md#supported-linux-operating-systems).

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

- [Erfahren Sie mehr](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) über die häufig gestellten Fragen (FAQs) zur Visualisierung von Abhängigkeiten.
- Informieren Sie sich über die [Vorgehensweise](how-to-create-group-dependencies.md) zum Verfeinern der Gruppe durch das Visualisieren von Gruppenabhängigkeiten.
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
