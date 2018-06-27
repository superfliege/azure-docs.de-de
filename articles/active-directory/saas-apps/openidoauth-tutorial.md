---
title: Schritte zum Konfigurieren einer OpenID/OAuth-Anwendung aus dem Azure AD-App-Katalog | Microsoft-Dokumentation
description: Schritte zum Konfigurieren einer OpenID/OAuth-Anwendung aus dem Azure AD-App-Katalog
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: eedebb76-e78c-428f-9cf0-5891852e79fb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 176af41197810059a17daf5ab09d29e0169e9640
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36225006"
---
# <a name="steps-to-configure-an-openidoauth-application-from-azure-ad-app-gallery"></a>Schritte zum Konfigurieren einer OpenID/OAuth-Anwendung aus dem Azure AD-App-Katalog

## <a name="process-of-open-id-application-addition-from-gallery"></a>Hinzufügen einer OpenID-Anwendung aus dem Katalog

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“](./media/openidoauth-tutorial/tutorial_general_01.png)

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](./media/openidoauth-tutorial/tutorial_general_02.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](./media/openidoauth-tutorial/tutorial_general_03.png)

4. Geben Sie im Suchfeld den **Anwendungsnamen** ein, wählen Sie im Ergebnisbereich die **gewünschte Anwendung** aus, und melden Sie sich bei der Anwendung an.

    ![Hinzufügen der Anwendung](./media/openidoauth-tutorial/addfromgallery.png)

    > [!NOTE]
    > Für OpenID Connect- und OAuth-Anwendungen ist die Schaltfläche „Hinzufügen“ standardmäßig deaktiviert. Hier muss der Mandantenadministrator auf die Schaltfläche **für die Registrierung** klicken und die Anwendung zulassen. So wird die Anwendung dem Kundenmandanten hinzugefügt. Ein explizites Hinzufügen und Konfigurieren ist nicht erforderlich.

    ![Schaltfläche „Hinzufügen“](./media/openidoauth-tutorial/addbutton.png)

5. Wenn Sie auf den Link zur Registrierung klicken, werden Sie auf die Azure AD-Seite mit den Anmeldeinformationen umgeleitet.

6. Nach der erfolgreicher Authentifizierung muss der Benutzer auf der Zustimmungsseite seine Erlaubnis erteilen. Danach wird die Startseite der Anwendung angezeigt.

    > [!NOTE]
    > Kunden dürfen nur eine Instanz der Anwendung hinzufügen. Wenn Sie bereits eine Instanz hinzugefügt haben und danach noch einmal versuchen, Ihre Zustimmung zu erteilen, wird die Anwendung dem Mandanten nicht erneut hinzugefügt. Das heißt, Sie können im Mandanten nur eine Anwendungsinstanz verwenden.

## <a name="authentication-flow-using-openid-connect"></a>Authentifizierungsfluss bei OpenID Connect

Der grundlegende Anmeldevorgang umfasst die folgenden Schritte – sie werden unten jeweils im Detail beschrieben.

![Authentifizierungsfluss bei OpenID Connect](./media/openidoauth-tutorial/authenticationflow.png)

* **Mehrinstanzenfähige Anwendung**: Eine mehrinstanzenfähige Anwendung ist für die Verwendung in mehreren Organisationen vorgesehen. Hierbei handelt es sich in der Regel um SaaS-Anwendungen (Software as a Service), die von einem unabhängigen Softwarehersteller (Independent Software Vendor, ISV) geschrieben wurden. Mehrinstanzenfähige Anwendungen müssen in jedem Verzeichnis bereitgestellt werden, in dem sie verwendet werden. Für die Registrierung ist also jeweils die Zustimmung des Benutzers oder Administrators erforderlich. Dieser Zustimmungsprozess beginnt, wenn eine Anwendung im Verzeichnis registriert wurde und Zugriff auf die Graph-API (oder ggf. eine andere Web-API) erhält. Wenn sich ein Benutzer oder Administrator aus einer anderen Organisation für die Anwendung registriert, erscheint ein Dialogfeld mit den erforderlichen Berechtigungen für die Anwendung. Stimmt der Benutzer oder Administrator der Anwendung zu, erhält die Anwendung Zugriff auf die angegebenen Daten und wird schließlich im Verzeichnis des Benutzers oder Administrators registriert.

    > [!NOTE]
    > Wenn Sie Ihre Anwendung Benutzern in mehreren Verzeichnissen bereitstellen, müssen Sie ermitteln, in welchem Mandaten sie sich befinden. Eine Einzelinstanzanwendung muss nur im eigenen Verzeichnis nach einem Benutzer suchen. Eine mehrinstanzenfähige Anwendung muss dagegen einen bestimmten Benutzer in sämtlichen Verzeichnissen in Azure AD identifizieren. Für diese Aufgabe bietet Azure AD anstelle eines mandantenspezifischen Endpunkts einen gemeinsamen Authentifizierungsendpunkt, an den jede mehrinstanzenfähige Anwendung Anmeldeanforderungen richten kann. Dieser Endpunkt lautet für alle Verzeichnisse in Azure AD [https://login.microsoftonline.com/common](https://login.microsoftonline.com/common). Im Gegensatz dazu sieht ein mandantenspezifischer Endpunkt beispielsweise wie folgt aus: [https://login.microsoftonline.com/contoso.onmicrosoft.com](https://login.microsoftonline.com/contoso.onmicrosoft.com). Der gemeinsame Endpunkt muss insbesondere bei der Anwendungsentwicklung berücksichtigt werden, da Sie die Logik implementieren müssen, die die Verarbeitung mehrerer Mandanten bei der Anmeldung, Abmeldung und Tokenüberprüfung ermöglicht.

Das Azure AD-Team empfiehlt standardmäßig mehrinstanzenfähige Anwendungen, da auf diese problemlos über verschiedene Organisationen zugegriffen werden kann und sie nach der Zustimmung einfach zu bedienen sind.

## <a name="what-is-consent-framework"></a>Was ist ein Zustimmungsframework?

Das Azure AD-Zustimmungs-Framework vereinfacht die Entwicklung von mehrinstanzenfähigen Webanwendungen und nativen Clientanwendungen. Diese Anwendungen ermöglichen die Anmeldung mit Benutzerkonten über einen Azure AD-Mandanten, der sich von dem Mandanten unterscheidet, unter dem die Anwendung registriert ist. Außerdem kann der Zugriff auf Web-APIs, z.B. die Microsoft Graph-API (zum Zugreifen auf Azure Active Directory, Intune und Office 365-Dienste), und andere APIs von Microsoft-Diensten zusätzlich zu Ihren eigenen Web-APIs erforderlich sein. Das Framework basiert darauf, dass Benutzer oder Administratoren ihre Zustimmung zur Registrierung einer Anwendung in ihrem Verzeichnis erteilen. Diese Zustimmung kann auch den Zugriff auf Verzeichnisdaten umfassen. Nach der Zustimmungserteilung kann die Clientanwendung die Microsoft Graph-API im Namen des Benutzers aufrufen und die Informationen nach Bedarf verwenden.

Die [Microsoft Graph-API](https://graph.microsoft.io/) ermöglicht den Zugriff auf Daten in Office 365 (z.B. Kalender und Nachrichten aus Exchange, Websites und Listen aus SharePoint, Dokumente aus OneDrive, OneNote-Notizbücher, Aufgaben aus dem Planer, Arbeitsmappen aus Excel usw.) sowie auf Benutzer und Gruppen von Azure AD und andere Datenobjekte aus weiteren Microsoft Cloud Services.

Die folgenden Schritte zeigen, wie das Zustimmungs-Framework in der Benutzeroberfläche für den Anwendungsentwickler und für den Benutzer umgesetzt wird.

1. Angenommen, Sie verfügen über eine Webclientanwendung, die für den Zugriff auf eine Ressource oder API bestimmte Berechtigungen anfordern muss. Das Azure-Portal wird verwendet, um zur Konfigurationszeit Berechtigungsanforderungen zu deklarieren. Wie andere Konfigurationseinstellungen auch, werden diese Teil der Azure AD-Registrierung der Anwendung:

    ![Graph-API](./media/openidoauth-tutorial/graphapi.png)

2. Angenommen, die Berechtigungen für Ihre Anwendung wurden aktualisiert, die Anwendung wird ausgeführt, und ein Benutzer möchte die Anwendung zum ersten Mal verwenden. Zuerst muss die Anwendung einen Autorisierungscode vom Azure AD-Endpunkt „/authorize“ anfordern. Der Autorisierungscode kann dann genutzt werden, um ein neues Zugriffs- und Aktualisierungstoken zu beschaffen.

3. Wenn der Benutzer noch nicht authentifiziert ist, wird er vom Azure AD-Endpunkt „/authorize“ zur Anmeldung aufgefordert.

    ![Authentifizierung](./media/openidoauth-tutorial/authentication.png)

4. Nachdem der Benutzer sich angemeldet hat, ermittelt Azure AD, ob für den Benutzer eine Seite für die Zustimmungserteilung angezeigt werden muss. Das Ergebnis dieser Ermittlung ist davon abhängig, ob der Benutzer (oder der Administrator seiner Organisation) die Anwendungszustimmung bereits erteilt hat. Wenn die Zustimmung noch nicht erteilt wurde, fordert Azure AD den Benutzer zur Zustimmung auf und zeigt an, welche Berechtigungen für die Funktionalität benötigt werden. Der Berechtigungssatz, der im Dialogfeld für die Zustimmung angezeigt wird, ist identisch mit der Auswahl im Azure-Portal unter „Delegierte Berechtigungen“.

    ![Zustimmungsseite](./media/openidoauth-tutorial/consentpage.png)

Einigen Berechtigungen kann ein regulärer Benutzer zustimmen, während andere die Zustimmung eines Mandantenadministrators erfordern.

## <a name="whats-the-difference-between-admin-consent-and-user-consent"></a>Was ist der Unterschied zwischen einer Zustimmung durch den Administrator und einer Zustimmung durch den Benutzer?

Als Administrator können Sie auch für alle Benutzer in Ihrem Mandanten den delegierten Berechtigungen einer Anwendung zustimmen. Durch die Administratorzustimmung wird verhindert, dass das Zustimmungsdialogfeld für jeden Benutzer im Mandanten angezeigt wird. Dies kann im Azure-Portal von Benutzern durchgeführt werden, die über die Administratorrolle verfügen. Klicken Sie auf der Seite „Einstellungen“ für Ihre Anwendung auf „Erforderliche Berechtigungen“ und dann auf die Schaltfläche „Berechtigungen erteilen“.

![Berechtigung erteilen](./media/openidoauth-tutorial/grantpermission.png)

> [!NOTE]
> Das explizite Gewähren der Zustimmung über die Schaltfläche „Berechtigungen erteilen“ ist derzeit für Single-Page-Webanwendungen (SPA) erforderlich, die „ADAL.js“ nutzen. Andernfalls tritt für die Anwendung ein Fehler auf, wenn das Zugriffstoken angefordert wird.

Nur für die App geltende Berechtigungen erfordern immer die Zustimmung eines Mandantenadministrators. Wenn die Anwendung eine nur für die App geltende Berechtigung anfordert und ein Benutzer versucht, sich bei der Anwendung anzumelden, wird eine Fehlermeldung angezeigt, die besagt, dass der Benutzer nicht zustimmen kann.

Wenn Ihre Anwendung Berechtigungen nutzt, die die Zustimmung des Administrators erfordern, müssen Sie z.B. eine Schaltfläche oder einen Link implementieren, damit der Administrator die Aktion initiieren kann. Die Anforderung, die die Anwendung für diese Aktion sendet, ist die reguläre Autorisierungsanforderung für OAuth2/OpenID Connect, die zusätzlich den Parameter der Abfragezeichenfolge „prompt=admin_consent“ enthält. Nachdem der Administrator zugestimmt hat und der Dienstprinzipal im Mandanten des Kunden erstellt wurde, ist für nachfolgende Anmeldeanforderungen der Parameter „prompt=admin_consent“ nicht mehr erforderlich. Da der Administrator entschieden hat, dass die angeforderten Berechtigungen zulässig sind, werden von diesem Zeitpunkt an keine anderen Benutzer im Mandanten zur Zustimmung aufgefordert. Ein Mandantenadministrator kann die Funktion deaktivieren, dass reguläre Benutzer Anwendungen zustimmen. Wenn diese Funktion deaktiviert ist, ist die Zustimmung des Administrators immer erforderlich, damit die Anwendung im Mandanten verwendet werden kann. Wenn Sie Ihre Anwendung mit deaktivierter Endbenutzerzustimmung testen möchten, können Sie die Konfiguration im [Azure-Portal](https://portal.azure.com/) im Abschnitt [Benutzereinstellungen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) unter **Unternehmensanwendungen** ändern.

Der Parameter „prompt=admin_consent“ kann auch von Anwendungen verwendet werden, die Berechtigungen anfordern, die keine Administratorzustimmung erfordern. Ein Beispiel für einen Anwendungsfall lautet wie folgt: Für die Anwendung ist ein Vorgang erforderlich, bei dem sich der Administrator des Mandanten einmal registriert, und danach werden keine anderen Benutzer mehr zur Zustimmung aufgefordert.

Wenn für eine Anwendung die Zustimmung des Administrators erforderlich ist und sich ein Administrator anmeldet, ohne dass der Parameter „prompt=admin_consent“ gesendet wird, gilt die erfolgreiche Zustimmung des Administrators nur für das jeweilige Benutzerkonto. Reguläre Benutzer können sich weiterhin nicht anmelden und der Anwendung nicht zustimmen. Diese Funktion ist sinnvoll, wenn Sie dem Mandantenadministrator die Möglichkeit geben möchten, Ihre Anwendung zu untersuchen, bevor Sie anderen Benutzern Zugriff gewähren.