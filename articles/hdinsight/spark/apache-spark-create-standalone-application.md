---
title: 'Tutorial: Erstellen einer Scala Maven-Anwendung für Spark in Azure HDInsight mithilfe von IntelliJ'
description: Erstellen Sie eine Spark-Anwendung, die in Scala mit Apache Maven als Buildsystem mit einem vorhandenen von IntelliJ IDEA bereitgestellten Maven-Archetyp für Scala geschrieben ist.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/07/2018
ms.openlocfilehash: 1583da76ee1a60ad3ba8a946c1d49c2d5567e43f
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621469"
---
# <a name="tutorial-create-a-scala-maven-application-for-spark-in-hdinsight-using-intellij"></a>Tutorial: Erstellen einer Scala Maven-Anwendung für Spark in HDInsight mithilfe von IntelliJ

In diesem Tutorial erfahren Sie, wie Sie eine in Scala geschriebene Spark-Anwendung erstellen, die Maven mit IntelliJ IDEA nutzt. Dabei wird Apache Maven als Buildsystem verwendet. Ausgangspunkt ist ein vorhandener, von IntelliJ IDEA bereitgestellter Maven-Archetyp für Scala.  Das Erstellen einer Scala-Anwendung in IntelliJ IDEA umfasst die folgenden Schritte:

* Verwenden von Maven als Buildsystem
* Aktualisieren der Projektobjektmodell-Datei (POM-Datei) zum Auflösen von Spark-Modulabhängigkeiten
* Schreiben der Anwendung in Scala
* Generieren einer JAR-Datei, die an HDInsight Spark-Cluster übermittelt werden kann
* Ausführen der Anwendung in einem Spark-Cluster mithilfe von Livy

