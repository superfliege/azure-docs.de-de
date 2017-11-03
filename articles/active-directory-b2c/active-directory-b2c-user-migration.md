---
title: "Azure Active Directory B2C: Ansätze für die Benutzermigration"
description: "Hier werden grundlegende und erweiterte Konzepte der Benutzermigration mithilfe der Graph-API (und optional mithilfe benutzerdefinierter Azure AD B2C-Richtlinien) erläutert."
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 10/04/2017
ms.author: yoelh
ms.openlocfilehash: f98f1826b492b8596f352b403b3b12775814c399
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2017
---
# <a name="azure-active-directory-b2c-user-migration"></a>Azure Active Directory B2C: Benutzermigration
Beim Migrieren Ihres Identitätsanbieters zu Azure Active Directory B2C (Azure AD B2C) müssen Sie unter Umständen auch das Benutzerkonto migrieren. In diesem Artikel wird erläutert, wie vorhandene Benutzerkonten von einem beliebigen Identitätsanbieter zu Azure AD B2C migriert werden. Der Artikel enthält keine verbindliche Anleitung, sondern beschreibt lediglich zwei der zahlreichen Möglichkeiten, die Ihnen zur Verfügung stehen. Der Entwickler ist jeweils dafür verantwortlich, dass ein Ansatz geeignet ist.

## <a name="user-migration-flows"></a>Benutzermigrationsmethoden
Mit Azure AD B2C können Sie Benutzer über die [Graph-API](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) migrieren. Für den Benutzermigrationsprozess stehen zwei Methoden zur Verfügung:

* **Prämigration**: Diese Methode trifft zu, wenn Sie entweder eindeutigen Zugriff auf die Anmeldeinformationen eines Benutzers haben (Benutzername und Kennwort) oder wenn die Anmeldeinformationen verschlüsselt sind und von Ihnen entschlüsselt werden können. Der Prozess der Prämigration umfasst das Lesen der Benutzer im alten Identitätsanbieter und das Erstellen neuer Konten im Azure AD B2C-Verzeichnis.

* **Prämigration und Kennwortzurücksetzung**: Diese Methode ist geeignet, wenn auf das Kennwort eines Benutzers nicht zugegriffen werden kann. Beispiel:
    * Das Kennwort wird im HASH-Format gespeichert.
    * Das Kennwort wird in einem Identitätsanbieter gespeichert, auf den Sie nicht zugreifen können. Der alte Identitätsanbieter überprüft die Benutzeranmeldeinformationen durch das Aufrufen eines Webdiensts.

Bei beiden Methoden führen Sie zunächst den Prozess für die Prämigration aus: Sie lesen die Benutzer im alten Identitätsanbieter und erstellen neue Konten im Azure AD B2C-Verzeichnis. Wenn Sie nicht über das Kennwort verfügen, erstellen Sie das Konto mit einem zufällig generierten Kennwort. Anschließend fordern Sie den Benutzer auf, das Kennwort zu ändern. Wenn sich der Benutzer zum ersten Mal anmeldet, wird der Benutzer von Azure AD B2C zum Zurücksetzen aufgefordert.

## <a name="password-policy"></a>Kennwortrichtlinie
Die Kennwortrichtlinie von Azure AD B2C (für lokale Konten) basiert auf der Azure AD-Kennwortrichtlinie. Die Azure AD B2C-Richtlinien zur Registrierung, Anmeldung und Kennwortzurücksetzung verwenden sichere Kennwörter, und die Kennwörter laufen nicht ab. Weitere Informationen finden Sie unter [Kennwortrichtlinie in Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx).

Wenn die Konten, die Sie migrieren möchten, über eine geringere Kennwortsicherheit als die [hohe Kennwortsicherheit unter Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx) verfügen, können Sie die Erzwingung sicherer Kennwörter deaktivieren. Legen Sie zum Ändern der standardmäßigen Kennwortrichtlinie die Eigenschaft `passwordPolicies` auf `DisableStrongPassword` fest. Beispielsweise können Sie die Anforderung zum Erstellen eines Benutzers wie folgt ändern: 

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

