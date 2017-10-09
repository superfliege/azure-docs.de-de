---
title: Verwenden von GPU in Azure Machine Learning | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Verwendung von Grafikprozessoren (GPUs, Graphical Processing Units) zum Trainieren tiefer neuronaler Netzwerke in Azure Machine Learning Workbench.
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/14/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: b75b9d82bd6c1e7e7a98fcff6497dbf3a765dcb8
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="how-to-use-gpu-in-azure-machine-learning"></a>Verwenden von GPUs in Azure Machine Learning
Ein Grafikprozessor (GPU) verarbeitet meistens rechenintensive Vorgänge, die in der Regel beim Trainieren bestimmter Modelle für tiefe neuronale Netzwerke vorkommen können. Mithilfe von GPUs können Sie die Trainingszeit der Modelle deutlich reduzieren. In diesem Dokument erfahren Sie, wie Sie Azure ML Workbench für die Verwendung der mit GPUs ausgestatteten [DSVM (Data Science Virtual Machine)](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-virtual-machine-overview) als Ausführungsziel konfigurieren. 

## <a name="prerequisites"></a>Voraussetzungen
- Um dieses Handbuch auszuführen, müssen Sie zuerst [Azure ML Workbench installieren](quickstart-installation.md).
- Sie benötigen Zugriff auf Computer, die mit NVidia-GPUs ausgestattet sind.
    - Sie können Ihre Skripts direkt auf dem lokalen Computer (Windows oder macOS) mit GPUs ausführen.
    - Sie können Skripts auch in einem Docker-Container auf einem Computer mit GPUs ausführen.

## <a name="execute-in-local-environment-with-gpus"></a>Ausführen in _lokaler_ Umgebung mit GPUs
Sie können Azure ML Workbench auf einem mit GPUs ausgestatteten Computer installieren und in der _lokalen_ Umgebung ausführen. Infrage kommt:
- Ein physischer Windows- oder macOS-Computer.
- Eine Windows-VM (Virtual Machine) wie z.B. eine Windows-DSVM, die mit einer VMs-Vorlage aus der Azure-NC-Serie bereitgestellt wird.

In diesem Fall ist keine spezielle Konfiguration in Azure ML Workbench erforderlich. Achten Sie nur darauf, dass Sie alle erforderlichen Treiber, Toolkits und GPU-fähigen Machine Learning-Bibliotheken lokal installiert haben. Führen Sie einfach in der _lokalen_ Umgebung aus, wo die Python-Laufzeit direkt auf die lokale GPU-Hardware zugreifen kann.

1. Öffnen Sie Azure ML Workbench. Wechseln Sie zu **File** und **Open Command Prompt**. 
2. Installieren Sie über die Befehlszeile GPU-fähiges Deep Learning-Framework wie Microsoft Cognitive Toolkit, TensorFlow etc. Beispiel:

```batch
REM install latest TensorFlow with GPU support
C:\MyProj> pip install tensorflow-gpu

REM install Microsoft Cognitive Toolkit 2.1 (1-bit SGD) with GPU support on Windows
C:\MyProj> pip install https://cntk.ai/PythonWheel/GPU-1bit-SGD/cntk-2.1-cp35-cp35m-win_amd64.whl
```

3. Schreiben Sie Python-Code, der die Deep Learning-Bibliotheken nutzt.
4. Wählen Sie _local_ als Computeumgebung, und führen Sie den Python-Code aus.

```batch
REM execute Python script in local environment
C:\MyProj> az ml experiment submit -c local my-deep-learning-script.py
```

## <a name="execute-in-docker-environment-on-linux-vm-with-gpus"></a>Ausführen in _docker_-Umgebung auf einer Linux-VM mit GPUs
Azure ML Workbench unterstützt auch die Ausführung in Docker auf einer Azure Linux-VM. Hier haben Sie eine sinnvolle Option, rechenintensive Aufträge auf leistungsstarker virtueller Hardware auszuführen, und zahlen nur für die Verwendung, weil Sie sie anschließend ausschalten. Es ist zwar im Prinzip möglich, GPUs auf allen virtuellen Linux-Computern zu verwenden, jedoch sind auf der Ubuntu-basierten DSVM die erforderlichen CUDA-Treiber und Bibliotheken vorinstalliert, was die Einrichtung wesentlich erleichtert. Führen Sie die folgenden Schritte aus:

