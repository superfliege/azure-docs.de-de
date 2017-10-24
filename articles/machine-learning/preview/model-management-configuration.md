---
title: Einrichtung und Konfiguration der Azure Machine Learning-Modellverwaltung | Microsoft-Dokumentation
description: Dieses Dokument beschreibt die Schritte und Konzepte zur Einrichtung und Konfiguration der Modellverwaltung in Azure Machine Learning.
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 08/29/2017
ms.openlocfilehash: c89596a6d721c4cba899b8a6e2859ee36cba7b80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="model-management-setup"></a>Einrichtung der Modellverwaltung

## <a name="overview"></a>Übersicht
Dieses Dokument erklärt Ihnen die ersten Schritte mit der Azure ML-Modellverwaltung zum Bereitstellen und Verwalten Ihrer Machine Learning-Modelle als Webdienste. 

Mithilfe der Azure ML-Modellverwaltung können Sie Machine Learning-Modelle, die mit einer Reihe von Frameworks wie SparkML, Keras, TensorFlow, Microsoft Cognitive Toolkit oder Python erstellt wurden, effizient implementieren und verwalten. 

Am Ende dieses Dokuments sollte Ihre Modellverwaltungsumgebung eingerichtet und für die Bereitstellung Ihrer Machine Learning-Modelle bereit sein.

## <a name="what-you-need-to-get-started"></a>Voraussetzungen
Um diesen Leitfaden bestmöglich zu nutzen, sollten Sie als Besitzer Zugriff auf ein Azure-Abonnement haben, in dem Sie Ihre Modelle bereitstellen können.
Die CLI ist in Azure Machine Learning Workbench und [Azure DSVMs](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-virtual-machine-overview) (Data Science Virtual Machines) vorinstalliert.

## <a name="using-the-cli"></a>Verwenden der CLI
Um die CLIs (Befehlszeilenschnittstellen) in der Workbench zu nutzen, klicken Sie auf **Datei** > **Befehlszeilenschnittstelle öffnen**. 

Stellen Sie auf einer DSVM eine Verbindung mit der Eingabeaufforderung her, und öffnen Sie sie. Geben Sie `az ml -h` ein, um die Optionen anzuzeigen. Geben Sie zum Anzeigen weiterer Informationen zu den Befehlen das Flag „--help“ ein.

Auf anderen Systemen müssen Sie die CLIs installieren.

### <a name="installing-or-updating-on-windows"></a>Installieren (oder Aktualisieren) unter Windows

Installieren Sie Python über https://www.python.org/. Stellen Sie sicher, dass Sie das Installieren von „pip“ ausgewählt haben.

Öffnen Sie eine Eingabeaufforderung mithilfe der Option „Als Administrator ausführen“, und führen Sie die folgenden Befehle aus:

```cmd
pip install azure-cli
pip install azure-cli-ml
```
 
>[!NOTE]
>Wenn Sie eine frühere Version haben, deinstallieren Sie sie zunächst mit dem folgenden Befehl:
>

```cmd
pip uninstall azure-cli-ml
```

### <a name="installing-or-updating-on-linux"></a>Installieren (oder Aktualisieren) unter Linux
Führen Sie den folgenden Befehl an der Befehlszeile aus, und folgen Sie den Anweisungen:

```bash
sudo -i
pip install azure-cli
pip install azure-cli-ml
```

Führen Sie im Anschluss an die Installation den folgenden Befehl aus:

```bash
sudo /opt/microsoft/azureml/initial_setup.sh
```

>[!NOTE]
>Melden Sie sich von Ihrer SSH-Sitzung ab und dann wieder an, damit die Änderungen wirksam werden.
>Sie können die vorherigen Befehle verwenden, um auf der DSVM eine frühere Version zu aktualisieren.
>

## <a name="deploying-your-model"></a>Bereitstellen des Modells
Verwenden Sie die CLIs, um Modelle als Webdienste bereitzustellen. Die Webdienste können lokal oder in einem Cluster bereitgestellt werden.

Beginnen Sie mit einer lokalen Bereitstellung. Überprüfen Sie, ob Ihr Modell und Code funktionieren. Stellen Sie dann den Webdienst in einem Cluster für den Einsatz in der Produktionsumgebung bereit.

Zuallererst müssen Sie Ihre Bereitstellungsumgebung einrichten. Dieser Vorgang muss nur einmal erfolgen. Nachdem die Einrichtung abgeschlossen ist, können Sie die Umgebung für nachfolgende Bereitstellungen wiederverwenden. Im folgenden Abschnitt finden Sie weitere Details.