## <a name="step-1-use-graph-api-to-migrate-users"></a>Schritt 1: Migrieren von Benutzern mithilfe der Graph-API
Sie erstellen das Azure AD B2C-Benutzerkonto mithilfe der Graph-API (mit dem bekannten oder einem zufällig generierten Kennwort). In diesem Abschnitt wird die Vorgehensweise zum Erstellen von Benutzerkonten im Azure AD B2C-Verzeichnis mit der Graph-API beschrieben.

### <a name="step-11-register-your-application-in-your-tenant"></a>Schritt 1.1: Registrieren Ihrer Anwendung in Ihrem Mandanten
Um mit der Graph-API zu kommunizieren, benötigen Sie zuerst ein Dienstkonto mit Administratorberechtigungen. In Azure AD registrieren Sie eine Anwendung und eine Authentifizierung für Azure AD. Die Anwendungsanmeldeinformationen bestehen aus **Anwendungs-ID** und **Anwendungsgeheimnis**. Die Anwendung verhält sich beim Aufrufen der Graph-API wie sie selbst und nicht wie ein Benutzer.

Registrieren Sie zuerst Ihre Migrationsanwendung bei Azure AD. Erstellen Sie anschließend einen Anwendungsschlüssel (Anwendungsgeheimnis), und statten Sie die Anwendung mit Schreibberechtigungen aus.

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com/)an.

2. Wählen Sie Ihren Azure AD **B2C**-Mandanten aus, indem Sie oben rechts im Fenster Ihr Konto auswählen.

3. Klicken Sie im linken Bereich auf **Azure Active Directory** (nicht auf Azure AD B2C). Unter Umständen müssen Sie **Weitere Dienste** auswählen, um die Option zu finden.

4. Wählen Sie **App-Registrierungen** aus.

5. Wählen Sie **Registrierung einer neuen Anwendung** aus.

    ![Registrierung einer neuen Anwendung](media/active-directory-b2c-user-migration/pre-migration-app-registration.png)

6. Erstellen Sie eine neue Anwendung, indem Sie wie folgt vorgehen:
    * Verwenden Sie für **Name** entweder **B2CUserMigration** oder einen anderen beliebigen Namen.
    * Verwenden Sie für **Anwendungstyp** die Option **Web-App/API**.
    * Verwenden Sie als **Anmelde-URL** die URL **https://localhost** (für diese Anwendung nicht relevant).
    * Klicken Sie auf **Erstellen**.

7. Wählen Sie nach der Erstellung der Anwendung in der Liste **Anwendungen** die neu erstellte Anwendung **B2CUserMigration**.

8. Klicken Sie auf **Eigenschaften**, kopieren Sie den Wert aus **Anwendungs-ID**, und speichern Sie ihn zur späteren Verwendung.

### <a name="step-12-create-the-application-secret"></a>Schritt 1.2: Erstellen des Anwendungsgeheimnisses
1. Wählen Sie im Azure-Portal im Fenster **Registrierte App** die Option **Schlüssel**.

2. Fügen Sie einen neuen Schlüssel hinzu (auch als geheimer Clientschlüssel bezeichnet), und kopieren Sie den Schlüssel dann zur späteren Verwendung.

    ![Anwendungs-ID und Schlüssel](media/active-directory-b2c-user-migration/pre-migration-app-id-and-key.png)

### <a name="step-13-grant-administrative-permission-to-your-application"></a>Schritt 1.3: Gewähren von Administratorberechtigungen für die Anwendung
1. Wählen Sie im Azure-Portal im Fenster **Registrierte App** die Option **Erforderliche Berechtigungen**.

2. Wählen Sie die Option **Windows Azure Active Directory**.

