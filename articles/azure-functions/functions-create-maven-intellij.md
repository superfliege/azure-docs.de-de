---
title: Erstellen einer Azure-Funktionen-App mit Java und IntelliJ | Microsoft-Dokumentation
description: Anleitung zum Erstellen und Veröffentlichen einer einfachen über HTTP ausgelösten serverlosen App für Azure Functions mithilfe von Java und IntelliJ.
services: functions
documentationcenter: na
author: jeffhollan
manager: jpconnock
keywords: azure functions, funktionen, ereignisverarbeitung, compute, serverlose architektur, java
ms.service: functions
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: 317d8bf40ef152e5b7dae6406be29330feaaa8d3
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "43842475"
---
# <a name="create-your-first-function-with-java-and-intellij-preview"></a>Erstellen der ersten Funktion mit Java und IntelliJ (Vorschauversion)

> [!NOTE] 
> Java für Azure Functions ist zurzeit als Preview verfügbar.

In diesem Artikel erfahren Sie, wie Sie ein [serverloses](https://azure.microsoft.com/overview/serverless-computing/) Functions-Projekt mit IntelliJ IDEA und Apache Maven erstellen, wie Sie dieses Projekt in der integrierte Entwicklungsumgebung Ihrer Computers testen und debuggen sowie für Azure Functions bereitstellen können. 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Wenn Sie eine Funktionen-App mit Java und IntelliJ entwickeln möchten, muss Folgendes installiert sein:

-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), Version 8.
-  [Apache Maven](https://maven.apache.org), Version 3.0 oder höher
-  [IntelliJ IDEA](https://www.jetbrains.com/idea/download), Community oder Ultimate mit Maven-Tools
-  [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> Damit Sie diesen Schnellstart durchführen können, muss die Umgebungsvariable JAVA_HOME auf den Installationsspeicherort des JDK festgelegt sein.

Es wird dringend empfohlen, auch [Version 2 der Azure Functions Core Tools](functions-run-local.md#v2) zu installieren. Diese Tools bieten eine lokale Entwicklungsumgebung zum Schreiben, Ausführen und Debuggen von Azure Functions. 


## <a name="create-a-functions-project"></a>Erstellen eines Functions-Projekts

1. Klicken Sie in IntelliJ IDEA auf **Neues Projekt erstellen**.  
1. Klicken Sie auf **Maven**, um den Erstellvorgang darüber auszuführen.
1. Aktivieren Sie das Kontrollkästchen **Create from archetype** (Über Archetype erstellen), und fügen Sie einen Archetype (**Add Archetype**) für [azure-functions-archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype) hinzu.
    - GroupId: com.microsoft.azure
    - ArtifactId: azure-functions-archetype
    - Version: Verwenden Sie die neuste Version des [zentralen Repositorys](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![Über IntelliJ/Maven erstellen](media/functions-create-first-java-intellij/functions-create-intellij.png).  
1. Klicken Sie auf **Weiter**, und geben Sie die Details zum aktuellen Projekt an. Klicken Sie anschließend auf **Fertig stellen**.

Maven erstellt die Projektdateien in einem neuen Ordner mit dem Namen _artifactId_. Bei dem generierten Code im Projekt handelt es sich um eine einfache [durch HTTP ausgelöste](/azure/azure-functions/functions-bindings-http-webhook) Funktion, die den Körper der über HTTP ausgelösten Anforderung wiederholt.

## <a name="run-functions-locally-in-the-ide"></a>Lokales Ausführen von Funktionen in der IDE

> [!NOTE]
> [Version 2 der Azure Functions Core Tools](functions-run-local.md#v2) muss installiert sein, damit Funktionen lokal ausgeführt und gedebuggt werden können.

1. Importieren Sie Änderungen oder vergewissern Sie sich, dass [auto import](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html) (automatischer Import) aktiviert ist.
1. Öffnen Sie die Symbolleiste **Maven-Projekte**.
1. Doppelklicken Sie unter „Lifecycle“ auf **Packen**, um die Projektmappe zu packen, zu kompilieren, und erstellen Sie anschließend ein Zielverzeichnis.
1. Doppelklicken Sie unter „Plug-Ins“ > „azure-functions“ auf **azure-functions:run**, um die lokale Azure Functions-Laufzeit zu starten.  
  ![Maven-Symbolleiste für Azure Functions](media/functions-create-first-java-intellij/functions-intellij-java-maven-toolbar.png)  

Schließen Sie das Dialogfeld „Ausführen“, wenn Sie den Funktionstest beendet haben. Es kann nicht mehr als ein Funktionshost gleichzeitig aktiv sein und lokal ausgeführt werden.

### <a name="debug-the-function-in-intellij"></a>Debuggen der Funktion in IntelliJ
Um den Funktionshost im Debugmodus zu starten, fügen Sie **-DenableDebug** als Argument hinzu, wenn Sie Ihre Funktion ausführen. Sie können die Funktion mit der Befehlszeile im Terminal ausführen oder in den [Maven-Zielen](https://www.jetbrains.com/help/idea/maven-support.html#run_goal) konfigurieren. Dann öffnet der Funktionshost einen Debugport bei 5005. 

```
mvn azure-functions:run -DenableDebug
```

Wählen Sie zum Debuggen in IntelliJ im Menü **Run** (Ausführen) **Edit Configurations** (Konfigurationen bearbeiten) aus. Klicken Sie auf **+**, um ein **Remote**-Element hinzuzufügen. Geben Sie **Name** (Name) und **Settings** (Einstellungen) ein, und klicken Sie dann auf **OK** (OK), um die Konfiguration zu speichern. Klicken Sie nach dem Setup auf **Debug** (Debuggen) „Name der Remotekonfiguration“ oder drücken Sie **UMSCHALT+F9**, um das Debuggen zu starten.

![Debuggen von Funktionen in IntelliJ](media/functions-create-first-java-intellij/debug-configuration-intellij.PNG)

Beenden Sie anschließend den Debugger, und führen Sie den Prozess lokal aus. Es kann nicht mehr als ein Funktionshost gleichzeitig aktiv sein und lokal ausgeführt werden.

## <a name="deploy-the-function-to-azure"></a>Bereitstellen der Funktion in Azure

Bei dem Bereitstellungsprozess in Azure Functions werden die Anmeldeinformationen aus der Azure CLI verwendet. [Melden Sie sich mit der Azure CLI an](/cli/azure/authenticate-azure-cli?view=azure-cli-latest), bevor Sie die Eingabeaufforderung Ihres Computers weiterverwenden.

```azurecli
az login
```

Stellen Sie den Code in Ihrer neuen Funktionen-App mithilfe des Ziels für `azure-functions:deploy`-Maven bereit, oder klicken Sie auf die Option „azure-functions:deploy“ im Fenster „Maven-Projekte“.

```
mvn azure-functions:deploy
```

Wenn die Bereitstellung abgeschlossen ist, wird die URL angezeigt, mit der Sie auf Ihre Azure-Funktionen-App zugreifen können:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Entwickeln von Java-Funktionen finden Sie im [Azure Functions-Java-Entwicklerhandbuch](functions-reference-java.md).
- Fügen Sie mit `azure-functions:add` als Maven-Ziel zusätzliche Funktionen mit verschiedenen Triggern zu Ihrem Projekt hinzu.
