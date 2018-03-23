---
title: Lokales Bereitstellen der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird gezeigt, wie die vorkonfigurierte Remoteüberwachungslösung auf Ihrem lokalen Computer für Test und Entwicklung bereitgestellt wird.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 03/07/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 77f40e2f10cbdb9930a22a4248e19bb3356af7af
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution-locally"></a>Lokales Bereitstellen der vorkonfigurierten Remoteüberwachungslösung

In diesem Artikel wird gezeigt, wie die vorkonfigurierte Remoteüberwachungslösung auf Ihrem lokalen Computer für Test und Entwicklung bereitgestellt wird. Dieser Ansatz stellt die Microservices in einem lokalen Docker-Container bereit und nutzt IoT Hub-, Cosmos DB- und Azure-Speicherdienste in der Cloud. Sie verwenden die CLI für vorkonfigurierte Lösungen (PCS, Preconfigured Solutions), um die Azure-Clouddienste bereitzustellen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Bereitstellung der von der vorkonfigurierten Remoteüberwachungslösung benötigten Azure-Dienste benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](http://azure.microsoft.com/pricing/free-trial/).

Um die lokale Bereitstellung abzuschließen, müssen die folgenden Tools auf dem lokalen Entwicklungscomputer installiert sein:

* [Git-Client](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker Compose](https://docs.docker.com/compose/install/)
* [Node.js](https://nodejs.org/) – diese Software ist eine Voraussetzung für die PCS-CLI.
* PCS-CLI

> [!NOTE]
> Diese Tools sind auf vielen Plattformen einschließlich Windows, Linux und iOS verfügbar.

### <a name="install-the-pcs-cli"></a>Installieren der PCS-CLI

Führen Sie zum Installieren der Befehlszeilenschnittstelle den folgenden Befehl in der Befehlszeilenumgebung aus:

```cmd/sh
npm install iot-solutions -g
```

Weitere Informationen zur Befehlszeilenschnittstelle finden Sie unter [How to use the CLI](https://github.com/Azure/pcs-cli/blob/master/README.md) (Verwenden der Befehlszeilenschnittstelle).

## <a name="deploy-the-azure-services"></a>Bereitstellen von Azure-Diensten

In diesem Artikel erfahren Sie zwar, wie Sie die Microservices lokal ausführen, doch sie hängen von drei Azure-Diensten ab, die in der Cloud ausgeführt werden. Sie können diese Azure-Dienste manuell über das Azure-Portal bereitstellen oder die PCS-CLI verwenden. Dieser Artikel beschreibt, wie Sie das `pcs`-Tool verwenden.

### <a name="sign-in-to-the-cli"></a>Anmelden bei der Befehlszeilenschnittstelle

Damit Sie die vorkonfigurierte Lösung bereitstellen können, müssen Sie sich wie folgt über die Befehlszeilenschnittstelle bei Ihrem Azure-Abonnement anmelden:

```cmd/sh
pcs login
```

Befolgen Sie die Anweisungen auf dem Bildschirm, um den Anmeldevorgang abzuschließen.

### <a name="run-a-local-deployment"></a>Ausführen einer lokalen Bereitstellung

Verwenden Sie den folgenden Befehl zum Starten der lokalen Bereitstellung:

```cmd/pcs
pcs -s local
```

Das Skript fordert Sie zur Eingabe der folgenden Informationen auf:

* Ein Lösungsname.
* Das zu verwendende Azure-Abonnement.
* Der Speicherort des zu verwendenden Azure-Rechenzentrums.

Das Skript erstellt eine IoT Hub-Instanz, eine Cosmos DB-Instanz und ein Azure-Speicherkonto in einer Ressourcengruppe in Ihrem Azure-Abonnement. Der Name der Ressourcengruppe ist der Name der Lösung, die Sie ausgewählt haben, als Sie das `pcs`-Tool ausgeführt haben.

Die Ausführung des Skripts dauert mehrere Minuten. Wenn der Vorgang abgeschlossen ist, sehen Sie eine Meldung `Copy the following environment variables to /scripts/local/.env file:`. Kopieren Sie die Umgebungsvariablendefinitionen, die der Nachricht folgen, denn Sie werden sie in einem späteren Schritt verwenden.

## <a name="download-the-source-code"></a>Herunterladen des Quellcodes

Das Remoteüberwachungs-Quellcoderepository umfasst die Docker-Konfigurationsdateien, die Sie benötigen, um die Docker-Images, die die Microservices enthalten, herunterzuladen, zu konfigurieren und auszuführen. Um das Repository zu klonen, navigieren Sie zu einem geeigneten Ordner auf dem lokalen Computer, und führen Sie einen der folgenden Befehle aus:

Führen Sie zum Installieren der Java-Implementierungen der Microservices Folgendes aus:

```cmd/sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

Führen Sie zum Installieren der .Net-Implementierungen der Microservices Folgendes aus:

```cmd\sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

Diese Befehle laden den Quellcode für alle Microservices herunter. Obwohl Sie den Quellcode nicht zum Ausführen der Microservices in Docker benötigen, ist der Quellcode hilfreich, wenn Sie später die vorkonfigurierte Lösung ändern und Ihre Änderungen lokal testen möchten.

## <a name="run-the-microservices-in-docker"></a>Ausführen der Microservices in Docker

Zum Ausführen der Microservices in Docker bearbeiten Sie zunächst die **scripts\\local\\.env**-Datei in der lokalen Kopie des Repositorys. Ersetzen Sie den gesamten Inhalt der Datei mit den Definitionen der Umgebungsvariablen, die Sie zuvor bei der Ausführung des `pcs`-Befehls notiert haben. Diese Umgebungsvariablen ermöglichen den Microservices im Docker-Container, eine Verbindung mit den Azure-Diensten herzustellen, die vom `pcs`-Tool erstellt wurden.

Navigieren Sie zum Ausführen der vorkonfigurierten Lösung zum Ordner **scripts\local** in Ihrer Befehlszeilenumgebung, und führen Sie den folgenden Befehl aus:

```cmd\sh
docker-compose up
```

Wenn Sie diesen Befehl zum ersten Mal ausführen, lädt Docker die Microservice-Images vom Docker-Hub herunter, um die Container lokal zu erstellen. Bei nachfolgenden Ausführungen führt Docker die Container sofort aus.

Sie können zum Anzeigen der Protokolle aus dem Container eine separate Shell verwenden. Suchen Sie zuerst die Container-ID mithilfe des `docker ps -a`-Befehls. Verwenden Sie dann `docker logs {container-id} --tail 1000` zur Anzeige der letzten 1.000 Protokolleinträge für den angegebenen Container.

Navigieren Sie für den Zugriff auf das Dashboard der Remoteüberwachungslösung in Ihrem Browser zu [http://localhost: 8080](http://localhost:8080).

## <a name="tidy-up"></a>Aufräumen

Um unnötige Gebühren zu vermeiden, entfernen Sie nach Abschluss Ihrer Tests die Clouddienste aus Ihrem Azure-Abonnement. Am einfachsten entfernen Sie die Dienste im Azure-Portal, indem Sie die vom `pcs`-Tool erstellte Ressourcengruppe löschen.

Entfernen Sie mit dem `docker-compose down --rmi all`-Befehl die Docker-Images, und geben Sie Speicherplatz auf dem lokalen Computer frei. Sie können auch die lokale Kopie des Remoteüberwachungsrepositorys löschen, das erstellt wurde, als Sie den Quellcode aus GitHub geklont haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konfigurieren der vorkonfigurierten Lösung
> * Bereitstellen der vorkonfigurierten Lösung
> * Anmelden bei der vorkonfigurierten Lösung

Nach dem Bereitstellen der Remoteüberwachungslösung können Sie nun im nächsten Schritt [die Funktionen des Lösungsdashboards untersuchen](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->