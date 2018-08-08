---
title: Verwenden des erweiterten Spark-Verlaufsservers zum Debuggen und Diagnostizieren von Spark-Anwendungen – Azure HDInsight | Microsoft-Dokumentation
description: Verwenden des erweiterten Spark-Verlaufsservers zum Debuggen und Diagnostizieren von Spark-Anwendungen – Azure HDInsight
services: hdinsight
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: ''
ms.topic: article
ms.date: 07/12/2018
ms.author: jejiang
ms.openlocfilehash: 61e857b01f75cb30e3d213703bd8ee8014d902d6
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39309630"
---
# <a name="use-extended-spark-history-server-to-debug-and-diagnose-spark-applications"></a>Verwenden des erweiterten Spark-Verlaufsservers zum Debuggen und Diagnostizieren von Spark-Anwendungen

Dieser Artikel enthält Anleitungen zur Verwendung des erweiterten Spark-Verlaufsservers zum Debuggen und Diagnostizieren abgeschlossener und aktiver Spark-Anwendungen. Die Erweiterung enthält derzeit die Registerkarten „Daten“ und „Diagramm“. Auf der Registerkarte „Daten“ können Benutzer die Eingabe- und Ausgabedaten des Spark-Auftrags überprüfen. Auf der Registerkarte „Diagramm“ können Benutzer den Datenfluss überprüfen und das Auftragsdiagramm wiedergeben.

## <a name="open-the-spark-history-server"></a>Öffnen des Spark-Verlaufsservers

Der Spark-Verlaufsserver ist die Webbenutzeroberfläche für abgeschlossene und ausgeführte Spark-Anwendungen. 

