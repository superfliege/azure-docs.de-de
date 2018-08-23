---
title: Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichterstellungs-API | Microsoft-Dokumentation
description: Erfahren Sie, welche Voraussetzungen für den Zugriff auf die Azure AD-Berichterstellungs-API gelten.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 8d610dc74b7e2ef10295bc0a3407cf7c3d781b51
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "42143992"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichterstellungs-API

Die [Berichtserstellungs-APIs von Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) bieten Ihnen über eine Gruppe von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese APIs über verschiedene Programmiersprachen und Tools aufrufen.

Die Berichterstellungs-API verwendet [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) zum Autorisieren des Zugriffs auf die Web-APIs.

Um auf die Berichterstellungs-API zugreifen zu können, müssen Sie folgende Schritte ausführen:

1. Zuweisen von Rollen
2. Registrieren einer Anwendung
3. Erteilen von Berechtigungen
4. Erfassen von Konfigurationseinstellungen



## <a name="assign-roles"></a>Zuweisen von Rollen

Um mithilfe der API auf die Berichtsdaten zuzugreifen, müssen Ihnen eine der folgenden Rollen zugewiesen sein:

- Sicherheit lesen

- Sicherheitsadministrator

- Globaler Administrator




## <a name="register-an-application"></a>Registrieren einer Anwendung

Sie müssen Apps auch dann registrieren, wenn Sie mithilfe eines Skripts auf die Berichterstellungs-API zugreifen. Dadurch wird Ihnen eine **Anwendungs-ID** zugewiesen, die für einen Autorisierungsaufruf erforderlich ist und den Empfang von Tokens über Ihren Code ermöglicht.

Um Ihr Verzeichnis für den Zugriff auf die Azure AD-Berichterstellungs-API zu konfigurieren, müssen Sie sich beim Azure-Portal mit dem Konto eines Azure-Administrators anmelden, das auch der Verzeichnisrolle **Globaler Administrator** Ihres Azure AD-Mandanten angehört.

> [!IMPORTANT]
> Clientanwendungen, die wie hier unter Anmeldeinformationen mit Administratorberechtigungen ausgeführt werden, können äußerst einflussreich sein. Sorgen Sie also unbedingt dafür, dass die ID und geheimen Anmeldeinformationen der Anwendung stets gut geschützt sind.
> 


**So registrieren Sie eine Azure Active Directory-Anwendung:**

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Klicken Sie auf der Seite **Azure Active Directory** auf **App-Registrierungen**.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. Klicken Sie auf der Seite **App-Registrierungen** auf der Symbolleiste am oberen Rand auf **Registrierung einer neuen Anwendung**.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. Führen Sie auf der Seite **Erstellen** die folgenden Schritte aus:

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. Geben Sie im Textfeld **Name** Folgendes ein: `Reporting API application`.

    b. Wählen Sie als **Anwendungstyp** **Web-App/API** aus.

    c. Geben Sie im Textfeld **Anmelde-URL** die URL `https://localhost` ein.

    d. Klicken Sie auf **Create**. 


## <a name="grant-permissions"></a>Erteilen von Berechtigungen 

Abhängig von der API, auf die Sie zugreifen möchten, müssen Sie Ihrer App die folgenden Berechtigungen erteilen:  

| API | Berechtigung |
| --- | --- |
| Microsoft Azure Active Directory | Verzeichnisdaten lesen |
| Microsoft Graph | Alle Überwachungsprotokolldaten lesen |


![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/36.png)


Im folgenden Abschnitt werden die Schritte für beide APIs aufgelistet. Wenn Sie nicht auf eine der APIs zugreifen möchten, können Sie die zugehörigen Schritte überspringen.
 

**So erteilen Sie Ihrer Anwendung die Berechtigung zur Verwendung der APIs**:

1. Klicken Sie auf der Seite **App-Registrierungen** in der App-Liste auf **Berichterstellungs-API-Anwendung**.

2. Klicken Sie auf der Seite **Berichterstellungs-API-Anwendung** auf der Symbolleiste am oberen Rand auf **Einstellungen**. 

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/05.png)

3. Klicken Sie auf der Seite **Einstellungen** auf **Erforderliche Berechtigungen**. 

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/06.png)

