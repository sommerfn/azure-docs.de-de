---
title: Schreiben von Code zum Verfolgen von Anforderungen mit Azure Application Insights | Microsoft-Dokumentation
description: Schreiben Sie Code zum Verfolgen von Anforderungen mit Application Insights, damit Sie Profile für Ihre Anforderungen abrufen können.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 3f449c98ed44f13fb6b3849ef2457cd8fbd916de
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72900014"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Schreiben von Code zum Verfolgen von Anforderungen mit Application Insights

Damit auf der Seite „Leistung“ Profile für Ihre Anwendung angezeigt werden, muss Azure Application Insights Anforderungen für Ihre Anwendung verfolgen. Für Anwendungen, die auf bereits instrumentierten Frameworks beruhen, kann Application Insights Anforderungen automatisch verfolgen. Zwei Beispiele sind ASP.NET und ASP.NET Core. 

Für andere Anwendungen, wie Azure Cloud Services-Workerrollen und zustandslose Service Fabric-APIs, müssen Sie Code schreiben, um Application Insights mitzuteilen, wo Ihre Anforderungen beginnen und enden. Nachdem Sie diesen Code geschrieben haben, werden die Telemetriedaten der Anforderungen an Application Insights gesendet. Sie können die Telemetriedaten auf der Seite „Leistung“ anzeigen, und es werden Profile für diese Anforderungen gesammelt. 

Gehen Sie folgendermaßen vor, um Anforderungen manuell zu verfolgen:

  1. Fügen Sie den folgenden Code zu einem frühen Zeitpunkt der Anwendungslebensdauer hinzu:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Weitere Informationen zu dieser globalen Konfiguration des Instrumentierungsschlüssels finden Sie unter [Verwenden von Service Fabric mit Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  1. Schließen Sie zu instrumentierende Codepassagen in eine `StartOperation<RequestTelemetry>` **using**-Anweisung ein, wie im folgenden Beispiel gezeigt:

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
