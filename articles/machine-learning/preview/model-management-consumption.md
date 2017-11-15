---
title: Nutzung eines Azure Machine Learning-Modells mithilfe der Modellverwaltung als Webdienst | Microsoft-Dokumentation
description: Dieses Dokument beschreibt die Schritte und Konzepte zur Nutzung von Webdiensten, die mithilfe der Azure Machine Learning-Modellverwaltung implementiert werden.
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ms.openlocfilehash: 39023712f72d5ac874de1f20a110bef9703ed5e8
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="consuming-web-services"></a>Nutzen von Webdiensten
Sobald Sie ein Modell als in Echtzeit arbeitenden Webdienst bereitgestellt haben, können Sie Daten an ihn senden und Vorhersagen von verschiedenen Plattformen und Anwendungen abrufen. Der in Echtzeit arbeitende Webdienst macht eine REST-API zum Abrufen von Vorhersagen verfügbar. Sie können Daten im ein- oder mehrzeiligen Format an den Webdienst senden, um eine oder mehrere Vorhersagen gleichzeitig zu erhalten.

Bei Nutzung eines [Azure Machine Learning-Webdiensts](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy) kommuniziert eine externe Anwendung synchron mit einem Vorhersagemodell, indem ein HTTP POST-Aufruf an die Dienst-URL gerichtet wird. Um einen Webdienst aufrufen zu können, muss die Clientanwendung den API-Schlüssel angeben, der beim Bereitstellen einer Vorhersage erstellt wird, und die Anforderungsdaten in den POST-Anforderungstext einfügen.

Beachten Sie, dass API-Schlüssel nur im Modus für die Bereitstellung in einem Cluster verfügbar sind. Lokale Webdienste haben keine Schlüssel.

## <a name="service-deployment-options"></a>Dienstbereitstellungsoptionen
Azure Machine Learning-Webdienste können mithilfe des Docker-Moduls in cloudbasierten Clustern sowohl für Produktions- und Testszenarien als auch auf lokalen Arbeitsstationen bereitgestellt werden. Die Funktionalität des Vorhersagemodells ist in beiden Fällen gleich. Die clusterbasierte Bereitstellung bietet skalierbare und leistungsstarke Lösungen auf Basis von Azure Container Services, während die lokale Bereitstellung für das Debugging genutzt werden kann. 

Die Azure Machine Learning-CLI und -API bieten komfortable Befehle zum Erstellen und Verwalten von Compute-Umgebungen für Dienstimplementierungen mithilfe der Option ```az ml env```. 

## <a name="list-deployed-services-and-images"></a>Auflisten der bereitgestellten Dienste und Images
Sie können die derzeit bereitgestellten Dienste und Docker-Images mithilfe des CLI-Befehls ```az ml service list realtime -o table``` auflisten. Beachten Sie, dass dieser Befehl stets für die aktuelle Compute-Umgebung ausgeführt wird und keine Dienste anzeigt, die in einer Umgebung bereitgestellt sind, die nicht als aktuell festgelegt ist. Legen Sie die Umgebung mit ```az ml env set``` fest. 

## <a name="get-service-information"></a>Abrufen von Dienstinformationen
Nachdem der Webdienst erfolgreich bereitgestellt wurde, verwenden Sie den folgenden Befehl, um die Dienst-URL und andere Details für das Aufrufen des Dienstendpunkts abzurufen. 

```
az ml service usage realtime -i <service name>
```

Dieser Befehl gibt die Dienst-URL, erforderlichen Anforderungsheader, Swagger-URL und Beispieldaten für den Aufruf des Diensts aus, wenn das API-Schema des Diensts zur Bereitstellungszeit bereitgestellt wurde.

Sie können den Dienst mithilfe der CLI direkt testen, ohne eine HTTP-Anforderung zusammenzustellen, indem Sie den CLI-Beispielbefehl mit den Eingabedaten eingeben:

```
az ml service run realtime -i <service name> -d "Your input data"
```

## <a name="get-the-service-api-key"></a>Abrufen des API-Schlüssels des Diensts
Um dem Webdienstschlüssel abzurufen, geben Sie den folgenden Befehl an:

```
az ml service keys realtime -i <web service id>
```
Verwenden Sie beim Erstellen einer HTTP-Anforderung den Schlüssel im Autorisierungsheader: "Authorization": "Bearer <key>"

## <a name="get-the-service-swagger-description"></a>Abrufen der Swagger-Beschreibung des Diensts
Wenn das API-Schema des Diensts angegeben wurde, macht der Dienstendpunkt ein Swagger-Dokument unter ```http://<ip>/api/v1/service/<service name>/swagger.json``` verfügbar. Das Swagger-Dokument kann verwendet werden, um den Dienstclient automatisch zu generieren und die erwarteten Eingabedaten und andere Details über den Service zu untersuchen.

