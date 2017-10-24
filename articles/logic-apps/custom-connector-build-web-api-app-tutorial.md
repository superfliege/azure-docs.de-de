---
title: "Erstellen benutzerdefinierter Connectors aus Web-APIs – Azure Logic Apps | Microsoft-Dokumentation"
description: Informationen zum Erstellen benutzerdefinierter Connectors aus Web-APIs
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 22bf335718721d29933557142b436e75778f8c86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-custom-connectors-from-web-apis"></a>Erstellen benutzerdefinierter Connectors aus Web-APIs

Um einen benutzerdefinierten Connector zu erstellen, den Sie in Azure Logic Apps, Microsoft Flow oder Microsoft PowerApps verwenden können, erstellen Sie zunächst eine Web-API, die Sie mit Azure Web Apps hosten können. Authentifizieren Sie sie mit Azure Active Directory, und registrieren Sie sie als Connector bei Logic Apps, Flow oder PowerApps. In diesem Tutorial wird gezeigt, wie Sie diese Aufgaben durch Erstellen einer ASP.NET-Web-API-App erledigen. Weitere Informationen zu Mustern, die zeigen, dass Sie Code für die Trigger und Aktionen Ihres Connectors strukturieren können, finden Sie unter [Erstellen benutzerdefinierter APIs, die Sie aus Logik-App-Workflows aufrufen können](../logic-apps/logic-apps-create-api-app.md).

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2013 oder höher](https://www.visualstudio.com/vs/). In diesem Tutorial wird Visual Studio 2015 verwendet.

* Code für Ihre Web-API. Falls Sie noch keinen haben, sehen Sie sich dieses Tutorial an: [Erste Schritte mit ASP.NET-Web-API 2 (C#)](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).

* Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie [mit einem kostenlosen Azure-Konto beginnen](https://azure.microsoft.com/free/). Registrieren Sie andernfalls [für ein Abonnement mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/pricing/purchase-options/).

## <a name="create-and-deploy-an-aspnet-web-app-to-azure"></a>Erstellen und Bereitstellen einer ASP.NET-Web-App in Azure

Erstellen Sie für dieses Tutorial eine Visual C#-ASP.NET-Webanwendung. 

1. Öffnen Sie Visual Studio, und wählen Sie **Datei** > **Neues Projekt**.

   1. Erweitern Sie den Bereich **Installiert**. Wechseln Sie zu **Vorlagen** > **Visual C#** > **Web**, und wählen Sie dann **ASP.NET-Webanwendung** aus.

   2. Geben Sie einen Projektnamen, Speicherort und Projektmappennamen für Ihre App an, und klicken Sie auf **OK**.

   Beispiel:

   ![Erstellen einer Visual C#-ASP.NET-Webanwendung](./media/custom-connector-build-web-api-app-tutorial/visual-studio-new-project-aspnet-web-app.png)

2. Wählen Sie im Feld **Neue ASP.NET-Webanwendung** die Vorlage **Web-API** aus. Falls nicht bereits ausgewählt, wählen Sie **In der Cloud hosten** aus. Klicken Sie auf **Authentifizierung ändern**.

   ![Die Vorlage „Web-API“, „In der Cloud hosten“, „Authentifizierung ändern“ auswählen](./media/custom-connector-build-web-api-app-tutorial/visual-studio-web-api-template.png)

3. Wählen Sie **Keine Authentifizierung**, und klicken Sie auf **OK**. Sie können die Authentifizierung später einrichten.

   ![„Keine Authentifizierung“ auswählen](./media/custom-connector-build-web-api-app-tutorial/visual-studio-change-authentication.png)

4. Klicken Sie im eingeblendeten Dialogfeld **Neue ASP.NET-Webanwendung** auf **OK**. 

5. Überprüfen Sie im Feld **App Service erstellen** die in der Tabelle beschriebenen Hostingeinstellungen, nehmen Sie die gewünschten Änderungen vor, und klicken Sie auf **Erstellen**. 

   Ein [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) stellt die Sammlung physischer Ressourcen dar, die zum Hosten von Apps in Ihrem Azure-Abonnement verwendet werden. Erfahren Sie mehr über [App Service](../app-service/app-service-value-prop-what-is.md).

   ![Erstellen eines App Service](./media/custom-connector-build-web-api-app-tutorial/visual-studio-create-app-service.png)

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | Ihr Geschäfts-, Schul- oder Unikonto oder Ihr persönliches Microsoft-Konto | *Ihr Benutzerkonto* | Wählen Sie Ihr Benutzerkonto aus. | 
   | **Web-App-Name** | *Benutzerdefinierter Web-API-App-Name* oder Standardname | Geben Sie den Namen für Ihre Web-API-App ein, der in der URL Ihrer App verwendet wird. Beispiel: http://*Web-API-App-Name*. | 
   | **Abonnement** | *Name des Azure-Abonnements* | Wählen Sie das Azure-Abonnement aus, das Sie verwenden möchten. | 
   | **Ressourcengruppe** | *Name der Azure-Ressourcengruppe* | Wählen Sie eine vorhandene Azure-Ressourcengruppe aus, oder erstellen Sie, falls noch nicht erfolgt, eine neue Ressourcengruppe. <p>**Hinweis**: Eine Azure-Ressourcengruppe dient zum Organisieren von Azure-Ressourcen in Ihrem Azure-Abonnement. | 
   | **App Service-Plan** | *Name des App Service-Plans* | Wählen Sie einen vorhandenen App Service-Plan aus, oder erstellen Sie, falls noch nicht erfolgt, einen neuen Plan. | 
   |||| 

   Wenn Sie ein App Service-Plan erstellen, geben Sie diese Einstellungen an:

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **Standort** | *Bereitstellungsregion* | Wählen Sie die Region aus, in der Ihre App bereitgestellt werden soll. | 
   | **Größe** | *Größe des App Service-Plans* | Wählen Sie Ihre Plangröße aus, die die Kosten und Rechenressourcenkapazität für Ihren Serviceplan bestimmt. | 
   |||| 

   Um weitere Ressourcen einzurichten, die von Ihrer App benötigt werden, wählen Sie **Weitere Azure Services durchsuchen** aus.

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **Ressourcentyp** | *Azure-Ressourcentyp* | Wählen Sie für Ihre App erforderliche zusätzliche Ressourcen aus, und richten Sie diese ein. | 
   |||| 

6. Nachdem Visual Studio Ihr Projekt bereitgestellt hat, erstellen Sie den Code für Ihre App.

## <a name="create-an-openapi-swagger-file-that-describes-your-web-api"></a>Erstellen einer OpenAPI- (Swagger)-Datei, die Ihre Web-API beschreibt

Um Ihre Web-API-App mit Logic Apps zu verbinden, benötigen Sie eine [OpenAPI-Datei (ehemals Swagger)](http://swagger.io/), die Ihre API-Vorgänge beschreibt. Sie können Ihre eigene OpenAPI-Definition für Ihre API mit dem [Swagger-Online-Editor](http://editor.swagger.io/) schreiben, doch in diesem Tutorial wird ein Open-Source-Tool namens [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle/blob/master/README.md) verwendet.

1. Falls noch nicht geschehen, installieren Sie das NuGet-Paket von Swashbuckle in Ihr Visual Studio-Projekt.

   1. Klicken Sie in Visual Studio auf **Extras** > **NuGet-Paket-Manager** > 
    **Paket-Manager-Konsole**.

   2. Wechseln Sie in der **Paket-Manager-Konsole** zum Projektverzeichnis Ihrer App, falls Sie sich nicht bereits dort befinden (führen Sie `Set-Location "project-path"` aus), und führen Sie dieses PowerShell-Cmdlet aus: 
   
      `Install-Package Swashbuckle`

      Beispiel:

      ![Paket-Manager-Konsole, Swashbuckle installieren](./media/custom-connector-build-web-api-app-tutorial/visual-studio-package-manager-install-swashbuckle.png)

   > [!TIP]
   > Wenn Sie Ihre App nach der Installation von Swashbuckle ausführen, wird von Swashbuckle an dieser URL eine OpenAPI-Datei generiert: 
   >
   > http://*{Stamm-URL-Ihrer Web-API-App}*/swagger/docs/v1
   > 
   > Swashbuckle generiert auch eine Benutzeroberfläche an dieser URL: 
   > 
   > http://*{Stamm-URL-Ihrer Web-API-App}*/swagger

3. Wenn Sie fertig sind, veröffentlichen Sie Ihre Web-API-App in Azure. Klicken Sie für die Veröffentlichung aus Visual Studio im Projektmappen-Explorer mit der rechten Maustaste auf Ihr Webprojekt. Wählen Sie **Veröffentlichen...** aus, und folgen Sie den Anweisungen.

   > [!IMPORTANT]
   > Doppelte Vorgang-IDs machen ein OpenAPI-Dokument ungültig. Wenn Sie die C#-Beispielvorlage verwendet haben, *wiederholt die Vorlage diese Vorgangs-ID zweimal*: `Values_Get` 
   > 
   > Um dieses Problem zu beheben, ändern Sie eine Instanz in `Value_Get`, und wiederholen Sie die Veröffentlichung.

4. Rufen Sie das OpenAPI-Dokument ab, indem Sie zu diesem Speicherort navigieren: 

   http://*{Stamm-URL-Ihrer Web-API-App}*/swagger/docs/v1

   Sie können auch ein [OpenAPI-Beispieldokument](https://pwrappssamples.blob.core.windows.net/samples/webAPI.json) aus diesem Tutorial herunterladen. 
   Entfernen Sie unbedingt die Kommentare, die mit „//“ beginnen, bevor Sie das Dokument verwenden.

5. Speichern Sie den Inhalt als JSON-Datei. Abhängig von Ihrem Browser müssen Sie den Text eventuell kopieren und in eine leere Textdatei einfügen.

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten der Authentifizierung für benutzerdefinierte Connectors](../logic-apps/custom-connector-azure-active-directory-authentication.md)











