---
title: 'Tutorial: Anpassen der Benutzeroberfläche Ihrer Anwendungen in Azure Active Directory B2C | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie die Benutzeroberfläche Ihrer Anwendungen in Azure Active Directory B2C mithilfe des Azure-Portals anpassen.
services: B2C
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 588ce454248f0577a52515a4327d1e43013d34a5
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52581798"
---
# <a name="tutorial-customize-the-user-interface-of-your-applications-in-azure-active-directory-b2c"></a>Tutorial: Anpassen der Benutzeroberfläche Ihrer Anwendungen in Azure Active Directory B2C

Für weitere gängige Benutzererfahrungen wie Registrierung, Anmeldung und Profilbearbeitung können Sie [integrierte Richtlinien](active-directory-b2c-reference-policies.md) in Azure Active Directory (Azure AD) B2C verwenden. Die Informationen in diesem Tutorial helfen Ihnen beim [Anpassen der Benutzeroberfläche (UI)](customize-ui-overview.md) dieser Erfahrungen unter Verwendung Ihrer eigenen HTML- und CSS-Dateien.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen von Dateien für die Benutzeroberflächenanpassung
> * Erstellen einer Registrierungs- und Anmelderichtlinie, die die Dateien verwendet
> * Testen der angepassten Benutzeroberfläche

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie Ihren eigenen [Azure AD B2C-Mandanten](tutorial-create-tenant.md) noch nicht erstellt haben, erstellen Sie jetzt einen. Sie können einen vorhandenen Mandanten verwenden, wenn Sie einen in einem vorherigen Tutorial erstellt haben.

## <a name="create-customization-files"></a>Erstellen von Anpassungsdateien

Sie erstellen ein Azure-Speicherkonto mit Container und legen dann einfache HTML- und CSS-Dateien in dem Container ab.

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Zwar können Sie Ihre Dateien auf viele Arten speichern, doch für dieses Tutorial speichern Sie sie in [Azure-Blob-Speicher](../storage/blobs/storage-blobs-introduction.md).

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihr Azure-Abonnement enthält. Wählen Sie im Hauptmenü den **Verzeichnis- und Abonnementfilter** aus, und wählen Sie das Verzeichnis aus, das Ihr Abonnement enthält. Dabei handelt es sich um ein anderes Verzeichnis als das, in dem Ihr Azure B2C-Mandant enthalten ist.

    ![Wechseln zum Abonnementverzeichnis](./media/tutorial-customize-ui/switch-directories.png)

2. Wählen Sie links oben im Azure-Portal „Alle Dienste“ aus, und suchen Sie dann nach **Speicherkonten**, und wählen Sie dies aus. 
3. Wählen Sie **Hinzufügen**.
4. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, geben Sie einen Namen für die neue Ressourcengruppe ein, und klicken Sie dann auf **OK**.
5. Geben Sie einen Namen für das Speicherkonto ein. Der ausgewählte Name muss in Azure eindeutig sein und zwischen 3 und 24 Zeichen aufweisen, und er darf nur Zahlen und Kleinbuchstaben enthalten.
6. Wählen Sie den Standort Ihres Speicherkontos aus, oder akzeptieren Sie den Standardstandort. 
7. Akzeptieren Sie alle anderen Standardwerte, wählen Sie **Überprüfen + erstellen** aus, und klicken Sie dann auf **Weiter**.
8. Wählen Sie nach dem Erstellen des neuen Speicherkontos **Zu Ressource wechseln** aus.

### <a name="create-a-container"></a>Erstellen eines Containers

1. Wählen Sie auf der Übersichtsseite des Speicherkontos **Blobs** aus.
2. Wählen Sie **Container** aus, geben Sie einen Namen für den Container ein, wählen Sie **Blob (anonymer Lesezugriff nur für Blobs)** aus, und klicken Sie dann auf **OK**.

### <a name="enable-cors"></a>Aktivieren von CORS

 Azure AD B2C-Code in einem Browser verwendet einen modernen Standardansatz zum Laden benutzerdefinierter Inhalte über eine URL, die Sie in einer Richtlinie angeben. Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) ermöglicht, dass eingeschränkte Ressourcen auf einer Webseite aus anderen Domänen angefordert werden können.

