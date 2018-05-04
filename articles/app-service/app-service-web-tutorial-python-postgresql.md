---
title: Erstellen einer Python- und PostgreSQL-Web-App in Azure | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Python-App in Azure mit Verbindung mit einer PostgreSQL-Datenbank ausführen.
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 01/25/2018
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: e342a10c2f3b6c32d8d0bc727bf3325c26fb53d6
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="tutorial-build-a-python-and-postgresql-web-app-in-azure"></a>Tutorial: Erstellen einer Python- und PostgreSQL-Web-App in Azure

> [!NOTE]
> In diesem Artikel wird eine App in App Service unter Windows bereitgestellt. Informationen zur Bereitstellung für App Service unter _Linux_ finden Sie unter [Erstellen einer Docker Python- und PostgreSQL-Web-App in Azure](./containers/tutorial-docker-python-postgresql-app.md).
>

Von [Azure App Service](app-service-web-overview.md) wird ein hochgradig skalierbarer Webhostingdienst mit Self-Patching bereitgestellt. In diesem Tutorial wird gezeigt, wie eine einfache Python-Web-App in Azure erstellt wird. Sie verbinden diese App zudem mit einer PostgreSQL-Datenbank. Wenn Sie fertig sind, wird eine Python Flask-Anwendung in App Service ausgeführt.

