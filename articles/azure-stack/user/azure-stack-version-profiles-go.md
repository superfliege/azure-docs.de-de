---
title: Verwenden von API-Versionsprofilen mit GO in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie mehr zur Verwendung von API-Versionsprofilen mit GO in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 84475302-EFC2-4C35-B4CF-60C857A96345
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 09a774e5dda71d87c3862a6152ff5d2c9468c40c
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="use-api-version-profiles-with-go-in-azure-stack"></a>Verwenden von API-Versionsprofilen mit Go in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

## <a name="go-and-version-profiles"></a>Go und Versionsprofile

Ein Profil ist eine Kombination aus verschiedenen Ressourcentypen mit unterschiedlichen Versionen aus verschiedenen Diensten. Mithilfe eines Profils können Sie verschiedene Ressourcentypen kombinieren. Profile bieten Folgendes:

 - Stabilität für Ihre Anwendung durch Festlegung auf bestimmte API-Versionen
 - Kompatibilität für Ihre Anwendung mit Azure Stack und regionalen Azure-Datencentern

Im Go SDK sind Profile unter dem Pfad „profiles/“ verfügbar. Ihre Version ist im Format **JJJJ-MM-TT** angegeben. Derzeit ist die neueste Azure Stack-Profilversion **2017-03-09**. Zum Importieren eines bestimmten Diensts aus einem Profil müssen Sie sein entsprechendes Moduls aus dem Profil importieren. So importieren Sie beispielsweise den Dienst **Compute** aus dem Profil **2017-03-09**:

````go
import "github.com/Azure/azure-sdk-for-go/profi1es/2e17-e3-eg/compute/mgmt/compute" 
````

