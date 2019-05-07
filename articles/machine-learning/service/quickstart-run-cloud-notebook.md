---
title: 'Schnellstart: Ausführen eines Notebooks in der Cloud'
titleSuffix: Azure Machine Learning service
description: Erste Schritte mit dem Azure Machine Learning Service. Verwenden Sie einen verwalteten Notebook-Server in der Cloud, um Ihren Arbeitsbereich auszuprobieren.  Ihr Arbeitsbereich bildet die Grundlage in der Cloud zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 3d4127226037bf28ba677a49f6444ca987118cb9
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65023927"
---
# <a name="quickstart-use-a-cloud-based-notebook-server-to-get-started-with-azure-machine-learning"></a>Schnellstart: Verwenden eines cloudbasierten Notebook-Servers für die ersten Schritte mit Azure Machine Learning

Erstellen Sie einen cloudbasierten Notebook-Server, und verwenden Sie ihn anschließend.  In diesem Schnellstart führen Sie Python-Code aus, der Werte im [Arbeitsbereich von Azure Machine Learning Service](concept-azure-machine-learning-architecture.md) protokolliert. Der Arbeitsbereich bildet die Grundlage in der Cloud zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen mit Machine Learning. 

Dieser Schnellstart zeigt, wie Sie eine Cloudressource in Ihrem Azure Machine Learning-Arbeitsbereich erstellen, die mit der für die Ausführung von Azure Machine Learning erforderlichen Python-Umgebung konfiguriert ist. Wenn Sie stattdessen Ihre eigene Umgebung verwenden möchten, lesen Sie die Informationen unter [Schnellstart: Verwenden Ihres eigenen Notebook-Servers für die ersten Schritte mit Azure Machine Learning](quickstart-run-local-notebook.md).  
 
Diese Schnellstartanleitung umfasst folgende Aktionen:

