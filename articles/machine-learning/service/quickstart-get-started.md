---
title: Schnellstart über das Azure-Portal
titleSuffix: Azure Machine Learning service
description: Erste Schritte mit dem Azure Machine Learning Service. Verwenden Sie das Azure-Portal, um einen Arbeitsbereich zu erstellen. Dieser bildet in der Cloud die Grundlage zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 01/18/2019
ms.custom: seodec18
ms.openlocfilehash: e3569f560224ab8c9a64ababb2fcea7e96e87367
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54812453"
---
# <a name="quickstart-use-the-azure-portal-to-get-started-with-azure-machine-learning"></a>Schnellstart: Verwenden des Azure-Portals zum Ausführen der ersten Schritte mit Azure Machine Learning

Verwenden Sie das Azure-Portal zum Erstellen eines Azure Machine Learning-Arbeitsbereichs. Dieser Arbeitsbereich bildet die Grundlage in der Cloud zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen mit Machine Learning. In dieser Schnellstartanleitung werden Cloudressourcen genutzt, und es ist keine Installation erforderlich. Wie Sie stattdessen Ihren eigenen Jupyter Notebook-Server konfigurieren, erfahren Sie unter [Schnellstart: Verwenden von Python für die ersten Schritte mit Azure Machine Learning](quickstart-create-workspace-with-python.md).  
 
Diese Schnellstartanleitung umfasst folgende Aktionen:

* Erstellen eines Arbeitsbereichs in Ihrem Azure-Abonnement.
* Testen mit Python in einem Jupyter Notebook und Protokollieren der Werte in den unterschiedlichen Iterationen
* Anzeigen der protokollierten Werte in Ihrem Arbeitsbereich