## <a name="install-azure-sdk-for-go"></a>Installieren des Azure SDK für Go

  1. Installieren Sie Git. Anweisungen hierzu finden Sie unter [Getting Started - Installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) (Erste Schritte: Installieren von Git).
  2. Installieren Sie die [Go-Programmiersprache](https://golang.org/dl).  
  Für API-Profile für Azure ist mindestens Version 1.9 von Go erforderlich.
  3. Installieren Sie das Go Azure SDK und seine Abhängigkeiten, indem Sie den folgenden Bash-Befehl ausführen:
  ```
    go get -u -d github.com/Azure/azure-sdk-for-go/...
  ```

### <a name="the-go-sdk"></a>Das Go SDK

Weitere Informationen zum Azure GO SDK finden Sie hier:
- [Installieren des Azure SDK für Go](https://docs.microsoft.com/go/azure/azure-sdk-go-install)
- Das Azure Go SDK ist öffentlich auf GitHub unter [azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) verfügbar.

### <a name="go-autorest-dependencies"></a>Go-AutoRest-Abhängigkeiten

Das GO SDK ist zum Senden von REST-Anforderungen an Azure Resource Manager-Endpunkte von den Azure-Go-AutoRest-Modulen abhängig. Sie müssen die Azure-Go-AutoRest-Modulabhängigkeiten von [Azure-Go-AutoRest auf GitHub](https://github.com/Azure/go-autorest) herunterladen. Die Bash-Befehle zur Installation finden Sie im Abschnitt **Install** (Installation).

## <a name="how-to-use-go-sdk-profiles-on-azure-stack"></a>Verwenden von GO SDK-Profilen in Azure Stack

So führen Sie ein Beispiel für Go-Code in Azure Stack aus
  1. Installieren Sie das Azure SDK für Go und seine Abhängigkeiten. Anweisungen finden Sie im vorherigen Abschnitt: [Installieren des Azure SDK für Go](#install-azure-sdk-for-go).
  2. Rufen Sie die Metadateninformationen vom Resource Manager-Endpunkt ab. Der Endpunkt gibt eine JSON-Datei mit den zum Ausführen des Go-Codes erforderlichen Informationen zurück.
  > [!note]  
  > Der **ResourceManagerUrl**-Wert im Azure Stack Development Kit (ASDK) lautet `https://management.local.azurestack.external/`.  
  > Der **ResourceManagerUrl**-Wert in integrierten Systemen lautet `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`.  
  > Zum Abrufen der erforderlichen Metadaten verwenden Sie Folgendes: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`.
  
  JSON-Beispieldatei:

  ```json
  { "galleryEndpoint": "https://portal.local.azurestack.external:30015/",  
    "graphEndpoint": "https://graph.windows.net/",  
    "portal Endpoint": "https://portal.local.azurestack.external/", 
    "authentication": {
      "loginEndpoint": "https://login.windows.net/", 
      "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
    }
  }
  ```

  3. Erstellen Sie ein Abonnement, wenn keins verfügbar ist, und speichern Sie die Abonnement-ID zur späteren Verwendung. Informationen zum Erstellen eines Abonnements finden Sie unter [Erstellen von Abonnements für Angebote in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm). 
  4. Erstellen Sie einen Dienstprinzipal mit dem Bereich „Abonnement“ und der Rolle **Besitzer**. Speichern Sie die ID und das Geheimnis des Dienstprinzipals. Informationen zum Erstellen eines Dienstprinzipals für Azure Stack finden Sie unter [Bereitstellen des Anwendungszugriffs auf Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#create-service-principal-for-azure-ad). Ihre Azure Stack-Umgebung ist nun eingerichtet.
  5. Importieren Sie ein Dienstmodul aus dem Go SDK-Profil in Ihren Code. Die aktuelle Version des Azure Stack-Profils ist **2017-03-09**. So importieren Sie beispielsweise ein Netzwerkmodul aus dem Profiltyp **2017-03-09**: 

  ````go
    package main 
    import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
  ````
  
  6. Erstellen und authentifizieren Sie in Ihrer Funktion einen Client mit einem Funktionsaufruf für**neue** Clients. Sie können den folgenden Code zum Erstellen eines Clients für virtuelle Netzwerke verwenden:  

  ````go
  package main 

  import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 

  func main() { 
      vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
      vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
  ````

  Legen Sie `<baseURI>` auf den in Schritt 2 verwendeten **ResourceManagerUrl**-Wert fest.
  Legen Sie `<subscriptionID>` auf den in Schritt 3 gespeicherten **SubscriptionID**-Wert fest.
  Informationen zum Erstellen eines Token finden Sie weiter unten im Abschnitt „Authentifizierung“.  

  7. Rufen Sie API-Methoden mithilfe des im vorherigen Schritt erstellten Clients auf. So erstellen Sie beispielsweise ein virtuelles Netzwerk mithilfe des Clients aus dem vorherigen Schritt: 
  
````go
package main

import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 
func main() { 
  vnetC1ient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
  vnetClient .Authorizer = autorest.NewBearerAuthorizer(token)

  vnetClient .CreateOrUpdate( ) 
````
  
  Ein vollständiges Beispiel zum Erstellen eines virtuellen Netzwerks in Azure Stack mithilfe des Go SDK-Profils finden Sie unter [Beispiel](#example).

## <a name="authentication"></a>Authentifizierung

Installieren Sie die Go-AutoRest-Module, um mithilfe des Go SDK die Authorizer-Eigenschaft aus Azure Active Directory abzurufen. Diese Module sollten bereits bei der Installation des Go SDK installiert worden sein. Ist dies nicht der Fall, installieren Sie das [Authentifizierungspaket auf GitHub](https://github.com/Azure/go-autorest/tree/master/autorest/adal).

Als Authorizer-Wert muss der Autorisierer für den Ressourcenclient festgelegt werden. Es gibt verschiedene Methoden zum Abrufen eines Authorizer-Werts. Die vollständige Liste finden Sie hier.

In diesem Abschnitt wird eine häufig verwendete Methode zum Abrufen von Autorisierertoken in Azure Stack mithilfe von Clientanmeldeinformationen erläutert:

  1. Wenn ein Dienstprinzipal mit der Rolle „Besitzer“ im Abonnement verfügbar ist, überspringen Sie diesen Schritt. Erstellen Sie andernfalls einen Dienstprinzipal (siehe [Anweisungen]( https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals)), und weisen Sie ihm die Rolle „Besitzer“ zu, die Ihrem Abonnement zugeordnet ist (siehe [Anweisungen]( https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal)). Speichern Sie die Anwendungs-ID und das Geheimnis des Dienstprinzipals. 

  2. Importieren Sie das **adal**-Paket aus Go-AutoRest in Ihren Code. 
  
  ````go
  package main
  import "github.com/Azure/go-autorest/autorest/adal" 
  ````

  3. Erstellen Sie ein oauthConfig-Element mithilfe der NewOAuthConfig-Methode aus dem **adal**-Modul. 
  
  ````go
  package main 

  import "github.com/Azure/go-autorest/autorest/ada1" 

  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
  ````
   
  Legen Sie `<activeDirectoryEndpoint>` auf den Wert der Eigenschaft „loginEndpoint“ aus den ResourceManagerUrl-Metadaten fest, die Sie weiter oben in diesem Dokument abgerufen haben.
  Legen Sie den Wert `<tenantID>` auf Ihre Azure Stack-Mandanten-ID fest. 

  4. Erstellen Sie abschließend mithilfe der NewServicePrincipalToken-Methode aus dem adal-Modul ein Dienstprinzipaltoken. 

  ````go
  package main 

  import "github.com/Azure/go-autorest/autorest/adal" 

  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
      token, err = adal.NewServicePrincipalToken(
          *oauthConfig,
          clientID,
          clientSecret,
          activeDirectoryResourceID)
      return token, err
  ````
  
  Legen Sie `<activeDirectoryResourceID>` auf einen der Werte in der Liste „audience“ der ResourceManagerUrl-Metadaten fest, die Sie weiter oben in diesem Dokument abgerufen haben.  
  Legen Sie `<clientID>` auf die Anwendungs-ID des Dienstprinzipals fest, die Sie beim Erstellen des Dienstprinzipals weiter oben in diesem Dokument gespeichert haben.  
  Legen Sie `<clientSecret>` auf das Anwendungsgeheimnis des Dienstprinzipals fest, das Sie beim Erstellen des Dienstprinzipals weiter oben in diesem Dokument gespeichert haben.  

## <a name="example"></a>Beispiel

Dieser Abschnitt enthält ein Beispiel für Go-Code zum Erstellen eines virtuellen Netzwerks in Azure Stack. Vollständige Beispiele des Go SDK finden Sie im [Repository mit Azure Go SDK-Beispielen](https://github.com/Azure-Samples/azure-sdk-for-go-samples). Azure Stack-Beispiele stehen unter dem Pfad „hybrid/“ in den Dienstordnern des Repositorys zur Verfügung.
> [!note]  
> Vergewissern Sie sich zum Ausführen des Codes in diesem Beispiel, dass für das verwendete Abonnement der **Netzwerkressourcenanbieter** als **registriert** festgelegt ist. Navigieren Sie dazu im Azure Stack-Portal zum Abonnement, und klicken Sie auf **Ressourcenanbieter**.

1. Importieren Sie die erforderlichen Pakete in Ihren Code. Sie sollten das neueste verfügbare Profil in Azure Stack verwenden, um das Netzwerkmodul zu importieren. 
  
  ````go
  package main

  import (
      "context"
      "fmt"
      "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
      "github.com/Azure/go-autorest/autorest"
      "github.com/Azure/go-autorest/autorest/adal"
      "github.com/Azure/go-autorest/autorest/to"
  )
  ````

2. Definieren Sie Ihre Umgebungsvariablen. Beachten Sie, dass Sie zum Erstellen eines virtuellen Netzwerks eine Ressourcengruppe benötigen. 

  ````go
  var (
      activeDirectoryEndpoint = "yourLoginEndpointFromResourceManagerUrlMetadata"
      tenantID = "yourAzureStackTenantID"
      clientID = "yourServicePrincipalApplicationID"
      clientSecret = "yourServicePrincipalSecret"
      activeDirectoryResourceID = "yourAudienceFromResourceManagerUrlMetadata"
      subscriptionID = "yourSubscriptionID"
      baseURI = "yourResourceManagerURL"
      resourceGroupName = "existingResourceGroupName"
  )
  ````

3. Sie haben die Umgebungsvariablen definiert. Fügen Sie nun eine Methode hinzu, um mithilfe des **adal**-Pakets ein Authentifizierungstoken zu erstellen. Ausführliche Informationen zur Authentifizierung finden Sie im vorhergehenden Abschnitt.
  
  ````go
  //CreateToken creates a service principal token
  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
      token, err = adal.NewServicePrincipalToken(
          *oauthConfig,
          clientID,
          clientSecret,
          activeDirectoryResourceID)
      return token, err
  }
  ````

4. Fügen Sie die main-Methode hinzu. Die main-Methode ruft zunächst ein Token ab. Dabei wird die im vorherigen Schritt definierte Methode verwendet. Anschließend wird mithilfe des Netzwerkmoduls aus dem Profil ein Client erstellt. Abschließend wird ein virtuelles Netzwerk erstellt. 
  
````go
package main

import (
    "context"
    "fmt"
    "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
    "github.com/Azure/go-autorest/autorest"
    "github.com/Azure/go-autorest/autorest/adal"
    "github.com/Azure/go-autorest/autorest/to"
)

var (
    activeDirectoryEndpoint = "yourLoginEndpointFromResourceManagerUrlMetadata"
    tenantID = "yourAzureStackTenantID"
    clientID = "yourServicePrincipalApplicationID"
    clientSecret = "yourServicePrincipalSecret"
    activeDirectoryResourceID = "yourAudienceFromResourceManagerUrlMetadata"
    subscriptionID = "yourSubscriptionID"
    baseURI = "yourResourceManagerURL"
    resourceGroupName = "existingResourceGroupName"
)

//CreateToken creates a service principal token
func CreateToken() (adal.OAuthTokenProvider, error) {
    var token adal.OAuthTokenProvider
    oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        clientID,
        clientSecret,
        activeDirectoryResourceID)
    return token, err
}

func main() {
    token, _ := CreateToken()
    vnetClient := network.NewVirtualNetworksClientWithBaseURI(baseURI, subscriptionID)
    vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
    future, _ := vnetClient.CreateOrUpdate(
        context.Background(),
        resourceGroupName,
        "sampleVnetName",
        network.VirtualNetwork{
            Location: to.StringPtr("local"),
            VirtualNetworkPropertiesFormat: &network.VirtualNetworkPropertiesFormat{
                AddressSpace: &network.AddressSpace{
                    AddressPrefixes: &[]string{"10.0.0.0/8"},
                },
                Subnets: &[]network.Subnet{
                    {
                        Name: to.StringPtr("subnetName"),
                        SubnetPropertiesFormat: &network.SubnetPropertiesFormat{
                            AddressPrefix: to.StringPtr("10.0.0.0/16"),
                        },
                    },
                },
            },
        })
    err := future.WaitForCompletion(context.Background(), vnetClient.Client)
    if err != nil {
        fmt.Printf(err.Error())
        return
    }
}

````

## <a name="next-steps"></a>Nächste Schritte
* [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md)
* [Konfigurieren der PowerShell-Umgebung des Azure Stack-Benutzers](azure-stack-powershell-configure-user.md)  
