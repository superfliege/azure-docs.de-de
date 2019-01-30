---
title: Visualisieren der Azure IoT Central-Daten in einem Power BI-Dashboard | Microsoft-Dokumentation
description: Verwenden Sie die Power BI-Lösungsvorlage von Azure IoT Central Analytics zum Visualisieren und Analysieren Ihrer IoT Central-Daten.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/16/2018
ms.topic: conceptual
ms.openlocfilehash: a37eb68d135d156c25c58bf91489d5b50ac2c170
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54423119"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Visualisieren und Analysieren der Azure IoT Central-Daten in einem Power BI-Dashboard

*Dieses Thema gilt für Administratoren.*

![Pipeline der Power BI-Lösungsvorlage](media/howto-connect-powerbi/iot-continuous-data-export.png)

Verwenden Sie die Power BI-Lösungsvorlage von Azure IoT Central Analytics, um ein leistungsfähiges Power BI-Dashboard zur Überwachung der Leistung Ihrer IoT-Geräte zu erstellen. In Ihrem Power BI-Dashboard können Sie folgende Aktionen ausführen:
- Nachverfolgen, wie viele Daten im Laufe der Zeit von Ihren Geräten gesendet werden
- Vergleichen der Datenmenge zwischen Telemetrie, Status und Ereignissen
- Identifizieren von Geräten, die viele Messungen melden
- Beobachten von historischen Trends der Gerätemessungen
- Identifizieren von problematischen Geräten, die viele kritische Ereignisse senden

Diese Lösungsvorlage richtet die Pipeline ein, mit der die Daten in Ihrem Azure Blob Storage-Konto aus dem [Fortlaufenden Datenexport](howto-export-data.md) verwendet werden. Diese Daten fließen zum Verarbeiten und Transformieren zu Azure Functions, Azure Data Factory und Azure SQL-Datenbank. Die Ausgabe kann in Power BI-Berichten, die Sie als PBIX-Datei herunterladen können, visuell dargestellt und analysiert werden. All diese Ressourcen werden in Ihrem Azure-Abonnement erstellt, damit Sie jede Komponente nach Ihren Anforderungen anpassen können. Diese Lösungsvorlage wird vollständig aus Open Source-Code erstellt. Sie können also mehr über die Architektur erfahren und die Lösung erweitern, indem Sie das [GitHub-Repository](https://aka.ms/iotcentralgithubpowerbisolutiontemplate) besuchen.

## <a name="get-the-azure-iot-central-analytics-solution-templatehttpsbpsolutiontemplatescomnamemicrosoft-iotcontinuousdataexporttemplate-from-microsoft-appsource"></a>Abrufen der [Azure IoT Central Analytics-Lösungsvorlage](https://bpsolutiontemplates.com/?name=Microsoft-IoTContinuousDataExportTemplate) von Microsoft AppSource

## <a name="prerequisites"></a>Voraussetzungen
Zum Einrichten der Vorlage benötigen Sie Folgendes:
- Zugriff auf ein Azure-Abonnement
- Mit [Fortlaufendem Datenexport](howto-export-data.md) exportierte Daten aus Ihrer IoT Central-App. Es wird empfohlen, dass Sie Datenströme zu Messungen, Geräten und Gerätevorlagen aktivieren, um das Power BI-Dashboard optimal zu nutzen.
- Power BI Desktop (neueste Version)
- Power BI Pro (wenn Sie das Dashboard für andere Benutzer freigeben möchten)

## <a name="reports"></a>Berichte

Zwei Berichte werden automatisch generiert. 

Der erste Bericht zeigt eine historische Ansicht der Messungen, die von Geräten gemeldet wurden. Er schlüsselt die verschiedenen Typen von Messungen und Geräten auf, die die höchste Anzahl von Messungen gesendet haben.

![Seite 1 des Power BI-Berichts](media/howto-connect-powerbi/template-page1-hasdata.PNG)

Der zweite Bericht zeigt mehr Details zu Ereignissen und eine historische Ansicht der gemeldeten Fehler und Warnungen. Er zeigt auch, welche Geräte die höchste Anzahl von Ereignissen insgesamt melden, sowie speziell Fehlerereignisse und Warnungsereignisse.

![Seite 2 des Power BI-Berichts](media/howto-connect-powerbi/template-page2-hasdata.PNG)

## <a name="resources"></a>Ressourcen

Besuchen Sie AppSource, um die [Azure IoT Central Analytics-Lösungsvorlage](https://bpsolutiontemplates.com/?name=Microsoft-IoTContinuousDataExportTemplate) abzurufen.

Besuchen Sie das [GitHub-Repository](https://aka.ms/iotcentralgithubpowerbisolutiontemplate), um mehr über die Architektur zu erfahren und die Lösung zu erweitern.

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun wissen, wie Sie Ihre Daten in Power BI visualisieren können, empfiehlt sich als nächster Schritt:

> [!div class="nextstepaction"]
> [Verwalten von Geräten](howto-manage-devices.md)
