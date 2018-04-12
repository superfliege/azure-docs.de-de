---
title: Ausführen eines Databricks-Notebooks mit der Databricks-Notebook-Aktivität in Azure Data Factory
description: Hier erfahren Sie, wie Sie die Databricks-Notebook-Aktivität in einer Azure Data Factory-Instanz verwenden, um ein Databricks-Notebook für den Cluster mit den Databricks-Aufträgen auszuführen.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/12/2018
ms.author: abnarain
ms.reviewer: douglasl
ms.openlocfilehash: edd3bbb5d47753890328ae86b15dc938dfcf7eb7
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="run-a-databricks-notebook-with-the-databricks-notebook-activity-in-azure-data-factory"></a>Ausführen eines Databricks-Notebooks mit der Databricks-Notebook-Aktivität in Azure Data Factory

In diesem Tutorial verwenden Sie das Azure-Portal, um eine Azure Data Factory-Pipeline zu erstellen, die ein Databricks-Notebook für den Cluster mit den Databricks-Aufträgen ausführt. Außerdem übergibt sie während der Ausführung Azure Data Factory-Parameter an das Databricks-Notebook.

In diesem Tutorial führen Sie die folgenden Schritte aus:

  - Erstellen einer Data Factory.

  - Erstellen einer Pipeline mit Databricks-Notebook-Aktivität

  - Auslösen einer Pipelineausführung

  - Überwachen der Pipelineausführung.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

  - **Azure Databricks-Arbeitsbereich** [Erstellen Sie einen Databricks-Arbeitsbereich](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal), oder verwenden Sie einen vorhandenen Arbeitsbereich. Sie erstellen ein Python-Notebook in Ihrem Azure Databricks-Arbeitsbereich. Anschließend führen Sie das Notebook aus und übergeben Parameter mit Azure Data Factory.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1.  Starten Sie den Webbrowser **Microsoft Edge** oder **Google Chrome**. Die Data Factory-Benutzeroberfläche wird zurzeit nur in den Webbrowsern Microsoft Edge und Google Chrome unterstützt.

2.  Klicken Sie im linken Menü auf **Neu** und anschließend auf **Daten + Analysen** und **Data Factory**.

    ![Erstellen einer neuen Data Factory](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image1.png)

3.  Geben Sie im Bereich **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein.

    Der Name der Azure Data Factory muss *global eindeutig*sein. Ändern Sie den Namen der Data Factory, wenn die folgende Fehlermeldung angezeigt wird. (Verwenden Sie beispielsweise **\<IhrName\>ADFTutorialDataFactory**.) Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](https://docs.microsoft.com/en-us/azure/data-factory/naming-rules).

    ![Angeben eines Namens für die neue Data Factory-Instanz](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image2.png)

4.  Wählen Sie unter **Abonnement** Ihr Azure-Abonnement aus, in dem die Data Factory erstellt werden soll.

5.  Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:
    
    - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe.
    
    - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.

    Bei einigen Schritten dieses Tutorials wird davon ausgegangen, dass Sie eine Ressourcengruppe mit dem Namen **ADFTutorialResourceGroup** verwenden. Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).

1.  Wählen Sie **V2 (Vorschau)** als **Version** aus.

2.  Wählen Sie unter **Standort** den Standort für die Data Factory aus.

    Derzeit können Sie mit Data Factory V2 nur in den Regionen „USA, Osten“, „USA, Osten 2“ und „Europa, Westen“ Data Factorys erstellen. Die von Data Factory verwendeten Datenspeicher (etwa Azure Storage und Azure SQL-Datenbank) und Computedienste (etwa HDInsight) können sich in anderen Regionen befinden.

3.  Wählen Sie die Option **An Dashboard anheften** aus.

4.  Klicken Sie auf **Erstellen**.

5.  Auf dem Dashboard sehen Sie die folgende Kachel mit dem Status **Deploying Data Factory** (Data Factory wird bereitgestellt...):

    ![](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image3.png)

6.  Nach Abschluss der Erstellung wird die Seite **Data Factory** angezeigt. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Anwendung für die Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu starten.

    ![Starten der Data Factory-UI-Anwendung](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image4.png)

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten

In diesem Abschnitt erstellen Sie einen verknüpften Databricks-Dienst. Dieser verknüpfte Dienst enthält die Verbindungsinformationen für den Databricks-Cluster:

