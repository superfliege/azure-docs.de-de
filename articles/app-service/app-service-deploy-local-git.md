---
title: Lokale Git-Bereitstellung in Azure App Service
description: Erfahren Sie, wie Sie die lokale Git-Bereitstellung in Azure App Service aktivieren.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: dariagrigoriu;cephalin
ms.openlocfilehash: 842cd6f67a04bec0ed06282bdeeea8b8a51c0667
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale Git-Bereitstellung in Azure App Service

In dieser Anleitung erfahren Sie, wie Sie Ihren Code aus einem Git-Repository auf dem lokalen Computer in [Azure App Service](app-service-web-overview.md) bereitstellen.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Ausführen der Schritte in dieser Anleitung:

* [Git installieren](http://www.git-scm.com/downloads).
* Verwalten Sie ein lokales Git-Repository mit Code, den Sie bereitstellen möchten.

Zur Verwendung eines Beispielrepositorys zum Nachzuvollziehen führen Sie den folgenden Befehl in Ihrem lokalen Terminalfenster aus:

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

## <a name="prepare-your-repository"></a>Vorbereiten Ihres Repositorys

Stellen Sie sicher, dass Ihr Repositorystamm die richtigen Dateien in Ihrem Projekt aufweist.

| Laufzeit | Stammverzeichnisdateien |
|-|-|
| ASP.NET (nur Windows) | _*.sln_, _*.csproj_ oder _default.aspx_ |
| ASP.NET Core | _*.sln_ oder _*.csproj_ |
| PHP | _index.php_ |
| Ruby (nur Linux) | _Gemfile_ |
| Node.js | _server.js_, _app.js_ oder _package.json_ mit einem Startskript |
| Python (nur Windows) | _\*.py_, _requirements.txt_ oder _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_ oder _iisstart.htm_ |
| WebJobs | _\<job_name>/run.\<erweiterung>_ unter _App\_Data/jobs/continuous_ (für fortlaufende WebJobs) oder _App\_Data/jobs/triggered_ (für ausgelöste WebJobs). Weitere Informationen finden Sie in der [Kudu-Dokumentation zu WebJobs](https://github.com/projectkudu/kudu/wiki/WebJobs). |
| Funktionen | Siehe [Kontinuierliche Bereitstellung für Azure Functions](../azure-functions/functions-continuous-deployment.md#continuous-deployment-requirements). |

Zum Anpassen Ihrer Bereitstellung können Sie eine Datei vom Typ _.deployment_ in dem Repositorystamm einschließen. Weitere Informationen finden Sie unter [Customizing deployments](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) (Anpassen von Bereitstellungen) und [Custom Deployment Script](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) (Benutzerdefiniertes Bereitstellungsskript).

> [!NOTE]
> Achten Sie darauf, `git commit` für alle Änderungen auszuführen, die Sie bereitstellen möchten.
>
>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user.md)]

## <a name="enable-git-for-your-app"></a>Aktivieren von Git für die App

Zum Aktivieren der Git-Bereitstellung für eine vorhandene App Service-App führen Sie [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_local_git) in Cloud Shell aus.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Wenn Sie stattdessen eine Git-fähige App erstellen möchten, führen Sie [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) in Cloud Shell mit dem Parameter `--deployment-local-git` aus.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

Die Ausgabe des Befehls `az webapp create` sollte ungefähr wie folgt aussehen:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

## <a name="deploy-your-project"></a>Bereitstellen des Projekts

Kehren Sie zum _lokalen Terminalfenster_ zurück, und fügen Sie Ihrem lokalen Git-Repository einen Azure-Remotespeicherort hinzu. Ersetzen Sie _\<Url>_ durch die URL des Git-Remoterepositorys, die Sie beim [Aktivieren von Git für die App](#enable-git-for-you-app) erhalten haben.

```bash
git remote add azure <url>
```

Führen Sie einen Pushvorgang zum Azure-Remotespeicherort durch, um Ihre App mit dem folgenden Befehl bereitzustellen. Stellen Sie sicher, dass Sie bei der entsprechenden Aufforderung das Kennwort eingeben, das Sie unter [Konfigurieren eines Bereitstellungsbenutzers](#configure-a-deployment-user) erstellt haben, und nicht das Kennwort für die Anmeldung am Azure-Portal.

```bash
git push azure master
```

Die Ausgabe enthält u.U. laufzeitspezifische Automatisierungen wie MSBuild für ASP.NET, `npm install` für Node.js und `pip install` für Python. 

Nach Abschluss der Bereitstellung sollte Ihre App im Azure-Portal jetzt einen Datensatz mit Ihrer `git push`-Anweisung auf der Seite **Bereitstellungsoptionen** aufweisen.

![](./media/app-service-deploy-local-git/deployment_history.png)

Navigieren Sie zu Ihrer App, um sicherzustellen, dass der Inhalt bereitgestellt wird.

## <a name="troubleshooting"></a>Problembehandlung

Die folgenden Fehler und Probleme treten häufig auf, wenn Git zum Veröffentlichen in einer App Service-App in Azure verwendet wird:

---
**Symptom**: `Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Ursache**: Dieser Fehler kann auftreten, wenn die App nicht ordnungsgemäß ausgeführt wird.

**Lösung**: Starten Sie die App im Azure-Portal. Die Git-Bereitstellung ist nicht verfügbar, wenn die Web-App beendet wurde.

---
**Symptom**: `Couldn't resolve host 'hostname'`

**Ursache**: Dieser Fehler kann auftreten, wenn die beim Erstellen des Remoterepositorys „azure“ eingegebenen Adressinformationen falsch waren.

**Lösung**: Verwenden Sie den Befehl `git remote -v`, um alle Remotewebsites zusammen mit der jeweils zugehörigen URL aufzulisten. Überprüfen Sie, ob die URL für die 'azure'-Remotewebsite korrekt ist. Entfernen Sie diese Remote-Website bei Bedarf und erstellen Sie sie mit der korrekten URL neu.

---
**Symptom**: `No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Ursache**: Dieser Fehler kann auftreten, wenn Sie während `git push` keine Verzweigung angeben, oder wenn Sie den Wert `push.default` in `.gitconfig` nicht festgelegt haben.

**Lösung**: Führen Sie `git push` erneut aus, und geben Sie dabei den Masterbranch an. Beispiel: 

```bash
git push azure master
```

---
**Symptom**: `src refspec [branchname] does not match any.`

**Ursache**: Dieser Fehler kann auftreten, wenn Sie versuchen, etwas per Push auf eine andere Verzweigung als die Hauptverzweigung auf dem Remoterepository „azure“ zu übertragen.

**Lösung**: Führen Sie `git push` erneut aus, und geben Sie dabei den Masterbranch an. Beispiel: 

```bash
git push azure master
```

---
**Symptom**: `RPC failed; result=22, HTTP code = 5xx.`

**Ursache:** Dieser Fehler kann auftreten, wenn Sie versuchen, ein großes Git-Repository über HTTPS mithilfe von Push zu übertragen.

**Lösung:** Ändern Sie die Git-Konfiguration auf dem lokalen Computer, sodass der postBuffer größer ist.

```bash
git config --global http.postBuffer 524288000
```

---
**Symptom**: `Error - Changes committed to remote repository but your web app not updated.`

**Ursache**: Dieser Fehler kann auftreten, wenn Sie eine Node.js-App mit einer Datei von Typ _package.json_ bereitstellen, die zusätzliche erforderliche Module angibt.

**Lösung:** Zusätzliche Meldungen mit „npm ERR!“ müssen vor diesem Fehler protokolliert werden und können zusätzlichen Kontext für diesen Fehler bereitstellen. Es folgen bekannte Ursachen für diesen Fehler und die entsprechende Meldung vom Typ „npm ERR!“ :

* **Ungültige package.json-Datei**: npm ERR! Couldn't read dependencies.
* **Systemeigenes Modul ohne binäre Distribution für Windows**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OR
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Weitere Ressourcen

* [Project Kudu documentation](https://github.com/projectkudu/kudu/wiki)
* [Continuous Deployment in Azure App Service](app-service-continuous-deployment.md)
* [Beispiel: Erstellen einer Web-App und Bereitstellen von Code über ein lokales Git-Repository (Azure CLI)](./scripts/app-service-cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
* [Beispiel: Erstellen einer Web-App und Bereitstellen von Code über ein lokales Git-Repository (PowerShell)](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
