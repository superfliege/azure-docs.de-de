---
title: Problembehandlung | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
manager: douge
ms.openlocfilehash: 4dee39b56cf0f6494f6e79c70b85bbf711d33d65
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044593"
---
# <a name="troubleshooting-guide"></a>Handbuch zur Problembehandlung

Dieses Handbuch enthält Informationen über allgemeine Probleme, die bei Verwendung von Azure Dev Spaces auftreten können.

## <a name="error-service-cannot-be-started"></a>Fehler „Dienst kann nicht gestartet werden.“

Dieser Fehler kann angezeigt werden, wenn der Dienstcode nicht gestartet wird. Die Ursache liegt häufig im Benutzercode. Um weitere Diagnoseinformationen zu erhalten, nehmen Sie die folgenden Änderungen Ihrer Befehle und Einstellungen vor:

In der Befehlszeile:

1. Verwenden Sie bei Verwendung von _azds.exe_ die Befehlszeilenoption „--verbose“, und verwenden Sie die Befehlszeilenoption „--output“ zur Angabe des Ausgabeformats.
 
    ```cmd
    azds up --verbose --output json
    ```

In Visual Studio:

1. Öffnen Sie **Tools > Optionen**, und wählen Sie unter **Projekte und Projektmappen** die Option **Erstellen und ausführen** aus.
2. Ändern Sie die Einstellungen für **Ausführlichkeit der MSBuild-Projektbuildausgabe:** zu **Detailliert** oder **Diagnose**.

    ![Screenshot des Dialogfelds „Tools > Optionen“](media/common/VerbositySetting.PNG)

## <a name="error-required-tools-and-configurations-are-missing"></a>Fehler „Erforderliche Tools und Konfigurationen fehlen“

Dieser Fehler kann beim Starten von Visual Studio Code auftreten: „[Azure Dev Spaces] Erforderliche Tools und Konfigurationen zum Erstellen und Debuggen von ‚[Projektname]‘ fehlen.“
Der Fehler weist darauf hin, dass „azds.exe“ nicht in der PATH-Umgebungsvariablen angegeben ist, wie in Visual Studio Code zu sehen ist.

### <a name="try"></a>Versuchen Sie Folgendes:

Starten Sie Visual Studio Code über eine Eingabeaufforderung, wo die PATH-Umgebungsvariable ordnungsgemäß eingerichtet ist.

## <a name="error-azds-is-not-recognized-as-an-internal-or-external-command-operable-program-or-batch-file"></a>Fehler: „azds“ wird nicht als interner oder externer Befehl, ausführbares Programm oder Batchdatei erkannt.
 
Dieser Fehler kann angezeigt werden, wenn „azds.exe“ nicht installiert oder nicht ordnungsgemäß konfiguriert ist.

### <a name="try"></a>Versuchen Sie Folgendes:

1. Überprüfen Sie, ob „azds.exe“ in „%ProgramFiles%/Microsoft SDKs\Azure\Azure Dev Spaces CLI (Preview)“ vorhanden ist. Wenn ja, fügen Sie diesen Speicherort der PATH-Umgebungsvariablen hinzu.
2. Wenn „azds.exe“ nicht installiert ist, führen Sie den folgenden Befehl aus:

    ```cmd
    az aks use-dev-spaces -n <cluster-name> -g <resource-group>
    ```

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Fehler „Upstream-Verbindungsfehler oder Trennung/Reset vor Headern“
Dieser Fehler kann angezeigt werden, wenn Sie versuchen, auf den Dienst zuzugreifen, beispielsweise, wenn Sie in einem Browser zur URL des Diensts wechseln. 

### <a name="reason"></a>Grund 
Der Containerport ist nicht verfügbar. Dieses Problem kann auftreten, weil: 
* Erstellung und Bereitstellung des Containers sind noch nicht abgeschlossen. Das Problem kann vorkommen, wenn Sie `azds up` ausführen oder den Debugger starten, und dann versuchen, auf den Container zuzugreifen, bevor er erfolgreich bereitgestellt wurde.
* Die Portkonfiguration ist nicht konsistent über _Dockerfile_, Helm-Diagramm und sämtlichen Servercode hinweg, durch den ein Port geöffnet wird.

