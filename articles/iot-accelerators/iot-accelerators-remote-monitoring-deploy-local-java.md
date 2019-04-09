---
title: Lokales Bereitstellen der Lösung für die Remoteüberwachung (über IntelliJ-IDE) – Azure | Microsoft-Dokumentation
description: In dieser Schrittanleitung wird gezeigt, wie der Solution Accelerator für die Remoteüberwachung zu Test- und Entwicklungszwecken mithilfe von IntelliJ auf Ihrem lokalen Computer bereitgestellt wird.
author: v-krghan
manager: dominicbetts
ms.author: v-krghan
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/24/2019
ms.topic: conceptual
ms.openlocfilehash: ccdb3e16ea60cf85ae28e533e3b2d9f473cc90c8
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316387"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---intellij"></a>Lokales Bereitstellen des Solution Accelerators für die Remoteüberwachung – IntelliJ

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

In diesem Artikel wird gezeigt, wie der Solution Accelerator für die Remoteüberwachung zu Test- und Entwicklungszwecken auf Ihrem lokalen Computer bereitgestellt wird. Sie erfahren, wie Sie die Microservices in IntelliJ ausführen. Eine lokale Microservices-Bereitstellung verwendet die folgenden Clouddienste: IoT Hub, Cosmos DB, Azure Stream Analytics und Azure Time Series Insights-Dienste in der Cloud.

Wenn Sie den Solution Accelerator für die Remoteüberwachung auf Ihrem lokalen Computer in Docker ausführen möchten, finden Sie entsprechende Informationen unter [Lokales Bereitstellen des Solution Accelerators für die Remoteüberwachung – Docker](iot-accelerators-remote-monitoring-deploy-local-docker.md).

## <a name="prerequisites"></a>Voraussetzungen

Für die Bereitstellung der vom Solution Accelerator für die Remoteüberwachung benötigten Azure-Dienste benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Einrichtung des Computers

Um die lokale Bereitstellung abzuschließen, müssen die folgenden Tools auf dem lokalen Entwicklungscomputer installiert sein:

