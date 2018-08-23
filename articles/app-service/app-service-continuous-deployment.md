---
title: Continuous Deployment in Azure App Service | Microsoft Docs
description: Erfahren Sie, wie Sie die kontinuierliche Bereitstellung in Azure App Service aktivieren.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: cephalin;dariagrigoriu
ms.openlocfilehash: 4d3f1c66c6403720bf02c80af1d6833dc3cee3f1
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2018
ms.locfileid: "42140955"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Continuous Deployment in Azure App Service
Dieser Artikel veranschaulicht das Konfigurieren von Continuous Deployment für [Azure App Service](app-service-web-overview.md). App Service ermöglicht Continuous Deployment aus BitBucket, GitHub und [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) durch Pullen der neuesten Updates aus Ihrem vorhandenen Repository in einen dieser Dienste.

Informationen zum manuellen Konfigurieren von Continuous Deployment in einem Cloudrepository, das nicht im Azure-Portal aufgeführt wird, (etwa [GitLab](https://gitlab.com/)), finden Sie unter [Setting up continuous deployment using manual steps](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps) (Manuelles Einrichten von Continuous Deployment).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

Veröffentlichen Sie Ihr vorbereitetes Repository in einem der unterstützten Dienste. Weitere Informationen zum Veröffentlichen Ihres Projekts in einem dieser Dienste finden Sie unter [Erstellen eines Repositorys (GitHub)], [Erstellen eines Repositorys (BitBucket)] und [Erste Schritte mit VSTS].

## <a name="deploy-continuously-from-github"></a>Continuous Deployment aus GitHub

Um Continuous Deployment mit GitHub zu aktivieren, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer App Service-App-Seite.

Klicken Sie im linken Menü auf **Bereitstellungscenter** > **GitHub** > **Autorisieren**. Befolgen Sie die Eingabeaufforderungen zur Autorisierung. 

![](media/app-service-continuous-deployment/github-choose-source.png)

Die Autorisierung mit GitHub muss nur ein Mal erfolgen. Wenn Sie bereits autorisiert sind, klicken Sie einfach auf **Weiter**. Sie können das autorisierte GitHub-Konto ändern, indem Sie auf **Konto ändern** klicken.

![](media/app-service-continuous-deployment/github-continue.png)

Wählen Sie auf der Seite **Buildanbieter** den Buildanbieter aus, und klicken Sie auf **Weiter**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Option 1: Verwenden des App Service-Kudu-Buildservers

Wählen Sie auf der Seite **Konfigurieren** die Organisation, das Repository und den Branch aus, aus dem Continuous Deployment erfolgen soll. Klicken Sie auf **Weiter**, wenn Sie fertig sind.

### <a name="option-2-use-vsts-continuous-delivery"></a>Option 2: Verwenden von Continuous Delivery mit VSTS

> [!NOTE]
> Damit App Service die erforderlichen Build- und Releasedefinitionen in Ihrem VSTS-Konto erstellt, muss Ihr Azure-Konto die Rolle **Besitzer** in Ihrem Azure-Abonnement aufweisen.
>

Wählen Sie auf der Seite **Konfigurieren** im Abschnitt **Code** die Organisation, das Repository und den Branch aus, aus dem Continuous Deployment erfolgen soll. Klicken Sie auf **Weiter**, wenn Sie fertig sind.

Konfigurieren Sie auf der Seite **Konfigurieren** im Abschnitt **Build** ein neues VSTS-Konto, oder geben Sie ein vorhandenes Konto an. Klicken Sie auf **Weiter**, wenn Sie fertig sind.

> [!NOTE]
> Wenn Sie ein vorhandenes VSTS-Konto verwenden möchten, das nicht aufgeführt wird, müssen Sie das [VSTS-Konto mit Ihrem Azure-Abonnement verknüpfen](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

Wählen Sie auf der Seite **Test** aus, ob Auslastungstests aktiviert werden sollen, und klicken Sie dann auf **Weiter**.

Abhängig vom [Tarif](https://azure.microsoft.com/pricing/details/app-service/plans/) Ihres App Service-Plans wird ggf. auch eine Seite **Für Staging bereitstellen** angezeigt. Wählen Sie aus, ob [Bereitstellungsslots aktiviert werden sollen](web-sites-staged-publishing.md), und klicken Sie dann auf **Weiter**.

### <a name="finish-configuration"></a>Fertigstellen der Konfiguration

Überprüfen Sie auf der Seite **Zusammenfassung** die Optionen, und klicken Sie dann auf **Fertig stellen**.

Wenn die Konfiguration abgeschlossen ist, werden neue Commits im ausgewählten Repository mithilfe von Continuous Deployment in Ihrer App Service-App bereitgestellt.

![](media/app-service-continuous-deployment/github-finished.png)

## <a name="deploy-continuously-from-bitbucket"></a>Continuous Deployment aus BitBucket

Um Continuous Deployment mit BitBucket zu aktivieren, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer App Service-App-Seite.

Klicken Sie im linken Menü auf **Bereitstellungscenter** > **BitBucket** > **Autorisieren**. Befolgen Sie die Eingabeaufforderungen zur Autorisierung. 

![](media/app-service-continuous-deployment/bitbucket-choose-source.png)

Die Autorisierung mit BitBucket muss nur ein Mal erfolgen. Wenn Sie bereits autorisiert sind, klicken Sie einfach auf **Weiter**. Sie können das autorisierte BitBucket-Konto ändern, indem Sie auf **Konto ändern** klicken.

![](media/app-service-continuous-deployment/bitbucket-continue.png)

Wählen Sie auf der Seite **Konfigurieren** das Repository und den Branch aus, aus dem Continuous Deployment erfolgen soll. Klicken Sie auf **Weiter**, wenn Sie fertig sind.

Überprüfen Sie auf der Seite **Zusammenfassung** die Optionen, und klicken Sie dann auf **Fertig stellen**.

Wenn die Konfiguration abgeschlossen ist, werden neue Commits im ausgewählten Repository mithilfe von Continuous Deployment in Ihrer App Service-App bereitgestellt.

## <a name="deploy-continuously-from-vsts"></a>Continuous Deployment aus VSTS

Um Continuous Deployment mit VSTS zu aktivieren, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer App Service-App-Seite.

Klicken Sie im linken Menü auf **Bereitstellungscenter** > **VSTS** > **Weiter**. 

![](media/app-service-continuous-deployment/vsts-choose-source.png)

Wählen Sie auf der Seite **Buildanbieter** den Buildanbieter aus, und klicken Sie auf **Weiter**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Option 1: Verwenden des App Service-Kudu-Buildservers

Wählen Sie auf der Seite **Konfigurieren** das VSTS-Konto, das Projekt, das Repository und den Branch aus, aus dem Continuous Deployment erfolgen soll. Klicken Sie auf **Weiter**, wenn Sie fertig sind.

### <a name="option-2-use-vsts-continuous-delivery"></a>Option 2: Verwenden von Continuous Delivery mit VSTS

> [!NOTE]
> Damit App Service die erforderlichen Build- und Releasedefinitionen in Ihrem VSTS-Konto erstellt, muss Ihr Azure-Konto die Rolle **Besitzer** in Ihrem Azure-Abonnement aufweisen.
>

Wählen Sie auf der Seite **Konfigurieren** im Abschnitt **Code** das VSTS-Konto, das Projekt, das Repository und den Branch aus, aus dem Continuous Deployment erfolgen soll. Klicken Sie auf **Weiter**, wenn Sie fertig sind.

> [!NOTE]
> Wenn Sie ein vorhandenes VSTS-Konto verwenden möchten, das nicht aufgeführt wird, müssen Sie das [VSTS-Konto mit Ihrem Azure-Abonnement verknüpfen](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

Geben Sie auf der Seite **Konfigurieren** im Abschnitt **Build** das Sprachframework an, das VSTS zum Ausführen der Buildtasks für Ihr ausgewähltes Repository verwenden soll. Klicken Sie auf **Weiter**, wenn Sie fertig sind.

Wählen Sie auf der Seite **Test** aus, ob Auslastungstests aktiviert werden sollen, und klicken Sie dann auf **Weiter**.

Abhängig vom [Tarif](https://azure.microsoft.com/pricing/details/app-service/plans/) Ihres App Service-Plans wird ggf. auch eine Seite **Für Staging bereitstellen** angezeigt. Wählen Sie aus, ob [Bereitstellungsslots aktiviert werden sollen](web-sites-staged-publishing.md), und klicken Sie dann auf **Weiter**. 

### <a name="finish-configuration"></a>Fertigstellen der Konfiguration

Überprüfen Sie auf der Seite **Zusammenfassung** die Optionen, und klicken Sie dann auf **Fertig stellen**.

Wenn die Konfiguration abgeschlossen ist, werden neue Commits im ausgewählten Repository mithilfe von Continuous Deployment in Ihrer App Service-App bereitgestellt.

## <a name="disable-continuous-deployment"></a>Deaktivieren der fortlaufenden Bereitstellung

Um Continuous Deployment zu deaktivieren, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer App Service-App-Seite.

Klicken Sie im linken Menü auf **Bereitstellungscenter** > **GitHub** oder **VSTS** oder **BitBucket** > **Trennen**.

![](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Weitere Ressourcen

* [How to investigate common issues with continuous deployment](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Verwenden von PowerShell für Azure]
* [Git-Dokumentation]
* [Projekt Kudu](https://github.com/projectkudu/kudu/wiki)
* [Use Azure to automatically generate a CI/CD pipeline to deploy an ASP.NET 4 app (Verwenden von Azure zum automatischen Erstellen einer CI/CD-Pipeline zum Bereitstellen einer ASP.NET 4-App)](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

[Azure portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Verwenden von PowerShell für Azure]: /powershell/azureps-cmdlets-docs
[Git-Dokumentation]: http://git-scm.com/documentation

[Erstellen eines Repositorys (GitHub)]: https://help.github.com/articles/create-a-repo
[Erstellen eines Repositorys (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Erste Schritte mit VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
