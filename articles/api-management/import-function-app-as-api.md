---
title: Importieren einer Azure Functions-App als API im Azure-Portal | Microsoft-Dokumentation
description: Dieses Tutorial veranschaulicht, wie Sie Azure API Management verwenden, um eine Azure Functions-App als API zu importieren.
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
ms.openlocfilehash: 670fa58de7155028b0f72f1f819b9f269e07b9eb
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239051"
---
# <a name="import-an-azure-functions-app-as-an-api"></a>Importieren einer Azure Functions-App als API

Dieser Artikel zeigt, wie Sie eine Azure Functions-App als API importieren. Der Artikel zeigt außerdem, wie Sie die Azure API Management-API testen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Importieren einer Functions-App als API
> * Testen der API im Azure-Portal
> * Testen der API im Entwicklerportal

## <a name="prerequisites"></a>Voraussetzungen

+ Absolvieren Sie den Schnellstart [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md).
+ Stellen Sie sicher, dass Ihr Abonnement eine Azure Functions-App enthält. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Funktion im Azure-Portal](../azure-functions/functions-create-first-azure-function.md#create-a-function-app).
+ [Erstellen Sie eine OpenAPI-Definition](../azure-functions/functions-openapi-definition.md) Ihrer Azure Functions-App.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importieren und Veröffentlichen einer Back-End-API

1. Wählen Sie unter **API MANAGEMENT** die Option **APIs** aus.
2. Wählen Sie **Functions-App** in der Liste **Neue API hinzufügen** aus.

    ![Functions-App](./media/import-function-app-as-api/function-app-api.png)
3. Klicken Sie auf **Durchsuchen**, um die Liste der Functions-Apps in Ihrem Abonnement anzuzeigen.
4. Wählen Sie die App aus. API Management sucht den Swagger, der der ausgewählten App zugeordnet ist, ruft ihn ab und importiert ihn. 
5. Fügen Sie ein API-URL-Suffix hinzu. Das Suffix ist ein Name, der diese spezifische API in dieser API Management-Instanz identifiziert. Das Suffix muss in dieser API Management-Instanz eindeutig sein.
6. Veröffentlichen Sie die API, indem Sie sie einem Produkt zuordnen. In diesem Fall wird das Produkt **Unlimited** verwendet. Wenn die API veröffentlicht werden und Entwicklern zur Verfügung stehen soll, fügen Sie sie einem Produkt hinzu. Sie können die API bei ihrer Erstellung oder später zu einem Produkt hinzufügen.

    Bei Produkten handelt es sich um API-Zuordnungen. Sie können mehrere APIs einfügen und sie Entwicklern über das Entwicklerportal zur Verfügung stellen. Entwickler müssen ein Produkt zunächst abonnieren, um Zugriff auf die API zu erhalten. Wenn ein Entwickler ein Produkt abonniert, erhält er einen Abonnementschlüssel, der für jede API in diesem Produkt gilt. Wenn Sie die API Management-Instanz erstellt haben, sind Sie Administrator. Administratoren haben standardmäßig alle Produkte abonniert.

    Standardmäßig enthält jede API Management-Instanz zwei Beispielprodukte:

    * **Starter**
    * **Unbegrenzt**   
7. Klicken Sie auf **Erstellen**.

## <a name="populate-azure-functions-keys-in-azure-api-management"></a>Auffüllen von Azure-Funktionsschlüsseln in Azure API Management

Wenn die importierten Azure Functions-Apps durch Schlüssel geschützt sind, erstellt API Management automatisch *benannte Werte* für die Schlüssel. API Management füllt die Einträge nicht mit Geheimnissen auf. Führen Sie für jeden Eintrag die folgenden Schritte aus:  

1. Navigieren Sie in der API Management-Instanz zur Registerkarte **Benannte Werte**.
2. Klicken Sie auf einen Eintrag und dann auf der Randleiste auf **Wert anzeigen**.

    ![Benannte Werte](./media/import-function-app-as-api/apim-named-values.png)

3. Wenn das Feld **Wert** einen Text wie **Code für \<Azure Functions-Name\>** enthält, navigieren Sie zu **Functions-Apps** und dann zu **Functions**.
4. Klicken Sie auf **Verwalten**, und kopieren Sie basierend auf der Authentifizierungsmethode Ihrer App den entsprechenden Schlüssel.

    ![Functions-App – Kopieren von Schlüsseln](./media/import-function-app-as-api/azure-functions-app-keys.png)

5. Fügen Sie den Schlüssel im Feld **Wert** ein, und wählen Sie dann **Speichern**.

    ![Functions-App – Einfügen von Schlüsselwerten](./media/import-function-app-as-api/apim-named-values-2.png)

## <a name="test-the-new-api-management-api-in-the-azure-portal"></a>Testen der neuen API Management-API im Azure-Portal

Sie können Vorgänge direkt über das Azure-Portal aufrufen. Das Azure-Portal bietet eine komfortable Möglichkeit, die Vorgänge einer API anzuzeigen und zu testen.  

1. Wählen Sie die im vorherigen Abschnitt erstellte API aus.
2. Wählen Sie die Registerkarte **Testen** aus.
3. Wählen Sie einen Vorgang aus.

    Die Seite zeigt Felder für Abfrageparameter und Felder für die Header. Einer der Header ist **Ocp-Apim-Subscription-Key**. Er steht für den Abonnementschlüssel des Produkts, das dieser API zugeordnet ist. Wenn Sie die API Management-Instanz erstellt haben, sind Sie bereits Administrator, sodass der Schlüssel automatisch eingetragen wird. 
4. Wählen Sie **Senden** aus.

    Das Back-End antwortet mit **200 OK** und einigen Daten.

## <a name="call-operation"></a>Aufrufen eines Vorgangs über das Entwicklerportal

Sie können Vorgänge auch über das Entwicklerportal aufrufen, um APIs zu testen. 

1. Wählen Sie die API aus, die Sie im Schritt [Importieren und Veröffentlichen einer Back-End-API](#create-api) erstellt haben.
2. Klicken Sie auf **Entwicklerportal**.

    Die Website „Entwicklerportal“ wird geöffnet.
3. Wählen Sie die **API** aus, die Sie erstellt haben.
4. Wählen Sie den Vorgang aus, den Sie testen möchten.
5. Wählen Sie **Testen**.
6. Wählen Sie **Senden** aus.
    
    Nach dem Aufruf der Operation zeigt das Entwicklerportal den **Antwortstatus**, die **Antwortheader** sowie den **Antwortinhalt** an.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Transformieren und Schützen einer veröffentlichten API](transform-api.md)