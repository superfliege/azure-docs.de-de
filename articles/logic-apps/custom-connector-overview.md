---
title: "Übersicht über benutzerdefinierte Connectors – Azure Logic Apps | Microsoft-Dokumentation"
description: "Hier erhalten Sie eine Übersicht über benutzerdefinierte Connectors zur Unterstützung und Erweiterung von Integrationsszenarios."
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
ms.openlocfilehash: 0515b21bc7c7cc737e14fda016606bbea1aab476
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="custom-connectors-overview"></a>Übersicht über benutzerdefinierte Connectors

Sie können mit [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), [Microsoft Flow](https://flow.microsoft.com) oder [Microsoft PowerApps](https://powerapps.microsoft.com) Workflows oder Apps erstellen, ohne dafür Code schreiben zu müssen. Um Ihnen die Integration Ihrer Daten- und Unternehmensprozesse zu erleichtern, bieten diese Dienste [über 100 Connectors](../connectors/apis-list.md), und das nicht nur für Dienste und Produkte von Microsoft (wie Azure und SQL Server), sondern auch für andere Dienste (wie GitHub, Salesforce, Twitter und mehr). 

Manchmal empfiehlt es sich jedoch, APIs, Dienste und Systeme aufzurufen, die nicht als vordefinierte Connectors zur Verfügung stehen. Um Szenarios zu unterstützen, die besser auf das Unternehmen und die Produktivitätsansprüche Ihres Benutzers zugeschnitten sind, können Sie *benutzerdefinierte Connectors* mit eigenen Triggern und Aktionen erstellen.
Benutzerdefinierte Connectors erhöhen die Integration, Reichweite, Auffindbarkeit und Verwendung Ihrer Dienste und Produkte, was wiederum zu einer erhöhten Kundenakzeptanz führt.

Dieses Diagramm veranschaulicht beispielsweise die Interaktion zwischen einer Web-API, einem benutzerdefinierten Logic Apps-Connector, der für diese API erstellt wurde, und einer Logik-App, die mithilfe des benutzerdefinierten Connectors mit dieser API arbeitet:

![Konzeptuelle Übersicht über eine Web-API, einen benutzerdefinierten Connector und eine Logik-App](./media/custom-connector-overview/custom-connector-conceptual.png)

Dieser Überblick stellt die allgemeinen Aufgaben zum Erstellen, Sichern, Registrieren und Verwenden sowie Freigeben und Zertifizieren (optional) Ihrer Connectors auf höchster Ebene dar:

![Schritte zum Erstellen benutzerdefinierter Connectors](./media/custom-connector-overview/authoring-steps.png)

## <a name="prerequisites"></a>Voraussetzungen

Um Ihren Connector komplett selbst zu erstellen, benötigen Sie Folgendes:

* Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, erstellen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/). Registrieren Sie sich andernfalls [für ein Abonnement mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/pricing/purchase-options/).

* Eine RESTful-API mit authentifiziertem Zugriff. Weitere Informationen finden Sie unter [Create custom connectors from Web APIs (Erstellen benutzerdefinierter Connectors aus Web-APIs)](../logic-apps/custom-connector-build-web-api-app-tutorial.md). Weitere Informationen zu Mustern, die Sie für die Trigger und Aktionen Ihres Connectors verwenden können, finden Sie unter [Erstellen benutzerdefinierter APIs als Connectors für Logik-Apps](../logic-apps/logic-apps-create-api-app.md).

* Eines der folgenden Elemente:

  * Eine [OpenAPI 2.0-Datei](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md), früher als Swagger bezeichnet
  * Eine URL einer OpenAPI-Definition
  * Eine [Postman-Sammlung](../logic-apps/custom-connector-api-postman-collection.md) für Ihre API 

  Wenn Sie keines davon besitzen, helfen wir Ihnen weiter.

* Optional: Ein Bild, das Sie als Symbol für Ihren benutzerdefinierten Connector verwenden können

## <a name="1-build-your-restful-api"></a>1. Erstellen Ihrer RESTful-API

Genau genommen ist ein Connector ein Wrapper um eine REST-API, die auf einer OpenAPI-Spezifikation (früher Swagger) basiert, und mit der Ihr zugrundeliegender Dienst mit Logic Apps, Flow und PowerApps kommunizieren kann. Deshalb benötigen Sie zunächst eine vollständig funktionierende API, bevor Sie Ihren benutzerdefinierten Connector erstellen können. 

Für jede API können Sie jede beliebige Sprache und Plattform verwenden. Für Microsoft-Technologien wird eine der folgenden Plattformen empfohlen:

