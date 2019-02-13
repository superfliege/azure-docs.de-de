---
title: Verwenden von CI/CD mit Azure Dev Spaces | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: DrEsteban
ms.author: stevenry
ms.date: 12/17/2018
ms.topic: article
manager: yuvalm
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, Container
ms.openlocfilehash: 0abe2902248c8203046cfe891d136ca7d5d0a75b
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2019
ms.locfileid: "55665971"
---
# <a name="use-cicd-with-azure-dev-spaces"></a>Verwenden von CI/CD mit Azure Dev Spaces

In diesem Artikel werden Sie durch die Einrichtung von Continuous Integration/Continuous Deployment (CI/CD) für den Azure Kubernetes Service (AKS) mit aktivierten Dev Spaces geführt. CI/CD für AKS ermöglicht die automatische Bereitstellung von App-Updates, wenn Code in Ihr Quellrepository übertragen wird, für den ein Commit ausgeführt wurde. Die Verwendung von CI/CD in Verbindung mit einem Dev Spaces-fähigen Cluster ist sinnvoll, da dadurch eine Baseline der Anwendung auf dem neuesten Stand gehalten werden kann, mit der das Team arbeiten kann.

![CI/CD-Beispieldiagramm](../media/common/ci-cd-simple.png)

Obwohl Sie in diesem Artikel mit Azure DevOps geführt werden, gelten dieselben Konzepte für CI/CD-Systeme wie Jenkins, TeamCity, usw.