### <a name="to-open-the-spark-history-server-web-ui-from-azure-portal"></a>So öffnen Sie die Webbenutzeroberfläche des Spark-Verlaufsservers über das Azure-Portal

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) den Spark-Cluster. Weitere Informationen finden Sie unter [Auflisten und Anzeigen von Clustern](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
2. Klicken Sie unter **Quicklinks** auf **Clusterdashboard** und dann auf **Spark-Verlaufsserver**. Geben Sie bei Aufforderung die Anmeldeinformationen für den Spark-Cluster ein. 

    ![Spark-Verlaufsserver](./media/apache-azure-spark-history-server/launch-history-server.png "Spark-Verlaufsserver")

### <a name="to-open-the-spark-history-server-web-ui-by-url"></a>So öffnen Sie die Webbenutzeroberfläche des Spark-Verlaufsservers mit einer URL
Öffnen Sie den Spark-Verlaufsserver durch Navigieren zur folgenden URL. Ersetzen Sie <ClusterName> mit dem Spark-Clusternamen des Kunden.

   ```
   https://<ClusterName>.azurehdinsight.net/sparkhistory
   ```

Die Webbenutzeroberfläche des Spark-Verlaufsservers sieht wie folgt aus:

![HDInsight Spark-Verlaufsserver](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)


## <a name="open-the-data-tab-from-spark-history-server"></a>Öffnen der Registerkarte „Daten“ des Spark-Verlaufsservers
Wählen Sie die Auftrags-ID aus, und klicken Sie im Toolmenü auf **Daten**, um die Datenansicht aufzurufen.

+ Überprüfen Sie die **Eingaben**, **Ausgaben** und **Tabellenvorgänge**, indem Sie die Registerkarten getrennt auswählen.

    ![Registerkarten unter „Daten“](./media/apache-azure-spark-history-server/sparkui-data-tabs.png)

+ Kopieren Sie alle Zeilen, indem Sie auf die Schaltfläche **Kopieren** klicken.

    ![Daten kopieren](./media/apache-azure-spark-history-server/sparkui-data-copy.png)

+ Speichern Sie alle Daten als CSV-Datei, indem Sie auf die Schaltfläche **CSV** klicken.

    ![Daten speichern](./media/apache-azure-spark-history-server/sparkui-data-save.png)

+ Suchen Sie durch Eingeben von Schlüsselwörtern in das Feld **Suche**. Das Suchergebnis wird sofort angezeigt.

    ![Daten suchen](./media/apache-azure-spark-history-server/sparkui-data-search.png)

+ Klicken Sie auf die Spaltenüberschrift, um die Tabelle zu sortieren. Klicken Sie auf das Pluszeichen, um eine Zeile zu erweitern und weitere Details anzuzeigen, oder klicken Sie auf das Minuszeichen, um eine Zeile zu reduzieren.

    ![Datentabelle](./media/apache-azure-spark-history-server/sparkui-data-table.png)

+ Laden Sie eine einzelne Datei herunter, indem Sie rechts auf Schaltfläche **Unvollständiger Download** klicken. Die ausgewählte Datei wird dann lokal heruntergeladen. Wenn die Datei nicht mehr vorhanden ist, wird eine neue Registerkarte geöffnet, um die Fehlermeldungen anzuzeigen.

    ![Zeile für Datendownload](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Kopieren Sie den vollständigen oder relativen Pfad durch Auswählen von **Vollständigen Pfad kopieren** oder **Relativen Pfad kopieren** aus dem Downloadmenü. Für Azure Data Lake Storage-Dateien wird mit **Im Azure Storage-Explorer öffnen** der Azure Storage-Explorer gestartet und der Ordner beim Anmelden aufgerufen.

    ![Pfad für Daten kopieren](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Klicken Sie auf die Zahl unter der Tabelle, um durch Seiten zu navigieren, wenn zu viele Zeilen für die Anzeige auf einer Seite vorhanden sind. 

    ![Datenseite](./media/apache-azure-spark-history-server/sparkui-data-page.png)

+ Zeigen Sie auf das Fragezeichen neben den Daten, um die QuickInfo anzuzeigen, oder klicken Sie auf das Fragezeichen, um weitere Informationen zu erhalten.

    ![Weitere Informationen zu Daten](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+ Senden Sie Feedback mit Problemen, indem Sie auf **Feedback abgeben** klicken.

    ![Feedback zum Diagramm](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="open-the-graph-tab-from-spark-history-server"></a>Öffnen der Registerkarte „Diagramm“ des Spark-Verlaufsservers
Wählen Sie die Auftrags-ID aus, und klicken Sie im Toolmenü auf **Diagramm**, um die Ansicht mit dem Auftragsdiagramm aufzurufen.

+ Überprüfen Sie die Übersicht über den Auftrag anhand des generierten Auftragsdiagramms. 

+ Standardmäßig werden alle Aufträge angezeigt. Die Ansicht kann nach **Auftrags-ID** gefiltert werden.

    ![Diagramm der Auftrags-IDs](./media/apache-azure-spark-history-server/sparkui-graph-jobid.png)

+ Standardmäßig ist **Fortschritt** ausgewählt. Der Benutzer kann den Datenfluss überprüfen, indem er **Gelesen/Geschrieben** in der Dropdownliste **Anzeigen** auswählt.

    ![Diagrammanzeige](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

    Die Diagrammknoten werden in Farben angezeigt, die ein Wärmebild darstellen.

    ![Wärmebild des Diagramms](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)

+ Geben Sie den Auftrag wieder, indem Sie auf die Schaltfläche **Wiedergabe** klicken. Sie können die Wiedergabe jederzeit beenden, indem Sie auf die Schaltfläche „Beenden“ klicken. Die Aufgaben werden in Farbe angezeigt, um verschiedene Status bei der Wiedergabe darzustellen:

    + Grün für Erfolg: Der Auftrag wurde erfolgreich abgeschlossen.
    + Orange für Wiederholung: Instanzen von Aufgaben mit Fehlern, die aber keine Auswirkungen auf das endgültige Ergebnis des Auftrags haben. Für diese Aufgaben gibt es doppelte oder Wiederholungsinstanzen, die später möglicherweise erfolgreich sind.
    + Rot für Fehler: Aufgaben mit Fehlern.
    + Blau für Ausführung: Die Aufgabe wird ausgeführt.
    + Weiß für übersprungene oder wartende Aufgaben: Die Aufgabe wartet auf die Ausführung, oder die Phase wurde übersprungen.

    ![Diagrammfarbbeispiel: Ausführung](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)
 
    ![Diagrammfarbbeispiel: Fehler](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)
 
    > [!NOTE]
    > Die Wiedergabe ist für jeden Auftrag zulässig. Wenn ein Auftrag keine Phasen aufweist oder noch nicht abgeschlossen ist, wird die Wiedergabe nicht unterstützt.


+ Vergrößern/verkleinern Sie das Auftragsdiagramm mit dem Mausrad, oder klicken Sie auf **Mit Zoom anpassen**, um es an die Bildschirmgröße anzupassen.
 
    ![Diagramm mit Zoom anpassen](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Zeigen Sie auf den Diagrammknoten, um die QuickInfo anzuzeigen, wenn es fehlgeschlagene Aufgaben gibt. Klicken Sie auf die Phase, um die Phasenseite zu öffnen.

    ![Diagramm-QuickInfo](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ Der Diagrammknoten des Auftrags zeigt die folgenden Informationen der einzelnen Phasen an:
    + ID
    + Name oder Beschreibung
    + Gesamtanzahl der Aufgaben
    + Gelesene Daten: die Summe der Eingabegröße und der Shuffle-Lesegröße
    + Geschriebene Daten: die Summe der Ausgabegröße und der Shuffle-Schreibgröße
    + Ausführungszeit: die Zeit zwischen der Startzeit des ersten Versuchs und Abschlusszeit des letzten Versuchs
    + Zeilenanzahl: die Summe der Eingabedatensätze, Ausgabedatensätze, Shuffle-Lesedatensätze und Shuffle-Schreibdatensätze
    + Fortschritt

    > [!NOTE]
    > Standardmäßig zeigt der Diagrammknoten des Auftrags Informationen zum letzten Versuch der einzelnen Phasen an (mit Ausnahme der Zeit für die Phasenausführung), aber während der Wiedergabe zeigt der Diagrammknoten Informationen zu jedem Versuch an.

    > [!NOTE]
    > Für die Datengröße von Lese- und Schreibvorgängen verwenden wir 1 MB = 1000 KB = 1000 * 1000 Bytes.

+ Senden Sie Feedback mit Problemen, indem Sie auf **Feedback abgeben** klicken.

    ![Feedback zum Diagramm](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="faq"></a>Häufig gestellte Fragen

### <a name="1-revert-to-community-version"></a>1. Wiederherstellen der Communityversion

Um die Communityversion wiederherzustellen, führen Sie die folgenden Schritte aus:

1. Öffnen Sie den Cluster in Ambari. Klicken Sie auf **Spark2** im linken Bereich.
2. Klicken Sie auf die Registerkarte **Configs**.
3. Erweitern Sie die Gruppe **Custom spark2-defaults**.
4. Klicken Sie auf **Add Property**, fügen Sie **spark.ui.enhancement.enabled=false** hinzu, und speichern Sie.
5. Die Eigenschaft ist jetzt auf **false** festgelegt.
6. Klicken Sie zum Speichern der Konfiguration auf **Speichern**.

    ![Feature wird deaktiviert](./media/apache-azure-spark-history-server/sparkui-turn-off.png)

7. Klicken Sie auf **Spark2** im linken Bereich, und klicken Sie auf der Registerkarte **Summary** auf **Spark2 History Server**.

    ![Server neu starten (1)](./media/apache-azure-spark-history-server/sparkui-restart-1.png) 

8. Starten Sie den Verlaufsserver neu, indem Sie für **Spark2 History Server** auf **Restart** klicken.

    ![Server neu starten (2)](./media/apache-azure-spark-history-server/sparkui-restart-2.png)  

9. Durch Aktualisieren der Webbenutzeroberfläche des Spark-Verlaufsservers wird die Communityversion wiederhergestellt.

### <a name="2-upload-history-server-event"></a>2. Hochladen von Verlaufsserverereignissen

Führen Sie bei einem Verlaufsserverfehler die Schritte zum Angeben des Ereignisses aus:
1. Laden Sie das Ereignis durch Klicken auf **Download** in der Webbenutzeroberfläche des Verlaufsservers herunter.

    ![Ereignis herunterladen](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. Klicken Sie auf der Registerkarte „Daten/Diagramm“ auf **Feedback abgeben**.

    ![Feedback zum Diagramm](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Geben Sie den Titel und eine Beschreibung des Fehlers an, ziehen Sie die ZIP-Datei in das Bearbeitungsfeld, und klicken Sie dann auf **Neues Problem melden**.

    ![Problem melden](./media/apache-azure-spark-history-server/sparkui-file-issue.png)


### <a name="3-upgrade-jar-file-for-hotfix-scenario"></a>3. Aktualisieren der JAR-Datei für das Hotfixszenario

Wenn Sie mit einem Hotfix eine Aktualisierung durchführen möchten, verwenden Sie das folgende Skript, mit dem „spark-enhancement.jar*“ aktualisiert wird.

**upgrade_spark_enhancement.sh**:

   ```bash
    #!/usr/bin/env bash

    # Copyright (C) Microsoft Corporation. All rights reserved.

    # Arguments:
    # $1 Enhancement jar path

    if [ "$#" -ne 1 ]; then
        >&2 echo "Please provide the upgrade jar path."
        exit 1
    fi

    install_jar() {
        tmp_jar_path="/tmp/spark-enhancement-hotfix-$( date +%s )"

        if wget -O "$tmp_jar_path" "$2"; then
            for FILE in "$1"/spark-enhancement*.jar
            do
                back_up_path="$FILE.original.$( date +%s )"
                echo "Back up $FILE to $back_up_path"
                mv "$FILE" "$back_up_path"
                echo "Copy the hotfix jar file from $tmp_jar_path   to $FILE"
                cp "$tmp_jar_path" "$FILE"

                "Hotfix done."
                break
            done
        else    
            >&2 echo "Download jar file failed."
            exit 1
        fi
    }

    jars_folder="/usr/hdp/current/spark2-client/jars"
    jar_path=$1

    if ls ${jars_folder}/spark-enhancement*.jar 1>/dev/null 2>&1;   then
        install_jar "$jars_folder" "$jar_path"
    else
        >&2 echo "There is no target jar on this node. Exit with no action."
        exit 0
    fi
   ```

**Verwendung**: 

`upgrade_spark_enhancement.sh https://${jar_path}`

**Beispiel**:

`upgrade_spark_enhancement.sh https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.tgz` 

**So verwenden Sie die Bash-Datei aus dem Azure-Portal**

1. Starten Sie das [Azure-Portal](https://ms.portal.azure.com), und wählen Sie Ihren Cluster aus.
2. Klicken Sie auf **Skriptaktionen**, klicken Sie dann **Neue übermitteln**. Füllen Sie das Formular **Skriptaktion übermitteln** aus, und klicken Sie dann auf die Schaltfläche **Erstellen**.
    
    + **Skripttyp**: Wählen Sie **Benutzerdefiniert** aus.
    + **Name**: Geben Sie einen Skriptnamen an.
    + **Bash-Skript-URI**: Laden Sie die Bash-Datei in den privaten Cluster hoch, und kopieren Sie dann hier die URL. Verwenden Sie alternativ den angegebenen URI.
    
   ```upgrade_spark_enhancement
    https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh
   ```

    + Aktivieren Sie **Hauptknoten** und **Worker**.
    + **Parameter**: Legen Sie die Parameter nach Bash-Verwendung fest.

    ![Protokoll hochladen oder Hotfix-Aktualisierung](./media/apache-azure-spark-history-server/sparkui-upload2.png)


## <a name="known-issue"></a>Bekannte Probleme

1.  Gegenwärtig können nur Spark 2.3-Cluster verwendet werden.

2.  Ein-/Ausgabedaten über RDD werden auf der Registerkarte „Daten“ nicht angezeigt.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Ressourcen für einen Spark-Cluster unter HDInsight](apache-spark-resource-manager.md)
* [Konfigurieren von Spark-Einstellungen](apache-spark-settings.md)


## <a name="contact-us"></a>Kontakt

Falls Sie uns Feedback geben möchten oder bei der Verwendung des Tools andere Probleme auftreten, können Sie uns eine E-Mail an [hdivstool@microsoft.com](mailto:hdivstool@microsoft.com) senden.