3. Wählen Sie im Bereich **Zugriff aktivieren** unter **Anwendungsberechtigungen** die Option **Lese- und Schreibzugriff auf Verzeichnisdaten** und anschließend **Speichern**.

4. Wählen Sie im Bereich **Erforderliche Berechtigungen** die Option **Berechtigungen erteilen**.

    ![Anwendungsberechtigungen](media/active-directory-b2c-user-migration/pre-migration-app-registration-permissions.png)

Sie verfügen jetzt über eine Anwendung mit Berechtigungen zum Erstellen, Lesen und Aktualisieren von Benutzern über Ihren Azure AD B2C-Mandanten.

### <a name="step-14-optional-environment-cleanup"></a>Schritt 1.4: Bereinigen der Umgebung (optional)
Die Berechtigung „Lese- und Schreibzugriff auf Verzeichnisdaten“ schließt *nicht* die Berechtigung zum Löschen von Benutzern ein. Damit Ihre Anwendung die Möglichkeit zum Löschen von Benutzern erhält (zum Bereinigen Ihrer Umgebung), müssen Sie einen zusätzlichen Schritt ausführen. Dies umfasst auch die Ausführung von PowerShell zum Festlegen von Benutzerkonto-Administratorberechtigungen. Andernfalls können Sie mit dem nächsten Abschnitt fortfahren.

> [!IMPORTANT]
> Sie müssen ein B2C-Mandantenadministratorkonto verwenden, das für den B2C-Mandanten *lokal* angeordnet ist. Die Syntax für den Kontonamen lautet *admin@contosob2c.onmicrosoft.com*.

>[!NOTE]
> Für das folgende PowerShell-Skript ist [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) erforderlich.

Gehen Sie in diesem PowerShell-Skript wie folgt vor:
1. Stellen Sie eine Verbindung mit dem Onlinedienst her. Führen Sie hierzu an der Windows PowerShell-Eingabeaufforderung das `Connect-AzureAD`-Cmdlet aus, und geben Sie Ihre Anmeldeinformationen an. 

2. Weisen Sie mithilfe der **Anwendungs-ID** der Anwendung die Rolle „Benutzerkontoadministrator“ zu. Da diese Rollen allgemein bekannte Bezeichner haben, müssen Sie lediglich Ihre **Anwendungs-ID** in das Skript eingeben.

```PowerShell
Connect-AzureAD

$AppId = "<Your application ID>"

# Fetch Azure AD application to assign to role
$roleMember = Get-AzureADServicePrincipal -Filter "AppId eq '$AppId'"

# Fetch User Account Administrator role instance
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}

# If role instance does not exist, instantiate it based on the role template
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq 'User Account Administrator'}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId

    # Fetch User Account Administrator role instance again
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}
}

# Add application to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
```

Ersetzen Sie den Wert `$AppId` durch Ihre **Anwendungs-ID** in Azure AD.

## <a name="step-2-pre-migration-application-sample"></a>Schritt 2: Anwendungsbeispiel für die Prämigration
[Laden Sie den Beispielcode herunter, und führen Sie ihn aus](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). Sie können den Download in Form einer ZIP-Datei durchführen.

### <a name="step-21-edit-the-migration-data-file"></a>Schritt 2.1: Bearbeiten der Migrationsdatendatei
Die Beispiel-App verwendet eine JSON-Datei mit Dummybenutzerdaten. Nach der erfolgreichen Ausführung des Beispiels können Sie den Code ändern, um die Daten aus Ihrer eigenen Datenbank zu nutzen. Oder Sie können das Benutzerprofil in eine JSON-Datei exportieren und die App dann so festlegen, dass diese Datei verwendet wird.

Öffnen Sie zum Bearbeiten der JSON-Datei die Visual Studio-Projektmappe `AADB2C.UserMigration.sln`. Öffnen Sie im Projekt `AADB2C.UserMigration` die Datei `UsersData.json`.

![Benutzerdatendatei](media/active-directory-b2c-user-migration/pre-migration-data-file.png)

