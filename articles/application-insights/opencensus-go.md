---
title: OpenCensus Go-Ablaufverfolgung mit Azure Application Insights | Microsoft-Dokumentation
description: Enthält Anweisungen zur Integration der OpenCensus Go-Ablaufverfolgung in die lokale Weiterleitung und Application Insights
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/15/2018
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6262c7029a76deec2dc1169783e9cbf6311fc3f1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958270"
---
# <a name="collect-distributed-traces-from-go-preview"></a>Sammeln verteilter Ablaufverfolgungsdaten von Go (Vorschau)

Application Insights unterstützt jetzt die verteilte Ablaufverfolgung von Go-Anwendungen durch die Integration in [OpenCensus](https://opencensus.io) und unsere neue [lokale Weiterleitung](./opencensus-local-forwarder.md). Dieser Artikel führt Sie Schritt für Schritt durch den Prozess der Einrichtung von OpenCensus für Go und die Weiterleitung Ihrer Ablaufverfolgungsdaten an Application Insights.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen ein Azure-Abonnement.
- Go muss installiert sein. In diesem Artikel wird Version 1.11 verwendet ([Go herunterladen](https://golang.org/dl/)).
- Führen Sie die Anweisungen zum Installieren der [lokalen Weiterleitung als Windows-Dienst](./opencensus-local-forwarder.md#windows-service) aus.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-application-insights-resource"></a>Erstellen einer Application Insights-Ressource

Zuerst müssen Sie eine Application Insights-Ressource erstellen, die einen Instrumentierungsschlüssel (ikey) generiert. Der ikey wird dann verwendet, um die lokale Weiterleitung so zu konfigurieren, dass verteilte Ablaufverfolgungsdaten aus Ihrer mit OpenCensus instrumentierten Anwendung an Application Insights gesendet werden.   

1. Klicken Sie auf **Ressource erstellen** > **Entwicklertools** > **Application Insights**.

   ![Hinzufügen einer Application Insights-Ressource](./media/opencensus-Go/0001-create-resource.png)

   Ein Dialogfeld für die Konfiguration wird geöffnet. Füllen Sie die Eingabefelder anhand der Informationen in der folgenden Tabelle aus:

    | Einstellungen        | Wert           | BESCHREIBUNG  |
   | ------------- |:-------------|:-----|
   | **Name**      | Global eindeutiger Wert | Der Name, der die zu überwachende App identifiziert. |
   | **Anwendungstyp** | Allgemein | Der Typ der zu überwachenden App. |
   | **Ressourcengruppe**     | myResourceGroup      | Der Name der neuen Ressourcengruppe, die Application Insights-Daten hosten soll. |
   | **Location** | USA (Ost) | Wählen Sie einen Standort in Ihrer Nähe oder in der Nähe des Standorts, in dem Ihre App gehostet wird. |

2. Klicken Sie auf **Create**.

## <a name="configure-local-forwarder"></a>Konfigurieren der lokalen Weiterleitung

1. Wählen Sie **Übersicht** > **Grundlagen** aus, und kopieren Sie den**Instrumentierungsschlüssel** Ihrer Anwendung.

   ![Screenshot des Instrumentierungsschlüssels](./media/opencensus-Go/0003-instrumentation-key.png)

2. Bearbeiten Sie Ihre `LocalForwarder.config`-Datei, und fügen Sie Ihren Instrumentierungsschlüssel hinzu. Wenn Sie den Anweisungen unter [Voraussetzungen](./opencensus-local-forwarder.md#windows-service) gefolgt sind, befindet die Datei sich unter `C:\LF-WindowsServiceHost`.

    ```xml
      <OpenCensusToApplicationInsights>
        <!--
          Instrumentation key to track telemetry to.
          -->
        <InstrumentationKey>{enter-instrumentation-key}</InstrumentationKey>
      </OpenCensusToApplicationInsights>
    
      <!-- Describes aspects of processing Application Insights telemetry-->
      <ApplicationInsights>
        <LiveMetricsStreamInstrumentationKey>{enter-instrumentation-key}</LiveMetricsStreamInstrumentationKey>
      </ApplicationInsights>
    </LocalForwarderConfiguration>
    ```

3. Starten Sie den Anwendungsdienst **lokale Weiterleitung** neu.

## <a name="opencensus-go-packages"></a>OpenCensus Go-Pakete

1. Installieren der OpenCensus-Pakete für Go über die Befehlszeile:

    ```go
    go get -u go.opencensus.io
    go get -u contrib.go.opencensus.io/exporter/ocagent
    ```

2. Fügen Sie den folgenden Code zu einer GO-Datei hinzu, dann generieren Sie den Code und führen ihn aus. (Dieses Beispiel stammt aus der offiziellen OpenCensus-Richtlinie mit zusätzlichem Code, der die Integration mit der lokalen Weiterleitung erleichtert)

     ```go
        // Copyright 2018, OpenCensus Authors
        //
        // Licensed under the Apache License, Version 2.0 (the "License");
        // you may not use this file except in compliance with the License.
        // You may obtain a copy of the License at
        //
        //     http://www.apache.org/licenses/LICENSE-2.0
        //
        // Unless required by applicable law or agreed to in writing, software
        // distributed under the License is distributed on an "AS IS" BASIS,
        // WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
        // See the License for the specific language governing permissions and
        // limitations under the License.
        package main
        
        import (
        
            "bytes"
            "fmt"
            "log"
            "net/http"
            os "os"
            
            ocagent "contrib.go.opencensus.io/exporter/ocagent"
            "go.opencensus.io/plugin/ochttp"
            "go.opencensus.io/plugin/ochttp/propagation/tracecontext"
            "go.opencensus.io/trace"
        
        )
        
        func main() {
            // Register stats and trace exporters to export the collected data.
            serviceName := os.Getenv("SERVICE_NAME")
            if len(serviceName) == 0 {
                serviceName = "go-app"
            }
            fmt.Printf(serviceName)
            agentEndpoint := os.Getenv("OCAGENT_TRACE_EXPORTER_ENDPOINT")

            if len(agentEndpoint) == 0 {
                agentEndpoint = fmt.Sprintf("%s:%d", ocagent.DefaultAgentHost, ocagent.DefaultAgentPort)
            }
        
            fmt.Printf(agentEndpoint)
            exporter, err := ocagent.NewExporter(ocagent.WithInsecure(), ocagent.WithServiceName(serviceName), ocagent.WithAddress(agentEndpoint))
        
            if err != nil {
                log.Printf("Failed to create the agent exporter: %v", err)
            }
        
            trace.RegisterExporter(exporter)
        
            trace.ApplyConfig(trace.Config{DefaultSampler: trace.AlwaysSample()})
        
            client := &http.Client{Transport: &ochttp.Transport{Propagation: &tracecontext.HTTPFormat{}}}
        
            http.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
                fmt.Fprintf(w, "hello world")
        
                var jsonStr = []byte(`[ { "url": "http://blank.org", "arguments": [] } ]`)
                r, _ := http.NewRequest("POST", "http://blank.org", bytes.NewBuffer(jsonStr))
                r.Header.Set("Content-Type", "application/json")
        
                // Propagate the trace header info in the outgoing requests.
                r = r.WithContext(req.Context())
                resp, err := client.Do(r)
                if err != nil {
                    log.Println(err)
                } else {
                    // TODO: handle response
                    resp.Body.Close()
                }
            })
        
            http.HandleFunc("/call_blank", func(w http.ResponseWriter, req *http.Request) {
                fmt.Fprintf(w, "hello world")
        
                r, _ := http.NewRequest("GET", "http://blank.org", nil)

                // Propagate the trace header info in the outgoing requests.
                r = r.WithContext(req.Context())
                resp, err := client.Do(r)
        
                if err != nil {
                    log.Println(err)
                } else {
                    // TODO: handle response
                    resp.Body.Close()
                }        
            })
        
            log.Fatal(http.ListenAndServe(":50030", &ochttp.Handler{Propagation: &tracecontext.HTTPFormat{}}))
        
        }
        ```

3. Once the simple go app is running navigate to `http://localhost:50030`. Each refresh of the browser will generate the text "hello world" accompanied by corresponding span data that is picked up by the local forwarder.

4. To confirm that the **local forwarder** is picking up the traces check the `LocalForwarder.config` file. If you followed the steps in the [prerequisite](https://docs.microsoft.com/azure/application-insights/local-forwarder#windows-service), it will be located in `C:\LF-WindowsServiceHost`.

    In the image below of the log file, you can see that prior to running the second script where we added an exporter `OpenCensus input BatchesReceived` was 0. Once we started running the updated script `BatchesReceived` incremented equal to the number of values we entered:
    
    ![New App Insights resource form](./media/opencensus-go/0004-batches-received.png)

## Start monitoring in the Azure portal

1. You can now reopen the Application Insights **Overview** page in the Azure portal, to view details about your currently running application. Select **Live Metric Stream**.

   ![Screenshot of overview pane with live metric stream selected in red box](./media/opencensus-go/0005-overview-live-metrics-stream.png)

2. If you run the second Go app again and start refreshing the browser for `http://localhost:50030`, you will see live trace data as it arrives in Application Insights from the local forwarder service.

   ![Screenshot of live metric stream with performance data displayed](./media/opencensus-go/0006-stream.png)

3. Navigate back to the **Overview** page and select **Application Map** for a visual layout of the dependency relationships and call timing between your application components.

    ![Screenshot of basic application map](./media/opencensus-go/0007-application-map.png)

    Since we were only tracing one method call, our application map isn't as interesting. But application map can scale to visualize far more distributed applications:

   ![Application Map](./media/app-insights-nodejs-quick-start/application-map.png)

4. Select **Investigate Performance** to perform detailed performance analysis and determine the root cause of slow performance.

    ![Screenshot of performance pane](./media/opencensus-go/0008-performance.png)

5. Selecting **Samples** and then clicking on any of the samples that appear in the right-hand pane will launch the end-to-end transaction details experience. While our sample app will just show us a single event, a more complex application would allow you to explore the end-to-end transaction down to level of an individual event's call stack.

     ![Screenshot of end-to-end transaction interface](./media/opencensus-go/0009-end-to-end-transaction.png)

## OpenCensus trace for Go

We only covered the basics of integrating OpenCensus for Go with the local forwarder and Application Insights. The [official OpenCensus Go usage guidance](https://godoc.org/go.opencensus.io) covers more advanced topics.

## Next steps

* [Application map](./app-insights-app-map.md)
* [End-to-end performance monitoring](./app-insights-tutorial-performance.md)