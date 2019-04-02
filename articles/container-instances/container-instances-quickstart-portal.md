---
title: 'Schnellstart: Bereitstellen von Docker-Containern in Azure Container Instances – Portal'
description: In diesem Schnellstart verwenden Sie das Azure-Portal, um schnell eine containerbasierte Web-App bereitzustellen, die in einer isolierten Azure-Containerinstanz ausgeführt wird.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: f4d232d4d6043ede3979db67e5cd35130d931bef
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369444"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-portal"></a>Schnellstart: Bereitstellen einer Containerinstanz in Azure mithilfe des Azure-Portals

Führen Sie mithilfe von Azure Container Instances serverlose Docker-Container schnell und einfach in Azure aus. Sie stellen eine Anwendung bedarfsgesteuert in einer Containerinstanz bereit, wenn Sie keine vollständige Containerorchestrierungsplattform wie Azure Kubernetes Service benötigen.

In diesem Schnellstart stellen Sie mithilfe des Azure-Portals einen isolierten Docker-Container bereit und machen seine Anwendung über einen vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) verfügbar. Nachdem Sie einige Einstellungen konfiguriert und den Container bereitgestellt haben, können Sie zur ausgeführten Anwendung navigieren:

![In Azure Container Instances bereitgestellte App im Browser][aci-portal-07]

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][azure-free-account] erstellen, bevor Sie beginnen.

## <a name="create-a-container-instance"></a>Erstellen einer Containerinstanz

Klicken Sie auf **Ressource erstellen** > **Container** > **Container Instances**.

![Beginnen Sie, eine neue Containerinstanz im Azure-Portal zu erstellen][aci-portal-01]

Geben Sie die folgenden Werte in die Textfelder **Containername**, **Containerimage** und **Ressourcengruppe** ein. Behalten Sie für die anderen Werte die Standardwerte bei, und klicken Sie auf **OK**.

* Containername: `mycontainer`
* Containerimage: `mcr.microsoft.com/azuredocs/aci-helloworld`
* Ressourcengruppe: **Neue erstellen** > `myResourceGroup`

![Konfigurieren grundlegender Einstellungen für eine neue Containerinstanz im Azure-Portal][aci-portal-03]

Behalten Sie für diesen Schnellstart die Standardeinstellung **Öffentlich** bei, um das öffentliche Microsoft-Image `aci-helloworld` bereitzustellen. Dieses Image verpackt eine kleine in Node.js geschriebene Web-App, die eine statische HTML-Seite bedient.

Geben Sie unter **Konfiguration** eine **DNS-Namensbezeichnung** für Ihren Container an. Der Name muss in der Azure-Region, in der Sie die Containerinstanz erstellen, eindeutig sein. Ihr Container ist öffentlich unter `<dns-name-label>.<region>.azurecontainer.io` erreichbar. Falls die Fehlermeldung „DNS-Namensbezeichnung ist nicht verfügbar.“ angezeigt wird, sollten Sie eine andere DNS-Namensbezeichnung verwenden.

Übernehmen Sie für die anderen Einstellungen unter **Konfiguration** die Standardwerte, und klicken Sie anschließend zum Überprüfen der Konfiguration auf **OK**.

![Konfigurieren einer neuen Containerinstanz im Azure-Portal][aci-portal-04]

Nach Abschluss der Überprüfung wird eine Zusammenfassung der Containereinstellungen angezeigt. Wählen Sie **OK** aus, um Ihre Anforderung zur Containerbereitstellung zu übermitteln.

![Zusammenfassung der Einstellungen für eine neue Containerinstanz im Azure-Portal][aci-portal-05]

Wenn die Bereitstellung startet, wird eine Benachrichtigung mit dem Hinweis angezeigt, dass die Bereitstellung durchgeführt wird. Eine weitere Benachrichtigung wird angezeigt, nachdem die Containergruppe bereitgestellt wurde.

![Status der Erstellung einer neuen Containerinstanz im Azure-Portal][aci-portal-08]

Öffnen Sie die Übersicht für die Containergruppe, indem Sie zu **Ressourcengruppen** > **myResourceGroup** > **mycontainer** navigieren. Beachten Sie den **FQDN** (vollqualifizierter Domänenname) der Containerinstanz sowie den **Status**.

![Übersicht über die Containergruppe im Azure-Portal][aci-portal-06]

Wenn der **Status** den Wert *Ausgeführt* erreicht hat, navigieren Sie in Ihrem Browser zum FQDN des Containers.

![Mit Azure Container Instances bereitgestellte App im Browser][aci-portal-07]

Glückwunsch! Mit wenigen Konfigurationsschritten haben Sie eine öffentlich zugängliche Anwendung in Azure Container Instances bereitgestellt.

## <a name="view-container-logs"></a>Anzeigen von Containerprotokollen

Die Protokolle für eine Containerinstanz sind hilfreich, wenn Sie Probleme mit dem Container oder mit der ausgeführten Anwendung behandeln.

Klicken Sie zum Anzeigen der Protokolle des Containers unter **Einstellungen** auf **Container** und dann auf **Protokolle**. Daraufhin sollte die HTTP GET-Anforderung angezeigt werden, die beim Betrachten der Anwendung in Ihrem Browser generiert wurde.

![Containerprotokolle im Azure-Portal][aci-portal-11]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Container nicht mehr benötigen, klicken Sie für die Containerinstanz *mycontainer* auf **Übersicht** und anschließend auf **Löschen**.

![Löschen der Containerinstanz im Azure-Portal][aci-portal-09]

Klicken Sie im Bestätigungsdialogfeld auf **Ja**.

![Löschen der Bestätigung einer Containerinstanz im Azure-Portal][aci-portal-10]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure-Containerinstanz aus einem öffentlichen Microsoft-Image erstellt. Fahren Sie mit dem Azure Container Instances-Tutorial fort, wenn Sie ein Containerimage erstellen und über eine private Azure-Containerregistrierung bereitstellen möchten.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png
[aci-portal-11]: ./media/container-instances-quickstart-portal/qs-portal-11.png

<!-- LINKS - External -->
[azure-free-account]: https://azure.microsoft.com/free/