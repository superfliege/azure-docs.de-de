---
title: Transformieren und Schützen Ihrer API mit Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre API mithilfe von Richtlinien für Kontingente und Drosselung (Beschränken der Aufrufhäufigkeit) schützen.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: bebfabfa2c9012fa55bfc6964dc0b638cb7ab3f1
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="transform-and-protect-your-api"></a>Transformieren und Schützen Ihrer API 

Das Tutorial zeigt, wie Sie Ihre API so transformieren, dass keine Informationen zu einem privaten Back-End offengelegt werden. Beispielsweise möchten Sie vielleicht die Informationen zum Technologiestapel ausblenden, der auf dem Back-End ausgeführt wird. Sie können auch die ursprünglich im Textkörper der HTTP-Antwort der API angezeigten URLs ausblenden und stattdessen eine Umleitung an das APIM-Gateway durchführen.

In diesem Tutorial wird außerdem veranschaulicht, wie einfach Sie Ihre Back-End-API schützen können, indem Sie ein Aufruflimit mithilfe von API Management konfigurieren. Beispielsweise können Sie die Anzahl von API-Aufrufen begrenzen, damit die API nicht übermäßig von den Entwicklern verwendet wird. Weitere Informationen finden Sie unter [API Management-Richtlinien](api-management-policies.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Transformieren einer API zum Entfernen von Antwortheadern
> * Ersetzen der ursprünglichen URLs im Textkörper der API-Antwort durch APIM-Gateway-URLs
> * Schützen einer API durch das Hinzufügen einer Richtlinie für ein Aufruflimit (Drosselung)
> * Testen der Transformationen

![Richtlinien](./media/transform-api/api-management-management-console.png)

## <a name="prerequisites"></a>Voraussetzungen

+ Absolvieren Sie den folgenden Schnellstart: [Erstellen einer Azure API Management-Instanz](get-started-create-service-instance.md).
+ Schließen Sie darüber hinaus das folgende Tutorial ab: [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md).
 
[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="transform-an-api-to-strip-response-headers"></a>Transformieren einer API zum Entfernen von Antwortheadern

In diesem Abschnitt wird gezeigt, wie Sie die HTTP-Header ausblenden, die Sie nicht für Ihre Benutzer anzeigen möchten. In diesem Beispiel werden die folgenden Header in der HTTP-Antwort gelöscht:

* **X-Powered-By**
* **X-AspNet-Version**

### <a name="test-the-original-response"></a>Testen der ursprünglichen Antwort

So zeigen Sie die ursprüngliche Antwort an

1. Klicken Sie auf die Registerkarte **API**.
2. Klicken Sie in der API-Liste auf **Demo Conference API**.
3. Wählen Sie den Vorgang **GetSpeakers** aus.
4. Klicken Sie oben auf der Seite auf die Registerkarte **Testen**.
5. Klicken Sie unten auf der Seite auf die Schaltfläche **Senden**. 

    Wie Sie sehen können, sieht die ursprüngliche Antwort so aus:

    ![Richtlinien](./media/transform-api/original-response.png)

### <a name="set-the-transformation-policy"></a>Festlegen der Transformationsrichtlinie

1. Navigieren Sie zu Ihrer APIM-Instanz.
2. Klicken Sie auf die Registerkarte **API**.
3. Klicken Sie in der API-Liste auf **Demo Conference API**.
4. Wählen Sie **Alle Vorgänge** aus.
5. Klicken Sie im oberen Seitenbereich auf die Registerkarte **Entwurf**.
6. Klicken Sie im Fenster **Ausgehende Verarbeitung** auf das Dreieck (neben dem Stift).
7. Klicken Sie auf **Code-Editor**.
    
     ![Richtlinie bearbeiten](./media/set-edit-policies/set-edit-policies01.png)
9. Positionieren Sie den Cursor im **&lt;outbound&gt;**-Element.
10. Klicken Sie im rechten Fenster unter **Transformationsrichtlinien** zweimal auf **+ HTTP-Header festlegen** (um zwei Richtlinienausschnitte einzufügen).

    ![Richtlinien](./media/transform-api/transform-api.png)
11. Ändern Sie Ihren **<outbound>**-Code, sodass er folgendermaßen aussieht:

        <set-header name="X-Powered-By" exists-action="delete" />
        <set-header name="X-AspNet-Version" exists-action="delete" />
                
## <a name="replace-original-urls-in-the-body-of-the-api-response-with-apim-gateway-urls"></a>Ersetzen der ursprünglichen URLs im Textkörper der API-Antwort durch APIM-Gateway-URLs

In diesem Abschnitt wird gezeigt, wie Sie die ursprünglich im Textkörper der HTTP-Antwort der API angezeigten URLs ausblenden und stattdessen eine Umleitung an das APIM-Gateway durchführen.

### <a name="test-the-original-response"></a>Testen der ursprünglichen Antwort

So zeigen Sie die ursprüngliche Antwort an

1. Klicken Sie auf die Registerkarte **API**.
2. Klicken Sie in der API-Liste auf **Demo Conference API**.
3. Wählen Sie den Vorgang **GetSpeakers** aus.
4. Klicken Sie oben auf der Seite auf die Registerkarte **Testen**.
5. Klicken Sie unten auf der Seite auf die Schaltfläche **Senden**. 

    Wie Sie sehen können, sieht die ursprüngliche Antwort so aus:

    ![Richtlinien](./media/transform-api/original-response2.png)

### <a name="set-the-transformation-policy"></a>Festlegen der Transformationsrichtlinie

1. Navigieren Sie zu Ihrer APIM-Instanz.
2. Klicken Sie auf die Registerkarte **API**.
3. Klicken Sie in der API-Liste auf **Demo Conference API**.
4. Wählen Sie **Alle Vorgänge** aus.
5. Klicken Sie im oberen Seitenbereich auf die Registerkarte **Entwurf**.
6. Klicken Sie im Fenster **Ausgehende Verarbeitung** auf das Dreieck (neben dem Stift).
7. Klicken Sie auf **Code-Editor**.
8. Positionieren Sie den Cursor im **&lt;outbound&gt;**-Element.
9. Klicken Sie im rechten Fenster unter **Transformationsrichtlinien** auf **+ Zeichenfolge im Text suchen und ersetzen**.
10. Ändern Sie Ihren **<find-and-replace**-Code (im Element **<outbound>**) ab, um die URL durch eine URL zu Ihrem APIM-Gateway zu ersetzen. Beispiel: 

        <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>

## <a name="protect-an-api-by-adding-rate-limit-policy-throttling"></a>Schützen einer API durch das Hinzufügen einer Richtlinie für ein Aufruflimit (Drosselung)

In diesem Abschnitt wird gezeigt, wie Sie Ihre Back-End-API schützen, indem Sie Aufruflimits konfigurieren. Beispielsweise können Sie die Anzahl von API-Aufrufen begrenzen, damit die API nicht übermäßig von den Entwicklern verwendet wird. In diesem Beispiel wird das Limit für jede Abonnement-ID auf 3 Aufrufe pro 15 Sekunden festgelegt. Nach 15 Sekunden kann ein Entwickler die API erneut aufrufen.

1. Navigieren Sie zu Ihrer APIM-Instanz.
2. Klicken Sie auf die Registerkarte **API**.
3. Klicken Sie in der API-Liste auf **Demo Conference API**.
4. Wählen Sie **Alle Vorgänge** aus.
5. Klicken Sie im oberen Seitenbereich auf die Registerkarte **Entwurf**.
6. Klicken Sie im Fenster **Eingehende Verarbeitung** auf das Dreieck (neben dem Stift).
7. Klicken Sie auf **Code-Editor**.
8. Positionieren Sie den Cursor im **&lt;inbound&gt;**-Element.
9. Klicken Sie im rechten Fenster unter **Richtlinien für die Zugriffsbeschränkung** auf **+ Aufrufrate pro Schlüssel einschränken**.
10. Ändern Sie Ihren **<rate-limit-by-key**-Code (im **<inbound>**-Element) folgendermaßen ab:

        <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />

## <a name="test-the-transformations"></a>Testen der Transformationen
        
An diesem Punkt sieht Ihr Richtliniencode so aus:

    <policies>
        <inbound>
            <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />
            <base />
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <set-header name="X-Powered-By" exists-action="delete" />
            <set-header name="X-AspNet-Version" exists-action="delete" />
            <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>

Im verbleibenden Teil dieses Abschnitts werden die Richtlinientransformationen getestet, die Sie in diesem Artikel festgelegt haben.

### <a name="test-the-stripped-response-headers"></a>Testen der entfernten Antwortheader

1. Navigieren Sie zu Ihrer APIM-Instanz.
2. Klicken Sie auf die Registerkarte **API**.
3. Klicken Sie in der API-Liste auf **Demo Conference API**.
4. Klicken Sie auf den Vorgang **GetSpeakers**.
5. Wählen Sie die Registerkarte **Testen** aus.
6. Klicken Sie auf **Senden**.

    Wie Sie sehen können, wurden die Header entfernt:

    ![Richtlinien](./media/transform-api/final-response1.png)

### <a name="test-the-replaced-url"></a>Testen der ersetzten URL

1. Navigieren Sie zu Ihrer APIM-Instanz.
2. Klicken Sie auf die Registerkarte **API**.
3. Klicken Sie in der API-Liste auf **Demo Conference API**.
4. Klicken Sie auf den Vorgang **GetSpeakers**.
5. Wählen Sie die Registerkarte **Testen** aus.
6. Klicken Sie auf **Senden**.

    Wie Sie sehen können, wurde die URL ersetzt.

    ![Richtlinien](./media/transform-api/final-response2.png)

### <a name="test-the-rate-limit-throttling"></a>Testen des Aufruflimits (Drosselung)

1. Navigieren Sie zu Ihrer APIM-Instanz.
2. Klicken Sie auf die Registerkarte **API**.
3. Klicken Sie in der API-Liste auf **Demo Conference API**.
4. Klicken Sie auf den Vorgang **GetSpeakers**.
5. Wählen Sie die Registerkarte **Testen** aus.
6. Klicken Sie dreimal hintereinander auf **Senden**.

    Nachdem Sie die Anforderung dreimal gesendet haben, erhalten Sie die Antwort **429 Zu viele Anforderungen**.
7. Warten Sie etwa 15 Sekunden ab, und klicken Sie erneut auf **Senden**. Jetzt sollten Sie die Antwort **200 OK** erhalten.

    ![Drosselung](./media/transform-api/test-throttling.png)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Transformieren einer API zum Entfernen von Antwortheadern
> * Ersetzen der ursprünglichen URLs im Textkörper der API-Antwort durch APIM-Gateway-URLs
> * Schützen einer API durch das Hinzufügen einer Richtlinie für ein Aufruflimit (Drosselung)
> * Testen der Transformationen

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Überwachen Ihrer API](api-management-howto-use-azure-monitor.md)
