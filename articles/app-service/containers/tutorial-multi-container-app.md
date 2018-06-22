---
title: Erstellen einer App mit mehreren Containern (Vorschauversion) über Azure Web-App für Container
description: Es wird beschrieben, wie Sie mehrere Container in Azure mit Docker Compose- und Kubernetes-Konfigurationsdateien per WordPress- und MySQL-App verwenden.
keywords: Azure App Service, Web-App, Linux, Docker, Compose, mehrere Container, Container, Kubernetes
services: app-service
documentationcenter: ''
author: msangapu
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/02/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: 61158af0bc978665c3d914c8de3376b8f5d5c69f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34651380"
---
# <a name="tutorial-create-a-multi-container-preview-app-in-web-app-for-containers"></a>Tutorial: Erstellen einer App mit mehreren Containern (Vorschauversion) über Web-App für Container

[Web-App für Container](app-service-linux-intro.md) umfasst eine flexible Möglichkeit zum Verwenden von Docker-Images. In diesem Tutorial wird beschrieben, wie Sie eine App mit mehreren Containern per WordPress und MySQL erstellen.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Konvertieren einer Docker Compose-Konfiguration für Web-App für Container
> * Konvertieren einer Kubernetes Compose-Konfiguration für Web-App für Container
> * Bereitstellen einer App mit mehreren Containern in Azure
> * Hinzufügen von Anwendungseinstellungen
> * Verwenden von beständigem Speicher für Ihre Container
> * Herstellen einer Verbindung mit Azure-Datenbank für MySQL
> * Beheben von Fehlern

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Installation der [Azure CLI](/cli/azure/install-azure-cli) (2.0.32 oder höher)
* Erfahrung mit [Docker Compose](https://docs.docker.com/compose/) oder [Kubernetes](https://kubernetes.io/)

## <a name="create-a-deployment-user"></a>Erstellen eines Bereitstellungsbenutzers

Erstellen Sie an der lokalen Eingabeaufforderung Anmeldeinformationen für die Bereitstellung, indem Sie den Befehl [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az_webapp_deployment_user_set) verwenden. Bei der FTP- und der lokalen Git-Bereitstellung für eine Web-App ist dieser Bereitstellungsbenutzer erforderlich. Der Benutzername und das Kennwort gelten auf der Kontoebene. _Sie unterscheiden sich von den Anmeldeinformationen Ihres Azure-Abonnements._

Ersetzen Sie im folgenden Beispiel *\<username>* und *\<password>* (einschließlich Klammern) durch einen neuen Benutzernamen und ein neues Kennwort. Der Benutzername muss in Azure eindeutig sein. Das Kennwort muss mindestens acht Zeichen lang sein und zwei der folgenden drei Elemente enthalten: Buchstaben, Zahlen, Symbole.

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Sie sollten eine JSON-Ausgabe erhalten, in der das Kennwort als `null` angezeigt wird. Wenn Sie den Fehler `'Conflict'. Details: 409` erhalten, müssen Sie den Benutzernamen ändern. Wenn Sie den Fehler `'Bad Request'. Details: 400` erhalten, müssen Sie ein sichereres Kennwort verwenden.

Sie müssen diesen Bereitstellungsbenutzer nur einmal erstellen und können ihn dann für alle Azure-Bereitstellungen verwenden.

> [!NOTE]
> Notieren Sie sich den Benutzernamen und das Kennwort. Sie verwenden diese Angaben später für die Bereitstellung der Web-App.
>
>

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

Erstellen Sie an der lokalen Eingabeaufforderung mit dem Befehl [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) eine Ressourcengruppe. Im folgenden Beispiel wird am Standort *South Central US* (USA, Süden-Mitte) die Ressourcengruppe *myResourceGroup* erstellt. Wenn Sie alle unterstützten Standorte für App Service unter Linux im **Standard**-Tarif anzeigen möchten, führen Sie den Befehl [`az appservice list-locations --sku S1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations) aus.

```azurecli-interactive
az group create --name myResourceGroup --location "South Central US"
```

Im Allgemeinen erstellen Sie Ressourcengruppen und Ressourcen in einer Region in Ihrer Nähe.

Nach Ausführung dieses Befehls werden die Ressourcengruppeneigenschaften in einer JSON-Ausgabe angezeigt.

## <a name="create-an-azure-app-service-plan"></a>Erstellen eines Azure App Service-Plans

Erstellen Sie an der lokalen Eingabeaufforderung mit dem Befehl [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) einen App Service-Plan in der Ressourcengruppe.

<!-- [!INCLUDE [app-service-plan](app-service-plan-linux.md)] -->

Im folgenden Beispiel wird ein App Service-Plan mit dem Namen `myAppServicePlan` in einem Linux-Container (`--is-linux`) für den Tarif **Standard** (`--sku S1`) erstellt.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Nach Erstellung des App Service-Plans zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "South Central US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "South Central US",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

## <a name="docker-compose-configuration-options"></a>Docker Compose-Konfigurationsoptionen

Für dieses Tutorial verwenden Sie die Compose-Datei von [Docker](https://docs.docker.com/compose/wordpress/#define-the-project), aber Sie ändern sie, um Azure Database for MySQL, beständigen Speicher und Redis einzubinden. Alternativ hierzu können Sie auch eine [Kubernetes-Konfiguration](#use-a-kubernetes-configuration-optional) nutzen. Die Konfigurationsdateien finden Sie in den [Beispielen für Azure](https://github.com/Azure-Samples/multicontainerwordpress).

In den folgenden Listen werden unterstützte und nicht unterstützte Docker Compose-Konfigurationsoptionen in Web-App für Container angezeigt:

### <a name="supported-options"></a>Unterstützte Optionen

* command
* entrypoint
* Environment
* image
* ports
* restart
* services
* volumes

### <a name="unsupported-options"></a>Nicht unterstützte Optionen

* build (unzulässig)
* depends_on (ignoriert)
* networks (ignoriert)
* secrets (ignoriert)

> [!NOTE]
> Alle weiteren Optionen, die nicht ausdrücklich aufgeführt sind, werden in der Public Preview ebenfalls ignoriert.

### <a name="docker-compose-with-wordpress-and-mysql-containers"></a>Docker Compose mit WordPress und MySQL-Containern

Kopieren Sie den folgenden YAML-Code, und fügen Sie ihn lokal in eine Datei mit dem Namen `compose-wordpress.yml` ein.

[!code-yml[Main](../../../azure-app-service-multi-container/docker-compose-wordpress.yml)]

## <a name="create-a-docker-compose-app"></a>Erstellen einer Docker Compose-App

Erstellen Sie in Ihrem lokalen Eingabeaufforderungsterminal eine [Web-App](app-service-linux-intro.md) mit mehreren Containern im `myAppServicePlan` App Service-Plan, indem Sie den Befehl [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) verwenden. Vergessen Sie nicht, _\<app_name>_ durch einen eindeutigen App-Namen zu ersetzen.

```bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

```json
{
  "additionalProperties": {},
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie zur bereitgestellten App unter `http://<app_name>.azurewebsites.net`. Das Laden der App kann einige Minuten dauern. Wenn Sie einen Fehler erhalten, sollten Sie einige Minuten warten und dann den Browser aktualisieren. Falls Probleme auftreten und Sie die Problembehandlung durchführen möchten, helfen Ihnen die [Containerprotokolle](#find-docker-container-logs) weiter.

![Beispiel-App mit mehreren Containern für Web-App für Container][1]

**Glückwunsch!** Sie haben in Web-App für Container eine App mit mehreren Containern erstellt. Als Nächstes konfigurieren Sie Ihre App für die Verwendung mit Azure Database for MySQL. Warten Sie noch mit der Installation von WordPress.

## <a name="connect-to-production-database"></a>Herstellen einer Verbindung mit der Produktionsdatenbank

Es wird nicht empfohlen, Datenbankcontainer in einer Produktionsumgebung zu verwenden. Die lokalen Container sind nicht skalierbar. Stattdessen nutzen Sie Azure Database for MySQL, da dieser Dienst skaliert werden kann.

### <a name="create-an-azure-database-for-mysql-server"></a>Erstellen eines Azure Database for MySQL-Servers

Erstellen Sie mit dem Befehl [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) einen Azure Database for MySQL-Server.

Ersetzen Sie im folgenden Befehl den Platzhalter _&lt;mysql_server_name>_ durch Ihren MySQL-Servernamen (gültige Zeichen sind `a-z`, `0-9` und `-`). Dieser Name ist Teil des Hostnamens des MySQL-Servers (`<mysql_server_name>.database.windows.net`) und muss global eindeutig sein.

```azurecli-interactive
az mysql server create --resource-group myResourceGroup --name <mysql_server_name>  --location "South Central US" --admin-user adminuser --admin-password My5up3rStr0ngPaSw0rd! --sku-name B_Gen4_1 --version 5.7
```

Nach dem Erstellen des MySQL-Servers zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "southcentralus",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Konfigurieren der Serverfirewall

Erstellen Sie mit dem Befehl [`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) eine Firewallregel für Ihren MySQL-Server, um Clientverbindungen zuzulassen. Wenn sowohl Start- als auch End-IP auf 0.0.0.0 festgelegt ist, wird die Firewall nur für andere Azure-Ressourcen geöffnet.

```azurecli-interactive
az mysql server firewall-rule create --name allAzureIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Sie können Ihre Firewallregel auch noch restriktiver gestalten und [nur die ausgehenden IP-Adressen verwenden, die Ihre App verwendet](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).
>

### <a name="create-the-wordpress-database"></a>Erstellen der WordPress-Datenbank

```bash
az mysql db create --resource-group myResourceGroup --server-name <mysql_server_name> --name wordpress
```

Nach Erstellung der Datenbank zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "additionalProperties": {},
  "charset": "latin1",
  "collation": "latin1_swedish_ci",
  "id": "/subscriptions/12db1644-4b12-4cab-ba54-8ba2f2822c1f/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>/databases/wordpress",
  "name": "wordpress",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DBforMySQL/servers/databases"
}
```

### <a name="configure-database-variables-in-wordpress"></a>Konfigurieren von Datenbankvariablen in WordPress

Zum Herstellen einer Verbindung für die WordPress-App mit diesem neuen MySQL-Server konfigurieren Sie einige WordPress-spezifische Umgebungsvariablen, z.B. den mit `MYSQL_SSL_CA` definierten SSL-Zertifizierungsstellenpfad. [Baltimore CyberTrust Root](https://www.digicert.com/digicert-root-certificates.htm) von [DigiCert](http://www.digicert.com/) ist unten im [benutzerdefinierten Image](https://docs.microsoft.com/en-us/azure/app-service/containers/tutorial-multi-container-app#use-a-custom-image-for-mysql-ssl-and-other-configurations) angegeben.

Verwenden Sie zum Vornehmen dieser Änderungen den Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) im lokalen Terminal für die Befehlseingabe. Bei App-Einstellungen wird die Groß-/Kleinschreibung berücksichtigt und die Einstellungen sind durch Leerzeichen getrennt.

```bash
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WORDPRESS_DB_HOST="<mysql_server_name>.mysql.database.azure.com" WORDPRESS_DB_USER="adminuser@<mysql_server_name>" WORDPRESS_DB_PASSWORD="My5up3rStr0ngPaSw0rd!" WORDPRESS_DB_NAME="wordpress" MYSQL_SSL_CA="BaltimoreCyberTrustroot.crt.pem"
```

Nach Erstellung der App-Einstellung zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
[
  {
    "name": "WORDPRESS_DB_HOST",
    "slotSetting": false,
    "value": "<mysql_server_name>.mysql.database.azure.com"
  },
  {
    "name": "WORDPRESS_DB_USER",
    "slotSetting": false,
    "value": "adminuser@<mysql_server_name>"
  },
  {
    "name": "WORDPRESS_DB_NAME",
    "slotSetting": false,
    "value": "wordpress"
  },
  {
    "name": "WORDPRESS_DB_PASSWORD",
    "slotSetting": false,
    "value": "My5up3rStr0ngPaSw0rd!"
  },
  {
    "name": "MYSQL_SSL_CA",
    "slotSetting": false,
    "value": "BaltimoreCyberTrustroot.crt.pem"
  }
]
```

### <a name="use-a-custom-image-for-mysql-ssl-and-other-configurations"></a>Verwenden eines benutzerdefinierten Images für MySQL-SSL und andere Konfigurationen

Von Azure Database for MySQL wird standardmäßig SSL genutzt. Für WordPress ist eine zusätzliche Konfiguration erforderlich, um SSL mit MySQL nutzen zu können. Mit dem offiziellen Image von WordPress wird nicht diese zusätzliche Konfiguration bereitgestellt, sondern es wurde ein [benutzerdefiniertes Image](https://hub.docker.com/r/microsoft/multicontainerwordpress/builds/) erstellt, um Ihnen den Vorgang zu vereinfachen. In der Praxis fügen Sie Ihrem eigenen Image die gewünschten Änderungen hinzu.

Das benutzerdefinierte Image basiert auf dem „offiziellen Image“ von [WordPress per Docker-Hub](https://hub.docker.com/_/wordpress/). An diesem benutzerdefinierten Image für Azure Database for MySQL wurden die folgenden Änderungen vorgenommen:

* [Die Baltimore Cyber Trust Root-Zertifikatsdatei für SSL wurde MySQL hinzugefügt.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L61)
* [Die App-Einstellung für das MySQL-SSL-Zertifizierungsstellenzertifikat wird in WordPress „wp-config.php“ verwendet.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L163)
* [Die für MySQL-SSL erforderliche WordPress-Definition für MYSQL_CLIENT_FLAGS wurde hinzugefügt.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L164)

Die folgenden Änderungen wurden für Redis vorgenommen (Verwendung in einem späteren Abschnitt):

* [Die PHP-Erweiterung für Redis v4.0.2 wurde hinzugefügt.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/Dockerfile#L35)
* [Erforderliches Entzippen für die Dateiextraktion wurde hinzugefügt.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L71)
* [Das Redis Object Cache 1.3.8-WordPress-Plug-In wurde hinzugefügt.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L74)
* [Die App-Einstellung für den Redis-Hostnamen in der WordPress-Datei „wp-config.php“ wird verwendet.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L162)

Für die Verwendung des benutzerdefinierten Images müssen Sie Ihre Datei „compose-wordpress.yml“ aktualisieren. Ändern Sie `image: wordpress` so, dass `image: microsoft/multicontainerwordpress` verwendet wird. Den Datenbankcontainer benötigen Sie nicht mehr. Entfernen Sie die Abschnitte `db`, `environment`, `depends_on` und `volumes` aus der Konfigurationsdatei. Ihre Datei sollte wie im folgenden Code aussehen:

```yaml
version: '3.3'

services:
   wordpress:
     image: microsoft/multicontainerwordpress
     ports:
       - "8000:80"
     restart: always
```

### <a name="update-app-with-new-configuration"></a>Aktualisieren der App mit einer neuen Konfiguration

Konfigurieren Sie Ihre [Web-App](app-service-linux-intro.md) mit mehreren Containern im lokalen Terminal für die Befehlseingabe mit dem Befehl [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set) neu. Vergessen Sie nicht, _\<app_name>_ durch den Namen der zuvor erstellten Web-App zu ersetzen.

```bash
az webapp config container set --resource-group myResourceGroup --name <app_name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

Nachdem die App neu konfiguriert wurde, zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "COMPOSE|dmVyc2lvbjogJzMuMycKCnNlcnZpY2VzOgogICB3b3JkcHJlc3M6CiAgICAgaW1hZ2U6IG1zYW5nYXB1L3dvcmRwcmVzcwogICAgIHBvcnRzOgogICAgICAgLSAiODAwMDo4MCIKICAgICByZXN0YXJ0OiBhbHdheXM="
  }
]
```

### <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie zur bereitgestellten App unter `http://<app_name>.azurewebsites.net`. Die App nutzt jetzt Azure Database for MySQL.

![Beispiel-App mit mehreren Containern für Web-App für Container][1]

## <a name="add-persistent-storage"></a>Hinzufügen von beständigem Speicher

Ihre App mit mehreren Containern wird jetzt in Web-App für Container ausgeführt. Wenn Sie jetzt WordPress installieren und Ihre App später neu starten, merken Sie, dass Ihre WordPress-Installation nicht mehr vorhanden ist. Dies ist der Fall, weil in Ihrer Docker Compose-Konfiguration derzeit auf einen Speicherort innerhalb Ihres Containers verwiesen wird. Die im Container installierten Dateien werden nicht über den Neustart der App hinaus beibehalten. In diesem Abschnitt fügen Sie Ihrem WordPress-Container beständigen Speicher hinzu.

### <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Für die Nutzung des beständigen Speichers aktivieren Sie die Einstellung in App Service. Verwenden Sie zum Vornehmen dieser Änderung den Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) im lokalen Terminal für die Befehlseingabe. Bei App-Einstellungen wird die Groß-/Kleinschreibung berücksichtigt und die Einstellungen sind durch Leerzeichen getrennt.

```bash
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

Nach Erstellung der App-Einstellung zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
[
  < JSON data removed for brevity. >
  {
    "name": "WORDPRESS_DB_NAME",
    "slotSetting": false,
    "value": "wordpress"
  },
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "TRUE"
  }
]
```

### <a name="modify-configuration-file"></a>Ändern der Konfigurationsdatei

Öffnen Sie erneut *compose-wordpress.yml*.

Mit der Option `volumes` wird das Dateisystem einem Verzeichnis im Container zugeordnet. `${WEBAPP_STORAGE_HOME}` ist eine Umgebungsvariable in App Service, die dem beständigen Speicher für Ihre App zugeordnet wird. Sie verwenden diese Umgebungsvariable in der Option „volumes“, damit die WordPress-Dateien nicht im Container installiert werden, sondern im beständigen Speicher. Nehmen Sie an der Datei die folgenden Änderungen vor:

Fügen Sie im Abschnitt `wordpress` die Option `volumes` hinzu, damit er wie im folgenden Codebeispiel aussieht:

```yaml
version: '3.3'

services:
   wordpress:
     image: microsoft/multicontainerwordpress
     volumes:
      - ${WEBAPP_STORAGE_HOME}/site/wwwroot:/var/www/html
     ports:
       - "8000:80"
     restart: always
```

### <a name="update-app-with-new-configuration"></a>Aktualisieren der App mit einer neuen Konfiguration

Konfigurieren Sie Ihre [Web-App](app-service-linux-intro.md) mit mehreren Containern im lokalen Terminal für die Befehlseingabe mit dem Befehl [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set) neu. Vergessen Sie nicht, _\<app_name>_ durch einen eindeutigen App-Namen zu ersetzen.

```bash
az webapp config container set --resource-group myResourceGroup --name <app_name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

Nachdem der Befehl ausgeführt wurde, wird die Ausgabe in etwa wie im folgenden Beispiel angezeigt:

```json
[
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "TRUE"
  },
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "COMPOSE|dmVyc2lvbjogJzMuMycKCnNlcnZpY2VzOgogICBteXNxbDoKICAgICBpbWFnZTogbXlzcWw6NS43CiAgICAgdm9sdW1lczoKICAgICAgIC0gZGJfZGF0YTovdmFyL2xpYi9teXNxbAogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgTVlTUUxfUk9PVF9QQVNTV09SRDogZXhhbXBsZXBhc3MKCiAgIHdvcmRwcmVzczoKICAgICBkZXBlbmRzX29uOgogICAgICAgLSBteXNxbAogICAgIGltYWdlOiB3b3JkcHJlc3M6bGF0ZXN0CiAgICAgcG9ydHM6CiAgICAgICAtICI4MDAwOjgwIgogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgV09SRFBSRVNTX0RCX1BBU1NXT1JEOiBleGFtcGxlcGFzcwp2b2x1bWVzOgogICAgZGJfZGF0YTo="
  }
]
```

### <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie zur bereitgestellten App unter `http://<app_name>.azurewebsites.net`.

Für den WordPress-Container werden jetzt Azure Database for MySQL und beständiger Speicher verwendet.

## <a name="add-redis-container"></a>Hinzufügen des Redis-Containers

 Im „offiziellen Image“ von WordPress sind die Abhängigkeiten für Redis nicht enthalten. Diese Abhängigkeiten und die zusätzliche Konfiguration, die für die Verwendung von Redis mit WordPress erforderlich ist, wurden für Sie in diesem [benutzerdefinierten Image](https://github.com/Azure-Samples/multicontainerwordpress) vorbereitet. In der Praxis fügen Sie Ihrem eigenen Image die gewünschten Änderungen hinzu.

Das benutzerdefinierte Image basiert auf dem „offiziellen Image“ von [WordPress per Docker-Hub](https://hub.docker.com/_/wordpress/). Die folgenden Änderungen wurden an diesem benutzerdefinierten Image für Redis vorgenommen:

* [Die PHP-Erweiterung für Redis v4.0.2 wurde hinzugefügt.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/Dockerfile#L35)
* [Erforderliches Entzippen für die Dateiextraktion wurde hinzugefügt.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L71)
* [Das Redis Object Cache 1.3.8-WordPress-Plug-In wurde hinzugefügt.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L74)
* [Die App-Einstellung für den Redis-Hostnamen in der WordPress-Datei „wp-config.php“ wird verwendet.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L162)

Fügen Sie den Redis-Container unten in der Konfigurationsdatei hinzu, damit diese wie im folgenden Beispiel aussieht:

[!code-yml[Main](../../../azure-app-service-multi-container/compose-wordpress.yml)]

### <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Für die Nutzung von Redis aktivieren Sie die Einstellung `WP_REDIS_HOST` in App Service. Dies ist eine *erforderliche Einstellung* für WordPress, um die Kommunikation mit dem Redis-Host zu ermöglichen. Verwenden Sie zum Vornehmen dieser Änderung den Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) im lokalen Terminal für die Befehlseingabe. Bei App-Einstellungen wird die Groß-/Kleinschreibung berücksichtigt und die Einstellungen sind durch Leerzeichen getrennt.

```bash
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WP_REDIS_HOST="redis"
```

Nach Erstellung der App-Einstellung zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
[
  < JSON data removed for brevity. >
  {
    "name": "WORDPRESS_DB_USER",
    "slotSetting": false,
    "value": "adminuser@<mysql_server_name>"
  },
  {
    "name": "WP_REDIS_HOST",
    "slotSetting": false,
    "value": "redis"
  }
]
```

### <a name="update-app-with-new-configuration"></a>Aktualisieren der App mit einer neuen Konfiguration

Konfigurieren Sie Ihre [Web-App](app-service-linux-intro.md) mit mehreren Containern im lokalen Terminal für die Befehlseingabe mit dem Befehl [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set) neu. Vergessen Sie nicht, _\<app_name>_ durch einen eindeutigen App-Namen zu ersetzen.

```bash
az webapp config container set --resource-group myResourceGroup --name <app_name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

Nachdem der Befehl ausgeführt wurde, wird die Ausgabe in etwa wie im folgenden Beispiel angezeigt:

```json
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "COMPOSE|dmVyc2lvbjogJzMuMycKCnNlcnZpY2VzOgogICBteXNxbDoKICAgICBpbWFnZTogbXlzcWw6NS43CiAgICAgdm9sdW1lczoKICAgICAgIC0gZGJfZGF0YTovdmFyL2xpYi9teXNxbAogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgTVlTUUxfUk9PVF9QQVNTV09SRDogZXhhbXBsZXBhc3MKCiAgIHdvcmRwcmVzczoKICAgICBkZXBlbmRzX29uOgogICAgICAgLSBteXNxbAogICAgIGltYWdlOiB3b3JkcHJlc3M6bGF0ZXN0CiAgICAgcG9ydHM6CiAgICAgICAtICI4MDAwOjgwIgogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgV09SRFBSRVNTX0RCX1BBU1NXT1JEOiBleGFtcGxlcGFzcwp2b2x1bWVzOgogICAgZGJfZGF0YTo="
  }
]
```

### <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie zur bereitgestellten App unter `http://<app_name>.azurewebsites.net`.

Führen Sie die Schritte aus, und installieren Sie WordPress.

### <a name="connect-wordpress-to-redis"></a>Herstellen einer Verbindung für WordPress mit Redis

Melden Sie sich an der WordPress-Administration an. Wählen Sie im linken Navigationsbereich die Option **Plugins** (Plug-Ins) und dann **Installed Plugins** (Installierte Plug-Ins).

![Auswählen von WordPress-Plug-Ins][2]

Anzeige aller Plug-Ins

Navigieren Sie auf der Seite mit den Plug-Ins zu **Redis Object Cache**, und klicken Sie auf **Activate** (Aktivieren).

![Aktivieren von Redis][3]

Klicken Sie auf **Settings** (Einstellungen).

![Klicken auf „Settings“ (Einstellungen)][4]

Klicken Sie auf die Schaltfläche **Enable Object Cache** (Object Cache aktivieren).

![Klicken auf die Schaltfläche „Enable Object Cache“ (Object Cache aktivieren)][5]

WordPress stellt die Verbindung mit dem Redis-Server her. Der **Status** der Verbindung wird auf derselben Seite angezeigt.

![WordPress stellt die Verbindung mit dem Redis-Server her. Der **Status** der Verbindung wird auf derselben Seite angezeigt.][6]

**Glückwunsch!**. Sie haben für WordPress eine Verbindung mit Redis hergestellt. Für die App, die jetzt für die Produktion bereit ist, werden nun **Azure Database for MySQL, beständiger Speicher und Redis** verwendet. Sie können Ihren App Service-Plan jetzt auf mehrere Instanzen horizontal hochskalieren.

## <a name="use-a-kubernetes-configuration-optional"></a>Verwenden einer Kubernetes-Konfiguration (optional)

In diesem Abschnitt wird beschrieben, wie Sie eine Kubernetes-Konfiguration zum Bereitstellen mehrerer Container verwenden. Stellen Sie sicher, dass Sie die obigen Schritte ausführen, um eine [Ressourcengruppe](#create-a-resource-group) und einen [App Service-Plan](#create-an-azure-app-service-plan) zu erstellen. Da die meisten Schritte den Schritten des Compose-Abschnitts ähneln, wurde die Konfigurationsdatei für Sie kombiniert.

### <a name="supported-kubernetes-options-for-multi-container"></a>Unterstützte Kubernetes-Optionen für mehrere Container

* args
* command
* containers
* image
* name
* ports
* spec

> [!NOTE]
>Alle weiteren Optionen, die nicht ausdrücklich aufgeführt sind, werden in der Public Preview nicht unterstützt.
>

### <a name="create-configuration-file"></a>Erstellen einer Konfigurationsdatei

Speichern Sie den folgenden YAML-Code in einer Datei mit dem Namen *kubernetes-wordpress.yml*.

[!code-yml[Main](../../../azure-app-service-multi-container/kubernetes-wordpress.yml)]

### <a name="create-an-azure-database-for-mysql-server"></a>Erstellen eines Servers für Azure-Datenbank für MySQL

Erstellen Sie in Azure Database for MySQL (Vorschauversion) mit dem Befehl [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) einen Server.

Ersetzen Sie im folgenden Befehl den Platzhalter _&lt;mysql_server_name>_ durch Ihren MySQL-Servernamen (gültige Zeichen sind `a-z`, `0-9` und `-`). Dieser Name ist Teil des Hostnamens des MySQL-Servers (`<mysql_server_name>.database.windows.net`) und muss global eindeutig sein.

```azurecli-interactive
az mysql server create --resource-group myResourceGroup --name <mysql_server_name>  --location "South Central US" --admin-user adminuser --admin-password My5up3rStr0ngPaSw0rd! --sku-name B_Gen4_1 --version 5.7
```

Nach dem Erstellen des MySQL-Servers zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "southcentralus",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Konfigurieren der Serverfirewall

Erstellen Sie mit dem Befehl [`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) eine Firewallregel für Ihren MySQL-Server, um Clientverbindungen zuzulassen. Wenn sowohl Start- als auch End-IP auf 0.0.0.0 festgelegt ist, wird die Firewall nur für andere Azure-Ressourcen geöffnet.

```azurecli-interactive
az mysql server firewall-rule create --name allAzureIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Sie können Ihre Firewallregel auch noch restriktiver gestalten und [nur die ausgehenden IP-Adressen verwenden, die Ihre App verwendet](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).
>

### <a name="create-the-wordpress-database"></a>Erstellen der WordPress-Datenbank

Erstellen Sie einen [Azure Database for MySQL-Server](#create-an-azure-database-for-mysql-server), falls Sie dies noch nicht getan haben.

```bash
az mysql db create --resource-group myResourceGroup --server-name <mysql_server_name> --name wordpress
```

Nach Erstellung der Datenbank zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "additionalProperties": {},
  "charset": "latin1",
  "collation": "latin1_swedish_ci",
  "id": "/subscriptions/12db1644-4b12-4cab-ba54-8ba2f2822c1f/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>/databases/wordpress",
  "name": "wordpress",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DBforMySQL/servers/databases"
}
```

### <a name="configure-database-variables-in-wordpress"></a>Konfigurieren von Datenbankvariablen in WordPress

Zum Herstellen einer Verbindung für die WordPress-App mit diesem neuen MySQL-Server konfigurieren Sie einige WordPress-spezifische Umgebungsvariablen. Verwenden Sie zum Vornehmen dieser Änderung den Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) im lokalen Terminal für die Befehlseingabe. Bei App-Einstellungen wird die Groß-/Kleinschreibung berücksichtigt und die Einstellungen sind durch Leerzeichen getrennt.

```bash
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WORDPRESS_DB_HOST="<mysql_server_name>.mysql.database.azure.com" WORDPRESS_DB_USER="adminuser@<mysql_server_name>" WORDPRESS_DB_PASSWORD="My5up3rStr0ngPaSw0rd!" WORDPRESS_DB_NAME="wordpress" MYSQL_SSL_CA="BaltimoreCyberTrustroot.crt.pem"
```

Nach Erstellung der App-Einstellung zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
[
  {
    "name": "WORDPRESS_DB_HOST",
    "slotSetting": false,
    "value": "<mysql_server_name>.mysql.database.azure.com"
  },
  {
    "name": "WORDPRESS_DB_USER",
    "slotSetting": false,
    "value": "adminuser@<mysql_server_name>"
  },
  {
    "name": "WORDPRESS_DB_NAME",
    "slotSetting": false,
    "value": "wordpress"
  },
  {
    "name": "WORDPRESS_DB_PASSWORD",
    "slotSetting": false,
    "value": "My5up3rStr0ngPaSw0rd!"
  }
]
```

### <a name="add-persistent-storage"></a>Hinzufügen von beständigem Speicher

Ihre App mit mehreren Containern wird jetzt in Web-App für Container ausgeführt. Die Daten werden beim Neustart gelöscht, weil die Dateien nicht dauerhaft gespeichert werden. In diesem Abschnitt fügen Sie Ihrem WordPress-Container beständigen Speicher hinzu.

### <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Für die Nutzung des beständigen Speichers aktivieren Sie die Einstellung in App Service. Verwenden Sie zum Vornehmen dieser Änderung den Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) im lokalen Terminal für die Befehlseingabe. Bei App-Einstellungen wird die Groß-/Kleinschreibung berücksichtigt und die Einstellungen sind durch Leerzeichen getrennt.

```bash
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

Nach Erstellung der App-Einstellung zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
[
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "TRUE"
  }
]
```

### <a name="create-a-multi-container-app-kubernetes"></a>Erstellen einer App mit mehreren Containern (Kubernetes)

Erstellen Sie in Ihrem lokalen Terminal für die Befehlseingabe eine [Web-App](app-service-linux-intro.md) mit mehreren Containern in der Ressourcengruppe `myResourceGroup` und im App Service-Plan `myAppServicePlan`, indem Sie den Befehl [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) verwenden. Vergessen Sie nicht, _\<app_name>_ durch einen eindeutigen App-Namen zu ersetzen.

```bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --multicontainer-config-type kube --multicontainer-config-file kubernetes-wordpress.yml
```

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie zur bereitgestellten App unter `http://<app_name>.azurewebsites.net`.

Die App führt jetzt mehrere Container in Web-App für Container aus.

![Beispiel-App mit mehreren Containern für Web-App für Container][1]

**Glückwunsch!** Sie haben in Web-App für Container eine App mit mehreren Containern erstellt.

Führen Sie für die Nutzung von Redis die Schritte unter [Herstellen einer Verbindung für WordPress mit Redis](#connect-wordpress-to-redis) aus.

## <a name="find-docker-container-logs"></a>Zugreifen auf Docker-Containerprotokolle

Falls bei der Verwendung mehrerer Container Probleme auftreten, können Sie auf die Containerprotokolle zugreifen, indem Sie zum folgenden Speicherort navigieren: `https://<app_name>.scm.azurewebsites.net/api/logs/docker`.

Ihnen wird daraufhin eine Ausgabe angezeigt, die in etwa wie im folgenden Beispiel aussieht:

```json
[
   {
      "machineName":"RD00XYZYZE567A",
      "lastUpdated":"2018-05-10T04:11:45Z",
      "size":25125,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_05_10_RD00XYZYZE567A_docker.log",
      "path":"/home/LogFiles/2018_05_10_RD00XYZYZE567A_docker.log"
   }
]
```

Es ist ein Protokoll für jeden Container und ein zusätzliches Protokoll für den übergeordneten Prozess vorhanden. Kopieren Sie den jeweiligen `href`-Wert in den Browser, um das Protokoll anzuzeigen.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Konvertieren einer Docker Compose-Konfiguration für Web-App für Container
> * Konvertieren einer Kubernetes Compose-Konfiguration für Web-App für Container
> * Bereitstellen einer App mit mehreren Containern in Azure
> * Hinzufügen von Anwendungseinstellungen
> * Verwenden von beständigem Speicher für Ihre Container
> * Herstellen einer Verbindung mit Azure-Datenbank für MySQL
> * Beheben von Fehlern

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden eines benutzerdefinierten Docker-Images für Web-App für Container](tutorial-custom-docker-image.md)

<!--Image references-->
[1]: ./media/tutorial-multi-container-app/azure-multi-container-wordpress-install.png
[2]: ./media/tutorial-multi-container-app/wordpress-plugins.png
[3]: ./media/tutorial-multi-container-app/activate-redis.png
[4]: ./media/tutorial-multi-container-app/redis-settings.png
[5]: ./media/tutorial-multi-container-app/enable-object-cache.png
[6]: ./media/tutorial-multi-container-app/redis-connected.png