### <a name="try"></a>Versuchen Sie Folgendes:
1. Wenn der Container gerade erstellt/bereitgestellt wird, können Sie 2 bis 3 Sekunden warten und dann erneut versuchen, auf den Dienst zuzugreifen. 
1. Überprüfen Sie Ihre Portkonfiguration. Die angegebenen Portnummern müssen in den folgenden Ressourcen **identisch** sein:
    * **Dockerfile:** Festgelegt durch die `EXPOSE`-Anweisung.
    * **[Helm-Diagramm](https://docs.helm.sh):** Festgelegt durch die `externalPort`- und `internalPort`-Werte für einen Dienst (oft in einer `values.yml`-Datei),
    * Alle Ports, die im Anwendungscode geöffnet werden (z. B. in Node.js): `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Config-Datei nicht gefunden
Sie führen `azds up` aus und erhalten den folgenden Fehler: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Grund
Führen Sie `azds up` aus dem Stammverzeichnis des auszuführenden Codes aus, und initialisieren Sie den Codeordner für die Ausführung mit Azure Dev Spaces.

### <a name="try"></a>Versuchen Sie Folgendes:
1. Ändern Sie das aktuelle Verzeichnis in den Stammordner, der Dienstcode enthält. 
1. Wenn im Codeordner keine _azds.yaml_-Datei vorhanden ist, führen Sie `azds prep` aus, um Docker-, Kubernetes- und Azure Dev Spaces-Objekte zu generieren.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Fehler: „Das Pipe-Programm ‚azds‘ wurde unerwartet mit Code 126 beendet.“
Das Starten des VS Code-Debuggers kann in einigen Fällen zu diesem Fehler führen. Dies ist ein bekanntes Problem.

### <a name="try"></a>Versuchen Sie Folgendes:
1. Schließen Sie VS Code, und öffnen Sie es erneut.
2. Drücken Sie erneut F5.


## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>Debuggen des Fehlers „Der konfigurierte Debugtyp ‚coreclr‘ wird nicht unterstützt.“
Beim Ausführen des VS Code-Debuggers wird der folgende Fehler gemeldet: `Configured debug type 'coreclr' is not supported.`

### <a name="reason"></a>Grund
Auf Ihrem Entwicklungscomputer ist die VS Code-Erweiterung für Azure Dev Spaces nicht installiert.

### <a name="try"></a>Versuchen Sie Folgendes:
Installieren Sie die [VS Code-Erweiterung für Azure Dev Spaces](get-started-netcore.md).

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>Der Typ- oder Namespacename „MyLibrary“ konnte nicht gefunden werden

### <a name="reason"></a>Grund 
Der Buildkontext befindet sich standardmäßig auf Projekt-/Dienstebene, aus diesem Grund kann ein verwendetes Bibliotheksprojekt nicht gefunden werden.

### <a name="try"></a>Versuchen Sie Folgendes:
Erforderliche Maßnahmen:
1. Ändern Sie die _azds.yaml_-Datei, um den Buildkontext auf Projektmappenebene festzulegen.
2. Ändern Sie die _Dockerfile_- und _Dockerfile.develop_-Dateien so, dass sie relativ zum neuen Buildkontext ordnungsgemäß auf die _csproj_-Dateien verweisen.
3. Platzieren Sie neben der SLN-Datei eine _DOCKERIGNORE_-Datei, und ändern Sie sie nach Bedarf.

Ein Beispiel finden Sie unter https://github.com/sgreenmsft/buildcontextsample

## <a name="microsoftdevspacesregisteraction-authorization-error"></a>Autorisierungsfehler „Microsoft.DevSpaces/register/action“
Der folgende Fehler wird möglicherweise angezeigt, wenn Sie einen Azure Dev Space verwalten und in einem Azure-Abonnement arbeiten, für das Sie nicht über Zugriff als Besitzer oder Mitwirkender verfügen.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Grund
Für das ausgewählte Azure-Abonnement ist der `Microsoft.DevSpaces`-Namespace nicht registriert.

### <a name="try"></a>Versuchen Sie Folgendes:
Jemand, der als Besitzer oder Mitwirkender Zugriff auf das Azure-Abonnement hat, kann den folgenden Azure CLI-Befehl ausführen, um den `Microsoft.DevSpaces`-Namespace manuell zu registrieren:

```cmd
az provider register --namespace Microsoft.DevSpaces
```

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Azure Dev Spaces scheint meine vorhandene Dockerfile nicht zum Erstellen eines Containers zu verwenden 

### <a name="reason"></a>Grund
Azure Dev Spaces kann so konfiguriert werden, dass es auf eine bestimmte _Dockerfile_ in Ihrem Projekt verweist. Wenn es den Anschein hat, dass Azure Dev Spaces zum Erstellen Ihrer Container nicht die erwartete _Dockerfile_ verwendet, müssen Sie Azure Dev Spaces möglicherweise explizit mitteilen, wo sich die gewünschte Dockerfile befindet. 

### <a name="try"></a>Versuchen Sie Folgendes:
Öffnen Sie die _azds.yaml_-Datei, die von Azure Dev Spaces in Ihrem Projekt generiert wurde. Verwenden Sie die `configurations->develop->build->dockerfile`-Direktive, um auf die Dockerfile zu verweisen, die Sie verwenden möchten:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```