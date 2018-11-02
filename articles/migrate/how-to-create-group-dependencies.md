---
title: Verfeinern einer Bewertungsgruppe per Mapping von Gruppenabhängigkeiten in Azure Migrate | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Bewertung verfeinern, indem Sie für den Azure Migrate-Dienst das Mapping von Gruppenabhängigkeiten verwenden.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 09/25/2018
ms.author: raynew
ms.openlocfilehash: 2d2688799b1a0b4518b9c91bbc530936c834c5e3
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407435"
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Verfeinern einer Gruppe per Mapping von Gruppenabhängigkeiten

Dieser Artikel beschreibt, wie Sie eine Gruppe verfeinern, indem Sie die Abhängigkeiten aller Computer in der Gruppe visualisieren. Sie verwenden diese Methode normalerweise, wenn Sie die Mitgliedschaften für eine vorhandene Gruppe durch eine Überprüfung der Gruppenabhängigkeiten vor dem Durchführen einer Bewertung verfeinern möchten. Durch das Optimieren einer Gruppe mithilfe der Visualisierung der Abhängigkeiten können Sie Ihre Migration zu Azure effizient planen. Ermitteln Sie alle unabhängigen Systeme, die zusammen migriert werden müssen. So können Sie sicherstellen, dass Sie nichts übersehen und dass es bei der Migration zu Azure nicht zu unvorhergesehenen Ausfällen kommt.


> [!NOTE]
> Gruppen, für die Sie Gruppenabhängigkeiten visualisieren möchten, sollten höchstens zehn Computer enthalten. Wenn mehr als zehn Computer in der Gruppe vorhanden sind, empfiehlt es sich, sie in kleinere Gruppen aufzuteilen, um die Visualisierung der Abhängigkeiten nutzen zu können.


## <a name="prepare-for-dependency-visualization"></a>Vorbereiten für die Visualisierung der Abhängigkeiten
Azure Migrate nutzt für die Visualisierung von Computerabhängigkeiten die Dienstzuordnungslösung in Log Analytics.

### <a name="associate-a-log-analytics-workspace"></a>Zuordnen eines Log Analytics-Arbeitsbereichs
Sie müssen jedem Azure Migrate-Projekt einen neuen oder vorhandenen Log Analytics-Arbeitsbereich zuordnen, um die Abhängigkeitsvisualisierung nutzen zu können. Sie können einen Arbeitsbereich nur in dem Abonnement erstellen oder anfügen, in dem das Migrationsprojekt erstellt wird.

- Um einem Projekt einen Log Analytics-Arbeitsbereich anzufügen, wechseln Sie unter **Overview** (Übersicht) zum Projektabschnitt **Essentials** (Zusammenfassung), und klicken Sie auf **Requires configuration** (Erfordert Konfiguration).

    ![Zuordnen von Log Analytics-Arbeitsbereichen](./media/concepts-dependency-visualization/associate-workspace.png)

- Wenn Sie einen neuen Arbeitsbereich erstellen, müssen Sie für diesen einen Namen angeben. Der Arbeitsbereich wird dann in demselben Abonnement erstellt wie das Migrationsprojekt und in einer Region in derselben [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/), in der auch das Migrationsprojekt erstellt wurde.
- Die Option **Use existing** (Vorhandene verwenden) listet nur diejenigen Arbeitsbereiche auf, die in Regionen erstellt wurden, in denen eine Dienstzuordnung verfügbar ist. Wenn Sie über einen Arbeitsbereich in einer Region ohne Dienstzuordnung verfügen, wird dieser in der Dropdown-Liste nicht angezeigt.

> [!NOTE]
> Den einem Migrationsprojekt zugeordneten Arbeitsbereich können Sie nicht ändern.

### <a name="download-and-install-the-vm-agents"></a>Herunterladen und Installieren der VM-Agents
Um Abhängigkeiten einer Gruppe anzuzeigen, müssen Sie auf allen lokalen Computern, die der Gruppe angehören, Agents herunterladen und installieren. Falls Sie über Computer ohne Internetverbindung verfügen, ist es außerdem erforderlich, dafür das [Log Analytics-Gateway](../log-analytics/log-analytics-oms-gateway.md) herunterzuladen und zu installieren.