4. Klicken Sie auf der Seite **Erforderliche Berechtigungen** in der **API**-Liste auf **Microsoft Azure Active Directory**. 

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/07.png)

5. Wählen Sie auf der Seite **Zugriff aktivieren** die Option **Verzeichnisdaten lesen** aus, und deaktivieren Sie **Anmelden und Benutzerprofil lesen**. 

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/08.png)

6. Klicken Sie auf der Symbolleiste am oberen Rand auf **Speichern**.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/15.png)

7. Klicken Sie auf der Seite **Erforderliche Berechtigungen** auf der Symbolleiste am oberen Rand auf **Hinzufügen**.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/32.png)

8. Klicken Sie auf der Seite **API-Zugriff hinzufügen** auf **API auswählen**.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/31.png)

9. Klicken Sie auf der Seite **API auswählen** auf **Microsoft Graph** und dann auf **Auswählen**.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/33.png)

10. Wählen Sie auf der Seite **Zugriff aktivieren** die Option **Alle Überwachungsprotokolldaten lesen** aus, und klicken Sie dann auf **Auswählen**.  

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/34.png)


11. Klicken Sie auf der Seite **API-Zugriff hinzufügen** auf **Fertig**.  

12. Klicken Sie auf der Seite **Erforderliche Berechtigungen** auf der Symbolleiste am oberen Rand auf **Berechtigungen erteilen** und dann auf **Ja**.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/17.png)


## <a name="gather-configuration-settings"></a>Erfassen von Konfigurationseinstellungen 

In diesem Abschnitt wird gezeigt, wie Sie die folgenden Einstellungen aus Ihrem Verzeichnis abrufen:

- Domänenname
- Client-ID
- Geheimer Clientschlüssel

Sie benötigen diese Werte, um Aufrufe an die Berichterstellungs-API zu konfigurieren. 

### <a name="get-your-domain-name"></a>Ermitteln des Domänennamens

**So ermitteln Sie den Domänennamen:**

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Klicken Sie auf der Seite **Azure Active Directory** auf **Benutzerdefinierte Domänennamen**.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Kopieren Sie Ihren Domänennamen aus der Liste der Domänen.


### <a name="get-your-applications-client-id"></a>Abrufen der Client-ID der Anwendung

**So rufen Sie die Client-ID der Anwendung ab:**

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Klicken Sie auf der Seite **App-Registrierungen** in der App-Liste auf **Berichterstellungs-API-Anwendung**.

3. Klicken Sie auf der Seite **Berichterstellungs-API-Anwendung** unter **Anwendungs-ID** auf **Zum Kopieren klicken**.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/11.png) 



### <a name="get-your-applications-client-secret"></a>Abrufen des geheimen Clientschlüssels der Anwendung
Um den geheimen Clientschlüssel Ihrer Anwendung abzurufen, müssen Sie einen neuen Schlüssel erstellen und dessen Wert beim Speichern des neuen Schlüssels speichern, weil dieser Wert später nicht mehr abgerufen werden kann.

**So rufen Sie den geheimen Clientschlüssel der Anwendung ab:**

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Klicken Sie auf der Seite **App-Registrierungen** in der App-Liste auf **Berichterstellungs-API-Anwendung**.


3. Klicken Sie auf der Seite **Berichterstellungs-API-Anwendung** auf der Symbolleiste am oberen Rand auf **Einstellungen**. 

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/05.png)

4. Klicken Sie auf der Seite **Einstellungen** im Abschnitt **APIR-Zugriff** auf **Schlüssel**. 

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/12.png)


5. Führen Sie auf der Seite **Schlüssel** die folgenden Schritte durch:

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/14.png)

    a. Geben Sie im Textfeld **Beschreibung** Folgendes ein: `Reporting API`.

    b. Wählen Sie für **Läuft ab** die Option **In 2 Jahren** aus.

    c. Klicken Sie auf **Speichern**.

    d. Kopieren Sie den Schlüsselwert.


## <a name="next-steps"></a>Nächste Schritte

* [Abrufen von Daten per Berichtserstellungs-API von Azure Active Directory mit Zertifikaten](tutorial-access-api-with-certificates.md)
* [Verschaffen eines ersten Eindrucks über die Berichterstellungs-APIs](concept-reporting-api.md)
* [Referenz zur Überwachungs-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Referenz zur Anmeldeaktivitätsbericht-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