* Erstellen eines neuen cloudbasierten Notebook-Servers in Ihrem Arbeitsbereich
* Starten der Jupyter-Weboberfläche
* Öffnen eines Notebooks, das Code zum Schätzen des Pi-Werts enthält und Fehler bei jeder Iteration protokolliert.
* Ausführen des Notebooks.
* Anzeigen der protokollierten Fehlerwerte in Ihrem Arbeitsbereich  Dieses Beispiel zeigt, wie der Arbeitsbereich Sie bei der Nachverfolgung der in einem Skript generierten Informationen unterstützt. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Machine Learning-Arbeitsbereich.  [Erstellen Sie Ihren Arbeitsbereich](setup-create-workspace.md#portal) jetzt, wenn noch kein Arbeitsbereich vorhanden ist.

## <a name="create-a-cloud-based-notebook-server"></a>Erstellen eines cloudbasierten Notebook-Servers

 Erstellen Sie in Ihrem Arbeitsbereich eine Cloudressource, um erste Schritte mit Jupyter-Notebooks auszuführen. Diese Ressource bietet Ihnen eine cloudbasierte Plattform, die mit allen Funktionen vorkonfiguriert ist, die Sie für die Ausführung von Azure Machine Learning Service benötigen.

1. Öffnen Sie Ihren Arbeitsbereich im [Azure-Portal](https://portal.azure.com/).  Wenn Sie sich nicht sicher sind, wie Sie Ihren Arbeitsbereich im Portal finden können, lesen Sie [Suchen nach Ihren Arbeitsbereich](how-to-manage-workspace.md#view).

1. Wählen Sie auf der Arbeitsbereichseite im Azure-Portal links **Notebook-VMs** aus.

1. Wählen Sie **+Neu** aus, um eine Notebook-VM zu erstellen.

     ![Auswählen der neuen VM](./media/quickstart-run-cloud-notebook/add-workstation.png)

1. Geben Sie einen Namen für Ihre VM an. Klicken Sie anschließend auf **Erstellen**. 

    ![Erstellen eines neuen virtuellen Computers](media/quickstart-run-cloud-notebook/create-new-workstation.png)

1. Warten Sie ungefähr 4 bis 5 Minuten, und wählen Sie dann **Aktualisieren** aus.  Aktualisieren Sie etwa alle 30 Sekunden, bis der Status **Running** (Wird ausgeführt) anzeigt wird.

    ![Aktualisieren](media/quickstart-run-cloud-notebook/refresh.png)

## <a name="launch-jupyter-web-interface"></a>Starten der Jupyter-Webbenutzeroberfläche

Nachdem Ihre VM ausgeführt wird, verwenden Sie den Abschnitt **Notebook-VMs**, um die Jupyter-Webbenutzeroberfläche zu starten.

1. Wählen Sie **Jupyter** in der Spalte **URI** für Ihre VM aus.  

    ![Starten des Jupyter Notebook-Servers](./media/quickstart-run-cloud-notebook/start-server.png)

    Der Link startet Ihren Notebook-Server und öffnet die Jupyter Notebook-Webseite im Browser auf einer neuen Registerkarte.  Dieser Link funktioniert nur für die Person, die die VM erstellt.

1. Wählen Sie auf der Jupyter Notebook-Webseite den Ordner **samples/quickstart** aus, um das Schnellstart-Notebook anzuzeigen.

## <a name="run-the-notebook"></a>Ausführen des Notebooks

Führen Sie ein Notebook aus, das den Wert von Pi schätzt und den Fehler in Ihrem Arbeitsbereich protokolliert.

1. Wählen Sie **01.run-experiment.ipynb**, um das Notebook zu öffnen.

1. Möglicherweise wird eine Meldung angezeigt, die besagt, dass der Kernel nicht festgelegt wurde.  Wählen Sie **Python 3.6 - AzureML** aus, und wählen Sie dann **Set Kernel** (Kernel festlegen) aus.

   ![Festlegen des Kernels](./media/quickstart-run-cloud-notebook/set-kernel.png)

1. Im Statusbereich werden Sie angewiesen, zu warten, bis der Kernel gestartet wurde. Die Meldung wird nicht mehr angezeigt, wenn der Kernel bereit ist.

    ![Warten, bis der Kernel gestartet wird](./media/quickstart-run-cloud-notebook/wait-for-kernel.png)

1.  Klicken Sie in der ersten Codezelle ein, und wählen Sie **Run** (Ausführen) aus.

    > [!NOTE]
    > Codezellen sind eckige Klammern vorangestellt. Wenn die eckigen Klammern leer sind (__[  ]__), wurde der Code nicht ausgeführt. Während der Code ausgeführt wird, sehen Sie ein Sternchen (__[*]__). Nachdem der Code abgeschlossen wurde, wird eine Zahl **[1]** angezeigt.  Die Zahl informiert Sie über die Reihenfolge, in der die Zellen ausgeführt wurden.
    >
    > Verwenden Sie **UMSCHALT+EINGABETASTE** als Tastenkombination zum Ausführen einer Zelle.

    ![Ausführen der ersten Codezelle](media/quickstart-run-cloud-notebook/cell1.png)

1. Führen Sie die zweite Codezelle aus. Wenn Anweisungen zur Authentifizierung angezeigt werden, kopieren Sie den Code, und folgen Sie dem Link, um sich anzumelden. Nachdem Sie sich anmeldet haben, speichert Ihr Browser diese Einstellung.  

    > [!TIP]
    > Achten Sie darauf, dass Sie das Leerzeichen hinter dem Code nicht kopieren.  

    ![Authentifizieren](media/quickstart-run-cloud-notebook/authenticate.png)

1. Wenn Sie fertig sind, wird die Zellennummer __[2]__ angezeigt.  Wenn Sie sich anmelden mussten, wird eine Statusmeldung zur erfolgreichen Authentifizierung angezeigt.   Wenn Sie sich nicht anmelden mussten, wird keine Ausgabe für diese Zelle angezeigt. Es wird nur die Zellennummer angezeigt, um anzugeben, dass die Zelle erfolgreich ausgeführt wurde.

    ![Erfolgsmeldung](media/quickstart-run-cloud-notebook/success.png)

1. Führen Sie die restlichen Codezellen aus.  Nach der Ausführung jeder Zelle wird ihre Zellennummer angezeigt. Nur die letzte Zelle zeigt eine andere Ausgabe an.  In der größten Codezelle sehen Sie an mehreren Stellen `run.log`. Jedes `run.log` fügt Ihrem Arbeitsbereich seinen Wert hinzu.


## <a name="view-logged-values"></a>Anzeigen protokollierter Werte

1. Die Ausgabe der Zelle `run` enthält einen Link zum Azure-Portal, in dem Sie die Experimentergebnisse in Ihrem Arbeitsbereich anzeigen können. 

    ![Anzeigen von Experimenten](./media/quickstart-run-cloud-notebook/view-exp.png)

1. Klicken Sie auf den **Link zum Azure-Portal**, um Informationen zur Ausführung in Ihrem Arbeitsbereich anzuzeigen.  Über diesen Link wird Ihr Arbeitsbereich im Azure-Portal geöffnet.

1. Die angezeigten Plots protokollierter Werte wurden automatisch im Arbeitsbereich erstellt. Wenn Sie mehrere Werte mit dem gleichen Namensparameter protokollieren, wird für Sie automatisch ein Plot generiert.

   ![Anzeigen des Verlaufs](./media/quickstart-run-cloud-notebook/web-results.png)

Da der Code für die Pi-Annäherung willkürliche Werte verwendet, enthalten Ihre Plots andere Werte.  

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

### <a name="stop-the-notebook-vm"></a>Beenden der Notebook-VM

Beenden Sie die Notebook-VM, wenn Sie sie nicht mehr verwenden, um Kosten zu sparen.  

1. Wählen Sie in Ihrem Arbeitsbereich **Notebook-VMs** aus.

   ![Beenden des VM-Servers](./media/quickstart-run-cloud-notebook/stop-server.png)

1. Wählen Sie den virtuellen Computer in der Liste aus.

1. Wählen Sie **Stop** (Beenden) aus.

1. Wenn Sie erneut bereit sind, den Server zu verwenden, wählen Sie **Start** aus.

### <a name="delete-everything"></a>Alles löschen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Sie können die Ressourcengruppe auch behalten und einen einzelnen Arbeitsbereich löschen. Zeigen Sie die Eigenschaften des Arbeitsbereichs an, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstart haben Sie die folgenden Aufgaben erledigt:

* Erstellen einer Notebook-VM
* Starten eines Jupyter Notebook-Servers auf Ihrer Notebook-VM
* Öffnen eines Notebooks, das Code zum Schätzen des Pi-Werts enthält und Fehler bei jeder Iteration protokolliert.
* Ausführen des Notebooks.
* Anzeigen der protokollierten Fehlerwerte in Ihrem Arbeitsbereich  Dieses Beispiel zeigt, wie der Arbeitsbereich Sie bei der Nachverfolgung der in einem Skript generierten Informationen unterstützt. 

Führen Sie Machine Learning-Tutorials zum Trainieren und Bereitstellen eines Modells aus, um sich ausführlicher mit dem Workflow zu beschäftigen:  

> [!div class="nextstepaction"]
> [Tutorial: Trainieren eines Bildklassifizierungsmodells](tutorial-train-models-with-aml.md)