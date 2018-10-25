---
title: Sichere Azure Machine Learning-Webdienste mit SSL
description: Erfahren Sie, wie Sie einen mit dem Azure Machine Learning-Dienst bereitgestellten Webdienst schützen. Mithilfe von SSL (Secure Sockets Layer) und schlüsselbasierter Authentifizierung können Sie den Zugriff auf Webdienste einschränken und die von Clients übermittelten Daten schützen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 10/02/2018
ms.openlocfilehash: 885d867d0733ef923d327d8d6a36fc1588fd4961
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801011"
---
# <a name="secure-azure-machine-learning-web-services-with-ssl"></a>Sichere Azure Machine Learning-Webdienste mit SSL

In diesem Artikel erfahren Sie, wie Sie einen mit dem Azure Machine Learning-Dienst bereitgestellten Webdienst schützen. Mithilfe von SSL (Secure Sockets Layer) und schlüsselbasierter Authentifizierung können Sie den Zugriff auf Webdienste einschränken und die von Clients übermittelten Daten schützen.

> [!Warning]
> Wenn Sie SSL nicht aktivieren, kann jeder Benutzer im Internet Aufrufe an den Webdienst ausführen.

SSL verschlüsselt die zwischen dem Client und dem Webdienst gesendeten Daten. SSL wird auch vom Client zum Überprüfen der Identität des Servers verwendet. Authentifizierung ist nur für Dienste aktiviert, die ein SSL-Zertifikat und einen Schlüssel angegeben haben.  Wenn Sie SSL aktivieren, ist ein Authentifizierungsschlüssel für den Zugriff auf den Webdienst erforderlich.

Ob Sie einen Webdienst mit aktiviertem SSL bereitstellen oder SSL für einen vorhandenen bereitgestellten Webdienst aktivieren, es sind die gleichen Schritte auszuführen:

1. Rufen Sie einen Domänennamen ab.

2. Rufen Sie ein SSL-Zertifikat ab.

3. Stellen Sie den Webdienst mit aktivierter SSL-Einstellung bereit, oder aktualisieren Sie ihn.

4. Aktualisieren Sie Ihren DNS, so dass er auf den Dienst verweist.

Es bestehen bei den verschiedenen [Bereitstellungszielen](how-to-deploy-and-where.md) geringfügige Unterschiede beim Schützen von Webdiensten. 

## <a name="get-a-domain-name"></a>Abrufen eines Domänennamens

Wenn Sie nicht bereits über einen Domänennamen verfügen, können Sie einen solchen von einer __Domänennamen-Registrierungsstelle__ erwerben. Der Vorgang unterscheidet sich (ebenso wie die Kosten) abhängig von der Domänenregistrierungsstelle. Die Registrierungsstelle stellt Ihnen außerdem Tools zum Verwalten des Domänennamens zur Verfügung. Diese Tools werden zum Zuordnen eines vollqualifizierten Domänennamens (z.B. www.contoso.com) zur IP-Adresse verwendet, unter der Ihr Dienst gehostet ist.

## <a name="get-an-ssl-certificate"></a>Beziehen eines SSL-Zertifikats

Es gibt viele Möglichkeiten, ein SSL-Zertifikat zu erhalten. Die verbreitetste Methode ist der Erwerb eines Zertifikats von einer __Zertifizierungsstelle__ (CA). Unabhängig davon, wo Sie das Zertifikat abrufen, benötigen Sie die folgenden Dateien:

* Ein __Zertifikat__. Das Zertifikat muss die vollständige Zertifikatkette enthalten und PEM-codiert sein.
* Einen __Schlüssel__. Der Schlüssel muss PEM-codiert sein.

Wenn Sie ein Zertifikat anfordern, müssen Sie den vollqualifizierten Domänennamen (FQDN) der Adresse angeben, die Sie für den Webdienst verwenden möchten. Beispielsweise www.contoso.com. Die in das Zertifikat gestempelte Adresse und die von den Clients verwendete Adresse werden verglichen, wenn die Identität des Webdiensts überprüft wird. Wenn die Adressen nicht übereinstimmen, erhalten die Clients einen Fehler. 

