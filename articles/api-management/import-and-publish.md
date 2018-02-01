---
title: Importieren und Verwalten Ihrer ersten API in Azure API Management | Microsoft-Dokumentation
description: "Erfahren sie, wie Sie Ihre erste API mit API Management importieren und veröffentlichen."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: apimpm
ms.openlocfilehash: ffe5ee95c66eee7dccd25a1afd2fe639cbc273f5
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="import-and-publish-your-first-api"></a>Importieren und Veröffentlichen Ihrer ersten API 

Dieses Tutorial veranschaulicht, wie Sie eine Back-End-API mit OpenAPI-Spezifikation aus „http://conferenceapi.azurewebsites.net?format=json“ importieren. Diese Back-End-API wird von Microsoft bereitgestellt und in Azure gehostet. 

Nachdem die Back-End-API in API Management (APIM) importiert wurde, wird die APIM-API zur Fassade für die Back-End-API. Zum Zeitpunkt des Imports der Back-End-API sind die Quell-API und die APIM-API identisch. APIM ermöglicht Ihnen das Anpassen der Fassade gemäß Ihren Anforderungen, ohne die Back-End-API ändern zu müssen. Weitere Informationen finden Sie unter [Transformieren und Schützen Ihrer API](transform-api.md). 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Importieren Ihrer ersten API
> * Testen der API im Azure-Portal
> * Testen der API im Entwicklerportal

![Neue API](./media/api-management-get-started/created-api.png)

## <a name="prerequisites"></a>Voraussetzungen

Absolvieren Sie den folgenden Schnellstart: [Erstellen einer Azure API Management-Instanz](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Importieren und Veröffentlichen einer Back-End-API

In diesem Abschnitt wird gezeigt, wie Sie eine Back-End-API mit OpenAPI-Spezifikation importieren und veröffentlichen.
 
1. Wählen Sie unter **API MANAGEMENT** die Option **APIs** aus.
2. Wählen Sie in der Liste die Option **OpenAPI-Spezifikation** aus.

    ![Erstellen einer API](./media/api-management-get-started/create-api.png)

    Sie können die API-Werte während der Erstellung oder später über die Registerkarte **Einstellungen** festlegen.  

    |Einstellung|Wert|BESCHREIBUNG|
    |---|---|---|
    |**OpenAPI-Spezifikation**|http://conferenceapi.azurewebsites.net?format=json|Verweist auf den Dienst zum Implementieren der API. API Management leitet Anfragen an diese Adresse weiter.|
    |**Anzeigename**|*Demo Conference API*|Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt. <br/>Dieser Name wird im Entwicklerportal angezeigt.|
    |**Name**|*demo-conference-api*|Stellt einen eindeutigen Namen für die API bereit. <br/>Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt.|
    |**Beschreibung**|Geben Sie eine optionale Beschreibung der API an.|Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt.|
    |**API-URL-Suffix**|*conference*|Das Suffix wird an die Basis-URL für den API Management-Dienst angehängt. API Management unterscheidet APIs durch deren Suffix. Daher muss jede API eines bestimmten Herausgebers ein eindeutiges Suffix haben.|
    |**URL-Schema**|*HTTPS*|Legt fest, welche Protokolle für den Zugriff auf die API verwendet werden können. |
    |**Produkte**|*Unbegrenzt*| Veröffentlichen Sie die API, indem Sie sie einem Produkt zuordnen. Geben Sie optional den Produktnamen ein, um diese neue API einem Produkt hinzuzufügen. Dieser Schritt kann mehrfach wiederholt werden, um die API mehreren Produkten hinzuzufügen.<br/>Bei Produkten handelt es sich um API-Zuordnungen. Sie können eine Reihe von APIs einfügen und sie Entwicklern über das Entwicklerportal zur Verfügung stellen. Entwickler müssen ein Produkt zunächst abonnieren, um Zugriff auf die API zu erhalten. Wenn sie ein Produkt abonnieren, erhalten sie einen Abonnementschlüssel, der für jede API in diesem Produkt gilt. Wenn Sie die APIM-Instanz erstellt haben, sind Sie bereits Administrator und haben dadurch standardmäßig alle Produkte abonniert.<br/> Standardmäßig enthält jede API Management-Instanz zwei Beispielprodukte: **Starter** und **Unlimited**. |
3. Klicken Sie auf **Erstellen**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testen der neuen APIM-API im Azure-Portal

Vorgänge können direkt aus dem Azure-Portal aufgerufen werden. Dies ist ein einfacher Weg, die Vorgänge einer API anzuzeigen und zu testen.  
1. Wählen Sie die API aus, die Sie im vorherigen Schritt erstellt haben.
2. Wählen Sie die Registerkarte **Testen** aus.  ![Test-API](./media/api-management-get-started/test-api.png)
3. Klicken Sie auf **GetSpeakers**.
    Auf der Seite werden Felder für Abfrageparameter angezeigt, aber in diesem Fall sind keine vorhanden. Die Seite enthält auch Felder für die Header. Einer der Header ist „Ocp-Apim-Subscription-Key“. Er steht für den Abonnementschlüssel des Produkts, das dieser API zugeordnet ist. Der Schlüssel wird automatisch eingefügt.
4. Klicken Sie auf **Senden**.

    Das Back-End antwortet mit **200 OK** und einigen Daten.

## <a name="call-operation"></a>Aufrufen einer Operation aus dem Entwicklerportal

Vorgänge können auch im **Entwicklerportal** aufgerufen werden, um APIs zu testen. 

1. Wählen Sie die API aus, die Sie im Schritt „Importieren und Veröffentlichen einer Back-End-API“ erstellt haben.
2. Klicken Sie auf **Entwicklerportal**.

    ![Testen im Entwicklerportal](./media/api-management-get-started/developer-portal.png)

    Die Website „Entwicklerportal“ wird geöffnet.
3. Wählen Sie die Option **API** aus.
4. Wählen Sie **Demo Conference API** aus.
5. Klicken Sie auf **GetSpeakers**.
    
    Auf der Seite werden Felder für Abfrageparameter angezeigt, aber in diesem Fall sind keine vorhanden. Die Seite enthält auch Felder für die Header. Einer der Header ist „Ocp-Apim-Subscription-Key“. Er steht für den Abonnementschlüssel des Produkts, das dieser API zugeordnet ist. Wenn Sie die APIM-Instanz erstellt haben, sind Sie bereits Administrator, sodass der Schlüssel automatisch eingetragen wird.
6. Wählen Sie **Ausprobieren**.
7. Klicken Sie auf **Senden**.
    
    Nach dem Aufruf der Operation zeigt das Entwicklerportal den **Antwortstatus**, die **Antwortheader** sowie den **Antwortinhalt** an.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Importieren Ihrer ersten API
> * Testen der API im Azure-Portal
> * Testen der API im Entwicklerportal

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Erstellen und Veröffentlichen eines Produkts](api-management-howto-add-products.md)
