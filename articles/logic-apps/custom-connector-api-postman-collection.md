---
title: "Beschreiben von benutzerdefinierten APIs und Connectors mit Postman – Azure Logic Apps | Microsoft-Dokumentation"
description: Erstellen Sie Postman-Sammlungen, um Ihre benutzerdefinierten APIs und Connectors zu beschreiben, zu gruppieren und zu organisieren.
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
ms.openlocfilehash: 072d544e5d29c4abb3d69651e53cfb33d5e0c9e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="describe-custom-apis-and-custom-connectors-with-postman"></a>Beschreiben von benutzerdefinierten APIs und benutzerdefinierten Connectors mit Postman

Um die Entwicklung von [benutzerdefinierten APIs](../logic-apps/logic-apps-create-api-app.md) und [benutzerdefinierten Connectors](../logic-apps/custom-connector-overview.md) schneller und einfacher zu gestalten, können Sie [Postman](https://www.getpostman.com/)-Sammlungen erstellen (anstatt OpenAPI-Dokumente), um Ihre APIs und Connectors zu beschreiben. Postman-Sammlungen dienen Ihnen als Hilfe beim Organisieren und Gruppieren von zusammengehörigen API-Anforderungen. Sie können Postman beispielsweise beim Erstellen von Connectors für Azure Logic Apps, Microsoft Flow oder Microsoft PowerApps verwenden. 

In diesem Tutorial wird veranschaulicht, wie Sie eine [Postman-Sammlung](https://www.getpostman.com/docs/postman/collections/creating_collections) erstellen, indem Sie die [API „Sprache erkennen“](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) in [Azure Cognitive Services Text Analytics](https://azure.microsoft.com/services/cognitive-services/text-analytics/) als Beispiel verwenden. Diese API identifiziert die Sprache, die Stimmung und Schlüsselwörter in dem Text, den Sie an die API übergeben.

## <a name="prerequisites"></a>Voraussetzungen

* [Installieren Sie die Postman-App](https://www.getpostman.com/apps), falls Sie Postman noch nicht genutzt haben.

* Ein Azure-Abonnement. Falls Sie nicht über ein Abonnement verfügen, können Sie [mit einem kostenlosen Azure-Konto beginnen](https://azure.microsoft.com/free/). Registrieren Sie sich andernfalls [für ein Abonnement mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/pricing/purchase-options/).

* Wenn Sie ein Azure-Abonnement besitzen, können Sie sich für die Textanalyse-APIs anmelden, indem Sie [hier Aufgabe 1 durchführen](../cognitive-services/text-analytics/how-tos/text-analytics-how-to-signup.md). 

## <a name="create-a-postman-collection"></a>Erstellen einer Postman-Sammlung

Bevor Sie eine Sammlung erstellen können, müssen Sie eine HTTP-Anforderung für Ihren API-Endpunkt erstellen. 

### <a name="create-an-http-request-for-your-api"></a>Erstellen einer HTTP-Anforderung für Ihre API

1. Öffnen Sie die Postman-App, damit Sie eine [HTTP-Anforderung](https://www.getpostman.com/docs/postman/sending_api_requests/requests) für Ihren API-Endpunkt erstellen können. Weitere Informationen finden Sie in der [Postman-Dokumentation zu Anforderungen](https://www.getpostman.com/docs/postman/sending_api_requests/requests).

   1. Wählen Sie auf der Registerkarte **Builder** die HTTP-Methode, geben Sie die Anforderungs-URL für Ihren API-Endpunkt ein, und wählen Sie ein Autorisierungsprotokoll aus (falls vorhanden). 
   Wählen Sie abschließend die Option **Params**.

      Für dieses Tutorial können Sie die Einstellungen in diesem Beispiel verwenden:

      ![Erstellen der Anforderung: „HTTP-Methode“, „Anforderungs-URL“, „Autorisierung“](./media/custom-connector-api-postman-collection/01-create-api-http-request.png)

      | Parameter | Empfohlener Wert | 
      | --------- | --------------- | 
      | **HTTP-Methode** | POST | 
      | **Anforderungs-URL** | `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages` | | 
      | **Autorisierung** | „No Auth“ (Keine Authentifizierung) | | 
      ||| 

   2. Sie können jetzt Schlüssel-Wert-Paare eingeben, um sie als Abfrage- oder Pfadparameter in der Anforderungs-URL zu verwenden. Postman kombiniert diese Elemente zusammen in einer Abfragezeichenfolge.
   Wählen Sie **Headers** aus, wenn Sie fertig sind.

      ![Fortsetzung der Anforderung: „Parameter“](./media/custom-connector-api-postman-collection/02-create-api-http-request-params.png)

      | Parameter | Empfohlener Wert | 
      | --------- | --------------- | 
      | **Params** | **Key**: „numberOfLanguagesToDetect“ </br>**Value**: „1“ | 
      ||| 

   3. Geben Sie Schlüssel-Wert-Paare für den Anforderungsheader ein. 
   Geben Sie eine beliebige Zeichenfolge als Headername ein. Häufig verwendete HTTP-Header können Sie in der Dropdownliste auswählen. Wählen Sie **Body** (Text), wenn Sie fertig sind. 
   
      ![Fortsetzung der Anforderung: „Headers“](./media/custom-connector-api-postman-collection/03-create-api-http-request-header.png)

      | Parameter | Wert | 
      | --------- | ----- | 
      | **Header** | **Key**: „Ocp-Apim-Subscription-Key“ </br>**Value**: *Ihr-API-Abonnementschlüssel*, den Sie in Ihrem Cognitive Services-Konto finden <p>**Key**: „Content-Type“ </br> **Value**: „application/json“ | 
      ||| 

   4. Geben Sie Inhalt ein, der im Anforderungstext gesendet werden soll. 
   Wählen Sie **Send** (Senden), um anhand einer Rückantwort zu prüfen, ob die Anforderung funktioniert. 
   
      ![Fortsetzung der Anforderung: „Body“ (Text)](./media/custom-connector-api-postman-collection/04-create-api-http-request-body.png)

      | Parameter | Empfohlener Wert | 
      | --------- | --------------- |    
      | **Text** | ```{"documents": [{ "id": "1", "text": "Hello World"}]}``` | 
      ||| 

      Das Antwortfeld enthält die vollständige Antwort der API, einschließlich des Ergebnisses oder, falls zutreffend, eines Fehlers.

      ![Erhalten einer Antwort auf die Anforderung](./media/custom-connector-api-postman-collection/05-create-api-http-request-response.png)

2. Nachdem Sie sichergestellt haben, dass Ihre Anforderung funktioniert, können Sie sie in einer Postman-Sammlung speichern. 

   1. Wählen Sie **Speichern** aus. 
      
      ![Auswählen von „Save“ (Speichern)](./media/custom-connector-api-postman-collection/06a-save-request.png)
 
   2. Geben Sie unter **Save Request** (Anforderung speichern) einen **Request Name** (Anforderungsnamen) und optional eine **Request description** (Anforderungsbeschreibung) ein. 

      > [!NOTE]
      > Diese Werte werden später von Ihrem benutzerdefinierten Connector verwendet. Achten Sie also darauf, dass Sie die gleichen Werte angeben, die Sie später für die Zusammenfassung und Beschreibung des Betriebs Ihrer benutzerdefinierten API verwenden.

   3. Wählen Sie **+ Create Collection** (+ Sammlung erstellen), und geben Sie einen Namen für die Sammlung an. 
   Wählen Sie das Häkchen aus, um einen Sammlungsordner zu erstellen, und wählen Sie dann **Save to *your-Postman-collection-name*** (Speichern unter Name-Ihrer-Postman-Sammlung).

      ![Speichern der Anforderung](./media/custom-connector-api-postman-collection/06b-save-request.png)

### <a name="save-the-request-response"></a>Speichern der Anforderungsantwort

Nachdem Sie Ihre Anforderung gespeichert haben, können Sie die Antwort speichern. Die Antwort wird dann als Beispiel angezeigt, wenn Sie die Anforderung später laden.

1. Wählen Sie oberhalb des Antwortfensters die Option **Save Response** (Antwort speichern). 

   ![Speichern der Antwort](./media/custom-connector-api-postman-collection/07-create-api-http-request-save-response.png)

   Benutzerdefinierte Connectors unterstützen nur eine Antwort pro Anforderung. 
   Wenn Sie mehrere Antworten pro Anforderung speichern, wird nur die erste Antwort verwendet.

2. Geben Sie einen Namen für Ihr Beispiel an, und wählen Sie **Save Example** (Beispiel speichern).

3. Erweitern Sie Ihre Postman-Sammlung um weitere Anforderungen und Antworten.

### <a name="export-your-postman-collection"></a>Exportieren Ihrer Postman-Sammlung

1. Wenn Sie fertig sind, können Sie Ihre Sammlung als JSON-Datei exportieren.

   ![Exportieren der Sammlung](./media/custom-connector-api-postman-collection/08-export-http-request.png)

2. Wählen Sie das Exportformat **Collection v1** (Sammlung v1) aus, und navigieren Sie an den Speicherort, an dem Sie die JSON-Datei speichern möchten.

   > [!NOTE]
   > Derzeit funktioniert nur v1 für benutzerdefinierte Connectors.

   ![Auswählen des Exportformats: „Collection v1“ (Sammlung v1)](./media/custom-connector-api-postman-collection/09-export-format.png)
   
Sie können diese Postman-Sammlung jetzt zum Erstellen von benutzerdefinierten APIs und Connectors verwenden. Nachdem Sie die Sammlung exportiert haben, können Sie die JSON-Datei in Logic Apps, Flow oder PowerApps importieren.

> [!IMPORTANT]
> Wenn Sie einen Connector aus einer Postman-Sammlung erstellen, sollten Sie darauf achten, dass Sie den Header `Content-type` aus Aktionen und Triggern entfernen. Der Zieldienst, z.B. Flow, fügt diesen Header automatisch hinzu. Entfernen Sie außerdem die Authentifizierungsheader im Abschnitt `securityDefintions` aus Aktionen und Triggern.

## <a name="next-steps"></a>Nächste Schritte

* [Registrieren benutzerdefinierter Connectors in Azure Logic Apps](../logic-apps/logic-apps-custom-connector-register.md)
* [Flow: Registrieren Ihres Connectors](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)
* [PowerApps: Registrieren Ihres Connectors](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector)
* [Häufig gestellte Fragen zu benutzerdefinierten Connectors](../logic-apps/custom-connector-faq.md)
