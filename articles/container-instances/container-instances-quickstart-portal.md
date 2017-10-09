---
title: 'Schnellstart: Erstellen Ihres ersten Azure Container Instances-Containers mit dem Azure-Portal'
description: Bereitstellung und erste Schritte mit Azure Container Instances
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: marsma
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 16ae3d77c084651416cbc3bb1c5d28fee5e9124b
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="create-your-first-container-in-azure-container-instances"></a>Erstellen Ihres ersten Containers in Azure Container Instances

Mit Azure Container Instances können Sie ganz einfach Container in Azure erstellen und verwalten. In dieser Schnellstartanleitung erstellen Sie einen Container in Azure und machen ihn mit einer öffentlichen IP-Adresse über das Internet verfügbar. Dieser Vorgang wird mit dem Azure-Portal durchgeführt. Nach wenigen Klicks wird in Ihrem Browser Folgendes angezeigt:

![Mit Azure Container Instances bereitgestellte App im Browser][aci-app-browser]

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-a-container-instance"></a>Erstellen einer Containerinstanz

Wählen Sie **Neu** > **Container** > **Azure Container Instances (Vorschau)** aus.

![Beginnen Sie, eine neue Containerinstanz im Azure-Portal zu erstellen][aci-portal-01]

Geben Sie die folgenden Werte in die Textfelder **Containername**, **Containerimage** und **Ressourcengruppe** ein. Behalten Sie für die anderen Werte die Standardwerte bei, und klicken Sie auf **OK**.

* Containername: `mycontainer`
* Containerimage: `microsoft/aci-helloworld`
* Ressourcengruppe: `myResourceGroup`

![Konfigurieren grundlegender Einstellungen für eine neue Containerinstanz im Azure-Portal][aci-portal-03]

Sie können mit Azure Container Instances sowohl Windows- als auch Linux-Container erstellen. In dieser Schnellstartanleitung ändern wir die Standardeinstellung **Linux** nicht, da wir im vorherigen Schritt einen Linux-basierten Container (`microsoft/aci-helloworld`) angegeben haben.

Übernehmen Sie für die anderen Einstellungen unter **Konfiguration** die Standardwerte, klicken Sie dann zum Überprüfen der Konfiguration auf **OK**.

![Konfigurieren einer neuen Containerinstanz im Azure-Portal][aci-portal-04]

Wenn die Überprüfung abgeschlossen ist, wird eine Zusammenfassung der Containereinstellungen angezeigt. Wählen Sie **OK** aus, um Ihre Anforderung zur Containerbereitstellung zu übermitteln.

![Zusammenfassung der Einstellungen für eine neue Containerinstanz im Azure-Portal][aci-portal-05]

Wenn die Bereitstellung gestartet wurde, wird eine Kachel auf dem Portaldashboard hinzugefügt, die den Fortschritt der Bereitstellung angibt. Nachdem die Bereitstellung abgeschlossen wurde, wird die Kachel aktualisiert und zeigt die neue Containergruppe **mycontainer-myc1** an.

![Status der Erstellung einer neuen Containerinstanz im Azure-Portal][aci-portal-08]

Wählen Sie die Containergruppe **mycontainer-myc1** aus, um die Eigenschaften der Containergruppe anzuzeigen. Notieren Sie die **IP-Adresse** der Containergruppe, und beachten Sie den **STATUS** des Containers.

![Übersicht über die Containergruppe im Azure-Portal][aci-portal-06]

Sobald der Container in den Status **Wird ausgeführt** wechselt, navigieren Sie zu der IP-Adresse, die Sie im vorherigen Schritt notiert haben, um die im neuen Container gehostete Anwendung anzuzeigen.

![Mit Azure Container Instances bereitgestellte App im Browser][aci-app-browser]

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-02]: ./media/container-instances-quickstart-portal/qs-portal-02.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-app-browser]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure-Containerinstanz aus einem Image in einem öffentlichen Docker Hub-Repository erstellt. Wenn Sie selbst einen Container erstellen und über Azure Container Registry in Azure Container Instances bereitstellen möchten, fahren Sie mit dem Azure Container Instances-Tutorial fort.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial](./container-instances-tutorial-prepare-app.md)