Folgende Azure-Ressourcen werden Ihrem Arbeitsbereich automatisch hinzugefügt, sofern sie regional verfügbar sind:

  - [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
  - [Azure Storage (in englischer Sprache)](https://azure.microsoft.com/services/storage/)
  - [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
  - [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)

Die von Ihnen erstellten Ressourcen können auch in anderen Tutorials und Anleitungen für den Machine Learning-Dienst verwendet werden. Genau wie bei anderen Azure-Diensten gelten auch für bestimmte Machine Learning-Ressourcen gewisse Grenzwerte. Ein Beispiel wäre etwa die Größe des Computeclusters. Erfahren Sie mehr über die [Standardgrenzwerte und das Erhöhen Ihres Kontingents](how-to-manage-quotas.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version des Azure Machine Learning Service](http://aka.ms/AMLFree) aus.


## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]


## <a name="use-the-workspace"></a>Verwenden des Arbeitsbereichs

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2F9Ad]



Sehen Sie sich nun an, wie ein Arbeitsbereich beim Verwalten Ihrer Machine Learning-Skripts hilft. In diesem Abschnitt führen Sie die folgenden Schritte aus:

* Öffnen eines Notebooks in Azure Notebooks
* Ausführen von Code zum Generieren von protokollierten Werten
* Anzeigen der protokollierten Werte in Ihrem Arbeitsbereich

Dieses Beispiel zeigt, wie der Arbeitsbereich Sie bei der Nachverfolgung der in einem Skript generierten Informationen unterstützt. 

### <a name="open-a-notebook"></a>Öffnen eines Notebooks 

[Azure Notebooks](https://notebooks.azure.com) bietet eine kostenlose Cloudplattform für Jupyter Notebooks, die mit allem vorkonfiguriert ist, was Sie für die Ausführung von Machine Learning benötigen. Über Ihren Arbeitsbereich können Sie diese Plattform starten, um mit der Verwendung des Azure Machine Learning Service-Arbeitsbereichs zu beginnen.

1. Wählen Sie auf der Seite des Arbeitsbereichs **Azure Machine Learning-Dienstarbeitsbereich erkunden**.

 ![Erkunden des Arbeitsbereichs](./media/quickstart-get-started/explore_aml.png)

1. Wählen Sie **Azure Notebooks öffnen**, um Ihr erstes Experiment in Azure Notebooks auszuprobieren.  Azure Notebooks ist ein separater Dienst, mit dem Sie Jupyter Notebooks kostenlos in der Cloud ausführen können.  Wenn Sie diesen Link zum Dienst verwenden, werden der von Ihnen in Azure Notebooks erstellten Bibliothek Informationen zum Herstellen einer Verbindung mit Ihrem Arbeitsbereich hinzugefügt.

 ![Öffnen von Azure Notebooks](./media/quickstart-get-started/explore_ws.png)

1. Melden Sie sich bei Azure Notebooks an.  Melden Sie sich unbedingt mit demselben Konto an, mit dem Sie sich auch beim Azure-Portal angemeldet haben. Unter Umständen müssen Sie in Ihrer Organisation zunächst die [Zustimmung des Administrators](https://notebooks.azure.com/help/signing-up/work-or-school-account/admin-consent) einholen, um sich anmelden zu können.

1. Nach Ihrer Anmeldung wird eine neue Registerkarte geöffnet und die Eingabeaufforderung `Clone Library` angezeigt. Beim Klonen dieser Bibliothek werde verschiedene Notebooks und andere Dateien in Ihrem Azure Notebooks-Konto geladen.  Mithilfe dieser Dateien können Sie die Funktionen von Azure Machine Learning Studio erkunden.

1. Deaktivieren Sie **Öffentlich**, damit Ihre Arbeitsbereichsinformationen nicht für andere Benutzer freigegeben werden.

1. Wählen Sie **Klonen** aus.

 ![Klonen einer Bibliothek](./media/quickstart-get-started/clone.png)

1. Wenn der Projektstatus „Beendet“ lautet, klicken Sie auf **Run on Free Computer** (Auf kostenlosem Computer ausführen), um den kostenlosen Notebook-Server zu verwenden.

    ![Ausführen eines Projekts auf kostenlosem Computer](./media/quickstart-get-started/run-project.png)

### <a name="run-the-notebook"></a>Ausführen des Notebooks

Die Liste der Dateien für dieses Projekt enthält eine Datei vom Typ `config.json`. Diese Konfigurationsdatei enthält Informationen zu dem Arbeitsbereich, den Sie im Azure-Portal erstellt haben.  Mithilfe dieser Datei kann Ihr Code eine Verbindung mit Ihrem Arbeitsbereich herstellen und ihm Informationen hinzufügen.

1. Wählen Sie **01.run-experiment.ipynb**, um das Notebook zu öffnen.

1. Im Statusbereich werden Sie angewiesen, zu warten, bis der Kernel gestartet wurde.  Die Meldung wird nicht mehr angezeigt, wenn der Kernel bereit ist.

    ![Warten, bis der Kernel gestartet wird](./media/quickstart-get-started/wait-for-kernel.png)

1. Führen Sie die Zellen jeweils nacheinander mit **UMSCHALT+EINGABE** aus, nachdem der Kernel gestartet wurde. Sie können aber auch **Zellen** > **Alle ausführen** wählen, um das gesamte Notebook auszuführen. Wenn neben einer Zelle ein Sternchen (__*__) angezeigt wird, wird diese Zelle noch ausgeführt. Nach Abschluss der Codeausführung für die Zelle wird eine Zahl angezeigt. 

1. Befolgen Sie die Anweisungen im Notebook, um Ihr Azure-Abonnement zu authentifizieren.

Nachdem Sie die Ausführung aller Zellen im Notebook abgeschlossen haben, können Sie die protokollierten Werte in Ihrem Arbeitsbereich anzeigen.

## <a name="view-logged-values"></a>Anzeigen protokollierter Werte

1. Die Ausgabe der Zelle `run` enthält einen Link zum Azure-Portal, in dem Sie die Experimentergebnisse in Ihrem Arbeitsbereich anzeigen können. 

    ![Anzeigen von Experimenten](./media/quickstart-get-started/view_exp.png)

1. Klicken Sie auf den **Link zum Azure-Portal**, um Informationen zur Ausführung in Ihrem Arbeitsbereich anzuzeigen.  Über diesen Link wird Ihr Arbeitsbereich im Azure-Portal geöffnet.

1. Die angezeigten Plots protokollierter Werte wurden automatisch im Arbeitsbereich erstellt. Wenn Sie mehrere Werte mit dem gleichen Namensparameter protokollieren, wird für Sie automatisch ein Plot generiert.

   ![Anzeigen des Verlaufs](./media/quickstart-get-started/web-results.png)

Da der Code für die Pi-Annäherung willkürliche Werte verwendet, enthalten Ihre Plots andere Werte.  

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Sie können die Ressourcengruppe auch behalten und einen einzelnen Arbeitsbereich löschen. Zeigen Sie die Eigenschaften des Arbeitsbereichs an, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Sie haben die Ressourcen erstellt, die zum Experimentieren und zum Bereitstellen von Modellen benötigt werden. Sie haben Code in einem Notebook ausgeführt. Und Sie haben den Ausführungsverlauf dieses Codes in Ihrem Arbeitsbereich in der Cloud untersucht.

Führen Sie Machine Learning-Tutorials zum Trainieren und Bereitstellen eines Modells aus, um sich ausführlicher mit dem Workflow zu beschäftigen:  

> [!div class="nextstepaction"]
> [Tutorial: Trainieren eines Bildklassifizierungsmodells](tutorial-train-models-with-aml.md)
