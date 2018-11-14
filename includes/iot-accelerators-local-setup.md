---
title: Includedatei
description: Includedatei
services: iot-accelerators
author: avneet723
ms.service: iot-accelerators
ms.topic: include
ms.date: 10/29/2018
ms.author: avneet723
ms.custom: include file
ms.openlocfilehash: 900d75f826830ea7336044a892506d3bec546e30
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283911"
---
## <a name="download-the-source-code"></a>Herunterladen des Quellcodes

Die Quellcoderepositorys für die Remoteüberwachung umfassen die Docker-Konfigurationsdateien, die Sie zum Ausführen der Docker-Images für die Microservices benötigen.

Um eine lokale Version des Repositorys zu klonen und zu erstellen, navigieren Sie über Ihre Befehlszeilenumgebung zu einem geeigneten Ordner auf dem lokalen Computer. Führen Sie dann einen der folgenden Befehlssätze aus, um das .NET- oder das Java-Repository zu klonen:

Um die neueste Version der .NET-Microservice-Implementierungen herunterzuladen, führen Sie Folgendes aus:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-dotnet
git submodule foreach git pull origin master
```

Um die neueste Version der Java-Microservice-Implementierungen herunterzuladen, führen Sie Folgendes aus:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> Diese Befehle laden den Quellcode für alle Microservices zusätzlich zu den Skripts herunter, mit denen Sie die Microservices lokal ausführen. Obwohl Sie den Quellcode zum Ausführen der Microservices in Docker nicht benötigen, ist er hilfreich, wenn Sie den Solution Accelerator später ändern und Ihre Änderungen lokal testen möchten.

## <a name="deploy-the-azure-services"></a>Bereitstellen von Azure-Diensten

In diesem Artikel erfahren Sie zwar, wie Sie die Microservices lokal ausführen, doch sie hängen von Azure-Diensten ab, die in der Cloud ausgeführt werden. Stellen Sie die Azure-Dienste mit dem nachfolgenden Skript bereit. Bei den nachstehenden Skriptbeispielen wird davon ausgegangen, dass Sie das .NET-Repository auf einem Windows-Computer verwenden. Wenn Sie in einer anderen Umgebung arbeiten, passen Sie die Pfade, Dateierweiterungen und Pfadtrennzeichen entsprechend an.

### <a name="create-new-azure-resources"></a>Erstellen neuer Azure-Ressourcen

Wenn Sie die erforderlichen Azure-Ressourcen noch nicht erstellt haben, führen Sie die folgenden Schritte aus:

1. Navigieren Sie in Ihrer Befehlszeilenumgebung zum Ordner **\services\scripts\local\launch** in Ihrer geklonten Kopie des Repositorys.

1. Führen Sie die folgenden Befehle aus, um das CLI-Tool **pcs** zu installieren und sich bei Ihrem Azure-Konto anzumelden:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Führen Sie das Skript **start.cmd** aus. Das Skript fordert Sie zur Eingabe der folgenden Informationen auf:
    * Ein Lösungsname.
    * Das zu verwendende Azure-Abonnement.
    * Der Speicherort des zu verwendenden Azure-Rechenzentrums.

    Das Skript erstellt die Ressourcengruppe in Azure mit dem Namen Ihrer Projektmappe. Diese Ressourcengruppe enthält die Azure-Ressourcen, die der Solution Accelerator verwendet. Sie können diese Ressourcengruppe löschen, wenn Sie die entsprechenden Ressourcen nicht mehr benötigen.

    Mit dem Skript werden auf dem lokalen Computer zudem mehrere Umgebungsvariablen mit dem Präfix **PCS** hinzugefügt. Wenn Sie die Docker-Container lokal starten, werden die zugehörigen Konfigurationswerte aus diesen Umgebungsvariablen gelesen.

> [!TIP]
> Nachdem das Skript abgeschlossen wurde, zeigt es eine Liste von Umgebungsvariablen an. Wenn Sie diese Werte in der Datei **services\\scripts\\local\\.env** speichern, können Sie sie für zukünftige Bereitstellungen des Solution Accelerators verwenden. Beachten Sie, dass alle auf dem lokalen Computer festgelegten Umgebungsvariablen die Werte in der Datei **services\\scripts\\local\\.env** überschreiben, wenn Sie **docker-compose** ausführen.

### <a name="use-existing-azure-resources"></a>Verwenden vorhandener Azure-Ressourcen

Wenn Sie die erforderlichen Azure-Ressourcen bereits erstellt haben, erstellen Sie die entsprechenden Umgebungsvariablen auf dem lokalen Computer. Möglicherweise haben Sie diese Werte als Bestandteil Ihrer letzten Bereitstellung in der Datei **services\\scripts\\local\\.env** gespeichert. Beachten Sie, dass die auf dem lokalen Computer festgelegten Umgebungsvariablen die Werte in der Datei **services\\scripts\\local\\.env** überschreiben, wenn Sie **docker-compose** ausführen.