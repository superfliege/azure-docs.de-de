---
title: Erstellen einer Python- und PostgreSQL-Web-App in Azure App Service | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie eine datengesteuerte Python-App in Azure mit Verbindung mit einer PostgreSQL-Datenbank ausführen.
services: app-service\web
documentationcenter: python
author: cephalin
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 09/28/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: f4ce197d541b8573e38fd85dcebb575c8ee99f59
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435733"
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Erstellen einer Docker Python- und PostgreSQL-Web-App in Azure

[App Service unter Linux](app-service-linux-intro.md) bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching. In diesem Tutorial wird erläutert, wie Sie eine datengesteuerte Python-Web-App mit PostgreSQL als Datenbank-Back-End erstellen. Wenn Sie diese Schritte ausgeführt haben, verfügen Sie über eine Python Flask-Anwendung, die in einem Docker-Container in App Service unter Linux ausgeführt wird.

![Docker Python Flask-App in App Service unter Linux](./media/tutorial-python-postgresql-app/docker-flask-in-azure.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer PostgreSQL-Datenbank in Azure
> * Herstellen einer Verbindung mit PostgreSQL für eine Python-App
> * Bereitstellen der Anwendung in Azure
> * Anzeigen von Diagnoseprotokollen
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Verwalten der App im Azure-Portal

Sie können die Schritte in diesem Artikel auch unter macOS ausführen. Die Anweisungen für Linux und Windows sind in den meisten Fällen identisch, doch die Unterschiede werden in diesem Tutorial nicht beschrieben.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

1. [Installation von Git](https://git-scm.com/)
1. [Installieren Sie Python.](https://www.python.org/downloads/)
1. [Laden Sie PostgreSQL herunter, und führen Sie es aus.](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Testen der lokalen PostgreSQL-Installation und Erstellen einer Datenbank

Führen Sie in einem Terminalfenster `psql` aus, um eine Verbindung mit Ihrem lokalen PostgreSQL-Server herzustellen.

```bash
sudo -u postgres psql postgres
```

Wenn eine Fehlermeldung ähnlich wie `unknown user: postgres` angezeigt wird, wird Ihre PostgreSQL-Installation möglicherweise mit Ihrem angemeldeten Benutzernamen konfiguriert. Versuchen Sie dann stattdessen den folgenden Befehl.

```bash
psql postgres
```

Wenn die Verbindung erfolgreich ist, wird die PostgreSQL-Datenbank ausgeführt. Stellen Sie andernfalls sicher, dass die lokale PostgreSQL-Datenbank gestartet wurde, indem Sie die Anweisungen für Ihr Betriebssystem unter [Downloads: PostgreSQL Core Distribution](https://www.postgresql.org/download/) befolgen.

Erstellen Sie eine Datenbank mit dem Namen *eventregistration*, und richten Sie einen separaten Datenbankbenutzer namens *manager* mit Kennwort *supersecretpass* ein.

```sql
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Geben Sie `\q` ein, um den PostgreSQL-Client zu beenden.

<a name="step2"></a>

## <a name="create-local-python-app"></a>Erstellen einer lokalen Python-App

In diesem Schritt richten Sie das lokale Python Flask-Projekt ein.

### <a name="clone-the-sample-app"></a>Klonen der Beispiel-App

Öffnen Sie das Terminalfenster, und wechseln Sie mit `CD` in ein Arbeitsverzeichnis.

Führen Sie die folgenden Befehle aus, um das Beispielrepository zu klonen.

```bash
git clone https://github.com/Azure-Samples/flask-postgresql-app.git
cd flask-postgresql-app
```

Dieses Beispielrepository enthält eine [Flask](http://flask.pocoo.org/)-Anwendung.

### <a name="run-the-app-locally"></a>Lokales Ausführen der App

Installieren Sie die erforderlichen Pakete, und starten Sie die Anwendung.

```bash
# Bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run

# PowerShell
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
Set-Item Env:FLASK_APP ".\app.py"
DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
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

![Lokal ausgeführte Python Flask-Anwendung](./media/tutorial-python-postgresql-app/local-app.png)

Die Flask-Beispielanwendung speichert Benutzerdaten in der Datenbank. Wenn Sie einen Benutzer erfolgreich registriert haben, schreibt Ihre App Daten in die lokale PostgreSQL-Datenbank.

Sie können den Flask-Server jederzeit beenden, indem Sie im Terminal STRG+C eingeben.

## <a name="create-a-production-postgresql-database"></a>Erstellen einer PostgreSQL-Produktionsdatenbank

In diesem Schritt erstellen Sie eine PostgreSQL-Datenbank in Azure. Wenn Ihre App in Azure bereitgestellt ist, nutzt sie diese Clouddatenbank.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>Erstellen einer Azure-Datenbank für PostgreSQL-Server

Erstellen Sie mit dem Befehl [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) in Cloud Shell einen PostgreSQL-Server.

Ersetzen Sie im folgenden Beispielbefehl *\<postgresql_name>* durch einen eindeutigen Servernamen und *\<admin_username>* und *\<admin_password>* durch die gewünschten Benutzeranmeldeinformationen. Die Benutzeranmeldeinformationen gelten für das Konto des Datenbankadministrators. Der Servername dient als Teil Ihrer PostgreSQL-Endpunkts (`https://<postgresql_name>.postgres.database.azure.com`). Daher muss der Name auf allen Servern in Azure eindeutig sein.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <admin_password> --sku-name B_Gen4_1
```

Nach dem Erstellen der Azure-Datenbank für den PostgreSQL-Server zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "administratorLogin": "<admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 1,
    "family": "Gen4",
    "name": "B_Gen4_1",
    "size": null,
    "tier": "Basic"
  },
  < JSON data removed for brevity. >
}
```

> [!NOTE]
> Notieren Sie die Werte für \<admin_username> und \<admin_password> für die spätere Verwendung. Sie benötigen diese Angaben zur Anmeldung beim PostgreSQL-Server und den zugehörigen Datenbanken.


### <a name="create-firewall-rules-for-the-postgresql-server"></a>Erstellen von Firewallregeln für den PostgreSQL-Server

Führen Sie in Cloud Shell die folgenden Azure CLI-Befehle aus, um den Zugriff von Azure-Ressourcen auf die Datenbank zu ermöglichen.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> Diese Einstellung ermöglicht Netzwerkverbindungen von allen IP-Adressen im Azure-Netzwerk. Versuchen Sie für den Produktivbetrieb möglichst restriktive Firewallregeln zu konfigurieren, indem Sie [nur die ausgehenden IP-Adressen zulassen, die von Ihrer App verwendet werden](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).

Führen Sie den Befehl in Cloud Shell erneut aus, um den Zugriff vom lokalen Computer zuzulassen. Ersetzen Sie dabei *\<your_ip_address>* durch [Ihre lokale IPv4-IP-Adresse](http://www.whatsmyip.org/).

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<your_ip_address> --end-ip-address=<your_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>Herstellen einer Verbindung zwischen der Python-App und der Produktionsdatenbank

In diesem Schritt verbinden Sie Ihre Flask-Beispiel-App mit dem von Ihnen erstellten Azure Database for PostgreSQL-Server.

### <a name="create-empty-database-and-user-access"></a>Erstellen einer leeren Datenbank und des Benutzerzugriffs

Stellen Sie im lokalen Terminalfenster eine Verbindung mit der Datenbank her, indem Sie den unten stehenden Befehl ausführen. Wenn Sie zur Eingabe des Administratorkennworts aufgefordert werden, verwenden Sie das gleiche Kennwort, das Sie auch unter [Erstellen eines Azure Database for PostgreSQL-Servers](#create-an-azure-database-for-postgresql-server) angegeben haben.

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Erstellen Sie die Datenbank und Benutzer auf dem Azure PostgreSQL-Server genauso wie auf Ihrem lokalen PostgreSQL-Server.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Geben Sie `\q` ein, um den PostgreSQL-Client zu beenden.

> [!NOTE]
> Es empfiehlt sich, Datenbankbenutzer mit eingeschränkten Berechtigungen für bestimmte Anwendungen zu erstellen, anstatt den Administratorbenutzer zu verwenden. In diesem Beispiel besitzt der Benutzer `manager` vollständige Berechtigungen _nur_ für die `eventregistration`-Datenbank.

### <a name="test-app-connectivity-to-production-database"></a>Testen der App-Konnektivität mit der Produktionsdatenbank

Führen Sie im lokalen Terminalfenster die folgenden Befehle aus, um die Flask-Datenbankmigration und den Flask-Server auszuführen.

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

![Lokal ausgeführte Python Flask-Anwendung](./media/tutorial-python-postgresql-app/local-app.png)

## <a name="deploy-to-azure"></a>Bereitstellen in Azure

In diesem Schritt stellen Sie die mit PostgreSQL verbundene Python-Anwendung in Azure App Service bereit.

### <a name="configure-repository"></a>Konfigurieren des Repositorys

Die Git-Bereitstellungs-Engine in App Service ruft die `pip`-Automatisierung auf, wenn sich im Repositorystamm eine _application.py_-Datei befindet. In diesem Tutorial überlassen Sie der Bereitstellungs-Engine die Ausführung der Automatisierung. Navigieren Sie im lokalen Terminalfenster zum Repositorystamm, erstellen Sie eine _application.py_-Dummydatei, und committen Sie Ihre Änderungen.

```bash
cd ..
touch application.py
git add .
git commit -m "ensure azure automation"
```

### <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan? 

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Erstellen einer Web-App 

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

### <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Weiter oben in diesem Tutorial haben Sie die Umgebungsvariablen für die Verbindung mit der PostgreSQL-Datenbank definiert.

In App Service verwenden Sie den Befehl [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in Cloud Shell, um Umgebungsvariablen als _App-Einstellungen_ festzulegen.

Im folgenden Beispiel werden die Details der Datenbankverbindung als App-Einstellungen angeben. 

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration"
```

### <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

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

### <a name="configure-entry-point"></a>Konfigurieren des Einstiegspunkts

Das integrierte Image sucht standardmäßig im Stammverzeichnis nach einer _wsgi.py_- oder _application.py_-Datei als Einstiegspunkt, Ihr Einstiegspunkt lautet aber _app/app.py_. Die _application.py_-Datei, die Sie zuvor hinzugefügt haben, ist leer und führt keine Aktion aus.

Führen Sie den Befehl [`az webapp config set`](/cli/azure/webapp/config?view=azure-cli-latest#az-webapp-config-set) in Cloud Shell aus, um ein benutzerdefiniertes Startskript festzulegen.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --startup-file "gunicorn '--bind=0.0.0.0' --chdir /home/site/wwwroot/app app:app"
```

Der Parameter `--startup-file` akzeptiert einen benutzerdefinierten Befehl oder den Pfad zu einer Datei, die den benutzerdefinierten Befehl enthält. Ihr benutzerdefinierter Befehl sollte das folgende Format aufweisen:

```
gunicorn '--bind=0.0.0.0' --chdir /home/site/wwwroot/<subdirectory> <module>:<variable>
```

Im benutzerdefinierten Befehl ist `--chdir` erforderlich, wenn sich Ihr Einstiegspunkt nicht im Stammverzeichnis befindet und `<subdirectory>` das Unterverzeichnis ist. `<module>` ist der Name der _.py_-Datei, und `<variable>` ist die Variable in dem Modul, das Ihre Web-App repräsentiert.

### <a name="browse-to-the-azure-web-app"></a>Navigieren Sie zur Azure-Web-App.

Navigieren Sie zur bereitgestellten Web-App. Das Starten dauert einen Moment, da bei der ersten Anforderung der App der Container heruntergeladen und ausgeführt werden muss. Wenn bei der Seite ein Timeout auftritt oder eine Fehlermeldung angezeigt wird, warten Sie einige Minuten, und aktualisieren Sie die Seite.

```bash
http://<app_name>.azurewebsites.net
```

Es werden die zuvor registrierten Gäste angezeigt, die im vorherigen Schritt in der Azure-Produktionsdatenbank gespeichert wurden.

![In Azure ausgeführte Python Flask-Anwendung](./media/tutorial-python-postgresql-app/docker-app-deployed.png)

**Glückwunsch!** Sie führen eine Python-App in App Service für Linux aus.

## <a name="access-diagnostic-logs"></a>Zugreifen auf Diagnoseprotokolle

Da die Python-App in einem Container ausgeführt wird, ermöglicht App Service unter Linux Ihnen den Zugriff auf die Konsolenprotokolle, die aus dem Container generiert werden. Um die Protokolle zu suchen, navigieren Sie zu dieser URL:

```
https://<app_name>.scm.azurewebsites.net/api/logs/docker
```

Sie sollten zwei JSON-Objekte sehen, jeweils mit einer `href`-Eigenschaft. Ein `href`-Wert verweist auf die Docker-Konsolenprotokolle (endet mit `_docker.log`), ein anderer `href`-Wert verweist auf die Konsolenprotokolle, die aus dem Python-Container generiert werden. 

```json
[  
   {  
      "machineName":"RD0003FF61ACD0_default",
      "lastUpdated":"2018-09-27T16:48:17Z",
      "size":4766,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_09_27_RD0003FF61ACD0_default_docker.log",
      "path":"/home/LogFiles/2018_09_27_RD0003FF61ACD0_default_docker.log"
   },
   {  
      "machineName":"RD0003FF61ACD0",
      "lastUpdated":"2018-09-27T16:48:19Z",
      "size":2589,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_09_27_RD0003FF61ACD0_docker.log",
      "path":"/home/LogFiles/2018_09_27_RD0003FF61ACD0_docker.log"
   }
]
```

Kopieren Sie den gewünschten `href`-Wert in ein Browserfenster, um zu den Protokollen zu navigieren. Die Protokolle werden nicht gestreamt, daher kann eine Verzögerung auftreten. Um neue Protokolle anzuzeigen, aktualisieren Sie die Browserseite.

## <a name="update-data-model-and-redeploy"></a>Aktualisieren und erneutes Bereitstellen des Datenmodells

In diesem Schritt fügen Sie die Anzahl von Teilnehmern für jede Ereignisregistrierung hinzu, indem Sie das `Guest`-Modell aktualisieren. Danach stellen Sie das Update wieder in Azure bereit.

Checken Sie im lokalen Terminalfenster mit dem folgenden Git-Befehl Dateien aus dem Branch `modelChange` aus:

```bash
git checkout origin/modelChange -- .
```

Dieser Vorgang führt bereits die erforderlichen Änderungen an Modell, Ansichten und Controllern durch. Es enthält auch eine Datenbankmigration, die über *alembic* (`flask db migrate`) generiert wurde. Sie können alle Änderungen über den folgenden Git-Befehl anzeigen:

```bash
git diff master origin/modelChange
```

### <a name="test-your-changes-locally"></a>Lokales Testen der Änderungen

Führen Sie im lokalen Terminalfenster die folgenden Befehle aus, um Ihre Änderungen lokal zu testen, indem Sie den Flask-Server ausführen.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Navigieren Sie in Ihrem Browser zu http://localhost:5000, um die Änderungen anzuzeigen. Erstellen Sie eine Testregistrierung.

![Lokal ausgeführte Python Flask-Anwendung basierend auf einem Docker-Container](./media/tutorial-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Veröffentlichen von Änderungen in Azure

Committen Sie alle Änderungen im lokalen Terminalfenster in Git, und übertragen Sie die Codeänderungen per Pushvorgang an Azure.

```bash 
git add . 
git commit -m "updated data model" 
git push azure master 
``` 

Wechseln Sie zu Ihrer Azure-Web-App, und testen Sie die neuen Funktionen erneut. Stellen Sie sicher, dass Sie die Seite aktualisieren.

```bash
http://<app_name>.azurewebsites.net
```

![Docker Python Flask-App in Azure App Service](./media/tutorial-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-web-app-in-the-azure-portal"></a>Überwachen Ihrer Web-App im Azure-Portal

Wechseln Sie zum [Azure-Portal](https://portal.azure.com), um die erstellte Web-App anzuzeigen.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/tutorial-python-postgresql-app/app-resource.png)

Standardmäßig wird im Portal die Seite **Übersicht** Ihrer Web-App angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten links auf der Seite zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![App Service-Seite im Azure-Portal](./media/tutorial-python-postgresql-app/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer PostgreSQL-Datenbank in Azure
> * Herstellen einer Verbindung mit PostgreSQL für eine Python-App
> * Bereitstellen der Anwendung in Azure
> * Anzeigen von Diagnoseprotokollen
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Verwalten der App im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihrer Web-App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"]
> [Konfigurieren des integrierten Python-Image](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps](../app-service-web-tutorial-custom-domain.md)