### <a name="create-an-azure-databricks-linked-service"></a>Erstellen eines verknüpften Azure Databricks-Diensts

1.  Wechseln Sie auf der Seite **Erste Schritte** im linken Bereich zur Registerkarte **Bearbeiten**.

    ![Bearbeiten des neuen verknüpften Diensts](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image5.png)

2.  Wählen Sie unten im Fenster die Option **Verbindungen** und dann **+ Neu**.
    
    ![Erstellen einer neuen Verbindung](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image6.png)

3.  Wählen Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) die Option **Datenspeicher**\>**Azure Databricks** und dann **Weiter**.
    
    ![Angeben eines verknüpften Databricks-Diensts](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image7.png)

4.  Führen Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) die folgenden Schritte aus:
    
    1.  Geben Sie unter **Name** den Namen ***AzureDatabricks\_LinkedService*** ein.
    
    2.  Wählen Sie unter **Cluster** die Option **Neuer Cluster**.
    
    3.  Wählen Sie unter **Domäne/Region** die Region aus, in der sich Ihr Azure Databricks-Arbeitsbereich befindet.
    
    4.  Wählen Sie unter **Cluster node type** (Clusterknotentyp) für dieses Tutorial die Option **Standard\_D3\_v2**.
    
    5.  Führen Sie für **Zugriffstoken** die Generierung über den Azure Databricks-Arbeitsbereich durch. Die Schritte finden Sie [hier](https://docs.databricks.com/api/latest/authentication.html#generate-token).
    
    6.  Wählen Sie unter **Clusterversion,** die Option **4.0 Beta** (aktuelle Version).
    
    7.  Geben Sie unter **Anzahl von Workerknoten** den Wert **2** ein.
    
    8.  Wählen Sie **Fertig stellen**.

        ![Fertigstellen der Erstellung des verknüpften Diensts](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image8.png)

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

1.  Klicken Sie auf die Schaltfläche **+** (Pluszeichen), und wählen Sie im Menü dann die Option **Pipeline**.

    ![Schaltflächen zum Erstellen einer neuen Pipeline](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image9.png)

2.  Erstellen Sie einen **Parameter** für die Verwendung in der **Pipeline**. Diesen Parameter übergeben Sie später an die Databricks-Notebook-Aktivität. Klicken Sie in der leeren Pipeline auf die Registerkarte **Parameter** und dann auf **Neu**, und geben Sie als Namen „**name**“ ein.

    ![Erstellen eines neuen Parameters](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image10.png)

    ![Erstellen des Parameters „name“](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image11.png)

3.  Erweitern Sie in der Toolbox **Aktivitäten** die Option **Databricks**. Ziehen Sie die **Notebook**-Aktivität aus der Toolbox **Aktivitäten** auf die Oberfläche des Pipeline-Designers.

    ![Ziehen des Notebooks auf die Oberfläche des Designers](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image12.png)

4.  Führen Sie in den Eigenschaften im unteren Bereich des Fensters für das **Databricks-****Notebook** die folgenden Schritte aus:

    a. Wechseln Sie zur Registerkarte **Einstellungen**.

    b. Wählen Sie **myAzureDatabricks\_LinkedService** aus (oben erstellt).

    c. Wählen Sie einen **Notebook-Pfad** für Databricks aus. Wir erstellen ein Notebook und geben den Pfad hier an. Sie erhalten den Notebook-Pfad, indem Sie die nächsten Schritte ausführen.

       1. Starten Ihres Azure Databricks-Arbeitsbereichs

       2. Erstellen Sie im Arbeitsbereich unter **Neuer Ordner** einen neuen Ordner, und geben Sie ihm den Namen **adftutorial**.

          ![Erstellen eines neuen Ordners](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image13.png)

       3. [Erstellen Sie ein neues Notebook](https://docs.databricks.com/user-guide/notebooks/index.html#creating-a-notebook) (Python) namens **mynotebook** im Ordner **adftutorial**,**** und klicken Sie anschließend auf **Erstellen**.

          ![Erstellen eines neuen Notebooks](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image14.png)

          ![Festlegen der Eigenschaften des neuen Notebooks](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image15.png)

       4. Fügen Sie im neu erstellten Notebook „mynotebook“ den folgenden Code hinzu:

           ```
           # Creating widgets for leveraging parameters, and printing the parameters

           dbutils.widgets.text("input", "","")
           dbutils.widgets.get("input")
           y = getArgument("input")
           print "Param -\'input':"
           print y
           ```

           ![Erstellen von Widgets für Parameter](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image16.png)

       5. Der **Notebook-Pfad** lautet hier **/adftutorial/mynotebook**.

5.  Wechseln Sie zurück zum **Erstellungstool für die Data Factory-Benutzeroberfläche**. Navigieren Sie unter **Notebook1-Aktivität** zur Registerkarte **Einstellungen**. 
    
    a.  Fügen Sie der Notebook-Aktivität mit **Parameter hinzufügen** einen Parameter hinzu. Verwenden Sie den Parameter, den Sie der **Pipeline** zuvor hinzugefügt haben.

       ![Hinzufügen eines Parameters](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image17.png)

    b.  Geben Sie dem Parameter den Namen **input**, und geben Sie den Wert als Ausdruck **@pipeline().parameters.name** an.

6.  Wählen Sie zum Überprüfen der Pipeline in der Symbolleiste die Schaltfläche **Überprüfen**. Wählen Sie die Schaltfläche **\>\>** (Pfeil nach rechts), um das Überprüfungsfenster zu schließen.

    ![Überprüfen der Pipeline](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image18.png)

7.  Wählen Sie **Alle veröffentlichen**. Die Data Factory-Benutzeroberfläche veröffentlicht Entitäten (verknüpfte Dienste und Pipeline) für den Azure Data Factory-Dienst.

    ![Veröffentlichen der neuen Data Factory-Entitäten](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image19.png)

## <a name="trigger-a-pipeline-run"></a>Auslösen einer Pipelineausführung

Wählen Sie in der Symbolleiste die Option **Trigger** und dann **Trigger Now** (Jetzt auslösen).

![Wählen des Befehls „Trigger Now“ (Jetzt auslösen)](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image20.png)

Im Dialogfeld **Pipeline Run** (Pipelineausführung) wird nach dem Parameter **name** gefragt. Verwenden Sie hier **/path/filename** als Parameter. Klicken Sie auf **Fertig stellen**.

![Angeben eines Werts für name-Parameter](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image21.png)

## <a name="monitor-the-pipeline-run"></a>Überwachen der Pipelineausführung

1.  Wechseln Sie zur Registerkarte **Überwachen**. Vergewissern Sie sich, dass eine Pipelineausführung angezeigt wird. Die Erstellung eines Databricks-Auftragsclusters, in dem das Notebook ausgeführt wird, dauert ca. fünf bis acht Minuten.

    ![Überwachen der Pipeline](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image22.png)

2.  Wählen Sie von Zeit zu Zeit die Option **Aktualisieren**, um den Status der Pipelineausführung zu überprüfen.

3.  Wenn Sie mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzeigen möchten, wählen Sie in der Spalte **Aktionen** die Optionen **View Activity Runs** (Aktivitätsausführungen anzeigen).

    ![Anzeigen der Aktivitätsausführungen](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image23.png)

Durch Wählen des Links **Pipelines** im oberen Bereich können Sie zur Ansicht mit den Pipelineausführungen zurückkehren.

## <a name="verify-the-output"></a>Überprüfen der Ausgabe

Sie können sich im **Azure Databricks-Arbeitsbereich** anmelden und zu **Cluster** navigieren. Unter **Auftrag** wird der Status *„Ausstehende Ausführung“, „Wird ausgeführt“ oder „Beendet“* angezeigt.

![Anzeigen des Auftragsclusters und des Auftrags](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image24.png)

Sie können auf den **Auftragsnamen** klicken und weitere Details anzeigen. Bei einer erfolgreichen Ausführung können Sie die übergebenen Parameter und die Ausgabe des Python-Notebooks überprüfen.

![Anzeigen der Ausführungsdetails und der Ausgabe](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image25.png)

## <a name="next-steps"></a>Nächste Schritte

Die Pipeline in diesem Beispiel löst eine Databricks-Notebook-Aktivität aus und übergibt einen Parameter. Es wurde Folgendes vermittelt:

  - Erstellen einer Data Factory.

  - Erstellen einer Pipeline, für die die Databricks-Notebook-Aktivität verwendet wird

  - Auslösen einer Pipelineausführung

  - Überwachen der Pipelineausführung.
