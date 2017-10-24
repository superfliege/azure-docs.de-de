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
ms.openlocfilehash: b102edd997e69fb3568780a42dfe93fc9a90e332
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-user-migration"></a>Azure Active Directory B2C: Benutzermigration
Wenn Sie Ihren Identitätsanbieter zu Azure AD B2C migrieren möchten, müssen Sie unter Umständen auch die Benutzerkonten migrieren. In diesem Artikel wird erläutert, wie vorhandene Benutzerkonten von einem beliebigen Identitätsanbieter zu Azure AD B2C migriert werden. Dieser Artikel enthält keine verbindliche Anleitung, sondern beschreibt lediglich zwei der zahlreichen Möglichkeiten, die Ihnen zur Verfügung stehen.  Die Wahl der geeigneten Methode liegt in der Verantwortung des Entwicklers.

## <a name="user-migration-flows"></a>Benutzermigrationsmethoden
Azure AD B2C ermöglicht über die [Graph-API](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) die Migration von Benutzern. Für die Benutzermigrationsprozess stehen zwei Methoden zur Verfügung:

* **Prämigration**: Diese Methode eignet sich, wenn Sie Zugriff auf Benutzeranmeldeinformationen (Benutzername und Kennwort) in Klartext besitzen. Oder die Anmeldeinformationen sind verschlüsselt, aber Sie können sie entschlüsseln. Der Prozess beinhaltet folgende Schritte: Lesen der Benutzer im alten Identitätsanbieter und Erstellen neuer Konten im Azure AD B2C-Verzeichnis.

* **Prämigration und Kennwortzurücksetzung**: Diese Methode ist geeignet, wenn auf das Kennwort des Benutzers nicht zugegriffen werden kann. Beispiel:
    * Kennwörter werden im HASH-Format gespeichert.
    * Kennwörter sind in einem Identitätsanbieter gespeichert, auf den Sie keinen Zugriff haben. Der alte Identitätsanbieter überprüft Benutzeranmeldeinformationen durch Aufrufen eines Webdiensts.

Bei beiden Methoden führen Sie zunächst den Prozess für die __Prämigration__ aus: Sie lesen die Benutzer im alten Identitätsanbieter und erstellen neue Konten im Azure AD B2C-Verzeichnis. Falls Sie das Kennwort nicht kennen, erstellen Sie das Konto mit einem von Ihnen generierten zufälligen Kennwort. Sie fordern die Benutzer zur Änderung ihres Kennworts auf. Oder Benutzer werden bei der ersten Anmeldung von B2C zum Zurücksetzen des Kennworts aufgefordert.

## <a name="password-policy"></a>Kennwortrichtlinie
Die Kennwortrichtlinie von Azure AD B2C (für lokale Konten) basiert auf der Azure AD-Kennwortrichtlinie. Die Azure AD B2C-Richtlinien zur Registrierung, Anmeldung und Kennwortzurücksetzung verwenden sichere Kennwörter, und die Kennwörter laufen nicht ab. Weitere Informationen finden Sie in der [Kennwortrichtlinie von Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx) .

Wenn die Konten, die Sie migrieren möchten, über eine geringere Kennwortsicherheit als die [hohe Kennwortsicherheit unter Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx) verfügen, können Sie die Erzwingung sicherer Kennwörter deaktivieren. Legen Sie zum Ändern der standardmäßigen Kennwortrichtlinie die Eigenschaft `passwordPolicies` auf `DisableStrongPassword` fest. Beispielsweise können Sie die Anforderung zum Erstellen eines Benutzers wie folgt ändern: 

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

## <a name="step-1-using-graph-api-to-migrate-users"></a>Schritt 1: Migrieren von Benutzern mithilfe der Graph-API
Sie erstellen das Azure AD B2C-Benutzerkonto mithilfe der Graph-API (mit dem bekannten oder dem zufälligen Kennwort). In diesem Abschnitt wird die Vorgehensweise zum Erstellen von Benutzerkonten im Azure AD B2C-Verzeichnis mit der Graph-API beschrieben.

