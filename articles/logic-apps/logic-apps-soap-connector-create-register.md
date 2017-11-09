---
title: "Erstellen und Registrieren von SOAP-Connectors – Azure Logic Apps | Microsoft-Dokumentation"
description: "Informationen zum Einrichten von SOAP-Connectors für die Nutzung in Azure Logic Apps"
author: divyaswarnkar
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
ms.date: 10/24/2017
ms.author: LADocs; divswa
ms.openlocfilehash: 2d4d009dfc4d43ccc3c69bb3da15ca2c478b9efe
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="create-and-register-soap-connectors-in-azure-logic-apps"></a>Erstellen und Registrieren von SOAP-Connectors in Azure Logic Apps

Um SOAP-Dienste in Ihre Logik-App-Workflows zu integrieren, können Sie einen benutzerdefinierten Simple Object Access-Protokoll-Connector (SOAP) mit der Web Services Description Language (WSDL), die Ihren SOAP-Dienst beschreibt, erstellen und registrieren. Die SOAP-Connectors funktionieren wie vorgefertigte Connectors, sodass Sie sie wie andere Connectors in Ihren Logik-Apps verwenden können.


## <a name="prerequisites"></a>Voraussetzungen

Um Ihren SOAP-Connector registrieren zu können, benötigen Sie diese Elemente:

* Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, erstellen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/). Registrieren Sie sich andernfalls [für ein Abonnement mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/pricing/purchase-options/).