Wie Sie sehen, enthält die Datei eine Liste mit den Benutzerentitäten. Jede Benutzerentität weist die folgenden Eigenschaften auf:
* E-Mail
* displayName
* firstName
* lastName
* password (kann leer sein)

> [!NOTE]
> Zur Kompilierzeit kopiert Visual Studio die Datei in das Verzeichnis `bin`.

### <a name="step-22-configure-the-application-settings"></a>Schritt 2.2: Konfigurieren der Anwendungseinstellungen
Öffnen Sie unter dem Projekt `AADB2C.UserMigration` die Datei *App.config*. Ersetzen Sie die folgenden App-Einstellungen durch Ihre eigenen Werte:

```XML
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
    <add key="MigrationFile" value="{Name of a JSON file containing the users' data; for example, UsersData.json}" />
    <add key="BlobStorageConnectionString" value="{Your connection Azure table string}" />
    
</appSettings>
```

> [!NOTE]
> * Die Verwendung der Azure-Tabellenverbindungszeichenfolge wird in den nächsten Abschnitten beschrieben.
> * Der Name des B2C-Mandanten ist die Domäne, die Sie bei der Erstellung des Mandanten eingegeben haben, und wird im Azure-Portal angezeigt. Der Mandantenname endet normalerweise auf das Suffix *.onmicrosoft.com* (z.B. *contosob2c.onmicrosoft.com*).
>

### <a name="step-23-run-the-pre-migration-process"></a>Schritt 2.3: Ausführen des Prämigrationsprozesses
Klicken Sie mit der rechten Maustaste auf die Projektmappe `AADB2C.UserMigration`, und erstellen Sie das Beispiel dann neu. War der Vorgang erfolgreich, sollte sich nun die ausführbare Datei `UserMigration.exe` unter `AADB2C.UserMigration\bin\Debug` befinden. Verwenden Sie zum Ausführen des Migrationsprozesses einen der folgenden Befehlszeilenparameter:

* Um **Benutzer mit Kennwort zu migrieren**, verwenden Sie den Befehl `UserMigration.exe 1`.

* Um **Benutzer mit zufälligem Kennwort zu migrieren**, verwenden Sie den Befehl `UserMigration.exe 2`. Bei diesem Vorgang wird zudem eine Azure-Tabellenentität erstellt. Später konfigurieren Sie die Richtlinie zum Aufrufen des REST-API-Diensts. Der Dienst nutzt eine Azure-Tabelle zum Nachverfolgen und Verwalten des Migrationsprozesses.

![Demo für den Migrationsprozess](media/active-directory-b2c-user-migration/pre-migration-demo.png)

### <a name="step-24-check-the-pre-migration-process"></a>Schritt 2.4: Überprüfen des Prämigrationsprozesses
Verwenden Sie zum Überprüfen der Migration eine der beiden folgenden Methoden:

* Verwenden Sie das Azure-Portal, um anhand des Anzeigenamens nach einem Benutzer zu suchen:
    
    a. Öffnen Sie **Azure AD B2C**, und wählen Sie die Option **Benutzer und Gruppen**.

    b. Geben Sie in das Suchfeld den Anzeigenamen des Benutzers ein, und zeigen Sie dann das Profil des Benutzers an. 

* Verwenden Sie diese Beispielanwendung, um einen Benutzer anhand der E-Mail-Adresse für die Anmeldung abzurufen:

    a. Führen Sie den folgenden Befehl aus:

    ```Console
        UserMigration.exe 3 {email address}
    ```
        
    > [!TIP]
    > Sie können auch den folgenden Befehl verwenden, um die Ausgabe mit dem folgenden Befehl in einer JSON-Datei zu speichern:
    >
    >```Console
    >    UserMigration.exe 3 {email address} >> UserProfile.json
    >```

    > [!TIP]
    > Sie können einen Benutzer anhand des Anzeigenamens abrufen, indem Sie den folgenden Befehl verwenden: `UserMigration.exe 4 <Display name>`.

    b. Öffnen Sie die Datei *UserProfile.json* in einem JSON-Editor. Mit Visual Studio Code können Sie ein JSON-Dokument formatieren, indem Sie UMSCHALT+ALT+F drücken oder im Kontextmenü die Option **Dokument formatieren** wählen.

    ![Datei „UserProfile.json“](media/active-directory-b2c-user-migration/pre-migration-get-by-email2.png)

