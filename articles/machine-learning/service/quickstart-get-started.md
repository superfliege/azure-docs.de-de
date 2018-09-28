---
title: 'Schnellstart: Erstellen eines Machine Learning-Arbeitsbereichs – Azure'
description: Verwenden Sie das Azure-Portal zum Erstellen eines Azure Machine Learning-Arbeitsbereichs. Dieser Arbeitsbereich bildet die Grundlage in der Cloud für das Experimentieren, Trainieren und Bereitstellen von Modellen für maschinelles Lernen mit dem Azure Machine Learning-Dienst.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: rastala
ms.author: roastala
ms.date: 09/24/2018
ms.openlocfilehash: c1610291e06255e2c724268f63d740f7e4debea4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46959987"
---
# <a name="quickstart-get-started-with-azure-machine-learning-service"></a>Schnellstart: erste Schritte mit dem Azure Machine Learning-Dienst

In diesem Schnellstart verwenden Sie das Azure-Portal zum Erstellen eines Azure Machine Learning-Arbeitsbereichs. Dieser Arbeitsbereich bildet die Grundlage in der Cloud für das Experimentieren, Trainieren und Bereitstellen von Modellen für maschinelles Lernen mit dem Azure Machine Learning-Dienst. 

In diesem Lernprogramm lernen Sie Folgendes:

* Erstellen eines Arbeitsbereichs in Ihrem Azure-Abonnement
* Testen mit Python in einem Azure-Notebook und Protokollieren der Werte in den unterschiedlichen Iterationen
* Anzeigen der protokollierten Werte in Ihrem Arbeitsbereich