### <a name="step-11-register-your-application-in-your-tenant"></a>Schritt 1.1: Registrieren Ihrer Anwendung in Ihrem Mandanten
Um mit der Graph-API zu kommunizieren, benötigen Sie zuerst ein Dienstkonto mit Administratorberechtigungen. In Azure AD registrieren Sie eine Anwendung und authentifizieren sie bei Azure AD. Die Anwendungsanmeldeinformationen bestehen aus **Anwendungs-ID** und **Anwendungsgeheimnis**. Die Anwendung verhält sich beim Aufrufen der Graph-API wie sie selbst und nicht wie ein Benutzer.

Registrieren Sie zuerst Ihre Migrationsanwendung bei Azure AD. Erstellen Sie anschließend einen Anwendungsschlüssel (Anwendungsgeheimnis), und statten Sie die Anwendung mit den richtigen Berechtigungen aus.

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie Ihren Azure AD **B2C**-Mandanten aus, indem Sie in der rechten oberen Ecke der Seite Ihr Konto auswählen.
3. Klicken Sie im linken Bereich auf **Azure Active Directory** (nicht Azure AD B2C). Möglicherweise müssen Sie **Weitere Dienste** auswählen, um das Blatt zu finden.
4. Wählen Sie **App-Registrierungen** aus.
5. Klicken Sie auf **Registrierung einer neuen Anwendung**.

    ![Registrierung einer neuen Anwendung](media/active-directory-b2c-user-migration/pre-migration-app-registration.png)
6. Folgen Sie der Anleitung, und erstellen Sie eine neue Anwendung.
    * Verwenden Sie für **Name** entweder **B2CUserMigratioin** oder einen anderen beliebigen Namen.
    * Verwenden Sie für **Anwendungstyp** die Option **Web-App/API**.
    * Verwenden Sie als **Anmelde-URL** die URL „https://localhost“ (für diese Anwendung nicht relevant).
    * Klicken Sie auf **Erstellen**.
7. Wählen Sie nach der Erstellung der Anwendung in der Liste der Anwendungen die neu erstellte Anwendung **B2CUserMigratioin** aus.
Klicken Sie auf **Eigenschaften**, kopieren Sie den Wert aus **Anwendungs-ID**, und speichern Sie ihn zur späteren Verwendung.

### <a name="step-12-create-application-secret"></a>Schritt 1.2: Erstellen eines Anwendungsgeheimnisses
8. Weiter geht es im Azure-Portal unter „Registrierte App“. Klicken Sie auf **Schlüssel**, und fügen Sie einen neuen Schlüssel hinzu (auch als geheimer Clientschlüssel bezeichnet). Kopieren Sie den Schlüssel außerdem zur späteren Verwendung.

    ![Anwendungs-ID und Schlüssel](media/active-directory-b2c-user-migration/pre-migration-app-id-and-key.png)

### <a name="step-13-grant-administrative-permission-to-your-application"></a>Schritt 1.3: Gewähren von Administratorberechtigungen für die Anwendung
9. Weiter geht es im Azure-Portal unter **Registrierte App**.
10. Klicken Sie auf **Erforderliche Berechtigungen**.
11. Klicken Sie auf **Windows Azure Active Directory**.
12. Wählen Sie unter **Zugriff aktivieren** im Abschnitt **Anwendungsberechtigungen** die Berechtigung **Lese- und Schreibzugriff auf Verzeichnisdaten** aus, und klicken Sie auf **Speichern**.
13. Klicken Sie schließlich wieder unter **Erforderliche Berechtigungen** auf die Schaltfläche **Berechtigungen erteilen**.

    ![Anwendungsberechtigungen](media/active-directory-b2c-user-migration/pre-migration-app-registration-permissions.png)

Sie verfügen jetzt über eine Anwendung mit der Berechtigung zum Erstellen, Lesen und Aktualisieren von Benutzern in Ihrem B2C-Mandanten.

### <a name="step-14-optional-environment-cleanup"></a>Schritt 1.4: Bereinigen der Umgebung [optional]
Die Berechtigung „Lese- und Schreibzugriff auf Verzeichnisdaten“ schließt NICHT die Möglichkeit ein, Benutzer zu löschen. Wenn Ihre Anwendung die Berechtigung zum Löschen von Benutzern (zur Umgebungsbereinigung) erhalten soll, müssen Sie einen zusätzlichen Schritt ausführen. Für diesen Schritt wird PowerShell ausgeführt, um Berechtigungen vom Typ __Benutzerkontoadministrator__ festzulegen. Andernfalls können Sie mit dem nächsten Abschnitt fortfahren.


