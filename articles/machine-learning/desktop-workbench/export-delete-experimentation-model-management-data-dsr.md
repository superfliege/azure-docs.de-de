---
title: Exportieren oder Löschen von Experimentier- oder Modellverwaltungsdaten – Azure Machine Learning | Microsoft-Dokumentation
description: In Azure Machine Learning können Sie Kontodaten exportieren oder löschen, die sich auf die Experimentier- und Modellverwaltung mit dem Azure-Portal, der CLI, dem SDK und authentifizierten REST-APIs beziehen. In diesem Artikel erfahren Sie, welche Schritte erforderlich sind.
services: machine-learning
author: cjgronlund
ms.author: cgronlun
manager: haining
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 5475ce3be24321b15ab78a078b758c25843f0ed3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723682"
---
# <a name="export-or-delete-your-experimentation-or-model-management-data-in-machine-learning"></a>Exportieren oder Löschen Ihrer Experimentier- oder Modellverwaltungsdaten in Machine Learning

In Azure Machine Learning Studio können Sie Kontodaten exportieren oder löschen, die sich auf die Experimentier- und Modellverwaltung mit der authentifizierten REST-API beziehen. In diesem Artikel erfahren Sie, wie das geht.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-account-data"></a>Steuern Ihrer Kontodaten
Von der Experimentier- und Modellverwaltung in Azure Machine Learning gespeicherte und im Produkt enthaltene Daten stehen über das Azure-Portal, die CLI, das SDK und authentifizierte REST-APIs zum Exportieren und Löschen zur Verfügung. Auf Telemetriedaten können Sie über das Azure Privacy-Portal zugreifen. 

In Azure Machine Learning bestehen personenbezogene Daten aus Benutzerinformationen in Laufverlaufsdokumenten und Telemetriedatensätzen einiger Benutzerinteraktionen mit dem Dienst.

## <a name="delete-account-data-with-the-rest-api"></a>Löschen von Kontodaten mit der REST-API 

Um Daten zu löschen, können die folgenden API-Aufrufe mit dem HTTP DELETE-Verb vorgenommen werden. Diese werden durch einen `Authorization: Bearer <arm-token>`-Header in der Anforderung autorisiert, wobei `<arm-token>` das AAD-Zugriffstoken für den Endpunkt `https://management.core.windows.net/` ist.  

Wie Sie dieses Token erhalten und Azure-Endpunkte aufrufen, erfahren Sie in der [REST-API-Dokumentation von Azure](https://docs.microsoft.com/rest/api/azure/).  

In den folgenden Beispielen ersetzen Sie den Text in {} durch die Instanznamen, die die zugehörige Ressource bestimmen.

## <a name="delete-from-a-hosting-account"></a>Löschen aller Hostingkonten

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningModelManagement/accounts/{account-name}?api-version=2017-09-01-preview      

## <a name="delete-from-the-model-management-service"></a>Löschen aus dem Modellverwaltungsdienst

### <a name="model-document"></a>Modelldokument
Verwenden Sie diesen Aufruf, um eine Liste der Modelle und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Einzelne Modelle können wie folgt gelöscht werden:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview

### <a name="manifest-document"></a>Manifestdokument
Verwenden Sie diesen Aufruf, um eine Liste der Manifeste und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Einzelne Manifeste können wie folgt gelöscht werden:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="service-documents"></a>Dienstdokumente
Verwenden Sie diesen Aufruf, um eine Liste der Dienste und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Einzelne Dienste können wie folgt gelöscht werden:    

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="image-document"></a>Imagedokument
Verwenden Sie diesen Aufruf, um eine Liste der Images und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Einzelne Images können wie folgt gelöscht werden:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview

## <a name="delete-run-history-artifact-and-notification-data"></a>Löschen von Ausführungsverlaufs-, Artefakt- und Benachrichtigungsdaten
Alle für ein Projekt gespeicherten Ausführungsverlaufs-, Artefakt- und Benachrichtigungsdaten werden nach dem Löschen des entsprechenden Projektdokuments gelöscht:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview
    
## <a name="delete-from-experimentation-account-resource-provider"></a>Löschen aus Ressourcenanbieter für Experimentierkonto
Projektdokumente werden wie folgt gelöscht:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview

Arbeitsbereichsdokumente werden wie folgt gelöscht:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}?api-version=2017-05-01-preview

Das gesamte Experimentierkonto wird wie folgt gelöscht:
    
    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}?api-version=2017-05-01-preview


