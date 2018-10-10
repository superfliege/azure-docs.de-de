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
ms.date: 09/24/2018
ms.openlocfilehash: ee67585a523ab96b1442d9eee3e9dfd55a758d32
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971483"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning"></a>Bereitstellen eines Modells als Webdienst auf einem FPGA mit Azure Machine Learning

Sie können ein Modell als Webdienst in [Field Programmable Gate Array (FPGAs)](concept-accelerate-with-fpgas.md) bereitstellen.  FPGAs bieten extrem geringe Latenzstörungen, sogar mit einer einzelnen Batchgröße.   

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Ein Azure Machine Learning-Arbeitsbereich und das Azure Machine Learning SDK für Python müssen installiert sein. Informationen zum Erhalten dieser Voraussetzungen finden Sie im Dokument [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).
 
  - Ihr Arbeitsbereich muss sich in der Region *USA, Osten 2* befinden.

  - Installieren Sie die contrib extras:

    ```shell
    pip install --upgrade azureml-sdk[contrib]
    ```  

## <a name="create-and-deploy-your-model"></a>Erstellen und Bereitstellen Ihres Modells
Erstellen Sie eine Pipeline, um das Image vorzuverarbeiten, statten Sie es mit ResNet 50 auf einem FPGA mit Features aus, und führen Sie diese über einen anhand eines ImageNet-Datasets trainierten Klassifizierer aus.

Befolgen Sie die Anweisungen für folgende Punkte:

* Definieren des Pipelinemodells
* Bereitstellen des Modells
* Nutzen des bereitgestellten Modells
* Löschen bereitgestellter Dienste

> [!IMPORTANT]
> Um die Latenzzeit und den Durchsatz zu optimieren, sollte sich Ihren Client in derselben Azure-Region wie der Endpunkt befinden.  Derzeit werden die APIs in der Azure-Region „USA, Osten“ erstellt.

### <a name="get-the-notebook"></a>Abrufen des Notebooks

Dieses Tutorial steht Ihnen auch als Jupyter Notebook zur Verfügung. Verwenden Sie eine dieser Methoden zum Ausführen des `project-brainwave/project-brainwave-quickstart.ipynb` Notebooks:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

### <a name="preprocess-image"></a>Vorverarbeiten des Image
Die erste Stufe der Pipeline ist das Vorverarbeiten der Images.

```python
import os
import tensorflow as tf

# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.contrib.brainwave.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```
### <a name="add-featurizer"></a>Hinzufügen des Featurizer
Initialisieren Sie das Modell und laden Sie einen TensorFlow-Checkpoint der quantisierten Version von ResNet50 herunter, der als Featurizer verwendet werden soll.

```python
from azureml.contrib.brainwave.models import QuantizedResnet50, Resnet50
model_path = os.path.expanduser('~/models')
model = QuantizedResnet50(model_path, is_frozen = True)
feature_tensor = model.import_graph_def(image_tensors)
print(model.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Hinzufügen des Klassifizierers
Dieser Klassifizierer wurde anhand eines ImageNet-Datasets trainiert.

```python
classifier_input, classifier_output = Resnet50.get_default_classifier(feature_tensor, model_path)
```

### <a name="create-service-definition"></a>Erstellen einer Dienstdefinition
Nachdem Sie jetzt die Bildvorverarbeitung, den Featurizer und den Klassifizierer, der auf dem Dienst ausgeführt, definiert haben, können Sie eine Dienstdefinition erstellen. Die Dienstdefinition ist eine Dateiensatz, der aus dem Modell generiert wird, das für den FPGA-Dienst bereitgestellt wird. Die Dienstdefinition besteht aus einer Pipeline. Die Pipeline ist eine Reihe von Stufen, die nacheinander ausgeführt werden.  TensorFlow-, Keras- und BrainWave-Stufen werden unterstützt.  Die Stufen werden nacheinander im Dienst ausgeführt, wobei die Ausgabe jeder Stufe in die nachfolgende Stufe eingeht.

Um eine TensorFlow-Stufe zu erstellen, geben Sie eine Sitzung an, die den Graph (in diesem Fall wird der Standardgraph verwendet) und die Ein- und Ausgabe-Tensoren zu dieser Stufe enthält.  Diese Informationen werden zum Speichern des Graph verwendet, sodass dieser auf dem Dienst ausgeführt werden kann.

```python
from azureml.contrib.brainwave.pipeline import ModelDefinition, TensorflowStage, BrainWaveStage

save_path = os.path.expanduser('~/models/save')
model_def_path = os.path.join(save_path, 'service_def.zip')

model_def = ModelDefinition()
with tf.Session() as sess:
    model_def.pipeline.append(TensorflowStage(sess, in_images, image_tensors))
    model_def.pipeline.append(BrainWaveStage(sess, model))
    model_def.pipeline.append(TensorflowStage(sess, classifier_input, classifier_output))
    model_def.save(model_def_path)
    print(model_def_path)
```

### <a name="deploy-model"></a>Bereitstellen des Modells
Erstellen Sie einen Dienst aus der Dienstdefinition.  Ihr Arbeitsbereich muss sich in der Region „USA, Osten 2“ befinden.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')

from azureml.core.model import Model
model_name = "resnet-50-rtai"
registered_model = Model.register(ws, model_def_path, model_name)

from azureml.core.webservice import Webservice
from azureml.exceptions import WebserviceException
from azureml.contrib.brainwave import BrainwaveWebservice, BrainwaveImage
service_name = "imagenet-infer"
service = None
try:
    service = Webservice(ws, service_name)
except WebserviceException:
    image_config = BrainwaveImage.image_configuration()
    deployment_config = BrainwaveWebservice.deploy_configuration()
    service = Webservice.deploy_from_model(ws, service_name, [registered_model], image_config, deployment_config)
    service.wait_for_deployment(true)
```

### <a name="test-the-service"></a>Testen des Diensts
Um ein Image an die API zu senden und die Antwort zu testen, fügen Sie ein Mapping von der Ausgabeklassen-ID auf den ImageNet-Klassennamen hinzu.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()
```

Rufen Sie Ihren Dienst auf und ersetzen Sie den untenstehenden Dateinamen „ihr-image.jpg“ durch ein Image von Ihrem Computer. 

```python
with open('your-image.jpg') as f:
    results = service.run(f)
# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
``` 

### <a name="clean-up-service"></a>Bereinigen des Dienstes
Löschen Sie den Dienst.

```python
service.delete()
    
registered_model.delete()
```

## <a name="secure-fpga-web-services"></a>Sichere FPGA-Webdienste

Auf FPGAs ausgeführte Azure Machine Learning-Modelle stellen SSL-Unterstützung und schlüsselbasierte Authentifizierung bereit. Dadurch können Sie den Zugriff auf Ihren Dienst einschränken und von Clients übermittelte Daten sichern.

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

### <a name="consume-authenticated-services"></a>Nutzen authentifizierter Dienste

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
