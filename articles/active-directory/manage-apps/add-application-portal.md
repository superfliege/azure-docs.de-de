---
title: Hinzufügen einer App zum Azure Active Directory-Mandanten | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung wird mithilfe des Azure-Portals eine Kataloganwendung zu Ihrem Azure Active Directory-Mandanten (Azure AD) hinzugefügt.
services: active-directory
author: barbkess
manager: daveba
ms.service: active-directory
ms.component: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 07/24/2018
ms.author: barbkess
ms.openlocfilehash: 0f2f912504ecf32ff5ba337951e24da0f816d463
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54461925"
---
# <a name="quickstart-add-an-application-to-your-azure-active-directory-tenant"></a>Schnellstart: Hinzufügen einer Anwendung zu Ihrem Azure Active Directory-Mandanten

Azure Active Directory (Azure AD) enthält einen Katalog mit Tausenden von vorab integrierten Anwendungen. Einige der von Ihrer Organisation verwendeten Anwendungen sind wahrscheinlich im Katalog enthalten. In dieser Schnellstartanleitung wird mithilfe des Azure-Portals eine Kataloganwendung zu Ihrem Azure Active Directory-Mandanten (Azure AD) hinzugefügt.

Nach dem Hinzufügen einer Anwendung zum Azure AD-Mandanten haben Sie folgende Möglichkeiten:

- Verwalten des Benutzerzugriffs auf die Anwendung mit einer Richtlinie für bedingten Zugriff
- Konfigurieren von Benutzern für einmaliges Anmelden bei der Anwendung mit ihren Azure AD-Konten

## <a name="before-you-begin"></a>Voraussetzungen

Zum Hinzufügen einer Anwendung zu Ihrem Mandanten benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Abonnement für Ihre Anwendung, für das einmaliges Anmelden aktiviert ist

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator für Ihren Azure AD-Mandanten, als Cloudanwendungsadministrator oder als Anwendungsadministrator an.

Es wird empfohlen, zum Testen der Schritte in diesem Tutorial keine Produktionsumgebung zu verwenden. Wenn Sie keine solche Umgebung besitzen, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-an-application-to-your-azure-ad-tenant"></a>Hinzufügen einer Anwendung zum Azure AD-Mandanten

So fügen Sie eine Kataloganwendung zu Ihrem Azure AD-Mandanten hinzu:

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**.

2. Klicken Sie auf dem Blatt **Azure Active Directory** auf **Unternehmensanwendungen**.

    ![Öffnen von „Unternehmensanwendungen“](media/add-application-portal/open-enterprise-apps.png)

3. Das Blatt **Alle Anwendungen** wird mit einer nach dem Zufallsprinzip ausgewählten Gruppe von Anwendungen in Ihrem Azure AD-Mandanten geöffnet.

    ![Blatt „Alle Anwendungen“](media/add-application-portal/applications-blade.png)


4. Klicken Sie oben auf dem Blatt **alle Anwendungen** auf **Neue Anwendung**.

    ![Neue Anwendung](media/add-application-portal/new-application.png)

5. Eine Liste der Anwendungen im Katalog zeigen Sie am besten über **Kategorien** an, da die Symbole unter **Ausgewählte Anwendungen** eine nach dem Zufallsprinzip ausgewählte Gruppe von Anwendungen darstellen.

    ![Suchen nach Name oder Kategorie](media/add-application-portal/categories.png)

    Wenn Sie weitere Anwendungen anzeigen möchten, klicken Sie auf **Mehr anzeigen**. Es wird nicht empfohlen, auf diese Weise zu suchen, da der Katalog Tausende Anwendungen enthält.

6. Geben Sie zum Suchen nach einer Anwendung unter **Aus Katalog hinzufügen** den Namen der hinzuzufügenden Anwendung ein. Wählen Sie die Anwendung in den Ergebnissen aus, und klicken Sie auf **Hinzufügen**. Das folgende Beispiel zeigt das Formular **App hinzufügen**, das nach dem Suchvorgang für GitHub.com angezeigt wird.

    ![Hinzufügen einer Anwendung](media/add-application-portal/add-an-application.png)

6. Im anwendungsspezifischen Formular können Sie Informationen zu Eigenschaften ändern. Beispielsweise können Sie den Namen der Anwendung entsprechend den Anforderungen Ihres Unternehmens bearbeiten. In diesem Beispiel wird der Name **GitHub-test** verwendet.

8. Wenn Sie die gewünschten Änderungen an den Eigenschaften vorgenommen haben, klicken Sie auf **Hinzufügen**.

9. Eine Seite mit ersten Schritten und den Optionen zum Konfigurieren der Anwendung für Ihre Organisation wird angezeigt.

    ![Menü „Erste Schritte“](media/add-application-portal/get-started.png)

Das Hinzufügen der Anwendung ist abgeschlossen. Sie können nun eine Pause einlegen. In den nächsten Abschnitten wird gezeigt, wie Sie das Logo ändern und weitere Eigenschaften für Ihre Anwendung bearbeiten.

## <a name="find-your-azure-ad-tenant-application"></a>Suchen nach Ihrer Azure AD-Mandantenanwendung

Angenommen, Sie mussten den Vorgang unterbrechen und möchten nun die Konfiguration Ihrer Anwendung fortsetzen. Zunächst müssen Sie die Anwendung suchen.

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf **Azure Active Directory**.

2. Klicken Sie auf dem Blatt „Azure Active Directory“ auf **Unternehmensanwendungen**.