![Python Flask-App in App Service unter Linux](./media/app-service-web-tutorial-python-postgresql/docker-flask-in-azure.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer PostgreSQL-Datenbank in Azure
> * Herstellen einer Verbindung mit MySQL für eine Python-App
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Verwalten der App im Azure-Portal

Sie können die Schritte in diesem Tutorial auch unter macOS ausführen. Die Anweisungen für Linux und Windows sind in den meisten Fällen identisch, doch die Unterschiede werden in diesem Tutorial nicht beschrieben.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

1. [Installation von Git](https://git-scm.com/)
1. [Installieren Sie Python.](https://www.python.org/downloads/)
1. [Laden Sie PostgreSQL herunter, und führen Sie es aus.](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Testen der lokalen PostgreSQL-Installation und Erstellen einer Datenbank

Öffnen Sie das Terminalfenster, und führen Sie `psql` aus, um eine Verbindung mit Ihrem lokalen PostgreSQL-Server herzustellen.

```bash
sudo -u postgres psql
```

Wenn die Verbindung erfolgreich ist, wird die PostgreSQL-Datenbank ausgeführt. Ist dies nicht der Fall, stellen Sie sicher, dass die lokale PostgreSQL-Datenbank gestartet wurde, indem Sie die Schritte unter [Downloads: PostgreSQL Core Distribution](https://www.postgresql.org/download/) ausführen.

Erstellen Sie eine Datenbank mit dem Namen *eventregistration*, und richten Sie einen separaten Datenbankbenutzer namens *manager* mit Kennwort *supersecretpass* ein.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
Geben Sie `\q` ein, um den PostgreSQL-Client zu beenden. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Erstellen der lokalen Python Flask-Anwendung

In diesem Schritt richten Sie das lokale Python Flask-Projekt ein.

### <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Öffnen Sie das Terminalfenster, und wechseln Sie mit `CD` in ein Arbeitsverzeichnis.

Führen Sie die folgenden Befehle aus, um das Beispielrepository zu klonen, und kehren Sie dann zum Commit-Vorgang für die ursprüngliche App zurück (vor `modelChange`).

```bash
git clone https://github.com/Azure-Samples/flask-postgresql-app
cd flask-postgresql-app
git revert modelChange --no-edit
```

Dieses Beispielrepository enthält eine [Flask](http://flask.pocoo.org/)-Anwendung. 

### <a name="run-the-application"></a>Ausführen der Anwendung

Installieren Sie die erforderlichen Pakete, und starten Sie die Anwendung.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Wenn die App vollständig geladen wurde, sollte eine ähnliche Meldung wie diese angezeigt werden:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Navigieren Sie in einem Browser zu `http://localhost:5000`. Klicken Sie auf **Registrieren**, und erstellen Sie einen Testbenutzer.

![Lokal ausgeführte Python Flask-Anwendung](./media/app-service-web-tutorial-python-postgresql/local-app.png)

Die Flask-Beispielanwendung speichert Benutzerdaten in der Datenbank. Wenn Sie einen Benutzer erfolgreich registriert haben, schreibt Ihre App Daten in die lokale PostgreSQL-Datenbank.

Sie können den Flask-Server jederzeit beenden, indem Sie im Terminal STRG+C eingeben. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-postgresql-in-azure"></a>Erstellen von PostgreSQL in Azure

In diesem Schritt erstellen Sie eine PostgreSQL-Datenbank in Azure. Wenn Ihre App in Azure bereitgestellt ist, nutzt sie diese Clouddatenbank.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-postgresql-server"></a>Erstellen eines PostgreSQL-Servers

Erstellen Sie mit dem Befehl [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create) einen PostgreSQL-Server.

Ersetzen Sie im folgenden Befehl den Platzhalter *\<postgresql_name>* durch einen eindeutigen Servernamen, den Platzhalter *\<admin_username>* durch einen Benutzernamen und den Platzhalter *\<admin_password>* durch ein Kennwort. Der Servername dient als Teil Ihrer PostgreSQL-Endpunkts (`https://<postgresql_name>.postgres.database.azure.com`). Daher muss der Name auf allen Servern in Azure eindeutig sein.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name mydemoserver --location "West Europe" --admin-user <admin_username> --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 9.6
```

Nach dem Erstellen der Azure-Datenbank für den PostgreSQL-Server zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "additionalProperties": {},
  "administratorLogin": "<my_admin_username>",
  "earliestRestoreDate": "2018-04-19T22:51:05.340000+00:00",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westeurope",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "additionalProperties": {},
    "capacity": 2,
    "family": "Gen4",
    "name": "GP_Gen4_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "additionalProperties": {},
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

### <a name="configure-server-firewall"></a>Konfigurieren der Serverfirewall

Führen Sie den folgenden Azure CLI-Befehl aus, um den Zugriff auf die Datenbank von allen IP-Adressen zu ermöglichen. Wenn sowohl Start- als auch End-IP auf 0.0.0.0 festgelegt ist, wird die Firewall nur für andere Azure-Ressourcen geöffnet. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0 --name AllowAzureIPs
```

Die Azure CLI bestätigt das Erstellen der Firewallregel mit mithilfe einer Ausgabe ähnlich wie im folgenden Beispiel:

```json
{
  "additionalProperties": {},
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
 "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

> [!TIP] 
> Sie können Ihre Firewallregel auch noch restriktiver gestalten und [nur die ausgehenden IP-Adressen verwenden, die Ihre App verwendet](app-service-ip-addresses.md#find-outbound-ips).
>

### <a name="create-a-production-database-and-user"></a>Erstellen einer Produktionsdatenbank und eines Benutzers

Erstellen Sie einen neuen Datenbankbenutzer mit Zugriff auf nur eine einzelne Datenbank. Sie verwenden diese Anmeldeinformationen, um zu vermeiden, dass der Anwendung Vollzugriff auf den Server gewährt wird.

Stellen Sie eine Verbindung mit der Datenbank her (Sie werden zur Eingabe Ihres Administratorkennworts aufgefordert).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <admin_username>@<postgresql_name> postgres
```

Erstellen Sie die Datenbank und den Benutzer über die PostgreSQL CLI.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Geben Sie `\q` ein, um den PostgreSQL-Client zu beenden.

### <a name="test-app-locally-with-azure-postgresql"></a>Lokales Testen der App mit Azure PostgreSQL

Nach dem Wechseln zurück zum Ordner *app* des geklonten GitHub-Repositorys können Sie die Python Flask-Anwendung ausführen, indem Sie die Umgebungsvariablen der Datenbank aktualisieren.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Wenn die App vollständig geladen wurde, sollte eine ähnliche Meldung wie diese angezeigt werden:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Navigieren Sie in einem Browser zu http://localhost:5000. Klicken Sie auf **Registrieren**, und erstellen Sie eine Testregistrierung. Sie schreiben nun Daten in die Datenbank in Azure.

![Lokal ausgeführte Python Flask-Anwendung](./media/app-service-web-tutorial-python-postgresql/local-app.png)

## <a name="deploy-to-azure"></a>Bereitstellen in Azure

In diesem Schritt stellen Sie die mit PostgreSQL verbundene Python-Anwendung in Azure App Service bereit.

Ihr Git-Repository enthält bereits die folgenden Dateien, die zum Ausführen der Flask-Web-App in App Service erforderlich sind:

- `.deployment`: Gibt das auszuführende Skript für die benutzerdefinierte Bereitstellung an.
- `deploy.cmd`: Das Bereitstellungsskript. Hier wird `pip install` ausgeführt.
- `web.config`: Gibt das Einstiegspunktskript an, das in einem `httpPlatformHandler`-Element in IIS ausgeführt wird.
- `run_waitress_server.py`: Das Einstiegspunktskript. Hiermit wird die Flask-Web-App auf einem [`waitress`](https://docs.pylonsproject.org/projects/waitress)-Server gestartet.

### <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-no-h.md)]

<a name="create"></a>
### <a name="create-a-web-app"></a>Erstellen einer Web-App

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)] 

### <a name="install-python"></a>Installieren von Python

In diesem Schritt installieren Sie Python 3.6.2 mit [Websiteerweiterungen](https://www.siteextensions.net/packages?q=Tags%3A%22python%22) in App Service. Sie verwenden die Anmeldeinformationen, die Sie unter [Konfigurieren eines Bereitstellungsbenutzers](#configure-a-deployment-user) konfiguriert haben, um die Authentifizierung für den REST-Endpunkt durchzuführen.

Führen Sie in der Cloud Shell den nächsten Befehl aus, um die Python 3.6.2-Paketinformationen abzurufen. Ersetzen Sie *\<deployment_user>* durch den konfigurierten Benutzernamen für die Bereitstellung und *\<app_name>* durch Ihren App-Namen. Verwenden Sie das zuvor konfigurierte Kennwort für die Bereitstellung, wenn die Aufforderung angezeigt wird.

```bash
packageinfo=$(curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/extensionfeed/python362x86)
```

Führen Sie in der Cloud Shell den nächsten Befehl aus, um das Python-Paket zu installieren. Ersetzen Sie *\<deployment_user>* durch den konfigurierten Benutzernamen für die Bereitstellung und *\<app_name>* durch Ihren App-Namen. Verwenden Sie das zuvor konfigurierte Kennwort für die Bereitstellung, wenn die Aufforderung angezeigt wird.

```bash
curl -X PUT -u <deployment_user> -H "Content-Type: application/json" -d '$packageinfo' https://<app_name>.scm.azurewebsites.net/api/siteextensions/python362x86
```

In der Befehlsausgabe können Sie sehen, dass Python unter dem Pfad `D:\home\python362x86\python.exe` installiert wurde.

### <a name="configure-database-settings"></a>Konfigurieren der Datenbankeinstellungen

Weiter oben in diesem Tutorial haben Sie die Umgebungsvariablen für die Verbindung mit der PostgreSQL-Datenbank definiert.

In App Service legen Sie Umgebungsvariablen als _App-Einstellungen_ fest, indem Sie den Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) verwenden.

Im folgenden Beispiel werden die Details der Datenbankverbindung als App-Einstellungen angeben. Ersetzen Sie *\<app_name>* durch Ihren App-Namen und *\<postgresql_name>* durch Ihren PostgreSQL-Servernamen.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration"
```

### <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done.
Total 5 (delta 3), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6c7c716eee'.
remote: Running custom deployment command...
remote: Running deployment command...
remote: Handling node.js deployment.
.
.
.
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

### <a name="browse-to-the-azure-web-app"></a>Navigieren Sie zur Azure-Web-App. 

Navigieren Sie in Ihrem Webbrowser zu der bereitgestellten Web-App. 

```bash 
http://<app_name>.azurewebsites.net 
```

Es werden die zuvor registrierten Gäste angezeigt, die im vorherigen Schritt in der Azure-Produktionsdatenbank gespeichert wurden.

![Lokal ausgeführte Python Flask-Anwendung](./media/app-service-web-tutorial-python-postgresql/docker-app-deployed.png)

**Glückwunsch!** Sie führen eine Python Flask-App in Azure App Service aus.

## <a name="update-data-model-and-redeploy"></a>Aktualisieren und erneutes Bereitstellen des Datenmodells

In diesem Schritt fügen Sie die Anzahl von Teilnehmern für jede Ereignisregistrierung hinzu, indem Sie das `Guest`-Modell aktualisieren.

Überprüfen Sie die Dateien, die mit dem `modelChange`-Commit gekennzeichnet sind:

```bash
git checkout modelChange -- *
```

Dieses Release enthält bereits die erforderlichen Änderungen an Sichten, Controllern und Modell. Es enthält auch eine Datenbankmigration, die über *alembic* (`flask db migrate`) generiert wurde. Sie können die Änderungen an allen Dateien in der [GitHub-Commit-Ansicht](https://github.com/Azure-Samples/flask-postgresql-app/commit/139a53023688631c3cc2caefd70086f4722ecd7e) anzeigen.

### <a name="test-your-changes-locally"></a>Lokales Testen der Änderungen

Führen Sie die folgenden Befehle aus, um Ihre Änderungen lokal zu testen, indem Sie den Flask-Server ausführen.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Navigieren Sie in Ihrem Browser zu http://localhost:5000, um die Änderungen anzuzeigen. Erstellen Sie eine Testregistrierung.

![Lokal ausgeführte Python Flask-Anwendung](./media/app-service-web-tutorial-python-postgresql/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Veröffentlichen von Änderungen in Azure

Committen Sie alle Änderungen im lokalen Terminalfenster in Git, und übertragen Sie die Codeänderungen per Pushvorgang an Azure.

```bash
git add .
git commit -m "updated data model"
git push azure master
```

Wechseln Sie zu Ihrer Azure-Web-App, und testen Sie die neuen Funktionen erneut. Erstellen Sie eine weitere Ereignisregistrierung.

```bash 
http://<app_name>.azurewebsites.net 
```

![Python Flask-App in Azure App Service](./media/app-service-web-tutorial-python-postgresql/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Verwalten Ihrer Azure-Web-App

Wechseln Sie zum [Azure-Portal](https://portal.azure.com), um die erstellte Web-App anzuzeigen.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/app-service-web-tutorial-python-postgresql/app-resource.png)

Standardmäßig wird im Portal die Seite **Übersicht** Ihrer Web-App angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten links auf der Seite zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![App Service-Seite im Azure-Portal](./media/app-service-web-tutorial-python-postgresql/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihrer Web-App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"]
> [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps](app-service-web-tutorial-custom-domain.md)