## <a name="export-service-data-with-the-rest-api"></a>Exportieren von Dienstdaten mit der REST-API
Um Daten zu exportieren, können die folgenden API-Aufrufe mit dem HTTP GET-Verb vorgenommen werden. Diese werden durch einen `Authorization: Bearer <arm-token>`-Header in der Anforderung autorisiert, wobei `<arm-token>` das AAD-Zugriffstoken für den Endpunkt `https://management.core.windows.net/` ist.  

Wie Sie dieses Token erhalten und Azure-Endpunkte aufrufen, erfahren Sie in der [REST-API-Dokumentation von Azure](https://docs.microsoft.com/rest/api/azure/).   

In den folgenden Beispielen ersetzen Sie den Text in {} durch die Instanznamen, die die zugehörige Ressource bestimmen.

## <a name="export-hosting-account-data"></a>Exportieren von Hostingkontodaten

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningModelManagement/accounts/{accountName}?api-version=2017-09-01-preview     

## <a name="export-model-management-service-data"></a>Exportieren von Daten des Modellverwaltungsdiensts
### <a name="model-document"></a>Modelldokument

Verwenden Sie diesen Aufruf, um eine Liste der Modelle und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Einzelne Modelle können wie folgt abgerufen werden:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview 

### <a name="manifests"></a>Manifeste
Verwenden Sie diesen Aufruf, um eine Liste der Manifeste und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Einzelne Manifeste können wie folgt abgerufen werden:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="services"></a>Dienste
Verwenden Sie diesen Aufruf, um eine Liste der Dienste und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Einzelne Dienste können wie folgt abgerufen werden: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="images"></a>Bilder
Verwenden Sie diesen Aufruf, um eine Liste der Images und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Einzelne Dienste können wie folgt abgerufen werden: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview     

## <a name="export-compute-data"></a>Exportieren von Computedaten
### <a name="compute-clusters"></a>Computecluster
Verwenden Sie diesen Aufruf, um eine Liste der Computecluster und ihrer Namen zu erhalten:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2018-03-01-preview

Einzelne Cluster können wie folgt abgerufen werden:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{compute-name}?api-version=2018-03-01-preview

### <a name="operationalization-clusters"></a>Operationalisierungscluster
Verwenden Sie diesen Aufruf, um eine Liste der Cluster und ihrer Namen zu erhalten:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters?api-version=2017-06-01-preview

Einzelne Cluster können wie folgt abgerufen werden:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters/{clusterName}?api-version=2017-06-01-preview

## <a name="export-run-history-data"></a>Exportieren von Ausführungsverlaufsdaten
Verwenden Sie diesen Aufruf, um eine Liste der Ausführungen und ihrer IDs zu erhalten:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs

Verwenden Sie diesen Aufruf, um eine Liste der Experimente und ihrer IDs zu erhalten:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments

Elemente des Ausführungsverlaufs können wie folgt abgerufen werden:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}

Elemente der Ausführungsmetriken können wie folgt abgerufen werden:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runmetrics

Ausführungsexperimente können wie folgt abgerufen werden:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments/{experimentId}    

Ausführungsverlaufsartefakte:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts

URIs von Ausführungsverlaufsartefakten:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacturi?name={artifactName}

## <a name="export-artifacts"></a>Exportieren von Artefakten
Verwenden Sie diesen Aufruf, um eine Liste der Objekte und ihrer Namen zu erhalten:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets

Verwenden Sie diesen Aufruf, um eine Liste der Artefakte und ihrer Pfade zu erhalten:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/origins/{origin}/containers/{runId}
        
        
### <a name="artifact-contents"></a>Artefaktinhalte

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/contentinfo/ExperimentRun/{runId}/{artifactPath}

### <a name="artifact-documents"></a>Artefaktdokumente

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts
        
### <a name="assets"></a>Objekte

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets/name/{name}

## <a name="export-notifications"></a>Exportieren von Benachrichtigungen

1. Wechseln Sie zum [Abschnitt „Benutzer“ des Azure-Portals](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/), und wählen Sie dann einen Benutzer aus der Spalte **Namen** aus. 
2. Notieren Sie die **Objekt-ID**, und verwenden Sie sie im folgenden Aufruf:     

        https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/users/{objectId}/jobs

## <a name="export-experimentation-account-information"></a>Exportieren von Experimentierkontoinformationen
### <a name="experimentation-account-information"></a>Experimentierkontoinformationen

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}?api-version=2017-05-01-preview
        
### <a name="workspace-information"></a>Informationen zum Arbeitsbereich

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}?api-version=2017-05-01-preview  

### <a name="project-information"></a>Projektinformationen

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}?api-version=2017-05-01-preview