> [!TIP]
> Wenn die Zertifizierungsstelle das Zertifikat und den Schlüssel nicht als PEM-codierte Dateien bereitstellen kann, können Sie ein Hilfsprogramm wie [OpenSSL](https://www.openssl.org/) zum Ändern des Formats verwenden.

> [!IMPORTANT]
> Selbstsignierte Zertifikate sollten nur für die Entwicklung verwendet werden. Sie sollten nicht in der Produktion eingesetzt werden. Wenn Sie ein selbstsigniertes Zertifikat verwenden, finden Sie spezifische Anweisungen im Abschnitt [Nutzen von Webdiensten mit selbstsignierten Zertifikaten](#self-signed).


## <a name="enable-ssl-and-deploy"></a>Aktivieren von SSL und Bereitstellen

Legen Sie zum Bereitstellen (oder erneuten Bereitstellen) des Diensts mit aktiviertem SSL den `ssl_enabled`-Parameter auf `True` fest, wenn zutreffend. Legen Sie den `ssl_certificate`-Parameter auf den Wert der __Zertifikat__datei und den `ssl_key` auf den Wert der __Schlüssel__datei fest. 

+ **Bereitstellen auf Azure Kubernetes Service (AKS)**
  
  Geben Sie beim Bereitstellen des AKS-Clusters Werte für die auf SSL bezogenen Parameter ein, wie in diesem Codeausschnitt gezeigt:

    ```python
    from azureml.core.compute import AksCompute
    
    provisioning_config = AksCompute.provisioning_configuration(ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Bereitstellen auf Azure Container Instances (ACI)**
 
  Geben Sie beim Bereitstellen auf ACI Werte für die auf SSL bezogenen Parameter ein, wie in diesem Codeausschnitt gezeigt:

    ```python
    from azureml.core.webservice import AciWebservice
    
    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Bereitstellen auf Field-Programmable Gate Arrays (FPGA)**

  Die Antwort vom `create_service`-Vorgang enthält die IP-Adresse des Diensts. Die IP-Adresse wird verwendet, wenn der DNS-Name der IP-Adresse des Diensts zugeordnet wird. Die Antwort enthält auch einen __Primärschlüssel__ und einen __Sekundärschlüssel__. Diese werden verwendet, um den Dienst zu nutzen. Geben Sie Werte für die auf SSL bezogenen Parameter ein, wie in diesem Codeausschnitt gezeigt:

    ```python
    from amlrealtimeai import DeploymentClient
    
    subscription_id = "<Your Azure Subscription ID>"
    resource_group = "<Your Azure Resource Group Name>"
    model_management_account = "<Your AzureML Model Management Account Name>"
    location = "eastus2"
    
    model_name = "resnet50-model"
    service_name = "quickstart-service"
    
    deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account, location)
    
    with open('cert.pem','r') as cert_file:
        with open('key.pem','r') as key_file:
            cert = cert_file.read()
            key = key_file.read()
            service = deployment_client.create_service(service_name, model_id, ssl_enabled=True, ssl_certificate=cert, ssl_key=key)
    ```

## <a name="update-your-dns"></a>Aktualisieren Ihres DNS

Als Nächstes müssen Sie Ihren DNS aktualisieren, so dass er auf den Dienst verweist.

+ **Für ACI und FPGA**:  

  Verwenden Sie die von Ihrer Domänennamen-Registrierungsstelle bereitgestellten Tools, um den DNS-Eintrag für Ihren Domänennamen zu aktualisieren. Der Eintrag muss auf die IP-Adresse des Diensts verweisen.  

  Abhängig von der Registrierungsstelle und dem TTL-Wert (Time-to-Live), der für den Domänennamen konfiguriert ist, kann es einige Minuten bis mehrere Stunden dauern, bevor Clients den Domänennamen auflösen können.

+ **Für AKS**: 

  Aktualisieren Sie den DNS auf der Registerkarte „Konfiguration“ unter „öffentliche IP-Adresse“ des AKS-Clusters, wie in der Abbildung dargestellt. Sie finden die öffentliche IP-Adresse als einen der Ressourcentypen, die unter der Ressourcengruppe erstellt wurden, die die AKS-Agent-Knoten und weitere Netzwerkressourcen enthält.

  ![Azure Machine Learning-Dienst: Schützen von Webdiensten mit SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="consume-authenticated-services"></a>Nutzen authentifizierter Dienste

### <a name="how-to-consume"></a>Nutzen 
+ **Für ACI und AKS**: 

  Informationen zum Nutzen von Webdiensten für ACI- und AKS-Webdienste finden Sie in diesen Artikeln:
  + [Bereitstellen für ACI](how-to-deploy-to-aci.md)

  + [Bereitstellen für AKS](how-to-deploy-to-aks.md)

+ **Für FPGA**:  

  In den folgenden Beispielen wird gezeigt, wie ein authentifizierter FPGA-Dienst in Python und C# genutzt wird.
  Ersetzen Sie `authkey` durch den Primär- oder Sekundärschlüssel, der beim Bereitstellen des Diensts zurückgegeben wurde.

  Beispiel für Python:
    ```python
    from amlrealtimeai import PredictionClient
    client = PredictionClient(service.ipAddress, service.port, use_ssl=True, access_token="authKey")
    image_file = R'C:\path_to_file\image.jpg'
    results = client.score_image(image_file)
    ```

  C#-Beispiel:
    ```csharp
    var client = new ScoringClient(host, 50051, useSSL, "authKey");
    float[,] result;
    using (var content = File.OpenRead(image))
        {
            IScoringRequest request = new ImageRequest(content);
            result = client.Score<float[,]>(request);
        }
    ```

### <a name="set-the-authorization-header"></a>Festlegen des Autorisierungsheaders
Andere gRPC-Clients können Anforderungen durch Festlegen eines Autorisierungsheaders authentifizieren. Die allgemeine Vorgehensweise besteht im Erstellen eines `ChannelCredentials`-Objekts, das `SslCredentials` mit `CallCredentials` kombiniert. Dieses wird dem Autorisierungsheader der Anforderung hinzugefügt. Weitere Informationen zum Implementieren von Unterstützung für Ihre jeweiligen Header finden Sie unter [https://grpc.io/docs/guides/auth.html](https://grpc.io/docs/guides/auth.html).

Die folgenden Beispiele veranschaulichen, wie die Header in C# und Go festgelegt werden:

+ Verwenden Sie C#, um den Header festzulegen:
    ```csharp
    creds = ChannelCredentials.Create(baseCreds, CallCredentials.FromInterceptor(
                          async (context, metadata) =>
                          {
                              metadata.Add(new Metadata.Entry("authorization", "authKey"));
                              await Task.CompletedTask;
                          }));
    
    ```

+ Verwenden Sie Go, um den Header festzulegen:
    ```go
    conn, err := grpc.Dial(serverAddr, 
        grpc.WithTransportCredentials(credentials.NewClientTLSFromCert(nil, "")),
        grpc.WithPerRPCCredentials(&authCreds{
        Key: "authKey"}))
    
    type authCreds struct {
        Key string
    }
    
    func (c *authCreds) GetRequestMetadata(context.Context, uri ...string) (map[string]string, error) {
        return map[string]string{
            "authorization": c.Key,
        }, nil
    }
    
    func (c *authCreds) RequireTransportSecurity() bool {
        return true
    }
    ```

<a id="self-signed"></a>

## <a name="consume-services-with-self-signed-certificates"></a>Nutzen von Diensten mit selbstsignierten Zertifikaten

Es gibt zwei Möglichkeiten, um den Client für die Authentifizierung mit einem Server zu aktivieren, der mit einem selbstsignierten Zertifikat gesichert ist:

* Legen Sie auf dem Clientsystem die `GRPC_DEFAULT_SSL_ROOTS_FILE_PATH`-Umgebungsvariable für das Clientsystem so fest, dass sie auf die Zertifikatdatei verweist.

* Übergeben Sie beim Generieren eines `SslCredentials`-Objekts den Inhalt der Zertifikatdatei an den Konstruktor.

Beide Methoden bewirken, dass gRPC das Zertifikat als Stammzertifikat verwendet.

> [!IMPORTANT]
> gRPC akzeptiert keine nicht vertrauenswürdigen Zertifikate. Das Verwenden eines nicht vertrauenswürdigen Zertifikats führt zu einem Fehler mit dem Statuscode `Unavailable`. Die Details des Fehlers enthalten die Angabe `Connection Failed`.