* Eines der folgenden Elemente:
  * Eine URL zu einer WSDL-Datei, die Ihren SOAP-Dienst und die APIs definiert
  * Eine WSDL-Datei, die Ihren SOAP-Dienst und die APIs definiert

  Für dieses Tutorial können Sie unser Beispiel [Orders SOAP Service](http://fazioapisoap.azurewebsites.net/FazioService.svc?singleWsdl) (Aufträge SOAP-Dienst) verwenden.

* Optional: Ein Bild, das Sie als Symbol für Ihren benutzerdefinierten Connector verwenden können


## <a name="1-create-your-connector"></a>1. Erstellen des Connectors

1. Wählen Sie im Azure-Portal im Azure-Hauptmenü **Neu** aus. Geben Sie in das Suchfeld „Logik Apps-Connector“ ein, und drücken Sie die EINGABETASTE.

   ![Neu, nach „Logic Apps-Connector“ suchen](./media/logic-apps-soap-connector-create-register/create-logic-apps-connector.png)

2. Wählen Sie in der Ergebnisliste **Logic Apps-Connector** > **Erstellen** aus.

   ![Erstellen eines Logic Apps-Connectors](./media/logic-apps-soap-connector-create-register/choose-logic-apps-connector.png)

3. Geben Sie Details für die Registrierung Ihres Connectors an, wie in der Tabelle beschrieben. Wenn Sie fertig sind, wählen Sie **An Dashboard anheften** > **Erstellen** aus.

   ![Details eines benutzerdefinierten Logik-App-Connectors](./media/logic-apps-soap-connector-create-register/logic-apps-soap-connector-details.png)

   | Eigenschaft | Empfohlener Wert | Beschreibung | 
   | -------- | --------------- | ----------- | 
   | **Name** | *soap-connector-name* | Geben Sie einen Namen für den Connector an. | 
   | **Abonnement** | *Name des Azure-Abonnements* | Wählen Sie Ihr Azure-Abonnement. | 
   | **Ressourcengruppe** | *Name der Azure-Ressourcengruppe* | Erstellen Sie eine Azure-Ressourcengruppe zum Organisieren Ihrer Azure-Ressourcen, oder wählen Sie eine aus. | 
   | **Standort** | *Bereitstellungsregion* | Wählen Sie eine Bereitstellungsregion für den Connector aus. | 
   |||| 

   Nachdem Azure den Connector bereitgestellt hat, wird das Menü des Logik-Apps-Connectors automatisch geöffnet. 
   Falls nicht, wählen Sie Ihren SOAP-Connector im Azure-Dashboard aus.

## <a name="2-define-your-connector"></a>2. Definieren Ihres Connectors

Geben Sie jetzt die WSDL-Datei oder URL zum Erstellen Ihres Connectors, die von Ihrem Connector verwendete Authentifizierung sowie die Aktionen und Trigger an, die Ihr SOAP-Connector bietet


### <a name="2a-specify-the-wsdl-file-or-url-for-your-connector"></a>2a. Geben Sie die WSDL-Datei oder URL für Ihren Connector an

1. Wählen Sie im Menü Ihres Connectors, falls nicht bereits ausgewählt, **Logic Apps-Connector**. Klicken Sie auf der Symbolleiste auf **Bearbeiten**.

   ![Bearbeiten eines benutzerdefinierten Connectors](./media/logic-apps-soap-connector-create-register/edit-soap-connector.png)

2. Wählen Sie **Allgemein**, damit Sie die Details in diesen Tabellen zum Erstellen, Schützen und Definieren der Aktionen und Trigger für Ihren SOAP-Connector angeben können.

   1. Wählen Sie für **Benutzerdefinierte Connectors** die Option **SOAP** für Ihren **API-Endpunkt** aus, damit Sie die WSDL-Datei bereitstellen können, die Ihre API beschreibt.

      ![Bereitstellen der WSDL-Datei für Ihre API](./media/logic-apps-soap-connector-create-register/provide-wsdl-file.png)

      | Option | Format |Beschreibung | 
      | ------ | ------ | ----------- | 
      | **WSDL aus Datei hochladen** | *WSDL-Datei* | Navigieren Sie zum Speicherort der WSDL-Datei, und wählen Sie diese Datei aus. | 
      | **WSDL über URL hochladen** | http://*Pfad-zu-WSDL-Datei* | Geben Sie die URL für die WSDL-Datei Ihres Diensts an. | 
      | **SOAP nach REST** |   | Wandeln Sie APIs im SOAP-Dienst in REST-APIs um. | 
      |||| 

   2. Laden Sie in **Allgemeine Informationen** ein Symbol Ihres Connectors hoch. 
   In der Regel werden die Felder **Beschreibung**, **Host** und **Basis-URL** automatisch anhand Ihrer WSDL-Datei ausgefüllt. 
   Falls nicht, fügen Sie diese Informationen wie in der Tabelle beschrieben hinzu, und klicken Sie auf **Weiter**. 

      ![Connectordetails](./media/logic-apps-soap-connector-create-register/add-general-details.png)

      | Option oder Einstellung | Format | Beschreibung | 
      | ----------------- | ------ | ----------- | 
      | **Symbol hochladen** | *PNG-oder JPG-Datei kleiner als 1 MB* | Ein Symbol, das Ihren Connector darstellt <p>Farbe: Vorzugsweise ein weißes Logo auf farbigem Hintergrund. <p>Ein Logo mit ca. 160 Pixeln innerhalb eines Quadrats mit 230 Pixeln | 
      | **Symbolhintergrundfarbe** | *Hexadezimalcode der Brandingfarbe des Symbols* | <p>Die Farbe hinter dem Symbol, die der Hintergrundfarbe in Ihrer Symboldatei entspricht. <p>Format: Hexadezimal. „#007ee5“ stellt beispielsweise die Farbe Blau dar. | 
      | **Beschreibung** | *Connector-Beschreibung* | Geben Sie eine Kurzbeschreibung des Connectors an. | 
      | **Host** | *Connectorhost* | Geben Sie die Hostdomäne für Ihren SOAP-Dienst an. | 
      | **Basis-URL** | *Basis-URL des Connectors* | Geben Sie die Basis-URL für Ihren SOAP-Dienst an. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Beschreiben der vom Connector verwendeten Authentifizierung

1. Wählen Sie nun **Sicherheit**, damit Sie die Authentifizierung, die Ihr Connector verwendet, überprüfen oder beschreiben können. Die Authentifizierung stellt sicher, dass die Identitäten Ihrer Benutzer zwischen Ihrem Dienst und beliebigen Clients ordnungsgemäß übertragen werden.

   Standardmäßig ist der **Authentifizierungstyp** des Connectors auf **Keine Authentifizierung** festgelegt.
   
   ![Authentifizierungsart](./media/logic-apps-soap-connector-create-register/security-authentication-options.png)

   Um den Authentifizierungstyp zu ändern, wählen Sie **Bearbeiten**. Sie können **Standardauthentifizierung** auswählen. Um statt der Standardwerte andere Parameterbezeichnungen zu verwenden, aktualisieren Sie sie unter **Parameterbezeichnung**.

   ![Standardauthentifizierung](./media/logic-apps-soap-connector-create-register/security.png)

   
2. Um Ihren Connector nach Eingabe der Sicherheitsinformationen zu speichern, wählen Sie oben auf der Seite **Connector aktualisieren** und anschließend **Weiter** aus. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Überprüfen, Aktualisieren oder Definieren von Aktionen und Triggern für Ihren Connector

1. Wählen Sie nun **Definition**, um neue Aktionen und Trigger zu überprüfen, zu bearbeiten oder zu definieren, die Benutzer ihren Workflows hinzufügen können.

   Aktionen und Trigger basieren auf den Vorgängen, die in Ihrer WSDL-Datei definiert sind und automatisch die Seite **Definition** mit den Anforderungs- und Antwortwerten auffüllen. Wenn hier also bereits die gewünschten Vorgänge angezeigt werden, können Sie zum nächsten Schritt im Registrierungsprozess übergehen, ohne Änderungen auf dieser Seite vorzunehmen.

   ![Connectordefinition](./media/logic-apps-soap-connector-create-register/definition.png)

2. Wenn Sie optional vorhandene Aktionen und Trigger bearbeiten oder neue hinzufügen möchten, [fahren Sie mit diesen Schritten fort](logic-apps-custom-connector-register.md#add-action-or-trigger).


## <a name="3-finish-creating-your-connector"></a>3. Fertigstellen des Connectors

Wählen Sie nach Fertigstellen des Connectors **Connector aktualisieren**, damit Sie den Connector bereitstellen können. 

Glückwunsch! Wenn Sie nun eine Logik-App erstellen, können Sie Ihren Connector im Designer für Logik-Apps finden und diesen Connector Ihrer Logik-App hinzufügen.

![Ihr Connector im Designer für Logik-Apps](./media/logic-apps-soap-connector-create-register/soap-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Freigeben des Connectors für andere Logic Apps-Benutzer

Registrierte, aber nicht zertifizierte benutzerdefinierte Connectors funktionieren wie von Microsoft verwaltete Connectors. Sie sind jedoch *nur* für den Ersteller des Connectors und Benutzer sichtbar und verfügbar, die denselben Azure Active Directory-Mandanten und dasselbe Azure-Abonnement für Logik-Apps in der Region aufweisen, in der diese Apps bereitgestellt werden. Auch wenn die Freigabe optional ist, gibt es möglicherweise Szenarien, in denen Sie Ihre Connectors mit anderen Benutzern gemeinsam nutzen möchten. 

> [!IMPORTANT]
> Wenn Sie einen Connector freigeben, könnten andere Benutzer von diesem Connector abhängig werden. 
> ***Beim Löschen des Connectors werden alle Verbindungen mit diesem Connector gelöscht.***
 
Um Ihren Connector für externe Benutzer außerhalb dieser Grenzen freizugeben, z.B. für alle Benutzer von Logic Apps, [reichen Sie Ihren Connector für die Microsoft-Zertifizierung ein](../logic-apps/custom-connector-submit-certification.md).

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