3. Wählen Sie im Dropdownmenü **Anwendungstyp** die Option **Alle Anwendungen** aus, und klicken Sie auf **Anwenden**. Weitere Informationen zu den Anzeigeoptionen finden Sie unter [Anzeigen aller von Ihnen verwaltbaren Unternehmens-Apps in Azure Active Directory](view-applications-portal.md).

4. Sie können jetzt eine Liste aller Anwendungen im Azure AD-Mandanten anzeigen. Die Liste enthält eine nach dem Zufallsprinzip ausgewählte Gruppe. Klicken Sie zum Anzeigen weiterer Anwendungen mehrmals auf **Mehr anzeigen**.

5. Um schnell eine Anwendung in Ihrem Mandanten zu finden, geben Sie den Anwendungsnamen ins Suchfeld ein, und klicken Sie auf **Anwenden**. In diesem Beispiel wird nach der zuvor hinzugefügten Anwendung „GitHub-test“ gesucht.

    ![Suchen einer Anwendung](media/add-application-portal/find-application.png)


## <a name="configure-user-sign-in-properties"></a>Konfigurieren der Eigenschaften für die Benutzeranmeldung

Sie haben die Anwendung gefunden. Nun können Sie sie öffnen und Anwendungseigenschaften konfigurieren.

So bearbeiten Sie die Anwendungseigenschaften

1. Klicken Sie auf die Anwendung, um sie zu öffnen.
2. Klicken Sie auf **Eigenschaften**, um das Blatt „Eigenschaften“ zur Bearbeitung zu öffnen.

    ![Blatt „Eigenschaften bearbeiten“](media/add-application-portal/edit-properties.png)

3. Nehmen Sie sich einen Moment Zeit, um die Anmeldeoptionen nachzuvollziehen. Die Werte für **Aktiviert für die Benutzeranmeldung?**, **Benutzerzuweisung erforderlich?** und **Für Benutzer sichtbar?** bestimmen zusammen, ob sich der Anwendung zugewiesene oder nicht zugewiesene Benutzer anmelden können. Sie legen außerdem fest, ob der Benutzer die Anwendung im Zugriffsbereich sehen kann.

    - Mit **Aktiviert für die Benutzeranmeldung?** wird festgelegt, ob sich Benutzer, die der Anwendung zugewiesen sind, anmelden können.
    - Mit **Benutzerzuweisung erforderlich?** wird festgelegt, ob sich Benutzer, die der Anwendung nicht zugewiesen sind, anmelden können.
    - Mit **Für Benutzer sichtbar?** wird festgelegt, ob für der App zugewiesene Benutzer die Anwendung im Zugriffsbereich und im Office 365-Startfeld angezeigt wird.

4. Anhand der folgenden Tabellen können Sie die Optionen auswählen, die am besten für Ihre Anforderungen geeignet sind.

    - Verhalten für **zugewiesene** Benutzer:

        | Eigenschafteneinstellungen der Anwendung | | | Zugewiesene Benutzer | |
        |---|---|---|---|---|
        | Aktiviert für die Benutzeranmeldung? | Benutzerzuweisung erforderlich? | Für Benutzer sichtbar? | Können sich zugewiesene Benutzer anmelden? | Können zugewiesene Benutzer die Anwendung sehen?* |
        | Ja | Ja | Ja | Ja | Ja  |
        | Ja | Ja | no  | Ja | no   |
        | Ja | no  | Ja | Ja | Ja  |
        | Ja | no  | no  | Ja | no   |
        | no  | Ja | Ja | no  | no   |
        | no  | Ja | no  | no  | no   |
        | no  | no  | Ja | no  | no   |
        | no  | no  | no  | no  | no   |

    - Verhalten für **nicht zugewiesene** Benutzer:

        | Eigenschafteneinstellungen der Anwendung | | | Nicht zugewiesene Benutzer | |
        |---|---|---|---|---|
        | Aktiviert für die Benutzeranmeldung? | Benutzerzuweisung erforderlich? | Für Benutzer sichtbar? | Können sich nicht zugewiesene Benutzer anmelden? | Können nicht zugewiesene Benutzer die Anwendung sehen?* |
        | Ja | Ja | Ja | no  | no   |
        | Ja | Ja | no  | no  | no   |
        | Ja | no  | Ja | Ja | no   |
        | Ja | no  | no  | Ja | no   |
        | no  | Ja | Ja | no  | no   |
        | no  | Ja | no  | no  | no   |
        | no  | no  | Ja | no  | no   |
        | no  | no  | no  | no  | no   |

    *Kann der Benutzer die Anwendung im Zugriffsbereich und im Office 365-App-Startfeld sehen?

## <a name="use-a-custom-logo"></a>Verwenden eines benutzerdefinierten Logos

So verwenden Sie ein benutzerdefiniertes Logo:

1. Erstellen Sie ein Logo mit 215 x 215 Pixeln, und speichern Sie es im PNG-Format.
2. Da Sie Ihre Anwendung bereits gefunden haben, klicken Sie darauf.
2. Klicken Sie auf dem linken Blatt auf **Eigenschaften**.
4. Laden Sie das Logo hoch.
5. Klicken Sie abschließend auf **Speichern**.

    ![Ändern des Logos](media/add-application-portal/change-logo.png)


## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie erfahren, wie eine Kataloganwendung zu Ihrem Azure AD-Mandanten hinzugefügt wird. Sie haben gelernt, wie Sie die Eigenschaften für eine Anwendung bearbeiten.

Nun können Sie die Anwendung für einmaliges Anmelden konfigurieren.

> [!div class="nextstepaction"]
> [Konfigurieren von einmaligem Anmelden](configure-single-sign-on-portal.md)