### <a name="step-25-optional-environment-cleanup"></a>Schritt 2.5: Bereinigen der Umgebung (optional)
Wenn Sie den Azure AD-Mandanten bereinigen und die Benutzer aus dem Azure AD-Verzeichnis entfernen möchten, führen Sie den Befehl `UserMigration.exe 5` aus.

> [!NOTE]
> * Konfigurieren Sie zum Bereinigen des Mandanten Benutzerkontoadministrator-Berechtigungen für Ihre Anwendung.
> * Die Beispielmigrations-App bereinigt alle in der JSON-Datei aufgelisteten Benutzer.

### <a name="step-26-sign-in-with-migrated-users-with-password"></a>Schritt 2.6: Anmelden mit migrierten Benutzern (mit Kennwort)
Nach dem Ausführen des Prämigrationsprozesses mit Benutzerkennwörtern können die Konten verwendet werden, und Benutzer können sich mit Azure AD B2C an Ihrer Anwendung anmelden. Wenn Sie keinen Zugriff auf die Benutzerkennwörter haben, können Sie mit dem nächsten Abschnitt fortfahren.

## <a name="step-3-help-users-reset-their-password"></a>Schritt 3: Unterstützen von Benutzern beim Zurücksetzen ihrer Kennwörter
Wenn Sie Benutzer mit einem zufällig generierten Kennwort migrieren, müssen diese ihr Kennwort zurücksetzen. Senden Sie ihnen als Hilfe zum Zurücksetzen des Kennworts eine Begrüßungs-E-Mail mit einem Link, über den das Kennwort zurückgesetzt werden kann.

Gehen Sie wie folgt vor, um den Link zu Ihrer Richtlinie für die Kennwortzurücksetzung zu erhalten:

1. Wählen Sie **Azure AD-B2C-Einstellungen** und dann die Richtlinieneigenschaften für **Kennwort zurücksetzen**.

2. Wählen Sie Ihre Anwendung aus.

    >[!NOTE]
    >Für „Jetzt ausführen“ ist es erforderlich, dass mindestens eine Anwendung vorab im Mandanten registriert wird. Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C. 

3. Wählen Sie **Jetzt ausführen**, und überprüfen Sie die Richtlinie.

4. Kopieren Sie im Feld **Endpunkt für sofortige Ausführung** die URL, und senden Sie sie anschließend an Ihre Benutzer.

    ![Festlegen von Diagnoseprotokollen](media/active-directory-b2c-user-migration/pre-migration-policy-uri.png)

## <a name="step-4-optional-change-your-policy-to-check-and-set-the-user-migration-status"></a>Schritt 4: Ändern der Richtlinie zum Überprüfen und Festlegen des Benutzermigrationsstatus (optional)

> [!NOTE]
> Sie müssen eine benutzerdefinierte Richtlinie verwenden, um den Benutzermigrationsstatus zu überprüfen und zu ändern. Weitere Informationen finden Sie unter [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md).
>

Wenn sich Benutzer anmelden, ohne zuerst das Kennwort zu ändern, sollte von der Richtlinie eine Fehlermeldung angezeigt werden. Beispiel: 
>*Ihr Kennwort ist abgelaufen. Wählen Sie den Link „Kennwort zurücksetzen“, um es zurückzusetzen.* 

Für diesen optionalen Schritt ist die Verwendung von benutzerdefinierten Azure AD B2C-Richtlinien erforderlich. Informationen hierzu finden Sie im Artikel [Azure Active Directory B2C: Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md).

