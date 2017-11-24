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
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/10/2017
ms.author: ryanwi
ms.openlocfilehash: 97bcf312621ec0fed28e26179d4c4aa101a8a92d
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-api-management-with-service-fabric"></a>Bereitstellen von API Management mit Service Fabric
Dieses Tutorial ist der dritte Teil einer Reihe.  Bei der Bereitstellung von Azure API Management mit Service Fabric handelt es sich um ein erweitertes Szenario.  API Management ist nützlich, wenn Sie APIs mit einem umfangreichen Satz von Routingregeln für Ihre Service Fabric-Back-End-Dienste veröffentlichen möchten. Cloudanwendungen benötigen normalerweise ein Front-End-Gateway, um für Benutzer, Geräte oder andere Anwendungen einen zentralen Eingangspunkt bereitzustellen. In Service Fabric kann ein Gateway ein beliebiger zustandsloser Dienst sein, der für den Eingang von Datenverkehr ausgelegt ist, z.B. eine APP.NET Core-Anwendung, Event Hubs, IoT Hub oder Azure API Management. 

In diesem Tutorial wird beschrieben, wie Sie [Azure API Management](../api-management/api-management-key-concepts.md) mit Service Fabric einrichten, um Datenverkehr an einen Back-End-Dienst in Service Fabric weiterzuleiten.  Am Ende des Tutorials haben Sie API Management für ein VNET bereitgestellt und einen API-Vorgang zum Senden von Datenverkehr an zustandslose Back-End-Dienste konfiguriert. Weitere Informationen zu den Azure API Management-Szenarios mit Service Fabric finden Sie im [Übersichtsartikel](service-fabric-api-management-overview.md).

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
> * [Horizontales Herunter- oder Hochskalieren eines Clusters](/service-fabric-tutorial-scale-cluster.md)
> * Bereitstellen von API Management mit Service Fabric

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Installieren Sie das [Azure PowerShell-Modul Version 4.1 oder höher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) oder [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Erstellen Sie einen sicheren [Windows-Cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oder [Linux-Cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) in Azure.
- Wenn Sie einen Windows-Cluster bereitstellen, richten Sie eine Windows-Entwicklungsumgebung ein. Installieren Sie [Visual Studio 2017](http://www.visualstudio.com) und die Workloads für **Azure-Entwicklung**, **ASP.NET und Webentwicklung** und **Plattformübergreifende .NET Core-Entwicklung**.  Richten Sie dann eine [.NET-Entwicklungsumgebung](service-fabric-get-started.md) ein.
- Wenn Sie einen Linux-Cluster bereitstellen, richten Sie eine Java-Entwicklungsumgebung unter [Linux](service-fabric-get-started-linux.md) oder [macOS](service-fabric-get-started-mac.md) ein.  Installieren Sie die [Service Fabric CLI](service-fabric-cli.md). 

## <a name="network-topology"></a>Netzwerktopologie
Nach dem Erstellen eines sicheren [Windows-Clusters](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oder [Linux-Clusters](service-fabric-tutorial-create-vnet-and-linux-cluster.md) in Azure stellen Sie API Management für das virtuelle Netzwerk (VNET) in dem Subnetz und der NSG bereit, die für API Management festgelegt wurden. Für dieses Tutorial ist die API Management-Resource Manager-Vorlage so vorkonfiguriert, dass die Namen des VNET, des Subnetzes und der NSG verwendet werden, die im vorherigen [Tutorial zum Windows-Cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oder [Tutorial zum Linux-Cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) eingerichtet wurden. In diesem Tutorial wird die folgende Topologie in Azure bereitgestellt, in der API Management und Service Fabric Subnetze im gleichen virtuellen Netzwerk sind:

 ![Bildunterschrift][sf-apim-topology-overview]

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Melden Sie sich bei Azure an, und wählen Sie Ihr Abonnement aus
Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus, bevor Sie Azure-Befehle ausführen.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Bereitstellen eines Service Fabric-Back-End-Diensts

Vor dem Konfigurieren von API Management für die Weiterleitung von Datenverkehr an einen Service Fabric-Back-End-Dienst benötigen Sie zunächst einen ausgeführten Dienst zum Akzeptieren von Anforderungen.  Wenn Sie zuvor einen [Windows-Cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) erstellt haben, stellen Sie einen .NET Service Fabric-Dienst bereit.  Wenn Sie zuvor einen [Linux-Cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) erstellt haben, stellen Sie einen Java Service Fabric-Dienst bereit.

### <a name="deploy-a-net-service-fabric-service"></a>Bereitstellen eines .NET Service Fabric-Diensts

In diesem Tutorial erstellen Sie mithilfe der Standardvorlage für Web-API-Projekte einen einfachen zustandslosen zuverlässigen ASP.NET Core-Dienst. Dadurch wird ein HTTP-Endpunkt für den Dienst erstellt, den Sie über Azure API Management verfügbar machen.

Starten Sie Visual Studio als Administrator, und erstellen Sie einen ASP.NET Core-Dienst:

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

    Das Entfernen des Ports ermöglicht Service Fabric, einen Port dynamisch aus dem Portbereich für Anwendungen anzugeben, der über die Netzwerksicherheitsgruppe in der Resource Manager-Vorlage des Clusters geöffnet wird, und ermöglicht so den Fluss von Datenverkehr aus API Management.
 
 6. Drücken Sie in Visual Studio F5, um zu überprüfen, ob die Web-API lokal verfügbar ist. 

    Öffnen Sie den Service Fabric Explorer, und führen Sie einen Drilldown zu einer bestimmten Instanz des ASP.NET Core-Diensts aus, um die Basisadresse herauszufinden, die durch den Dienst überwacht wird. Fügen Sie `/api/values` an die Basisadresse an, und öffnen Sie diese Adresse in einem Browser. Dadurch wird die Get-Methode für den ValuesController in der Web-API-Vorlage aufgerufen. Hierbei wird die von der Vorlage angegebene Standardantwort zurückgegeben, nämlich ein JSON-Array, das zwei Zeichenfolgen enthält:

    ```json
    ["value1", "value2"]`
    ```

    Dies ist der Endpunkt, den Sie über API Management in Azure verfügbar machen.

 7. Stellen Sie schließlich die Anwendung auf Ihrem Cluster in Azure bereit. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Anwendungsprojekt, und wählen Sie **Veröffentlichen** aus. Geben Sie Ihren Clusterendpunkt (z.B. `mycluster.southcentralus.cloudapp.azure.com:19000`) an, um die Anwendung in Ihrem Service Fabric-Cluster in Azure bereitzustellen.

Ein zustandsloser ASP.NET Core-Dienst mit dem Namen `fabric:/ApiApplication/WebApiService` sollte jetzt in Ihrem Service Fabric-Cluster in Azure ausgeführt werden.

### <a name="create-a-java-service-fabric-service"></a>Erstellen eines Java Service Fabric-Diensts
In diesem Tutorial erstellen Sie einen einfachen Webserver, der Nachrichten als Echo zurück an den Benutzer leitet. Die Echoserver-Beispielanwendung enthält einen HTTP-Endpunkt für den Dienst, den Sie über Azure API Management verfügbar machen.

1. Klonen Sie die Beispiele zu den ersten Schritten mit Java.

   ```bash
   git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
   cd service-fabric-java-getting-started
   ```

2. Bearbeiten Sie die Datei *Services/EchoServer/EchoServer1.0/EchoServerApplication/EchoServerPkg/ServiceManifest.xml*. Aktualisieren Sie den Endpunkt, sodass der Dienst an Port 8081 lauscht.

   ```xml
   <Endpoint Name="WebEndpoint" Protocol="http" Port="8081" />
   ```

3. Speichern Sie die Datei *ServiceManifest.xml*, und erstellen Sie dann die Anwendung EchoServer1.0.

   ```bash
   cd Services/EchoServer/EchoServer1.0/
   gradle
   ```

4. Stellen Sie die Anwendung im Cluster bereit.

   ```bash
   cd Scripts
   sfctl cluster select --endpoint http://mycluster.southcentralus.cloudapp.azure.com:19080
   ./install.sh
   ```

   Ein zustandsloser Java-Dienst mit dem Namen `fabric:/EchoServerApplication/EchoServerService` sollte jetzt in Ihrem Service Fabric-Cluster in Azure ausgeführt werden.

5. Öffnen Sie einen Browser, und geben Sie diese URL ein: http://mycluster.southcentralus.cloudapp.azure.com:8081/getMessage. Es sollte „[Version 1.0]Hello World!!!“ angezeigt werden.

## <a name="download-and-understand-the-resource-manager-template"></a>Herunterladen und Verstehen der Resource Manager-Vorlage
Laden Sie die folgende Resource Manager-Vorlage und -Parameterdatei herunter, und speichern Sie sie:
 
- [apim.json][apim-arm]
- [apim.parameters.json][apim-parameters-arm]

In den folgenden Abschnitten werden die Ressourcen beschrieben, die durch die Vorlage *apim.json* definiert werden. Weitere Informationen finden Sie über die Links zur Referenzdokumentation der jeweiligen Vorlage in den einzelnen Abschnitten. Die in der Parameterdatei *apim.parameters.json* definierten konfigurierbaren Parameter werden weiter unten in diesem Artikel festgelegt.

### <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
[Microsoft.ApiManagement/service](/azure/templates/microsoft.apimanagement/service) beschreibt die API Management-Dienstinstanz: Name, SKU oder Ebene, Speicherort der Ressourcengruppe, Herausgeberinformationen und virtuelles Netzwerk.

### <a name="microsoftapimanagementservicecertificates"></a>Microsoft.ApiManagement/service/certificates
[Microsoft.ApiManagement/service/certificates](/azure/templates/microsoft.apimanagement/service/certificates) konfiguriert die API Management-Sicherheit. API Management muss bei Ihrem Service Fabric-Cluster mit Clientzertifikaten, die über Zugriff auf den Cluster verfügen, für die Dienstermittlung authentifiziert werden. In diesem Tutorial wird dasselbe Zertifikat verwendet, das zuvor beim Erstellen des [Windows-Clusters](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor) oder [Linux-Clusters](service-fabric-tutorial-create-vnet-and-linux-cluster.md#createvaultandcert_anchor) angegeben wurde und das standardmäßig für den Zugriff auf Ihren Cluster verwendet werden kann. 

In diesem Tutorial wird dasselbe Zertifikat für die Clientauthentifizierung und die Knoten-zu-Knoten-Sicherheit im Cluster verwendet. Sie können ein separates Clientzertifikat verwenden, wenn Sie den Zugriff auf Ihr Service Fabric-Cluster konfiguriert haben. Geben Sie den **Namen**, das **Kennwort** und die **Daten** (Base64-codierte Zeichenfolge) der privaten Schlüsseldatei (.pfx) des Clusterzertifikats an, die Sie beim Erstellen des Service Fabric-Clusters angegeben haben.

### <a name="microsoftapimanagementservicebackends"></a>Microsoft.ApiManagement/service/backends
[Microsoft.ApiManagement/service/backends](/azure/templates/microsoft.apimanagement/service/backends) beschreibt den Back-End-Dienst, an den Datenverkehr weitergeleitet wird. 

Bei Service Fabric-Back-Ends dient der Service Fabric-Cluster anstatt eines bestimmten Service Fabric-Diensts als Back-End. Dies ermöglicht einer einzelnen Richtlinie das Weiterleiten an mehr als einen Dienst im Cluster. Das hier angegebene Feld **url** ist ein vollqualifizierter Dienstname eines Diensts in Ihrem Cluster, an den standardmäßig alle Anforderungen weitergeleitet werden, wenn in einer Back-End-Richtlinie kein Dienstname angegeben ist. Sie können einen fingierten Dienstnamen wie z.B. „fabric:/fake/service“ verwenden, wenn Sie keinen Fallbackdienst einsetzen möchten. **resourceId** gibt den Verwaltungsendpunkt des Clusters an.  **clientCertificateThumbprint** und **serverCertificateThumbprints** identifizieren die zur Authentifizierung mit dem Cluster verwendeten Zertifikate.

### <a name="microsoftapimanagementserviceproducts"></a>Microsoft.ApiManagement/service/products
[Microsoft.ApiManagement/service/products](/azure/templates/microsoft.apimanagement/service/products) erstellt ein Produkt. Produkte in Azure API Management enthalten eine oder mehrere APIs sowie ein Nutzungskontingent und Nutzungsbedingungen. Nachdem ein Produkt veröffentlicht wurde, können Entwickler das Produkt abonnieren und die APIs des Produkts verwenden. 

Geben Sie für **displayName** einen beschreibenden Anzeigenamen und für **description** eine Beschreibung für das Produkt ein. Für dieses Tutorial ist ein Abonnement, jedoch keine Abonnementgenehmigung durch einen Administrator erforderlich.  Der unter **state** angegebene Zustand dieses Produkts lautet „veröffentlicht“ und ist für Abonnenten sichtbar. 

### <a name="microsoftapimanagementserviceapis"></a>Microsoft.ApiManagement/service/apis
[Microsoft.ApiManagement/service/apis](/azure/templates/microsoft.apimanagement/service/apis) erstellt eine API. Eine API in API Management besteht aus einem Satz von Operationen, die von Clientanwendungen aufgerufen werden können. Nachdem Sie die Operationen hinzugefügt haben, wird die API zu einem Produkt hinzugefügt und kann veröffentlicht werden. Nach der Veröffentlichung der API können Entwickler diese abonnieren und verwenden.

- **displayName** kann ein beliebiger Name für die API sein. Verwenden Sie für dieses Tutorial „Service Fabric App“.
- **name** enthält einen eindeutigen und beschreibenden Namen für die API, z.B. „service-fabric-app“. Dieser Name wird in den Entwickler- und Herausgeberportalen angezeigt. 
- **serviceUrl** verweist auf den HTTP-Dienst, der die API implementiert. API Management leitet Anfragen an diese Adresse weiter. Bei Service Fabric-Back-Ends wird dieser URL-Wert nicht verwendet. Sie können hier einen beliebigen Wert einfügen. Verwenden Sie für dieses Tutorial z.B. „http://servicefabric“. 
- **path** wird an die Basis-URL für den API Management-Dienst angefügt. Alle in einer API Management-Dienstinstanz gehosteten APIs teilen sich dieselbe Basis-URL. API Management unterscheidet APIs durch deren Suffix. Daher muss jede API eines bestimmten Herausgebers ein eindeutiges Suffix haben. 
- **protocols** legt fest, welche Protokolle für den Zugriff auf die API verwendet werden können. Geben Sie für dieses Tutorial **http** und **https** an.
- **path** ist ein Suffix für die API. Verwenden Sie für dieses Tutorial „myapp“.

### <a name="microsoftapimanagementserviceapisoperations"></a>Microsoft.ApiManagement/service/apis/operations
[Microsoft.ApiManagement/service/apis/operations](/azure/templates/microsoft.apimanagement/service/apis/operations): Damit eine API in API Management verwendet werden kann, müssen ihr Vorgänge hinzugefügt werden.  Externe Clients verwenden einen Vorgang zur Kommunikation mit dem im Service Fabric-Cluster ausgeführten zustandslosen ASP.NET Core-Dienst.

Geben Sie zum Hinzufügen eines Vorgangs für eine Front-End-API folgende Werte an:

- **displayName** und **description** beschreiben den Vorgang. Verwenden Sie für dieses Tutorial „Values“.
- **method** gibt das HTTP-Verb an.  Geben Sie für dieses Tutorial **GET** an.
- **urlTemplate** wird an die Basis-URL der API angefügt und identifiziert einen einzelnen HTTP-Vorgang.  Verwenden Sie für dieses Tutorial `/api/values`, wenn Sie den .NET-Back-End-Dienst hinzugefügt haben, oder `getMessage`, wenn Sie den Java-Back-End-Dienst hinzugefügt haben.  Standardmäßig ist der hier angegebene URL-Pfad der URL-Pfad, der an den Back-End-Service Fabric-Dienst gesendet wird. Wenn Sie hier den gleichen URL-Pfad verwenden, den Ihr Dienst verwendet, z.B. „/api/values“, wird der Vorgang ohne weitere Änderung ausgeführt. Sie können hier auch einen URL-Pfad angeben, der sich von dem von Ihrem Service Fabric-Back-End-Dienst verwendeten URL-Pfad unterscheidet. In diesem Fall müssen Sie später in Ihrer Vorgangsrichtlinie auch ein Pfad-Rewrite angeben.

### <a name="microsoftapimanagementserviceapispolicies"></a>Microsoft.ApiManagement/service/apis/policies
[Microsoft.ApiManagement/service/apis/policies](/azure/templates/microsoft.apimanagement/service/apis/policies) erstellt eine Back-End-Richtlinie, die alles verbindet. Hier konfigurieren Sie den Back-End-Service Fabric-Dienst, an den Anforderungen weitergeleitet werden. Sie können diese Richtlinie auf alle API-Vorgänge anwenden.  Weitere Informationen finden Sie in der [Übersicht über Richtlinien](/azure/api-management/api-management-howto-policies). 

Die [Back-End-Konfiguration für Service Fabric](/azure/api-management/api-management-transformation-policies#SetBackendService) bietet die folgenden Anforderungsrouting-Steuerelemente: 
 - Dienstinstanzauswahl durch Angabe eines Service Fabric-Dienst-Instanznamens entweder hartcodiert (z.B. `"fabric:/myapp/myservice"`) oder aus der HTTP-Anforderung generiert (z.B. `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Lösung der Partition durch Generieren eines Partitionsschlüssels mithilfe eines Service Fabric-Partitionierungsschemas.
 - Replikatauswahl für zustandsbehaftete Dienste.
 - Retry-Bedingungen der Lösung, mit denen Sie die Bedingungen für das erneute Beheben eines Dienstspeicherorts und erneutes Senden einer Anforderung festlegen können.

**policyContent** ist der mit Escapezeichen versehene Json-XML-Inhalt der Richtlinie.  Erstellen Sie für dieses Tutorial eine Back-End-Richtlinie, die Anforderungen direkt an den zuvor bereitgestellten zustandslosen .NET- oder Java-Dienst weiterleitet. Fügen Sie eine `set-backend-service`-Richtlinie unter „inbound policies“ hinzu.  Ersetzen Sie „service-name“ durch `fabric:/ApiApplication/WebApiService`, wenn Sie zuvor den .NET-Back-End-Dienst bereitgestellt haben, oder durch `fabric:/EchoServerApplication/EchoServerService`, wenn Sie den Java-Dienst bereitgestellt haben.
    
```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service 
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
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

## <a name="set-parameters-and-deploy-api-management"></a>Festlegen von Parametern und Bereitstellen von API Management
Geben Sie für die Bereitstellung die folgenden leeren Parameter in der Datei *apim.parameters.json* an. 

|Parameter|Wert|
|---|---|
|apimInstanceName|sf-apim|
|apimPublisherEmail|myemail@contosos.com|
|apimSku|Developer|
|serviceFabricCertificateName|sfclustertutorialgroup320171031144217|
|certificatePassword|q6D7nN%6ck@6| 
|serviceFabricCertificateThumbprint|C4C1E541AD512B8065280292A8BA6079C3F26F10 |
|serviceFabricCertificate|&lt;Base64-codierte Zeichenfolge&gt;|
|url_path|/api/values|
|clusterHttpManagementEndpoint|https://mysfcluster.southcentralus.cloudapp.azure.com:19080|
|inbound_policy|&lt;XML-Zeichenfolge&gt;|

*certificatePassword* und *serviceFabricCertificateThumbprint* müssen mit dem zum Einrichten des Clusters verwendeten Clusterzertifikat übereinstimmen.  

*serviceFabricCertificate* ist das Zertifikat als Base64-codierte Zeichenfolge, die mithilfe des folgenden Skripts generiert werden kann:

```powershell
$bytes = [System.IO.File]::ReadAllBytes("C:\mycertificates\sfclustertutorialgroup220171109113527.pfx");
$b64 = [System.Convert]::ToBase64String($bytes);
[System.Io.File]::WriteAllText("C:\mycertificates\sfclustertutorialgroup220171109113527.txt", $b64);
```

Ersetzen Sie in *inbound_policy* „service-name“ durch `fabric:/ApiApplication/WebApiService`, wenn Sie zuvor den .NET-Back-End-Dienst bereitgestellt haben, oder durch `fabric:/EchoServerApplication/EchoServerService`, wenn Sie den Java-Dienst bereitgestellt haben.

```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service 
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
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

Verwenden Sie das folgende Skript, um die Resource Manager-Vorlagendatei und die Parameterdatei für API Management bereitzustellen:

```powershell
$ResourceGroupName = "sfclustertutorialgroup"
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json 
```

## <a name="test-it"></a>Testen

Sie können jetzt versuchen, eine Anforderung an den Back-End-Dienst in Service Fabric über API Management direkt im [Azure-Portal](https://portal.azure.com) zu senden.

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
$ResourceGroupName = "sfclustertutorialgroup"
Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="conclusion"></a>Zusammenfassung
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

<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-tutorial-deploy-api-management/sf-apim-topology-overview.png