## <a name="get-service-logs"></a>Abrufen von Dienstprotokollen
Es gibt mehrere Möglichkeiten zum Abrufen von Dienstprotokollen, um das Dienstverhalten zu verstehen und Probleme zu diagnostizieren:
- CLI-Befehl ```az ml service logs realtime -i <service id>```. Dieser Befehl funktioniert im Cluster- und lokalen Modus.
- Wenn die Dienstprotokollierung bei der Bereitstellung aktiviert wurde, werden der Dienstprotokolle auch an Application Insights gesendet werden. Der CLI-Befehl ```az ml service usage realtime -i <service id>``` zeigt die Application Insights-URL an. Beachten Sie, dass die Application Insights-Protokolle mit einer Verzögerung von 2 bis 5 Minuten angezeigt werden.
- Clusterprotokolle können über die Kubernetes-Konsole angezeigt werden, mit der beim Festlegen der aktuellen Clusterumgebung mit ```az ml env set``` eine Verbindung hergestellt wird.
- Lokale Docker-Protokolle sind in den Protokollen des Docker-Moduls verfügbar, sollte der Dienst lokal ausgeführt werden.

## <a name="call-the-service-using-c"></a>Aufrufen des Diensts mithilfe von C#
Verwenden Sie die Dienst-URL zum Senden einer Anforderung aus einer C#-Konsolenanwendung. 

1. Erstellen Sie in Visual Studio eine neue Konsolenanwendung: 
    * Klicken Sie auf „Datei > Neu > Projekt“.
    * Klicken Sie unter „Visual Studio C#“ auf „Windows-Klasse > Desktop“, und wählen Sie dann „Konsolen-App“ aus.
2. Geben Sie _MyFirstService_ als Namen des Projekts ein, und klicken Sie dann auf „OK“.
3. Legen Sie in „Projektverweise“ Verweise auf _System.Net_ und _System.Net.Http_ fest.
4. Klicken Sie auf „Extras > NuGet-Paket-Manager > Paket-Manager-Konsole“, und installieren Sie dann das Paket „Microsoft.AspNet.WebApi.Client“.
5. Öffnen Sie die Datei „Program.cs“, und ersetzen Sie den Code durch folgenden Code:
6. Aktualisieren Sie die Parameter _SERVICE_URL_ und _API_KEY_ mit den Informationen aus Ihrem Webdienst.
7. Führen Sie das Projekt aus.

```csharp
using System;
using System.Collections.Generic;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MyFirstService
{
    class Program
    {
        // Web Service URL (update it with your service url)
        private const string SERVICE_URL = "http://<service ip address>:80/api/v1/service/<service name>/score";
        private const string API_KEY = "your service key";

        static void Main(string[] args)
        {
            Program.PostRequest();
            Console.ReadLine();
        }

        private static void PostRequest()
        {
            HttpClient client = new HttpClient();
            client.BaseAddress = new Uri(SERVICE_URL);
            //For local web service, comment out this line.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", API_KEY);

            var inputJson = new List<RequestPayload>();
            RequestPayload payload = new RequestPayload();
            List<InputDf> inputDf = new List<InputDf>();
            inputDf.Add(new InputDf()
            {
                feature1 = value1,
                feature2 = value2,
            });
            payload.Input_df_list = inputDf;
            inputJson.Add(payload);

            try
            {
                var request = new HttpRequestMessage(HttpMethod.Post, string.Empty);
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;

                Console.Write(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
        public class InputDf
        {
            public double feature1 { get; set; }
            public double feature2 { get; set; }
        }
        public class RequestPayload
        {
            [JsonProperty("input_df")]
            public List<InputDf> Input_df_list { get; set; }
        }
    }
}
```

## <a name="call-the-web-service-using-python"></a>Aufrufen des Webdiensts mithilfe von Python
Verwenden Sie Python, um in Echtzeit eine Anforderung an den Webdienst zu senden. 

1. Kopieren Sie das folgende Codebeispiel in eine neue Python-Datei.
2. Aktualisieren Sie die Parameter „data“, „url“ und „api_key“. Entfernen Sie für lokale Webdienste die „Authorization“-Header.
3. Führen Sie den Code aus. 

```python
import requests
import json

data = "{\"input_df\": [{\"feature1\": value1, \"feature2\": value2}]}"
body = str.encode(json.dumps(data))

url = 'http://<service ip address>:80/api/v1/service/<service name>/score'
api_key = 'your service key' 
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

resp = requests.post(url, data, headers=headers)
resp.text
```