> [!NOTE]
> HDInsight stellt auch ein IntelliJ IDEA-Plug-In bereit, um die Erstellung und Übermittlung von Anwendungen an einen HDInsight Spark-Cluster unter Linux zu vereinfachen. Weitere Informationen finden Sie unter [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen von Spark Scala-Anwendungen für HDInsight Spark-Cluster unter Linux (Vorschau)](apache-spark-intellij-tool-plugin.md).
> 

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Verwenden von IntelliJ zum Entwickeln einer Scala Maven-Anwendung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* Oracle Java Development Kit. Das Installationspaket finden Sie [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Eine Java-IDE. In diesem Artikel wird IntelliJ IDEA 18.1.1 verwendet. Das Installationspaket finden Sie [hier](https://www.jetbrains.com/idea/download/).

## <a name="use-intellij-to-create-application"></a>Erstellen der Anwendung mit IntelliJ

1. Starten Sie IntelliJ IDEA, und erstellen Sie ein Projekt. Führen Sie im Dialogfeld **Neues Projekt** folgende Schritte aus: 

   a. Wählen Sie **HDInsight** > **Spark auf HDInsight (Scala)** aus.

   b. Wählen Sie in der Liste **Build-Tool** je nach Ihren Anforderungen einen der folgenden Einträge aus:

      * **Maven** für die Unterstützung des Scala-Projekterstellungs-Assistenten
      * **SBT** zum Verwalten von Abhängigkeiten und Erstellen für das Scala-Projekt

   ![Dialogfeld „Neues Projekt“](./media/apache-spark-create-standalone-application/create-hdi-scala-app.png)

1. Klicken Sie auf **Weiter**.

1. Der Scala-Projekterstellungs-Assistent erkennt automatisch, ob das Scala-Plug-In installiert ist. Wählen Sie **Installieren** aus.

   ![Scala-Plug-In-Prüfung](./media/apache-spark-create-standalone-application/Scala-Plugin-check-Reminder.PNG) 

1. Wählen Sie **OK** aus, um das Scala-Plug-In herunterzuladen. Befolgen Sie die Anweisungen zum Neustart von IntelliJ. 

   ![Das Installationsdialogfeld für das Scala Plug-In](./media/apache-spark-create-standalone-application/Choose-Scala-Plugin.PNG)

1. Führen Sie im Fenster **Neues Projekt** folgende Schritte aus:  

    ![Auswählen des Spark-SDK](./media/apache-spark-create-standalone-application/hdi-new-project.png)

   a. Geben Sie einen Projektnamen und einen Speicherort ein.

   b. Wählen Sie in der Dropdownliste **Projekt-SDK** eine **Java 1.8** für Spark 2.x-Cluster bzw. **Java 1.7** für Spark 1.x-Cluster aus.

   c. In der Dropdownliste **Spark-Version** fügt der Scala-Projekterstellungs-Assistent die richtige Version von Spark-SDK und Scala-SDK ein. Wenn die Spark-Clusterversion niedriger als 2.0 ist, wählen Sie **Spark 1.x** aus. Wählen Sie andernfalls **Spark 2.x** aus. In diesem Beispiel wird **Spark 2.0.2 (Scala 2.11.8)** verwendet.

1. Wählen Sie **Fertig stellen** aus.

## <a name="install-scala-plugin-for-intellij-idea"></a>Installieren des Scala-Plug-Ins für IntelliJ IDEA
Installieren Sie das Scala-Plug-In in folgenden Schritten:

1. Öffnen Sie IntelliJ IDEA.
1. Wählen Sie auf dem Begrüßungsbildschirm **Configure (Konfigurieren)** und dann **Plugins (Plug-Ins)** aus.
   
    ![Scala-Plug-In aktivieren](./media/apache-spark-create-standalone-application/enable-scala-plugin.png)
1. Wählen Sie im nächsten Bildschirm links unten **Install JetBrains plugin** (JetBrains-Plug-In installieren) aus. 
1. Suchen Sie im Dialogfeld **Browse JetBrains Plugins (JetBrains-Plug-Ins durchsuchen)** nach **Scala**, und wählen Sie anschließend **Install (Installieren)** aus.
   
    ![Scala-Plug-In installieren](./media/apache-spark-create-standalone-application/install-scala-plugin.png)
1. Nach erfolgreicher Plug-In-Installation müssen Sie die IDE neu starten.

## <a name="create-a-standalone-scala-project"></a>Erstellen eines eigenständigen Scala-Projekts
1. Öffnen Sie IntelliJ IDEA.
1. Wählen Sie im Menü **Datei** zum Erstellen eines neuen Projekts **Neu > Projekt** aus.
1. Wählen Sie im Dialogfeld für das neue Projekt Folgendes aus:
   
    ![Maven-Projekt erstellen](./media/apache-spark-create-standalone-application/create-maven-project.png)
   
   * Wählen Sie den Projekttyp **Maven** aus.
   * Geben Sie ein **Projekt-SDK**an. Klicken Sie auf **Neu**, und navigieren Sie zum Installationsverzeichnis von Java, üblicherweise `C:\Program Files\Java\jdk1.8.0_66`.
   * Aktivieren Sie das Kontrollkästchen **Archetypbasierte Erstellung** .
   * Wählen Sie in der Liste mit den Archetypen die Option **org.scala-tools.archetypes:scala-archetype-simple**aus. Dieser Archetyp erstellt die passende Verzeichnisstruktur und lädt die erforderlichen Standardabhängigkeiten zum Schreiben des Scala-Programms herunter.
1. Klicken Sie auf **Weiter**.
1. Geben Sie passende Werte für **GroupId**, **ArtifactId** und **Version** an. In diesem Tutorial werden die folgenden Werte verwendet:

    - GroupId: com.microsoft.spark.example
    - ArtifactId: SparkSimpleApp
1. Klicken Sie auf **Weiter**.
1. Überprüfen Sie die Einstellungen, und wählen Sie **Weiter** aus.
1. Überprüfen Sie den Projektnamen und einen Speicherort, und wählen Sie anschließend **Fertig stellen** aus.
1. Wählen Sie im linken Bereich **src > test > scala > com > microsoft > spark > example** aus, klicken Sie mit der rechten Maustaste auf **MySpec**, und wählen Sie dann **Delete (Löschen)** aus. Sie benötigen diese Datei nicht für die Anwendung.
  
1. In den folgenden Schritten wird die Datei „pom.xml“ aktualisiert, um die Abhängigkeiten für die Spark Scala-Anwendung zu definieren. Damit diese Abhängigkeiten automatisch heruntergeladen und aufgelöst werden, muss Maven entsprechend konfiguriert sein.
   
    ![Maven für automatische Downloads konfigurieren](./media/apache-spark-create-standalone-application/configure-maven.png)
   
   1. Wählen Sie im Menü **Datei** die Option **Einstellungen**.
   1. Navigieren Sie im Dialogfeld **Einstellungen** zu **Erstellung, Ausführung, Bereitstellung** > **Buildtools** > **Maven** > **Importieren**.
   1. Aktivieren Sie das Kontrollkästchen **Maven-Projekte automatisch importieren**.
   1. Klicken Sie auf **Apply** (Anwenden) und dann auf **OK**.
1. Wählen Sie im linken Bereich **src > main > scala > com.microsoft.spark.example** aus, und doppelklicken Sie dann auf **App**, um „App.scala“ zu öffnen.

1. Ersetzen Sie den vorhandenen Beispielcode durch den folgenden Code, und speichern Sie die Änderungen. Dieser Code liest die Daten aus der Datei „HVAC.csv“ (für alle HDInsight Spark-Cluster verfügbar), ruft die Zeilen ab, die nur eine Ziffer in der sechsten Spalte enthalten, und schreibt die Ausgabe in **/HVACOut** unter dem Standardspeichercontainer für den Cluster.

        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO to wasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows which have only one digit in the 7th column in the CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
1. Doppelklicken Sie im linken Bereich auf **pom.xml**.
   
   1. Fügen Sie in `<project>\<properties>` folgende Segmente hinzu:
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   1. Fügen Sie in `<project>\<dependencies>` folgende Segmente hinzu:
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      Speichern Sie die Änderungen in „pom.xml“.
1. Erstellen Sie die JAR-Datei. IntelliJ IDEA ermöglicht die Erstellung von JAR als Projektartefakt. Führen Sie die folgenden Schritte aus:
    
    1. Wählen Sie im Menü **File** (Datei) die Option **Project Structure** (Projektstruktur) aus.
    1. Wählen Sie im Dialogfeld **Project Structure (Projektstruktur)** die Option **Artifacts (Artefakte)** und anschließend das Pluszeichen aus. Wählen Sie im Popupdialogfeld **JAR** und anschließend **From modules with dependencies (Aus Modulen mit Abhängigkeiten)** aus.
       
        ![Erstellen einer JAR-Datei](./media/apache-spark-create-standalone-application/create-jar-1.png)
    1. Wählen Sie im Dialogfeld **Create JAR from Modules (JAR aus Modulen erstellen)** die Auslassungspunkte (![...](./media/apache-spark-create-standalone-application/ellipsis.png)) für die **Main Class (Hauptklasse)** aus.
    1. Wählen Sie im Dialogfeld **Select Main Class (Hauptklasse auswählen)** die standardmäßig angezeigte Klasse und anschließend **OK** aus.
       
        ![Erstellen einer JAR-Datei](./media/apache-spark-create-standalone-application/create-jar-2.png)
    1. Stellen Sie im Dialogfeld **Create JAR from Modules (JAR aus Modulen erstellen)** sicher, dass die Option zum **Extrahieren in die JAR-Zieldatei** aktiviert ist, und wählen Sie anschließend **OK** aus.  Mit dieser Einstellung wird eine einzelne JAR-Datei mit allen Abhängigkeiten erstellt.
       
        ![Erstellen einer JAR-Datei](./media/apache-spark-create-standalone-application/create-jar-3.png)
    1. Die Ausgabelayout-Registerkarte führt alle JAR-Dateien des Maven-Projekts auf. Sie können die Dateien auswählen und löschen, zu denen die Scala-Anwendung keine direkte Abhängigkeit hat. Bei der hier erstellten Anwendung können Sie alle bis auf die letzte (**SparkSimpleApp-Kompilierungsausgabe**) entfernen. Wählen Sie die zu löschenden JAR-Dateien und anschließend das Symbol **Löschen** aus.
       
        ![Erstellen einer JAR-Datei](./media/apache-spark-create-standalone-application/delete-output-jars.png)
       
        Vergewissern Sie sich, dass das Kontrollkästchen **Include in project build (In Projektbuild einbeziehen)** aktiviert ist, damit die JAR-Datei bei jeder Projekterstellung oder -aktualisierung erstellt wird. Wählen Sie **Übernehmen** und dann **OK** aus.
    1. Wählen Sie im Menü **Build (Erstellen)** die Option **Build Artifacts (Artefakte erstellen)** aus, um die JAR-Datei zu erstellen. Die JAR-Ausgabe wird unter **\out\artifacts** erstellt.
       
        ![Erstellen einer JAR-Datei](./media/apache-spark-create-standalone-application/output.png)

## <a name="run-the-application-on-the-spark-cluster"></a>Ausführen der Anwendung im Spark-Cluster
Sie können die folgenden Ansätze nutzen, um die Anwendung im Cluster auszuführen:

* **Kopieren Sie die JAR-Anwendungsdatei in das dem Cluster zugeordnete Azure-Speicher-BLOB**. Hierzu können Sie das Befehlszeilenprogramm [**AzCopy**](../../storage/common/storage-use-azcopy.md) verwenden. Daneben gibt es aber auch noch zahlreiche andere Clients, die Sie zum Hochladen von Daten verwenden können. Weitere Informationen finden Sie unter [Hochladen von Daten für Hadoop-Aufträge in HDInsight](../hdinsight-upload-data.md).
* **Verwenden Sie Livy, um einen Anwendungsauftrag remote an den Spark-Cluster zu übermitteln**. Spark-Cluster in HDInsight enthalten Livy, um REST-Endpunkte für die Remoteübermittlung von Spark-Aufträgen verfügbar zu machen. Weitere Informationen finden Sie unter [Remoteübermittlung von Spark-Aufträgen unter Verwendung von Livy mit Spark-Clustern in HDInsight](apache-spark-livy-rest-interface.md).

## <a name="next-step"></a>Nächster Schritt

In diesem Artikel haben Sie gelernt, wie eine Spark Scala-Anwendung erstellt wird. Im nächsten Artikel erfahren Sie, wie Sie diese Anwendung in einem HDInsight Spark-Cluster mit Livy ausführen.

> [!div class="nextstepaction"]
>[Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](./apache-spark-livy-rest-interface.md)

