---
title: "Deep Learning- und KI-Frameworks – Azure | Microsoft-Dokumentation"
description: Deep Learning- und KI-Frameworks
keywords: "Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: 46662c15374f781a6527bb1435d883ae060c63f4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="deep-learning-and-ai-frameworks"></a>Deep Learning- und KI-Frameworks
Die [Data Science-VM](http://aka.ms/dsvm) (DSVM) und die [Deep Learning-VM](http://aka.ms/dsvm/deeplearning) unterstützen eine Reihe von Deep Learning-Frameworks zur Unterstützung bei der Erstellung von KI-Anwendungen mit Predictive Analytics und kognitiven Fähigkeiten wie Bild- und Sprachverständnis. 

Hier finden Sie Details zu allen auf der DSVM verfügbaren Deep Learning-Frameworks.

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

|    |           |
| ------------- | ------------- |
| Was ist das?   | Deep Learning-Framework      |
| Unterstützte DSVM-Editionen      | Windows, Linux     |
| Konfiguration/Installation auf der DSVM  | Das Microsoft Cognitive Toolkit (CNTK) ist in Python 2.7 (in der _root_-Umgebung) und in Python 3.5 (in der _py35_-Umgebung) installiert.   |
| Links zu Beispielen      | Jupyter-Beispielnotebooks sind enthalten.     |
| Verwandte Tools auf der DSVM      | Keras      |
| Verwendung/Ausführung    | Öffnen Sie Jupyter, und suchen Sie nach dem Ordner „CNTK“.  |

## <a name="tensorflow"></a>TensorFlow

|    |           |
| ------------- | ------------- |
| Was ist das?   | Deep Learning-Framework      |
| Unterstützte DSVM-Editionen      | Windows, Linux     |
| Konfiguration/Installation auf der DSVM  | Unter Linux ist TensorFlow in der Python 2.7-Umgebung (_root_) sowie in der Python 3.5-Umgebung (_py35_) installiert. Unter Windows ist TensorFlow in der Python 3.5-Umgebung (_py35_) installiert.  |
| Links zu Beispielen      | Jupyter-Beispielnotebooks sind enthalten.     |
| Verwandte Tools auf der DSVM      | Keras      |
| Verwendung/Ausführung    | Öffnen Sie Jupyter, und suchen Sie nach dem Ordner „TensorFlow“.  |

## <a name="keras"></a>Keras

|    |           |
| ------------- | ------------- |
| Was ist das?   | Deep Learning-Framework      |
| Unterstützte DSVM-Editionen      | Windows, Linux     |
| Konfiguration/Installation auf der DSVM  | Keras ist in der Python 2.7-Umgebung (_root_) sowie in der Python 3.5-Umgebung (_py35_) installiert.   |
| Links zu Beispielen      | https://github.com/fchollet/keras/tree/master/examples      |
| Verwandte Tools auf der DSVM      | Microsoft Cognitive Toolkit, TensorFlow, Theano      |
| Verwendung/Ausführung    | Laden Sie das Beispiel aus Github herunter, kopieren Sie es in ein Verzeichnis unter „~/notebooks“, und öffnen Sie es in Jupyter.   |




## <a name="caffe"></a>Caffe

|    |           |
| ------------- | ------------- |
| Was ist das?   | Deep Learning-Framework      |
| Unterstützte DSVM-Editionen      | Linux     |
| Konfiguration/Installation auf der DSVM  | Caffe ist unter `/opt/caffe` installiert.    |
| Links zu Beispielen      | Beispiele finden Sie unter `/opt/caffe/examples`.      |
| Verwandte Tools auf der DSVM      | Caffe2      |
### <a name="how-to-use--run-it"></a>Verwendung/Ausführung  

Melden Sie sich mithilfe von X2Go bei Ihrem virtuellen Computer an. Starten Sie anschließend ein neues Terminal, und geben Sie Folgendes ein:

```
cd /opt/caffe/examples
jupyter notebook
```

Daraufhin öffnet sich ein neues Browserfenster mit Beispielnotebooks.

## <a name="caffe2"></a>Caffe2

|    |           |
| ------------- | ------------- |
| Was ist das?   | Deep Learning-Framework      |
| Unterstützte DSVM-Editionen      | Linux     |
| Konfiguration/Installation auf der DSVM  | Caffe2 ist unter `/opt/caffe2` installiert. Es ist auch verfügbar für die Python 2.7-Conda-Umgebung (_root_).     |
| Links zu Beispielen      | Jupyter-Beispielnotebooks sind enthalten.     |
| Verwandte Tools auf der DSVM      | Caffe      |
| Verwendung/Ausführung    | Öffnen Sie Jupyter, und navigieren Sie zum Caffe2-Verzeichnis. Dort finden Sie Beispielnotebooks. Für einige Notebooks muss im Python-Code der Caffe2-Stamm festgelegt werden. Geben Sie „/opt/caffe2“ ein.   |


## <a name="chainer"></a>Chainer

|    |           |
| ------------- | ------------- |
| Was ist das?   | Deep Learning-Framework      |
| Unterstützte DSVM-Editionen      | Windows, Linux     |
| Konfiguration/Installation auf der DSVM  | Chainer ist in der Python 2.7-Umgebung (_root_) sowie in der Python 3.5-Umgebung (_py35_) installiert. ChainerRL und ChainerCV sind ebenfalls installiert.   |
| Links zu Beispielen      | Jupyter-Beispielnotebooks sind enthalten.      |
| Verwandte Tools auf der DSVM      | Caffe      |

### <a name="how-to-use--run-it"></a>Verwendung/Ausführung  

Aktivieren Sie an einem Terminal die gewünschte Python-Version (_root_ oder _py35_), führen Sie _python_ aus, und importieren Sie anschließend Chainer. Wählen Sie in Jupyter den Python 2.7- oder den Python 3.5-Kernel aus, und importieren Sie anschließend Chainer.


## <a name="deep-water"></a>Deep Water

|    |           |
| ------------- | ------------- |
| Was ist das?   | Deep Learning-Framework für H2O      |
| Unterstützte DSVM-Editionen      | Linux     |
| Konfiguration/Installation auf der DSVM  | Deep Water ist unter `/dsvm/tools/deep_water` installiert.   |
| Links zu Beispielen      | Beispiele werden über den Deep Water-Server bereitgestellt.      |
| Verwandte Tools auf der DSVM      | H2O, Sparkling Water      |

### <a name="how-to-use--run-it"></a>Verwendung/Ausführung  

Stellen Sie mithilfe von X2Go eine Verbindung mit dem virtuellen Computer her. Starten Sie an einem Terminal den Deep Water-Server:

    java -jar /dsvm/tools/deep_water/h2o.jar

Öffnen Sie anschließend einen Browser, und stellen Sie eine Verbindung mit `http://localhost:54321` her.



## <a name="mxnet"></a>MXNet

|    |           |
| ------------- | ------------- |
| Was ist das?   | Deep Learning-Framework      |
| Unterstützte DSVM-Editionen      | Windows, Linux     |
| Konfiguration/Installation auf der DSVM  | MXNet ist unter `C:\dsvm\tools\mxnet` (Windows) bzw. unter `/dsvm/tools/mxnet` (Linux) installiert. Python-Bindungen sind in der Python 2.7-Umgebung (_root_) sowie in der Python 3.5-Umgebung (_py35_) installiert. R-Bindungen sind ebenfalls installiert.   |
| Links zu Beispielen      | Jupyter-Beispielnotebooks sind enthalten.    |
| Verwandte Tools auf der DSVM      | Keras      |
| Verwendung/Ausführung    | Öffnen Sie Jupyter, und suchen Sie nach dem Ordner „mxnet“.  |

## <a name="nvidia-digits"></a>NVIDIA DIGITS

|    |           |
| ------------- | ------------- |
| Was ist das?   | Deep Learning-System von NVIDIA zum schnellen Trainieren von Deep Learning-Modellen      |
| Unterstützte DSVM-Editionen      | Linux     |
| Konfiguration/Installation auf der DSVM  | DIGITS ist unter `/dsvm/tools/DIGITS` installiert und als Dienst mit der Bezeichnung _digits_ verfügbar.   |
### <a name="how-to-use--run-it"></a>Verwendung/Ausführung  

Melden Sie sich mithilfe von X2Go bei dem virtuellen Computer an. Starten Sie den Dienst über ein Terminal:

    sudo systemctl start digits

Das Starten des Diensts dauert ca. eine Minute. Starten Sie einen Webbrowser, und navigieren Sie zu `http://localhost:5000`.



## <a name="nvdia-smi"></a>nvidia-smi

|    |           |
| ------------- | ------------- |
| Was ist das?   | NVIDIA-Tool zum Abfragen der GPU-Aktivität      |
| Unterstützte DSVM-Editionen      | Windows, Linux     |
| Konfiguration/Installation auf der DSVM  | _nvidia-smi_ steht unter dem Systempfad zur Verfügung.   |
| Verwendung/Ausführung | Starten Sie eine Eingabeaufforderung (Windows) oder ein Terminal (Linux), und führen Sie _nvidia-smi_ aus.



## <a name="theano"></a>Theano

|    |           |
| ------------- | ------------- |
| Was ist das?   | Deep Learning-Framework      |
| Unterstützte DSVM-Editionen      | Linux     |
| Konfiguration/Installation auf der DSVM  | Theano ist in der Python 2.7-Umgebung (_root_) sowie in der Python 3.5-Umgebung (_py35_) installiert.   |
| Verwandte Tools auf der DSVM      | Keras      |
| Verwendung/Ausführung    | Aktivieren Sie an einem Terminal die gewünschte Python-Version („root“ oder „py35“), führen Sie Python aus, und importieren Sie anschließend Theano. Wählen Sie in Jupyter den Python 2.7- oder den Python 3.5-Kernel aus, und importieren Sie anschließend Theano.  |



## <a name="torch"></a>Torch

|    |           |
| ------------- | ------------- |
| Was ist das?   | Deep Learning-Framework      |
| Unterstützte DSVM-Editionen      | Linux     |
| Konfiguration/Installation auf der DSVM  | Torch ist unter `/dsvm/tools/torch` installiert. PyTorch ist in der Python 2.7-Umgebung (_root_) sowie in der Python 3.5-Umgebung (_py35_) installiert.   |
| Links zu Beispielen      | Torch-Beispiele finden Sie unter `/dsvm/samples/torch`. PyTorch-Beispiele finden Sie unter `/dsvm/samples/pytorch`.      |