## <a name="prerequisites"></a>Voraussetzungen
* [AKS-Cluster (Azure Kubernetes Service) mit aktivierten Azure Dev Spaces](../get-started-netcore.md)
* [Installierte Azure Dev Spaces-Befehlszeilenschnittstelle](upgrade-tools.md)
* [Azure DevOps-Organisation mit einem Projekt](https://docs.microsoft.com/azure/devops/user-guide/sign-up-invite-teammates?view=vsts)
* [Azure Container Registry (ACR)](../../container-registry/container-registry-get-started-azure-cli.md)
    * Verfügbare Details zum [Administratorkonto](../../container-registry/container-registry-authentication.md#admin-account) für Azure Container Registry
* [Autorisieren des AKS-Clusters zum Ausführen eines Pulls von Azure Container Registry](../../container-registry/container-registry-auth-aks.md)

## <a name="download-sample-code"></a>Beispielcode herunterladen
Erstellen Sie eine Fork des Beispielcode-GitHub-Repositorys, um Zeit zu sparen. Wechseln Sie zu https://github.com/Azure/dev-spaces, und wählen Sie **Fork** aus. Nachdem der Fork-Prozess abgeschlossen ist, **klonen** Sie Ihre Fork-Version des Repositorys lokal. Standardmäßig wird der _Masterbranch_ ausgecheckt, aber wir haben einige zeitsparende Änderungen in den Branch _azds_updates_ einbezogen, die auch während Ihres Forks hätten übertragen werden sollen. Der Branch _azds_updates_ enthält Updates, die Sie in den Tutorialabschnitten zu Dev Spaces manuell vornehmen müssen, sowie einige vorgefertigte YAML- und JSON-Dateien zur Optimierung der Bereitstellung des CI/CD-Systems. Sie können einen Befehl wie `git checkout -b azds_updates origin/azds_updates` verwenden, um den Branch _azds_updates_ in Ihrem lokalen Repository auszuchecken.

## <a name="dev-spaces-setup"></a>Dev Spaces-Setup
Erstellen Sie einen neuen Bereich namens _dev_ mit dem Befehl `azds space select`. Der Bereich _dev_ wird von Ihrer CI/CD-Pipeline verwendet, um Ihre Codeänderungen zu pushen. Er wird auch verwendet, um _untergeordnete Bereiche_ basierend auf _dev_ zu erstellen.

```cmd
azds space select -n dev
```

Wenn Sie zum Auswählen eines übergeordneten Entwicklungsbereichs aufgefordert werden, wählen Sie _\<Kein\>_ aus.

Der Bereich _dev_ enthält immer den neuesten Zustand des Repositorys, eine Baseline, sodass Entwickler _untergeordnete Bereiche_ aus _dev_ erstellen können, um ihre isolierten Änderungen im Kontext der größeren App zu testen. Dieses Konzept wird in den Tutorials zu Dev Spaces näher erläutert.

## <a name="creating-the-build-definition"></a>Erstellen der Builddefinition
Öffnen Sie Ihr Azure DevOps-Teamprojekt in einem Webbrowser, und navigieren Sie zum Abschnitt _Pipelines_. Klicken Sie zunächst in der oberen rechten Ecke der Azure DevOps-Website auf Ihr Profilfoto, öffnen Sie den Previewfunktionsbereich, und deaktivieren Sie die Option _Neue Benutzeroberfläche zum Erstellen einer YAML-Pipeline_:

![Öffnen des Previewfunktionsbereichs](../media/common/preview-feature-open.png)

Die zu deaktivierende Option:

![Option „Neue Benutzeroberfläche zum Erstellen einer YAML-Pipeline“](../media/common/yaml-pipeline-preview-feature.png)

> [!Note]
> Die Azure DevOps-Previewfunktion _Neue Benutzeroberfläche zum Erstellen einer YAML-Pipeline_ steht zu diesem Zeitpunkt in Konflikt mit der Erstellung vordefinierter Buildpipelines. Sie müssen sie vorerst deaktivieren, um unsere vordefinierte Buildpipeline bereitzustellen.

In den Branch _azds_updates_ haben wir eine einfache [Azure Pipeline YAML](https://docs.microsoft.com/azure/devops/pipelines/yaml-schema?view=vsts&tabs=schema) einbezogen, die die für *mywebapi* und *webfrontend* erforderlichen Buildschritte definiert.

Je nach gewählter Sprache wurde die Pipeline-YAML in einem ähnlichen Pfad wie dem folgenden eingecheckt: `samples/dotnetcore/getting-started/azure-pipelines.dotnetcore.yml`

So erstellen eine Pipeline aus dieser Datei
1. Navigieren Sie auf der Hauptseite Ihres DevOps-Projekts zu „Pipelines > Builds“.
1. Wählen Sie die Option zum Erstellen einer **neuen** Buildpipeline aus.
1. Wählen Sie **GitHub** als Quelle aus, autorisieren Sie sich bei Bedarf mit Ihrem GitHub-Konto, und wählen Sie den Branch _azds_updates_ aus Ihrer Fork-Version des Repositorys „dev-spaces sampleapp“ aus.
1. Wählen Sie **Konfiguration als Code** oder **YAML** als Vorlage aus.
1. Es wird Ihnen jetzt eine Konfigurationsseite für Ihre Buildpipeline angezeigt. Wie bereits erwähnt, geben Sie den sprachenabhängigen Pfad für den **YAML-Dateipfad** ein. Zum Beispiel, `samples/dotnetcore/getting-started/azure-pipelines.dotnetcore.yml`
1. Wechseln Sie zur Registerkarte „Variablen“.
1. Fügen Sie _dockerId_ manuell als Variable hinzu, wobei es sich um den Benutzernamen Ihres [Azure Container Registry Administratorkontos](../../container-registry/container-registry-authentication.md#admin-account) handelt. (In den Voraussetzungen zum Artikel erwähnt)
1. Fügen Sie _dockerPassword_ manuell als Variable hinzu, wobei es sich um das Kennwort Ihres [Azure Container Registry Administratorkontos](../../container-registry/container-registry-authentication.md#admin-account) handelt. Stellen Sie sicher, dass Sie _dockerPassword_ aus Sicherheitsgründen als Geheimnis (durch Auswahl des Schlosssymbols) angeben.
1. Wählen Sie **Speichern und in Warteschlange einreihen** aus.

Sie verfügen jetzt über eine CI-Lösung, die automatisch *mywebapi* und *webfrontend* für jedes Update erstellt, das per Push in den Branch _azds_updates_ Ihrer GitHub-Fork gesendet wird. Sie können überprüfen, ob die Docker-Images per Push übertragen wurden, indem Sie zum Azure-Portal navigieren, Ihre Azure Container Registry auswählen und die Registerkarte _Repositorys_ durchsuchen:

![Azure Container Registry-Repositorys](../media/common/ci-cd-images-verify.png)

## <a name="creating-the-release-definition"></a>Erstellen der Releasedefinition

1. Navigieren Sie auf der Hauptseite Ihres DevOps-Projekts zu „Pipelines > Releases“.
1. Wenn Sie an einem neuen DevOps-Projekt arbeiten, das noch keine Releasedefinition enthält, müssen Sie zuerst eine leere Releasedefinition erstellen, bevor Sie fortfahren können. Die Importoption wird auf der Benutzeroberfläche erst angezeigt, wenn Sie über eine bestehende Releasedefinition verfügen.
1. Klicken Sie links auf die Schaltfläche **+ Neu** und dann auf **Pipeline importieren**.
1. Wählen Sie die JSON-Datei unter `samples/release.json` aus.
1. Klicken Sie auf "OK". Beachten Sie, dass der Bereich „Pipeline“ mit der Bearbeitungsseite für die Releasedefinition geladen wurde. Beachten Sie außerdem, dass einige rote Warnsymbole angezeigt werden, die auf clusterspezifische Details hinweisen, die noch konfiguriert werden müssen.
1. Klicken Sie links im Bereich „Pipeline“ auf **Artefakt hinzufügen**.
1. Wählen Sie in der Dropdownliste **Quelle** die Buildpipeline aus, die zuvor in diesem Dokument erstellt wurde.
1. Für die **Standardversion** wird empfohlen, die Option **Neueste aus dem Standardbranch der Buildpipeline** auszuwählen. Sie müssen keine Tags angeben.
1. Legen Sie für **Quellalias** den Wert `drop` fest. Die vordefinierten Freigabetasks verwenden **Quellalias**, daher ist die Angabe erforderlich.
1. Klicken Sie auf **Hinzufügen**.
1. Klicken Sie jetzt auf das Blitzsymbol auf der neu erstellten `drop`-Artefaktquelle, wie unten gezeigt:

    ![Continuous Deployment-Setup für Releaseartefakt](../media/common/release-artifact-cd-setup.png)
1. Aktivieren Sie den **Continuous Deployment-Trigger**.
1. Wechseln Sie jetzt zum Taskbereich. Die Stufe _dev_ muss aktiviert sein, und es sollten wie unten drei rote Dropdownsteuerelemente angezeigt werden:

    ![Setup der Releasedefinition](../media/common/release-setup-tasks.png)
1. Geben Sie das Azure-Abonnement, die Ressourcengruppe und den Cluster an, den Sie mit Azure Dev Spaces verwenden.
1. An dieser Stelle sollte nur ein weiterer Abschnitt rot angezeigt werden: **Agentauftrag**. Wechseln Sie dorthin, und geben Sie **Hosted Ubuntu 1604** als Agentpool für diese Stufe an.
1. Bewegen Sie den Mauszeiger im oberen Bereich über die Aufgabenauswahl, und wählen Sie _prod_ aus.
1. Geben Sie das Azure-Abonnement, die Ressourcengruppe und den Cluster an, den Sie mit Azure Dev Spaces verwenden.
1. Konfigurieren Sie unter **Agentauftrag** die Option **Hosted Ubuntu 1604** als Agentpool.
1. Klicken Sie oben rechts auf **Speichern** und dann auf **OK**.
1. Klicken Sie auf **+ Release** (neben der Schaltfläche „Speichern“), und **erstellen Sie ein Release**.
1. Vergewissern Sie sich, dass der neueste Build aus Ihrer Buildpipeline im Abschnitt „Artefakte“ ausgewählt ist, und drücken Sie auf **Erstellen**.

Jetzt beginnt ein automatisierter Releaseprozess und stellt die Diagramme *mywebapi* und *webfrontend* in Ihrem Kubernetes-Cluster im obersten Bereich von _dev_ bereit. Sie können den Releasefortschritt auf dem Azure DevOps-Webportal verfolgen.

> [!TIP]
> Wenn Ihr Release mit einer Fehlermeldung wie *FEHLER BEIM UPGRADE: Timeout beim Warten auf die Bedingung* fehlschlägt, versuchen Sie, die Pods in Ihrem Cluster [ mit dem Kubernetes-Dashboard](../../aks/kubernetes-dashboard.md) zu überprüfen. Wenn Sie feststellen, dass die Pods nicht mit Fehlermeldungen wie *Fehler beim Pullen von Image "azdsexample.azurecr.io/mywebapi:122": RPC-Fehler: code = Unbekannt desc = Fehlerantwort von Daemon: Abrufen von https://azdsexample.azurecr.io/v2/mywebapi/manifests/122: nicht autorisiert: Authentifizierung erforderlich* beginnen können, kann es daran liegen, dass Ihr Cluster nicht die Berechtigung erhalten hat, einen Pull aus Ihrer Azure Container Registry auszuführen. Vergewissern Sie sich, dass Sie die Voraussetzung [Autorisieren des AKS-Clusters zum Ausführen eines Pulls von Azure Container Registry](../../container-registry/container-registry-auth-aks.md) abgeschlossen haben.

Sie verfügen jetzt über eine vollständig automatisierte CI/CD-Pipeline für Ihre GitHub-Fork der Dev Spaces-Beispielanwendungen. Jedes Mal, wenn Sie einen Commit und Push für Code ausführen, erstellt die Buildpipeline die Images *mywebapi* und *webfrontend* und verschiebt sie in Ihre benutzerdefinierte ACR-Instanz. Dann stellt die Releasepipeline das Helm-Diagramm für jede Anwendung im Bereich _dev_ auf Ihrem Dev Spaces-fähigen Cluster bereit.

## <a name="accessing-your-dev-services"></a>Zugreifen auf Ihre _dev_-Dienste
Nach der Bereitstellung kann auf die _dev_-Version von *webfrontend* mit einer öffentlichen URL wie `http://dev.webfrontend.<hash>.<region>.aksapp.io` zugegriffen werden.

Sie können diese URL über die *kubectl*-Befehlszeilenschnittstelle finden:
```cmd
kubectl get ingress -n dev webfrontend -o=jsonpath="{.spec.rules[0].host}"
```

## <a name="deploying-to-production"></a>Bereitstellen in der Produktionsumgebung
Klicken Sie in Ihrer Releasedefinition auf **Bearbeiten** und beachten Sie, dass eine _prod_-Stufe definiert ist:

![Produktionsstufe](../media/common/prod-env.png)

So stufen Sie ein bestimmtes Release mit dem in diesem Tutorial erstellten CI/CD-System manuell auf _prod_ hoch
1. Öffnen Sie ein zuvor erfolgreiches Release im DevOps-Portal.
1. Zeigen Sie auf die Stufe „prod“.
1. Wählen Sie „Bereitstellen“ aus.

![Höherstufen in die Produktionsumgebung](../media/common/prod-promote.png)

Unser Beispiel für eine CI/CD-Pipeline verwendet Variablen, um das DNS-Präfix für *webfrontend* je nach bereitgestellter Umgebung zu ändern. Sie können also eine URL wie die folgende verwenden, um auf Ihre „prod“-Dienste zuzugreifen: `http://prod.webfrontend.<hash>.<region>.aksapp.io`.

Nach der Bereitstellung können Sie diese URL mit der *kubectl*-Befehlszeilenschnittstelle finden: <!-- TODO update below to use list-uris when the product has been updated to list non-azds ingresses #769297 -->

```cmd
kubectl get ingress -n prod webfrontend -o=jsonpath="{.spec.rules[0].host}"
```

## <a name="dev-spaces-instrumentation-in-production"></a>Dev Spaces-Instrumentierung in der Produktionsumgebung
Obwohl die Instrumentierung von Dev Spaces _nicht_ so konzipiert wurde, dass sie den normalen Betrieb Ihrer Anwendung behindert, wird empfohlen, Ihre Produktionsworkloads in einem Kubernetes-Namespace auszuführen, der nicht mit Dev Spaces aktiviert ist. Die Verwendung dieser Art von Kubernetes-Namespace bedeutet, dass Sie entweder Ihren Produktionsnamespace mithilfe der `kubectl`-Befehlszeilenschnittstelle erstellen oder Ihrem CI/CD-System gestatten sollten, ihn während der ersten Helm-Bereitstellung zu erstellen. Wenn Sie einen Bereich mit den Dev Spaces-Tools _auswählen_ oder anderweitig erstellen, wird die Instrumentierung von Dev Spaces zu diesem Namespace hinzugefügt.

Hier folgt eine Beispielnamespacestruktur, die die Featureentwicklung, die Entwicklungsumgebung, _und_ die Produktionsumgebung in einem einzigen Kubernetes-Cluster unterstützt:

![Beispielnamespacestruktur](../media/common/cicd-namespaces.png)

> [!Tip]
> Wenn Sie bereits einen `prod`-Bereich erstellt haben und ihn einfach von der Dev Spaces-Instrumentierung ausschließen möchten (ohne ihn zu löschen), können Sie dies mit dem folgenden Befehl der Dev Spaces-Befehlszeilenschnittstelle erreichen:
>
> `azds space remove -n prod --no-delete`
>
> Möglicherweise müssen Sie danach alle Pods im Namespace `prod` löschen, damit sie ohne Dev Spaces-Instrumentierung wiederhergestellt werden können.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Teamentwicklung mit Azure Dev Spaces](../team-development-netcore.md)