Schritte im Anschluss an die Einrichtung der Umgebung:
- Sie werden aufgefordert, sich bei Azure anzumelden. Öffnen Sie dazu in einem Webbrowser die Seite https://aka.ms/devicelogin, und geben Sie zur Authentifizierung den angegebenen Code ein.
- Während des Authentifizierungsvorgangs werden Sie für die Authentifizierung zur Angabe eines Kontos aufgefordert. Wichtig: Wählen Sie ein Konto mit einem gültigen Azure-Abonnement und ausreichenden Berechtigungen zum Erstellen von Ressourcen im Konto aus. Wenn die Anmeldung abgeschlossen ist, werden Ihre Abonnementinformationen angezeigt, und Sie werden gefragt, ob Sie mit dem ausgewählten Konto fortfahren möchten.

### <a name="environment-setup"></a>Einrichten der Umgebung
Zum Starten des Einrichtungsvorgangs müssen Sie den Umgebungsanbieter registrieren, indem Sie folgenden Befehl eingeben:

```azurecli
az provider register -n Microsoft.MachineLearningCompute
```

#### <a name="local-deployment"></a>Lokale Bereitstellung
Um Ihren Webdienst auf dem lokalen Computer zu installieren und zu testen, richten Sie mit folgendem Befehl eine lokale Umgebung ein:

```azurecli
az ml env setup -l [location of Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>Die lokale Webdienstbereitstellung erfordert die Installation von Docker auf dem lokalen Computer. 
>

Der Befehl zum Einrichten der lokalen Umgebung erstellt die folgenden Ressourcen in Ihrem Abonnement:
- Eine Ressourcengruppe (falls nicht angegeben)
- Ein Speicherkonto
- Eine Azure Container Registry (ACR)
- Application Insights

Legen Sie nach erfolgreichem Abschluss der Einrichtung die zu verwendende Umgebung mit dem folgenden Befehl fest:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>Clusterbereitstellung
Wählen Sie die Clusterbereitstellung für Produktionsszenarien in großem Maßstab. Ein ACS-Cluster mit Kubernetes als Orchestrator wird eingerichtet. Der ACS-Cluster kann horizontal hochskaliert werden, um für Aufrufe Ihres Webdiensts einen größeren Durchsatz zu bewältigen.

Um den Webdienst in einer Produktionsumgebung bereitzustellen, legen Sie zuerst die Umgebung mit dem folgenden Befehl fest:

```azurecli
az ml env setup -c --name [your environment name] --location [Azure region e.g. eastus2] [-g [resource group]]
```

Der Befehl zum Einrichten der Clusterumgebung erstellt die folgenden Ressourcen in Ihrem Abonnement:
- Eine Ressourcengruppe (falls nicht angegeben)
- Ein Speicherkonto
- Eine Azure Container Registry (ACR)
- Eine Kubernetes-Bereitstellung in einem Azure Container Service-Cluster (ACS)
- Application Insights

Die Ressourcengruppe, das Speicherkonto und die ACR werden schnell erstellt. Die ACS-Bereitstellung kann bis zu 20 Minuten dauern. 

Nach Abschluss der Einrichtung müssen Sie die für diese Bereitstellung zu verwendende Umgebung festlegen. Verwenden Sie den folgenden Befehl:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> Nach Erstellen der Umgebung müssen Sie für nachfolgende Bereitstellungen nur den oben genannten Befehl „set“ angeben, um sie wiederzuverwenden.
>

>[!NOTE] 
>Geben Sie ein SSL-Zertifikat an, um einen HTTPS-Endpunkt zu erstellen, wenn Sie einen Cluster mit den Optionen „--cert“ und „--cert-perm“ in „az ml env setup“ verwenden. So behandelt der Cluster Anforderungen in HTTPS, das durch das bereitgestellte Zertifikat gesichert wird. Erstellen Sie, nachdem die Einrichtung abgeschlossen wurde, einen CNAME DNS-Datensatz, der auf den vollqualifizierten Domänennamen (FQDN) des Clusters zeigt.

### <a name="create-an-account"></a>Erstellen eines Kontos
Für die Bereitstellung von Modellen ist ein Konto erforderlich. Dieser Schritt muss pro Konto einmal erfolgen. Sie können dasselbe Konto in mehreren Bereitstellungen wiederverwenden.

Verwenden Sie den folgenden Befehl, um ein neues Konto zu erstellen:

```azurecli
az ml account modelmanagement create -l [Azure region, e.g. eastus2] -n [your account name] -g [resource group name] --sku-instances [number of instances, e.g. 1] --sku-name [Pricing tier for example S1]
```

Um ein vorhandenes Konto zu verwenden, geben Sie den folgenden Befehl ein:
```azurecli
az ml account modelmanagement set -n [your account name] -g [resource group it was created in]
```

### <a name="deploy-your-model"></a>Bereitstellen Ihres Modells
Sie können nun Ihr gespeichertes Modell als Webdienst bereitstellen. 

```azurecli
az ml service create realtime --model-file [model file/folder path] -f [scoring file e.g. score.py] -n [your service name] -s [schema file e.g. service_schema.json] -r [runtime for the Docker container e.g. spark-py or python] -c [conda dependencies file for additional python packages]
```

### <a name="next-steps"></a>Nächste Schritte
Probieren Sie eines der vielen Beispiele im Katalog aus.
