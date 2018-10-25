---
title: Informationen zum Authentifizieren und Autorisieren mithilfe der API in Azure Time Series Insights
description: In diesem Artikel wird die Konfiguration der Authentifizierung und Autorisierung für eine benutzerdefinierte Anwendung erläutert, die die Azure Time Series Insights-API aufruft.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/27/2017
ms.openlocfilehash: cbd851737d7122ff8b0c75eb2050ab481c8cecc0
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958464"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Authentifizierung und Autorisierung für die Azure Time Series Insights-API

In diesem Artikel wird die Konfiguration der in einer benutzerdefinierten Anwendung verwendeten Authentifizierung und Autorisierung erläutert, die die Azure Time Series Insights-API aufruft.

## <a name="service-principal"></a>Dienstprinzipal

In diesem Abschnitt wird erläutert, wie Sie eine Anwendung für den Zugriff auf die Azure Time Series Insights-API im Namen der Anwendung konfigurieren. Die Anwendung kann dann mit Anwendungsanmeldeinformationen anstelle von Benutzeranmeldeinformationen in der Time Series Insights-Umgebung Daten abfragen oder Verweisdaten veröffentlichen.

Wenn Sie über eine Anwendung verfügen, die auf Time Series Insights zugreifen muss, müssen Sie eine Azure Active Directory-Anwendung einrichten und die Datenzugriffsrichtlinien in der Time Series Insights-Umgebung zuweisen. Dieser Ansatz ist dem Ausführen der App mit Ihren Anmeldeinformationen aus folgenden Gründen vorzuziehen:

* Sie können der App-Identität Berechtigungen zuweisen, die sich von Ihren eigenen Berechtigungen unterscheiden. In der Regel sind diese Berechtigungen auf diejenigen beschränkt, die für die App erforderlich sind. Beispielsweise können Sie festlegen, dass die App in einer bestimmten Time Series Insights-Umgebung Daten nur lesen darf.
* So müssen Sie die Anmeldeinformationen für die App nicht ändern, wenn sich Ihre Zuständigkeiten ändern.
* Sie können ein Zertifikat oder einen Anwendungsschlüssel verwenden, um die Authentifizierung beim Ausführen eines unbeaufsichtigten Skripts zu automatisieren.

In diesem Artikel erfahren Sie, wie diese Schritte über das Azure-Portal ausgeführt werden. Der Schwerpunkt liegt dabei auf einer Anwendung mit nur einem Mandanten, die nur zur Ausführung in einer einzigen Organisation vorgesehen ist. Anwendungen mit nur einem Mandanten werden in der Regel für innerhalb Ihrer Organisation ausgeführte Branchenanwendungen verwendet.

Das Setup umfasst die folgenden drei Hauptschritte:

1. Erstellen einer Anwendung in Azure Active Directory.
2. Autorisieren dieser Anwendung zum Zugriff auf die Time Series Insights-Umgebung.
3. Verwenden der Anwendungs-ID und des Schlüssels zum Abrufen eines Tokens für die Benutzergruppe oder Ressource von `"https://api.timeseries.azure.com/"`. Mit dem Token kann dann die Time Series Insights-API aufgerufen werden.

Ausführliche Erläuterung der Schritte:

1. Wählen Sie im Azure-Portal **Azure Active Directory** > **App-Registrierungen** > **Registrierung einer neuen Anwendung** aus.

   ![Neue Anwendung in Azure Active Directory registrieren](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. Benennen Sie die Anwendung, wählen Sie als Typ **Web-App/API** aus, wählen Sie einen beliebigen gültigen URI unter **URL für Anmeldung** aus, und klicken Sie auf **Erstellen**.

   ![Anwendung in Azure Active Directory erstellen](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. Wählen Sie die neu erstellte Anwendung aus, und kopieren Sie die Anwendungs-ID in einen Text-Editor.

   ![Anwendungs-ID kopieren](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. Wählen Sie **Schlüssel** aus, geben Sie den Namen des Schlüssels ein, wählen Sie den Ablauf aus, und klicken Sie auf **Speichern**.

   ![Anwendungsschlüssel auswählen](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Schlüsselnamen und Ablauf eingeben und auf „Speichern“ klicken](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. Kopieren Sie den Schlüssel in einen Text-Editor.

   ![Anwendungsschlüssel kopieren](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. Wählen Sie für die Time Series Insights-Umgebung **Datenzugriffsrichtlinien** aus, und klicken Sie auf **Hinzufügen**.

   ![Neue Datenzugriffsrichtlinie zur Time Series Insights-Umgebung hinzufügen](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. Fügen Sie im Dialogfeld **Benutzer auswählen** den Anwendungsnamen (aus Schritt 2) oder die Anwendungs-ID (aus Schritt 3) ein.

   ![Anwendung im Dialogfeld „Benutzer auswählen“ suchen](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. Wählen Sie die Rolle aus (**Leser** zum Abfragen von Daten, **Mitwirkender** zum Abfragen von Daten und Ändern von Verweisdaten), und klicken Sie auf **OK**.

   ![Im Dialogfeld „Rolle auswählen“ Option „Leser“ oder „Mitwirkender“ auswählen](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. Speichern Sie die Richtlinie, indem Sie auf **OK** klicken.

10. Verwenden Sie die Anwendungs-ID (aus Schritt 3) und den Anwendungsschlüssel (aus Schritt 5), um das Token im Namen der Anwendung abzurufen. Das Token kann dann im `Authorization`-Header übergeben werden, wenn die Anwendung die Time Series Insights-API aufruft.

    Wenn Sie C# verwenden, können Sie mit dem folgenden Code das Token für die Anwendung abrufen. Ein vollständiges Beispiel finden Sie unter [Abfragen von Daten aus einer Azure Time Series Insights-Umgebung mit C#](time-series-insights-query-data-csharp.md).

    ```csharp
    // Enter your Active Directory tenant domain name
    var tenant = "YOUR_AD_TENANT.onmicrosoft.com";
    var authenticationContext = new AuthenticationContext(
        $"https://login.microsoftonline.com/{tenant}",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set the resource URI to the Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/", 
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

Verwenden Sie die Anwendungs-ID und den Anwendungsschlüssel in Ihrer Anwendung für die Authentifizierung bei Azure Time Series Insight. 

## <a name="next-steps"></a>Nächste Schritte
- Beispielcode, in dem die Time Series Insights-API aufgerufen wird, finden Sie unter [Abfragen von Daten aus einer Azure Time Series Insights-Umgebung mit C#](time-series-insights-query-data-csharp.md).
- API-Referenzinformationen finden Sie in der Referenz zur [Abfrage-API](/rest/api/time-series-insights/time-series-insights-reference-queryapi).

> [!div class="nextstepaction"]
> [Erstellen eines Dienstprinzipals](../active-directory/develop/howto-create-service-principal-portal.md)
