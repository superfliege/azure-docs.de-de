---
title: Bereitstellen eines Modells als Webdienst auf einem FPGA mit Azure Machine Learning
description: Erfahren Sie, wie ein Webdienst mit einem Modell bereitgestellt wird, das auf einem FPGA mit Azure Machine Learning ausgeführt wird.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ms.openlocfilehash: f3237980a1ad1969b5cf8d42d547ddf96608dd97
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning"></a>Bereitstellen eines Modells als Webdienst auf einem FPGA mit Azure Machine Learning

In diesem Dokument erfahren Sie, wie die Arbeitsstationumgebung eingerichtet und ein Modell als Webdienst auf [FPGAs (Field Programmable Gate Arrays)](concept-accelerate-with-fpgas.md) bereitgestellt wird. Der Webdienst verwendet Project Brainwave, um das Modell auf dem FPGA auszuführen.

FPGAs bieten extrem geringe Latenzstörungen, sogar mit einer einzelnen Batchgröße.

## <a name="create-an-azure-machine-learning-model-management-account"></a>Erstellen eines Azure Machine Learning-Modellverwaltungskontos

1. Navigieren Sie im [Azure-Portal](https://aka.ms/aml-create-mma) zur Erstellungsseite für das Modellverwaltungskonto.

2. Erstellen Sie im Portal in der Region **USA, Osten 2** ein Modellverwaltungskonto.

   ![Abbildung des Bildschirms zum Erstellen eines Modellverwaltungskontos](media/how-to-deploy-fpga-web-service/azure-portal-create-mma.PNG)

3. Benennen Sie Ihr Modellverwaltungskonto, und wählen Sie dann ein Abonnement und eine Ressourcengruppe aus.

   >[!IMPORTANT]
   >Als Standort MÜSSEN Sie die Region **USA, Osten 2** auswählen.  Es sind zurzeit keine anderen Regionen verfügbar.

4. Wählen Sie einen Tarif aus (S1 ist ausreichend, aber S2 und S3 funktionieren auch).  Der DevTest-Tarif wird nicht unterstützt.  

5. Klicken Sie auf **Auswählen**, um den Tarif zu bestätigen.

6. Klicken Sie für die ML-Modellverwaltung auf der linken Seite auf **Erstellen**.

## <a name="get-model-management-account-information"></a>Abrufen von Modellverwaltungs-Kontoinformationen

Um Informationen zu Ihrem Modellverwaltungskonto (Model Management Account, MMA) zu erhalten, klicken Sie im Azure-Portal auf das __Modellverwaltungskonto__.

Kopieren Sie die Werte der folgenden Elemente:

+ Name des Modellverwaltungskontos (in der oberen linken Ecke)
+ Ressourcengruppenname
+ Abonnement-ID
+ Standort (verwenden Sie „eastus2“)

![Modellverwaltungs-Kontoinformationen](media/how-to-deploy-fpga-web-service/azure-portal-mma-info.PNG)

## <a name="set-up-your-machine"></a>Einrichten Ihres Computers

Gehen Sie folgendermaßen vor, um Ihre Arbeitsstation für die FPGA-Bereitstellung einzurichten:

1. Laden Sie die aktuelle Version von [Git](https://git-scm.com/downloads) herunter, und installieren Sie sie.

2. Installieren Sie [Anaconda (Python 3.6)](https://conda.io/miniconda.html).

3. Um die Anaconda-Umgebung herunterzuladen, verwenden Sie den folgenden Befehl an einer Git-Eingabeaufforderung:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```

4. Öffnen Sie zum Erstellen der Umgebung eine **Anaconda-Eingabeaufforderung** (keine Azure Machine Learning Workbench-Eingabeaufforderung), und führen Sie den folgenden Befehl aus:

    > [!IMPORTANT]
    > Die Datei „`environment.yml`“ befindet sich im Git-Repository, das Sie im vorherigen Schritt geklont haben. Ändern Sie den Pfad so, dass er auf die Datei auf Ihrer Arbeitsstation verweist.

    ```
    conda env create -f environment.yml
    ```

5. Um die Umgebung zu aktivieren, verwenden Sie den folgenden Befehl:

    ```
    conda activate amlrealtimeai
    ```

6. Um den Jupyter Notebook-Server zu starten, verwenden Sie den folgenden Befehl:

    ```
    jupyter notebook
    ```

    Die Ausgabe dieses Befehls ähnelt dem folgenden Text:

    ```text
    Copy/paste this URL into your browser when you connect for the first time, to login with a token:
        http://localhost:8888/?token=bb2ce89cc8ae931f5df50f96e3a6badfc826ff4100e78075
    ```

    > [!TIP]
    > Sie erhalten bei jeder Ausführung des Befehls ein anderes Token.

    Wenn Ihr Browser nicht automatisch mit dem Jupyter Notebook geöffnet wird, verwenden Sie die vom vorherigen Befehl zurückgegebene HTTP-URL, um die Seite zu öffnen.

    ![Abbildung der Jupyter Notebook-Webseite](./media/how-to-deploy-fpga-web-service/jupyter-notebook.png)

## <a name="deploy-your-model"></a>Bereitstellen Ihres Modells

Öffnen Sie über Jupyter Notebook das `00_QuickStart.ipynb`-Notebook aus dem Verzeichnis „`notebooks/resnet50`“. Befolgen Sie die Anweisungen im Notebook, um die folgenden Aktionen auszuführen:

* Definieren des Diensts
* Bereitstellen des Modells
* Nutzen des bereitgestellten Modells
* Löschen bereitgestellter Dienste

> [!IMPORTANT]
> Um die Latenzzeit und den Durchsatz zu optimieren, sollte sich Ihre Arbeitsstation in derselben Azure-Region wie der Endpunkt befinden.  Derzeit werden die APIs in der Azure-Region „USA, Osten“ erstellt.

## <a name="ssltls-and-authentication"></a>SSL/TLS und Authentifizierung

Azure Machine Learning stellt SSL-Unterstützung und schlüsselbasierte Authentifizierung bereit. Dadurch können Sie den Zugriff auf Ihren Dienst einschränken und von Clients übermittelte Daten sichern.

> [!NOTE]
> Die Schritte in diesem Abschnitt gelten nur für Azure Machine Learning Hardware Accelerated-Modelle. Informationen zu Azure Machine Learning-Standarddiensten finden Sie im Dokument [Einrichten von SSL für Azure Machine Learning Compute](https://docs.microsoft.com/azure/machine-learning/preview/how-to-setup-ssl-on-mlc).

> [!IMPORTANT]
> Authentifizierung ist nur für Dienste aktiviert, die ein SSL-Zertifikat und einen Schlüssel angegeben haben. 
>
> Wenn Sie SSL nicht aktivieren, kann jeder Benutzer im Internet Aufrufe an den Dienst ausführen.
>
> Wenn Sie SSL aktivieren, ist ein Authentifizierungsschlüssel erforderlich, wenn auf den Dienst zugegriffen wird.

SSL verschlüsselt Daten, die zwischen dem Client und dem Dienst gesendet werden. SSL wird auch vom Client zum Überprüfen der Identität des Servers verwendet.

Sie können einen Dienst mit aktiviertem SSL bereitstellen oder einen bereits bereitgestellten Dienst für die Aktivierung aktualisieren. Die Schritte sind identisch:

1. Abrufen eines Domänennamens.

2. Abrufen eines SSL-Zertifikats.

3. Bereitstellen des Diensts mit aktiviertem SSL oder Aktualisieren des Diensts.

4. Aktualisieren Ihres DNS, um auf den Dienst zu verweisen.

### <a name="acquire-a-domain-name"></a>Abrufen eines Domänennamens

Wenn Sie nicht bereits über einen Domänennamen verfügen, können Sie einen solchen von einer __Domänennamen-Registrierungsstelle__ erwerben. Der Vorgang unterscheidet sich (ebenso wie die Kosten) abhängig von der Domänenregistrierungsstelle. Die Registrierungsstelle stellt Ihnen außerdem Tools zum Verwalten des Domänennamens zur Verfügung. Diese Tools werden zum Zuordnen eines vollqualifizierten Domänennamens (z.B. www.contoso.com) zur IP-Adresse verwendet, unter der Ihr Dienst gehostet wird.

### <a name="acquire-an-ssl-certificate"></a>Abrufen eines SSL-Zertifikats

Es gibt viele Möglichkeiten, ein SSL-Zertifikat zu erhalten. Die verbreitetste Methode ist der Erwerb eines Zertifikats von einer __Zertifizierungsstelle__ (CA). Unabhängig davon, wo Sie das Zertifikat abrufen, benötigen Sie die folgenden Dateien:

* Ein __Zertifikat__. Das Zertifikat muss die vollständige Zertifikatkette enthalten und PEM-codiert sein.
* Einen __Schlüssel__. Der Schlüssel muss PEM-codiert sein.

> [!TIP]
> Wenn die Zertifizierungsstelle das Zertifikat und den Schlüssel nicht als PEM-codierte Dateien bereitstellen kann, können Sie ein Hilfsprogramm wie [OpenSSL](https://www.openssl.org/) zum Ändern des Formats verwenden.

> [!IMPORTANT]
> Selbstsignierte Zertifikate sollten nur für die Entwicklung verwendet werden. Sie sollten nicht in der Produktion eingesetzt werden.
>
> Wenn Sie ein selbstsigniertes Zertifikat verwenden, finden Sie spezifische Anweisungen im Abschnitt [Nutzen von Diensten mit selbstsignierten Zertifikaten](#self-signed).

> [!WARNING]
> Wenn Sie ein Zertifikat anfordern, müssen Sie den vollqualifizierten Domänennamen (FQDN) der Adresse angeben, die Sie für den Dienst verwenden möchten. Beispielsweise www.contoso.com. Die in das Zertifikat gestempelte Adresse und die von den Clients verwendete Adresse werden verglichen, wenn die Identität des Diensts überprüft wird.
>
> Wenn die Adressen nicht übereinstimmen, erhalten die Clients einen Fehler. 

### <a name="deploy-or-update-the-service-with-ssl-enabled"></a>Bereitstellen des Diensts mit aktiviertem SSL oder Aktualisieren des Diensts

Legen Sie zum Bereitstellen des Diensts mit aktiviertem SSL den `ssl_enabled`-Parameter auf `True` fest. Legen Sie den `ssl_certificate`-Parameter auf den Wert der __Zertifikat__datei und den `ssl_key` auf den Wert der __Schlüssel__datei fest. Das folgende Beispiel veranschaulicht die Bereitstellung eines Diensts mit aktiviertem SSL:

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

Die Antwort vom `create_service`-Vorgang enthält die IP-Adresse des Diensts. Die IP-Adresse wird verwendet, wenn der DNS-Name der IP-Adresse des Diensts zugeordnet wird.

Die Antwort enthält auch einen __Primärschlüssel__ und einen __Sekundärschlüssel__. Diese werden verwendet, um den Dienst zu nutzen.

### <a name="update-your-dns-to-point-to-the-service"></a>Aktualisieren Ihres DNS, um auf den Dienst zu verweisen

Verwenden Sie die von Ihrer Domänennamen-Registrierungsstelle bereitgestellten Tools, um den DNS-Eintrag für Ihren Domänennamen zu aktualisieren. Der Eintrag muss auf die IP-Adresse des Diensts verweisen.

> [!NOTE]
> Abhängig von der Registrierungsstelle und dem TTL-Wert (Time-to-Live), der für den Domänennamen konfiguriert ist, kann es einige Minuten bis mehrere Stunden dauern, bevor Clients den Domänennamen auflösen können.

### <a name="consuming-authenticated-services"></a>Nutzen authentifizierter Dienste

In den folgenden Beispielen wird gezeigt, wie ein authentifizierter Dienst mit Python und C# genutzt wird:

> [!NOTE]
> Ersetzen Sie `authkey` durch den Primär- oder Sekundärschlüssel, der beim Erstellen des Diensts zurückgegeben wird.

```python
from amlrealtimeai import PredictionClient
client = PredictionClient(service.ipAddress, service.port, use_ssl=True, access_token="authKey")
image_file = R'C:\path_to_file\image.jpg'
results = client.score_image(image_file)
```

```csharp
var client = new ScoringClient(host, 50051, useSSL, "authKey");
float[,] result;
using (var content = File.OpenRead(image))
    {
        IScoringRequest request = new ImageRequest(content);
        result = client.Score<float[,]>(request);
    }
```

Andere gRPC-Clients können Anforderungen durch Festlegen eines Autorisierungsheaders authentifizieren. Die allgemeine Vorgehensweise besteht im Erstellen eines `ChannelCredentials`-Objekts, das `SslCredentials` mit `CallCredentials` kombiniert. Dieses wird dem Autorisierungsheader der Anforderung hinzugefügt. Weitere Informationen zum Implementieren von Unterstützung für Ihre jeweiligen Header finden Sie unter [https://grpc.io/docs/guides/auth.html](https://grpc.io/docs/guides/auth.html).

Die folgenden Beispiele veranschaulichen, wie die Header in C# und Go festgelegt werden:

```csharp
creds = ChannelCredentials.Create(baseCreds, CallCredentials.FromInterceptor(
                      async (context, metadata) =>
                      {
                          metadata.Add(new Metadata.Entry("authorization", "authKey"));
                          await Task.CompletedTask;
                      }));

```

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

### <a id="self-signed"></a>Nutzen von Diensten mit selbstsignierten Zertifikaten

Es gibt zwei Möglichkeiten, um den Client für die Authentifizierung mit einem Server zu aktivieren, der mit einem selbstsignierten Zertifikat gesichert ist:

* Legen Sie auf dem Clientsystem die `GRPC_DEFAULT_SSL_ROOTS_FILE_PATH`-Umgebungsvariable für das Clientsystem so fest, dass sie auf die Zertifikatdatei verweist.

* Übergeben Sie beim Generieren eines `SslCredentials`-Objekts den Inhalt der Zertifikatdatei an den Konstruktor.

Beide Methoden bewirken, dass gRPC das Zertifikat als Stammzertifikat verwendet.

> [!IMPORTANT]
> gRPC akzeptiert keine nicht vertrauenswürdigen Zertifikate. Das Verwenden eines nicht vertrauenswürdigen Zertifikats führt zu einem Fehler mit dem Statuscode `Unavailable`. Die Details des Fehlers enthalten die Angabe `Connection Failed`.