Der Einfachheit halber werden die folgenden Azure-Ressourcen bei regionaler Verfügbarkeit Ihrem Arbeitsbereich automatisch hinzugefügt: [Containerregistrierung](https://azure.microsoft.com/services/container-registry/), [Storage](https://azure.microsoft.com/services/storage/), [Application Insights](https://azure.microsoft.com/services/application-insights/) und [Key Vault](https://azure.microsoft.com/services/key-vault/).

Die von Ihnen erstellten Ressourcen können ggf. auch in anderen Azure Machine Learning-Tutorials und -Anleitungen verwendet werden. Ebenso wie bei anderen Azure-Diensten gelten bei bestimmten Ressourcen (z.B. BatchAI-Clustergröße) in Verbindung mit dem Azure Machine Learning-Dienst Einschränkungen. Lesen Sie [diesen](how-to-manage-quotas.md) Artikel zu den Standardgrenzwerten und zum Anfordern zusätzlicher Kontingente.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

Klicken Sie auf der Seite des Arbeitsbereichs auf `Explore your Azure Machine Learning Workspace`.

 ![Erkunden des Arbeitsbereichs](./media/quickstart-get-started/explore_aml.png)


## <a name="use-the-workspace"></a>Verwenden des Arbeitsbereichs

Sehen Sie sich nun an, wie ein Arbeitsbereich beim Verwalten Ihrer Machine Learning-Skripts hilft. In diesem Abschnitt führen Sie folgende Schritte aus:

* Öffnen eines Notebooks in Azure Notebooks
* Ausführen von Code zum Generieren von protokollierten Werten
* Anzeigen der protokollierten Werte in Ihrem Arbeitsbereich

Dies ist ein Beispiel dafür, wie der Arbeitsbereich Ihnen beim Nachverfolgen der in einem Skript generierten Informationen helfen kann. 

### <a name="open-a-notebook"></a>Öffnen eines Notebooks 

Azure Notebooks bietet eine kostenlose Cloudplattform für Jupyter Notebooks, die mit allem vorkonfiguriert ist, was Sie für die Ausführung des Azure Machine Learning-Diensts benötigen.  

Klicken Sie auf die Schaltfläche `Open Azure Notebooks`, um Ihr erstes Experiment zu testen.

 ![Starten von Azure Notebooks](./media/quickstart-get-started/explore_ws.png)

Nach Ihrer Anmeldung wird eine neue Registerkarte geöffnet und die Eingabeaufforderung `Clone Library` angezeigt.  Klicken Sie auf `Clone`.


### <a name="run-the-notebook"></a>Ausführen des Notebooks

Neben zwei Notebooks sehen Sie eine Datei `config.json`.  Diese Konfigurationsdatei enthält Informationen zu dem Arbeitsbereich, den Sie gerade erstellt haben.  

Klicken Sie auf `01.run-experiment.ipynb`, um das Notebook zu öffnen.

Sie können die Zellen nacheinander ausführen, indem Sie `Shift`+`Enter` verwenden.  Oder verwenden Sie das Menü `Cells` > `Run All`, um das gesamte Notebook auszuführen.

Sie werden möglicherweise aufgefordert, sich anzumelden.  Kopieren Sie den Code in der Meldung, klicken Sie dann auf den Link, und fügen Sie den Code im neuen Fenster ein.  Achten Sie darauf, dass Sie kein Leerzeichen vor oder hinter dem Code kopieren.

 ![Anmeldung](./media/quickstart-get-started/login.png)

Im Notebook liest die zweite Zelle aus `config.json`, um eine Verbindung mit Ihrem Arbeitsbereich herzustellen.
```
ws = Workspace.from_config()
```

Die dritte Zelle im Code startet ein Experiment mit dem Namen „my-first-experiment“.  Sie verwenden diesen Namen, um Informationen zur Ausführung in Ihrem Arbeitsbereich zu suchen.

```
experiment = Experiment(workspace_object=ws, name = "my-first-experiment")
```

Beachten Sie die Werte in der letzten Zelle des Notebooks, die in eine Protokolldatei geschrieben werden.

```
# Log final results
run.log("Final estimate: ",pi_estimate)
run.log("Final error: ",math.pi-pi_estimate)
```

Sie können diese Werte in Ihrem Arbeitsbereich anzeigen, nachdem der Code ausgeführt wurde.

## <a name="view-logged-values"></a>Anzeigen protokollierter Werte

Nachdem alle Zellen im Notebook fertiggestellt wurden, wechseln Sie zurück zur Portalseite.  

Klicken Sie auf `View Experiments`.

![Anzeigen von Experimenten](./media/quickstart-get-started/view_exp.png)

Schließen Sie das Popupelement `Reports`.

Klicken Sie auf `my-first-experiment`.

Sehen Sie sich die Informationen zu der Ausführung, die Sie gerade durchgeführt haben, an.  Scrollen Sie auf der Seite nach unten, um die Tabelle mit den Ausführungen anzuzeigen, und klicken auf den Link der Ausführungsnummer.

 ![Liste „Ausführungsverlauf“](./media/quickstart-get-started/report.png)

Daraufhin werden Plots angezeigt, die automatisch aus den protokollierten Werte erstellt wurden:

   ![Anzeigen des Verlaufs](./media/quickstart-get-started/plots.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Sie können auch die Ressourcengruppe beibehalten, aber einen einzelnen Arbeitsbereich löschen, indem Sie die Eigenschaften des Arbeitsbereichs anzeigen und die Schaltfläche „Löschen“ auswählen.

## <a name="next-steps"></a>Nächste Schritte

Sie haben damit die erforderlichen Ressourcen erstellt, um mit dem Experimentieren und Bereitstellen von Modellen zu beginnen. Außerdem haben Sie Code in einem Notebook ausgeführt und den Ausführungsverlauf dieses Codes in Ihrem Arbeitsbereich in der Cloud untersucht.

Führen Sie für ausführliche Workflows die Tutorials für Azure Machine Learning zum Trainieren und Bereitstellen eines Modells aus.  

> [!div class="nextstepaction"]
> [Tutorial: Trainieren eines Imageklassifizierungsmodells](tutorial-train-models-with-aml.md)
