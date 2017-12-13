---
title: Importieren einer OpenAPI-Spezifikation mit dem Azure-Portal | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine OpenAPI-Spezifikation mit API Management importieren.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: f0c77c6e959ca99698b3ea704756a6abf36147f3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="import-an-openapi-specification"></a>Importieren einer OpenAPI-Spezifikation

In diesem Artikel wird veranschaulicht, wie Sie eine Back-End-API vom Typ „OpenAPI-Spezifikation“ importieren, die sich unter „http://conferenceapi.azurewebsites.net?format=json“ befindet. Diese Back-End-API wird von Microsoft bereitgestellt und unter Azure gehostet. Im Artikel wird auch beschrieben, wie Sie die APIM-API testen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Importieren einer Back-End-API vom Typ „OpenAPI-Spezifikation“
> * Testen der API im Azure-Portal
> * Testen der API im Entwicklerportal

## <a name="prerequisites"></a>Voraussetzungen

Absolvieren Sie das folgende Schnellstarttutorial: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Importieren und Veröffentlichen einer Back-End-API

1. Wählen Sie unter **API MANAGEMENT** die Option **APIs** aus.
2. Wählen Sie in der Liste **Add a new API** (Neue API hinzufügen) die Option **OpenAPI-Spezifikation**.
    ![OpenAPI-Spezifikation](./media/import-api-from-oas/oas-api.png)
3. Nehmen Sie die entsprechenden Einstellungen vor. Sie können alle API-Werte während der Erstellung festlegen. Alternativ dazu können Sie einige Werte später festlegen, indem Sie die Registerkarte **Einstellungen** verwenden. <br/> Wenn Sie die **TAB-TASTE** drücken, werden einige (oder alle) Felder mit den Informationen aus dem angegebenen Back-End-Dienst gefüllt.

    ![Erstellen einer API](./media/api-management-get-started/create-api.png)

    |Einstellung|Wert|Beschreibung|
    |---|---|---|
    |**OpenAPI-Spezifikation**|http://conferenceapi.azurewebsites.net?format=json|Verweist auf den Dienst zum Implementieren der API. API Management leitet Anfragen an diese Adresse weiter.|
    |**Anzeigename**|*Demo Conference API*|Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt. <br/>Dieser Name wird im Entwicklerportal angezeigt.|
    |**Name**|*demo-conference-api*|Stellt einen eindeutigen Namen für die API bereit. <br/>Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt.|
    |**Beschreibung**|Geben Sie eine optionale Beschreibung der API an.|Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt.|
    |**API-URL-Suffix**|*conference*|Das Suffix wird an die Basis-URL für den API Management-Dienst angehängt. API Management unterscheidet APIs durch deren Suffix. Daher muss jede API eines bestimmten Herausgebers ein eindeutiges Suffix haben.|
    |**URL-Schema**|*HTTPS*|Legt fest, welche Protokolle für den Zugriff auf die API verwendet werden können. |
    |**Produkte**|*Unlimited*| Veröffentlichen Sie die API, indem Sie die API einem Produkt zuordnen. Geben Sie optional den Produktnamen ein, um diese neue API einem Produkt hinzuzufügen. Dieser Schritt kann mehrfach wiederholt werden, um die API mehreren Produkten hinzuzufügen.<br/>Bei Produkten handelt es sich um API-Zuordnungen. Sie können eine Reihe von APIs einfügen und sie Entwicklern über das Entwicklerportal zur Verfügung stellen. Entwickler müssen ein Produkt zunächst abonnieren, um Zugriff auf die API zu erhalten. Wenn sie ein Produkt abonnieren, erhalten sie einen Abonnementschlüssel, der für jede API in diesem Produkt gilt. Wenn Sie die APIM-Instanz erstellt haben, sind Sie bereits Administrator und haben dadurch standardmäßig alle Produkte abonniert.<br/> Standardmäßig enthält jede API Management-Instanz zwei Beispielprodukte: **Starter** und **Unlimited**. |

4. Klicken Sie auf **Erstellen**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testen der neuen APIM-API im Azure-Portal

Vorgänge können direkt aus dem Azure-Portal aufgerufen werden. Dies ist ein einfacher Weg, um die Vorgänge einer API anzuzeigen und zu testen.  

1. Wählen Sie die API aus, die Sie im vorherigen Schritt erstellt haben.
2. Wählen Sie die Registerkarte **Test**.

    ![Test-API](./media/api-management-get-started/test-api.png)
1. Klicken Sie auf **GetSpeakers**.

    Auf der Seite werden Felder für Abfrageparameter angezeigt, aber in diesem Fall sind keine vorhanden. Die Seite enthält auch Felder für die Header. Einer der Header ist „Ocp-Apim-Subscription-Key“. Er steht für den Abonnementschlüssel des Produkts, das dieser API zugeordnet ist. Wenn Sie die APIM-Instanz erstellt haben, sind Sie bereits Administrator, sodass der Schlüssel automatisch eingetragen wird. 
4. Klicken Sie auf **Senden**.

    Das Back-End antwortet mit **200 OK** und einigen Daten.

## <a name="call-operation"> </a>Aufrufen eines Vorgangs über das Entwicklerportal

Vorgänge können auch im **Entwicklerportal** aufgerufen werden, um APIs zu testen. 

1. Wählen Sie die API aus, die Sie im Schritt „Importieren und Veröffentlichen einer Back-End-API“ erstellt haben.
2. Navigieren Sie zum **Entwicklerportal**.

    ![Testen im Entwicklerportal](./media/api-management-get-started/developer-portal.png)

    Die Website „Entwicklerportal“ wird geöffnet.
3. Wählen Sie die Option **API**.
4. Wählen Sie **Demo Conference API**.
5. Klicken Sie auf **GetSpeakers**.
    
    Auf der Seite werden Felder für Abfrageparameter angezeigt, aber in diesem Fall sind keine vorhanden. Die Seite enthält auch Felder für die Header. Einer der Header ist „Ocp-Apim-Subscription-Key“. Er steht für den Abonnementschlüssel des Produkts, das dieser API zugeordnet ist. Wenn Sie die APIM-Instanz erstellt haben, sind Sie bereits Administrator, sodass der Schlüssel automatisch eingetragen wird.
6. Wählen Sie **Ausprobieren**.
7. Klicken Sie auf **Senden**.
    
    Nach dem Aufruf der Operation zeigt das Entwicklerportal den **Antwortstatus**, die **Antwortheader** sowie den **Antwortinhalt** an.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Transformieren und Schützen veröffentlichter APIs](transform-api.md)
