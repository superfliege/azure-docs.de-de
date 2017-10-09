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
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 61ecea71874b05c2c5f7572aa6128fc320422b1f
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="model-management-setup"></a>Einrichtung der Modellverwaltung

Dieses Dokument erklärt Ihnen die ersten Schritte mit der Azure ML-Modellverwaltung zum Bereitstellen und Verwalten Ihrer Machine Learning-Modelle als Webdienste. 

Mithilfe der Azure ML-Modellverwaltung können Sie Machine Learning-Modelle, die mit einer Reihe von Frameworks wie SparkML, Keras, TensorFlow, Microsoft Cognitive Toolkit oder Python erstellt wurden, effizient implementieren und verwalten. 

Am Ende dieses Dokuments sollte Ihre Modellverwaltungsumgebung eingerichtet und für die Bereitstellung Ihrer Machine Learning-Modelle bereit sein.

## <a name="what-you-need-to-get-started"></a>Voraussetzungen
Um diesen Leitfaden bestmöglich zu nutzen, sollten Sie als Besitzer Zugriff auf ein Azure-Abonnement haben, in dem Sie Ihre Modelle bereitstellen können.
Die CLI ist in Azure Machine Learning Workbench und auf [Azure-DSVMs](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-virtual-machine-overview) (Data Science Virtual Machines) vorinstalliert.

## <a name="using-the-cli"></a>Verwenden der CLI
Um die CLIs (Befehlszeilenschnittstellen) in Workbench zu nutzen, klicken Sie auf **Datei** -> **Eingabeaufforderung öffnen**. 

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
Führen Sie den folgenden Befehl über die Befehlszeile aus, und folgen Sie den Anweisungen:

```bash
sudo -i
pip install azure-cli
pip install azure-cli-ml
```

### <a name="configuring-docker-on-linux"></a>Konfigurieren von Docker unter Linux
Befolgen Sie zum Konfigurieren von Docker unter Linux für die Verwendung von Nichtstammbenutzern die Anweisungen unter [Post-installation steps for Linux](https://docs.docker.com/engine/installation/linux/linux-postinstall/) (Schritte nach der Installation für Linux).

>[!NOTE]
> Auf einer Linux-DSVM können Sie das nachfolgende Skript ausführen, um Docker ordnungsgemäß zu konfigurieren. **Denken Sie daran, sich nach der Ausführung des Skripts ab-und wieder anzumelden.**
>```
>sudo /opt/microsoft/azureml/initial_setup.sh
>```

## <a name="deploying-your-model"></a>Bereitstellen des Modells
Verwenden Sie die CLI, um Modelle als Webdienste bereitzustellen. Die Webdienste können lokal oder in einem Cluster bereitgestellt werden.

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
Um Ihren Webdienst auf dem lokalen Computer zu installieren und zu testen, richten Sie mit folgendem Befehl eine lokale Umgebung ein. Der Name der Ressourcengruppe ist optional.

```azurecli
az ml env setup -l [Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>Die lokale Webdienstbereitstellung erfordert die Installation von Docker auf dem lokalen Computer. 
>

Der Befehl zum Einrichten der lokalen Umgebung erstellt die folgenden Ressourcen in Ihrem Abonnement:
- Eine Ressourcengruppe (sofern keine angegeben bzw. der angegebene Name nicht vorhanden ist)
- Ein Speicherkonto
- Eine Azure Container Registry-Instanz (ACR)
- Ein Application Insights-Konto

Legen Sie nach erfolgreichem Abschluss der Einrichtung die zu verwendende Umgebung mit dem folgenden Befehl fest:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>Clusterbereitstellung
Verwenden Sie Clusterbereitstellung für Produktionsszenarien in großem Maßstab. Ein ACS-Cluster mit Kubernetes als Orchestrator wird eingerichtet. Der ACS-Cluster kann horizontal hochskaliert werden, um für Aufrufe Ihres Webdiensts einen größeren Durchsatz zu bewältigen.

Um den Webdienst in einer Produktionsumgebung bereitzustellen, legen Sie zuerst die Umgebung mit dem folgenden Befehl fest:

```azurecli
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. eastus2] [-g [resource group]]
```

Der Befehl zum Einrichten der Clusterumgebung erstellt die folgenden Ressourcen in Ihrem Abonnement:
- Eine Ressourcengruppe (sofern keine angegeben bzw. der angegebene Name nicht vorhanden ist)
- Ein Speicherkonto
- Eine Azure Container Registry-Instanz (ACR)
- Eine Kubernetes-Bereitstellung in einem Azure Container Service-Cluster (ACS)
- Ein Application Insights-Konto

>[!IMPORTANT]
> Zum Erstellen einer Clusterumgebung müssen Sie ein Besitzer im Azure-Abonnement sein und außerdem einen Dienstprinzipal erstellen können. Um zu überprüfen, ob Sie über ausreichende Berechtigungen verfügen, befolgen Sie die Anweisungen auf dieser Seite: [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal).

Die Ressourcengruppe, das Speicherkonto und die ACR werden schnell erstellt. Die ACS-Bereitstellung kann bis zu 20 Minuten dauern. 

Verwenden Sie den folgenden Befehl, um den Status einer aktuellen Clusterbereitstellung zu überprüfen:

```azurecli
az ml env show -n [environment name] -g [resource group]
```

Nach Abschluss der Einrichtung müssen Sie die für diese Bereitstellung zu verwendende Umgebung festlegen. Verwenden Sie den folgenden Befehl:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> Nach Erstellen der Umgebung müssen Sie für nachfolgende Bereitstellungen nur den oben genannten Befehl „set“ angeben, um sie wiederzuverwenden.
>

### <a name="create-a-model-management-account"></a>Erstellen eines Modellverwaltungskontos
Für die Bereitstellung von Modellen ist ein Modellverwaltungskonto erforderlich. Dieser Schritt muss pro Abonnement einmal erfolgen. Sie können dasselbe Konto in mehreren Bereitstellungen wiederverwenden.

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

## <a name="next-steps"></a>Nächste Schritte
Probieren Sie eines der vielen Beispiele im Katalog aus.

