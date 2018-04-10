---
title: Erstellen Ihrer ersten Java-Web-App in Azure
description: Erfahren Sie, wie Sie Web-Apps in App Service ausführen, indem Sie eine einfache Java-App bereitstellen.
services: app-service\web
documentationcenter: ''
author: rmcmurray
manager: mbaldwin
editor: ''
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 03/26/2018
ms.author: cephalin;robmcm
ms.custom: mvc, devcenter
ms.openlocfilehash: 854ae54992a1389ec7c7f7892c738d070421264d
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="create-your-first-java-web-app-in-azure"></a>Erstellen Ihrer ersten Java-Web-App in Azure

[Azure-Web-Apps](app-service-web-overview.md) bietet einen hoch skalierbaren Webhostingdienst mit Self-Patching. Dieser Schnellstart veranschaulicht die Bereitstellung einer Java-Web-App in App Service über die [Eclipse-IDE für Java EE-Entwickler](http://www.eclipse.org/).

> [!NOTE]
>
> Die Schritte in dieser Schnellstartanleitung zeigen, wie Sie mithilfe der Eclipse-IDE eine Java-Web-App für App Service veröffentlichen. Sie können aber auch die Ultimate Edition oder die Community Edition von IntelliJ IDEA verwenden. Weitere Informationen finden Sie unter [Erstellen einer „Hello World“-Web-App für Azure mit IntelliJ](/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app).
>

Wenn Sie diesen Schnellstart abgeschlossen haben, entspricht Ihre Anwendung bei der Anzeige in einem Webbrowser etwa der folgenden Abbildung:

![„Hello Azure!“ Beispiel-Web-App](./media/app-service-web-get-started-java/browse-web-app-1.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Schnellstart müssen Sie Folgendes installieren:

* Die kostenlose <a href="http://www.eclipse.org/downloads/" target="_blank">Eclipse-IDE für Java EE-Entwickler</a>. In diesem Schnellstart wird Eclipse Neon verwendet.
* Das <a href="/java/azure/eclipse/azure-toolkit-for-eclipse-installation" target="_blank">Azure-Toolkit für Eclipse</a>.

> [!NOTE]
>
> Für die Schritte in dieser Schnellstartanleitung müssen Sie sich unter Verwendung des Azure-Toolkits für Eclipse bei Ihrem Azure-Konto anmelden. Eine entsprechende Anleitung finden Sie [hier](/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions).
>

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Erstellen eines dynamischen Webprojekts in Eclipse

Wählen Sie in Eclipse **File** (Datei) > **New** (Neu) > **Dynamic Web Project** (Dynamisches Webprojekt) aus.

Geben Sie dem Projekt im Dialogfeld **New Dynamic Web Project** (Neues dynamisches Webprojekt) den Namen **MyFirstJavaOnAzureWebApp**, und wählen Sie **Finish** (Fertig stellen) aus.
   
![Dialogfeld „New Dynamic Web Project“ (Neues dynamisches Webprojekt)](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a>Hinzufügen einer JSP-Seite

Wenn der Project Explorer nicht angezeigt wird, stellen Sie ihn wieder her.

![Java EE-Arbeitsbereich für Eclipse](./media/app-service-web-get-started-java/pe.png)

Erweitern Sie im Project Explorer das Projekt **MyFirstJavaOnAzureWebApp**.
Klicken Sie mit der rechten Maustaste auf **WebContent**, und wählen Sie dann **New** (Neu) > **JSP File** (JSP-Datei) aus.

![Menü für eine neue JSP-Datei im Project Explorer](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

Im Dialogfeld **New JSP File** (Neue JSP-Datei):

* Geben Sie der Datei den Namen **index.jsp**.
* Wählen Sie **Fertig stellen** aus.

  ![Dialogfeld „New JSP File“ (Neue JSP-Datei)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

Ersetzen Sie in der Datei „index.jsp“ das `<body></body>`-Element durch folgendes Markup:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Speichern Sie die Änderungen.

> [!NOTE]
>
> Sollte die erste Zeile einen Fehler enthalten, der auf eine fehlende Java-Servlet-Klasse verweist, können Sie diesen ignorieren.
> 
> ![Unbedenklicher Java-Servlet-Fehler](./media/app-service-web-get-started-java/java-servlet-benign-error.png)
>

## <a name="publish-the-web-app-to-azure"></a>Veröffentlichen der Web-App in Azure

Klicken Sie im Projektexplorer mit der rechten Maustaste auf Ihr Projekt, und wählen Sie dann **Azure** > **Publish as Azure Web App** (Als Azure-Web-App veröffentlichen) aus.

![Kontextmenü „Publish as Azure Web App“ (Als Azure-Web-App veröffentlichen)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

Sollte das**** Azure-Anmeldedialogfeld angezeigt werden, gehen Sie wie in der [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse-sign-in-instructions) beschrieben vor, um Ihre Anmeldeinformationen einzugeben.

### <a name="deploy-web-app-dialog-box"></a>Dialogfeld „Deploy Web App“ (Web-App bereitstellen)

Nachdem Sie sich bei Ihrem Azure-Konto angemeldet haben, wird das Dialogfeld **Deploy Web App** (Web-App bereitstellen) angezeigt.

Klicken Sie auf **Erstellen**.

![Dialogfeld „Deploy Web App“ (Web-App bereitstellen)](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a>Dialogfeld „App Service erstellen“

Das Dialogfeld **Create App Service** (App Service erstellen) wird mit Standardwerten angezeigt. In Ihrem Dialogfeld wird eine andere Nummer als die in der folgenden Abbildung aufgeführte Nummer **170602185241** angezeigt.

![Dialogfeld „App Service erstellen“](./media/app-service-web-get-started-java/cas1.png)

Im Dialogfeld **Create App Service** (App Service erstellen):

* Geben Sie einen eindeutigen Namen für Ihre Web-App ein, oder behalten Sie den generierten Namen bei. Dieser Name muss innerhalb von Azure eindeutig sein. Der Name ist Teil der URL-Adresse für die Web-App. Wenn die Web-App beispielsweise den Namen **MyJavaWebApp** trägt, lautet die URL *myjavawebapp.azurewebsites.net*.
* Behalten Sie für diese Schnellstartanleitung den Standardwebcontainer bei.
* Wählen Sie ein Azure-Abonnement aus.
* Auf der Registerkarte **App service plan** (App Service-Plan):

  * **Create new** (Neu erstellen): Behalten Sie den Standardwert, d.h. den Namen des App Service-Plans, bei.
  * **Location** (Standort): Wählen Sie **West Europe** (Europa, Westen) oder einen Standort in Ihrer Nähe aus.
  * **Pricing tier** (Tarif): Wählen Sie die Option „Free“ aus. Eine Beschreibung der Features finden Sie unter [App Service – Preise](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

   ![Dialogfeld „App Service erstellen“](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a>Registerkarte „Resource group“ (Ressourcengruppe)

Wählen Sie die Registerkarte **Resource group** (Ressourcengruppe) aus. Behalten Sie den standardmäßig generierten Wert für die Ressourcengruppe bei.

![Registerkarte „Resource group“ (Ressourcengruppe)](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Klicken Sie auf **Erstellen**.

<!--
### The JDK tab

Select the **JDK** tab. Keep the default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

Das Azure-Toolkit erstellt die Web-App, und ein Statusdialogfeld wird angezeigt.

![Dialogfeld „Create App Service Progress“ (Status der App Service-Erstellung)](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a>Dialogfeld „Deploy Web App“ (Web-App bereitstellen)

Wählen Sie im Dialogfeld **Deploy Web App** (Web-App bereitstellen) die Option **Deploy to root** (Im Stamm bereitstellen) aus. Wenn Sie eine App Service-Instanz unter *wingtiptoys.azurewebsites.net* verwenden und die Bereitstellung im Stamm nicht auswählen, wird die Web-App mit dem Namen **MyFirstJavaOnAzureWebApp** unter *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp* bereitgestellt.

![Dialogfeld „Deploy Web App“ (Web-App bereitstellen)](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

Im Dialogfeld wird die Auswahl für Azure, das JDK und den Webcontainer angezeigt.

Wählen Sie **Deploy** (Bereitstellen) aus, um die Web-App in Azure zu veröffentlichen.

Wählen Sie nach Abschluss der Veröffentlichung im Dialogfeld **Azure Activity Log** (Azure-Aktivitätsprotokoll) den Link **Published** (Veröffentlicht) aus.

![Dialogfeld „Azure Activity Log“ (Azure-Aktivitätsprotokoll)](./media/app-service-web-get-started-java/aal.png)

Glückwunsch! Sie haben Ihre Web-App erfolgreich in Azure bereitgestellt. 

![„Hello Azure!“ Beispiel-Web-App](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-the-web-app"></a>Aktualisieren der Web-App

Ersetzen Sie den JSP-Beispielcode durch einen anderen Text.

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

Speichern Sie die Änderungen.

Klicken Sie im Project Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Azure** > **Publish as Azure Web App** (Als Azure-Web-App veröffentlichen) aus.

Im dann geöffneten Dialogfeld **Deploy Web App** (Web-App bereitstellen) wird die zuvor erstellte App Service-Instanz angezeigt. 

> [!NOTE] 
> Wählen Sie bei jeder Veröffentlichung die Option **Deploy to root** (Im Stamm bereitstellen) aus. 
> 

Markieren Sie die Web-App, und wählen Sie **Deploy** (Bereitstellen) aus, um die Änderungen zu veröffentlichen.

Wenn der Link **Publishing** (Veröffentlichung) angezeigt wird, wählen Sie ihn aus, um zur Web-App zu navigieren und die Änderungen anzuzeigen.

## <a name="manage-the-web-app"></a>Verwalten der Web-App

Wechseln Sie zum <a href="https://portal.azure.com" target="_blank">Azure-Portal</a>, um die erstellte Web-App anzuzeigen.

Wählen Sie im linken Menü die Option **Ressourcengruppen** aus.

![Portalnavigation zu „Ressourcengruppen“](media/app-service-web-get-started-java/rg.png)

Wählen Sie die Ressourcengruppe aus. Auf der Seite werden die Ressourcen angezeigt, die Sie in diesem Schnellstart erstellt haben.

![Ressourcengruppe](media/app-service-web-get-started-java/rg2.png)

Wählen Sie die Web-App (**webapp-170602193915** in der vorherigen Abbildung) aus.

Die Seite **Übersicht** wird angezeigt. Diese Seite bietet eine Übersicht über den Status der App. Hier können Sie einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Starten, Neustarten und Löschen durchführen. Auf den Registerkarten links auf der Seite werden die verschiedenen Konfigurationen angezeigt, die Sie öffnen können. 

![App Service-Seite im Azure-Portal](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Zuordnen einer benutzerdefinierten Domäne](app-service-web-tutorial-custom-domain.md)