In diesem Abschnitt ändern Sie die Richtlinie, um den Benutzermigrationsstatus bei der Anmeldung zu überprüfen. Wenn der Benutzer das Kennwort nicht geändert hat, sollte eine HTTP 409-Fehlermeldung mit der Aufforderung zurückgegeben werden, dass der Benutzer den Link **Kennwort vergessen?** wählen soll. 

Verwenden Sie eine Azure-Tabelle, um die Kennwortänderung nachzuverfolgen. Wenn Sie den Prozess für die Prämigration mit dem Befehlszeilenparameter `2` ausführen, erstellen Sie eine Benutzerentität in einer Azure-Tabelle. Ihr Dienst führt Folgendes durch:

* Bei der Anmeldung ruft die Azure AD B2C-Richtlinie den RESTful-Migrationsdienst auf und sendet eine E-Mail als Eingabeanspruch. Der Dienst sucht in der Azure-Tabelle nach der E-Mail-Adresse. Wenn die Adresse vorhanden ist, löst der Dienst eine Fehlermeldung aus: *You must change password* (Ihr Kennwort muss geändert werden).

* Nachdem der Benutzer das Kennwort geändert hat, wird die Entität aus der Azure-Tabelle entfernt.

>[!NOTE]
>Zur Vereinfachung des Beispiels wird eine Azure-Tabelle verwendet. Sie können den Migrationsstatus in einer beliebigen Datenbank oder als benutzerdefinierte Eigenschaft im Azure AD B2C-Konto speichern.

### <a name="41-update-your-application-setting"></a>4.1: Aktualisieren Ihrer Anwendungseinstellung
1. Öffnen Sie zum Testen des RESTful-API-Demos in Visual Studio die VS-Lösung `AADB2C.UserMigration.sln`. 

2. Öffnen Sie im Projekt `AADB2C.UserMigration.API` die Datei *App.config*. Ersetzen Sie die App-Einstellung durch Ihren eigenen Wert:

    ```XML
    <appSettings>
        <add key="BlobStorageConnectionString" value="{The Azure Blob storage connection string"} />
    </appSettings>
    ```

### <a name="step-42-deploy-your-web-application-to-azure-app-service"></a>Schritt 4.2: Bereitstellen der Webanwendung in Azure App Service
Veröffentlichen Sie Ihren API-Dienst in Azure App Service. Weitere Informationen finden Sie unter [Lokale Git-Bereitstellung in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service-web/web-sites-deploy).

### <a name="step-43-add-a-technical-profile-and-technical-profile-validation-to-your-policy"></a>Schritt 4.3: Hinzufügen eines technischen Profils und der Überprüfung technischer Profile zur Richtlinie 
1. Öffnen Sie die Erweiterungsrichtliniendatei *TrustFrameworkExtensions.xml* in Ihrem Arbeitsverzeichnis. 

2. Suchen Sie nach dem Knoten `<ClaimsProviders>`, und fügen Sie im Knoten dann den folgenden XML-Codeausschnitt hinzu. Achten Sie darauf, den Wert von `ServiceUrl` so zu ändern, dass er auf Ihre Endpunkt-URL verweist. 

    ```XML
    <ClaimsProvider>
        <DisplayName>REST APIs</DisplayName>
        <TechnicalProfiles>
    
        <TechnicalProfile Id="LocalAccountSignIn">
            <DisplayName>Local account just in time migration</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/LoalAccountSignIn</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
            <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="email" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
    
        <TechnicalProfile Id="LocalAccountPasswordReset">
            <DisplayName>Local account just in time migration</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/PasswordUpdated</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
            <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        </TechnicalProfiles>
    </ClaimsProvider>
    
    <ClaimsProvider>
        <DisplayName>Local Account</DisplayName>
        <TechnicalProfiles>
    
        <!-- This technical profile uses a validation technical profile to authenticate the user. -->
        <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
            <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="LocalAccountSignIn" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    
        <TechnicalProfile Id="LocalAccountWritePasswordUsingObjectId">
            <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="LocalAccountPasswordReset" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    
        </TechnicalProfiles>
    </ClaimsProvider>
    ```

