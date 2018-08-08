---
title: Automatisieren von Betriebssystem- und Frameworkpatching mit Azure Container Registry Build (ACR Build)
description: 'Einführung in ACR Build: eine Suite mit Features in Azure Container Registry für sichere, automatisierte Build- und Patchingvorgänge für Containerimages in der Cloud.'
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 08/01/2018
ms.author: marsma
ms.openlocfilehash: 63bbd9b5711330207c34ac4aa05aac3a71304653
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413578"
---
# <a name="automate-os-and-framework-patching-with-acr-build"></a>Automatisieren von Betriebssystem- und Frameworkpatching mit ACR Build

Container bieten neue Virtualisierungsmöglichkeiten und trennen Anwendungs- und Entwicklerabhängigkeiten von Infrastruktur- und Betriebsanforderungen. Ein Aspekt, der allerdings weiterhin behandelt werden muss, ist das Patchen dieser Anwendungsvirtualisierung.

**ACR Build** ist eine Suite von Funktionen in Azure Container Registry. Sie bietet cloudbasierte Containerimageerstellung für Linux, Windows und ARM. Außerdem kann sie [Betriebssystem- und Frameworkpatching](#automate-os-and-framework-patching) für Ihre Docker-Container automatisieren.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

## <a name="what-is-acr-build"></a>Was ist ACR Build?

Azure Container Registry Build ist ein nativer Azure-Dienst für die Erstellung von Containerimages. ACR Build ermöglicht die cloudbasierte Entwicklung für die innere Schleife mit bedarfsgesteuerten Buildvorgängen für Containerimages und automatisierten Buildvorgängen nach dem Committen von Quellcode oder dem Aktualisieren eines Basisimages.

Lösen Sie Containerimage-Buildvorgänge automatisch aus, wenn Code in einem Git-Repository committet oder das Basisimage eines Containers aktualisiert wird. Mit Triggern für Basisimageaktualisierungen können Sie Ihren Workflow für Betriebssystem- und Anwendungspatches automatisieren und so die Sicherheit von Umgebungen gewährleisten sowie die Prinzipien unveränderlicher Container einhalten.

## <a name="quick-build-inner-loop-extended-to-the-cloud"></a>Schnellerstellung: Erweiterung der inneren Schleife auf die Cloud

Die Lebenszyklusverwaltung beginnt bereits, bevor Entwickler ihre ersten Codezeilen committen. Das [Schnellerstellungsfeature](container-registry-tutorial-quick-build.md) von ACR Build bietet eine integrierte, lokale Entwicklungsumgebung für die innere Schleife, durch die Buildvorgänge an Azure ausgelagert werden. Mit der Schnellerstellung können Sie Ihre automatisierten Builddefinitionen überprüfen, bevor Sie Ihren Code committen.

Der Befehl [az acr build][az-acr-build] der Azure CLI verwendet das vertraute `docker build`-Format. Er akzeptiert einen **Kontext** (Satz zu erstellender Dateien), sendet ihn an den ACR Build-Dienst und pusht das erstellte Image nach dessen Fertigstellung standardmäßig an die entsprechende Registrierung.

Die folgende Tabelle zeigt einige Beispiele von unterstützten Kontextspeicherorten für ACR Build:

| Kontextspeicherort | BESCHREIBUNG | Beispiel |
| ---------------- | ----------- | ------- |
| Lokales Dateisystem | Dateien in einem Verzeichnis auf dem lokalen Dateisystem. | `/home/user/projects/myapp` |
| GitHub-Masterbranch | Dateien im Masterbranch (oder einem anderen Standardbranch) eines GitHub-Repositorys.  | `https://github.com/gituser/myapp-repo.git` |
| GitHub-Branch | Bestimmter Branch eines GitHub-Repositorys.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| GitHub-Pull Request | Pull Request in einem GitHub-Repository. | `https://github.com/gituser/myapp-repo.git#pull/23/head` |
| GitHub-Unterordner | Dateien in einem Unterordner in einem GitHub-Repository. Das Beispiel zeigt die Kombination der Spezifikation aus Pull Request und Unterordner. | `https://github.com/gituser/myapp-repo.git#pull/24/head:myfolder` |
| Remotetarball | Dateien in einem komprimierten Archiv auf einem Remotewebserver. | `http://remoteserver/myapp.tar.gz` |

ACR Build berücksichtigt auch Ihre georeplizierten Registrierungen und ermöglicht verteilten Entwicklungsteams dadurch die Nutzung der nächstgelegenen replizierten Registrierung.

ACR Build ist als Grundtyp für den Containerlebenszyklus konzipiert. Sie können ACR Build also beispielsweise in Ihre CI/CD-Lösung integrieren. Wenn Sie [az login][az-login] mit einem [Dienstprinzipal][az-login-service-principal] ausführen, kann Ihre CI/CD-Lösung mithilfe von Befehlen vom Typ [az acr build][az-acr-build] Imagebuildvorgänge initiieren.

Informationen zur Verwendung der Schnellerstellung finden Sie im ersten ACR Build-Tutorial: [Erstellen von Containerimages in der Cloud mit Azure Container Registry Build](container-registry-tutorial-quick-build.md).

## <a name="automatic-build-on-source-code-commit"></a>Automatische Erstellung nach dem Committen von Quellcode

Lösen Sie mithilfe von ACR Build automatisch einen Containerimage-Buildvorgang aus, wenn Code in einem Git-Repository committet wird. Buildaufgaben können mithilfe des Azure CLI-Befehls [az acr build-task][az-acr-build-task] konfiguriert werden und ermöglichen die Angabe eines Git-Repositorys und optional einer Verzweigung und einer Dockerfile. Wenn Ihr Team Code im Repository committet, löst ein von ACR Build erstellter Webhook einen Buildvorgang für das im Repository definierte Containerimage aus.

Informationen zum Auslösen von Buildvorgängen nach dem Committen von Quellcode finden Sie im zweiten ACR Build-Tutorial: [Automatisieren von Buildvorgängen für Containerimages mit Azure Container Registry Build](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>Automatisierung von Betriebssystem- und Frameworkpatching

Durch die Möglichkeit zur Erkennung von Basisimageaktualisierungen bietet ACR Build einen echten Mehrwert für Ihre Containererstellungspipeline. Wenn das aktualisierte Basisimage an Ihre Registrierung gepusht wird, kann ACR Build automatisch alle darauf basierenden Anwendungsimages erstellen.

Containerimages lassen sich grob in *Basisimages* und *Anwendungsimages* unterteilen. Basisimages enthalten neben anderen Anpassungen in der Regel das Betriebssystem und die Anwendungsframeworks, auf denen Ihre Anwendung basiert. Diese Basisimages basieren in der Regel auf öffentlichen Upstreamimages (z. B. [Alpine Linux ][base-alpine], [Windows][base-windows], [.NET][base-dotnet] oder [Node.js][base-node]). Ein Basisimage kann mehreren Anwendungsimages zugrunde liegen.

Wenn ein Betriebssystem- oder App-Framework-Image durch die zuständige Upstream-Instanz aktualisiert wird (etwa mit einem wichtigen Sicherheitspatch für das Betriebssystem), müssen Sie auch Ihre Basisimages aktualisieren, um die Korrektur zu integrieren. Daraufhin müssen auch alle Anwendungsimages neu erstellt werden, um die Upstreamkorrekturen aus Ihrem Basisimage zu integrieren.

Da Basisimageabhängigkeiten bei der Erstellung eines Containerimages von ACR Build dynamisch erkannt werden, kann ACR Build auch erkennen, dass das Basisimage eines Anwendungsimages aktualisiert wurde. Mit einer einzelnen vorkonfigurierten [Buildaufgabe](container-registry-tutorial-base-image-update.md#create-build-task) kann ACR Build dann **automatisch alle Anwendungsimages neu erstellen**. Die automatische Erkennung und Neuerstellung von ACR Build spart Zeit, die ansonsten für die manuelle Nachverfolgung und Aktualisierung der einzelnen Anwendungsimages aufgewendet werden müsste, die auf Ihr aktualisiertes Basisimage verweisen.

Informationen zum Betriebssystem- und Frameworkpatching finden Sie im dritten ACR Build-Tutorial: [Automatisieren von Buildvorgängen für Images nach der Aktualisierung des Basisimages mit Azure Container Registry Build](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> In der ersten Vorschauversion müssen sich Basisimages und Anwendungsimages in der gleichen Azure-Containerregistrierung oder dem gleichen öffentlichen Docker Hub-Repository befinden, damit nach der Aktualisierung eines Basisimages ein Buildvorgang ausgelöst wird.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die dreiteilige ACR Build-Tutorialreihe an, wenn Sie Ihre Containerimages in der Cloud erstellen möchten, um das Betriebssystem- und Frameworkpatching zu automatisieren.

> [!div class="nextstepaction"]
> [Erstellen von Containerimages in der Cloud mit Azure Container Registry Build](container-registry-tutorial-quick-build.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-build-task]: /cli/azure/acr#az-acr-build-task
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli#log-in-with-a-service-principal

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
