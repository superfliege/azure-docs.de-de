---
title: Installieren von Azure Data Lake Tools für Visual Studio
description: In diesem Artikel erfahren Sie, wie Sie Azure Data Lake Tools für Visual Studio installieren.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.topic: conceptual
ms.date: 05/03/2018
ms.openlocfilehash: c520c437212c23cc9dc8327c95b9f2a77b08e1ac
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34622869"
---
# <a name="install-data-lake-tools-for-visual-studio"></a>Installieren von Data Lake-Tools für Visual Studio

Erfahren Sie, wie Sie mit Visual Studio Azure Data Lake Analytics-Konten erstellen, Aufträge in [U-SQL](data-lake-analytics-u-sql-get-started.md) definieren und Aufträge an den Data Lake Analytics-Dienst übermitteln. Weitere Informationen zu Data Lake Analytics finden Sie unter [Übersicht über Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* **Visual Studio:** alle Editionen außer Express werden unterstützt.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **Microsoft Azure SDK für .NET**, Version 2.7.1 oder höher.  Führen Sie die Installation mit dem [Webplattform-Installer](http://www.microsoft.com/web/downloads/platform.aspx)durch.
* Ein **Data Lake Analytics**-Konto. Informationen zum Erstellen eines Kontos finden Sie unter [Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio-2017"></a>Installieren von Azure Data Lake Tools für Visual Studio 2017

Azure Data Lake Tools für Visual Studio wird in Visual Studio 2017 ab Version 15.3 unterstützt. Das Tool ist Teil der Workloads **Datenspeicherung und -verarbeitung** und **Azure-Entwicklung** im Visual Studio-Installer. Aktivieren Sie eine dieser beiden Workloads im Rahmen Ihrer Visual Studio-Installation.  

Aktivieren Sie die Workload **Datenspeicherung und -verarbeitung** wie folgt: ![Aktivieren der Workload „Datenspeicherung und -verarbeitung“](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-01.png)

Aktivieren Sie die Workload **Azure-Entwicklung** wie folgt: ![Aktivieren der Workload „Azure-Entwicklung“](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-02.png)

## <a name="install-azure-data-lake-tools-for-visual-studio-2013-and-2015"></a>Installieren von Azure Data Lake Tools für Visual Studio 2013 und 2015

Sie können Azure Data Lake Tools für Visual Studio [über das Download Center](http://aka.ms/adltoolsvs) herunterladen und installieren. Beachten Sie nach der Installation Folgendes:
* Der Knoten **Server-Explorer** > **Azure** enthält den Knoten **Data Lake Analytics**. 
* Das Menü **Extras** enthält die Option **Data Lake**.


## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Protokollieren von Diagnoseinformationen finden Sie unter [Accessing diagnostic logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Weitere Informationen zur Verwendung der Scheitelpunktausführungsansicht finden Sie unter [Verwenden der Scheitelpunktausführungsansicht in Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
