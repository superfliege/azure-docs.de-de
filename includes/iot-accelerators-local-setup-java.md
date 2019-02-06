---
title: Includedatei
description: Includedatei
services: iot-accelerators
author: v-krghan
ms.service: iot-accelerators
ms.topic: include
ms.date: 01/25/2019
ms.author: v-krghan
ms.custom: include file
ms.openlocfilehash: 5909533c2153fe7c6697f516ce7568787b82bdab
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55105840"
---
## <a name="download-the-source-code"></a>Herunterladen des Quellcodes

Die Quellcoderepositorys für die Remoteüberwachung umfassen den Quellcode und die Docker-Konfigurationsdateien, die Sie zum Ausführen der Docker-Images für die Microservices benötigen.

Um eine lokale Version des Repositorys zu klonen und zu erstellen, navigieren Sie über Ihre Befehlszeilenumgebung zu einem geeigneten Ordner auf dem lokalen Computer. Führen Sie dann einen der folgenden Befehlssätze aus, um das Java-Repository zu klonen:

Um die neueste Version der Java-Microserviceimplementierungen herunterzuladen, führen Sie Folgendes aus:


```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> Diese Befehle laden den Quellcode für alle Microservices zusätzlich zu den Skripts herunter, mit denen Sie die Microservices lokal ausführen. Obwohl Sie den Quellcode zum Ausführen der Microservices in Docker nicht benötigen, ist er hilfreich, wenn Sie den Solution Accelerator später ändern und Ihre Änderungen lokal testen möchten.

## <a name="deploy-the-azure-services"></a>Bereitstellen von Azure-Diensten

In diesem Artikel erfahren Sie zwar, wie Sie die Microservices lokal ausführen, doch sie hängen von Azure-Diensten ab, die in der Cloud ausgeführt werden. Stellen Sie die Azure-Dienste mit dem nachfolgenden Skript bereit. Bei den nachstehenden Skriptbeispielen wird davon ausgegangen, dass Sie das Java-Repository auf einem Windows-Computer verwenden. Wenn Sie in einer anderen Umgebung arbeiten, passen Sie die Pfade, Dateierweiterungen und Pfadtrennzeichen entsprechend an.

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

    Mit dem Skript werden auf dem lokalen Computer zudem mehrere Umgebungsvariablen mit dem Präfix **PCS** hinzugefügt. Wenn Sie die Docker-Container oder Microserviceprojekte lokal starten, werden die zugehörigen Konfigurationswerte aus diesen Umgebungsvariablen gelesen.

    > [!TIP]
    > Nach Abschluss des Skripts werden außerdem die Umgebungsvariablen in der Datei **\<Ihr Stammordner\>\\.pcs\\\<Lösungsname\>.env** gespeichert. Sie können sie für zukünftige Bereitstellungen des Solution Accelerators verwenden. Beachten Sie, dass alle auf dem lokalen Computer festgelegten Umgebungsvariablen die Werte in der Datei **services\\scripts\\local\\.env** überschreiben, wenn Sie **docker-compose** ausführen.

1. Beenden Sie die Befehlszeilenumgebung.

### <a name="use-existing-azure-resources"></a>Verwenden vorhandener Azure-Ressourcen

Wenn Sie die erforderlichen Azure-Ressourcen bereits erstellt haben, erstellen Sie die entsprechenden Umgebungsvariablen auf dem lokalen Computer. Möglicherweise sind diese in der Datei **\<Ihr Stammordner\>\\.pcs\\\<Lösungsname\>.env** aus der Bereitstellung gespeichert. Beachten Sie, dass die auf dem lokalen Computer festgelegten Umgebungsvariablen die Werte in der Datei **services\\scripts\\local\\.env** überschreiben, wenn Sie **docker-compose** ausführen.