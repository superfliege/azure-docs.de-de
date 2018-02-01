---
title: Lokale Git-Bereitstellung in Azure App Service
description: Erfahren Sie, wie Sie die lokale Git-Bereitstellung in Azure App Service aktivieren.
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: cfowler
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 948c54a2e9be2260d0a7d2cce31b67ffbbd23d03
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale Git-Bereitstellung in Azure App Service

In diesem Tutorial erfahren Sie, wie Sie Ihre App aus einem Git-Repository auf dem lokalen Computer in [Azure-Web-Apps](app-service-web-overview.md) bereitstellen. App Service unterstützt diesen Ansatz mit der Bereitstellungsoption **Lokales Git** im [Azure-Portal].

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Git. Sie können die Binärdateien für die Installation [hier](http://www.git-scm.com/downloads)herunterladen.
* Git-Grundkenntnisse.
* Ein Microsoft Azure-Konto. Falls Sie noch kein Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial) oder [Ihre Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
>

## <a name="Step1"></a>Schritt 1: Erstellen eines lokalen Repositorys

Führen Sie die folgenden Aufgaben durch, um ein neues Git-Repository zu erstellen.

1. Starten Sie ein Befehlszeilentool wie **GitBash** (Windows) oder **Bash** (Unix Shell). Auf OS X-Systemen können Sie auf die Befehlszeile über die **Terminal**-Anwendung zugreifen.
1. Navigieren Sie zum Verzeichnis, in dem sich der bereitzustellende Inhalt befinden wird.
1. Verwenden Sie den folgenden Befehl, um ein neues Git-Repository zu initialisieren.

  ```bash
  git init
  ```

## <a name="Step2"></a>Schritt 2: Ausführen eines Commits für die Inhalte

App Service unterstützt in verschiedenen Programmiersprachen erstellte Anwendungen.

1. Enthält Ihr Repository noch keinen Inhalt, fügen Sie eine statische HTML-Datei wie folgt hinzu, oder überspringen Sie diesen Schritt:
   * Erstellen Sie mit einem Text-Editor eine neue Datei namens **index.html** im Stammverzeichnis des Git-Repositorys.
   * Geben Sie den folgenden Text als Inhalt der Datei „index.html“ ein, und speichern Sie die Datei: *Hello Git!*
1. Stellen Sie mit der Befehlszeile sicher, dass Sie sich im Stammverzeichnis Ihres Git-Repositorys befinden. Verwenden Sie dann den folgenden Befehl, um dem Repository Dateien hinzuzufügen:

        git add -A 
1. Übernehmen Sie dann die Änderungen am Repository mithilfe des folgenden Befehls:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>Schritt 3: Aktivieren des App Service-App-Repositorys

Führen Sie die folgenden Schritte durch, um ein Git-Repository für Ihre App Service-App zu aktivieren.

1. Melden Sie sich beim [Azure-Portal]an.
1. Klicken Sie in der Ansicht Ihrer App Service-App auf **Einstellungen > Bereitstellungsquelle**. Klicken Sie auf **Quelle auswählen**, dann auf **Lokales Git-Repository** und schließlich auf **OK**.

    ![Lokales Git-Repository](./media/app-service-deploy-local-git/local_git_selection.png)

1. Wenn Sie zum ersten Mal ein Repository in Azure einrichten, müssen Sie dafür Anmeldeinformationen erstellen. Sie verwenden diese, um eine Anmeldung beim Azure-Repository vorzunehmen und Änderungen aus Ihrem lokalen Git-Repository mithilfe von Push zu übertragen. Klicken Sie in der Ansicht Ihrer Web-App auf **Einstellungen > Anmeldeinformationen für Bereitstellung**, und konfigurieren Sie dann den Benutzernamen und das Kennwort für die Bereitstellung. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Schritt 4: Bereitstellen des Projekts

Führen Sie die folgenden Schritte durch, um Ihre App mit einem lokalen Git in App Service zu veröffentlichen:

1. Klicken Sie im Azure-Portal in der Ansicht Ihrer Web-App für die **Git-URL** auf **Einstellungen > Eigenschaften**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **Einstellungen &amp;gt; Eigenschaften** ist die Remotereferenz für die Bereitstellung aus dem lokalen Repository. Verwenden Sie diese URL in den nächsten Schritten.
1. Stellen Sie mithilfe der Befehlszeile sicher, dass Sie sich im Stammverzeichnis Ihres lokalen Git-Repositorys befinden.
1. Fügen Sie mit `git remote` die in Schritt 1 unter **Git-URL** aufgeführte Remotereferenz hinzu. Ihr Befehl sieht in etwa wie folgt aus:

    ```bash
    git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git
    ```

   > [!NOTE]
   > Der Befehl **remote** fügt einem Remoterepository einen benannten Verweis hinzu. In diesem Beispiel wird ein Verweis mit dem Namen "azure" für das Repository Ihrer Web-App erstellt.
   >

1. Übertragen Sie die Inhalte mit dem neuen Remoteverweis **azure**, den Sie erstellt haben, in App Service.

    ```bash
    git push azure master
    ```

    Sie werden zur Eingabe des Kennworts aufgefordert, das zuvor beim Zurücksetzen Ihrer Anmeldeinformationen für die Bereitstellung im Azure-Portal erstellt wurde. Geben Sie das Kennwort ein (beachten Sie, dass Gitbash keine Sternchen in der Konsole anzeigt, wenn Sie Ihr Kennwort eingeben). 
1. Kehren Sie zu Ihrer App im Azure-Portal zurück. Ein Protokolleintrag des letzten Pushvorgangs sollte in der Ansicht **Bereitstellungen** angezeigt werden.

    ![](./media/app-service-deploy-local-git/deployment_history.png)

1. Klicken Sie oben auf der Web-App-Seite auf die Schaltfläche **Durchsuchen**, um zu überprüfen, ob die Inhalte bereitgestellt wurden.

## <a name="Step5"></a>Problembehandlung

Die folgenden Fehler und Probleme treten häufiger auf, wenn Git zum Veröffentlichen in einer App Service-App in Azure verwendet wird:

---
**Symptom**: `Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Ursache**: Dieser Fehler kann auftreten, wenn die App nicht ordnungsgemäß ausgeführt wird.

**Lösung**: Starten Sie die App im Azure-Portal. Die Git-Bereitstellung ist nicht verfügbar, wenn die Web-App beendet wurde.

---
**Symptom**: `Couldn't resolve host 'hostname'`

**Ursache**: Dieser Fehler kann auftreten, wenn die beim Erstellen der "azure"-Remotewebsite eingegebenen Adressinformationen falsch waren.

**Lösung**: Verwenden Sie den Befehl `git remote -v`, um alle Remotewebsites zusammen mit der jeweils zugehörigen URL aufzulisten. Überprüfen Sie, ob die URL für die 'azure'-Remotewebsite korrekt ist. Entfernen Sie diese Remote-Website bei Bedarf und erstellen Sie sie mit der korrekten URL neu.

---
**Symptom**: `No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Ursache**: Dieser Fehler kann auftreten, wenn Sie keine Verzweigung während `git push` angeben, oder wenn Sie den Wert `push.default` in `.gitconfig` nicht festgelegt haben.

**Lösung**: Führen Sie den Pushvorgang unter Angabe der Hauptverzweigung erneut durch. Beispiel: 

```bash
git push azure master
```

---
**Symptom**: `src refspec [branchname] does not match any.`

**Ursache**: Dieser Fehler kann auftreten, wenn Sie versuchen, etwas per Push auf eine andere Verzweigung als die Hauptverzweigung auf der "azure"-Remotewebsite zu übertragen.

**Lösung**: Führen Sie den Pushvorgang unter Angabe der Hauptverzweigung erneut durch. Beispiel: 

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

**Ursache**: Dieser Fehler kann auftreten, wenn Sie eine Node.js-App mit einer Datei „package.json“ bereitstellen, die zusätzliche erforderliche Module angibt.

**Lösung:** Zusätzliche Meldungen mit „npm ERR!“ sollten vor diesem Fehler protokolliert werden und können zusätzlichen Kontext für diesen Fehler bereitstellen. Es folgen bekannte Ursachen für diesen Fehler und die entsprechende Meldung vom Typ „npm ERR!“ :

* **Ungültige package.json-Datei**: npm ERR! Couldn't read dependencies.
* **Systemeigenes Modul ohne binäre Distribution für Windows**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OR
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Weitere Ressourcen

* [Git-Dokumentation](http://git-scm.com/documentation)
* [Project Kudu documentation](https://github.com/projectkudu/kudu/wiki)
* [Continuous Deployment in Azure App Service](app-service-continuous-deployment.md)
* [Verwenden von PowerShell für Azure](/powershell/azure/overview)
* [Verwenden der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md)

[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure-Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure Command-Line Interface]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
