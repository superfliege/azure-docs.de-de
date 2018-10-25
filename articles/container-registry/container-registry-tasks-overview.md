---
title: Automatisieren von Betriebssystem- und Frameworkpatches mit Azure Container Registry Tasks (ACR Tasks)
description: 'Einführung in ACR Tasks: eine Suite mit Features in Azure Container Registry für sichere, automatisierte Build- und Patchvorgänge für Containerimages in der Cloud.'
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 09/24/2018
ms.author: danlep
ms.openlocfilehash: 63affd4ad22d5246274ddfa3160d5675f702003f
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855761"
---
# <a name="automate-os-and-framework-patching-with-acr-tasks"></a>Automatisieren von Betriebssystem- und Frameworkpatches mit ACR Tasks

Container bieten neue Virtualisierungsmöglichkeiten und trennen Anwendungs- und Entwicklerabhängigkeiten von Infrastruktur- und Betriebsanforderungen. Ein Aspekt, der allerdings weiterhin behandelt werden muss, ist das Patchen dieser Anwendungsvirtualisierung.

## <a name="what-is-acr-tasks"></a>Was ist ACR Tasks?

**ACR Tasks** ist eine Suite von Funktionen in Azure Container Registry. Sie bietet cloudbasierte Containerimageerstellung für Linux, Windows und ARM. Außerdem kann sie [Betriebssystem- und Frameworkpatching](#automate-os-and-framework-patching) für Ihre Docker-Container automatisieren. ACR Tasks erweitert nicht nur Ihren „inneren“ Entwicklungszyklus für die Cloud mit bedarfsgesteuerten Containerimagebuilds, sondern ermöglicht auch automatisierte Builds beim Quellcodecommit oder wenn das Basisimage eines Containers aktualisiert wird. Mit Triggern für Basisimageaktualisierungen können Sie Ihren Workflow für Betriebssystem- und Anwendungspatches automatisieren und so die Sicherheit von Umgebungen gewährleisten sowie die Prinzipien unveränderlicher Container einhalten.

Es gibt vier Möglichkeiten zum Erstellen und Testen von Containerimages mit ACR Tasks:

* [Schnelltask](#quick-task): Bedarfsgesteuertes Erstellen und Pushen von Containerimages in Azure ohne lokale Docker Engine-Installation. Kurz: `docker build`, `docker push` in die Cloud. Buildvorgang aus lokalem Quellcode oder einem Git-Repository.
* [Buildvorgang beim Quellcodecommit](#automatic-build-on-source-code-commit): Lösen Sie automatisch einen Containerimage-Buildvorgang aus, wenn Code in ein Git-Repository committet wird.
* [Buildvorgang beim Aktualisieren des Basisimages](#automate-os-and-framework-patching): Lösen Sie einen Containerimage-Buildvorgang aus, wenn das Basisimage des jeweiligen Images aktualisiert wurde.
* [Mehrstufige Aufgaben](#multi-step-tasks-preview) (Vorschau): Definieren Sie mehrstufige Aufgaben, die Images erstellen, Container als Befehle ausführen und Images in eine Registrierung pushen. Dieses Vorschaufeature von ACR Tasks unterstützt die bedarfsgesteuerte Aufgabenausführung und parallele Imagebuilds, Tests und Pushvorgänge.

## <a name="quick-task"></a>Schnelltask

Der „interne“ Entwicklungszyklus, der iterative Prozess, der das Schreiben von Code sowie das Erstellen und Testen Ihrer Anwendung umfasst, bevor sie zur Quellcodeverwaltung committet wird, ist wirklich der Beginn der Lebenszyklusverwaltung des Containers.

Bevor Sie Ihre erste Zeile Code committen, kann das Feature [Schnelltask](container-registry-tutorial-quick-task.md) von ACR Tasks eine integrierte Entwicklungsumgebung bereitstellen, indem Ihre Containerimage-Buildvorgänge in Azure ausgelagert werden. Mit Schnelltasks können Sie Ihre automatisierten Builddefinitionen überprüfen und potenzielle Probleme abfangen, bevor Sie Ihren Code committen.

Der Befehl [az acr build][az-acr-build] der Azure CLI verwendet das vertraute `docker build`-Format. Er akzeptiert einen *Kontext* (Satz zu erstellender Dateien), sendet ihn an ACR Tasks und pusht das erstellte Image nach dessen Fertigstellung standardmäßig in die entsprechende Registrierung.

Die folgende Tabelle zeigt einige Beispiele von unterstützten Kontextspeicherorten für ACR Tasks:

| Kontextspeicherort | BESCHREIBUNG | Beispiel |
| ---------------- | ----------- | ------- |
| Lokales Dateisystem | Dateien in einem Verzeichnis auf dem lokalen Dateisystem. | `/home/user/projects/myapp` |
| GitHub-Masterbranch | Dateien im Masterbranch (oder einem anderen Standardbranch) eines GitHub-Repositorys.  | `https://github.com/gituser/myapp-repo.git` |
| GitHub-Branch | Bestimmter Branch eines GitHub-Repositorys.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| GitHub-Pull Request | Pull Request in einem GitHub-Repository. | `https://github.com/gituser/myapp-repo.git#pull/23/head` |
| GitHub-Unterordner | Dateien in einem Unterordner in einem GitHub-Repository. Das Beispiel zeigt die Kombination der Spezifikation aus Pull Request und Unterordner. | `https://github.com/gituser/myapp-repo.git#pull/24/head:myfolder` |
| Remotetarball | Dateien in einem komprimierten Archiv auf einem Remotewebserver. | `http://remoteserver/myapp.tar.gz` |

ACR Tasks ist als Grundtyp für den Containerlebenszyklus konzipiert. Sie können ACR Tasks also beispielsweise in Ihre CI/CD-Lösung integrieren. Wenn Sie [az login][az-login] mit einem [Dienstprinzipal][az-login-service-principal] ausführen, kann Ihre CI/CD-Lösung mithilfe von Befehlen vom Typ [az acr build][az-acr-build] Imagebuildvorgänge initiieren.

Informationen zur Verwendung der Schnelltasks finden Sie im ersten ACR Tasks-Tutorial: [Erstellen von Containerimages in der Cloud mit Azure Container Registry Tasks](container-registry-tutorial-quick-task.md).

## <a name="automatic-build-on-source-code-commit"></a>Automatische Erstellung nach dem Committen von Quellcode

Lösen Sie mithilfe von ACR Tasks automatisch einen Containerimage-Buildvorgang aus, wenn Code in ein Git-Repository committet wird. Buildaufgaben können mithilfe des Azure CLI-Befehls [az acr task][az-acr-task] konfiguriert werden und ermöglichen die Angabe eines Git-Repositorys und optional eines Branch und einer Dockerfile-Datei. Wenn Ihr Team Code in das Repository committet, löst ein von ACR Tasks erstellter Webhook einen Buildvorgang für das im Repository definierte Containerimage aus.

> [!IMPORTANT]
> Wenn Sie in der Vorschauversion bereits Aufgaben mit dem `az acr build-task`-Befehl erstellt haben, müssen diese Aufgaben mit dem Befehl [az acr task][az-acr-task] neu erstellt werden.

Informationen zum Auslösen von Buildvorgängen nach dem Committen von Quellcode finden Sie im zweiten ACR Tasks-Tutorial: [Automatisieren von Buildvorgängen für Containerimages mit Azure Container Registry Tasks](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>Automatisierung von Betriebssystem- und Frameworkpatching

Durch die Möglichkeit zur Erkennung von Basisimageaktualisierungen bietet ACR Tasks einen echten Mehrwert für Ihren Containererstellungsworkflow. Wenn das aktualisierte Basisimage in Ihre Registrierung gepusht wird, kann ACR Tasks automatisch alle darauf basierenden Anwendungsimages erstellen.

Containerimages lassen sich grob in *Basisimages* und *Anwendungsimages* unterteilen. Basisimages enthalten neben anderen Anpassungen in der Regel das Betriebssystem und die Anwendungsframeworks, auf denen Ihre Anwendung basiert. Diese Basisimages basieren in der Regel auf öffentlichen Upstreamimages (z. B. [Alpine Linux ][base-alpine], [Windows][base-windows], [.NET][base-dotnet] oder [Node.js][base-node]). Ein Basisimage kann mehreren Anwendungsimages zugrunde liegen.

Wenn ein Betriebssystem- oder App-Framework-Image durch die zuständige Upstream-Instanz aktualisiert wird (etwa mit einem wichtigen Sicherheitspatch für das Betriebssystem), müssen Sie auch Ihre Basisimages aktualisieren, um die Korrektur zu integrieren. Daraufhin müssen auch alle Anwendungsimages neu erstellt werden, um die Upstreamkorrekturen aus Ihrem Basisimage zu integrieren.

Da Basisimageabhängigkeiten bei der Erstellung eines Containerimages von ACR Tasks dynamisch erkannt werden, kann ACR Tasks auch erkennen, dass das Basisimage eines Anwendungsimages aktualisiert wurde. Mit einer einzelnen vorkonfigurierten [Buildaufgabe](container-registry-tutorial-base-image-update.md#create-a-task) kann ACR Tasks dann **automatisch alle Anwendungsimages neu erstellen**. Die automatische Erkennung und Neuerstellung von ACR Tasks spart Zeit, die ansonsten für die manuelle Nachverfolgung und Aktualisierung der einzelnen Anwendungsimages aufgewendet werden müsste, die auf Ihr aktualisiertes Basisimage verweisen.

Informationen zu Betriebssystem- und Frameworkpatches finden Sie im dritten ACR Tasks-Tutorial: [Automatisieren von Buildvorgängen für Images nach der Aktualisierung des Basisimages mit Azure Container Registry Tasks](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> Aktualisierungen von Basisimages lösen Builds nur dann aus, wenn sich das Basis- und das Anwendungsimage in der gleichen Azure-Containerregistrierung befinden oder das Basisimage in einem öffentlichen Docker Hub-Repository gespeichert ist.

## <a name="multi-step-tasks-preview"></a>Mehrstufige Aufgaben (Vorschau)

Mehrstufige Aufgaben, eine Vorschaufunktion von ACR Tasks, bieten eine schrittbasierte Aufgabendefinition und Ausführung für das Erstellen, Testen und Patchen von Containerimages in der Cloud. Aufgabenschritte definieren einzelne Containerimagebuild- und Pushvorgänge. Sie können auch die Ausführung eines oder mehrerer Container mit jedem Schritt definieren, wobei der Container als Ausführungsumgebung verwendet wird.

Beispielsweise können Sie eine mehrstufige Aufgaben erstellen, die die folgenden Vorgänge automatisiert:

1. Erstellen eines Webanwendungsimages
1. Ausführen des Webanwendungscontainers
1. Erstellen eines Webanwendungs-Testimages
1. Ausführen den Webanwendungs-Testcontainers, der Tests für den aktuell ausgeführten Anwendungscontainer durchführt
1. Wenn die Tests bestanden werden, Erstellen eines Helm-Diagramm-Archivpakets
1. Ausführen eines `helm upgrade` mithilfe des neuen Helm-Diagramm-Archivpakets

Mit mehrstufigen Aufgaben können Sie das Erstellen, Ausführen und Testen eines Images in mehrere individuelle Schritte aufteilen, wobei Abhängigkeiten zwischen den Schritten unterstützt werden. Mit mehrstufigen Aufgaben in ACR Tasks erhalten Sie eine bessere Kontrolle über die Workflows zur Erstellung von Images, Tests, Betriebssystem- und Frameworkpatches.

Weitere Informationen zu mehrstufigen Aufgaben finden Sie unter [Ausführen von mehrstufigen Build-, Test- und Patchaufgaben in ACR Tasks](container-registry-tasks-multi-step.md).

> [!IMPORTANT]
> Die Funktion „mehrstufige Aufgaben“ von ACR Tasks befindet sich zurzeit in der Vorschau. Wenn Sie Vorschauversionen nutzen möchten, müssen Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieser Funktion werden bis zur allgemeinen Verfügbarkeit (General Availability, GA) unter Umständen noch geändert.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die dreiteilige ACR Tasks-Tutorialreihe an, wenn Sie Ihre Containerimages in der Cloud erstellen möchten, um Betriebssystem- und Frameworkpatches zu automatisieren.

> [!div class="nextstepaction"]
> [Erstellen von Containerimages in der Cloud mit Azure Container Registry Tasks](container-registry-tutorial-quick-task.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-task]: /cli/azure/acr#az-acr-task
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli#log-in-with-a-service-principal

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png