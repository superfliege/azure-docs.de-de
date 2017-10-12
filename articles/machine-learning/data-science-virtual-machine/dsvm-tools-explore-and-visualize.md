---
title: "Tools zum Durchsuchen und Visualisieren von Daten – Azure | Microsoft-Dokumentation"
description: "Tools zum Durchsuchen und Visualisieren von Daten für die Data Science-VM."
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
ms.openlocfilehash: d8f0616f17dbaa02082492cc1c68f1d989ea5aae
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Auf der Data Science-VM verfügbare Tools zum Durchsuchen und Visualisieren von Daten

Bei Data Science geht es unter anderem darum, Daten zu verstehen. Hierbei helfen Tools zum Durchsuchen und Visualisieren von Daten. Im Anschluss sind einige Tools aufgeführt, die zur Vereinfachung dieses wichtigen Schritts auf der DSVM zur Verfügung stehen. 

## <a name="apache-drill"></a>Apache Drill
|    |           |
| ------------- | ------------- |
| Was ist das?   | Open Source-SQL-Abfragemodul für Big Data    |
| Unterstützte DSVM-Versionen      | Windows, Linux  |
| Konfiguration/Installation auf der DSVM      |  Installiert unter `/dsvm/tools/drill*` (nur im eingebetteten Modus)   |
| Typische Verwendung      |  Lokales Durchsuchen von Daten ohne ETL. Abfragen verschiedener Datenquellen und Formate – einschließlich CSV, JSON, relationale Tabellen und Hadoop.     |
| Verwendung/Ausführung      | Desktopverknüpfung  <br/> [Drill in 10 Minutes](https://drill.apache.org/docs/drill-in-10-minutes/) (Drill in zehn Minuten)  |
| Verwandte Tools auf der DSVM      |   Rattle, Weka, SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Was ist das?   |  Weka ist eine Sammlung von Machine Learning-Algorithmen für Data Mining. Die Algorithmen können entweder direkt auf ein Dataset angewendet oder über Ihren eigenen Java-Code aufgerufen werden. Weka enthält Tools für Datenvorverarbeitung, Klassifizierung, Regression, Clustering, Zuordnungsregeln und Visualisierung. |
| Unterstützte DSVM-Editionen     | Windows, Linux     |
| Typische Verwendung      | Allgemeines ML-Tool     |
| Verwendung/Ausführung      | Unter Windows finden Sie Weka im Startmenü. Melden Sie sich unter Linux mithilfe von X2Go an, und navigieren Sie anschließend zu „Anwendungen“ > „Entwicklung“ > „Weka“. |
| Links zu Beispielen      | [Beispiele für Weka](http://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Verwandte Tools auf der DSVM      |LightGBM, Rattle, XGBoost   |

## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| Was ist das?   |   Eine grafische Benutzeroberfläche für Data Mining unter Verwendung von R.   |
| Unterstützte DSVM-Editionen     | Windows, Linux     |
| Typische Verwendung      | Allgemeines UI-basiertes Data Mining-Tool für R    |
| Verwendung/Ausführung      | UI-Tool. Starten Sie unter Windows eine Eingabeaufforderung, und führen Sie R und `rattle()` (innerhalb von R) aus. Stellen Sie unter Linux eine Verbindung über X2Go her, starten Sie ein Terminal, und führen Sie R und `rattle()` (innerhalb von R) aus. |
| Links zu Beispielen      | [Rattle](https://togaware.com/onepager/) |
| Verwandte Tools auf der DSVM      |LightGBM, Weka, XGBoost   |

## <a name="powerbi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| Was ist das?   | Interaktives Tool für Datenvisualisierung und BI    |
| Unterstützte DSVM-Versionen      | Windows  |
| Typische Verwendung      |  Datenvisualisierung und Dashboarderstellung   |
| Verwendung/Ausführung      | Desktopverknüpfung (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Verwandte Tools auf der DSVM      |   Visual Studio 2017, Visual Studio Code, Juno      |

