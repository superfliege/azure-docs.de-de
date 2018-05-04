---
title: Anpassen des Containerimages für die Bereitstellung von Azure ML-Modellen | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie ein Containerimage für Azure Machine Learning-Modelle anpassen.
services: machine-learning
author: tedway
ms.author: tedway, raymondl
manager: mwinkle
ms.reviewer: mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 3/26/2018
ms.openlocfilehash: f56b651c40187e42361ac12f0cbf4e509385e0d2
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="customize-the-container-image-used-for-azure-ml-models"></a>Anpassen des Containerimages für Azure ML-Modelle

In diesem Artikel wird beschrieben, wie Sie ein Containerimage für Azure Machine Learning-Modelle anpassen.  In Azure ML Workbench werden Container zum Bereitstellen von Machine Learning-Modellen verwendet. Die Modelle werden zusammen mit ihren Abhängigkeiten bereitgestellt, und Azure ML erstellt aus dem Modell, den Abhängigkeiten und den zugeordneten Dateien ein Image.

## <a name="how-to-customize-the-docker-image"></a>Anpassen des Docker-Images
Passen Sie das von Azure ML bereitgestellte Docker-Image an, indem Sie Folgendes verwenden:

1. Eine `dependencies.yml`-Datei: Dient zum Verwalten von Abhängigkeiten, die über [PyPi]( https://pypi.python.org/pypi) installiert werden können. Sie können die Datei „`conda_dependencies.yml`“ aus dem Workbench-Projekt verwenden oder eine eigene Datei erstellen. Dies ist der empfohlene Ansatz zum Installieren von Python-Abhängigkeiten, die über pip installiert werden können.

   CLI-Beispielbefehl:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> -c amlconfig\conda_dependencies.yml
   ```

   Beispieldatei „conda_dependencies“: 
   ```yaml
   name: project_environment
   dependencies:
      - python=3.5.2
      - scikit-learn
      - ipykernel=4.6.1
      
      - pip:
        - azure-ml-api-sdk==0.1.0a11
        - matplotlib
   ```
        
2. Datei mit Docker-Schritten: Mit dieser Option passen Sie das bereitgestellte Image an, indem Sie Abhängigkeiten installieren, die nicht über PyPi installiert werden können. 

   Die Datei sollte Docker-Installationsschritte wie in einer Dockerfile enthalten. Die folgenden Befehle sind in der Datei zulässig: 

    RUN, ENV, ARG, LABEL, EXPOSE

   CLI-Beispielbefehl:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> --docker-file <myDockerStepsFileName> 
   ```

   Für Image-, Manifest- und Dienstbefehle wird das Flag „docker-file“ akzeptiert.

   Beispiel für Datei mit Docker-Schritten:
   ```docker
   # Install tools required to build the project
   RUN apt-get update && apt-get install -y git libxml2-dev
   # Install library dependencies
   RUN dep ensure -vendor-only
   ```

> [!NOTE]
> Das Basisimage für Azure ML-Container ist Ubuntu und kann nicht geändert werden. Wenn Sie ein anderes Basisimage angeben, wird es ignoriert.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihr Containerimage angepasst haben, können Sie es in einem Cluster für den großflächigen Einsatz bereitstellen.  Weitere Informationen zum Einrichten eines Clusters für die Bereitstellung von Webdiensten finden Sie unter [Konfiguration der Modellverwaltung](deployment-setup-configuration.md). 