> [!IMPORTANT]
> Sie müssen ein B2C-Mandantenadministratorkonto verwenden, das für den B2C-Mandanten **lokal** ist. Diese Konten sehen wie folgt aus: admin@contosob2c.onmicrosoft.com.

>[!NOTE]
> Für das folgende PowerShell-Skript ist [Azure Active Directory PowerShell **Version 2**](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) erforderlich.

Im PowerShell-Skript weiter unten führen Sie folgende Schritte aus:
* Sie stellen eine Verbindung mit dem Onlinedienst her. Führen Sie hierzu an der Windows PowerShell-Eingabeaufforderung das Cmdlet `Connect-AzureAD` aus, und geben Sie Ihre Anmeldeinformationen an. 
* Weisen Sie mithilfe der **Anwendungs-ID** der Anwendung die Rolle „Benutzerkontoadministrator“ zu. Diese Rollen haben allgemein bekannte Bezeichner, daher müssen Sie lediglich Ihre **Anwendungs-ID** in das Skript einfügen.

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
Laden Sie den Beispielcode herunter, und bereiten Sie ihn für die Ausführung vor. Sie können den [Beispielcode als ZIP-Datei herunterladen](http://www.github.com).

### <a name="step-21-edit-the-migration-data-file"></a>Schritt 2.1: Bearbeiten der Migrationsdatendatei
Die Beispiel-App verwendet eine JSON-Datei mit Dummybenutzerdaten. Ändern Sie nach der erfolgreichen Ausführung des Beispiels den Code, um die Daten aus Ihrer eigenen Datenbank zu nutzen. Oder exportieren Sie das Benutzerprofil in eine JSON-Datei, und legen Sie die App so fest, dass sie diese Datei verwendet.
Öffnen Sie zum Bearbeiten der JSON-Datei die Visual Studio-Projektmappe `AADB2C.UserMigration.sln`. Öffnen Sie im Projekt `AADB2C.UserMigration` die Datei `UsersData.json`.


![Benutzerdatendatei](media/active-directory-b2c-user-migration/pre-migration-data-file.png)

Wie Sie sehen, enthält die Datei eine Liste der Benutzerentitäten. Jede Benutzerentität enthält folgende Einträge:
* E-Mail
* displayName
* firstName
* lastName
* password (kann leer sein)

> [!NOTE]
> Zur Kompilierzeit kopiert Visual Studio die Datei ins `bin`-Verzeichnis.

### <a name="step-22-configure-the-application-settings"></a>Schritt 2.2: Konfigurieren der Anwendungseinstellungen
Öffnen Sie im Projekt `AADB2C.UserMigration` die Datei „App.config“. Ersetzen Sie die folgenden App-Einstellungen durch Ihre eigenen Werte:

```XML
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
    <add key="MigrationFile" value="{Name of a JSON file containing the users data e.g. UsersData.json}" />
    <add key="BlobStorageConnectionString" value="{Your connection Azure Table string}" />
    
</appSettings>
```

> [!NOTE]
> * Die Verwendung der Table Storage-Verbindungszeichenfolge wird in den nächsten Abschnitten beschrieben.
> * Der Name des B2C-Mandanten ist die Domäne, die Sie bei der Erstellung des Mandanten eingegeben haben, und wird im Azure-Portal angezeigt. In der Regel endet er mit dem Suffix `.onmicrosoft.com`, z. B. `contosob2c.onmicrosoft.com`.
>

### <a name="step-23-run-the-pre-migration-process"></a>Schritt 2.3: Ausführen des Prämigrationsprozesses
Klicken Sie mit der rechten Maustaste auf die Projektmappe `AADB2C.UserMigration`, und erstellen Sie das Beispiel neu. War der Vorgang erfolgreich, sollte sich nun die ausführbare Datei `UserMigration.exe` unter `AADB2C.UserMigration\bin\Debug` befinden. Verwenden Sie zum Ausführen des Migrationsprozesses einen der folgenden Befehlszeilenparameter:

* Um **Benutzer mit Kennwort zu migrieren**, verwenden Sie den Befehl `UserMigration.exe 1`.

* Um **Benutzer mit zufälligem Kennwort zu migrieren**, verwenden Sie den Befehl `UserMigration.exe 2`. Bei diesem Vorgang wird zudem eine Table Storage-Entität erstellt. Später konfigurieren Sie die Richtlinie zum Aufrufen des REST-API-Diensts. Der Dienst nutzt Table Storage zum Nachverfolgen und Verwalten des Migrationsprozesses.

![Demo für den Migrationsprozess](media/active-directory-b2c-user-migration/pre-migration-demo.png)

### <a name="step-24-check-the-pre-migration-process"></a>Schritt 2.4: Überprüfen des Prämigrationsprozesses
Öffnen Sie zum Überprüfen des Ergebnisses im Azure-Portal **Azure AD B2C**, und klicken Sie auf **Benutzer und Gruppen**. Geben Sie in das Suchfeld den Anzeigenamen eines Benutzers ein, und sehen Sie sich das Benutzerprofil an. Alternativ können Sie mit dieser Beispielanwendung Benutzer anhand der **E-Mail-Adresse für die Anmeldung** abrufen. Führen Sie zum Suchen eines Benutzers anhand der Anmelde-E-Mail-Adresse den folgenden Befehl aus:

```Console
UserMigration.exe 3 {email address}
```

Sie können die Ausgabe auch wie folgt in eine JSON-Datei speichern:
```Console
UserMigration.exe 3 {email address} >> UserProfile.json
```


Öffnen Sie die Datei „UserProfile.json“ mit dem gewünschten JSON-Editor. Mit Visual Studio Code können Sie ein JSON-Dokument mit `Shift+Alt+F` oder über die Option „Dokument formatieren“ im Kontextmenü formatieren.

![UserProfile.json](media/active-directory-b2c-user-migration/pre-migration-get-by-email2.png)

Sie können Benutzer auch anhand des **Anzeigenamens** abrufen. Verwenden Sie dazu den Befehl `UserMigration.exe 4 <Display name>`.

### <a name="step-25-optional-environment-cleanup"></a>Schritt 2.5: Bereinigen der Umgebung [optional]
Wenn Sie den Azure AD-Mandanten bereinigen und die Benutzer aus dem Azure AD-Verzeichnis entfernen möchten, führen Sie den Befehl `UserMigration.exe 5` aus.

> [!NOTE]
> * Konfigurieren Sie zum Bereinigen des Mandanten __Benutzerkontoadministrator__-Berechtigungen für ihre Anwendung.
> * Die Beispielmigrations-App bereinigt alle in der JSON-Datei aufgelisteten Benutzer.

### <a name="step-26-sing-in-with-migrated-users-with-password"></a>Schritt 2.6: Anmelden mit migrierten Benutzern (mit Kennwort)
Nach dem Ausführen des Prämigrationsprozesses mit dem Kennwort der Benutzer können die Konten verwendet werden, und Benutzer können sich mit Azure AD B2C bei Ihrer Anwendung anmelden. Wenn Sie Zugriff auf die Kennwörter der Benutzer haben, fahren Sie mit dem nächsten Abschnitt fort.

## <a name="step-3-password-reset"></a>Schritt 3: Kennwortzurücksetzung
Falls Sie Benutzer mit zufälligen Kennwörtern migrieren, müssen Benutzer ihr Kennwort zurücksetzen. So setzen Sie das Kennwort zurück:
* Senden Sie eine Begrüßungs-E-Mail mit einem Link zum Zurücksetzen des Kennworts.
* [Optional] Ändern Sie die Richtlinie, um Szenarien zu behandeln, in denen Benutzer das Kennwort nicht zurücksetzen und versuchen, sich anzumelden. Bei der Anmeldung überprüft die Richtlinie den Migrationsstatus. Wenn ein Benutzer das Kennwort nicht geändert hat, wird eine Fehlermeldung angezeigt, in der er zum Klicken auf „Haben Sie Ihr Kennwort vergessen?“ aufgefordert wird.

    > [!NOTE]
    > Sie müssen eine benutzerdefinierte Richtlinie verwenden, um den Benutzermigrationsstatus zu überprüfen und zu ändern. Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.
    >

### <a name="step-31-send-a-welcome-email-with-link-to-reset-password"></a>Schritt 3.1: Senden einer Begrüßungs-E-Mail mit einem Link zum Zurücksetzen des Kennworts
So erhalten Sie den Link zur Richtlinie zur Kennwortzurücksetzung:

1.  Öffnen Sie die **Azure AD B2C-Einstellungen** und anschließend die Richtlinieneigenschaften für **Kennwort zurücksetzen**.

2. Wählen Sie Ihre Anwendung.
    >[!NOTE]
    >
    >**Jetzt ausführen** macht erforderlich, dass mindestens eine Anwendung im Mandanten vorab registriert wird. Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.  

2.  Klicken Sie auf **Jetzt ausführen**, und überprüfen Sie die Richtlinie.
3.  **Kopieren** Sie die URL, und senden Sie sie an Ihre Benutzer.

    ![Festlegen von Diagnoseprotokollen](media/active-directory-b2c-user-migration/pre-migration-policy-uri.png)

## <a name="step-4-optional-change-your-policy-to-check-and-set-user-migration-status"></a>Schritt 4: Ändern der Richtlinie zum Überprüfen und Festlegen des Benutzermigrationsstatus [optional]

Wenn sich Benutzer anmelden, ohne zuerst das Kennwort zu ändern, sollte von der Richtlinie eine Fehlermeldung angezeigt werden. Beispiel: Ihr Kennwort ist abgelaufen. Klicken Sie zum Zurücksetzen des Kennworts auf den Link „Kennwort zurücksetzen“.  Für diesen optionalen Schritt ist die Verwendung von Azure AD B2C und benutzerdefinierten Richtlinien erforderlich. Informationen hierzu finden Sie im Artikel [Azure Active Directory B2C: Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md).

In diesem Abschnitt ändern Sie die Richtlinie, um den Migrationsstatus bei der Anmeldung zu überprüfen. Wenn ein Benutzer das Kennwort nicht geändert hat, wird eine HTTP 409-Fehlermeldung angezeigt, in der er zum Klicken auf den Link „Haben Sie Ihr Kennwort vergessen?“ aufgefordert wird . Verwenden Sie Table Storage zum Nachverfolgen der Kennwortänderung. Wenn Sie den Prämigrationsprozess mit dem Befehlszeilenparameter `2` ausführen, wird eine Benutzerentität in Table Storage erstellt. Ihr Dienst:

* Bei der Anmeldung ruft die Azure AD B2C-Richtlinie den RESTful-Migrationsdienst auf und sendet eine E-Mail als Eingabeanspruch. Der Dienst sucht die E-Mail-Adresse in Table Storage. Ist der Name vorhanden, wird die Fehlermeldung „You must change password“ (Ihr Kennwort muss geändert werden.) angezeigt.

* Nachdem der Benutzer das Kennwort geändert hat, wird bei der Kennwortzurücksetzung die Entität aus Table Storage entfernt.


> [!NOTE]
>Zur Vereinfachung des Beispiels wird Table Storage verwendet. Sie können den Migrationsstatus in einer beliebigen Datenbank oder als benutzerdefinierte Eigenschaft im Azure AD B2C-Konto speichern.

### <a name="41-application-settings"></a>4.1: Anwendungseinstellungen
Testen Sie die Demo-RESTful-API. Öffnen Sie die Visual Studio-Projektmappe `AADB2C.UserMigration.sln` in Visual Studio. Öffnen Sie im Projekt `AADB2C.UserMigration.API` die Datei „App.config“. Ersetzen Sie die drei App-Einstellungen durch Ihre eigenen Werte:

```XML
<appSettings>
    <add key="BlobStorageConnectionString" value="{The Azure Blob storage connection string"} />
</appSettings>
```

### <a name="step-42-deploy-your-web-application-to-azure-app-services"></a>Schritt 4.2: Bereitstellen der Webanwendung in Azure App Service
Veröffentlichen Sie Ihren API-Dienst in Azure App Service Weitere Informationen finden Sie unter [Lokale Git-Bereitstellung in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service-web/web-sites-deploy).

### <a name="step-43-add-technical-profile-and-technical-profile-validation-to-your-policy"></a>Schritt 4.3: Hinzufügen eines technischen Profils und der Überprüfung technischer Profile zur Richtlinie 
1.  Öffnen Sie die Erweiterungsrichtliniendatei (TrustFrameworkExtensions.xml) von Ihrem Arbeitsverzeichnis aus. 
2. Suchen Sie nach dem `<ClaimsProviders>`-Abschnitt.
3. Fügen Sie folgenden XML-Codeausschnitt unter dem `ClaimsProviders`-Element hinzu.
4. Ändern Sie den Wert von `ServiceUrl` so, dass er auf Ihre Endpunkt-URL verweist. 

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

Das technische Profil definiert einen Eingabeanspruch: `signInName` (als E-Mail senden). Bei der Anmeldung führt der Anspruch den Sendevorgang an den RESTful-Endpunkt durch.

Definieren Sie das technische Profil für die RESTful-API, und konfigurieren Sie anschließend die Azure AD B2C-Richtlinie zum Aufrufen dieses technischen Profils. Der XML-Codeausschnitt setzt das in der Basisrichtlinie definierte Element `SelfAsserted-LocalAccountSignin-Email` außer Kraft. Darüber hinaus fügt der XML-Codeausschnitt `ValidationTechnicalProfile` mit einer Referenz-ID hinzu, die auf das technische Profil `LocalAccountUserMigration` verweist. 

### <a name="step-44-upload-the-policy-to-your-tenant"></a>Schritt 4.4: Hochladen der Richtlinie in Ihren Mandanten
1.  Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und klicken Sie auf **Azure AD B2C**.
2.  Wählen Sie **Framework für die Identitätsfunktion** aus.
3.  Klicken Sie auf **All Policies** (Alle Richtlinien).
4.  Wählen Sie **Richtlinie hochladen** aus.
5.  Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
6.  **Laden** Sie „TrustFrameworkExtensions.xml“ hoch, und stellen Sie sicher, dass kein Fehler bei der Validierung auftritt.
7.  Wiederholen Sie den letzten Schritt, und laden Sie die Datei „SignUpOrSignIn.xml“ hoch.

### <a name="step-45-test-the-custom-policy-by-using-run-now"></a>Schritt 4.5: Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“
1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.
2.  Öffnen Sie **B2C_1A_signup_signin** – die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3. Versuchen Sie, sich mit einem der migrierten Benutzer anzumelden, und klicken Sie auf **Anmelden**. Die folgende von der REST-API ausgelöste Fehlermeldung sollte angezeigt werden:

    ![Festlegen von Diagnoseprotokollen](media/active-directory-b2c-user-migration/pre-migration-error-message.png)

### <a name="step-46-optional-troubleshooting-your-rest-api"></a>Schritt 4.6: Problembehandlung der REST-API [optional]
Sie können Protokollierungsinformationen nahezu in Echtzeit überwachen und anzeigen.

1. Scrollen Sie im Einstellungsmenü der RESTful-Anwendung nach unten zum Abschnitt **Überwachung**, und klicken Sie auf **Diagnoseprotokolle**. 
2. Aktivieren Sie die Anwendungsprotokollierung (Dateisystem). 
3. Legen Sie für **Ebene** die Option **Ausführlich** fest.
4. Klicken Sie unten auf der Seite auf **Speichern**

    ![Festlegen von Diagnoseprotokollen](media/active-directory-b2c-user-migration/pre-migration-diagnostic-logs.png)

5. Klicken Sie im Menü „Einstellungen“ auf **Protokollstream**.
6. Überprüfen Sie die Ausgabe der RESTful-API.

Weitere Informationen finden Sie unter [Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service-web/web-sites-streaming-logs-and-console).

> [!IMPORTANT]
> Die __Diagnoseprotokolle__ sollten nur während des Entwickelns und Testens verwendet werden. Die RESTful-API-Ausgabe enthält unter Umständen vertrauliche Informationen, die während der Produktion nicht verfügbar gemacht werden dürfen.
>

## <a name="download-the-complete-policy-files"></a>Herunterladen der vollständigen Richtliniendateien
Optional: Sie sollten nach Abschluss der exemplarischen Vorgehensweise „Erste Schritte mit benutzerdefinierten Richtlinien“ Ihr Szenario mithilfe Ihrer eigenen Dateien mit benutzerdefinierten Richtlinien erstellen, anstatt diese Beispieldateien zu verwenden.  [Beispielrichtliniendateien zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration)