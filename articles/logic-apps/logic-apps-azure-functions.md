---
title: Hinzufügen und Ausführen von benutzerdefiniertem Code in Azure Logic Apps mit Azure Functions | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie benutzerdefinierte Codeausschnitte in Azure Logic Apps mit Azure Functions hinzufügen und ausführen.
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 07/25/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 20ad738541554279ff9fd6dd6babe90a38676c00
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263189"
---
# <a name="add-and-run-custom-code-snippets-in-azure-logic-apps-with-azure-functions"></a>Hinzufügen und Ausführen von benutzerdefinierten Codeausschnitten in Azure Logic Apps mit Azure Functions

Wenn Sie nur so viel Code erstellen und ausführen möchten, wie für die Behebung eines bestimmten Problems in Ihren Logik-Apps benötigt wird, können Sie mit [Azure Functions](../azure-functions/functions-overview.md) Ihre eigenen Funktionen erstellen. Dieser Dienst verfügt über eine Funktion zum Erstellen und Ausführen von benutzerdefinierten Codeausschnitten, die in Ihren Logik-Apps mit Node.js oder C# geschrieben werden. So müssen Sie sich nicht um die Erstellung einer gesamten App oder um die Infrastruktur zur Ausführung Ihres Codes kümmern. Azure Functions ermöglicht serverloses Computing in der Cloud und ist nützlich, um Aufgaben durchzuführen, z.B. diese Beispiele:

* Erweitern Sie das Verhalten Ihrer Logik-App um Funktionen, die von Node.js oder C# unterstützt werden.
* Führen Sie in Ihrem Logik-App-Workflow Berechnungen durch.
* Wenden Sie in Ihren Logik-Apps die erweiterte Formatierung oder Computefelder an.

Sie können [Logik-Apps auch aus Azure-Funktionen aufrufen](#call-logic-app).

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie diesem Artikel folgen können, benötigen Sie die folgenden Komponenten:

* Wenn Sie noch kein Azure-Abonnement haben, <a href="https://azure.microsoft.com/free/" target="_blank">melden Sie sich für ein kostenloses Azure-Konto an</a>. 

* Logik-App, der Sie die Funktion hinzufügen möchten

  Wenn Sie noch nicht mit Logik-Apps vertraut sind, lesen Sie [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten Logik-App-Workflows](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Ein [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), der als erster Schritt in Ihrer Logik-App verwendet wird. 

  Bevor Sie Aktionen zum Ausführen von Funktionen hinzufügen können, muss Ihre Logik-App über einen Trigger gestartet werden.

* Eine Azure-Funktionen-App, bei der es sich um einen Container für Azure-Funktionen handelt, und Ihre Azure-Funktion. Wenn Sie nicht über eine Funktionen-App verfügen, müssen Sie [zuerst Ihre Funktionen-App erstellen](../azure-functions/functions-create-first-azure-function.md). Sie können Ihre Funktion dann entweder [separat außerhalb Ihrer Logik-App](#create-function-external) oder [aus Ihrer Logik-App](#create-function-designer) im Logik-App-Designer erstellen.

  Sowohl für neue als auch für vorhandene Azure-Funktionen-Apps gelten die gleichen Anforderungen in Bezug auf die Arbeit mit Ihren Logik-Apps:

  * Ihre Funktionen-App muss demselben Azure-Abonnement wie Ihre Logik-App angehören.

  * Für Ihre Funktion muss die Funktionsvorlage **Generischer Webhook** verwendet werden – entweder für **JavaScript** oder für **C#**. Diese Vorlage kann Inhalte mit dem Typ `application/json` von Ihrer Logik-App akzeptieren. Diese Vorlagen unterstützen den Logik-App-Designer auch beim Suchen und Anzeigen der benutzerdefinierten Funktionen, die Sie mit diesen Vorlagen erstellen, wenn Sie diese Funktionen Ihren Logik-Apps hinzufügen.

  * Stellen Sie sicher, dass die **Mode**-Eigenschaft Ihrer Funktionsvorlage auf **Webhook** und die **Webhook type**-Eigenschaft auf **Generic JSON** festgelegt ist.

    1. Melden Sie sich beim <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> an.
    2. Wählen Sie im Hauptmenü von Azure die Option **Funktionen-Apps**. 
    3. Wählen Sie in der Liste **Funktionen-Apps** Ihre Funktionen-App aus, erweitern Sie die Funktion, und wählen Sie **Integrieren**. 
    4. Stellen Sie sicher, dass die **Mode**-Eigenschaft Ihrer Vorlage auf **Webhook** und die **Webhook type**-Eigenschaft auf **Generic JSON** festgelegt ist. 

  * Falls Ihre Funktion über eine [API-Definition](../azure-functions/functions-openapi-definition.md) verfügt (früher als [Swagger-Datei](http://swagger.io/) bezeichnet), wird im Logik-App-Designer eine umfassendere Benutzeroberfläche für die Arbeit mit Funktionsparametern bereitgestellt. 
  Bevor Ihre Logik-App nach Funktionen mit Swagger-Beschreibungen suchen und diese finden kann, [müssen Sie Ihre Funktionen-App mit diesen Schritten einrichten](#function-swagger).

<a name="create-function-external"></a>

## <a name="create-functions-outside-logic-apps"></a>Erstellen von Funktionen außerhalb von Logik-Apps

Erstellen Sie im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> Ihre Azure-Funktionen-App, die über dasselbe Azure-Abonnement wie Ihre Logik-App verfügen muss, und erstellen Sie dann Ihre Azure-Funktion. Falls Sie noch nicht mit Azure Functions vertraut sind, können Sie sich darüber informieren, wie Sie [Ihre erste Funktion im Azure-Portal erstellen](../azure-functions/functions-create-first-azure-function.md). Beachten Sie aber diese Anforderungen zum Erstellen von Azure-Funktionen, die Sie hinzufügen und aus Logik-Apps aufrufen können.

* Stellen Sie sicher, dass Sie die Funktionsvorlage **Generischer Webhook** für **JavaScript** oder **C#** auswählen.

  ![Generischer Webhook – JavaScript oder C#](./media/logic-apps-azure-functions/generic-webhook.png)

* Nachdem Sie Ihre Azure-Funktion erstellt haben, sollten Sie überprüfen, dass die Eigenschaften **Mode** und **Webhook type** der Vorlage richtig festgelegt sind.

  1. Erweitern Sie in der Liste mit den **Funktionen-Apps** Ihre Funktion, und wählen Sie **Integrieren**. 

  2. Stellen Sie sicher, dass die **Mode**-Eigenschaft Ihrer Vorlage auf **Webhook** und die **Webhook type**-Eigenschaft auf **Generic JSON** festgelegt ist. 

     ![„Integrieren“-Eigenschaften Ihrer Funktionsvorlage](./media/logic-apps-azure-functions/function-integrate-properties.png)

<a name="function-swagger"></a>

* (Optional) Wenn Sie [eine API-Definition generieren](../azure-functions/functions-openapi-definition.md) (früher als [Swagger-Datei](http://swagger.io/) bezeichnet), können Sie für Ihre Funktion eine umfassendere Benutzeroberfläche nutzen, wenn Sie im Logik-App-Designer mit Funktionsparametern arbeiten. Richten Sie Ihre Funktionen-App wie folgt ein, damit Ihre Logik-App Funktionen finden und darauf zugreifen kann, die über Swagger-Beschreibungen verfügen:

  * Stellen Sie sicher, dass Ihre Funktionen-App aktiv ausgeführt wird.

  * Richten Sie in Ihrer Funktionen-App die [Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) ein, damit alle Ursprünge zulässig sind:

    1. Wählen Sie über die Liste mit den **Funktionen-Apps** Ihre Funktionen-App und dann **Plattformfeatures** > **CORS** aus.

       ![Auswählen von Funktionen-App > Plattformfeatures > CORS](./media/logic-apps-azure-functions/function-platform-features-cors.png)

    2. Fügen Sie unter **CORS** das Platzhalterzeichen `*` hinzu, aber entfernen Sie alle anderen Ursprünge in der Liste, und wählen Sie **Speichern**.

       ![Auswählen von Funktionen-App > Plattformfeatures > CORS](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

### <a name="access-property-values-inside-http-requests"></a>Zugreifen auf Eigenschaftswerte in HTTP-Anforderungen

Webhook-Funktionen können HTTP-Anforderungen als Eingaben akzeptieren und diese Anforderungen an andere Funktionen übergeben. Obwohl Logic Apps beispielsweise über [Funktionen zum Konvertieren von DateTime-Werten](../logic-apps/workflow-definition-language-functions-reference.md) verfügt, wird in diesem einfachen Beispiel für eine JavaScript-Funktion Folgendes veranschaulicht: Zugreifen auf eine Eigenschaft in einem Anforderungsobjekt, das an die Funktion übergeben wird, und Durchführen von Vorgängen für diesen Eigenschaftswert. Für den Zugriff auf Eigenschaften in Objekten wird in diesem Beispiel der Operator [Punkt (.)](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Property_accessors) verwendet: 

```javascript
function convertToDateString(request, response){
   var data = request.body;
   response = {
      body: data.date.ToDateString();
   }
}
```

In dieser Funktion läuft Folgendes ab:

1. Die Funktion erstellt die Variable `data` und weist das `body`-Objekt im `request`-Objekt dieser Variablen zu. In der Funktion wird der Operator „Punkt“ (.) eingesetzt, um auf das `body`-Objekt im `request`-Objekt zu verweisen: 

   ```javascript
   var data = request.body;
   ```

2. Die Funktion kann nun über die Variable `data` auf die `date`-Eigenschaft zugreifen und diesen Eigenschaftswert aus dem DateTime-Typ in den DateString-Typ konvertieren, indem die Funktion `ToDateString()` aufgerufen wird. Die Funktion gibt auch das Ergebnis über die `body`-Eigenschaft in der Antwort der Funktion zurück: 

   ```javascript
   body: data.date.ToDateString();
   ```

Nachdem Sie nun Ihre Azure-Funktion erstellt haben, können Sie die Schritte zum [Hinzufügen von Funktionen zu Logik-Apps](#add-function-logic-app) ausführen.

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>Erstellen von Funktionen innerhalb von Logik-Apps

Bevor Sie eine Azure-Funktion erstellen können, indem Sie in Ihrer Logik-App im Logik-App-Designer beginnen, müssen Sie zuerst über eine Azure-Funktionen-App verfügen. Hierbei handelt es sich um einen Container für Ihre Funktionen. Erstellen Sie zuerst diese Funktionen-App, falls sie noch nicht vorhanden ist. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Funktion im Azure-Portal](../azure-functions/functions-create-first-azure-function.md). 

1. Öffnen Sie Ihre Logik-App über das <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> im Logik-App-Designer. 

2. Wählen Sie im Schritt zum Erstellen und Hinzufügen der Funktion **Neuer Schritt** > **Aktion hinzufügen**. 

3. Geben Sie im Suchfeld „azure functions“ als Filter ein.
Wählen Sie in der Liste mit den Aktionen die Aktion **Azure-Funktion wählen – Azure Functions** aus. 

   ![Suchen nach „Azure Functions“](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. Wählen Sie in der Liste mit den Funktionen-Apps Ihre Funktionen-App aus. Wählen Sie nach dem Öffnen der Liste mit den Aktionen die Aktion **Azure Functions – Neue Funktion erstellen** aus.

   ![Auswählen Ihrer Funktionen-App](./media/logic-apps-azure-functions/select-function-app-create-function.png)

5. Definieren Sie im Editor für Funktionsdefinitionen Ihre Funktion:

   1. Geben Sie im Feld **Funktionsname** einen Namen für Ihre Funktion an. 

   2. Fügen Sie im Feld **Code** der Vorlage Ihren Funktionscode hinzu. Binden Sie auch die Antwort und Nutzlast ein, die für Ihre Logik-App zurückgegeben werden soll, nachdem die Ausführung Ihrer Funktion abgeschlossen ist. 
   Mit dem Kontextobjekt im Vorlagencode werden die Nachricht und der Inhalt beschrieben, die bzw. der von Ihrer Logik-App an Ihre Funktion übergeben wird, z.B.:

      ![Definieren Ihrer Funktion](./media/logic-apps-azure-functions/function-definition.png)

      In Ihrer Funktion können Sie auf die Eigenschaften im Kontextobjekt mit dieser Syntax verweisen:

      ```text
      context.<token-name>.<property-name>
      ```
      Für dieses Beispiel lautet die Syntax wie folgt:

      ```text
      context.body.content
      ```

   3. Wenn Sie fertig sind, wählen Sie **Erstellen** aus.

6. Geben Sie im Feld **Anforderungstext** das Kontextobjekt an, das als Eingabe für Ihre Funktion übergeben werden soll. Hierfür muss das JSON-Format (JavaScript Object Notation) verwendet werden. Wenn Sie in das Feld **Anforderungstext** klicken, wird die Liste mit dem dynamischen Inhalt geöffnet, und Sie können Token für Eigenschaften auswählen, die aus vorherigen Schritten verfügbar sind. 

   In diesem Beispiel wird das Objekt im **Body**-Token aus dem E-Mail-Trigger übergeben:  

   ![Beispiel „Anforderungstext“ –Nutzlast des Kontextobjekts](./media/logic-apps-azure-functions/function-request-body-example.png)

   Basierend auf dem Inhalt im Kontextobjekt generiert der Logik-App-Designer eine Funktionsvorlage, die Sie dann inline bearbeiten können. 
   Logic Apps erstellt Variablen auch basierend auf dem Eingabekontextobjekt.

   In diesem Beispiel wird das Kontextobjekt nicht in eine Zeichenfolge umgewandelt, sodass der Inhalt direkt der JSON-Nutzlast hinzugefügt wird. 
   Falls das Objekt aber kein JSON-Token ist, wobei es sich um eine Zeichenfolge, ein JSON-Objekt oder ein JSON-Array handeln muss, erhalten Sie einen Fehler. 
   Fügen Sie doppelte Anführungszeichen hinzu, um das Kontextobjekt in eine Zeichenfolge umzuwandeln, z.B.:

   ![Umwandeln eines Objekts in eine Zeichenfolge](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

7. Wählen Sie **Erweiterte Optionen anzeigen**, um andere Details anzugeben, z.B. die zu verwendende Methode, Anforderungsheader oder Abfrageparameter.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Hinzufügen von vorhandenen Funktionen zu Logik-Apps

Zum Aufrufen von vorhandenen Azure-Funktionen aus Ihren Logik-Apps können Sie Azure-Funktionen wie alle anderen Aktionen im Logik-App-Designer hinzufügen. 

1. Öffnen Sie Ihre Logik-App über das <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> im Logik-App-Designer. 

2. Wählen Sie im Schritt zum Hinzufügen der Funktion **Neuer Schritt** > **Aktion hinzufügen**. 

3. Geben Sie im Suchfeld „azure functions“ als Filter ein.
Wählen Sie in der Liste mit den Aktionen die Aktion **Azure-Funktion wählen – Azure Functions** aus. 

   ![Suchen nach „Azure Functions“](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. Wählen Sie in der Liste mit den Funktionen-Apps Ihre Funktionen-App aus. Wählen Sie Ihre Funktion aus, wenn die Liste mit den Funktionen angezeigt wird. 

   ![Auswählen Ihrer Funktionen-App und Azure-Funktion](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   Für Funktionen, die über API-Definitionen (Swagger-Beschreibungen) verfügen und [so eingerichtet sind, dass Ihre Logik-App diese Funktionen finden und darauf zugreifen kann](#function-swagger), können Sie **Swagger-Aktionen** auswählen:

   ![Auswählen von Funktionen-App, „Swagger-Aktionen“ und Azure-Funktion](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

5. Geben Sie im Feld **Anforderungstext** das Kontextobjekt an, das als Eingabe für Ihre Funktion übergeben werden soll. Hierfür muss das JSON-Format (JavaScript Object Notation) verwendet werden. Mit diesem Kontextobjekt werden die Nachricht und der Inhalt beschrieben, die bzw. der von Ihrer Logik-App an Ihre Funktion gesendet wird. 

   Wenn Sie in das Feld **Anforderungstext** klicken, wird die Liste mit dem dynamischen Inhalt geöffnet, und Sie können Token für Eigenschaften auswählen, die aus vorherigen Schritten verfügbar sind. 
   In diesem Beispiel wird das Objekt im **Body**-Token aus dem E-Mail-Trigger übergeben:

   ![Beispiel „Anforderungstext“ –Nutzlast des Kontextobjekts](./media/logic-apps-azure-functions/function-request-body-example.png)

   In diesem Beispiel wird das Kontextobjekt nicht in eine Zeichenfolge umgewandelt, sodass der Inhalt direkt der JSON-Nutzlast hinzugefügt wird. 
   Falls das Objekt aber kein JSON-Token ist, wobei es sich um eine Zeichenfolge, ein JSON-Objekt oder ein JSON-Array handeln muss, erhalten Sie einen Fehler. 
   Fügen Sie doppelte Anführungszeichen hinzu, um das Kontextobjekt in eine Zeichenfolge umzuwandeln, z.B.:

   ![Umwandeln eines Objekts in eine Zeichenfolge](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

6. Wählen Sie **Erweiterte Optionen anzeigen**, um andere Details anzugeben, z.B. die zu verwendende Methode, Anforderungsheader oder Abfrageparameter.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-functions"></a>Aufrufen von Logik-Apps aus Funktionen

Um eine Logik-App aus einer Azure-Funktion auszulösen, muss diese Logik-App über einen aufrufbaren Endpunkt verfügen – genauer gesagt über einen **Anforderungstrigger**. Senden Sie dann aus Ihrer Funktion eine HTTP POST-Anforderung an die URL für diesen **Anforderungstrigger**, und binden Sie die Nutzlast ein, die von dieser Logik-App verarbeitet werden soll. Weitere Informationen hierzu finden Sie unter [Aufrufen, Auslösen oder Schachteln von Logik-Apps](../logic-apps/logic-apps-http-endpoint.md). 

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Logic Apps-Connectors](../connectors/apis-list.md).
