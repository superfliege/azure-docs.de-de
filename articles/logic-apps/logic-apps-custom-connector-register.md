---
title: "Registrieren benutzerdefinierter Connectors – Azure Logic Apps | Microsoft-Dokumentation"
description: "Informationen zum Einrichten benutzerdefinierter Connectors für die Nutzung in Azure Logic Apps"
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
ms.openlocfilehash: 9e3d88abe751b37700590cc68c458f208d5868d2
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="register-custom-connectors-in-azure-logic-apps"></a>Registrieren benutzerdefinierter Connectors in Azure Logic Apps

Nachdem Sie Ihre REST-API erstellt, die Authentifizierung eingerichtet und Ihre OpenAPI-Definitionsdatei oder eine Postman Collection abgerufen haben, können Sie Ihren Connector registrieren. 

## <a name="prerequisites"></a>Voraussetzungen

Um Ihren benutzerdefinierten Connector registrieren zu können, benötigen Sie diese Elemente:

* Details für die Registrierung Ihres Connectors in Azure, wie z.B. die Angaben „Name“, „Azure-Abonnement“, „Azure-Ressourcengruppe“ und „Region“

* Eine OpenAPI- (Swagger)-Datei oder Postman Collection, die Ihre Web-API beschreibt

  Für dieses Tutorial können Sie die [Azure Resource Manager-Beispieldatei für OpenAPI](http://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json) verwenden.

* Ein Symbol, das Ihren Connector darstellt

* Eine kurze Beschreibung des Connectors

* Den Hostspeicherort Ihrer API

## <a name="1-create-your-connector"></a>1. Erstellen des Connectors

1. Wählen Sie im Azure-Portal im Azure-Hauptmenü **Neu** aus. Geben Sie in das Suchfeld „Logik Apps-Connector“ ein, und drücken Sie die EINGABETASTE.

   ![Neu, nach „Logic Apps-Connector“ suchen](./media/logic-apps-custom-connector-register/create-logic-apps-connector.png)

2. Wählen Sie in der Ergebnisliste **Logic Apps-Connector** > **Erstellen** aus.

   ![Erstellen eines Logic Apps-Connectors](./media/logic-apps-custom-connector-register/choose-logic-apps-connector.png)

3. Geben Sie Details für die Registrierung Ihres Connectors an, wie in der Tabelle beschrieben. Wenn Sie fertig sind, wählen Sie **An Dashboard anheften** > **Erstellen** aus.

   ![Details eines benutzerdefinierten Logik-App-Connectors](./media/logic-apps-custom-connector-register/logic-apps-connector-details.png)

   | Eigenschaft | Empfohlener Wert | Beschreibung | 
   | -------- | --------------- | ----------- | 
   | **Name** | *Name des benutzerdefinierten Connectors* | Geben Sie einen Namen für den Connector an. | 
   | **Abonnement** | *Name des Azure-Abonnements* | Wählen Sie Ihr Azure-Abonnement. | 
   | **Ressourcengruppe** | *Name der Azure-Ressourcengruppe* | Erstellen Sie eine Azure-Ressourcengruppe zum Organisieren Ihrer Azure-Ressourcen, oder wählen Sie eine aus. | 
   | **Standort** | *Bereitstellungsregion* | Wählen Sie eine Bereitstellungsregion für den Connector aus. | 
   |||| 

   Nachdem Azure den Connector bereitgestellt hat, wird das Menü des benutzerdefinierten Connectors automatisch geöffnet. 
   Falls nicht, wählen Sie Ihren benutzerdefinierten Connector im Azure-Dashboard aus.

## <a name="2-define-your-connector"></a>2. Definieren Ihres Connectors

Geben Sie nun die OpenAPI-Datei oder Postman Collection zum Erstellen Ihres Connectors, die Authentifizierung, die Ihr Connector verwendet, die Aktionen und Trigger, die Ihr benutzerdefinierter Connector bereitstellt, und die Parameter an, die Aktionen und Trigger verwenden können.

### <a name="2a-specify-the-openapi-file-or-postman-collection-for-your-connector"></a>2a. Angeben der OpenAPI-Datei oder Postman Collection für den Connector

1. Wählen Sie im Menü Ihres Connectors, falls nicht bereits ausgewählt, **Logic Apps-Connector**. Klicken Sie auf der Symbolleiste auf **Bearbeiten**.

   ![Bearbeiten eines benutzerdefinierten Connectors](./media/logic-apps-custom-connector-register/edit-custom-connector.png)

2. Wählen Sie **Allgemein**, damit Sie die Details in diesen Tabellen zum Erstellen, Schützen und Definieren der Aktionen und Trigger für Ihren benutzerdefinierten Connector angeben können.

   1. Wählen Sie für **Benutzerdefinierte Connectors** eine Option aus, damit Sie die OpenAPI- bzw. Swagger-Datei bereitstellen können, die Ihre API beschreibt.

      ![Bereitstellen der OpenAPI-Datei für Ihre API](./media/logic-apps-custom-connector-register/provide-openapi-file.png)

      | Option | Format |Beschreibung | 
      | ------ | ------ | ----------- | 
      | **OpenAPI-Datei hochladen** | *OpenAPI- bzw. Swagger-JSON-Datei* | Navigieren Sie zum Speicherort der OpenAPI-Datei, und wählen Sie diese Datei aus. | 
      | **OpenAPI-Datei verwenden** | http://*Pfad-zur-Swagger-JSON-Datei* | Geben Sie die URL für die OpenAPI-Datei Ihrer API an. | 
      | **Postman Collection V1 hochladen** | *Exportierte Postman Collection V1-Datei* | Navigieren Sie zum Speicherort einer exportierten Postman Collection im V1-Format. | 
      |||| 

   2. Laden Sie in **Allgemeine Informationen** ein Symbol Ihres Connectors hoch. 
   In der Regel werden die Felder **Beschreibung**, **Host** und **Basis-URL** automatisch anhand Ihrer OpenAPI-Datei ausgefüllt. 
   Falls nicht, fügen Sie diese Informationen wie in der Tabelle beschrieben hinzu, und klicken Sie auf **Weiter**. 

      ![Connectordetails](./media/logic-apps-custom-connector-register/add-connector-details.png)

      | Option oder Einstellung | Format | Beschreibung | 
      | ----------------- | ------ | ----------- | 
      | **Symbol hochladen** | *PNG-oder JPG-Datei kleiner als 1 MB* | Ein Symbol, das Ihren Connector darstellt <p>Farbe: Vorzugsweise ein weißes Logo auf farbigem Hintergrund. <p>Ein Logo mit ca. 160 Pixeln innerhalb eines Quadrats mit 230 Pixeln | 
      | **Symbolhintergrundfarbe** | *Hexadezimalcode der Brandingfarbe des Symbols* | <p>Die Farbe hinter dem Symbol, die der Hintergrundfarbe in Ihrer Symboldatei entspricht. <p>Format: Hexadezimal. „#007ee5“ stellt beispielsweise die Farbe Blau dar. | 
      | **Beschreibung** | *Connector-Beschreibung* | Geben Sie eine Kurzbeschreibung des Connectors an. | 
      | **Host** | *Connectorhost* | Geben Sie die Hostdomäne für Ihre Web-API an. | 
      | **Basis-URL** | *Basis-URL des Connectors* | Geben Sie die Basis-URL Ihrer Web-API an. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Beschreiben der vom Connector verwendeten Authentifizierung

1. Wählen Sie nun **Sicherheit**, damit Sie die Authentifizierung, die Ihr Connector verwendet, überprüfen oder beschreiben können. Die Authentifizierung stellt sicher, dass die Identitäten Ihrer Benutzer zwischen Ihrem Dienst und beliebigen Clients ordnungsgemäß übertragen werden.

   ![Authentifizierungsart](./media/logic-apps-custom-connector-register/security.png)

   * Wenn Sie eine OpenAPI-Datei hochladen, erkennt der Registrierungs-Assistent automatisch den Authentifizierungstyp, den Ihre Web-API verwendet. Er füllt den Abschnitt **Sicherheit** im Assistenten auf der Grundlage des `securityDefinitions`-Objekts in dieser Datei auf. Hier ist zum Beispiel ein Abschnitt, der die Verwendung von OAuth2.0 angibt:

     ``` json
     "securityDefinitions": {
       "AAD": {
       "type": "oauth2",
       "flow": "accessCode",
       "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
       "tokenUrl": "https://login.windows.net/common/oauth2/token",
       "scopes": {}
       }
     },
     ```

   * Wenn Ihre OpenAPI-Datei den Authentifizierungstyp und die Eigenschaften nicht ausgefüllt hat, wählen Sie **Bearbeiten**, damit Sie diese Informationen hinzufügen können. 
   
     Wenn Sie z.B. in diesem Tutorial die [Azure AD-Authentifizierung bereits eingerichtet haben](../logic-apps/custom-connector-azure-active-directory-authentication.md), haben Sie Azure AD-Apps zum Schützen Ihrer Web-API und Ihres Connectors erstellt. 
     Daher können Sie nun die Anwendungs-ID und den Clientschlüssel angeben, die/den Sie zuvor gespeichert haben:
    
     | Einstellung | Empfohlener Wert | Beschreibung | 
     | ------- | --------------- | ----------- | 
     | **Authentifizierungstyp** | OAuth 2.0 | | 
     | **Identitätsanbieter** | Azure Active Directory | | 
     | **Client-ID** | *Anwendungs-ID für Azure AD-App des Connectors* | Die Anwendungs-ID, die Sie zuvor für die Azure AD-App Ihres Connectors gespeichert haben | 
     | **Geheimer Clientschlüssel** | *Clientschlüssel für Azure AD-App des Connectors* | Der Clientschlüssel für die Azure AD-App des Connectors | 
     | **Anmelde-URL** | `https://login.windows.net` | | 
     | **Ressourcen-URL** | `https://management.core.windows.net/` | Achten Sie darauf, dass Sie die URL genau wie angegeben hinzufügen, einschließlich des abschließenden Schrägstrichs. | 
     |||| 

   * Eine Postman Collection, die automatisch eine OpenAPI-Datei für Sie generiert, füllt den Authentifizierungstyp *nur* dann automatisch aus, wenn Sie unterstützte Authentifizierungstypen wie OAuth 2.0 oder „Basic“ verwenden.

2. Um Ihren Connector nach Eingabe der Sicherheitsinformationen zu speichern, wählen Sie oben auf der Seite **Connector aktualisieren** und anschließend **Weiter** aus. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Überprüfen, Aktualisieren oder Definieren von Aktionen und Triggern für Ihren Connector

1. Wählen Sie nun **Definition**, um neue Aktionen und Trigger zu überprüfen, zu bearbeiten oder zu definieren, die Benutzer ihren Workflows hinzufügen können.

   Aktionen und Trigger basieren auf den Vorgängen, die in Ihrer OpenAPI-Datei oder Postman Collection definiert sind und die automatisch die Seite **Definition** mit den Anforderungs- und Antwortwerten auffüllen. Wenn hier also bereits die gewünschten Vorgänge angezeigt werden, können Sie zum nächsten Schritt im Registrierungsprozess übergehen, ohne Änderungen auf dieser Seite vorzunehmen.

   ![Connectordefinition](./media/logic-apps-custom-connector-register/definition.png)

2. Wenn Sie optional vorhandene Aktionen und Trigger bearbeiten oder neue hinzufügen möchten, fahren Sie mit diesen Schritten fort.

<a name="add-action-or-trigger"></a> 

#### <a name="edit-or-add-actions-for-your-connector"></a>Bearbeiten oder Hinzufügen von Aktionen für den Connector

1. Um eine vorhandene Aktion zu bearbeiten, wählen Sie die Nummer der Aktion aus. Um eine Aktion hinzuzufügen, die nicht in Ihrer OpenAPI-Datei oder Postman Collection vorhanden war, wählen Sie unter **Aktionen** die Option **Neue Aktion**.

2. Geben Sie unter **Allgemein** diese Informationen für die Aktion an, oder bearbeiten Sie sie:
   
   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **Zusammenfassung** | *Vorgangsname* | Der Titel dieser Aktion. | 
   | **Beschreibung** | *Beschreibung des Vorgangs* | Die Beschreibung dieser Aktion. <p>**Tipp**: Stellen Sie sicher, dass Ihre Beschreibung mit einem Punkt endet. |
   | **Vorgangs-ID** | *Vorgangsbezeichner* | Ein eindeutiger Name zum Identifizieren dieser Aktion. Verwenden Sie die wortinterne gemischte Groß-/Kleinschreibung, z.B. GetPullRequest. | 
   |**Sichtbarkeit**| **keine**, **erweitert**, **intern** oder **wichtig** | Die Sichtbarkeit dieser Aktion für Benutzer. Weitere Informationen finden Sie unter [OpenAPI-Erweiterungen](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   |||| 

3. Definieren Sie nun die Anforderung für die Aktion.
  
   1. Wählen Sie im Abschnitt **Anforderung** die Option **Aus Beispiel importieren**. 

   2. Fügen Sie auf der Seite **Aus Beispiel importieren** eine Beispielanforderung ein. 

      Üblicherweise sind Beispielanforderungen in der API-Dokumentation verfügbar, in der Sie Informationen zu den Feldern **Verb**, **URL**, **Headers** und **Body** erhalten. Siehe beispielsweise die [Dokumentation zur Textanalyse-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![Importieren einer Beispielanforderung](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > Wenn Sie einen Connector anhand einer Postman Collection erstellen, stellen Sie sicher, dass Sie den Header `Content-type` aus Aktionen und Triggern entfernen. Logic Apps fügt diesen Header automatisch hinzu. Entfernen Sie außerdem aus Aktionen und Triggern die im Abschnitt `Security` definierten Authentifizierungsheader.

      Erweiterte OpenAPI-Funktionen finden Sie unter [OpenAPI-Erweiterungen für benutzerdefinierte Connectors](../logic-apps/custom-connector-openapi-extensions.md).

   3. Wählen Sie im Abschnitt „Anforderung“ die Option **Importieren**.

4. Definieren Sie nun die Antwort für die Aktion.

   1. Unter **Antwort** können Sie eine Standardantwort angeben. 
   Wählen Sie **Standardantwort hinzufügen**.

   2. Fügen Sie auf der Seite **Aus Beispiel importieren** eine Beispielantwort ein, und wählen Sie dann **Importieren**.

5. Überprüfen und beheben Sie schließlich unter **Validierung** mögliche Probleme, die für die Aktion ausgemacht wurden.

#### <a name="edit-or-add-triggers-for-your-connector"></a>Bearbeiten oder Hinzufügen von Triggern für den Connector

1. Um einen vorhandenen Trigger zu bearbeiten, wählen Sie die Nummer des Triggers aus. Um einen Trigger hinzuzufügen, der nicht in Ihrer OpenAPI-Datei oder Postman Collection vorhanden war, wählen Sie unter **Aktionen** die Option **Neuer Trigger**.

2. Geben Sie unter **Allgemein** diese Informationen für den Trigger an, oder bearbeiten Sie sie:

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **Zusammenfassung** | *Vorgangsname* | Der Titel dieses Triggers. | 
   | **Beschreibung** | *Beschreibung des Vorgangs* | Die Beschreibung dieses Triggers. <p>**Tipp**: Stellen Sie sicher, dass Ihre Beschreibung mit einem Punkt endet. | 
   | **Vorgangs-ID** | *Vorgangsbezeichner* | Ein eindeutiger Name zum Identifizieren dieses Triggers. Verwenden Sie die wortinterne gemischte Groß-/Kleinschreibung, z.B. TriggerOnGitHubPushEvent. | 
   |**Sichtbarkeit**| **keine**, **erweitert**, **intern** oder **wichtig** | Die Sichtbarkeit dieses Triggers für Benutzer. Weitere Informationen finden Sie unter [OpenAPI-Erweiterungen](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   | **Triggertyp** | **Webhook** oder **Abruf** | Der Typs dieses Triggers. Ein Webhooktrigger wartet beispielsweise auf ein bestimmtes Ereignis, bevor er ausgelöst wird. Ein Abruftrigger überprüft einen Dienstendpunkt regelmäßig in einem festgelegten Intervall und mit einer bestimmten Häufigkeit. <p>Weitere Informationen zu Mustern von Webhook- und Abruftriggern finden Sie unter [Erstellen benutzerdefinierter APIs](../logic-apps/logic-apps-create-api-app.md). | 
   |||| 

3. Definieren Sie nun die Anforderung, die den Trigger erstellt. 

   1. Wählen Sie im Abschnitt **Anforderung** die Option **Aus Beispiel importieren**.

   2. Fügen Sie auf der Seite **Aus Beispiel importieren** eine Beispielanforderung ein. 

      Üblicherweise sind Beispielanforderungen in der API-Dokumentation verfügbar, in der Sie Informationen zu den Feldern **Verb**, **URL**, **Headers** und **Body** erhalten. Siehe beispielsweise die [Dokumentation zur Textanalyse-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![Importieren einer Beispielanforderung](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > Wenn Sie einen Connector anhand einer Postman Collection erstellen, stellen Sie sicher, dass Sie den Header `Content-type` aus Aktionen und Triggern entfernen. Logic Apps fügt diesen Header automatisch hinzu. Entfernen Sie außerdem aus Aktionen und Triggern die im Abschnitt `Security` definierten Authentifizierungsheader.

      Erweiterte OpenAPI-Funktionen finden Sie unter [OpenAPI-Erweiterungen für benutzerdefinierte Connectors](../logic-apps/custom-connector-openapi-extensions.md).

   3. Wählen Sie im Abschnitt „Anforderung“ die Option **Importieren**. 

4. Definieren Sie jetzt die Antwort des Triggers. Gehen Sie im Abschnitt **Antwort** basierend auf dem Typ des Triggers wie folgt vor:

   **Webhooktrigger**
   1. Wählen Sie im Abschnitt **Webhookantwort** die Option **Aus Beispiel importieren**. 

   2. Fügen Sie auf der Seite **Aus Beispiel importieren** eine Beispielantwort ein, und wählen Sie dann **Importieren**. Eine Beispielantwort finden Sie in der [GitHub-API-Referenz für das Erstellen eines Webhooks](https://developer.github.com/v3/repos/hooks/#create-a-hook).

   3. Wählen Sie unter **Triggerkonfiguration** einen Parameter für die Webhook-Erstellungsanforderung aus. Logic Apps verwendet diesen Parameterwert, um die Rückruf-URL aufzufüllen, die Ihr Dienst zur Kommunikation mit dem Trigger verwendet.

   **Abruftrigger**
   1. Unter **Antwort** können Sie eine Standardantwort angeben. 
   Wählen Sie **Standardantwort hinzufügen**.

   2. Fügen Sie auf der Seite **Aus Beispiel importieren** eine Beispielantwort ein, und wählen Sie dann **Importieren**.

   3. Geben Sie unter **Triggerkonfiguration** den Abfrageparameter, den an den Parameter zu übergebenden Wert und einen *Triggerhinweis* an, der ein geeignetes Abrufintervall für die nächste Anforderung angibt.

5. Überprüfen und beheben Sie schließlich unter **Validierung** mögliche Probleme, die für den Trigger ausgemacht wurden.

#### <a name="review-reference-definitions-for-your-connector"></a>Überprüfen von Verweisdefinitionen für den Connector

Der Abschnitt **Verweise** wird automatisch mithilfe Ihrer API-Beschreibung aufgefüllt und beschreibt Parameterfelder, auf die Aktionen und Trigger verweisen können. 

1. Wählen Sie unter **Verweise** die Nummer der Verweisdefinition aus, die Sie überprüfen möchten.

2. Überprüfen oder aktualisieren Sie unter **Name** den Namen der Verweisdefinition.

3. Überprüfen und beheben Sie schließlich unter **Validierung** mögliche Probleme, die für die Verweisdefinition ausgemacht wurden.

## <a name="3-finish-creating-your-connector"></a>3. Fertigstellen des Connectors

Wenn Sie soweit sind, wählen Sie **Erstellen**, damit Sie den Connector bereitstellen können. Wenn Sie einen vorhandenen Connector aktualisieren, wählen Sie **Connector aktualisieren**.

Glückwunsch! Wenn Sie nun eine Logik-App erstellen, können Sie Ihren Connector im Designer für Logik-Apps finden und diesen Connector Ihrer Logik-App hinzufügen.

![Ihr Connector im Designer für Logik-Apps](./media/logic-apps-custom-connector-register/custom-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Freigeben des Connectors für andere Logic Apps-Benutzer

Registrierte, aber nicht zertifizierte benutzerdefinierte Connectors funktionieren wie von Microsoft verwaltete Connectors. Sie sind jedoch *nur* für den Ersteller des Connectors und Benutzer sichtbar und verfügbar, die denselben Azure Active Directory-Mandanten und dasselbe Azure-Abonnement für Logik-Apps in der Region aufweisen, in der diese Apps bereitgestellt werden. Auch wenn die Freigabe optional ist, gibt es möglicherweise Szenarien, in denen Sie Ihre Connectors mit anderen Benutzern gemeinsam nutzen möchten. 

> [!IMPORTANT]
> Wenn Sie einen Connector freigeben, könnten andere Benutzer von diesem Connector abhängig werden. 
> ***Beim Löschen des Connectors werden alle Verbindungen mit diesem Connector gelöscht.***
 
Um Ihren Connector für externe Benutzer außerhalb dieser Grenzen freizugeben, z.B. für alle Benutzer von Logic Apps, Flow und PowerApps, [reichen Sie Ihren Connector für die Microsoft-Zertifizierung ein](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>Häufig gestellte Fragen

**F:** Gelten Grenzwerte für benutzerdefinierte Connectors? </br>
**A:** Siehe [die hier angegebenen Grenzwerte für benutzerdefinierte Connectors](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Support

* Wenn Sie Unterstützung bei der Entwicklung und Einbindung benötigen oder Funktionen anfordern möchten, die im Registrierungs-Assistenten nicht zur Verfügung stehen, wenden Sie sich bitte an [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com). Microsoft überwacht dieses Konto auf Entwicklerfragen und -probleme und leitet sie an das entsprechende Team weiter.

* Im [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) können Sie Fragen stellen und beantworten und sich über die Aktivitäten anderer Azure Logic Apps-Benutzer informieren.

* Zur Optimierung von Logic Apps können Sie auf der Website für [Benutzerfeedback zu Logic Apps](http://aka.ms/logicapps-wish) über Ideen abstimmen oder selbst Ideen einreichen. 

## <a name="next-steps"></a>Nächste Schritte

* [Optional: Zertifizieren Ihres Connectors](../logic-apps/custom-connector-submit-certification.md)
* [Häufig gestellte Fragen zu benutzerdefinierten Connectors](../logic-apps/custom-connector-faq.md)