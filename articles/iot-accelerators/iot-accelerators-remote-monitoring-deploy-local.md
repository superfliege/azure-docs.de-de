---
title: Lokales Bereitstellen der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird gezeigt, wie der Solution Accelerator für Remoteüberwachung auf Ihrem lokalen Computer für Test und Entwicklung bereitgestellt wird.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/07/2018
ms.topic: conceptual
ms.openlocfilehash: 21bc8c27a44c940279b0c5bdcdbe04e579dc4bfa
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188660"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>Lokales Bereitstellen des Solution Accelerators für die Remoteüberwachung

In diesem Artikel wird gezeigt, wie der Solution Accelerator für die Remoteüberwachung zu Test- und Entwicklungszwecken auf Ihrem lokalen Computer bereitgestellt wird. Dieser Ansatz stellt die Microservices in einem lokalen Docker-Container bereit und nutzt IoT Hub-, Cosmos DB- und Azure-Speicherdienste in der Cloud. Sie verwenden die CLI für Solution Accelerators (PCS, Preconfigured Solutions), um die Azure-Clouddienste bereitzustellen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Bereitstellung der vom Solution Accelerator für die Remoteüberwachung benötigten Azure-Dienste benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](http://azure.microsoft.com/pricing/free-trial/).

Um die lokale Bereitstellung abzuschließen, müssen die folgenden Tools auf dem lokalen Entwicklungscomputer installiert sein:

* [Git-Client](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker Compose](https://docs.docker.com/compose/install/)
* [Node.js](https://nodejs.org/) – diese Software ist eine Voraussetzung für die PCS-CLI.
* PCS-CLI
* Lokales Repository des Quellcodes

> [!NOTE]
> Diese Tools sind auf vielen Plattformen einschließlich Windows, Linux und iOS verfügbar.

### <a name="install-the-pcs-cli"></a>Installieren der PCS-CLI

Führen Sie zum Installieren der PCS-CLI mit dem NPM (Network Performance Monitor, Netzwerkleistungsmonitor) den folgenden Befehl in der Befehlszeilenumgebung aus:

```cmd/sh
npm install iot-solutions -g
```

Weitere Informationen zur Befehlszeilenschnittstelle finden Sie unter [How to use the CLI](https://github.com/Azure/pcs-cli/blob/master/README.md) (Verwenden der Befehlszeilenschnittstelle).

### <a name="download-the-source-code"></a>Herunterladen des Quellcodes

 Das Quellcoderepository für die Remoteüberwachung umfasst die Docker-Konfigurationsdateien, die Sie benötigen, um die Docker-Images, die die Microservices enthalten, herunterzuladen, zu konfigurieren und auszuführen. Um eine lokale Version des Repositorys zu klonen und zu erstellen, navigieren Sie über Ihre bevorzugte Befehlszeile oder Ihr bevorzugtes Terminal zu einem geeigneten Ordner auf dem lokalen Computer, und führen Sie einen der folgenden Befehle aus:

Führen Sie zum Installieren der Java-Implementierungen der Microservices Folgendes aus:

```cmd/sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

Führen Sie zum Installieren der .Net-Implementierungen der Microservices Folgendes aus:

```cmd\sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

Vorkonfigurierte Lösung für die Remoteüberwachung – Repository und Submodule [ [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java) | [.Net](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) ]

> [!NOTE]
> Diese Befehle laden den Quellcode für alle Microservices herunter. Obwohl Sie den Quellcode nicht zum Ausführen der Microservices in Docker benötigen, ist der Quellcode hilfreich, wenn Sie später die vorkonfigurierte Lösung ändern und Ihre Änderungen lokal testen möchten.

## <a name="deploy-the-azure-services"></a>Bereitstellen von Azure-Diensten

In diesem Artikel erfahren Sie zwar, wie Sie die Microservices lokal ausführen, doch sie hängen von drei Azure-Diensten ab, die in der Cloud ausgeführt werden. Sie können diese Azure-Dienste [manuell über das Azure-Portal bereitstellen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup) oder die PCS-CLI verwenden. Dieser Artikel beschreibt, wie Sie das `pcs`-Tool verwenden.

### <a name="sign-in-to-the-cli"></a>Anmelden bei der Befehlszeilenschnittstelle

Damit Sie den Solution Accelerator bereitstellen können, müssen Sie sich wie folgt über die CLI bei Ihrem Azure-Abonnement anmelden:

```cmd/sh
pcs login
```

Befolgen Sie die Anweisungen auf dem Bildschirm, um den Anmeldevorgang abzuschließen. Achten Sie darauf, dass Sie nicht an einer beliebigen Stelle in die CLI klicken, da sonst bei der Anmeldung ein Fehler auftreten kann. Bei erfolgreicher Anmeldung wird eine entsprechende Meldung in der CLI angezeigt. 

### <a name="run-a-local-deployment"></a>Ausführen einer lokalen Bereitstellung

Verwenden Sie den folgenden Befehl zum Starten der lokalen Bereitstellung. Damit werden die erforderlichen Azure-Ressourcen erstellt und Umgebungsvariablen in der Konsole ausgegeben. 

```cmd/pcs
pcs -s local
```

Das Skript fordert Sie zur Eingabe der folgenden Informationen auf:

* Ein Lösungsname.
* Das zu verwendende Azure-Abonnement.
* Der Speicherort des zu verwendenden Azure-Rechenzentrums.

> [!NOTE]
> Das Skript erstellt eine IoT Hub-Instanz, eine Cosmos DB-Instanz und ein Azure-Speicherkonto in einer Ressourcengruppe in Ihrem Azure-Abonnement. Der Name der Ressourcengruppe ist der Name der Lösung, die Sie ausgewählt haben, als Sie oben das `pcs`-Tool ausgeführt haben. 

> [!IMPORTANT]
> Die Ausführung des Skripts dauert mehrere Minuten. Wenn der Vorgang abgeschlossen ist, sehen Sie eine Meldung `Copy the following environment variables to /scripts/local/.env file:`. Notieren Sie die Umgebungsvariablendefinitionen, die der Nachricht folgen, denn Sie werden sie in einem späteren Schritt verwenden.

## <a name="run-the-microservices-in-docker"></a>Ausführen der Microservices in Docker

Zum Ausführen der Microservices in Docker bearbeiten Sie zunächst die **scripts\\local\\.env**-Datei in der lokalen Kopie des Repositorys, das Sie in einem früheren Schritt geklont haben. Ersetzen Sie den gesamten Inhalt der Datei mit den Definitionen der Umgebungsvariablen, die Sie im letzten Schritt bei der Ausführung des `pcs`-Befehls notiert haben. Diese Umgebungsvariablen ermöglichen den Microservices im Docker-Container, eine Verbindung mit den Azure-Diensten herzustellen, die vom `pcs`-Tool erstellt wurden.

Navigieren Sie zum Ausführen des Solution Accelerators zum Ordner **scripts\local** in Ihrer Befehlszeilenumgebung, und führen Sie den folgenden Befehl aus:

```cmd\sh
docker-compose up
```

Wenn Sie diesen Befehl zum ersten Mal ausführen, lädt Docker die Microservice-Images vom Docker-Hub herunter, um die Container lokal zu erstellen. Bei nachfolgenden Ausführungen führt Docker die Container sofort aus.

Sie können zum Anzeigen der Protokolle aus dem Container eine separate Shell verwenden. Suchen Sie zuerst die Container-ID mithilfe des `docker ps -a`-Befehls. Verwenden Sie dann `docker logs {container-id} --tail 1000` zur Anzeige der letzten 1.000 Protokolleinträge für den angegebenen Container.

Navigieren Sie für den Zugriff auf das Dashboard der Remoteüberwachungslösung in Ihrem Browser zu [http://localhost:8080](http://localhost:8080).

## <a name="clean-up"></a>Bereinigen

Um unnötige Gebühren zu vermeiden, entfernen Sie nach Abschluss Ihrer Tests die Clouddienste aus Ihrem Azure-Abonnement. Am einfachsten entfernen Sie die Dienste, indem Sie zum [Azure-Portal](https://ms.portal.azure.com) navigieren und die vom `pcs`-Tool erstellte Ressourcengruppe löschen.

Entfernen Sie mit dem `docker-compose down --rmi all`-Befehl die Docker-Images, und geben Sie Speicherplatz auf dem lokalen Computer frei. Sie können auch die lokale Kopie des Remoteüberwachungsrepositorys löschen, die erstellt wurde, als Sie den Quellcode aus GitHub geklont haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Einrichten einer lokalen Entwicklungsumgebung
> * Konfigurieren des Solution Accelerators
> * Bereitstellen des Solution Accelerators
> * Anmelden beim Solution Accelerator

Nach Bereitstellung der Remoteüberwachungslösung können Sie sich als Nächstes [mit den Funktionen des Lösungsdashboards vertraut machen](quickstart-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->