1. Klicken Sie unter **Übersicht** auf **Verwalten** > **Gruppen**, und wechseln Sie zur gewünschten Gruppe.
2. Klicken Sie in der Liste der Computer in der Spalte **Abhängigkeits-Agent** auf **Installation erforderlich**, um Anweisungen zum Herunterladen und Installieren der Agents anzuzeigen.
3. Laden Sie auf der Seite **Abhängigkeiten** den Microsoft Monitoring Agent (MMA) herunter, und installieren Sie diesen sowie den Abhängigkeits-Agent auf allen VMs, die der Gruppe angehören.
4. Kopieren Sie die Arbeitsbereichs-ID und den dazugehörigen Schlüssel. Sie benötigen diese Angaben beim Installieren des MMA auf dem lokalen Computer.

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

Erfahren Sie mehr zur Unterstützung des Dependency-Agents für die Betriebssysteme [Windows](../monitoring/monitoring-service-map-configure.md#supported-windows-operating-systems) und [Linux](../monitoring/monitoring-service-map-configure.md#supported-linux-operating-systems).

## <a name="refine-the-group-based-on-dependency-visualization"></a>Verfeinern der Gruppe anhand der Visualisierung der Abhängigkeiten
Nachdem Sie die Agents auf allen Computern der Gruppe installiert haben, können Sie die Abhängigkeiten der Gruppe visualisieren und diese anhand der unten aufgeführten Schritte verfeinern.

1. Klicken Sie im Azure Migrate-Projekt unter **Verwalten** auf  **Gruppen**, und wählen Sie eine Gruppe aus.
2. Klicken Sie auf der Seite der Gruppe auf  **Abhängigkeiten anzeigen**, um die Übersicht mit den Gruppenabhängigkeiten zu öffnen.
3. In der Abhängigkeitsübersicht für die Gruppe werden die folgenden Informationen angezeigt:
    - Eingehende (Clients) und ausgehende TCP-Verbindungen (Server) von/zu allen Computern in der Gruppe
        - Die abhängigen Computer, auf denen keine MMA- und Abhängigkeits-Agents installiert sind, gruppiert nach Portnummern
        - Die abhängigen Computer, auf denen MMA- und Abhängigkeits-Agents installiert sind, als separate Felder
    - Innerhalb des Computers ausgeführte Prozesse können Sie einblenden, indem Sie das Feld für den jeweiligen Computer erweitern.
    - Eigenschaften wie der vollqualifizierte Domänenname, das Betriebssystem, die MAC-Adresse usw. für jeden Computer können Sie anzeigen, indem Sie auf das Feld für den jeweiligen Computer klicken.

     ![Anzeigen der Gruppenabhängigkeiten](./media/how-to-create-group-dependencies/view-group-dependencies.png)

3. Klicken Sie auf den Zeitraum, und ändern Sie ihn, um ausführlichere Informationen zu den Abhängigkeiten zu erhalten. Standardmäßig ist ein Bereich von einer Stunde ausgewählt. Sie können den Zeitraum ändern oder das Start- und Enddatum und die Dauer angeben.
4. Überprüfen Sie die abhängigen Computer und die auf jedem Computer ausgeführten Prozesse, und identifizieren Sie die Computer, die der Gruppe hinzugefügt oder aus dieser entfernt werden sollen.
5. Klicken Sie bei gedrückter STRG-Taste, um Computer in der Übersicht auszuwählen, die der Gruppe hinzugefügt oder daraus entfernt werden sollen.
    - Sie können nur Computer hinzufügen, die ermittelt wurden.
    - Wenn Sie Computer für eine Gruppe hinzufügen und entfernen, werden die vorherigen Bewertungen dafür ungültig.
    - Sie können optional eine neue Bewertung erstellen, wenn Sie die Gruppe ändern.
5. Klicken Sie auf **OK**, um die Gruppe zu speichern.

    ![Hinzufügen oder Entfernen von Computern](./media/how-to-create-group-dependencies/add-remove.png)

Wenn Sie die Abhängigkeiten eines bestimmten Computers überprüfen möchten, der in der Übersicht mit den Gruppenabhängigkeiten angezeigt wird, können Sie eine [Übersicht mit den Computerabhängigkeiten einrichten](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) über die häufig gestellten Fragen (FAQs) zur Visualisierung von Abhängigkeiten.
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
