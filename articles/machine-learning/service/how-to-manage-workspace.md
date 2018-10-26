---
title: Erstellen und Verwalten von Azure Machine Learning Service-Arbeitsbereichen
description: Hier erfahren Sie, wie Sie Azure Machine Learning Service-Arbeitsbereiche im Azure-Portal erstellen, anzeigen und löschen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 09/24/2018
ms.openlocfilehash: c0bb27dccdaf25da818d5d54a8634556a95da737
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238675"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Erstellen und Verwalten von Azure Machine Learning Service-Arbeitsbereichen

In diesem Artikel wird erläutert, wie Sie [**Azure Machine Learning Service-Arbeitsbereiche**](concept-azure-machine-learning-architecture.md#workspace) im Azure-Portal für [Azure Machine Learning Service](overview-what-is-azure-ml.md) erstellen, anzeigen und löschen.  Sie können Arbeitsbereiche auch mit der [CLI](reference-azure-machine-learning-cli.md) oder mit [Python-Code](http://aka.ms/aml-sdk) erstellen und löschen.

Zum Erstellen eines Arbeitsbereichs benötigen Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="view-a-workspace"></a>Anzeigen eines Arbeitsbereichs

1. Wählen Sie links oben im Portal die Option **Alle Dienste** aus. 

1. Geben Sie **Machine Learning Service-Arbeitsbereich** in das Filterfeld **Alle Dienste** ein.  

   ![Suchen nach dem Azure Machine Learning Service-Arbeitsbereich](media/how-to-manage-workspace/allservices-search1.png)

1. Wählen Sie in den Filterergebnissen **Machine Learning Service-Arbeitsbereich** aus, um eine Liste Ihrer Arbeitsbereiche anzuzeigen. 

   ![Suchen nach dem Azure Machine Learning Service-Arbeitsbereich](media/how-to-manage-workspace/allservices-search.PNG)

1. Durchsuchen Sie die Liste der gefundenen Arbeitsbereiche. Sie können basierend auf Abonnement, Ressourcengruppen und Standorten filtern.  

   ![Liste mit Azure Machine Learning Service-Arbeitsbereichen](media/how-to-manage-workspace/allservices_view_workspace.PNG)

1. Wählen Sie den eben erstellten Arbeitsbereich aus, um seine Eigenschaften anzuzeigen.

   ![png](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Löschen eines Arbeitsbereichs

Verwenden Sie die Schaltfläche „Löschen“ oben in dem Arbeitsbereich, den Sie löschen möchten.

  ![png](media/how-to-manage-workspace/delete-workspace.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

Schließen Sie das gesamte Tutorial ab, um zu erfahren, wie Sie einen Arbeitsbereich zum Erstellen, Trainieren und Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst verwenden.

> [!div class="nextstepaction"]
> [Tutorial: Trainieren von Modellen](tutorial-train-models-with-aml.md)
