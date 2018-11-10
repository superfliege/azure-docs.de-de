---
title: Entwicklungsumgebungen für Azure Machine Learning | Microsoft-Dokumentation
description: Enthält eine Übersicht über Entwicklungsumgebungen, die Sie mit dem Azure Machine Learning-Dienst verwenden können. Python 3 ist die einzige Anforderung für Ihre Entwicklungsumgebung, aber wir empfehlen Ihnen auch die Verwendung von Conda-Umgebungen. In Bezug auf Entwicklungstools empfehlen wir Ihnen Jupyter Notebooks, Azure Notebooks und IDE-/Code-Editoren.
services: machine-learning
author: rastala
ms.author: roastala
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
ms.topic: conceptual
ms.date: 9/24/2018
ms.openlocfilehash: 4d25e147044053aa76afe2da482b71c24efc2325
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242873"
---
# <a name="development-environment-for-azure-machine-learning"></a>Entwicklungsumgebung für Azure Machine Learning 

Informieren Sie sich über die Entwicklungsumgebungen, die Sie mit dem Azure Machine Learning-Dienst verwenden können. 

Der Azure Machine Learning-Dienst ist plattformunabhängig und benötigt keine bestimmte Entwicklungsumgebung. __Python 3__ ist erforderlich, und wir empfehlen die Verwendung von __Conda__ zum Erstellen von isolierten Umgebungen. __Wenn Sie bereits über eine Entwicklungsumgebung verfügen, die diese Anforderungen erfüllt__, können Sie dieses Dokument überspringen und mit dem Dokument zum Thema [Konfigurieren Ihrer Entwicklungsumgebung](how-to-configure-environment.md) fortfahren.

Im restlichen Teil dieses Dokuments werden die von uns empfohlenen Entwicklungsumgebungen beschrieben:

* __Jupyter-Notebooks__
* __Azure Notebooks__
* __Integrierte Entwicklungsumgebungen (IDEs) und Code-Editoren__
* __Virtueller Computer für Data Science__

Ein Vergleich zwischen diesen Umgebungen ist schwierig, da sowohl Notebooks als auch IDEs erweitert werden können. Beispielsweise können einige IDEs als Clients für Jupyter Notebooks genutzt werden. Im Allgemeinen sind __Notebooks__ für __interaktive Experimente__ und die __Visualisierung__ konzipiert. __IDEs und Code-Editoren__ stellen Tools für die __Verbesserung der Codequalität__ und __Integration in externe Systeme__ (z.B. die Versionskontrolle) bereit.

> [!TIP]
> Wenn Sie eine Umgebung verwenden, schließt dies nicht die Nutzung der anderen Umgebung aus. Notebooks und IDEs sind lediglich Tools, die je nach Bedarf kombiniert werden können. Beispielsweise können Sie in einem Notebook mit dem Experimentieren beginnen und dann ein Python-Skript zum Bearbeiten und Debuggen in einer IDE exportieren.
>
> Dies ist der Grund, warum die Data Science Virtual Machine sowohl über Jupyter Notebooks als auch über mehrere beliebte Python-IDEs verfügt.

## <a name="jupyter-notebooks"></a>Jupyter-Notebooks

Jupyter Notebooks sind Teil des [Jupyter-Projekts](https://jupyter.org/). Sie sollen vor allem für die Bereitstellung einer interaktiven Codierumgebung sorgen, mit der Sie Dokumente erstellen können, in denen Livecode mit beschreibendem Text und Grafiken gemischt ist. Sie können Jupyter Notebooks auf vielen unterschiedlichen Plattformen installieren.

Wenn Sie über eine eigene Jupyter Notebook-Installation verfügen, können Sie die Umgebung wie gewünscht installieren und konfigurieren. Sie sind dann aber für die Wartung des Systems verantwortlich.

## <a name="azure-notebooks"></a>Azure-Notebooks

[Azure Notebooks](https://notebooks.azure.com) (Vorschauversion) ist eine Notebookumgebung in der Azure-Cloud. Sie basiert auf Jupyter, und es wird eine Umgebung bereitgestellt, in der beliebte Bibliotheken bereits vorab geladen sind. Das Azure Machine Learning SDK ist in Azure Notebooks bereits installiert, sodass Sie nahezu ohne Setupaufwand mit dem Experimentieren beginnen können.

Mit Azure Notebooks wird zudem der Prozess für das Freigeben Ihrer Notebooks vereinfacht. Sie können eine URL für Ihre Notebooks teilen, Ihre Bibliothek öffentlich verfügbar machen oder diese über die Azure Notebooks-Schnittstelle in sozialen Medien bereitstellen.

Der Nachteil von Azure Notebooks ist, dass Sie keine vollständige Kontrolle über die Umgebung haben und ggf. keine benutzerdefinierte Software installieren können, die Sie benötigen. Außerdem handelt es sich um eine freigegebene Umgebung, sodass Ihre Notebooks unter Umständen langsamer als unter einer dedizierten Jupyter Notebook-Installation ausgeführt werden.

## <a name="ides-and-code-editors"></a>IDEs und Code-Editoren

Es gibt viele IDEs und Code-Editoren, die mit Azure Machine Learning funktionieren. Die einzige Softwareanforderung ist das Azure Machine Learning SDK, das mit dem Hilfsprogramm `pip` installiert werden kann.

Wir empfehlen die Nutzung von [Visual Studio Code](https://code.visualstudio.com/), weil Sie damit über Tools für die Arbeit sowohl mit der Python-Sprache als auch mit Azure Machine Learning verfügen. Die Anwendung ist für Linux-, macOS- und Windows-Plattformen verfügbar.

## <a name="data-science-virtual-machine"></a>Virtueller Computer für Data Science

Die DSVM (Data Science Virtual Machine) ist eine Kombination aus den vorherigen Umgebungen. Es handelt sich um eine VM auf der Azure Platform, für die Jupyter Notebooks, Visual Studio Code und das Azure Machine Learning SDK vorinstalliert sind. Da die erforderliche Software auf dem VM-Image vorinstalliert ist, können Sie schnell mit dem Experimentieren mit Azure Machine Learning beginnen, nachdem die VM erstellt wurde.

Mit der DSVM können Sie die benötigten Computeressourcen auswählen, z.B. CPU, GPU und Arbeitsspeicher. Außerdem sind noch andere Editoren vorinstalliert, z.B. PyCharm, sowie beliebte Machine Learning-Software wie TensorFlow, Keras und PyTorch. Falls die für Sie erforderliche Software nicht installiert ist, können Sie die Installation selbst durchführen.

Weitere Informationen zu den vorinstallierten Elementen finden Sie im Dokument [Einführung in Azure Data Science Virtual Machine für Linux und Windows](../data-science-virtual-machine/overview.md).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich jetzt über die Entwicklungsumgebungen informiert haben, können Sie mit dem Artikel zum [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md) fortfahren.