* [Git-Client](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/)
* [IntelliJ-Plug-In Scala](https://plugins.jetbrains.com/plugin/1347-scala)
* [IntelliJ-Plug-In SBT](https://plugins.jetbrains.com/plugin/5007-sbt)
* [IntelliJ-Plug-In SBT Executor](https://plugins.jetbrains.com/plugin/7247-sbt-executor)
* [Nginx](https://nginx.org/en/download.html)
* [Node.js v8](https://nodejs.org/) – Diese Software ist eine Voraussetzung für die PCS-CLI, die von den Skripts zum Erstellen von Azure-Ressourcen genutzt wird. Verwenden Sie nicht Node.js v10.

> [!NOTE]
> Die IntelliJ-IDE ist für Windows und Mac verfügbar.

[!INCLUDE [iot-accelerators-local-setup-java](../../includes/iot-accelerators-local-setup-java.md)]

## <a name="run-the-microservices"></a>Ausführen der Microservices

In diesem Abschnitt führen Sie die Microservices für die Remoteüberwachung aus. Sie führen die Webbenutzeroberfläche nativ, die Dienste für Gerätesimulation, Authentifizierung und ASA-Manager in Docker und die Microservices in IntelliJ aus.

### <a name="run-the-device-simulation-service"></a>Ausführen des Gerätesimulationsdiensts

Öffnen Sie eine neues Eingabeaufforderungsfenster, um sicherzustellen, dass Sie Zugriff auf die Umgebungsvariablen haben, die in der vorherigen Sitzung mit dem Skript **start.cmd** festgelegt wurden.

Führen Sie den folgenden Befehl aus, um den Docker-Container für den Gerätesimulationsdienst zu starten. Der Dienst simuliert Geräte für die Lösung für die Remoteüberwachung.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="run-the-auth-service"></a>Ausführen des Authentifizierungsdiensts

Öffnen Sie ein neues Eingabeaufforderungsfenster, und führen Sie den folgenden Befehl aus, um den Docker-Container für den Authentifizierungsdienst zu starten. Der Dienst ermöglicht es, die Benutzer zu verwalten, die für den Zugriff auf Azure-IoT-Lösungen autorisiert sind.

```cmd
<path_to_cloned_repository>\services\auth\scripts\docker\run.cmd
```

### <a name="run-the-asa-manager-service"></a>Ausführen des ASA-Manager-Diensts

Öffnen Sie ein neues Eingabeaufforderungsfenster, und führen Sie den folgenden Befehl aus, um den Docker-Container für den ASA-Manager-Dienst zu starten. Der Dienst ermöglicht die Verwaltung von Azure Stream Analytics-Aufträgen (ASA), einschließlich des Festlegens der Konfiguration und des Startens, Beendens und Überwachens ihres Status.

```cmd
<path_to_cloned_repository>\services\asa-manager\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Bereitstellen aller anderen Microservices auf dem lokalen Computer

In den folgenden Schritten wird erläutert, wie Sie die Microservices für die Remoteüberwachung in IntelliJ ausführen:

#### <a name="import-project"></a>Importieren des Projekts

1. Starten der IntelliJ-IDE
1. Wählen Sie **Projekt importieren** und dann **azure-iot-pcs-remote-monitoring-java\services\build.sbt** aus.

#### <a name="create-run-configurations"></a>Erstellen von Laufzeitkonfigurationen

1. Wählen Sie **Ausführen > Konfigurationen bearbeiten** aus.
1. Wählen Sie **Neue Konfiguration hinzufügen > sbt task (SBT-Aufgabe)** aus. 
1. Geben Sie einen **Namen** und dann **Tasks** als Ausführung ein. 
1. Wählen Sie das **Arbeitsverzeichnis** basierend auf dem auszuführenden Dienst aus.
1. Klicken Sie auf **Anwenden > OK**, um Ihre Auswahl zu speichern.
1. Erstellen Sie die Laufzeitkonfiguration für die folgenden Dienste:
    * WebService (services\config)
    * WebService (services\device-telemetry)
    * WebService (services\iothub-manager)
    * WebService (services\storage-adapter)

Die folgende Abbildung zeigt als Beispiel das Hinzufügen einer Konfiguration für einen Dienst:

[![Add-Configuration](./media/deploy-locally-intelliJ/run-configurations.png)](./media/deploy-locally-intelliJ/run-configurations.png#lightbox)


#### <a name="create-compound-configuration"></a>Erstellen einer Kombinationskonfiguration

1. Wählen Sie zum gemeinsamen Ausführen aller Dienste **Neue Konfiguration hinzufügen > Kombination** aus.
1. Geben Sie einen **Namen** ein, und **fügen Sie SBT-Aufgaben hinzu**.
1. Klicken Sie auf **Anwenden > OK**, um Ihre Auswahl zu speichern.

Die folgende Abbildung zeigt als Beispiel das Hinzufügen aller SBT-Aufgaben zu einer einzigen Konfiguration:


[![Add-All-Services](./media/deploy-locally-intelliJ/all-services.png)](./media/deploy-locally-intelliJ/all-services.png#lightbox)



1. Klicken Sie auf **Ausführen**, um die Webdienste auf dem lokalen Computer zu erstellen und auszuführen.

Für jeden Webdienst werden eine Eingabeaufforderung und ein Webbrowserfenster geöffnet. An der Eingabeaufforderung wird die Ausgabe von dem ausgeführten Dienst angezeigt, und im Browserfenster können Sie den Status überwachen. Schließen Sie die Eingabeaufforderungen und Webseiten nicht, dadurch werden die Webdienste beendet.


Um auf den Status der Dienste zuzugreifen, können Sie zu den folgenden URLs navigieren:
* IoT-Hub-Manager: [http://localhost:9002/v1/status](http://localhost:9002/v1/status)
* Gerätetelemetrie: [http://localhost:9004/v1/status](http://localhost:9004/v1/status)
* Konfiguration: [http://localhost:9005/v1/status](http://localhost:9005/v1/status)
* Speicheradapter: [http://localhost:9022/v1/status](http://localhost:9022/v1/status)


### <a name="start-the-stream-analytics-job"></a>Starten des Stream Analytics-Auftrags

Führen Sie folgende Schritte aus, um den Stream Analytics-Auftrag zu starten:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
1. Navigieren Sie zu der für Ihre Projektmappe erstellten **Ressourcengruppe**. Der Name der Ressourcengruppe ist der Name der Projektmappe, den Sie beim Ausführen des Skripts **start.cmd** ausgewählt haben**.
1. Klicken Sie in der Liste der Ressourcen auf den **Stream Analytics-Auftrag**.
1. Klicken Sie auf der Seite **Übersicht** des Stream Analytics-Auftrags auf die Schaltfläche **Starten**. Klicken Sie dann auf **Starten**, um den Auftrag direkt zu starten.

### <a name="run-the-web-ui"></a>Ausführen der Webbenutzeroberfläche

In diesem Schritt starten Sie die Webbenutzeroberfläche. Öffnen Sie eine neues Eingabeaufforderungsfenster, um sicherzustellen, dass Sie Zugriff auf die Umgebungsvariablen haben, die mit dem Skript **start.cmd** festgelegt wurden. Navigieren Sie in der lokalen Kopie des Repositorys zum Ordner **webui**, und führen Sie die folgenden Befehle aus:

```cmd
npm install
npm start
```

Nach Abschluss des Starts wird im Browser die Seite **http:\//localhost:3000/dashboard** angezeigt. Die Fehler auf dieser Seite sind zu erwarten. Um die Anwendung ohne Fehler anzuzeigen, führen Sie den folgenden Schritt aus.

### <a name="configure-and-run-nginx"></a>Konfigurieren und Ausführen von NGINX

Richten Sie einen Reverseproxyserver ein, um die Webanwendung und die Microservices zu verknüpfen, die auf dem lokalen Computer ausgeführt werden:

* Kopieren Sie die Datei **nginx.conf** aus dem Ordner **webui\scripts\localhost** in Ihrer lokalen Kopie des Repositorys in das Installationsverzeichnis **nginx\conf**.
* Führen Sie **NGINX** aus.

Weitere Informationen zum Ausführen von **NGINX** finden Sie unter [nginx for Windows](https://nginx.org/en/docs/windows.html) (nginx für Windows).

### <a name="connect-to-the-dashboard"></a>Verbinden mit dem Dashboard

Navigieren Sie für den Zugriff auf das Dashboard der Remoteüberwachungslösung in Ihrem Browser zu „http:\//localhost:9000“.

## <a name="clean-up"></a>Bereinigen

Um unnötige Gebühren zu vermeiden, entfernen Sie nach Abschluss Ihrer Tests die Clouddienste aus Ihrem Azure-Abonnement. Navigieren Sie zum Entfernen der Dienste zum [Azure-Portal](https://ms.portal.azure.com), und löschen Sie die Ressourcengruppe, die mit dem Skript **start.cmd** erstellt wurde.

Sie können auch die lokale Kopie des Remoteüberwachungsrepositorys löschen, die erstellt wurde, als Sie den Quellcode aus GitHub geklont haben.

## <a name="next-steps"></a>Nächste Schritte

Nach Bereitstellung der Lösung für die Remoteüberwachung können Sie sich als Nächstes [mit den Funktionen des Lösungsdashboards vertraut machen](quickstart-remote-monitoring-deploy.md).
