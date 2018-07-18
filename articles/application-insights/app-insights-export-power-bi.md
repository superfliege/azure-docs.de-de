---
title: Exportieren nach Power BI aus Azure Application Insights | Microsoft-Dokumentation
description: Analytics-Abfragen können in Power BI angezeigt werden.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/18/2016
ms.author: mbullwin
ms.openlocfilehash: dee3313082fbe75d76bf27105979cf7e869fafad
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294121"
---
# <a name="feed-power-bi-from-application-insights"></a>Datenimport nach Power BI aus Application Insights
[Power BI](http://www.powerbi.com/) ist eine Suite aus Unternehmenstools, mit denen Sie Daten analysieren und Informationen teilen können. Auf jedem Gerät stehen leistungsfähige Dashboards zur Verfügung. Sie können Daten aus vielen Quellen kombinieren, z.B. Analytics-Abfragen aus [Azure Application Insights](app-insights-overview.md).

Es gibt drei empfohlene Methoden zum Exportieren von Application Insights-Daten nach Power BI. Diese können Sie separat oder zusammen verwenden.

* [**Power BI-Adapter**](#power-pi-adapter). Richten Sie ein vollständiges Dashboard mit den Telemetriedaten Ihrer App ein. Die Gruppe von Diagrammen ist vordefiniert, aber Sie können auch eigene Abfragen aus anderen Quellen hinzufügen.
* [**Exportieren von Analytics-Abfragen**](#export-analytics-queries). Schreiben Sie eine beliebige Abfrage, und exportieren Sie sie in Power BI. Schreiben Sie mit Analytics oder über die Verwendungstrichter eine beliebige Abfrage. Sie können diese Abfrage zusammen mit anderen Daten in einem Dashboard anordnen.
* [**Fortlaufender Export und Azure Stream Analytics**](app-insights-export-stream-analytics.md). Diese Methode ist hilfreich, wenn Sie Ihre Daten über längere Zeiträume aufbewahren möchten. Verwenden Sie andernfalls eine der anderen Methoden, da für diese Methode mehr Einrichtungsaufwand anfällt.

## <a name="power-bi-adapter"></a>Power BI-Adapter
Mit dieser Methode wird für Sie ein vollständiges Dashboard mit Telemetriedaten erstellt. Das anfängliche Dataset ist vordefiniert, aber Sie können weitere Daten hinzufügen.

### <a name="get-the-adapter"></a>Abrufen des Adapters
1. Melden Sie sich bei [Power BI](https://app.powerbi.com/) an.
2. Öffnen Sie **Daten abrufen**, **Dienste** und dann **Application Insights**.
   
    ![Screenshots für das Abrufen aus der Application Insights-Datenquelle](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. Geben Sie die Details Ihrer Application Insights-Ressource an.
   
    ![Screenshot für das Abrufen aus der Application Insights-Datenquelle](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. Warten Sie ein oder zwei Minuten, bis die Daten importiert wurden.
   
    ![Screenshot des Power BI-Adapters](./media/app-insights-export-power-bi/010.png)

Sie können das Dashboard bearbeiten und die Application Insights-Diagramme mit den Diagrammen anderer Quellen und mit Analytics-Abfragen kombinieren. Es gibt einen Katalog mit visuellen Elementen, in dem weitere Diagramme bereitgestellt werden. Jedes dieser Diagramme verfügt über Parameter, die Sie festlegen können.

Nach dem anfänglichen Import werden das Dashboard und die Berichte täglich aktualisiert. Sie können den Aktualisierungszeitplan im Dataset steuern.

## <a name="export-analytics-queries"></a>Exportieren von Analytics-Abfragen
Mit dieser Route können Sie eine beliebige Analytics-Abfrage schreiben oder aus Verwendungstrichtern exportieren und dann in ein Power BI-Dashboard exportieren. (Sie können dem mit dem Adapter erstellten Dashboard Elemente hinzufügen.)

### <a name="one-time-install-power-bi-desktop"></a>Einmalig: Installieren von Power BI Desktop
Zum Importieren Ihrer Application Insights-Abfrage verwenden Sie die Desktopversion von Power BI. Diese können Sie dann im Web oder in Ihrem Power BI-Cloudarbeitsbereich veröffentlichen. 

Installieren Sie [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Exportieren einer Analytics-Abfrage
1. [Öffnen Sie Analytics, und schreiben Sie Ihre Abfrage](app-insights-analytics-tour.md).
2. Testen und optimieren Sie die Abfrage, bis Sie mit den Ergebnissen zufrieden sind. Stellen Sie sicher, dass die Abfrage ordnungsgemäß in Analytics ausgeführt wird, bevor Sie sie exportieren.
3. Wählen Sie im Menü **Exportieren** die Option **Power BI (M)**. Speichern Sie die Textdatei.
   
    ![Screenshot von Analytics mit hervorgehobenem Menü „Exportieren“](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. Klicken Sie in Power BI Desktop auf **Daten abrufen** > **Leere Abfrage**. Klicken Sie dann im Abfrage-Editor unter **Ansicht** auf **Erweiterter Editor**.

    Fügen Sie das exportierte M-Sprachskript in den erweiterten Editor ein.

    ![Screenshot von Power BI Desktop mit hervorgehobenem erweiterten Editor](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. Unter Umständen müssen Sie Anmeldeinformationen angeben, um für Power BI den Zugriff auf Azure zuzulassen. Verwenden Sie die Option **Organisationskonto**, um sich mit Ihrem Microsoft-Konto anzumelden.
   
    ![Screenshot des Power BI-Dialogfelds für Abfrageeinstellungen](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    Wenn Sie die Anmeldeinformationen überprüfen müssen, verwenden Sie den Menübefehl **Datenquelleneinstellungen** im Abfrage-Editor. Sie müssen unbedingt Ihre Anmeldeinformationen für Azure verwenden, die sich von denen für Power BI möglicherweise unterscheiden.
2. Wählen Sie eine Visualisierung für Ihre Abfrage aus, und wählen Sie die Felder für X-Achse, Y-Achse und Segmentierungsdimension.
   
    ![Screenshot der Visualisierungsoptionen von Power BI Desktop](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. Veröffentlichen Sie den Bericht in Ihrem Power BI-Cloudarbeitsbereich. Von hier aus können Sie eine synchronisierte Version in andere Webseiten einbetten.
   
    ![Screenshot von Power BI Desktop mit hervorgehobener Schaltfläche „Veröffentlichen“](./media/app-insights-export-power-bi/publish-power-bi.png)
4. Aktualisieren Sie den Bericht in bestimmten Abständen manuell, oder richten Sie auf der Seite mit den Optionen eine geplante Aktualisierung ein.

### <a name="export-a-funnel"></a>Exportieren eines Trichters
1. [Erstellen Sie Ihren Trichter.](usage-funnels.md)
2. Wählen Sie **Power BI**aus. 

   ![Screenshot der Power BI-Schaltfläche](./media/app-insights-export-power-bi/button.png)
   
3. Klicken Sie in Power BI Desktop auf **Daten abrufen** > **Leere Abfrage**. Klicken Sie dann im Abfrage-Editor unter **Ansicht** auf **Erweiterter Editor**.

   ![Screenshot von Power BI Desktop mit hervorgehobener Schaltfläche „Leere Abfrage“](./media/app-insights-export-power-bi/blankquery.png)

   Fügen Sie das exportierte M-Sprachskript in den erweiterten Editor ein. 

   ![Screenshot von Power BI Desktop mit hervorgehobenem erweiterten Editor](./media/app-insights-export-power-bi/advancedquery.png)

4. Wählen Sie Elemente aus der Abfrage und eine Trichtervisualisierung aus.

   ![Screenshot der Visualisierungsoptionen von Power BI Desktop](./media/app-insights-export-power-bi/selectsequence.png)

5. Geben Sie einen aussagekräftigen Titel an, und veröffentlichen Sie den Bericht in Ihrem Power BI-Cloudarbeitsbereich. 

   ![Screenshot von Power BI Desktop mit hervorgehobenem Titel](./media/app-insights-export-power-bi/changetitle.png)

## <a name="troubleshooting"></a>Problembehandlung

Es können Fehler in Bezug auf die Anmeldeinformationen oder die Größe des Datasets auftreten. Hier finden Sie einige Informationen zum Beheben dieser Fehler.

### <a name="unauthorized-401-or-403"></a>Nicht autorisiert (401 oder 403)
Dieser Fall kann eintreten, wenn Ihr Aktualisierungstoken nicht aktualisiert wurde. Probieren Sie diese Schritte aus, um sicherzustellen, dass Sie weiterhin Zugriff haben:

1. Melden Sie sich beim Azure-Portal an, und stellen Sie sicher, dass Sie auf die Ressource zugreifen können.
2. Versuchen Sie, die Anmeldeinformationen für das Dashboard zu aktualisieren.

 Falls Sie Zugriff haben und die Aktualisierung der Anmeldeinformationen das Problem nicht behebt, erstellen Sie ein Supportticket.

### <a name="bad-gateway-502"></a>Ungültiges Gateway (502)
Dies wird normalerweise durch eine Analytics-Abfrage verursacht, die zu viele Daten zurückgibt. Versuchen Sie es mit einem kleineren Zeitbereich für die Abfrage. 

Wenn das Verkleinern des von der Analytics-Abfrage stammenden Datasets Ihre Anforderungen nicht erfüllt, sollten Sie erwägen, mithilfe der [API](https://dev.applicationinsights.io/documentation/overview) ein größeres Dataset abzurufen. So wird der M-Query-Export zum Verwenden der API konvertiert:

1. Erstellen Sie einen [API-Schlüssel](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).
2. Aktualisieren Sie das M-Skript von Power BI, das Sie aus Analytics exportiert haben, indem Sie die Azure Resource Manager-URL durch die Application Insights-API ersetzen.
   * Ersetzen Sie **https://management.azure.com/subscriptions/...**
   * durch  **https://api.applicationinsights.io/beta/apps/...**
3. Aktualisieren Sie schließlich die Anmeldeinformationen in „basic“, und verwenden Sie Ihren API-Schlüssel.
  

**Vorhandenes Skript**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Aktualisiertes Skript**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>Informationen zur Stichprobenerstellung (Sampling)
Wenn für Ihre Anwendung große Datenmengen gesendet werden, sollten Sie ggf. die Funktion für die adaptive Stichprobenerstellung verwenden, bei der nur ein prozentualer Anteil der Telemetriedaten gesendet. Dasselbe gilt, wenn Sie die Stichprobenerstellung entweder im SDK oder bei der Erfassung manuell festgelegt haben. [Erfahren Sie mehr über Sampling](app-insights-sampling.md).


## <a name="next-steps"></a>Nächste Schritte
* [Power BI – Informationen](http://www.powerbi.com/learning/)
* [Analytics-Tutorial](app-insights-analytics-tour.md)

