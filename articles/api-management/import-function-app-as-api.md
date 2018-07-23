---
title: Importieren einer Funktionen-App als eine API mit dem Azure-Portal | Microsoft-Dokumentation
description: Dieses Tutorial veranschaulicht, wie Sie API Management (APIM) verwenden, um Funktionen-Apps als API zu importieren.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/15/2018
ms.author: apimpm
ms.openlocfilehash: 0ee83446bb08e66c7f325bdd5585b8cc0484a74e
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090927"
---
# <a name="import-a-function-app-as-an-api"></a>Importieren von Funktionen-Apps als API

Dieser Artikel zeigt, wie Sie eine Funktionen-App als eine API importieren. Der Artikel zeigt außerdem, wie Sie die APIM-API testen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Importieren von Funktionen-Apps als API
> * Testen der API im Azure-Portal
> * Testen der API im Entwicklerportal

## <a name="prerequisites"></a>Voraussetzungen

+ Absolvieren Sie den folgende Schnellstart: [Erstellen einer Azure API Management-Instanz](get-started-create-service-instance.md)
+ Stellen Sie sicher, dass Ihr Abonnement eine Azure-Funktionen-App enthält. Weitere Informationen finden Sie unter [Erstellen einer Funktionen-App](../azure-functions/functions-create-first-azure-function.md#create-a-function-app).
+ [Erstellen Sie eine OpenAPI-Definition](../azure-functions/functions-openapi-definition.md) Ihrer Azure-Funktionen-App.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Importieren und Veröffentlichen einer Back-End-API

1. Wählen Sie unter **API MANAGEMENT** die Option **APIs** aus.
2. Wählen Sie **Funktionen-App** in der Liste **Neue API hinzufügen** aus.

    ![Funktionen-App](./media/import-function-app-as-api/function-app-api.png)
3. Drücken Sie auf **Durchsuchen**, um die Liste der Funktionen-Apps in Ihrem Abonnement anzuzeigen.
4. Wählen Sie die App aus. APIM sucht den Swagger, der der ausgewählten App zugeordnet ist, ruft ihn ab und importiert ihn. 
5. Fügen Sie ein API-URL-Suffix hinzu. Das Suffix ist ein Name, der diese spezifische API in dieser APIM-Instanz identifiziert. Es muss in dieser APIM-Instanz eindeutig sein.
6. Veröffentlichen Sie die API, indem Sie sie einem Produkt zuordnen. In diesem Fall wird das Produkt „*Unlimited*“ verwendet.  Wenn Sie möchten, dass die API veröffentlicht wird und dann Entwicklern zur Verfügung steht, fügen Sie sie einem Produkt hinzu. Sie können dies während der Erstellung der API vornehmen oder später festlegen.

    Bei Produkten handelt es sich um API-Zuordnungen. Sie können eine Reihe von APIs einfügen und sie Entwicklern über das Entwicklerportal zur Verfügung stellen. Entwickler müssen ein Produkt zunächst abonnieren, um Zugriff auf die API zu erhalten. Wenn sie ein Produkt abonnieren, erhalten sie einen Abonnementschlüssel, der für jede API in diesem Produkt gilt. Wenn Sie die APIM-Instanz erstellt haben, sind Sie bereits Administrator und haben dadurch standardmäßig alle Produkte abonniert.

    Standardmäßig enthält jede API Management-Instanz zwei Beispielprodukte:

    * **Starter**
    * **Unbegrenzt**   
7. Klicken Sie auf **Erstellen**.

## <a name="populate-azure-functions-keys-in-azure-api-management"></a>Auffüllen von Azure-Funktionsschlüsseln in Azure API Management

Wenn die importierten Azure-Funktionen durch Schlüssel geschützt sind, erstellt Azure API Management automatisch **benannte Werte** dafür, füllt die Einträge aber nicht mit geheimen Schlüsseln auf. Für jeden Eintrag müssen Sie die nachstehenden Schritte ausführen.  

1. Navigieren Sie zur Registerkarte **Benannte Werte** in der API Management-Instanz.
2. Klicken Sie auf einen Eintrag und dann auf der Randleiste auf **Wert anzeigen**.

    ![Benannte Werte](./media/import-function-app-as-api/apim-named-values.png)

3. Wenn der Inhalt *Code für {Name der Azure-Funktion}* ähnelt, navigieren Sie zur importierten Azure-Funktionen-App und dann zu Ihrer Azure-Funktion.
4. Wechseln Sie zum Bereich **Verwalten** der gewünschten Azure-Funktion, und kopieren Sie den jeweiligen Schlüssel entsprechend der Authentifizierungsmethode Ihrer Azure-Funktion.

    ![Funktionen-App](./media/import-function-app-as-api/azure-functions-app-keys.png)

5. Fügen Sie den Schlüssel in das Textfeld von **Benannte Werte** ein, und klicken Sie auf **Speichern**.

    ![Funktionen-App](./media/import-function-app-as-api/apim-named-values-2.png)

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testen der neuen APIM-API im Azure-Portal

Vorgänge können direkt aus dem Azure-Portal aufgerufen werden. Dies ist ein einfacher Weg, die Vorgänge einer API anzuzeigen und zu testen.  

1. Wählen Sie die API aus, die Sie im vorherigen Schritt erstellt haben.
2. Wählen Sie die Registerkarte **Testen** aus.
3. Wählen Sie einen Vorgang aus.

    Die Seite zeigt Felder für Abfrageparameter und Felder für die Header. Einer der Header ist „Ocp-Apim-Subscription-Key“. Er steht für den Abonnementschlüssel des Produkts, das dieser API zugeordnet ist. Wenn Sie die APIM-Instanz erstellt haben, sind Sie bereits Administrator, sodass der Schlüssel automatisch eingetragen wird. 
1. Klicken Sie auf **Senden**.

    Das Back-End antwortet mit **200 OK** und einigen Daten.

## <a name="call-operation"></a>Aufrufen einer Operation aus dem Entwicklerportal

Vorgänge können auch im **Entwicklerportal** aufgerufen werden, um APIs zu testen. 

1. Wählen Sie die API aus, die Sie im Schritt „Importieren und Veröffentlichen einer Back-End-API“ erstellt haben.
2. Klicken Sie auf **Entwicklerportal**.

    Die Website „Entwicklerportal“ wird geöffnet.
3. Wählen Sie die **API** aus, die Sie erstellt haben.
4. Klicken Sie auf den Vorgang, den Sie testen möchten.
5. Klicken Sie auf **Ausprobieren**.
6. Klicken Sie auf **Senden**.
    
    Nach dem Aufruf der Operation zeigt das Entwicklerportal den **Antwortstatus**, die **Antwortheader** sowie den **Antwortinhalt** an.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Transformieren und Schützen einer veröffentlichten API](transform-api.md)