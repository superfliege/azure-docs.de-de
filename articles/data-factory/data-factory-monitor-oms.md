---
title: Überwachen von Azure Data Factory mit OMS | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Data Factory überwachen, indem Sie Daten an Operations Management Suite (OMS) zur Analyse weiterleiten.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: douglasl
ms.openlocfilehash: fb5da75d0e42dd0700cad008d348ff846d602409
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "42146779"
---
# <a name="monitor-azure-data-factory-with-operations-management-suite-oms"></a>Überwachen von Azure Data Factory mit der Operations Management Suite (OMS)

Sie können die Azure Data Factory-Integration in Azure Monitor zur Weiterleitung von Daten an Operations Management Suite (OMS) verwenden. Diese Integration ist in den folgenden Szenarien nützlich:

1.  Sie möchten komplexe Abfragen auf einen umfangreichen Satz von Metriken schreiben, die von Data Factory an OMS veröffentlicht werden. Sie können auch eigene Benachrichtigungen zu diesen Abfragen über OMS erstellen.

2.  Sie möchten Data Factorys übergreifend überwachen. Sie können Daten aus mehreren Data Factorys an einen einzigen OMS-Arbeitsbereich weiterleiten.

Das folgende Video enthält eine siebenminütige Einführung und Demonstration dieses Features:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

## <a name="get-started"></a>Erste Schritte

### <a name="configure-diagnostic-settings-and-oms-workspace"></a>Konfigurieren von Diagnoseeinstellungen und OMS-Arbeitsbereich

Aktivieren Sie die Diagnoseeinstellungen für Ihre Data Factory.

1.  Wählen Sie **Azure Monitor** -> **Diagnoseeinstellungen** -> Wählen Sie die Data Factory aus -> Aktivieren Sie die Diagnose.

    ![monitor-oms-image1.png](media/data-factory-monitor-oms/monitor-oms-image1.png)

2.  Geben Sie die Diagnoseeinstellungen einschließlich der Konfiguration des OMS-Arbeitsbereichs an.

    ![monitor-oms-image2.png](media/data-factory-monitor-oms/monitor-oms-image2.png)

### <a name="install-azure-data-factory-analytics-oms-pack-from-azure-marketplace"></a>Installieren des OMS-Pakets für Azure Data Factory-Analysen aus dem Azure Marketplace

![monitor-oms-image3.png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![monitor-oms-image4.png](media/data-factory-monitor-oms/monitor-oms-image4.png)

Klicken Sie auf **Erstellen**, und wählen Sie den OMS-Arbeitsbereich und die OMS-Arbeitsbereichseinstellungen aus.

![monitor-oms-image5.png](media/data-factory-monitor-oms/monitor-oms-image5.png)

## <a name="monitor-azure-data-factory-metrics-using-oms"></a>Überwachen von Azure Data Factory-Metriken mit OMS

Durch die Installation des OMS-Pakets für **Azure Data Factory-Analysen** wird ein Standardsatz von Ansichten für die folgenden Metriken erstellt:

- ADF-Ausführungen – 1) Pipelineausführungen nach Data Factory

- ADF-Ausführungen – 2) Aktivitätsausführungen nach Data Factory

- ADF-Ausführungen – 3) Triggerausführungen nach Data Factory

- ADF-Fehler – 1) Top 10-Pipelinefehler nach Data Factory

- ADF-Fehler – 2) Top 10-Aktivitätsfehler nach Data Factory

- ADF-Fehler – 3) Top 10-Triggerfehler nach Data Factory

- ADF-Statistik – 1) Aktivitätsausführungen nach Typ

- ADF-Statistik – 2) Triggerausführungen nach Typ

- ADF-Statistik – 3) Höchstdauer der Pipelineausführungen

![monitor-oms-image6.png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![monitor-oms-image7.png](media/data-factory-monitor-oms/monitor-oms-image7.png)

Sie können u.a. die obigen Metriken visualisieren, die Abfragen hinter diesen Metriken betrachten, die Abfragen bearbeiten oder Benachrichtigungen erstellen.

![monitor-oms-image8.png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="next-steps"></a>Nächste Schritte

Unter [Programmgesteuertes Überwachen und Verwalten von Pipelines](https://docs.microsoft.com/en-us/azure/data-factory/monitor-programmatically) erfahren Sie mehr zum Überwachen und Verwalten von Pipelines durch das Ausführen von Skripts.
