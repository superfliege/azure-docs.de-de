---
title: "ZIP-Push-Bereitstellung für Azure Functions | Microsoft-Dokumentation"
description: "Verwenden Sie die Funktionen zur Bereitstellung von ZIP-Dateien des Kudu-Bereitstellungdiensts zum Veröffentlichen Ihrer Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/06/2017
ms.author: glenga
ms.openlocfilehash: faddb73522200f60f18294dc43e8d235943f8bbb
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2017
---
# <a name="zip-push-deployment-for-azure-functions"></a>ZIP-Push-Bereitstellung für Azure Functions 
In diesem Artikel wird beschrieben, wie Sie Ihre Projektdateien für Funktions-Apps aus einer ZIP-Datei (komprimierten Datei) in Azure bereitstellen. Sie erfahren, wie Push-Bereitstellungen ausgeführt werden, sowohl mithilfe der Azure-Befehlszeilenschnittstelle als auch mithilfe der REST-APIs. 

Azure Functions bietet die ganze Bandbreite an Optionen für Continuous Deployment und Integration, die in Azure App Service zur Verfügung stehen. Weitere Informationen finden Sie unter [Continuous Deployment für Azure Functions](functions-continuous-deployment.md). 

Um während der Entwicklung eine schnellere Iteration zu erzielen, ist es oft einfacher, die Projektdateien Ihrer Funktions-Apps direkt aus einer komprimierten ZIP-Datei bereitzustellen. Bei dieser Bereitstellung per ZIP-Datei wird der gleiche Kudu-Dienst verwendet, der auch bei der Continuous Integration-basierten Bereitstellungen zum Einsatz kommt, einschließlich dieser Funktionalität:

+ Löschen von Dateien, die von früheren Bereitstellungen übrig geblieben sind
+ Anpassen der Bereitstellung, einschließlich der Ausführung von Bereitstellungsskripts
+ Bereitstellungsprotokolle
+ Synchronisierung von Funktionstriggern in einer [Verbrauchstarif](functions-scale.md)-Funktions-App

Weitere Informationen finden Sie in der [ZIP Push-Bereitstellungsreferenz](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

## <a name="deployment-zip-file-requirements"></a>Anforderungen an ZIP-Bereitstellungsdateien
Die ZIP-Datei, die Sie für die Push-Bereitstellung verwenden, muss alle Projektdateien Ihrer Funktions-App enthalten, einschließlich Ihres Funktions-Codes. 

>[!IMPORTANT]
> Wenn Sie ZIP-Push-Bereitstellung verwenden, werden alle Dateien aus einer vorhandenen Bereitstellung, die nicht in der ZIP-Datei vorhanden sind, aus Ihrer Funktions-App gelöscht.  

### <a name="function-app-folder-structure"></a>Funktions-App – Ordnerstruktur

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

### <a name="download-your-function-app-project"></a>Herunterladen Ihres Funktions-App-Projekts

Wenn Sie auf einem lokalen Computer entwickeln, ist es einfach, eine ZIP-Datei des Projektordners der Funktions-App auf Ihrem Entwicklungscomputer zu erstellen. 

Allerdings haben Sie Ihre Funktionen möglicherweise mithilfe des Editors im Azure-Portal erstellt. So laden Sie Ihr Funktions-App-Projekt aus dem Portal herunter: 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie dann zu Ihrer Funktions-App.

2. Wählen Sie auf der Registerkarte **Übersicht** den Befehl **App-Inhalt herunterladen** aus. Wählen Sie Ihre Downloadoptionen und anschließend **Herunterladen** aus.     

    ![Herunterladen des Funktions-App-Projekts](./media/deployment-zip-push/download-project.png)

Die heruntergeladene ZIP-Datei weist das richtige Format auf, um mithilfe der ZIP-Push-Bereitstellung wieder in Ihrer Funktions-App veröffentlicht zu werden.

Sie können aber auch eine ZIP-Datei aus einem GitHub-Repository herunterladen. Beachten Sie, dass GitHub beim Herunterladen eines GitHub-Repositorys als ZIP-Datei eine zusätzliche Ordnerebene für den Branch hinzufügt. Diese zusätzliche Ordnerebene bedeutet, dass Sie die ZIP-Datei nicht direkt so, wie Sie sie von GitHub heruntergeladen haben, bereitstellen können. Wenn Sie ein GitHub-Repository zum Verwalten Ihrer Funktions-App verwenden, sollten Sie die App mithilfe von [Continuous Integration](functions-continuous-deployment.md) bereitstellen.  

## <a name="cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle

Sie können eine Push-Bereitstellung auch mithilfe der Azure-Befehlszeilenschnittstelle auslösen. Führen Sie eine Push-Bereitstellung einer ZIP-Datei in Ihrer Funktions-App mithilfe des Befehls [az functionapp deployment source config-zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) aus. Zum Ausführen dieses Befehls müssen Sie Azure CLI, Version 2.0.21 oder höher, verwenden. Sie können mithilfe des `az --version`-Befehls anzeigen, welche Azure CLI-Version Sie verwenden.

Ersetzen Sie im folgenden Befehl den `<zip_file_path>`-Platzhalter durch den Pfad zum Speicherort der ZIP-Datei. Ersetzen Sie außerdem `<app_name>` durch den eindeutigen Namen Ihrer Funktions-App. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
Mit diesem Befehl werden Projektdateien aus der heruntergeladenen ZIP-Datei in Ihrer Funktions-App in Azure bereitgestellt. Anschließend wird die App neu gestartet. Um die Liste der Bereitstellungen für diese Funktions-App anzuzeigen, müssen Sie die REST-APIs verwenden.

Wenn Sie die Azure-Befehlszeilenschnittstelle auf ihrem lokalen Computer verwenden, ist `<zip_file_path>` der Pfad zur ZIP-Datei auf Ihrem Computer. Sie können die Azure-Befehlszeilenschnittstelle ebenfalls in der [Azure Cloud Shell](../cloud-shell/overview.md) ausführen. Wenn Sie Cloud Shell verwenden, müssen Sie zunächst die ZIP-Datei für die Bereitstellung auf das Azure Files-Konto hochladen, das Ihrer Cloud Shell zugeordnet ist. In diesem Fall ist `<zip_file_path>` der Speicherort, der von Ihren Cloud Shell-Konto verwendet wird. Weitere Informationen finden Sie unter [Beibehalten von Dateien in Azure Cloud Shell](../cloud-shell/persisting-shell-storage.md).


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Kontinuierliche Bereitstellung für Azure Functions](functions-continuous-deployment.md)

[.zip push deployment reference topic]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
