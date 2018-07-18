---
title: Verfeinern einer Bewertungsgruppe per Mapping von Gruppenabhängigkeiten in Azure Migrate | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Bewertung verfeinern, indem Sie für den Azure Migrate-Dienst das Mapping von Gruppenabhängigkeiten verwenden.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: a91543dcea1fb9fb6a33b5a7bc3892a2bac7b241
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215782"
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Verfeinern einer Gruppe per Mapping von Gruppenabhängigkeiten

Dieser Artikel beschreibt, wie Sie eine Gruppe verfeinern, indem Sie die Abhängigkeiten aller Computer in der Gruppe visualisieren. Sie verwenden diese Methode normalerweise, wenn Sie die Mitgliedschaften für eine vorhandene Gruppe durch eine Überprüfung der Gruppenabhängigkeiten vor dem Durchführen einer Bewertung verfeinern möchten. Durch das Optimieren einer Gruppe mithilfe der Visualisierung der Abhängigkeiten können Sie Ihre Migrationen zu Azure effektiv planen. Sie können alle ineinandergreifende Systeme ermitteln, die zusammen migriert werden müssen. So können Sie sicherstellen, dass Sie nichts übersehen und dass es bei der Migration zu Azure nicht zu unvorhergesehenen Ausfällen kommt. 


> [!NOTE]
> Gruppen, für die Sie Gruppenabhängigkeiten visualisieren möchten, sollten höchstens zehn Computer enthalten. Wenn mehr als zehn Computer in der Gruppe vorhanden sind, empfiehlt es sich, sie in kleinere Gruppen aufzuteilen, um die Visualisierung der Abhängigkeiten nutzen zu können.


# <a name="prepare-the-group-for-dependency-visualization"></a>Vorbereiten der Gruppe für die Visualisierung der Abhängigkeiten
Um Abhängigkeiten einer Gruppe anzuzeigen, müssen Sie auf allen lokalen Computern, die der Gruppe angehören, Agents herunterladen und installieren. Falls Sie über Computer ohne Internetverbindung verfügen, ist es außerdem erforderlich, dafür das [OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md) herunterzuladen und zu installieren.

### <a name="download-and-install-the-vm-agents"></a>Herunterladen und Installieren der VM-Agents
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

[Erfahren Sie mehr](../monitoring/monitoring-service-map-configure.md#supported-operating-systems) zu den Betriebssystemen, die vom Abhängigkeits-Agent unterstützt werden. 

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

[Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
