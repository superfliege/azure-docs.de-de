---
title: 'Benutzerdefinierte Bing-Suche: Erstellen einer benutzerdefinierten Suchwebseite | Microsoft Docs'
description: Beschreibt, wie eine benutzerdefinierte Suchinstanz konfiguriert und in eine Webseite integriert wird.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/16/2017
ms.author: v-brapel
ms.openlocfilehash: c1431ec852cab943e00d3933ef4f0500a4fdb151
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374755"
---
# <a name="build-a-custom-search-web-page"></a>Erstellen einer benutzerdefinierten Suchwebseite
Die benutzerdefinierte Bing-Suche ermöglicht das Erstellen einer maßgeschneiderten Suchbenutzeroberfläche für Themen, die Sie interessieren. Wenn Sie beispielsweise Besitzer einer Kampfsportwebsite sind, die eine Suchbenutzeroberfläche bietet, können Sie die Domänen, Unterwebsites und Webseiten angeben, die Bing durchsucht. Ihre Benutzer sehen Suchergebnisse, die auf den Inhalt zugeschnitten sind, für den sie sich interessieren, anstatt durch allgemeine Suchergebnisse blättern zu müssen, die möglicherweise irrelevante Inhalte enthalten. 

Dieses Tutorial zeigt, wie eine benutzerdefinierte Suchinstanz konfiguriert und in eine neue Webseite integriert wird.

Die folgenden Aufgaben werden beschrieben:

> [!div class="checklist"]
> - Erstellen einer benutzerdefinierten Suchinstanz
> - Hinzufügen aktiver Einträge
> - Hinzufügen blockierter Einträge
> - Hinzufügen angehefteter Einträge
> - Integrieren der benutzerdefinierte Suche in eine Webseite

