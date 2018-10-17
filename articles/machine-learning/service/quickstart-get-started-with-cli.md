---
title: Erste Schritte mit Azure Machine Learning Services mithilfe der CLI-Erweiterung | Microsoft-Dokumentation
description: In diesem Schnellstart lernen Sie die ersten Schritte mit Azure Machine Learning Services mithilfe der Azure Machine Learning-CLI-Erweiterung.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
author: rastala
ms.author: roastala
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 856f9629e97f8cf7cf811e7d591cbcad6067f47a
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237159"
---
# <a name="quickstart-get-started-with-azure-machine-learning-using-the-cli-extension"></a>Schnellstart: Erste Schritte mit Azure Machine Learning mithilfe der CLI-Erweiterung

In diesem Schnellstart verwenden Sie eine Machine Learning-CLI-Erweiterung zum Einstieg in [Azure Machine Learning Services](overview-what-is-azure-ml.md) (Vorschau).

Mithilfe der Befehlszeilenschnittstelle lernen Sie:

1. Erstellen eines Arbeitsbereichs in Ihrem Azure-Abonnement. Der Arbeitsbereich wird von einem oder mehreren Benutzern zum Speichern von Computeressourcen, Modellen, Bereitstellungen und Ausführungsverläufen in der Cloud verwendet.
1. Hinzufügen eines Projekts zu Ihrem Arbeitsbereich.   Ein Projekt ist ein lokaler Ordner mit den Skripts und Konfigurationsdateien, die erforderlich sind, um Ihr Machine Learning-Problem zu lösen.  
1. Ausführen eines Python-Skripts in Ihrem Projekt, das einige Werte über mehrere Iterationen hinweg protokolliert.
1. Anzeigen der protokollierten Werte im Ausführungsverlauf Ihres Arbeitsbereichs.

> [!NOTE]
> Der Einfachheit halber werden die folgenden Azure-Ressourcen bei regionaler Verfügbarkeit Ihrem Arbeitsbereich automatisch hinzugefügt: [Containerregistrierung](https://azure.microsoft.com/services/container-registry/), [Storage](https://azure.microsoft.com/services/storage/), [Application Insights](https://azure.microsoft.com/services/application-insights/) und [Key Vault](https://azure.microsoft.com/services/key-vault/).

Die von Ihnen erstellten Ressourcen können ggf. auch in anderen Azure Machine Learning-Tutorials und -Anleitungen verwendet werden.

Diese CLI wurde auf der Basis des Python-basierten <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a> für Azure Machine Learning Services erstellt.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie mit den Schritten des Schnellstarts beginnen:

+ Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
+ [Python 3.5 oder höher](https://www.python.org/) installiert
+ [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) installiert

## <a name="install-the-cli-extension"></a>Installieren der CLI-Erweiterung

Öffnen Sie auf Ihrem Computer einen Befehlszeilen-Editor, und installieren Sie [die Machine Learning-Erweiterung für Azure CLI](reference-azure-machine-learning-cli.md).  Die Installation kann einige Minuten dauern.

```azurecli
az extension add azureml-sdk
```

## <a name="install-the-sdk"></a>Installieren des SDKs

[!INCLUDE [aml-install-sdk](../../../includes/aml-install-sdk.md)]

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. Melden Sie sich mithilfe der Azure CLI bei Azure an, geben Sie das Abonnement an, und erstellen Sie eine Ressourcengruppe.

Melden Sie sich in einem Befehlszeilenfenster mit dem Azure CLI-Befehl `az login` an. Führen Sie die Anweisungen für die interaktive Anmeldung aus:
    
   ```azurecli
   az login
   ```

Listen Sie die verfügbaren Azure-Abonnements auf, und geben Sie dasjenige an, das Sie verwenden möchten:
   ```azurecli
   az account list --output table
   az account set --subscription <your-subscription-id>
   az account show
   ```
   Dabei ist \<your-subscription-id\> der ID-Wert für das gewünschte Abonnement. Geben Sie die Klammern nicht mit an.

Erstellen Sie eine Ressourcengruppe für Ihren Arbeitsbereich.
In dieser Schnellstartanleitung gilt:
   + Der Name der Ressourcengruppe ist `docs-aml`.
   + Die Region ist `eastus2`. 

   ```azurecli
   az group create -n docs-aml -l eastus2
   ```

## <a name="create-a-workspace-and-a-project-folder"></a>Erstellen eines Arbeitsbereichs und eines Projektordners

Erstellen Sie im Befehlszeilenfenster einen Arbeitsbereich für den Azure Machine Learning-Dienst unter der Ressourcengruppe.


   In dieser Schnellstartanleitung gilt:
   + Der Arbeitsbereichsname ist `docs-ws`.
   + Der Name der Ressourcengruppe lautet `docs-aml`.

   ```azurecli
   az ml workspace create -n docs-ws -g docs-aml
   ```

Erstellen Sie im Befehlszeilenfenster für Ihr Azure Machine Learning-Projekt einen Ordner auf Ihrem lokalen Computer.

   ```
   mkdir docs-prj
   cd docs-prj
   ```

## <a name="create-a-python-script"></a>Erstellen eines Python-Skripts

[!INCLUDE [aml-create-script-pi](../../../includes/aml-create-script-pi.md)]

## <a name="run-the-script"></a>Ausführen des Skripts

Fügen Sie den Ordner als Projekt dem Arbeitsbereich hinzu. Das `--history`-Argument gibt einen Namen für die Ausführungsverlaufsdatei an, die die Metriken für jede Ausführung aufzeichnet.

   ```azurecli
   az ml project attach --history my_history -w docs-ws -g docs-aml
   ```

Führen Sie das Skript auf Ihrem lokalen Computer aus.

   ```azurecli
   az ml run submit -c local pi.py
   ```

   Dieser Befehl führt den Code aus und gibt einen Weblink in der Konsole aus. Kopieren Sie den Link, und fügen Sie ihn in Ihren Webbrowser ein.

Rufen Sie in einem Webbrowser die URL auf. Ein Webportal mit den Ergebnissen der Ausführung wird angezeigt. Sie können die Ergebnisse dieser oder vorheriger Ausführungen überprüfen, sofern vorhanden.

Das Portaldashboard wird in Edge-, Chrome- und Firefox-Browsern unterstützt.

   ![Anzeigen des Verlaufs](./media/quickstart-get-started/web-results.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte
Sie haben damit die erforderlichen Ressourcen erstellt, um mit dem Experimentieren und Bereitstellen von Modellen zu beginnen. Außerdem haben Sie ein neues Projekt erstellt, ein Skript ausgeführt und den Ausführungsverlauf des Skripts untersucht.

Ausführliche Workflows finden Sie in den Tutorials für Azure Machine Learning zum Erstellen, Trainieren und Bereitstellen eines Modells.

> [!div class="nextstepaction"]
> [Tutorial 1: Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-train-models-with-aml.md)