Mit dem obigen technischen Profil wird ein Eingabeanspruch definiert: `signInName` (als E-Mail senden). Bei der Anmeldung wird der Anspruch an Ihren RESTful-Endpunkt gesendet.

Definieren Sie das technische Profil für die RESTful-API, und konfigurieren Sie anschließend die Azure AD B2C-Richtlinie für das Aufrufen des technischen Profils. Der XML-Codeausschnitt setzt das in der Basisrichtlinie definierte Element `SelfAsserted-LocalAccountSignin-Email` außer Kraft. Darüber hinaus fügt der XML-Codeausschnitt `ValidationTechnicalProfile` mit einer Referenz-ID hinzu, die auf das technische Profil `LocalAccountUserMigration` verweist. 

### <a name="step-44-upload-the-policy-to-your-tenant"></a>Schritt 4.4: Hochladen der Richtlinie in Ihren Mandanten
1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und wählen Sie anschließend **Azure AD B2C**.

2. Wählen Sie **Framework für die Identitätsfunktion** aus.

3. Wählen Sie die Option **Alle Richtlinien**.

4. Wählen Sie **Richtlinie hochladen** aus.

5. Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.

6. Laden Sie die Datei *TrustFrameworkExtensions.xml* hoch, und stellen Sie sicher, dass sie die Überprüfung besteht.

### <a name="step-45-test-the-custom-policy-by-using-run-now"></a>Schritt 4.5: Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“
1. Wählen Sie die Option **Azure AD-B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.

2. Öffnen Sie **B2C_1A_signup_signin**. Dies ist die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie anschließend **Jetzt ausführen**.

3. Versuchen Sie, sich mit den Anmeldeinformationen eines migrierten Benutzers anzumelden, und wählen Sie anschließend **Anmelden**. Ihre REST-API sollte die folgende Fehlermeldung auslösen:

    ![Festlegen von Diagnoseprotokollen](media/active-directory-b2c-user-migration/pre-migration-error-message.png)

### <a name="step-46-optional-troubleshoot-your-rest-api"></a>Schritt 4.6: Behandeln von Problemen mit der REST-API (optional)
Sie können Protokollierungsinformationen nahezu in Echtzeit anzeigen und überwachen.

1. Wählen Sie im Menü mit den Einstellungen für Ihre RESTful-Anwendung unter **Überwachung** die Option **Diagnoseprotokolle**. 

2. Legen Sie **Anwendungsprotokollierung (Dateisystem)** auf **Ein** fest. 

3. Legen Sie die **Ebene** auf **Ausführlich** fest.

4. Wählen Sie **Speichern** aus.

    ![Festlegen von Diagnoseprotokollen](media/active-directory-b2c-user-migration/pre-migration-diagnostic-logs.png)

5. Wählen Sie im Menü **Einstellungen** die Option **Protokollstream**.

6. Überprüfen Sie die Ausgabe der RESTful-API.

Weitere Informationen finden Sie unter [Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service-web/web-sites-streaming-logs-and-console).

> [!IMPORTANT]
> Verwenden Sie die Diagnoseprotokolle nur während der Entwicklung und bei Tests. Die RESTful-API-Ausgabe enthält unter Umständen vertrauliche Informationen, die während der Produktion nicht verfügbar gemacht werden dürfen.
>

## <a name="optional-download-the-complete-policy-files"></a>(Optional) Herunterladen der vollständigen Richtliniendateien
Nachdem Sie die exemplarische Vorgehensweise unter [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) abgeschlossen haben, raten wir Ihnen, mit Ihren eigenen benutzerdefinierten Richtliniendateien ein Szenario zu erstellen. Zu Referenzzwecken haben wir [Beispiele für Richtliniendateien](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration) bereitgestellt. 
