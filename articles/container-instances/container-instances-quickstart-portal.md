---
title: 'Schnellstart: Erstellen Ihres ersten Azure Container Instances-Containers mit dem Azure-Portal'
description: In dieser Schnellstartanleitung stellen Sie mithilfe des Azure-Portals einen Windows-Container in Azure Container Instances bereit.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: quickstart
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 6aa6fb27b2aa7c8b9614e5812fadc629b1e185f8
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076237"
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Schnellstartanleitung: Erstellen Ihres ersten Containers in Azure Container Instances

Azure Container Instances erleichtert die Erstellung und Verwaltung von Docker-Containern in Azure, ohne dass Sie virtuelle Computer bereitstellen oder einen übergeordneten Dienst einführen müssen. In dieser Schnellstartanleitung erstellen Sie mithilfe des Azure-Portals einen Container in Azure und machen ihn mit einem vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) über das Internet verfügbar. Nach dem Konfigurieren einiger Einstellungen wird in Ihrem Browser Folgendes angezeigt:

![Mit Azure Container Instances bereitgestellte App im Browser][aci-portal-07]

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][azure-free-account] erstellen, bevor Sie beginnen.

## <a name="create-a-container-instance"></a>Erstellen einer Containerinstanz

Klicken Sie auf **Ressource erstellen** > **Container** > **Container Instances**.

![Beginnen Sie, eine neue Containerinstanz im Azure-Portal zu erstellen][aci-portal-01]

Geben Sie die folgenden Werte in die Textfelder **Containername**, **Containerimage** und **Ressourcengruppe** ein. Behalten Sie für die anderen Werte die Standardwerte bei, und klicken Sie auf **OK**.

* Containername: `mycontainer`
* Containerimage: `microsoft/aci-helloworld`
* Ressourcengruppe: `myResourceGroup`

![Konfigurieren grundlegender Einstellungen für eine neue Containerinstanz im Azure-Portal][aci-portal-03]

Sie können mit Azure Container Instances sowohl Windows- als auch Linux-Container erstellen. Übernehmen Sie für diese Schnellstartanleitung die Standardeinstellung **Linux**, um das Linux-basierte Image `microsoft/aci-helloworld` bereitzustellen.

Geben Sie unter **Konfiguration** eine **DNS-Namensbezeichnung** für Ihren Container an. Der Name muss innerhalb der Azure-Region, in der Sie die Containerinstanz erstellen, eindeutig sein. Ihr Container ist öffentlich unter `<dns-name-label>.<region>.azurecontainer.io` erreichbar.

Übernehmen Sie für die anderen Einstellungen unter **Konfiguration** die Standardwerte, und klicken Sie anschließend zum Überprüfen der Konfiguration auf **OK**.

![Konfigurieren einer neuen Containerinstanz im Azure-Portal][aci-portal-04]

Nach Abschluss der Überprüfung wird eine Zusammenfassung der Containereinstellungen angezeigt. Wählen Sie **OK** aus, um Ihre Anforderung zur Containerbereitstellung zu übermitteln.

![Zusammenfassung der Einstellungen für eine neue Containerinstanz im Azure-Portal][aci-portal-05]

Wenn die Bereitstellung beginnt, gibt eine Kachel auf dem Portaldashboard an, dass die Bereitstellung durchgeführt wird. Nach Abschluss der Bereitstellung zeigt die Kachel Ihre neue Containerinstanz an.

![Status der Erstellung einer neuen Containerinstanz im Azure-Portal][aci-portal-08]

Wählen Sie die Containerinstanz **mycontainer** aus, um die dazugehörigen Eigenschaften anzuzeigen. Beachten Sie den **FQDN** (vollqualifizierter Domänenname) der Containerinstanz sowie den **Status**.

![Übersicht über die Containergruppe im Azure-Portal][aci-portal-06]

Wenn der **Status** den Wert *Ausgeführt* erreicht hat, navigieren Sie in Ihrem Browser zum FQDN des Containers.

![Mit Azure Container Instances bereitgestellte App im Browser][aci-portal-07]

Glückwunsch! Mit wenigen Konfigurationsschritten haben Sie eine öffentlich zugängliche Anwendung in Azure Container Instances bereitgestellt.

## <a name="view-container-logs"></a>Anzeigen von Containerprotokollen

Die Protokolle für eine Containerinstanz sind hilfreich, wenn Sie Probleme mit dem Container oder mit der ausgeführten Anwendung behandeln.

Klicken Sie zum Anzeigen der Protokolle des Containers unter **EINSTELLUNGEN** auf **Container** > **Protokolle**. Daraufhin sollte die HTTP GET-Anforderung angezeigt werden, die beim Betrachten der Anwendung in Ihrem Browser generiert wurde.

![Containerprotokolle im Azure-Portal][aci-portal-11]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Container nicht mehr benötigen, klicken Sie für die Containerinstanz *mycontainer* auf **Übersicht** und anschließend auf **Löschen**.

![Löschen der Containerinstanz im Azure-Portal][aci-portal-09]

Klicken Sie im Bestätigungsdialogfeld auf **Ja**.

![Löschen der Bestätigung einer Containerinstanz im Azure-Portal][aci-portal-10]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure-Containerinstanz auf der Grundlage eines Images in der öffentlichen Docker Hub-Registrierung erstellt. Wenn Sie selbst ein Containerimage erstellen und über eine private Azure-Containerregistrierung in Azure Container Instances bereitstellen möchten, fahren Sie mit dem Azure Container Instances-Tutorial fort.

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