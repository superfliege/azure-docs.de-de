---
title: Erstellen einer einseitigen Azure Time Series Insights-Web-App | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine einseitige Webanwendung erstellen, die Daten aus einer TSI-Umgebung abfragt und rendert.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 06/14/2018
ms.author: anshan
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: fccd509d4f16cee86d30feb0e838f1493cae4e0b
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275838"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Tutorial: Erstellen einer einseitigen Azure Time Series Insights-Web-App

In diesem Tutorial werden die Schritte zum Erstellen Ihrer eigenen einseitigen Webanwendung (Single-Page Application, SPA) für den Zugriff auf TSI-Daten basierend auf der [Time Series Insights-Beispielanwendung (TSI)](https://insights.timeseries.azure.com/clientsample) erläutert. In diesem Tutorial erhalten Sie Informationen zu Folgendem:

> [!div class="checklist"]
> * Anwendungsentwurf
> * Registrieren Ihrer Anwendung bei Azure Active Directory (AD)
> * Erstellen, Veröffentlichen und Testen Ihrer Webanwendung 

## <a name="prerequisites"></a>Voraussetzungen

Registrieren Sie sich für ein [kostenloses Azure-Abonnement](https://azure.microsoft.com/free/), falls Sie noch kein Azure-Abonnement besitzen. 

Installieren Sie außerdem Visual Studio, sofern noch nicht geschehen. Für dieses Tutorial können Sie die [kostenlose Community-Version oder eine kostenlose Testversion herunterladen/installieren](https://www.visualstudio.com/downloads/).

## <a name="application-design-overview"></a>Übersicht über den Anwendungsentwurf

Wie bereits erwähnt bildet die TSI-Beispielanwendung die Grundlage für den in diesem Tutorial verwendeten Entwurf und Code. In dem Code wird unter anderem die TSI-JavaScript-Clientbibliothek verwendet. Die TSI-Clientbibliothek bietet eine Abstraktion für zwei API-Hauptkategorien:

- **Wrappermethoden für den Aufruf der TSI-Abfrage-APIs:** REST-APIs, mit denen Sie TSI-Daten unter Verwendung JSON-basierter Ausdrücke abrufen können. Die Methoden sind unter dem `TsiClient.server`-Namespace der Bibliothek organisiert.
- **Methoden zum Erstellen und Auffüllen mehrerer Typen von Diagrammsteuerelementen:** Methoden, mit denen die TSI-Daten auf einer Webseite visualisiert werden. Die Methoden sind unter dem `TsiClient.ux`-Namespace der Bibliothek organisiert.

In diesem Tutorial werden darüber hinaus die Daten aus der TSI-Umgebung der Beispielanwendung verwendet. Ausführliche Informationen zur Struktur der TSI-Beispielanwendung und zu ihrer Verwendung der TSI-Clientbibliothek finden Sie im Tutorial [Erkunden der Azure Time Series Insights-JavaScript-Clientbibliothek](tutorial-explore-js-client-lib.md).

## <a name="register-the-application-with-azure-ad"></a>Registrieren der Anwendung bei Azure AD 

Vor dem Erstellen der Anwendung müssen Sie sie bei Azure AD registrieren. Durch die Registrierung wird die Identitätskonfiguration für eine Anwendung bereitgestellt und die Verwendung der OAuth-Unterstützung für einmaliges Anmelden ermöglicht. OAuth setzt voraus, dass SPAs die „implizite“ Autorisierungsgewährung verwenden. Diese wird im Anwendungsmanifest aktualisiert. Ein Anwendungsmanifest ist eine JSON-Darstellung der Identitätskonfiguration einer Anwendung. 

1. Melden Sie sich unter Verwendung Ihres Azure-Abonnementkontos am [Azure-Portal](https://portal.azure.com) an.  
1. Klicken Sie im linken Bereich auf die Ressource **Azure Active Directory** und dann auf **App-Registrierungen** und **+ Registrierung einer neuen Anwendung**:  
   
   ![Azure-Portal: Registrierung einer Anwendung bei Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png)

1. Füllen Sie auf der Seite **Erstellen** die erforderlichen Parameter aus:
   
   Parameter|BESCHREIBUNG
   ---|---
   **Name** | Geben Sie einen aussagekräftigen Registrierungsnamen an.  
   **Anwendungstyp** | Sie erstellen eine SPA-Webanwendung, übernehmen Sie daher „Web-App/API“.
   **Anmelde-URL** | Geben Sie die URL für die Start-/Anmeldeseite der Anwendung ein. Da die Anwendung (später) in Azure App Service gehostet wird, müssen Sie eine URL in der Domäne „https://azurewebsites.net“ verwenden. In diesem Beispiel basiert der Name auf dem Registrierungsnamen.

   Wenn Sie fertig sind, klicken Sie auf **Erstellen**, um die Registrierung der neuen Anwendung zu erstellen.

   ![Azure-Portal: Registrierung einer Anwendung bei Azure AD – Erstellung](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png)

1. Ressourcenanwendungen stellen REST-APIs zur Verwendung durch andere Anwendungen bereit und werden ebenfalls bei Azure AD registriert. APIs bieten durch die Bereitstellung von „Bereichen“ differenzierten/sicheren Zugriff auf Clientanwendungen. Da Ihre Anwendung die API „Azure Time Series Insights“ aufruft, müssen Sie die API und den Bereich angeben. Zur Laufzeit wird die Berechtigung dafür angefordert/gewährt. Klicken Sie auf **Einstellungen** und anschließend auf **Erforderliche Berechtigungen** und **+ Hinzufügen**:

   ![Azure-Portal: Hinzufügen von Berechtigungen für Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png)

1. Klicken Sie auf der Seite **API-Zugriff hinzufügen** auf **1 API auswählen**, um die TSI-API anzugeben. Geben Sie auf der Seite **API auswählen** im Suchfeld „azure time“ ein. Wählen Sie anschließend in der Ergebnisliste die API „Azure Time Series Insights“ aus, und klicken Sie auf **Auswählen**: 

   ![Azure-Portal: Hinzufügen von Berechtigungen für Azure AD – API](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png)

1. Nun geben Sie einen Bereich für die API an. Klicken Sie auf der Seite **API-Zugriff hinzufügen** auf **2 Berechtigungen auswählen**. Wählen Sie auf der Seite **Zugriff aktivieren** den Bereich „Access Azure Time Series Insights service“ (Auf Azure Time Series Insights-Dienst zugreifen) aus. Klicken Sie auf **Auswählen**. Dadurch gelangen Sie zurück auf die Seite **API-Zugriff hinzufügen**. Klicken Sie dort auf **Fertig**:

   ![Azure-Portal: Hinzufügen von Berechtigungen für Azure AD – Bereich](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png)

1. Beachten Sie, dass auf der Seite **Erforderliche Berechtigungen** nun die API „Azure Time Series Insights“ aufgeführt ist. Sie müssen außerdem vorab für alle Benutzer die entsprechende Berechtigung erteilen, damit die Anwendung auf die API und den Bereich zugreifen kann. Klicken Sie oben auf die Schaltfläche **Berechtigungen erteilen** und anschließend auf **Ja**:

   ![Azure-Portal: Erforderliche Berechtigungen für Azure AD – Zustimmung](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png)

1. Wie bereits erwähnt müssen Sie darüber hinaus das Anwendungsmanifest aktualisieren. Klicken Sie auf der Breadcrumb-Leiste auf den Namen der Anwendung, um zurück zur Seite **Registrierte App** zu gelangen. Klicken Sie auf **Manifest**, ändern Sie die Eigenschaft `oauth2AllowImplicitFlow` in `true`, und klicken Sie auf **Speichern**:

   ![Azure-Portal: Aktualisieren des Manifests – Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png)

1. Klicken Sie abschließend auf die Breadcrumb-Leiste, um erneut die Seite **Registrierte App** aufzurufen, und kopieren Sie die Eigenschaften der **URL der Startseite** und der **Anwendungs-ID** für Ihre Anwendung. Sie verwenden diese Eigenschaften in einem späteren Schritt:

   ![Azure-Portal: Azure AD-Eigenschaften](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png)

## <a name="build-and-publish-the-web-application"></a>Erstellen und Veröffentlichen der Webanwendung

1. Erstellen Sie ein Verzeichnis, um die Anwendungsprojektdateien zu speichern. Navigieren Sie dann zu den folgenden URLs, klicken Sie oben rechts auf der Seite mit der rechten Maustaste auf den Link „Roh“, und klicken Sie zum Speichern im Projektverzeichnis auf „Speichern unter“:

   > [!NOTE]
   > Je nach Browser müssen Sie vor dem Speichern der Datei die Dateierweiterung korrigieren (HTML oder CSS).

   - **index.html**: HTML und JavaScript für die Seite https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html
   - **sampleStyles.css:** CSS-Stylesheet: https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css
    
1. Starten Sie Visual Studio, und melden Sie Sich an, um ein Projekt für die Webanwendung zu erstellen. Klicken Sie im Menü **Datei** auf **Öffnen** > **Website**. Wählen Sie im Dialogfeld **Website öffnen** das Arbeitsverzeichnis aus, in dem Sie die HTML- und CSS-Dateien gespeichert haben, und klicken Sie dann auf **Öffnen**:

   ![VS: Datei > Website öffnen](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)

1. Öffnen Sie in Visual Studio den **Projektmappen-Explorer** über das Menü **Ansicht**. Daraufhin sollte die neue Projektmappe mit einem Websiteprojekt (Globussymbol) angezeigt werden, das die HTML- und CSS-Dateien enthält:

   ![VS: Neue Projektmappe im Projektmappen-Explorer](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)

1. Bevor Sie die Anwendung veröffentlichen können, müssen Sie Teile des JavaScript-Codes in **index.html** aktualisieren: 

   a. Ändern Sie zunächst die Pfade für den Verweis auf die JavaScript- und Stylesheet-Dateien im Element `<head>`. Öffnen Sie die Datei **index.html** in Ihrer Visual Studio-Projektmappe, und suchen Sie die folgenden Zeilen JavaScript-Code. Heben Sie die Auskommentierung der drei Zeilen unter „PROD RESOURCE LINKS“ auf, und kommentieren Sie die drei Zeilen unter „DEV RESOURCE LINKS“ aus:
   
      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      Der geänderte Code sollte in etwa wie im folgenden Beispiel aussehen:

      ```javascript
      <!-- PROD RESOURCE LINKS -->
      <link rel="stylesheet" type="text/css" href="sampleStyles.css"></link>
      <script src="https://unpkg.com/tsiclient@1.1.4/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="https://unpkg.com/tsiclient@1.1.4/tsiclient.css"></link>

      <!-- DEV RESOURCE LINKS -->
      <!-- <link rel="stylesheet" type="text/css" href="pages/samples/sampleStyles.css"></link>
      <script src="dist/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="dist/tsiclient.css"></link> -->
      ```

   b. Ändern Sie als Nächstes die Logik des Zugriffstokens so, dass sie die Registrierung der neuen Azure AD-Anwendung verwendet. Ändern Sie die Variablen `clientID` und `postLogoutRedirectUri` entsprechend, sodass sie die Anwendungs-ID und die URL der Startseite verwenden, die Sie in Schritt 9 des Abschnitts [Registrieren der Anwendung bei Azure AD](#register-the-application-with-azure-ad) kopiert haben:

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      Der geänderte Code sollte in etwa wie im folgenden Beispiel aussehen:

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ``` 

   c. Speichern Sie die Datei **index.html**, wenn Sie mit dem Bearbeiten fertig sind.

1. Veröffentlichen Sie nun die Webanwendung als Azure App Service-Instanz in Ihrem Azure-Abonnement:  

   > [!NOTE]
   > Einige Felder in den folgenden Dialogfeldern werden mit Daten aus Ihrem Azure-Abonnement aufgefüllt. Daher kann es einige Sekunden dauern, bis die einzelnen Dialogfelder vollständig geladen werden und Sie den Vorgang fortsetzen können.  

   a. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Websiteprojektknoten, und wählen Sie **Web-App veröffentlichen** aus.  

      ![VS: Veröffentlichen der Web-App – Projektmappen-Explorer](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)

   b. Wählen Sie **Microsoft Azure App Service** aus, um ein Veröffentlichungsziel zu erstellen:  

      ![VS: Veröffentlichungsprofil](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)  

   c. Wählen Sie das Abonnement aus, das Sie zum Veröffentlichen der Anwendung verwenden möchten, und klicken Sie auf „Neu“: 

      ![VS: Veröffentlichungsprofil – App Service](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)  

   d. Warten Sie einige Sekunden, bis im Dialogfeld **App Service erstellen** alle Felder geladen wurden, und passen Sie dann die folgenden Felder an:
   
      Feld | BESCHREIBUNG
      ---|---
      **App-Name** | Ändern Sie den Namen in den Namen der Azure AD-Anwendungsregistrierung, den Sie in Schritt 3 unter [Registrieren der Anwendung bei Azure AD](#register-the-application-with-azure-ad) verwendet haben. 
      **Ressourcengruppe** | Ändern Sie über die Schaltfläche **Neu...** die Angabe so, dass sie mit dem Feld **App-Name** übereinstimmt.
      **App Service-Plan** | Ändern Sie über die Schaltfläche **Neu...** die Angabe so, dass sie mit dem Feld **App-Name** übereinstimmt.

      Klicken Sie auf **Erstellen**, wenn Sie fertig sind. Die Schaltfläche **Exportieren** links unten wird während der Erstellung der Azure-Ressourcen einige Sekunden lang durch „Wird bereitgestellt:“ ersetzt. Nach der Erstellung der Ressourcen wird wieder das vorherige Dialogfeld angezeigt. 

      ![VS: Veröffentlichungsprofil – Neuen App Service hinzufügen](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service-create.png)  

   e. Kehren Sie zum Dialogfeld **Veröffentlichen** zurück, und stellen Sie sicher, dass für **Veröffentlichungsmethode** die Option „Web Deploy“ festgelegt ist. Ändern Sie darüber hinaus die **Ziel-URL**, sodass sie `https` anstelle von `http` verwendet und der Azure AD-Anwendungsregistrierung entspricht. Klicken Sie dann auf „Veröffentlichen“, um die Webanwendung bereitzustellen:  

      ![VS: Veröffentlichen der Web-App – Veröffentlichen des App Service](media/tutorial-create-tsi-sample-spa/vs-publish-publish.png)  

   f. Im Visual Studio-Fenster **Ausgabe** sollte daraufhin ein Protokoll mit der erfolgreichen Veröffentlichung angezeigt werden. Nach der Bereitstellung öffnet Visual Studio außerdem die Webanwendung auf einer Browserregisterkarte, auf der Sie zur Anmeldung aufgefordert werden. Nach der erfolgreichen Anmeldung sind alle TSI-Steuerelemente mit Daten aufgefüllt:  

      [![VS: Veröffentlichen einer Web-App – Ausgabe des Veröffentlichungsprotokolls](media/tutorial-create-tsi-sample-spa/vs-publish-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-output.png#lightbox)

      ![TSI-SPA-App: Anmeldung](media/tutorial-create-tsi-sample-spa/tsispaapp-azurewebsites-net.png)  

## <a name="troubleshooting"></a>Problembehandlung  

Fehlercode/Bedingung | BESCHREIBUNG
---------------------| -----------
*AADSTS50011: No reply address is registered for the application.* (AADSTS50011: Für die Anwendung ist keine Antwortadresse registriert.) | In der Azure AD-Registrierung fehlt die Eigenschaft für die Antwort-URL (Reply URL). Navigieren Sie für Ihre Azure AD-Anwendungsregistrierung zu **Einstellungen** / **Antwort-URLs**. Überprüfen Sie, ob die in Schritt 3 des Abschnitts [Registrieren der Anwendung bei Azure AD](#register-the-application-with-azure-ad) angegebene **Anmelde-URL** vorhanden ist. 
*AADSTS50011: The reply url specified in the request does not match the reply urls configured for the application: '<Application ID GUID>'.* (AADSTS50011: Die in der Anforderung angegebene Antwort-URL entspricht nicht den für die Anwendung konfigurierten Anwort-URLs: <Anwendungs-ID (GUID)>.) | Der in Schritt 4.b des Abschnitts [Erstellen und Veröffentlichen der Webanwendung](#build-and-publish-the-web-application) angegebene Wert für `postLogoutRedirectUri` muss dem Wert entsprechen, den Sie in der Azure AD-Anwendungsregistrierung unter der Eigenschaft **Einstellungen** / **Antwort-URLs** angegeben haben. Achten Sie außerdem darauf, dass Sie die **Ziel-URL** so ändern, dass sie `https` verwendet (gemäß Schritt 5.e des Abschnitts [Erstellen und Veröffentlichen der Webanwendung](#build-and-publish-the-web-application)).
Die Webanwendung wird zwar geladen, es wird jedoch eine nicht formatierte Nur-Text-Anmeldeseite mit weißem Hintergrund angezeigt. | Vergewissern Sie sich, dass die in Schritt 4.a des Abschnitts [Erstellen und Veröffentlichen der Webanwendung](#build-and-publish-the-web-application) erläuterten Pfade korrekt sind. Kann die Webanwendung die CSS-Dateien nicht finden, ist die Seite nicht ordnungsgemäß formatiert.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Tutorial werden mehrere ausgeführte Azure-Dienste erstellt. Falls Sie diese Tutorialreihe nicht abschließen möchten, empfehlen wir Ihnen, alle Ressourcen zu löschen, damit keine unnötigen Kosten entstehen. 

Gehen Sie im Azure-Portal im Menü auf der linken Seite wie folgt vor:

1. Klicken Sie auf das Symbol **Ressourcengruppen**, und wählen Sie dann die Ressourcengruppe aus, die Sie für die TSI-Umgebung erstellt haben. Klicken Sie oben auf der Seite auf **Ressourcengruppe löschen**, geben Sie den Namen der Ressourcengruppe ein, und klicken Sie dann auf **Löschen**. 
1. Klicken Sie auf das Symbol **Ressourcengruppen**, und wählen Sie dann die Ressourcengruppe aus, die mit dem Solution Accelerator für die Gerätesimulation erstellt wurde. Klicken Sie oben auf der Seite auf **Ressourcengruppe löschen**, geben Sie den Namen der Ressourcengruppe ein, und klicken Sie dann auf **Löschen**. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Anwendungsentwurf
> * Registrieren Ihrer Anwendung bei Azure Active Directory (AD)
> * Erstellen, Veröffentlichen und Testen Ihrer Webanwendung 

In diesem Tutorial erfolgt die Integration in Azure AD. Dabei wird die angemeldete Identität des Benutzers zum Abrufen eines Zugriffstokens verwendet. Informationen zum Zugreifen auf die TSI-API mithilfe der Identität einer Dienst-/Daemon-Anwendung finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Authentifizierung und Autorisierung für die Azure Time Series Insights-API](time-series-insights-authentication-and-authorization.md)
