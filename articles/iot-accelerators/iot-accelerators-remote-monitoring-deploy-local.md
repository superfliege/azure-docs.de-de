---
title: Lokales Bereitstellen der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In dieser Schrittanleitung wird gezeigt, wie der Solution Accelerator für die Remoteüberwachung zu Test- und Entwicklungszwecken auf Ihrem lokalen Computer bereitgestellt wird.
author: asdonald
manager: timlt
ms.author: asdonald
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/26/2018
ms.topic: conceptual
ms.openlocfilehash: 7007b1406dbcfab3af4700418ac2ce07b9e521c0
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407432"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>Lokales Bereitstellen des Solution Accelerators für die Remoteüberwachung

In diesem Artikel wird gezeigt, wie der Solution Accelerator für die Remoteüberwachung zu Test- und Entwicklungszwecken auf Ihrem lokalen Computer bereitgestellt wird. Der in diesem Artikel beschriebene Ansatz stellt die Microservices in einem lokalen Docker-Container bereit und nutzt IoT Hub-, Cosmos DB- und Azure Time Series Insights-Dienste in der Cloud. Wenn Sie erfahren möchten, wie der Solution Accelerator für die Remoteüberwachung in einer IDE auf Ihrem lokalen Computer ausgeführt wird, lesen Sie [Starten von Microservices in lokaler Umgebung](https://github.com/Azure/remote-monitoring-services-java/blob/master/docs/LOCAL_DEPLOYMENT.md) auf GitHub.

## <a name="prerequisites"></a>Voraussetzungen

Für die Bereitstellung der vom Solution Accelerator für die Remoteüberwachung benötigten Azure-Dienste benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](http://azure.microsoft.com/pricing/free-trial/).

Um die lokale Bereitstellung abzuschließen, müssen die folgenden Tools auf dem lokalen Entwicklungscomputer installiert sein:

* [Git-Client](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker Compose](https://docs.docker.com/compose/install/)
* [Node.js v8](https://nodejs.org/) – Diese Software ist eine Voraussetzung für die PCS-CLI, die von den Skripts zum Erstellen von Azure-Ressourcen genutzt wird. Verwenden Sie nicht Node.js v10.

> [!NOTE]
> Diese Tools sind auf vielen Plattformen einschließlich Windows, Linux und iOS verfügbar.

### <a name="download-the-source-code"></a>Herunterladen des Quellcodes

Das GitHub-Quellcoderepository für die Remoteüberwachung umfasst die Docker-Konfigurationsdateien, die Sie benötigen, um die Docker-Images, die die Microservices enthalten, herunterzuladen, zu konfigurieren und auszuführen. Um eine lokale Version des Repositorys zu klonen und zu erstellen, navigieren Sie über Ihre Befehlszeilenumgebung zu einem geeigneten Ordner auf dem lokalen Computer, und führen Sie einen der folgenden Befehle aus:

Um die neueste Version der Java-Microservice-Implementierungen herunterzuladen, führen Sie Folgendes aus:

```cmd/sh
git clone https://github.com/Azure/remote-monitoring-services-java.git
```

Um die neueste Version der .NET-Microservice-Implementierungen herunterzuladen, führen Sie Folgendes aus:

```cmd\sh
git clone https://github.com/Azure/remote-monitoring-services-dotnet.git
```

> [!NOTE]
> Diese Befehle laden den Quellcode für alle Microservices zusätzlich zu den Skripts herunter, mit denen Sie die Microservices lokal ausführen. Obwohl Sie den Quellcode zum Ausführen der Microservices in Docker nicht benötigen, ist er hilfreich, wenn Sie den Solution Accelerator später ändern und Ihre Änderungen lokal testen möchten.

## <a name="deploy-the-azure-services"></a>Bereitstellen von Azure-Diensten

In diesem Artikel erfahren Sie zwar, wie Sie die Microservices lokal ausführen, doch sie hängen von Azure-Diensten ab, die in der Cloud ausgeführt werden. Sie können diese Azure-Dienste [manuell über das Azure-Portal](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup) bereitstellen oder das dafür vorgesehene Skript verwenden. Bei den nachstehenden Skriptbeispielen wird davon ausgegangen, dass Sie das .NET-Repository auf einem Windows-Computer verwenden. Wenn Sie in einer anderen Umgebung arbeiten, passen Sie die Pfade, Dateierweiterungen und Pfadtrennzeichen entsprechend an. So verwenden Sie die bereitgestellten Skripts für folgende Aufgaben:

### <a name="create-new-azure-resources"></a>Erstellen neuer Azure-Ressourcen

Wenn Sie die erforderlichen Azure-Ressourcen noch nicht erstellt haben, führen Sie die folgenden Schritte aus:

1. Navigieren Sie in Ihrer Befehlszeilenumgebung zum Ordner **remote-monitoring-services-dotnet\scripts\local\launch** in Ihrer geklonten Kopie des Repositorys.

2. Führen Sie das Skript **start.cmd** aus, und folgen Sie den Anweisungen. Das Skript fordert Sie auf, sich bei Ihrem Azure-Konto anzumelden und das Skript neu zu starten. Anschließend fordert es Sie zur Eingabe der folgenden Informationen auf:
    * Ein Lösungsname.
    * Das zu verwendende Azure-Abonnement.
    * Der Speicherort des zu verwendenden Azure-Rechenzentrums.

    Das Skript erstellt die Ressourcengruppe in Azure mit dem Namen Ihrer Projektmappe. Diese Ressourcengruppe enthält die Azure-Ressourcen, die der Solution Accelerator verwendet.

3. Nachdem das Skript abgeschlossen wurde, zeigt es eine Liste von Umgebungsvariablen an. Folgen Sie den Anweisungen in der Befehlsausgabe, um diese Variablen in der Datei **remote-monitoring-services-dotnet\\scripts\\local\\.env** zu speichern.

### <a name="use-existing-azure-resources"></a>Verwenden vorhandener Azure-Ressourcen

Wenn Sie die erforderlichen Azure-Ressourcen bereits erstellt haben, bearbeiten Sie die Definitionen der Umgebungsvariablen in der Datei **remote-monitoring-services-dotnet\\scripts\\local\\.env** mit den erforderlichen Werten. Die **.env**-Datei enthält detaillierte Informationen zum Speicherort der erforderlichen Werte.

## <a name="run-the-microservices-in-docker"></a>Ausführen der Microservices in Docker

Die in den lokalen Docker-Containern ausgeführten Microservices müssen auf die in Azure ausgeführten Dienste zugreifen. Sie können die Internetkonnektivität Ihrer Docker-Umgebung mit dem folgenden Befehl testen, der einen kleinen Container startet und versucht, eine Internetadresse zu pingen:

```cmd/sh
docker run --rm -ti library/alpine ping google.com
```

Navigieren Sie zum Ausführen des Solution Accelerators zum Ordner **remote-monitoring-services-dotnet\\scripts\\local** in Ihrer Befehlszeilenumgebung, und führen Sie den folgenden Befehl aus:

```cmd\sh
docker-compose up
```

Wenn Sie diesen Befehl zum ersten Mal ausführen, lädt Docker die Microservice-Images vom Docker-Hub herunter, um die Container lokal zu erstellen. Bei nachfolgenden Ausführungen führt Docker die Container sofort aus.

Sie können zum Anzeigen der Protokolle aus dem Container eine separate Shell verwenden. Suchen Sie zuerst die Container-ID mithilfe des `docker ps -a`-Befehls. Verwenden Sie dann `docker logs {container-id} --tail 1000` zur Anzeige der letzten 1.000 Protokolleinträge für den angegebenen Container.

Navigieren Sie für den Zugriff auf das Dashboard der Remoteüberwachungslösung in Ihrem Browser zu [http://localhost:8080](http://localhost:8080).

## <a name="clean-up"></a>Bereinigen

Um unnötige Gebühren zu vermeiden, entfernen Sie nach Abschluss Ihrer Tests die Clouddienste aus Ihrem Azure-Abonnement. Am einfachsten entfernen Sie die Dienste, indem Sie zum [Azure-Portal](https://ms.portal.azure.com) navigieren und die Ressourcengruppe löschen, die bei der Ausführung des Skripts **start.cmd** erstellt wurde.

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
