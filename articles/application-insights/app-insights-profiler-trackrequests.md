---
title: Schreiben von Code zum Verfolgen von Anforderungen mit Azure Application Insights | Microsoft-Dokumentation
description: Schreiben Sie Code zum Verfolgen von Anforderungen mit Application Insights, damit Sie Profile für Ihre Anforderungen abrufen können.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 20f408d9dd32c3fd7a0e319e4051483e3aa54dd9
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2018
ms.locfileid: "52722121"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Schreiben von Code zum Verfolgen von Anforderungen mit Application Insights

Damit auf der Seite „Leistung“ Profile für Ihre Anwendung angezeigt werden, muss Application Insights Anforderungen für Ihre Anwendung verfolgen. Für Anwendungen wie ASP.NET und ASP.NET Core, die auf bereits instrumentierten Frameworks beruhen, kann Application Insights Anforderungen automatisch verfolgen. Für andere Anwendungen, wie Azure Cloud Services-Workerrollen und zustandslose Service Fabric-APIs, müssen Sie Code schreiben, um Application Insights mitzuteilen, wo Ihre Anforderungen beginnen und enden. Sobald Sie diesen Code geschrieben haben, werden Anforderungstelemetriedaten an Application Insights gesendet. Die Telemetriedaten werden auf der Seite „Leistung“ angezeigt, und es werden Profile für diese Anforderungen gesammelt. 

Im Folgenden werden die Schritte aufgeführt, die Sie zum manuellen Verfolgen von Anforderungen ausführen müssen:


  1. Fügen Sie den folgenden Code zu einem frühen Zeitpunkt der Anwendungslebensdauer hinzu:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Weitere Informationen zu dieser globalen Konfiguration des Instrumentierungsschlüssels finden Sie unter [Verwenden von Service Fabric mit Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  1. Schließen Sie zu instrumentierende Codepassagen in eine `StartOperation<RequestTelemetry>` **USING**-Anweisung ein, wie im folgenden Beispiel gezeigt:

        ```csharp
        using Microsoft.ApplicationInsights;
        using Microsoft.ApplicationInsights.DataContracts;
        ...
        var client = new TelemetryClient();
        ...
        using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
        {
          // ... Code I want to profile.
        }
        ```

        Das Aufrufen von `StartOperation<RequestTelemetry>` innerhalb eines anderen `StartOperation<RequestTelemetry>`-Bereichs wird nicht unterstützt. Sie können stattdessen `StartOperation<DependencyTelemetry>` im geschachtelten Bereich verwenden. Beispiel:   
        
        ```csharp
        using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
        {
        try
        {
          ProductDetail details = new ProductDetail() { Id = productId };
          getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();
        
          // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
          using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
          {
              double price = await _priceDataBase.GetAsync(productId);
              if (IsTooCheap(price))
              {
                  throw new PriceTooLowException(productId);
              }
              details.Price = price;
          }
        
          // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
          using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
          {
              details.Reviews = await _reviewDataBase.GetAsync(productId);
          }
        
          getDetailsOperation.Telemetry.Success = true;
          return details;
        }
        catch(Exception ex)
        {
          getDetailsOperation.Telemetry.Success = false;
        
          // This exception gets linked to the 'GetProductDetails' request telemetry.
          client.TrackException(ex);
          throw;
        }
        }
        ```
