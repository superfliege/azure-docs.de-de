---
title: Erstellen einer Azure Data Factory mithilfe von Python | Microsoft-Dokumentation
description: Erstellen Sie eine Azure Data Factory, um Daten von einem Speicherort in einem Azure Blob Storage in einen anderen Speicherort im selben Blob Storage zu kopieren.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 09/19/2017
ms.author: jingwang
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 9c6f7c8e052aed2ab5aba2ee54065bf40e81a022
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="create-a-data-factory-and-pipeline-using-python"></a>Erstellen einer Data Factory und Pipeline mithilfe von Python
Azure Data Factory ist ein cloudbasierter Datenintegrationsdienst, mit dem Sie datengesteuerte Workflows in der Cloud erstellen können, um Datenverschiebungen und Datentransformationen zu orchestrieren und zu automatisieren. Mit Azure Data Factory können Sie datengesteuerte Workflows (sogenannte Pipelines) erstellen und planen, die Daten aus unterschiedlichen Datenspeichern erfassen. Die Pipelines können diese Daten mithilfe von Computediensten wie Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics und Azure Machine Learning verarbeiten/transformieren und die Ausgabedaten für Datenspeicher wie Azure SQL Data Warehouse veröffentlichen, damit diese von Business Intelligence-Anwendungen (BI) genutzt werden können. 