## <a name="prerequisites"></a>Voraussetzungen
- Um dem Tutorial folgen zu können, benötigen Sie einen Abonnementschlüssel für die API für die benutzerdefinierte Bing-Suche.  Unter [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) erfahren Sie, wie Sie einen Schlüssel erhalten.
- Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die **kostenlose** [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden.

## <a name="create-a-custom-search-instance"></a>Erstellen einer benutzerdefinierten Suchinstanz
So erstellen Sie eine benutzerdefinierte Bing-Suchinstanz:

1.  Öffnen Sie einen Internetbrowser.
2.  Navigieren Sie zum benutzerdefinierten Such[portal](https://customsearch.ai).
3.  Melden Sie sich mit einem Microsoft-Konto (MSA) am Portal an. Wenn Sie kein MSA besitzen, klicken Sie auf **Microsoft-Konto erstellen**. Wenn Sie das Portal zum ersten Mal nutzen, werden Sie nach Berechtigungen für den Zugriff auf Ihre Daten gefragt. Klicken Sie auf **Ja**.
4.  Klicken Sie nach der Anmeldung auf **Neue benutzerdefinierte Suche**. Geben Sie im Fenster **Neue benutzerdefinierte Suchinstanz erstellen** einen Namen ein, der sinnvoll ist und den Typ des Inhalts beschreibt, den die Suche zurückgibt. Sie können den Namen jederzeit ändern.
 
    ![Screenshot des Felds „Neue benutzerdefinierte Suchinstanz erstellen“](../media/newCustomSrch.png)

5.  Klicken Sie auf „OK“, geben Sie eine URL an, und geben Sie an, ob Unterseiten der Basisseite eingeschlossen werden sollen:

    ![Screenshot der URL-Definitionsseite](../media/newCustomSrch1-a.png)

## <a name="add-active-entries"></a>Hinzufügen aktiver Einträge
Um Ergebnisse von bestimmten Websites oder URLs einzuschließen, fügen Sie diese der Registerkarte **Aktiv** hinzu.

1.  Klicken Sie im **Definitions-Editor** auf die Registerkarte **Aktiv**, und geben Sie die URL mindestens einer Website ein, die Sie in die Suche einbeziehen möchten.

    ![Screenshot der Registerkarte „Aktiv“ des Definitions-Editors](../media/customSrchEditor.png)

2.  Um zu bestätigen, dass Ihre Instanz Ergebnisse zurückgibt, geben Sie eine Abfrage im Vorschaubereich auf der rechten Seite ein. Bing gibt nur Ergebnisse für öffentliche Websites zurück, die indiziert wurden.

## <a name="add-blocked-entries"></a>Hinzufügen blockierter Einträge
Um Ergebnisse von bestimmten Websites oder URLs auszuschließen, fügen Sie diese der Registerkarte **Blockiert** hinzu.

1. Klicken Sie im **Definitions-Editor** auf die Registerkarte **Blockiert**, und geben Sie die URL mindestens einer Website ein, die Sie aus der Suche ausschließen möchten.

    ![Screenshot der Registerkarte „Blockiert“ des Definitions-Editors](../media/blockedCustomSrch.png)


2. Um zu bestätigen, dass Ihre Instanz keine Ergebnisse aus blockierten Websites zurückgibt, geben Sie eine Abfrage im Vorschaubereich auf der rechten Seite ein. 

## <a name="add-pinned-entries"></a>Hinzufügen angehefteter Einträge
Um eine bestimmte Webseite am Anfang der Suchergebnisse anzuheften, fügen Sie auf der Registerkarte **Angeheftet** die Webseite und den Abfragebegriff hinzu.  Die Registerkarte **Angeheftet** enthält eine Liste mit Webseiten- und Abfragebegriffpaaren, die die Webseite angeben, die als oberstes Ergebnis für eine bestimmte Abfrage angezeigt wird. Der Abfragebegriff des Benutzers muss mit dem angehefteten Abfragebegriff für die Webseite genau übereinstimmen, damit diese am Anfang angeheftet wird.

1. Klicken Sie im **Definitions-Editor** auf die Registerkarte **Angeheftet**, und geben Sie die Webseite und den Abfragebegriff der Webseite ein, die als oberstes Ergebnis zurückgegeben werden soll.

    ![Screenshot der Registerkarte „Angeheftet“ des Definitions-Editors](../media/pinnedCustomSrch.png)

2. Um zu bestätigen, dass die Instanz die angegebene Webseite als oberstes Ergebnis zurückgibt, geben Sie den Abfrageausdruck ein, den Sie im Vorschaubereich auf der rechten Seite angeheftet haben.

## <a name="configure-hosted-ui"></a>Konfigurieren der gehosteten Benutzeroberfläche
Die benutzerdefinierte Suche stellt eine gehostete Benutzeroberfläche zum Rendern der JSON-Antwort von Ihrer benutzerdefinierten Suchinstanz bereit.  So definieren Sie die Benutzeroberfläche:

1. Klicken Sie auf die Registerkarte **Gehostete Benutzeroberfläche**.
2. Wählen Sie ein Layout aus.

    ![Screenshot des Schritts zum Auswählen des Layouts der gehosteten Benutzeroberfläche](./media/custom-search-hosted-ui-select-layout.png)

3. Wählen Sie ein Farbdesign aus.

    ![Screenshot des Schritts zum Auswählen des Layouts der gehosteten Benutzeroberfläche](./media/custom-search-hosted-ui-select-color-theme.png)

4. Geben Sie die zusätzlichen Konfigurationsoptionen an.

    ![Screenshot des Schritts der zusätzlichen Konfigurationen der gehosteten Benutzeroberfläche](./media/custom-search-hosted-ui-additional-configurations.png)

5. Fügen Sie Ihren **Abonnementschlüssel** ein. Weitere Informationen finden Sie unter [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search-api).

    ![Screenshot des Schritts der zusätzlichen Konfigurationen der gehosteten Benutzeroberfläche](./media/custom-search-hosted-ui-subscription-key.png)

[!INCLUDE[publish or revert](../includes/publish-revert.md)]

<a name="consuminghostedui"></a>
## <a name="consuming-hosted-ui"></a>Nutzen der gehosteten Benutzeroberfläche
Es gibt zwei Möglichkeiten, die gehostete Benutzeroberfläche zu nutzen.  

- Option 1: Integrieren Sie den bereitgestellten JavaScript-Codeausschnitt in Ihre Anwendung.
- Option 2: Verwenden Sie den bereitgestellten HTML-Endpunkt.

Im weiteren Verlauf dieses Tutorials wird **Option 1: Javascript-Codeausschnitt** beschrieben.  

## <a name="set-up-your-visual-studio-solution"></a>Einrichten Ihrer Visual Studio-Projektmappe
1. Öffnen Sie auf Ihrem Computer **Visual Studio**.
2. Wählen Sie im Menü **Datei** die Option **Neu** und anschließend **Projekt** aus.
3. Wählen Sie im Fenster **Neues Projekt** die Option **Visual C#-/Web-/ ASP.NET Core-Webanwendung** aus, benennen Sie Ihr Projekt, und klicken Sie dann auf **OK**.
   
    ![Screenshot des Fensters „Neues Projekt“](./media/custom-search-new-project.png)

4. Wählen Sie im Fenster **Neue ASP.NET Core-Webanwendung** die Option **Webanwendung** aus, und klicken Sie auf **OK**.
    
    ![Screenshot des Fensters „Neues Projekt“](./media/custom-search-new-webapp.png)

## <a name="edit-indexcshtml"></a>Bearbeiten von „Index.cshtml“
1. Erweitern Sie im **Projektmappen-Explorer** die Option **Seiten**, und doppelklicken Sie auf **index.cshtml**, um die Datei zu öffnen.

    ![Screenshot des Projektmappen-Explorers mit erweiterten Seiten und ausgewählter Datei „index.cshtml“](./media/custom-search-visual-studio-webapp-solution-explorer-index.png)

2. Löschen Sie in „index.cshtml“ den gesamten Code ab Zeile 7 (einschließlich).
    
    ``` razor
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }    
    ```

3. Fügen Sie ein Zeilenumbruchelement und ein div-Element hinzu, das als Container fungiert.

    ``` html
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }
    <br />
    <div id="customSearch"></div>
    ```

4. Scrollen Sie auf der Registerkarte **Gehostete Benutzeroberfläche** zum Abschnitt mit dem Titel **Nutzen der Benutzeroberfläche**. Kopieren Sie den JavaScript-Codeausschnitt.

    ![Screenshot der Schaltfläche „Speichern“ der gehosteten Benutzeroberfläche](./media/custom-search-hosted-ui-consuming-ui.png)

5. Fügen Sie das Skriptelement in den Container ein, den Sie hinzugefügt haben.

    ``` html
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }
    <br />
    <div id="customSearch">
        <script type="text/javascript"
                id="bcs_js_snippet"
                src="https://ui.customsearch.ai/api/ux/render?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate">
        </script>
    </div>
    ```

6. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **wwwroot**, und klicken Sie auf **Im Browser anzeigen**.

    ![Screenshot des Projektmappen-Explorers: Auswählen der Option „Ansicht“ aus dem wwwroot-Kontextmenü im Browser](./media/custom-search-webapp-view-in-browser.png)

Die neue benutzerdefinierte Suchwebseite sollte ähnlich wie das folgende Beispiel aussehen:

![Screenshot der benutzerdefinierten Suchwebseite](./media/custom-search-webapp-browse-index.png)

Beim Ausführen einer Suche werden die Ergebnisse wie folgt gerendert.

![Screenshot der benutzerdefinierten Suchergebnisse](./media/custom-search-webapp-results.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> - Erstellen einer benutzerdefinierten Suchinstanz
> - Hinzufügen aktiver Einträge
> - Hinzufügen blockierter Einträge
> - Hinzufügen angehefteter Einträge
> - Integrieren der benutzerdefinierte Suche in eine Webseite

Sie können jetzt damit beginnen, Endpunkte für die benutzerdefinierte Bing-Suche programmgesteuert aufzurufen.

> [!div class="nextstepaction"]
> [Aufrufen eines Endpunkts für die benutzerdefinierte Bing-Suche (C#)](../call-endpoint-csharp.md)