1. Wählen Sie im Menü **CORS** aus.
2. Geben Sie für **Zulässige Ursprünge** den Wert `https://your-tenant-name.b2clogin.com` ein. Ersetzen Sie `your-tenant-name` durch den Namen des Azure AD B2C-Mandanten. Beispiel: `https://fabrikam.b2clogin.com`. Sie dürfen bei der Eingabe Ihres Mandantennamens ausschließlich Kleinbuchstaben verwenden.
3. Wählen Sie für **Zulässige Methoden** sowohl `GET` als auch `OPTIONS` aus.
4. Geben Sie für **Zulässige Header** ein Sternchen (*) ein.
5. Geben Sie für **Verfügbar gemachte Header** ein Sternchen (*) ein.
6. Für **Max. Alter** geben Sie 200 ein.

    ![Aktivieren von CORS](./media/tutorial-customize-ui/enable-cors.png)

5. Klicken Sie auf **Speichern**.

### <a name="create-the-customization-files"></a>Erstellen der Anpassungsdateien

Um die Benutzeroberfläche der Registrierungserfahrung anzupassen, beginnen Sie mit dem Erstellen einer einfache HTML- und CSS-Datei. Sie können Ihren HTML-Code beliebig konfigurieren, aber er muss ein **div**-Element mit einem Bezeichner von `api` enthalten. Beispiel: `<div id="api"></div>`. Azure AD B2C fügt Elemente in den `api`-Container ein, wenn die Seite angezeigt wird.