* [Azure-Funktionen](https://azure.microsoft.com/services/functions/)
* [Azure-Web-Apps](https://azure.microsoft.com/services/app-service/web/)
* [Azure API-Apps](https://azure.microsoft.com/services/app-service/api/)

In diesem Tutorial lernen Sie z.B., [wie Sie einen benutzerdefinierten Connector aus einer Web-API erstellen](../logic-apps/custom-connector-build-web-api-app-tutorial.md). Weitere Informationen zu Mustern, die Sie für die Trigger und Aktionen Ihres Connectors verwenden können, finden Sie unter [Erstellen benutzerdefinierter APIs als Connectors für Logik-Apps](../logic-apps/logic-apps-create-api-app.md).

## <a name="2-secure-your-api"></a>2. Sichern Ihrer API

Sie können folgende Authentifizierungsstandards für Ihre APIs und Connectors verwenden:

   * [OAuth 2.0](https://oauth.net/2/), einschließlich [Azure Active Directory](https://azure.microsoft.com/develop/identity/) oder spezifische Dienste, wie z.B. Dropbox, GitHub und Salesforce
   * Generisches OAuth 2.0
   * [Standardauthentifizierung](https://swagger.io/docs/specification/authentication/basic-authentication/)
   * [API-Schlüssel](https://swagger.io/docs/specification/authentication/api-keys/)

Sie können die Azure Active Directory-Authentifizierung (Azure AD) für Ihre API über das Azure-Portal einrichten, sodass Sie die Authentifizierung nicht mit Code implementieren müssen. Alternativ können Sie die Authentifizierung über den API-Code anfordern oder erzwingen. 

Weitere Informationen finden Sie in den entsprechenden Tutorials:

* [Logic Apps: Sichern Ihres benutzerdefinierten Connectors](../logic-apps/custom-connector-azure-active-directory-authentication.md)
* [Flow: Sichern Ihres benutzerdefinierten Connectors](https://ms.flow.microsoft.com/documentation/customapi-azure-resource-manager-tutorial/)
* [PowerApps: Sichern Ihres benutzerdefinierten Connectors](https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/)

## <a name="3-describe-your-api"></a>3. Beschreiben Ihrer API 

Wenn wir davon ausgehen, dass Ihre API über authentifizierten Zugriff verfügt, dann müssen Sie die Schnittstelle und Vorgänge Ihrer API beschreiben, damit Logic Apps, Flow oder PowerApps mit Ihrer API kommunizieren können.
Verwenden Sie eine dieser in der Branche gängigen Definitionen:

* Eine [OpenAPI 2.0-Datei](https://swagger.io/). Beginnen Sie mit dem Erstellen mithilfe einer vorhandenen OpenAPI-Datei.

  Falls Sie mit OpenAPI noch nicht vertraut sind, sehen Sie sich [Getting started with Swagger (Einführung in Swagger)](http://swagger.io/getting-started/) auf der swagger.io-Website an.
  Ein Beispiel für eine OpenAPI-Datei finden Sie in der [Dokumentation zur Textanalyse-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/export?DocumentFormat=Swagger&ApiName=Azure). 

* Eine Postman-Sammlung, die automatisch eine OpenAPI-Datei für Sie erzeugt. Wenn Sie noch keine OpenAPI-Datei besitzen, aber keine selbst erstellen möchten, können Sie einen benutzerdefinierten Connector ganz leicht mit einer Postman-Sammlung erstellen.

  Wenn Sie noch nicht mit Postman vertraut sind, [installieren Sie die Postman-App](https://www.getpostman.com/apps) von der Website. Weitere Informationen finden Sie unter [Describe custom connectors with Postman (Beschreiben benutzerdefinierter Connectors mit Postman)](../logic-apps/custom-connector-api-postman-collection.md).

> [!IMPORTANT]
> Die Größe Ihrer Datei muss weniger als 1 MB betragen.

Logic Apps, Flow und PowerApps machen Folgendes im Hintergrund: Sie verwenden OpenAPI, analysieren eine Postman-Sammlung und übersetzen die Sammlung in eine OpenAPI-Definitionsdatei. Auch wenn OpenAPI 2.0 und Postman-Sammlungen unterschiedliche Formate verwenden, sind beide sprachunabhängige, maschinenlesbare Dokumente, die die Vorgänge und Parameter Ihrer API beschreiben. Sie können diese Dokumente mit verschiedenen Tools erstellen, abhängig von der Sprache und Plattform, die für Ihre API verwendet wurden. Sie können auch eine OpenAPI-Datei erstellen, wenn Sie Ihren Connector registrieren.

Sie können z.B eine OpenAPI-Datei oder eine Postman-Sammlung von einem beliebigen REST-API-Endpunkt aus erstellen. Dazu zählen:

* Öffentlich verfügbare Connectors, wie z.B. [Spotify](https://developer.spotify.com/), [Slack](https://api.slack.com/), [Rackspace](http://docs.rackspace.com/) usw.

* Eine von Ihnen erstellte und an einen Cloudhostinganbieter bereitgestellte API, wie z.B. Azure, Heroku, Google Cloud und mehr

* Eine benutzerdefinierte Branchen-API, die in Ihrem Netzwerk bereitgestellt wird, aber nur, wenn diese API im öffentlichen Internet verfügbar gemacht wird

## <a name="4-register-your-connector"></a>4. Registrieren Ihres Connectors

Durch den Registrierungsprozess kann Logic Apps, Flow oder PowerApps die Merkmale Ihrer API besser verstehen, einschließlich der Beschreibung, der erforderlichen Authentifizierung und Vorgänge, was wiederum Parameter und Ausgaben für jeden Vorgang einschließt. Wenn Sie mit dem Registrierungsprozess beginnen, können Sie entweder eine OpenAPI-Datei oder eine Postman-Sammlung bereitstellen, die automatisch die Metadatenfelder im Registrierungsassistenten auffüllen. Die Werte dieser Felder können Sie jederzeit bearbeiten.

![Beschreiben Ihrer API](./media/custom-connector-overview/choose-api-definition-file.png)

Um Ihren Connector zu registrieren, schließen Sie das entsprechende Tutorial ab:

* [Logic Apps: Registrieren Ihres Connectors](../logic-apps/logic-apps-custom-connector-register.md)
* [Flow: Registrieren Ihres Connectors](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)
* [PowerApps: Registrieren Ihres Connectors](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector)

## <a name="5-use-your-connector-in-a-logic-app-flow-or-app"></a>5. Verwenden Ihres Connectors in einer Logik-App, einem Flow oder einer App 

Sie können Ihren Connector genauso wie von Microsoft verwaltete Connectors einsetzen. Fügen Sie z.B. in einem Logik-App-Workflow Ihren benutzerdefinierten Connector hinzu, damit Sie eine Verbindung mit Ihrer API herstellen können. Rufen Sie einen beliebigen Vorgang der API auf, genauso wie Sie auch einen von Microsoft verwalteten Connector aufrufen würden.

## <a name="6-share-your-connector"></a>6. Freigeben Ihres Connectors 

Sie können Ihren Connector für andere Benutzer in Ihrer Organisation freigeben. Dies funktioniert genauso wie das Freigeben von Ressourcen in Logic Apps, Flow und PowerApps. Auch wenn die Freigabe optional ist, gibt es möglicherweise Szenarios in denen Sie Ihre Connectors mit anderen Benutzern gemeinsam nutzen möchten.

Registrierte, aber nicht zertifizierte benutzerdefinierte Connectors funktionieren wie von Microsoft verwaltete Connectors, sind jedoch *nur* für den Ersteller des Connectors und Benutzer sichtbar und verfügbar, die denselben Azure Active Directory-Mandanten und dasselbe Azure-Abonnement für Logik-Apps in der Region aufweisen, in der diese Apps bereitgestellt werden. Im nächsten Schritt wird beschrieben, wie Sie Ihren Connector für externe Benutzer außerhalb dieser Grenzen freigeben, z.B. für alle Benutzer von Logic Apps, Flow und PowerApps.

> [!IMPORTANT]
> Wenn Sie einen Connector freigeben, kann es sein, dass andere Benutzer sich auf diesen Connector verlassen. 
> ***Beim Löschen des Connectors werden alle Verbindungen mit diesen Connector gelöscht.***

* [Logic Apps: Freigeben Ihres Connectors](../logic-apps/logic-apps-custom-connector-register.md)
* [Flow: Freigeben Ihres Connectors](https://ms.flow.microsoft.com/documentation/register-custom-api/#share-your-custom-connector)
* [PowerApps: Freigeben Ihres Connectors](https://powerapps.microsoft.com/tutorials/register-custom-api/#share-your-custom-connector)

## <a name="7-certify-your-connector"></a>7. Zertifizieren Ihres Connectors

Um Ihren Connector optional für alle Benutzer von Logic Apps, Flow und PowerApps freizugeben, reichen Sie Ihren Connector zur Zertifizierung durch Microsoft ein. In diesem Prozess wird Ihr Connector überprüft, auf technische und inhaltliche Konformität getestet sowie auf die Funktionstüchtigkeit in Logic Apps, Flow und PowerApps. Erst wenn er alle nötigen Kriterien erfüllt, wird er von Microsoft veröffentlicht. Erfahren Sie, [wie Sie Connectors für die Microsoft-Zertifizierung einreichen](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Support

* Unterstützung beim Onboarding und Entwickeln erhalten Sie unter folgender E-Mai-Adresse: [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com). Microsoft überwacht dieses Konto auf Entwicklerfragen und -probleme und leitet sie an das entsprechende Team weiter. 

* Häufig gestellte Fragen finden Sie in den [Häufig gestellten Fragen zu benutzerdefinierten Connectors](../logic-apps/custom-connector-faq.md).

## <a name="next-steps"></a>Nächste Schritte

* [Build a custom connector from a Web API (Erstellen eines benutzerdefinierten Connectors für eine Web-API)](../logic-apps/custom-connector-build-web-api-app-tutorial.md)
* [Set up authentication for custom connectors (Einrichten der Authentifizierung für benutzerdefinierte Connectors)](../logic-apps/custom-connector-azure-active-directory-authentication.md)
* [Describe custom connectors with Postman collections (Beschreiben benutzerdefinierter Connectors mit Postman-Sammlungen)](../logic-apps/custom-connector-api-postman-collection.md)
* [Submit custom connectors for Microsoft certification (Einreichen von Connectors für die Microsoft-Zertifizierung)](../logic-apps/custom-connector-submit-certification.md)
