---
title: Integrieren von Azure Service Fabric in API Management | Microsoft-Dokumentation
description: Lernen Sie die ersten Schritte bei Azure API Management und Service Fabric kennen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/13/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 1cadb543660b636ce7f0285973e6fb4141841b99
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="deploy-api-management-with-service-fabric"></a>Bereitstellen von API Management mit Service Fabric
Dieses Tutorial ist der zweite Teil einer Reihe. In diesem Tutorial wird beschrieben, wie Sie [Azure API Management](../api-management/api-management-key-concepts.md) mit Service Fabric einrichten, um Datenverkehr an einen Back-End-Dienst in Service Fabric weiterzuleiten.  Am Ende des Tutorials haben Sie API Management in einem VNET bereitgestellt und einen API-Vorgang zum Senden von Datenverkehr an zustandslose Back-End-Dienste konfiguriert. Weitere Informationen zu den Azure API Management-Szenarios mit Service Fabric finden Sie im [Übersichtsartikel](service-fabric-api-management-overview.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereitstellen von API Management
> * Konfigurieren von API Management
> * Erstellen eines API-Vorgangs
> * Konfigurieren Sie eine Back-End-Richtlinie
> * Hinzufügen der API zu einem Produkt

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines sicheren [Windows-Clusters](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oder [Linux-Clusters](service-fabric-tutorial-create-vnet-and-linux-cluster.md) in Azure mithilfe einer Vorlage
> * Bereitstellen von API Management mit Service Fabric

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Installieren Sie das [Azure PowerShell-Modul Version 4.1 oder höher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) oder [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Erstellen Sie einen sicheren [Windows-Cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oder [Linux-Cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) in Azure.

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Anmelden bei Azure und Auswählen Ihres Abonnements
In diesem Tutorial wird [Azure PowerShell][azure-powershell] verwendet. Wenn Sie eine neue PowerShell-Sitzung starten, melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus, bevor Sie Azure-Befehle ausführen.
 
Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus:

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="deploy-api-management"></a>Bereitstellen von API Management
Cloudanwendungen benötigen normalerweise ein Front-End-Gateway, um für Benutzer, Geräte oder andere Anwendungen einen zentralen Eingangspunkt bereitzustellen. In Service Fabric kann ein Gateway ein beliebiger zustandsloser Dienst sein, z.B. eine ASP.NET Core-Anwendung, oder ein anderer Dienst, der für den Eingang von Datenverkehr ausgelegt ist, z.B. Event Hubs, IoT Hub oder Azure API Management. Dieses Tutorial bietet eine Einführung in die Verwendung von Azure API Management als Gateway für Ihre Service Fabric-Anwendungen. API Management ist direkt in Service Fabric integriert, sodass Sie APIs mit einem umfassenden Satz von Routingregeln für Ihre Service Fabric-Back-End-Dienste veröffentlichen können. 

Nach dem Erstellen eines sicheren [Windows-Clusters](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oder [Linux-Clusters](service-fabric-tutorial-create-vnet-and-linux-cluster.md) in Azure stellen Sie API Management für das virtuelle Netzwerk (VNET) in dem Subnetz und der NSG bereit, die für API Management festgelegt wurden. Für dieses Tutorial ist die API Management-Resource Manager-Vorlage so vorkonfiguriert, dass die Namen des VNET, des Subnetzes und der NSG verwendet werden, die im vorherigen [Tutorial zum Windows-Cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oder [Tutorial zum Linux-Cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) eingerichtet wurden. 

Laden Sie die folgende Resource Manager-Vorlage und -Parameterdatei herunter:
 
- [apim.json][apim-arm]
- [apim.parameters.json][apim-parameters-arm]

Füllen Sie die leeren Parameter in der `apim.parameters.json` für Ihre Bereitstellung aus.

Verwenden Sie das folgende Skript, um die Resource Manager-Vorlagendatei und die Parameterdatei für API Management bereitzustellen:

```powershell
$ResourceGroupName = "tutorialgroup"
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
```

```azurecli
ResourceGroupName="tutorialgroup"
az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json 
```

## <a name="configure-api-management"></a>Konfigurieren von API Management

Nachdem Ihre API Management- und Service Fabric-Cluster bereitgestellt wurden, können Sie Sicherheitseinstellungen und ein Service Fabric-Back-End in API Management konfigurieren. Dadurch können Sie eine Back-End-Dienstrichtlinie erstellen, die Datenverkehr an Ihren Service Fabric-Cluster sendet.

### <a name="configure-api-management-security"></a>Konfigurieren der API Management-Sicherheit

Zum Konfigurieren des Service Fabric-Back-Ends müssen Sie zuerst die API Management-Sicherheitseinstellungen konfigurieren. Wechseln Sie zum Konfigurieren der Sicherheitseinstellungen zu Ihrem API Management-Dienst im Azure-Portal.

#### <a name="enable-the-api-management-rest-api"></a>Aktivieren der API Management-REST-API

Die API Management-REST-API ist derzeit die einzige Möglichkeit zum Konfigurieren eines Back-End-Diensts. Der erste Schritt ist das Aktivieren und Sichern der API Management-REST-API.

 1. Wählen Sie im API Management-Dienst unter **Sicherheit** die Option **Management API** aus.
 2. Aktivieren Sie das Kontrollkästchen **API Management-REST-API aktivieren**.
 3. Notieren Sie sich die **URL der Verwaltungs-API** – dies ist die URL, die später zur Einrichtung des Service Fabric-Back-Ends verwendet wird.
 4. Generieren Sie ein **Zugriffstoken**, indem Sie ein Ablaufdatum und einen Schlüssel auswählen, und klicken Sie dann auf die Schaltfläche **Generieren** unten auf der Seite.
 5. Kopieren Sie das **Zugriffstoken**, und speichern Sie es.  Dieses Zugriffstoken wird in den folgenden Schritten verwendet. Beachten Sie, dass sich dies vom primären und sekundären Schlüssel unterscheidet.

#### <a name="upload-a-service-fabric-client-certificate"></a>Hochladen eines Service Fabric-Clientzertifikats

API Management muss bei Ihrem Service Fabric-Cluster mit Clientzertifikaten, die über Zugriff auf den Cluster verfügen, für die Dienstermittlung authentifiziert werden. Der Einfachheit halber wird in diesem Tutorial das gleiche Zertifikat verwendet, das zuvor beim Erstellen des [Windows-Clusters](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor) oder [Linux-Clusters](service-fabric-tutorial-create-vnet-and-linux-cluster.md#createvaultandcert_anchor) angegeben wurde und das standardmäßig für den Zugriff auf Ihren Cluster verwendet werden kann.

 1. Wählen Sie im API Management-Dienst unter **Sicherheit** die Option **Clientzertifikate** aus.
 2. Klicken Sie auf die Schaltfläche **+ Hinzufügen**.
 2. Wählen Sie die private Schlüsseldatei (PFX) des Clusterzertifikats aus, die Sie beim Erstellen ihres Service Fabric-Clusters angegeben haben, und geben Sie einen Namen und das Kennwort für den privaten Schlüssel ein.

> [!NOTE]
> In diesem Tutorial wird dasselbe Zertifikat für die Clientauthentifizierung und die Knoten-zu-Knoten-Sicherheit im Cluster verwendet. Sie können ein separates Clientzertifikat verwenden, wenn Sie den Zugriff auf Ihr Service Fabric-Cluster konfiguriert haben.

### <a name="configure-the-backend"></a>Konfigurieren des Back-Ends

Nachdem die API Management-Sicherheit konfiguriert wurde, können Sie nun das Service Fabric-Back-End konfigurieren. Bei Service Fabric-Back-Ends fungiert kein bestimmter Service Fabric-Dienst, sondern der Service Fabric-Cluster als Back-End. Dies ermöglicht einer einzelnen Richtlinie das Weiterleiten an mehr als einen Dienst im Cluster.

Dieser Schritt erfordert das zuvor generierte Zugriffstoken und den Fingerabdruck für Ihr Clusterzertifikat, das Sie zuvor in API Management hochgeladen haben.

> [!NOTE]
> Wenn Sie im vorherigen Schritt ein separates Clientzertifikat für API Management verwendet haben, benötigen Sie in diesem Schritt zusätzlich zu dem Fingerabdruck des Clusterzertifikats den Fingerabdruck für das Clientzertifikat.

Senden Sie die folgende HTTP-PUT-Anforderung an die URL von API Management-API, die Sie sich vorher bei der Aktivierung der API Management-REST-API zum Konfigurieren des Service Fabric-Back-End-Diensts notiert haben. Wenn der Befehl erfolgreich abgeschlossen wurde, sollte `HTTP 201 Created` als Antwort angezeigt werden. Weitere Informationen zu jedem Feld können Sie der [Back-End-API-Referenzdokumentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) zu API Management entnehmen.

HTTP-Befehl und URL:
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

Anforderungsheader:
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

Anforderungstext:
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

Der hier angegebene **url**-Parameter ist ein vollqualifizierter Dienstname eines Diensts in Ihrem Cluster, an den alle Anforderungen standardmäßig weitergeleitet werden, sollte in einer Back-End-Richtlinie kein Dienstname angegeben sein. Sie können einen fingierten Dienstnamen wie z.B. „fabric:/fake/service“ verwenden, wenn Sie keinen Fallbackdienst einsetzen möchten.

In der [Back-End-API-Referenzdokumentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) zu API Management finden Sie detaillierte Informationen zu jedem Feld.

```python
#import requests library for making REST calls
import requests
import json

#specify url
url = 'https://ryanwiapimgmt.management.azure-api.net/backends/servicefabric?api-version=2016-10-10'

token = "SharedAccessSignature integration&201710140514&Lqnbei7n2Sot6doiNtxMFPUi/m9LsNa+1ZK/PdxqFl49JFWjXh1wW5Gd99r/tIOeHp6dU8UV5iZUdOPfcrm5hg=="

payload = {
    "description": "My Service Fabric backend",
    "url": "fabric:/ApiApplication/ApiWebService",
    "protocol": "http",
    "resourceId": "https://tutorialcluster.eastus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "https://tutorialcluster.eastus.cloudapp.azure.com:19080" ],
            "clientCertificateThumbprint": "97EDD7E4979FB072AF3E480A5E5EE34B1B89DD80",
            "serverCertificateThumbprints": [ "97EDD7E4979FB072AF3E480A5E5EE34B1B89DD80" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}

headers = {'Authorization': token, "Content-Type": "application/json"}

#Call REST API
response = requests.put(url, data=json.dumps(payload), headers=headers)

#Print Response
print(response.status_code)
print(response.text)
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Bereitstellen eines Service Fabric-Back-End-Diensts

Nachdem Sie nun Service Fabric als Back-End für API Management konfiguriert haben, können Sie für Ihre APIs Back-End-Richtlinien erstellen, die Datenverkehr an Ihre Service Fabric-Dienste senden. Zunächst benötigen Sie jedoch einen Dienst, der in Service Fabric ausgeführt wird, um Anforderungen zu akzeptieren.

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a>Erstellen Sie einen Service Fabric-Dienst mit einem HTTP-Endpunkt

In diesem Tutorial erstellen wir mithilfe der Standardvorlage für Web-API-Projekte einen einfachen zustandslosen zuverlässigen ASP.NET Core-Dienst. Dadurch wird ein HTTP-Endpunkt für den Dienst erstellt, den Sie über Azure API Management verfügbar machen:

```
/api/values
```

Beginnen Sie mit dem [Einrichten Ihrer Entwicklungsumgebung für die Entwicklung in ASP.NET Core](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).

Nachdem Sie Ihre Entwicklungsumgebung eingerichtet haben, starten Sie Visual Studio als Administrator, und erstellen Sie einen ASP.NET Core-Dienst:

 1. Wählen Sie in Visual Studio "Datei -> Neues Projekt" aus.
 2. Wählen Sie die Service Fabric-Anwendungsvorlage unter Cloud aus, und benennen Sie sie **„ApiApplication“**.
 3. Wählen Sie die ASP.NET Core-Dienstvorlage aus, und benennen Sie das Projekt **„WebApiService“**.
 4. Wählen Sie die Web-API ASP.NET Core 1.1-Projektvorlage aus.
 5. Nachdem das Projekt erstellt wurde, öffnen Sie die `PackageRoot\ServiceManifest.xml`, und entfernen Sie das `Port`-Attribut aus der Endpunktressourcenkonfiguration:
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    Dies ermöglicht Service Fabric, einen Port dynamisch aus dem Portbereich für Anwendungen anzugeben, den wir über die Netzwerksicherheitsgruppe in der Cluster-Resource Manager-Vorlage geöffnet haben, und ermöglicht so den Fluss von Datenverkehr aus API Management.
 
 6. Drücken Sie in Visual Studio F5, um zu überprüfen, ob die Web-API lokal verfügbar ist. 

    Öffnen Sie den Service Fabric Explorer, und führen Sie einen Drilldown zu einer bestimmten Instanz des ASP.NET Core-Diensts aus, um die Basisadresse herauszufinden, die durch den Dienst überwacht wird. Fügen Sie `/api/values` an die Basisadresse an, und öffnen Sie sie in einem Browser. Dadurch wird die Get-Methode für den ValuesController in der Web-API-Vorlage aufgerufen. Hierbei wird die von der Vorlage angegebene Standardantwort zurückgegeben, nämlich ein JSON-Array, das zwei Zeichenfolgen enthält:

    ```json
    ["value1", "value2"]`
    ```

    Dies ist der Endpunkt, den Sie über API Management in Azure verfügbar machen müssen.

 7. Stellen Sie schließlich die Anwendung auf Ihrem Cluster in Azure bereit. Klicken Sie [in Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box) mit der rechten Maustaste auf das Anwendungs-Projekt, und wählen Sie **Veröffentlichen** aus. Geben Sie Ihren Clusterendpunkt (z.B. `mycluster.westus.cloudapp.azure.com:19000`) an, um die Anwendung in Ihrem Service Fabric-Cluster in Azure bereitzustellen.

Ein zustandsloser ASP.NET Core-Dienst mit dem Namen `fabric:/ApiApplication/WebApiService` sollte jetzt in Ihrem Service Fabric-Cluster in Azure ausgeführt werden.

## <a name="create-an-api-operation"></a>Erstellen eines API-Vorgangs

Nun sind wir bereit, einen Vorgang in API Management zu erstellen, welchen externe Clients zur Kommunikation mit dem im Service Fabric-Cluster ausgeführten zustandslosen ASP.NET Core-Dienst verwenden.

1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu Ihrer Bereitstellung des API Management-Diensts.
2. Wählen Sie auf dem Blatt des API Management-Diensts die Option **APIs** aus.
3. Fügen Sie eine neue API hinzu, indem Sie auf **+API** und dann auf das Feld **Leere API** klicken und folgende Eingaben im Dialogfeld vornehmen:

    - **Webdienst-URL**: für Service Fabric-Back-Ends wird dieser URL-Wert nicht verwendet. Sie können hier einen beliebigen Wert einfügen. Verwenden Sie für dieses Tutorial „http://servicefabric“.
    - **Anzeigename:** Geben Sie einen Namen für Ihre API an. Verwenden Sie für dieses Tutorial „Service Fabric App“.
    - **Name:** Geben Sie „service-fabric-app“ ein.
    - **URL-Schema:** Wählen Sie **HTTP**, **HTTPS** oder **beide** aus. Verwenden Sie für dieses Tutorial **beide**.
    - **API-URL-Suffix**: Geben Sie ein Suffix für unsere API ein. Verwenden Sie für dieses Tutorial „myapp“.
 
4. Wählen Sie **Service Fabric App** in der Liste der APIs aus, und klicken Sie auf **+ Vorgang hinzufügen**, um einen Front-End-API-Vorgang hinzuzufügen. Füllen Sie die Werte aus:
    
    - **URL:** Wählen Sie **GET** aus, und geben Sie einen URL-Pfad für die API an. Verwenden Sie für dieses Tutorial „/api/values“.  Standardmäßig ist der hier angegebene URL-Pfad der URL-Pfad, der an den Back-End-Service Fabric-Dienst gesendet wird. Wenn Sie hier den gleichen URL-Pfad verwenden, den Ihr Dienst verwendet, in diesem Fall „/api/values“, funktioniert der Vorgang ohne weitere Änderung. Sie können hier auch einen URL-Pfad angeben, der sich von dem von Ihrem Back-End-Service Fabric-Dienst verwendeten URL-Pfad unterscheidet. In diesem Fall müssen Sie auch später in Ihrer Vorgangsrichtlinie einen Pfad-Rewrite angeben.
    - **Anzeigename**: Geben Sie einen Namen für die API ein. Verwenden Sie für dieses Tutorial „Values“.

5. Klicken Sie auf **Speichern**.

## <a name="configure-a-backend-policy"></a>Konfigurieren Sie eine Back-End-Richtlinie

Die Back-End-Richtlinie verbindet alles miteinander. Hier konfigurieren Sie den Back-End-Service Fabric-Dienst, an den Anforderungen weitergeleitet werden. Sie können diese Richtlinie auf alle API-Vorgänge anwenden. Die [Back-End-Konfiguration für Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) bietet die folgenden Anforderungsrouting-Steuerelemente: 
 - Dienstinstanzauswahl durch Angabe eines Service Fabric-Dienst-Instanznamens entweder hartcodiert (z.B. `"fabric:/myapp/myservice"`) oder aus der HTTP-Anforderung generiert (z.B. `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Lösung der Partition durch Generieren eines Partitionsschlüssels mithilfe eines Service Fabric-Partitionierungsschemas.
 - Replikatauswahl für zustandsbehaftete Dienste.
 - Retry-Bedingungen der Lösung, mit denen Sie die Bedingungen für das erneute Beheben eines Dienstspeicherorts und erneutes Senden einer Anforderung festlegen können.

Erstellen Sie für dieses Tutorial eine Back-End-Richtlinie, die Anforderungen direkt an den zuvor bereitgestellten zustandslosen ASP.NET Core-Dienst weiterleitet:

 1. Wählen Sie die **eingehenden Richtlinien** für den Values-Vorgang aus, und bearbeiten Sie diese, indem Sie auf das Symbol „Bearbeiten“ klicken und anschließend **Codeansicht** auswählen.
 2. Fügen Sie, wie hier dargestellt, im Gruppenrichtlinien-Code-Editor eine `set-backend-service`-Richtlinie unter „Eingehende RichtlinienE hinzu, und klicken Sie auf die Schaltfläche **Speichern**:
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
           sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
      </inbound>
      <backend>
        <base/>
      </backend>
      <outbound>
        <base/>
      </outbound>
    </policies>
    ```

Einen vollständigen Satz von Attributen für die Service Fabric-Back-End-Richtlinie finden Sie in der [API Management-Back-End-Dokumentation](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)

### <a name="add-the-api-to-a-product"></a>Hinzufügen der API zu einem Produkt 

Bevor Sie die API aufrufen können, müssen Sie sie einem Produkt hinzufügen, bei dem Sie Benutzern Zugriff gewähren können. 

 1. Wählen Sie im API Management-Dienst die Option **Produkte** aus.
 2. Standardmäßig bietet API Management zwei Produkte an: Starter und Unbegrenzt. Wählen Sie das Produkt Unbegrenzt.
 3. Wählen Sie **+ API hinzufügen** aus.
 4. Wählen Sie die in den vorherigen Schritten erstellte `Service Fabric App`-API aus, und klicken Sie auf die Schaltfläche **Auswählen**.

### <a name="test-it"></a>Testen

Sie können jetzt versuchen, eine Anforderung an den Back-End-Dienst in Service Fabric über API Management direkt vom Azure-Portal aus zu senden.

 1. Wählen Sie im API Management-Dienst die Option **API** aus.
 2. Wählen Sie in der in den vorherigen Schritten erstellten **Service Fabric-App**-API die Registerkarte **Test** und dann den Vorgang **Values** aus.
 3. Klicken Sie auf die Schaltfläche **Senden**, um eine Testanforderung an den Back-End-Dienst zu senden.  Daraufhin sollte eine HTTP-Antwort ähnlich der folgenden angezeigt werden:

    ```http
    HTTP/1.1 200 OK

    Transfer-Encoding: chunked

    Content-Type: application/json; charset=utf-8

    Vary: Origin

    Access-Control-Allow-Origin: https://apimanagement.hosting.portal.azure.net

    Access-Control-Allow-Credentials: true

    Access-Control-Expose-Headers: Transfer-Encoding,Date,Server,Vary,Ocp-Apim-Trace-Location

    Ocp-Apim-Trace-Location: https://apimgmtstuvyx3wa3oqhdbwy.blob.core.windows.net/apiinspectorcontainer/RaVVuJBQ9yxtdyH55BAsjQ2-1?sv=2015-07-08&sr=b&sig=Ab6dPyLpTGAU6TdmlEVu32DMfdCXTiKAASUlwSq3jcY%3D&se=2017-09-15T05%3A49%3A53Z&sp=r&traceId=ed9f1f4332e34883a774c34aa899b832

    Date: Thu, 14 Sep 2017 05:49:56 GMT


    [
    "value1",
    "value2"
    ]
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Ein Cluster besteht neben der Clusterressource selbst noch aus weiteren Azure-Ressourcen. Die einfachste Möglichkeit zum Löschen des Clusters und aller darin genutzten Ressourcen besteht darin, die Ressourcengruppe zu löschen.

Melden Sie sich bei Azure an, und wählen Sie die Abonnement-ID aus, mit der Sie den Cluster entfernen möchten.  Sie finden Ihre Abonnement-ID, indem Sie sich beim [Azure-Portal](http://portal.azure.com) anmelden. Löschen Sie die Ressourcengruppe und alle Clusterressourcen mithilfe des Cmdlets [Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
$ResourceGroupName = "tutorialgroup"
Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="tutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen von API Management
> * Konfigurieren von API Management
> * Erstellen eines API-Vorgangs
> * Konfigurieren Sie eine Back-End-Richtlinie
> * Hinzufügen der API zu einem Produkt


[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[network-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json