1. Erstellen Sie die folgende Datei in einem lokalen Ordner, und stellen Sie sicher, dass Sie `your-storage-account` in den Namen des Speicherkontos ändern und `your-container` in den Namen des Containers, den Sie erstellt haben. Beispiel: `https://store1.blob.core.windows.net/b2c/style.css`.

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>My B2C Application</title>
        <link rel="stylesheet" href="https://your-storage-account.blob.core.windows.net/your-container/style.css">
      </head>
      <body>  
        <h1>My B2C Application</h1>
        <div id="api"></div>
      </body>
    </html>
    ```

    Die Seite kann entworfen werden, wie Sie möchten, aber das **api** div-Element ist für jede HTML-Anpassungsdatei, die Sie erstellen, erforderlich. 

3. Speichern Sie die Datei als *custom-ui.html*.
4. Erstellen Sie den folgenden einfachen CSS-Code, der alle Elemente auf der Registrierungs- oder Anmeldeseite, einschließlich der Elemente, die Azure AD B2C einfügt, zentriert.

    ```css
    h1 {
      color: blue;
      text-align: center;
    }
    .intro h2 {
      text-align: center; 
    }
    .entry {
      width: 300px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    .divider h2 {
      text-align: center; 
    }
    .create {
      width: 300px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    ```

5. Speichern Sie die Datei als *style.css*.

### <a name="upload-the-customization-files"></a>Hochladen der Anpassungsdateien

In diesem Tutorial speichern Sie die Dateien, die Sie im Speicherkonto erstellt haben, damit Azure AD B2C darauf zugreifen können.

1. Wählen Sie links oben im Azure-Portal **Alle Dienste** aus, und suchen Sie dann nach **Speicherkonten**, und wählen Sie dies aus.
2. Wählen Sie das von Ihnen erstellte Speicherkonto aus, wählen Sie **Blobs** aus, und wählen Sie dann den Container aus, den Sie erstellt haben.
3. Wählen Sie **Hochladen** aus, navigieren Sie zur Datei *custom-ui.html*, wählen Sie sie aus, und klicken Sie dann auf **Hochladen**.

    ![Hochladen von Anpassungsdateien](./media/tutorial-customize-ui/upload-blob.png)

4. Kopieren Sie die URL für die Datei, die Sie hochgeladen haben, um sie später in diesem Tutorial zu verwenden.
5. Wiederholen Sie Schritt 3 und 4 für die Datei *style.css*.

## <a name="create-a-sign-up-and-sign-in-policy"></a>Erstellen einer Registrierungs- und Anmelderichtlinie

Um die Schritte in diesem Tutorial abzuschließen, müssen Sie eine Testanwendung und eine Registrierungs- oder Anmelderichtlinie in Azure AD B2C erstellen. Sie können die in diesem Tutorial beschriebenen Prinzipien auf die anderen Erfahrungen anwenden, z. B. die Profilbearbeitung.

### <a name="create-an-azure-ad-b2c-application"></a>Erstellen einer Azure AD B2C-Anwendung

Die Kommunikation mit Azure AD B2C erfolgt über eine Anwendung, die Sie in Ihrem Mandanten erstellen. Mit den folgenden Schritten wird eine Anwendung erstellt, die das Autorisierungstoken, das zurückgegeben wird, an [https://jwt.ms](https://jwt.ms) umleitet.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
3. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
4. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
5. Geben Sie einen Namen für die Anwendung ein, z.B. *testapp1*.
6. Wählen Sie für **Web-App/Web-API** die Option `Yes` aus, und geben Sie dann für die **Antwort-URL** die Zeichenfolge `https://jwt.ms` ein.
7. Klicken Sie auf **Create**.

### <a name="create-the-policy"></a>Erstellen der Richtlinie

Um Ihre Anpassungsdateien zu testen, erstellen Sie eine integrierte Registrierungs- oder Anmelderichtlinie, die die Anwendung verwendet, die Sie zuvor erstellt haben.

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten die Option **Registrierungs- oder Anmelderichtlinien** aus, und klicken Sie dann auf **Hinzufügen**.
2. Geben Sie einen Namen für die Richtlinie ein. Z. B. *signup_signin*. Das Präfix *B2C_1* wird dem Namen automatisch hinzugefügt, wenn die Richtlinie erstellt wird.
3. Wählen Sie **Identitätsanbieter** aus, legen Sie **E-Mail-Registrierung** für ein lokales Konto fest, und klicken Sie dann auf **OK**.
4. Wählen Sie **Registrierungsattribute** aus, und wählen Sie die Attribute aus, die Sie vom Kunden bei der Registrierung erfassen möchten. Legen Sie z. B. **Land/Region**, **Anzeigename** und **Postleitzahl** fest, und klicken Sie dann auf **OK**.
5. Wählen Sie **Anwendungsansprüche** aus, wählen Sie die Ansprüche aus, die in den Autorisierungstoken nach einer erfolgreichen Registrierung oder Anmeldung an Ihre Anwendung zurückgegeben werden sollen. Wählen Sie z.B. **Anzeigename**, **Identitätsanbieter**, **Postleitzahl**, **Benutzer ist neu** und **Objekt-ID des Benutzers** aus, und klicken Sie dann auf **OK**.
6. Wählen Sie **Seite für die Benutzeroberflächenanpassung** aus, wählen Sie **Seite für einheitliche Registrierung oder Anmeldung** aus, und klicken Sie dann auf **Ja** für **Benutzerdefinierte Seite verwenden**.
7. Geben Sie in **Benutzerdefinierter Seiten-URI** die URL für die Datei *custom-ui.html* ein, die Sie zuvor aufgezeichnet haben, und klicken Sie dann auf **OK**.
8. Klicken Sie auf **Create**.

## <a name="test-the-policy"></a>Testen der Richtlinie

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten **Registrierungs- oder Anmelderichtlinien** aus, und wählen Sie dann die Richtlinie aus, die Sie erstellt haben. Z. B. *B2C_1_signup_signin*.
2. Stellen Sie sicher, dass die von Ihnen erstellte Anwendung im Feld **Anwendung auswählen** ausgewählt ist, und klicken Sie dann auf **Jetzt ausführen**.

    ![Ausführen der Registrierungs- oder Anmelderichtlinie](./media/tutorial-customize-ui/signup-signin.png)

    Es sollte eine Seite ähnlich dem folgenden Beispiel mit den zentrierten Elementen angezeigt werden, basierend auf der CSS-Datei, die Sie erstellt haben:

    ![Richtlinienergebnisse](./media/tutorial-customize-ui/run-now.png) 

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen von Dateien für die Benutzeroberflächenanpassung
> * Erstellen einer Registrierungs- und Anmelderichtlinie, die die Dateien verwendet
> * Testen der angepassten Benutzeroberfläche

> [!div class="nextstepaction"]
> [Sprachanpassung in Azure Active Directory B2C](active-directory-b2c-reference-language-customization.md)