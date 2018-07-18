---
title: Hinzufügen der Zwischenspeicherung zum Verbessern der Leistung in Azure API Management | Microsoft Docs
description: Erfahren Sie, wie Sie Latenz, Bandbreitennutzung und Webdienstauslastung für API Management-Dienstaufrufe verbessern.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7458ad6e0a864d742f74ce743ce3179594113c00
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
ms.locfileid: "26127776"
---
# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Hinzufügen der Zwischenspeicherung zum Verbessern der Leistung in Azure API Management
Operationen in API Management können für das Zwischenspeichern von Antworten konfiguriert werden. Das Zwischenspeichern von Antworten kann API-Latenz, Bandbreitennutzung und Webdienstlast für Daten, die sich eher selten ändern, drastisch senken.
 
Ausführlichere Informationen zum Zwischenspeichern finden Sie unter [Cacherichtlinien für API Management](api-management-caching-policies.md) sowie unter [Benutzerdefiniertes Caching in Azure API Management](api-management-sample-cache-by-key.md).

![Zwischenspeicherungsrichtlinien](media/api-management-howto-cache/cache-policies.png)

Sie lernen Folgendes:

> [!div class="checklist"]
> * Hinzufügen des Zwischenspeicherns von Antworten für Ihre API
> * Überprüfen des Zwischenspeicherns in Aktion

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

+ [Erstellen Sie eine Azure API Management-Instanz.](get-started-create-service-instance.md)
+ [Importieren und Veröffentlichen Sie eine API.](import-and-publish.md)

## <a name="caching-policies"> </a>Hinzufügen der Zwischenspeicherungsrichtlinien

Mit den Zwischenspeicherungsrichtlinien in diesem Beispiel gibt die erste Anforderung an den Vorgang **GetSpeakers** eine Antwort vom Back-End-Dienst zurück. Diese Antwort wird zwischengespeichert und erhält einen Schlüssel, der auf den angegebenen Headern und Abfrageparametern basiert. Bei nachfolgenden Aufrufen der Operation mit denselben Parametern wird die zwischengespeicherte Antwort zurückgegeben, bis das Ablaufintervall abgelaufen ist.

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.
2. Navigieren Sie zu Ihrer APIM-Instanz.
3. Klicken Sie auf die Registerkarte **API**.
4. Klicken Sie in der API-Liste auf **Demo Conference API**.
5. Wählen Sie **GetSpeakers** aus.
6. Klicken Sie im oberen Seitenbereich auf die Registerkarte **Entwurf**.
7. Klicken Sie im Fenster **Eingehende Verarbeitung** auf das Dreieck (neben dem Stift).

    ![Code-Editor](media/api-management-howto-cache/code-editor.png)
8. Klicken Sie auf **Code-Editor**.
9. Fügen Sie im Element für den **Eingang** die folgende Richtlinie hinzu:

        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
            <vary-by-header>Accept</vary-by-header>
            <vary-by-header>Accept-Charset</vary-by-header>
            <vary-by-header>Authorization</vary-by-header>
        </cache-lookup>

10. Fügen Sie im Element für den **Ausgang** die folgende Richtlinie hinzu:

        <cache-store caching-mode="cache-on" duration="20" />

    **Dauer** gibt das Ablaufintervall der gespeicherten Antworten an. In diesem Beispiel beträgt das Intervall **20** Sekunden.

## <a name="test-operation"></a>Aufrufen einer Operation und Testen der Zwischenspeicherung
Um die Zwischenspeicherung in Aktion zu sehen, können Sie den Vorgang über das Entwicklerportal aufrufen.

1. Navigieren Sie im Azure-Portal zu Ihrer APIM-Instanz.
2. Klicken Sie auf die Registerkarte **APIs**.
3. Wählen Sie die API aus, der Sie Zwischenspeicherungsrichtlinien hinzugefügt haben.
4. Wählen Sie den Vorgang **GetSpeakers** aus.
5. Klicken Sie im Menü rechts oben auf die Registerkarte **Test**.
6. Klicken Sie auf **Senden**.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu Richtlinien für die Zwischenspeicherung finden Sie unter [Richtlinien für die Zwischenspeicherung][Caching policies] in der [Richtlinienreferenz für API Management][API Management policy reference].
* Informationen zum Zwischenspeichern von Elementen nach Schlüssel mithilfe von Richtlinienausdrücken finden Sie unter [Benutzerdefiniertes Zwischenspeichern in Azure API Management](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Create an API Management service instance]: get-started-create-service-instance.md

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