### <a name="create-a-ubuntu-based-linux-data-science-virtual-machine-in-azure"></a>Erstellen einer Ubuntu-basierten Linux-Data Science Virtual Machine in Azure
1. Navigieren Sie in Ihrem Webbrowser zum [Azure-Portal](https://portal.azure.com).

2. Wählen Sie **+ Neu** auf der linken Seite des Portals.

3. Suchen Sie im Marketplace nach „Data Science Virtual Machine for Linux (Ubuntu)“.

4. Klicken Sie auf **Erstellen**, um eine Ubuntu-DSVM zu erstellen.

5. Geben Sie im Formular **Grundlagen** die erforderlichen Informationen ein.
Wenn Sie den Speicherort für Ihren virtuellen Computer auswählen, beachten Sie, dass GPU-VMs nur in bestimmten Azure-Regionen verfügbar sind, z.B. **USA, Süden-Mitte**. Siehe [Products available by region](https://azure.microsoft.com/en-us/regions/services/) (Verfügbare Produkte nach Region).
Klicken Sie auf „OK“, um die **Grundlagen**-Informationen zu Speichern.

6. Wählen Sie die Größe des virtuellen Computers. Wählen Sie eine der Größen mit virtuellen Computern mit dem NC-Präfix, die mit NVidia-GPU-Chips ausgestattet sind.  Klicken Sie auf **Alle anzeigen**, um die vollständige Liste nach Bedarf anzuzeigen. Unter [GPU](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-gpu) erfahren Sie mehr.

7. Schließen Sie die verbleibenden Einstellungen ab, und überprüfen Sie die Erwerbsinformationen. Klicken Sie auf „Kaufen“, um den virtuellen Computer zu erstellen. Notieren Sie die IP-Adresse, die Sie dem virtuellen Computer zugewiesen haben. 

### <a name="create-a-new-project-in-azure-ml-workbench"></a>Erstellen eines neuen Projekts in Azure ML Workbench 
Sie können das Beispiel _Classifying MNIST using TensorFlow_ oder _Classifying MNIST dataset with CNTK_ verwenden.

### <a name="create-a-new-compute-target"></a>Erstellen eines Computeziels
Starten Sie die Befehlszeile in Azure ML Workbench. Geben Sie den folgenden Befehl ein. Ersetzen Sie den Platzhaltertext aus dem Beispiel unten durch Ihre eigenen Werte für Name, IP-Adresse, Benutzername und Kennwort. 

```batch
C:\MyProj> az ml computetarget attach --name "my_dsvm" --address "my_dsvm_ip_address" --username "my_name" --password "my_password" --type remotedocker
```

### <a name="configure-azure-ml-workbench-to-access-gpu"></a>Konfigurieren von Azure ML Workbench für den GPU-Zugriff
Wechseln Sie wieder zum Projekt, öffnen Sie **File View**, und klicken Sie auf die **Refresh**-Schaltfläche. Nun sehen Sie zwei neue Konfigurationsdateien `my_dsvm.compute` und `my_dsvm.runconfig`.
 
Öffnen Sie die Datei `my_dsvm.compute`. Ändern Sie `baseDockerImage` in `microsoft/mmlspark:plus-gpu-0.7.9`, und fügen Sie eine neue Zeile `nvidiaDocker: true` hinzu. Die Datei sollte also diese beiden Zeilen enthalten:
 
```yaml
...
baseDockerImage: microsoft/mmlspark:plus-gpu-0.7.91
nvidiaDocker: true
```
 
Öffnen Sie jetzt `my_dsvm.runconfig`, und ändern Sie den `Framework`-Wert von `PySpark` in `Python`. Wenn diese Zeile nicht angezeigt wird, fügen Sie sie hinzu, da der Standardwert `PySpark` wäre.

```yaml
"Framework": "Python"
```
### <a name="reference-deep-learning-framework"></a>Verweisen auf das Deep Learning-Framework 
Öffnen Sie die Datei `conda_dependencies.yml`, und stellen Sie sicher, dass Sie die GPU-Version des Deep Learning-Framework-Python-Pakets verwenden. In den Beispielprojekten werden CPU-Versionen aufgelistet, sodass Sie diese Änderung vornehmen müssen.

Beispiel für TensorFlow: 
```
name: project_environment
dependencies:
  - python=3.5.2
  # latest TensorFlow library with GPU support
  - tensorflow-gpu
```

Beispiel für Microsoft Cognitive Toolkit:
```yaml
name: project_environment
dependencies:
  - python=3.5.2
  - pip: 
    # use the Linux build of Microsoft Cognitive Toolkit 2.1 with GPU support
    - https://cntk.ai/PythonWheel/GPU/cntk-2.1-cp35-cp35m-win_amd64.whl
```

Sie können auch die 1-Bit-SGD-Version von Microsoft Cognitive Toolkit verwenden, die Leistungsverbesserungen auf VMs mit mehreren GPUs bietet. Beachten Sie [die Lizenzanforderungen für 1-Bit-SGD](https://docs.microsoft.com/en-us/cognitive-toolkit/cntk-1bit-sgd-license).

```yaml
name: project_environment
dependencies:
  - python=3.5.2
  - pip:    
    # use the Linux build of the Microsoft Cognitive Toolkit 2.1 with 1-bit SGD and GPU support
    - https://cntk.ai/PythonWheel/GPU-1bit-SGD/cntk-2.1-cp35-cp35m-linux_x86_64.whl
```

### <a name="execute"></a>Ausführen
Sie können nun Ihre Python-Skripts ausführen. Sie können Sie in der Azure ML Workbench mit dem `my_dsvm`-Kontext ausführen oder von der Befehlszeile aus starten:
 
```batch
C:\myProj> az ml experiment submit -c my_dsvm my_tensorflow_or_cntk_script.py
```
 
Um sicherzustellen, dass die GPU verwendet wird, prüfen Sie, ob die Ausgabe der Ausführung der folgenden ähnelt:

```
name: Tesla K80
major: 3 minor: 7 memoryClockRate (GHz) 0.8235
pciBusID 06c3:00:00.0
Total memory: 11.17GiB
Free memory: 11.11GiB
```

Glückwunsch! Das Skript nutzt nun die GPU-Leistungsfähigkeit über einen Docker-Container!

## <a name="next-steps"></a>Nächste Schritte
Betrachten Sie ein Beispiel der GPU-Verwendung zum Beschleunigen des Trainings tiefer neuronaler Netzwerke im Azure Machine Learning-Katalog.

