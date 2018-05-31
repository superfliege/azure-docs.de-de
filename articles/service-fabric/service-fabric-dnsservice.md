---
title: DNS-Dienst in Azure Service Fabric | Microsoft-Dokumentation
description: Verwenden Sie den DNS-Dienst von Azure Service Fabric zum Ermitteln von Microservices aus dem Cluster heraus.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/27/2017
ms.author: msfussell
ms.openlocfilehash: 656aed1f1fbd3294c4318520058ace480fd2219c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204993"
---
# <a name="dns-service-in-azure-service-fabric"></a>DNS-Dienst in Azure Service Fabric
Der DNS-Dienst ist ein optionaler Systemdienst, den Sie in Ihrem Cluster aktivieren können, um andere Dienste mithilfe des DNS-Protokolls zu ermitteln. 

Viele Dienste, insbesondere in Containern gepackte Dienste, können über einen vorhandenen URL-Namen verfügen. Diese über das standardmäßige DNS-Protokoll (statt über das Naming Service-Protokoll) auflösen zu können, ist wünschenswert, besonders in „Lift & Shift“-Szenarien. Mit dem DNS-Dienst können Sie einem Dienstnamen DNS-Namen zuordnen und so Endpunkt-IP-Adressen auflösen. 

Der DNS-Dienst ordnet DNS-Namen den Dienstnamen zu, die wiederum vom Naming Service aufgelöst werden, um den Dienstendpunkt zurückzugeben. Der DNS-Name für den Dienst wird zum Zeitpunkt der Erstellung bereitgestellt.

![Dienstendpunkte](./media/service-fabric-dnsservice/dns.png)

Dynamische Ports werden vom DNS-Dienst nicht unterstützt. Verwenden Sie zum Auflösen der Dienste, die über dynamische Ports verfügbar gemacht werden, den [Reverseproxydienst](./service-fabric-reverseproxy.md).

## <a name="enabling-the-dns-service"></a>Aktivieren des DNS-Diensts
Wenn Sie einen Cluster mithilfe des Portals erstellen, ist der DNS-Dienst im Kontrollkästchen **DNS-Dienst einschließen** im Menü **Clusterkonfiguration** standardmäßig aktiviert:

![Aktivieren des DNS-Diensts über das Portal][2]

Wenn Sie nicht das Portal verwenden, um Ihren Cluster zu erstellen, oder wenn Sie einen vorhandenen Cluster aktualisieren, müssen Sie den DNS-Dienst in einer Vorlage aktivieren:

- Sie können für das Bereitstellen eines neuen Clusters die [Beispielvorlagen](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) verwenden oder eine Resource Manager-Vorlage erstellen. 
- Zum Aktualisieren eines vorhandenen Clusters können Sie im Portal zur Ressourcengruppe des Clusters navigieren und auf **Automatisierungsskript** klicken, um eine Vorlage zu verwenden, die den aktuellen Zustand des Clusters und anderer Ressourcen in der Gruppe widerspiegelt. Weitere Informationen finden Sie unter [Exportieren der Vorlage aus der Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template#export-the-template-from-resource-group).

Nachdem Sie die Vorlage erstellt haben, aktivieren Sie den DNS-Dienst mit den folgenden Schritten:

1. Überprüfen Sie, ob `apiversion` für die `Microsoft.ServiceFabric/clusters`-Ressource auf `2017-07-01-preview` oder höher festgelegt ist. Wenn nicht, aktualisieren Sie sie wie im folgenden Codeausschnitt:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Jetzt aktivieren Sie den DNS-Dienst wie im folgenden Codeausschnitt, indem Sie den folgenden `addonFeatures`-Abschnitt nach dem Abschnitt `fabricSettings` hinzufügen: 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. Nachdem Sie die Clustervorlage mit den vorhergehenden Änderungen aktualisiert haben, wenden Sie die Änderungen an, und lassen Sie das Upgrade fortfahren. Nach Abschluss des Upgrades wird der DNS-Systemdienst in Ihrem Cluster ausgeführt. Der Dienstname lautet `fabric:/System/DnsService`. Sie finden ihn im Service Fabric-Explorer im Dienstabschnitt **System**. 


## <a name="setting-the-dns-name-for-your-service"></a>Festlegen des DNS-Namens für den Dienst
Wenn der DNS-Dienst in Ihrem Cluster ausgeführt wird, können Sie einen DNS-Namen für Ihre Dienste entweder deklarativ für Standarddienste in `ApplicationManifest.xml` oder über PowerShell-Befehle festlegen.

Der DNS-Name für den Dienst kann im gesamten Cluster aufgelöst werden. Es wird dringend empfohlen, dass Sie ein Benennungsschema für `<ServiceDnsName>.<AppInstanceName>` verwenden, z.B. `service1.application1`. Auf diese Weise wird die Eindeutigkeit des DNS-Namens im Cluster sichergestellt. Wenn eine Anwendung mit Docker Compose bereitgestellt wird, werden den Diensten automatisch anhand dieses Benennungsschemas DNS-Namen zugewiesen.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>Festlegen des DNS-Namens für einen Standarddienst in der Datei „ApplicationManifest.xml“
Öffnen Sie das Projekt in Visual Studio oder Ihrem bevorzugten Editor, und öffnen Sie die Datei `ApplicationManifest.xml`. Navigieren Sie zum Abschnitt der Standarddienste, und fügen Sie für jeden Dienst das Attribut `ServiceDnsName` hinzu. Im folgenden Beispiel wird gezeigt, wie Sie den DNS-Namen des Diensts auf `service1.application1` festlegen.

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
Nachdem die Anwendung bereitgestellt wurde, wird der DNS-Name für die Dienstinstanz in dieser Dienstinstanz im Service Fabric Explorer wie in der folgenden Abbildung angezeigt: 

![Dienstendpunkte][1]

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a>Festlegen des DNS-Namens für einen Dienst mithilfe von Powershell
Sie können den DNS-Namen für einen Dienst bei dessen Erstellung mit dem Powershell-Befehl `New-ServiceFabricService` festlegen. Im folgenden Beispiel wird ein neuer zustandsloser Dienst namens mit dem DNS-Namen `service1.application1` erstellt.

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="using-dns-in-your-services"></a>Verwenden von DNS in Ihren Diensten
Wenn Sie mehr als einen Dienst bereitstellen, finden Sie die Endpunkte der Dienste, mit denen Sie kommunizieren möchten, über einen DNS-Namen. Der DNS-Dienst ist nur für zustandslose Dienste verfügbar, da das DNS Protokoll mit zustandsbehafteten Diensten nicht kommunizieren kann. Bei zustandsbehafteten Diensten können Sie den integrierten [Reverseproxydienst](./service-fabric-reverseproxy.md) für HTTP-Aufrufe verwenden, um eine bestimmte Dienstpartition aufzurufen. Dynamische Ports werden vom DNS-Dienst nicht unterstützt. Sie können den Reverseproxydienst verwenden, um Dienste aufzulösen, die dynamische Ports verwenden.

Der folgende Code zeigt, wie Sie einen anderen Dienst aufrufen. Dabei handelt es sich einfach um einen regulären HTTP-Aufruf, bei dem Sie den Port und ggf. einen optionalen Pfad als Bestandteil der URL angeben.

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über Dienstkommunikation innerhalb des Clusters unter [Herstellung einer Verbindung mit Diensten und Kommunikation mit diesen Diensten](service-fabric-connect-and-communicate-with-services.md).

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
