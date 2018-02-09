---
title: Erstellen einer Ruby- und MySQL-Web-App in Azure App Service unter Linux | Microsoft Docs
description: "Hier erhalten Sie Informationen zum Ausführen einer Ruby-App in Azure, die Sie mit einer MySQL-Datenbank in Azure verbinden."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: ruby
ms.topic: tutorial
ms.date: 12/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 951e66e47cf8fbe9d2cdf1606a8d63054bcada13
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="build-a-ruby-and-mysql-web-app-in-azure-app-service-on-linux"></a>Erstellen einer Ruby- und MySQL-Web-App in Azure App Service unter Linux

[App Service unter Linux](app-service-linux-intro.md) bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching unter Linux-Betriebssystemen. In diesem Tutorial wird gezeigt, wie Sie eine Ruby-Web-App erstellen und mit einer MySQL-Datenbank verbinden. Nachdem Sie diese Schritte ausgeführt haben, verfügen Sie über eine [Ruby on Rails](http://rubyonrails.org/)-App, die in App Service unter Linux ausgeführt wird.

![In Azure App Service ausgeführte Ruby on Rails-App](./media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer MySQL-Datenbank in Azure
> * Verbinden einer Ruby on Rails-App mit MySQL
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Diagnoseprotokollen aus Azure
> * Verwalten der App im Azure-Portal

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Installation von Git](https://git-scm.com/)
* [Installieren von Ruby 2.3](https://www.ruby-lang.org/documentation/installation/)
* [Installieren von Ruby on Rails 5.1](http://guides.rubyonrails.org/v5.1/getting_started.html)
* [Installieren und Starten von MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

## <a name="prepare-local-mysql"></a>Vorbereiten der lokalen MySQL-Instanz

In diesem Schritt erstellen Sie eine Datenbank auf einem lokalen MySQL-Server für die Verwendung in diesem Tutorial.

### <a name="connect-to-local-mysql-server"></a>Verbinden mit dem lokalen MySQL-Server

Stellen Sie in einem Terminalfenster eine Verbindung mit Ihrem lokalen MySQL-Server her. Sie können dieses Terminalfenster verwenden, um alle Befehle dieses Tutorials auszuführen.

```bash
mysql -u root -p
```

Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, geben Sie das Kennwort für das `root`-Konto ein. Wenn Sie das Kennwort für Ihr Root-Konto vergessen haben, lesen Sie unter [MySQL: How to Reset the Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html) (MySQL: Zurücksetzen des Root-Kennworts) nach.

Wenn der Befehl erfolgreich ausgeführt wurde, wird der MySQL-Server ausgeführt. Stellen Sie andernfalls anhand der [Nachinstallationsschritte für MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) sicher, dass Ihr lokaler MySQL-Server gestartet wurde.

Beenden Sie die Serververbindung durch Eingabe von `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-ruby-on-rails-app-locally"></a>Lokales Erstellen einer Ruby on Rails-App
In diesem Schritt rufen Sie eine Ruby on Rails-Beispielanwendung ab, konfigurieren ihre Datenbankverbindung und führen sie lokal aus. 

### <a name="clone-the-sample"></a>Klonen des Beispiels

Wechseln Sie im Terminalfenster mit `cd` in ein Arbeitsverzeichnis.

Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen.

```bash
git clone https://github.com/Azure-Samples/rubyrails-tasks.git
```

Wechseln Sie mit `cd` in das geklonte Verzeichnis. Installieren Sie die erforderlichen Pakete.

```bash
cd rubyrails-tasks
bundle install --path vendor/bundle
```

### <a name="configure-mysql-connection"></a>Konfigurieren der MySQL-Verbindung

Öffnen Sie im Repository die Datei _config/database.yml_, und geben Sie den lokalen MySQL-Root-Benutzernamen und das Kennwort (line 13) an. Wenn Sie MySQL mit einem Tool wie [Homebrew](https://brew.sh/) installiert haben, sind die Anmeldeinformationen in der Datei bereits auf die Standardwerte (auf `root` und ein leeres Kennwort) festgelegt.

```txt
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  socket: /tmp/mysql.sock
```

### <a name="run-the-sample-locally"></a>Lokales Ausführen des Beispiels

Führen Sie [die Rails-Migrationen](http://guides.rubyonrails.org/active_record_migrations.html#running-migrations) aus, um die von der Anwendung benötigten Tabellen zu erstellen. Im Git-Repository können Sie im Verzeichnis _db/migrate_ sehen, welche Tabellen bei den Migrationen erstellt werden.

```bash
rake db:create
rake db:migrate
```

Führen Sie die Anwendung aus.

```bash
rails server
```

Navigieren Sie in einem Browser zu `http://localhost:3000`. Fügen Sie auf der Seite einige Aufgaben hinzu.

![Erfolgreiche Verbindung zwischen Ruby on Rails und MySQL](./media/tutorial-ruby-mysql-app/mysql-connect-success.png)

Um den Rails-Server zu beenden, geben Sie `Ctrl + C` im Terminal ein.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Erstellen von MySQL in Azure

In diesem Schritt erstellen Sie eine MySQL-Datenbank in [Azure-Datenbank für MySQL (Vorschau)](/azure/mysql). Später konfigurieren Sie die Ruby on Rails-Anwendung für eine Verbindung mit dieser Datenbank.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Erstellen eines MySQL-Servers

Erstellen Sie in Azure Database for MySQL (Vorschauversion) mit dem Befehl [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) einen Server.

Ersetzen Sie im folgenden Befehl den Platzhalter _&lt;mysql_server_name>_ durch Ihren MySQL-Servernamen (gültige Zeichen sind `a-z`, `0-9` und `-`). Dieser Name ist Teil des Hostnamens des MySQL-Servers (`<mysql_server_name>.mysql.database.azure.com`) und muss global eindeutig sein.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password My5up3r$tr0ngPa$w0rd!
```

Nach dem Erstellen des MySQL-Servers zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Konfigurieren der Serverfirewall

Erstellen Sie mit dem Befehl [`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) eine Firewallregel für Ihren MySQL-Server, um Clientverbindungen zuzulassen.

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Für Azure-Datenbank für MySQL (Vorschauversion) besteht derzeit keine Beschränkung für ausschließliche Verbindungen mit Azure-Diensten. Da IP-Adressen in Azure dynamisch zugewiesen werden, ist es besser, alle IP-Adressen zu aktivieren. Der Dienst befindet sich in der Vorschauphase. Bessere Methoden zum Schützen Ihrer Datenbank sind geplant.
>

### <a name="connect-to-production-mysql-server-locally"></a>Lokales Verbinden mit dem MySQL-Produktionsserver

Stellen Sie im Terminalfenster eine Verbindung mit dem MySQL-Server in Azure her. Verwenden Sie den Wert, den Sie zuvor für _&lt;mysql_server_name>_ angegeben haben.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, verwenden Sie _My5up3r$tr0ngPa$w0rd!_, das Sie beim Erstellen des Datenbankservers angegeben haben.

### <a name="create-a-production-database"></a>Erstellen einer Produktionsdatenbank

Erstellen Sie an der Eingabeaufforderung `mysql` eine Datenbank.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Erstellen eines Benutzers mit Berechtigungen

Erstellen Sie einen Datenbankbenutzer mit dem Namen _railsappuser_, und weisen Sie ihm alle Berechtigungen in der Datenbank `sampledb` zu.

```sql
CREATE USER 'railsappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'railsappuser';
```

Beenden Sie die Serververbindung durch Eingabe von `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Verbinden der App mit Azure MySQL

In diesem Schritt verbinden Sie die Ruby on Rails-Anwendung mit der MySQL-Datenbank, die Sie in Azure Database for MySQL (Vorschau) erstellt haben.

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Konfigurieren der Datenbankverbindung

Öffnen Sie im Repository die Datei _config/database.yml_. Ersetzen Sie am Ende der Datei die Produktionsvariablen durch den folgenden Code. Verwenden Sie für den Platzhalter _&lt;mysql_server_name>_ den Namen des MySQL-Servers, den Sie erstellt haben.

```txt
production:
  <<: *default
  host: <%= ENV['DB_HOST'] %>
  port: 3306
  database: <%= ENV['DB_DATABASE'] %>
  username: <%= ENV['DB_USERNAME'] %>
  password: <%= ENV['DB_PASSWORD'] %>
  sslca: ssl/BaltimoreCyberTrustRoot.crt.pem
```

Speichern Sie die Änderungen.

> [!NOTE]
> Die `sslca` wird hinzugefügt und verweist auf eine im Beispielrepository vorhandene _PEM_-Datei. Standardmäßig erzwingt Azure-Datenbank für MySQL SSL-Verbindungen von Clients. Mit diesem `.pem`-Zertifikat stellen Sie SSL-Verbindungen mit Azure Database for MySQL her. Weitere Informationen finden Sie unter [Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit Azure-Datenbank für MySQL](../../mysql/howto-configure-ssl.md).
>

### <a name="test-the-application-locally"></a>Lokales Testen der Anwendung

Legen Sie im lokalen Terminal die folgenden Umgebungsvariablen fest:

```bash
export DB_HOST=<mysql_server_name>.mysql.database.azure.com
export DB_DATABASE=sampledb 
export DB_USERNAME=railsappuser@<mysql_server_name>
export DB_PASSWORD=MySQLAzure2017
```

Führen Sie die Rails-Datenbankmigrationen mit den Produktionswerten aus, die Sie soeben konfiguriert haben, um die Tabellen in Ihrer MySQL-Datenbank in Azure Database for MySQL (Vorschau) zu erstellen. 

```bash
rake db:migrate RAILS_ENV=production
```

Bei Ausführung in der Produktionsumgebung benötigt die Rails-Anwendung vorkompilierte Ressourcen. Generieren Sie die erforderlichen Ressourcen mit dem folgenden Befehl:

```bash
rake assets:precompile
```

In der Rails-Produktionsumgebung werden auch geheime Schlüssel zum Verwalten der Sicherheit verwendet. Generieren Sie einen geheimen Schlüssel.

```bash
rails secret
```

Speichern Sie den geheimen Schlüssel für die jeweiligen Variablen, die von der Rails-Produktionsumgebung verwendet werden. Verwenden Sie der Einfachheit halber für beide Variablen den gleichen Schlüssel.

```bash
export RAILS_MASTER_KEY=<output_of_rails_secret>
export SECRET_KEY_BASE=<output_of_rails_secret>
```

Aktivieren Sie die Rails-Produktionsumgebung für die Bereitstellung von JavaScript- und CSS-Dateien.

```bash
export RAILS_SERVE_STATIC_FILES=true
```

Führen Sie die Beispielanwendung in der Produktionsumgebung aus.

```bash
rails server -e production
```

Navigieren Sie zu `http://localhost:3000`. Wenn die Seite ohne Fehler geladen wird, stellt die Ruby on Rails-Anwendung eine Verbindung mit der MySQL-Datenbank in Azure her.

Fügen Sie auf der Seite einige Aufgaben hinzu.

![Erfolgreiche Verbindung zwischen Ruby on Rails und Azure Database for MySQL (Vorschau)](./media/tutorial-ruby-mysql-app/azure-mysql-connect-success.png)

Um den Rails-Server zu beenden, geben Sie `Ctrl + C` im Terminal ein.

### <a name="commit-your-changes"></a>Committen Ihrer Änderungen

Führen Sie die folgenden Git-Befehle aus, um für Ihre Änderungen einen Commit durchzuführen:

```bash
git add .
git commit -m "database.yml updates"
```

Ihre App kann jetzt bereitgestellt werden.

## <a name="deploy-to-azure"></a>Bereitstellen in Azure

In diesem Schritt stellen Sie die mit MySQL verbundene Rails-Anwendung in Azure App Service bereit.

### <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Erstellen einer Web-App

Erstellen Sie in Cloud Shell mit dem Befehl [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) eine Web-App im App Service-Plan `myAppServicePlan`. 

Ersetzen Sie im folgenden Beispiel `<app_name>` durch einen global eindeutigen App-Namen (gültige Zeichen sind `a-z`, `0-9` und `-`). Die Runtime ist auf `RUBY|2.3` festgelegt, die das [Ruby-Standardbild](https://hub.docker.com/r/appsvc/ruby/) bereitstellt. Führen Sie [`az webapp list-runtimes`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes) aus, um alle unterstützten Laufzeiten anzuzeigen. 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "RUBY|2.3" --deployment-local-git
```

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

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

Sie haben eine leere neue Web-App mit aktivierter Git-Bereitstellung erstellt.

> [!NOTE]
> Die URL des Git-Remotespeicherorts wird in der `deploymentLocalGitUrl`-Eigenschaft im Format `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git` angezeigt. Speichern Sie diese URL, da Sie sie später noch benötigen.
>

### <a name="configure-database-settings"></a>Konfigurieren der Datenbankeinstellungen

Verwenden Sie in App Service den Befehl [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) in Cloud Shell, um Umgebungsvariablen als _App-Einstellungen_ festzulegen.

Mit dem folgenden Cloud Shell-Befehl werden die App-Einstellungen `DB_HOST`, `DB_DATABASE`, `DB_USERNAME` und `DB_PASSWORD` konfiguriert. Ersetzen Sie die Platzhalter _&lt;appname>_ und _&lt;mysql_server_name>_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.mysql.database.azure.com" DB_DATABASE="sampledb" DB_USERNAME="railsappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017"
```

### <a name="configure-rails-environment-variables"></a>Konfigurieren der Rails-Umgebungsvariablen

Generieren Sie im lokalen Terminal einen neuen geheimen Schlüssel für die Rails-Produktionsumgebung in Azure.

```bash
rails secret
```

Konfigurieren Sie die für die Rails-Produktionsumgebung erforderlichen Variablen.

Ersetzen Sie im folgenden Cloud Shell-Befehl die beiden _&lt;output_of_rails_secret>_-Platzhalter durch den neuen geheimen Schlüssel, den Sie im lokalen Terminal generiert haben.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings RAILS_MASTER_KEY="<output_of_rails_secret>" SECRET_KEY_BASE="<output_of_rails_secret>" RAILS_SERVE_STATIC_FILES="true" ASSETS_PRECOMPILE="true"
```

`ASSETS_PRECOMPILE="true"` weist den Ruby-Standardcontainer an, bei jeder Git-Bereitstellung Ressourcen vorzukompilieren.

### <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

Fügen Sie im lokalen Terminal Ihrem lokalen Git-Repository eine Azure-Remoteinstanz hinzu.

```bash
git remote add azure <paste_copied_url_here>
```

Führen Sie einen Pushvorgang an die Azure-Remoteinstanz durch, um die Ruby on Rails-Anwendung bereitzustellen. Sie werden zur Angabe des Kennworts aufgefordert, das Sie zuvor bei der Erstellung des Bereitstellungsbenutzers angegeben haben.

```bash
git push azure master
```

Während der Bereitstellung meldet Azure App Service seinen Status mit Git.

```bash
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 291 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'a5e076db9c'.
remote: Running custom deployment command...
remote: Running deployment command...
...
< Output has been truncated for readability >
```

### <a name="browse-to-the-azure-web-app"></a>Navigieren Sie zur Azure-Web-App.

Browsen Sie zu `http://<app_name>.azurewebsites.net`, und fügen Sie der Liste einige Aufgaben hinzu.

![In Azure App Service ausgeführte Ruby on Rails-App](./media/tutorial-ruby-mysql-app/ruby-mysql-in-azure.png)

Glückwunsch! Sie führen eine datengesteuerte Ruby on Rails-App in Azure App Service aus.

## <a name="update-model-locally-and-redeploy"></a>Lokales Aktualisieren und erneutes Bereitstellen des Modells

In diesem Schritt nehmen Sie eine einfache Änderung am `task`-Datenmodell und der Web-App vor und veröffentlichen das Update dann in Azure.

Für das tasks-Szenario ändern Sie die Anwendung, damit Sie eine Aufgabe als abgeschlossen kennzeichnen können.

### <a name="add-a-column"></a>Hinzufügen eines Spaltennamens

Navigieren Sie im Terminal zum Stamm des Git-Repositorys.

Generieren Sie eine neue Migration, die der Tabelle `Tasks` eine boolesche Spalte namens `Done` hinzufügt:

```bash
rails generate migration add_Done_to_Tasks Done:boolean
```

Mit diesem Befehl wird im Verzeichnis _db/migrate_ eine neue Migrationsdatei generiert.


Führen Sie im Terminal Rails-Datenbankmigrationen aus, um die Änderung in der lokalen Datenbank vorzunehmen.

```bash
rake db:migrate
```

### <a name="update-application-logic"></a>Aktualisieren der Anwendungslogik

Öffnen Sie die Datei *app/controllers/tasks_controller.rb*. Suchen Sie am Ende der Datei nach der folgenden Zeile:

```rb
params.require(:task).permit(:Description)
```

Ändern Sie diese Zeile so, dass sie den neuen Parameter `Done` enthält.

```rb
params.require(:task).permit(:Description, :Done)
```

### <a name="update-the-views"></a>Aktualisieren der Ansichten

Öffnen Sie die Datei *app/views/tasks/_form.html.erb*, bei der es sich um das Bearbeitungsformular handelt.

Suchen Sie die Zeile `<%=f.error_span(:Description) %>`, und fügen Sie direkt darunter den folgenden Code ein:

```erb
<%= f.label :Done, :class => 'control-label col-lg-2' %>
<div class="col-lg-10">
  <%= f.check_box :Done, :class => 'form-control' %>
</div>
```

Öffnen Sie die Datei *app/views/tasks/show.html.erb*, bei der es sich um die Ansichtsseite mit einem Datensatz handelt. 

Suchen Sie die Zeile `<dd><%= @task.Description %></dd>`, und fügen Sie direkt darunter den folgenden Code ein:

```erb
  <dt><strong><%= model_class.human_attribute_name(:Done) %>:</strong></dt>
  <dd><%= check_box "task", "Done", {:checked => @task.Done, :disabled => true}%></dd>
```

Öffnen Sie die Datei *app/views/tasks/index.html.erb*, bei der es sich um die Indexseite für alle Datensätze handelt.

Suchen Sie die Zeile `<th><%= model_class.human_attribute_name(:Description) %></th>`, und fügen Sie direkt darunter den folgenden Code ein:

```erb
<th><%= model_class.human_attribute_name(:Done) %></th>
```

Suchen Sie in derselben Datei die Zeile `<td><%= task.Description %></td>`, und fügen Sie direkt darunter den folgenden Code ein:

```erb
<td><%= check_box "task", "Done", {:checked => task.Done, :disabled => true} %></td>
```

### <a name="test-the-changes-locally"></a>Lokales Testen der Änderungen

Führen Sie im lokalen Terminal den Rails-Server aus.

```bash
rails server
```

Navigieren Sie zu `http://localhost:3000`, um die Änderung des Aufgabenstatus anzuzeigen, und fügen Sie Elemente hinzu, oder bearbeiten Sie die Elemente.

![Hinzugefügtes Kontrollkästchen in der Aufgabe](./media/tutorial-ruby-mysql-app/complete-checkbox.png)

Um den Rails-Server zu beenden, geben Sie `Ctrl + C` im Terminal ein.

### <a name="publish-changes-to-azure"></a>Veröffentlichen von Änderungen in Azure

Führen Sie im Terminal Rails-Datenbankmigrationen für die Produktionsumgebung aus, um die Änderung in der Azure-Datenbank vorzunehmen.

```bash
rake db:migrate RAILS_ENV=production
```

Führen Sie für alle Änderungen in Git einen Commit aus, und übertragen Sie dann die Codeänderungen per Pushvorgang an Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Wechseln Sie nach Abschluss des `git push`-Vorgangs zur Azure-Web-App, und testen Sie die neuen Funktionen.

![Auf Azure veröffentlichte Änderungen an Modell und Datenbank](media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Wenn Sie Aufgaben hinzugefügt haben, werden sie in der Datenbank beibehalten. Bei Updates des Datenschemas bleiben vorhandene Daten erhalten.

## <a name="manage-the-azure-web-app"></a>Verwalten der Azure-Web-App

Wechseln Sie zum [Azure-Portal](https://portal.azure.com), um die erstellte Web-App zu verwalten.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-Web-App.

![Portalnavigation zur Azure-Web-App](./media/tutorial-php-mysql-app/access-portal.png)

Die Übersichtsseite Ihrer Web-App wird angezeigt. Hier können Sie einfache Verwaltungsaufgaben wie Beenden, Starten, Neustarten, Durchsuchen und Löschen durchführen.

Im linken Menü werden Seiten für die Konfiguration Ihrer App angezeigt.

![App Service-Seite im Azure-Portal](./media/tutorial-php-mysql-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer MySQL-Datenbank in Azure
> * Verbinden einer Ruby on Rails-App mit MySQL
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Diagnoseprotokollen aus Azure
> * Verwalten der App im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie einer Web-App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"]
> [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps](../app-service-web-tutorial-custom-domain.md)
