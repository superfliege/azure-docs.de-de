---
title: "Machine Learning- und Data Science-Tools – Azure | Microsoft-Dokumentation"
description: Machine Learning- und Data Science-Tools
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
ms.openlocfilehash: c772fa16b94d09e0fc0450ce86503553c26f8a24
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="machine-learning-and-data-science-tools"></a>Machine Learning- und Data Science-Tools
Die Data Science-VM (DSVM) verfügt über ein umfangreiches Angebot an Tools und Bibliotheken für Machine Learning in gängigen Sprachen wie Python, R und Julia. 

Im Anschluss sind einige der Machine Learning-Tools und -Bibliotheken auf der DSVM aufgeführt. 

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| Was ist das?   |    Schnelle, portable und verteilte Gradient-Boosted-Bibliothek (GBDT, GBRT oder GBM) für Python, R, Java, Scala, C++ und mehr. Geeignet für einen einzelnen Computer, Hadoop und Spark.    |
| Unterstützte DSVM-Editionen     | Windows, Linux     |
| Typische Verwendung      | Allgemeine ML-Bibliothek      |
| Konfiguration/Installation auf der DSVM      |  Installiert mit GPU-Unterstützung   |
| Verwendung/Ausführung      | Als Python-Bibliothek (2.7 und 3.5), R-Paket und pfadbasiertes Befehlszeilentool (`C:\dsvm\tools\xgboost\bin\xgboost.exe` für Windows, `/dsvm/tools/xgboost/xgboost` für Linux)    |
| Links zu Beispielen      | Beispiele stehen auf dem virtuellen Computer unter `/dsvm/tools/xgboost/demo` (Linux) bzw. unter `C:\dsvm\tools\xgboost\demo` (Windows) zur Verfügung.   |
| Verwandte Tools auf der DSVM      | LightGBM, MXNet   |



## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| Was ist das?   |   Vowpal Wabbit (VW) ist eine schnelle, Open Source-basierte Out-of-Core-Lernsystembibliothek.    |
| Unterstützte DSVM-Editionen     | Windows, Linux     |
| Typische Verwendung      | Allgemeine ML-Bibliothek      |
| Konfiguration/Installation auf der DSVM      |  MSI (Windows), apt-get (Linux) |
| Verwendung/Ausführung      | Als pfadbasiertes Befehlszeilentool (`C:\Program Files\VowpalWabbit\vw.exe` unter Windows, `/usr/bin/vw` unter Linux)    |
| Links zu Beispielen      | [Beispiele für VowPal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| Verwandte Tools auf der DSVM      |LightGBM, MXNet, XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Was ist das?   |  Weka ist eine Sammlung von Machine Learning-Algorithmen für Data Mining. Die Algorithmen können entweder direkt auf ein Dataset angewendet oder über Ihren eigenen Java-Code aufgerufen werden. Weka enthält Tools für Datenvorverarbeitung, Klassifizierung, Regression, Clustering, Zuordnungsregeln und Visualisierung. |
| Unterstützte DSVM-Editionen     | Windows, Linux     |
| Typische Verwendung      | Allgemeines ML-Tool     |
| Verwendung/Ausführung      | Unter Windows finden Sie Weka im Startmenü. Melden Sie sich unter Linux mithilfe von X2Go an, und navigieren Sie anschließend zu „Anwendungen“ > „Entwicklung“ > „Weka“. |
| Links zu Beispielen      | [Beispiele für Weka](http://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Verwandte Tools auf der DSVM      |LightGBM, Rattle, XGBooost   |

## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| Was ist das?   |   Eine grafische Benutzeroberfläche für Data Mining unter Verwendung von R.   |
| Unterstützte DSVM-Editionen     | Windows, Linux     |
| Typische Verwendung      | Allgemeines UI-basiertes Data Mining-Tool für R    |
| Verwendung/Ausführung      | UI-Tool. Starten Sie unter Windows eine Eingabeaufforderung, und führen Sie R und `rattle()` (innerhalb von R) aus. Stellen Sie unter Linux eine Verbindung über X2Go her, starten Sie ein Terminal, und führen Sie R und `rattle()` (innerhalb von R) aus. |
| Links zu Beispielen      | [Rattle](https://togaware.com/onepager/) |
| Verwandte Tools auf der DSVM      |LightGBM, Weka, XGBoost   |

## <a name="lightgbm"></a>LightGBM
|    |           |
| ------------- | ------------- |
| Was ist das?   | Ein schnelles, verteiltes Gradient-Boosted-Hochleistungsframework (GBDT, GBRT, GBM oder MART) auf der Grundlage von Entscheidungsstrukturalgorithmen für Rangfolgen, Klassifizierungen und viele andere Machine Learning-Aufgaben.    |
| Unterstützte DSVM-Versionen      | Windows, Linux    |
| Typische Verwendung      | Allgemeines Gradient-Boosted-Framework      |
| Konfiguration/Installation auf der DSVM      | Unter Windows ist LightGBM als Python-Paket installiert. Unter Linux befindet sich die über die Befehlszeile ausführbare Datei unter `/opt/LightGBM/lightgbm`. Außerdem sind das R-Paket und Python-Pakete installiert.     |
| Links zu Beispielen      | [Leitfaden zu LightGBM](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| Verwandte Tools auf der DSVM      | MXNet, XGBoost  |

## <a name="h2o"></a>H2O
|    |           |
| ------------- | ------------- |
| Was ist das?   | Eine Open Source-KI-Plattform für verteiltes, schnelles und skalierbares In-Memory-Machine Learning.  |
| Unterstützte DSVM-Versionen      | Linux   |
| Typische Verwendung      | Allgemeines verteiltes und skalierbares ML   |
| Konfiguration/Installation auf der DSVM      | H2O ist unter `/dsvm/tools/h2o` installiert.      |
| Verwendung/Ausführung      | Stellen Sie mithilfe von X2Go eine Verbindung mit dem virtuellen Computer her. Starten Sie ein neues Terminal, und führen Sie `java -jar /dsvm/tools/h2o/current/h2o.jar` aus. Stellen Sie dann über einen Webbrowser eine Verbindung mit `http://localhost:54321` her.      |
| Links zu Beispielen      | Beispiele stehen auf dem virtuellen Computer in Jupyter im Verzeichnis `h2o` zur Verfügung.      |
| Verwandte Tools auf der DSVM      | Apache Spark, MXNet, XGBoost, Sparkling Water, Deep Water    |

Auf der DSVM befinden sich noch einige andere ML-Bibliotheken wie etwa das beliebte `scikit-learn`-Paket, das im Rahmen der auf der DSVM installierten Anaconda Python-Distribution bereitgestellt wird. Sehen Sie sich mithilfe der entsprechenden Paket-Manager die Liste mit den verfügbaren Paketen für Python, R und Julia an. 