Diese Schnellstartanleitung beschreibt, wie Sie Python verwenden, um eine Azure Data Factory zu erstellen. Die Pipeline in dieser Data Factory kopiert Daten aus einem Ordner in einen anderen Ordner in ein und demselben Azure Blob Storage.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* **Azure Storage-Konto**. Sie verwenden den Blob Storage als **Quelldatenspeicher** und als **Senkendatenspeicher**. Wenn Sie kein Azure Storage-Konto besitzen, finden Sie im Artikel [Erstellen eines Speicherkontos](../storage/common/storage-create-storage-account.md#create-a-storage-account) Schritte zum Erstellen eines solchen Kontos. 
* **Erstellen Sie eine Anwendung in Azure Active Directory**, indem Sie [diese Anweisungen](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application) befolgen. Notieren Sie sich die folgenden Werte, die Sie in späteren Schritten benötigen: **Anwendungs-ID**, **Authentifizierungsschlüssel** und **Mandanten-ID**. Weisen Sie die Anwendung der Rolle **Mitwirkender** zu, indem Sie die Anweisungen im gleichen Artikel befolgen. 

### <a name="create-and-upload-an-input-file"></a>Erstellen und Hochladen einer Eingabedatei

1. Starten Sie den Editor. Kopieren Sie den folgenden Text, und speichern Sie ihn als **input.txt**-Datei auf einem Datenträger.
    
    ```
    John|Doe
    Jane|Doe
    ```
2.  Verwenden Sie Tools wie [Azure Storage-Explorer](http://storageexplorer.com/), um den Container **adfv2tutorial** zu erstellen und die Datei „input.txt“ in den Container hochzuladen. 

## <a name="install-the-python-package"></a>Installieren des Python-Pakets
1. Öffnen Sie ein Terminal oder eine Eingabeaufforderung mit Administratorberechtigungen.  
2. Um das Python-Paket für Data Factory zu installieren, führen Sie folgenden Befehl aus:

    ```
    pip install azure-mgmt-datafactory
    ```

    Das [Python SDK für Data Factory](https://github.com/Azure/azure-sdk-for-python) unterstützt Python 2.7, 3.3, 3.4, 3.5 und 3.6.
## <a name="create-a-data-factory-client"></a>Erstellen eines Data Factory-Clients

1. Erstellen Sie eine Datei mit dem Namen **datafactory.py**. Fügen Sie die folgenden Anweisungen ein, um Verweise auf Namespaces hinzuzufügen.
    
    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from msrestazure.azure_cloud import Cloud, CloudEndpoints, CloudSuffixes
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.datafactory import DataFactoryManagementClient
    from azure.mgmt.datafactory.models import *
    import json
    import time    
    ```
2. Fügen Sie die folgenden Funktionen hinzu, die Informationen ausgeben. 

    ```python
    def print_item(group):
      """Print an Azure object instance."""
      print("\tName: {}".format(group.name))
      print("\tId: {}".format(group.id))
      if hasattr(group, 'location'):
          print("\tLocation: {}".format(group.location))
      if hasattr(group, 'tags'):
          print("\tTags: {}".format(group.tags))
      if hasattr(group, 'properties'):
          print_properties(group.properties)
    
    def print_properties(props):
      """Print a ResourceGroup properties instance."""
      if props and hasattr(props, 'provisioning_state') and props.provisioning_state:
          print("\tProperties:")
          print("\t\tProvisioning State: {}".format(props.provisioning_state))
      print("\n\n")    
    ```
3. Fügen Sie der **Main**-Methode den folgenden Code hinzu, der eine Instanz der DataFactoryManagementClient-Klasse erstellt. Sie verwenden dieses Objekt, um die Data Factory, einen verknüpften Dienst, Datasets und eine Pipeline zu erstellen. Sie verwenden dieses Objekt ebenfalls zum Überwachen der Ausführungsdetails der Pipeline.

    ```python   
    subscription_id = '<your subscription ID where the factory resides>'
    credentials = ServicePrincipalCredentials(
            client_id='<yourClientId>',
            secret='<YourPassword>',
            tenant='<YourTenandId>'
    )
    resource_client = ResourceManagementClient(credentials, subscription_id)
    adf_client = DataFactoryManagementClient(credentials, subscription_id)
    
    rg_params = {'location':'eastus'}
    df_params = {'location':'eastus'}
    rg_name = '<Your Resource Group Name>'
    df_name = '<Your Data Factory Name>'
    ```

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der eine **Data Factory** erstellt. 

```python
#Create Resource Group
resource_client.resource_groups.create_or_update(rg_name, rg_params)

#Create Data Factory
df_resource = Factory(location='eastus')
df = adf_client.factories.create_or_update(rg_name, df_name, df_resource)
print_item(df)
while df.provisioning_state != 'Succeeded':
    df = adf_client.factories.get(rg_name, df_name)
    time.sleep(1)
print_item(adf_client.factories.get(rg_name, df_name))
```

## <a name="create-a-linked-service"></a>Erstellen eines verknüpften Diensts

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der einen **verknüpften Azure Storage-Dienst** erstellt.

Um Ihre Datenspeicher und Compute Services mit der Data Factory zu verknüpfen, können Sie verknüpfte Dienste in einer Data Factory erstellen. In dieser Schnellstartanleitung müssen Sie nur einen verknüpften Azure Storage-Dienst für die Quell- und Senkendatenspeicher für den Kopiervorgang erstellen – in diesem Beispiel als „AzureStorageLinkedService“ bezeichnet.

```python
#Create Storage Linked Service
ls_name = 'storageLinkedService'

#Replace Storage String with your credentials
storage_string = SecureString('DefaultEndpointsProtocol=https;AccountName=<replace>;AccountKey=<replace>')

ls_azure_storage = AzureStorageLinkedService(connection_string=storage_string)
ls = adf_client.linked_services.create_or_update(rg_name, df_name, ls_name, ls_azure_storage)
print_item(ls)
```
## <a name="create-datasets"></a>Erstellen von Datasets
In diesem Abschnitt erstellen Sie zwei Datasets: eins für die Quelle und das andere für die Senke.

### <a name="create-a-dataset-for-source-azure-blob"></a>Erstellen eines Datasets für das Azure-Quellblob
Fügen Sie der Main-Methode den folgenden Code hinzu, der ein Azure-Blobdataset erstellt. Weitere Informationen zu den Eigenschaften eines Azure-Blobdatasets finden Sie im Artikel [Azure-Blobconnector](connector-azure-blob-storage.md#dataset-properties). 

Sie definieren ein Dataset, das die Quelldaten im Azure-Blob darstellt. Dieses Blobdataset verweist auf den verknüpften Azure Storage-Dienst, den Sie im vorherigen Schritt erstellt haben.

```python
#Create Dataset Input
ds_name = 'ds_in'
ds_ls = LinkedServiceReference(ls_name)
blob_path= 'adfv2tutorial/'
blob_filename = 'input.txt'
ds_azure_blob= AzureBlobDataset(ds_ls, folder_path=blob_path, file_name = blob_filename)
ds = adf_client.datasets.create_or_update(rg_name, df_name, ds_name, ds_azure_blob)
print_item(ds)
```

### <a name="create-a-dataset-for-sink-azure-blob"></a>Erstellen eines Datasets für das Azure-Senkenblob
Fügen Sie der Main-Methode den folgenden Code hinzu, der ein Azure-Blobdataset erstellt. Weitere Informationen zu den Eigenschaften eines Azure-Blobdatasets finden Sie im Artikel [Azure-Blobconnector](connector-azure-blob-storage.md#dataset-properties). 

Sie definieren ein Dataset, das die Quelldaten im Azure-Blob darstellt. Dieses Blobdataset verweist auf den verknüpften Azure Storage-Dienst, den Sie im vorherigen Schritt erstellt haben.

```python
#Create Dataset Output
dsOut_name = 'ds_out'
output_blobpath = 'output/'
dsOut_azure_blob = AzureBlobDataset(ds_ls, folder_path=output_blobpath)
dsOut = adf_client.datasets.create_or_update(rg_name, df_name, dsOut_name, dsOut_azure_blob)
print_item(dsOut)
```

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der eine **Pipeline mit einer Kopieraktivität** erstellt.

```python
#Create 1st activity: Copy Activity
act_name =  'copyBlobtoBlob'
blob_source = BlobSource()
blob_sink = BlobSink()
dsin_ref = DatasetReference(ds_name)
dsOut_ref = DatasetReference(dsOut_name)
copy_activity = CopyActivity(act_name,inputs=[dsin_ref], outputs=[dsOut_ref], source=blob_source, sink=blob_sink)

#Create Pipeline
p_name =  'copyPipeline'
params_for_pipeline = {}
p_obj = PipelineResource(activities=[copy_activity], parameters=params_for_pipeline)
p = adf_client.pipelines.create_or_update(rg_name, df_name, p_name, p_obj)
print_item(p)
```


## <a name="create-a-pipeline-run"></a>Erstellen einer Pipelineausführung

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der eine **Pipelineausführung auslöst**.

```python
#Create Pipeline Run
print(adf_client.pipelines.create_run(rg_name, df_name, p_name,
    {
    }
))
```

## <a name="run-the-code"></a>Ausführen des Codes
Erstellen und starten Sie die Anwendung, und überprüfen Sie dann die Pipelineausführung.

Die Konsole gibt den Status der Erstellung der Data Factory, des verknüpften Diensts, der Datasets, der Pipeline und der Pipelineausführung aus. Warten Sie, bis Sie die Ausführungsdetails der Kopieraktivität mit der Größe der gelesenen/geschriebenen Daten sehen. Verwenden Sie dann Tools wie z.B. [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/), um zu überprüfen, ob die Blobs wie von Ihnen in den Variablen angegeben von „inputBlobPath“ nach „outputBlobPath“ kopiert werden.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Um die Data Factory programmgesteuert zu löschen, fügen Sie die folgenden Codezeilen zum Programm hinzu: 

```csharp
adf_client.data_factories.delete(rg_name, df_name)
```

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in einem Azure Blob Storage von einem Speicherort in einen anderen. Arbeiten Sie die [Tutorials](tutorial-copy-data-dot-net.md) durch, um zu erfahren, wie Sie Data Factory in anderen Szenarien verwenden können. 
