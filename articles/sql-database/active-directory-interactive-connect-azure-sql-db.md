---
title: ActiveDirectoryInteractive stellt eine Verbindung mit SQL her Microsoft-Dokumentation
description: C#-Codebeispiel mit Erklärungen für das Herstellen der Verbindung mit Azure SQL-Datenbank mit dem SqlAuthenticationMethod.ActiveDirectoryInteractive-Modus.
services: sql-database
author: GithubMirek
manager: craigg
ms.service: sql-database
ms.custom: active directory
ms.topic: article
ms.date: 04/06/2018
ms.author: MirekS
ms.reviewer: GeneMi
ms.openlocfilehash: 6489fb5630e1990c942b461859650e2e469cda73
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34365014"
---
# <a name="use-activedirectoryinteractive-mode-to-connect-to-azure-sql-database"></a>Verwenden Sie den ActiveDirectoryInteractive Modus zum Herstellen der Verbindung mit Azure SQL-Datenbank

Dieser Artikel enthält ein ausführbares C#-Codebeispiel, das die Verbindung mit Ihrer Microsoft Azure SQL-Datenbank herstellt. Das C#-Programm verwendet den interaktiven Modus der Authentifizierung, der die mehrstufige Authentifizierung in Azure AD (Multi-Factor Authentication, MFA) unterstützt. Ein Verbindungsversuch kann z.B. einen Prüfcode enthalten, der an Ihr Mobiltelefon gesendet wird.

Weitere Informationen zur MFA-Unterstützung für SQL-Tools finden Sie unter [Azure Active Directory-Unterstützung in SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory).




## <a name="sqlauthenticationmethod-activedirectoryinteractive-enum-value"></a>SqlAuthenticationMethod. ActiveDirectoryInteractive-Enumerationswert

Ab .NET Framework Version 4.7.2 hat Enumeration [**SqlAuthenticationMethod**](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) einen neuen Wert **. ActiveDirectoryInteractive**. Bei Verwendung durch ein Client C#-Programm weist dieser Enumerationswert das System auf die Verwendung des interaktiven Azure AD-Modus hin, der MFA zur Authentifizierung bei Azure SQL-Datenbank unterstützt. Der Benutzer, der das Programm ausführt, erkennt die folgenden Dialogfelder:

1. Ein Dialogfeld, das einen Azure AD-Benutzernamen anzeigt und zur Eingabe des Kennworts des Azure AD-Benutzers auffordert.
    - Dieses Dialogfeld wird nicht angezeigt, wenn kein Kennwort erforderlich ist. Es ist kein Kennwort erforderlich, wenn die Domäne des Benutzers sich in einem Verbund mit Azure AD befindet.

    Wenn durch die in Azure AD festgelegte Richtlinie MFA für den Benutzer erzwungen wird, werden die folgenden Dialogfeldern angezeigt.

2. Nur wenn der Benutzer das MFA-Szenario erstmalig durchläuft, zeigt das System einen zusätzlichen Dialog an. Das Dialogfeld fordert die Eingabe einer Mobiltelefonnummer an, an die SMS-Nachrichten gesendet werden. Jede Nachricht enthält den *Überprüfungscode*, den der Benutzer im nächsten Dialogfeld eingeben muss.

3. Ein weiteres Dialogfeld fordert die Eingabe des MFA-Überprüfungscodes an, den das System an ein Mobiltelefon gesendet hat.

Informationen zum Konfigurieren von Azure AD, sodass MFA erforderlich ist, finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication in der Cloud](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud).

Screenshots zu diesen Dialogen finden Sie unter [Konfigurieren der mehrstufigen Authentifizierung für SQL Server Management Studio und Azure AD](sql-database-ssms-mfa-authentication-configure.md).

> [!TIP]
> Unsere allgemeine Suchseite für alle Arten von .NET Framework-APIs finden Sie unter dem folgenden Link zu unserem praktischen **.NET API Browser**-Tool:
>
> [https://docs.microsoft.com/dotnet/api/](https://docs.microsoft.com/dotnet/api/)
>
> Bei Hinzufügen des Typnamens zum optional angefügten **?term=**-Parameter kann die Suchseite unser Ergebnis für uns bereit halten:
>
> [https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod](https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod)


## <a name="preparations-for-c-by-using-the-azure-portal"></a>Vorbereitungen für C# mithilfe des Azure-Portals

Es wird davon ausgegangen, dass Sie bereits eine [Azure SQL-Datenbankserver-Instanz erstellt](sql-database-get-started-portal.md) haben und diese verfügbar ist.


### <a name="a-create-an-app-registration"></a>A. Erstellen einer App-Registrierung

Um Azure AD-Authentifizierung verwenden zu können, muss Ihr C#-Clientprogramm eine GUID als *clientId* angeben, wenn ihr Programm versucht, eine Verbindung herzustellen. Bei Abschluss einer App-Registrierung wird die GUID generiert und im Azure-Portal mit der Bezeichnung **Anwendungs-ID** angezeigt. Die Navigationsschritte lauten folgendermaßen:

1. Azure-Portal &gt; **Azure Active Directory** &gt; **App-Registrierung**

    ![App-Registrierung](media\active-directory-interactive-connect-azure-sql-db\sshot-create-app-registration-b20.png)

2. Der **Anwendungs-ID**-Wert wird generiert und angezeigt.

    ![Angezeigte App-ID](media\active-directory-interactive-connect-azure-sql-db\sshot-application-id-app-regis-mk49.png)

3. **Registrierte App** &gt; **Einstellungen** &gt; **Erforderliche Berechtigungen** &gt; **Hinzufügen**

    ![Berechtigungseinstellungen für die registrierte App](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

4. **Erforderliche Berechtigungen**  &gt; **API hinzufügen** &gt; **Eine API auswählen** &gt; **Azure SQL-Datenbank**

    ![Hinzufügen von Zugriff auf eine API für Azure SQL-Datenbank](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

5. **API-Zugriff** &gt; **Berechtigungen auswählen** &gt; **Delegierte Berechtigungen**

    ![Delegieren von Berechtigungen an die API für Azure SQL-Datenbank](media\active-directory-interactive-connect-azure-sql-db\sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)


### <a name="b-set-azure-ad-admin-on-your-sql-database-server"></a>B: Festlegen des Azure AD-Administrators auf dem SQL-Datenbankserver

Jede Azure SQL-Datenbank-Serverinstanz verfügt über einen eigenen logischen SQL-Server von Azure AD. Für unser C#-Szenario müssen Sie einen Azure AD-Administrator für Ihren Azure SQL-Server festlegen.

1. **SQL Server** &gt; **Active Directory-Administrator** &gt; **Administrator festlegen**

    - Weitere Informationen zu Azure AD-Administratoren und Benutzern für Azure SQL-Datenbank finden Sie unter [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit SQL-Datenbank, der verwalteten SQL-Datenbank-Instanz oder SQL Data Warehouse](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) in den Screenshots im Abschnitt **Bereitstellen eines Azure Active Directory-Administrators für Ihren Azure SQL-Datenbank-Server**.


### <a name="c-prepare-an-azure-ad-user-to-connect-to-a-specific-database"></a>C. Vorbereiten eines Azure AD-Benutzers auf das Herstellen der Verbindung mit einer bestimmten Datenbank

In dem spezifischen Azure AD für Ihre Azure SQL-Datenbank-Serverinstanz können Sie einen Benutzer hinzufügen, der Zugriff auf eine bestimmte Datenbank hat.

Weitere Informationen finden Sie unter [Verwenden der Azure Active Directory-Authentifizierung für die Authentifizierung bei SQL-Datenbank, der verwalteten Instanz oder SQL Data Warehouse](sql-database-aad-authentication.md).


### <a name="d-add-a-non-admin-user-to-azure-ad"></a>D: Hinzufügen eines Benutzers ohne Administratorrechte zu Azure AD

Der Azure AD-Administrator der SQL-Datenbank-Serverinstanz kann für das Herstellen der Verbindung mit Ihrer SQL-Datenbank-Serverinstanz verwendet werden. Das Hinzufügen eines Benutzers ohne Administratorrechte zu Azure AD ist jedoch ein allgemeinerer Fall. Wenn der Benutzer ohne Administratorrechte zum Herstellen der Verbindung verwendet wird, wird die MFA-Sequenz aufgerufen, wenn für diesen Benutzer MFA von Azure AD erzwungen wird.




## <a name="azure-active-directory-authentication-library-adal"></a>Azure Active Directory Authentication Library (ADAL)

Das C#-Programm hängt von dem Namespace **Microsoft.IdentityModel.Clients.ActiveDirectory, Version** ab. Die Klassen für diesen Namespace befinden sich in der Assembly gleichen Namens.

- Verwenden Sie NuGet zum Herunterladen und Installieren der ADAL-Assembly.
    - [https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)

- Fügen Sie der Assembly einen Verweis hinzu, um eine Kompilierung des C#-Programms zu unterstützen.




## <a name="sqlauthenticationmethod-enum"></a>SqlAuthenticationMethod-Enumeration

Ein Namespace, von dem das C#-Beispiel abhängt, ist **System.Data.SqlClient**. Von besonderem Interesse ist die Enumeration **SqlAuthenticationMethod**. Diese Enumeration hat die folgenden Werte:

- **SqlAuthenticationMethod.ActiveDirectory*Interactive***:&nbsp; Verwenden Sie diese mit einem Azure AD-Benutzernamen, um mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) zu erzielen.
    - Dieser Wert bildet den Schwerpunkt dieses Artikels. Er erzeugt eine interaktive Benutzeroberfläche durch die Anzeige von Dialogfeldern für das Kennwort des Benutzers und dann für die MFA-Überprüfung, wenn MFA für diesen Benutzer erzwungen wird.
    - Dieser Wert steht ab .NET Framework Version 4.7.2 zur Verfügung.

- **SqlAuthenticationMethod.ActiveDirectory*Integrated ***:&nbsp; Verwenden Sie diese für ein *Verbundkonto*. Bei einem Verbundkonto ist der Benutzername der Windows-Domäne bekannt. Diese Methode wird von MFA nicht unterstützt.

- **SqlAuthenticationMethod.ActiveDirectory*Password***:&nbsp; Verwenden Sie diese zur Authentifizierung, die einen Azure AD-Benutzer und das Kennwort des Benutzers erfordert. Azure SQL-Datenbank führt die Authentifizierung durch. Diese Methode wird von MFA nicht unterstützt.




## <a name="prepare-c-parameter-values-from-the-azure-portal"></a>Vorbereiten von C#-Parameterwerten aus dem Azure-Portal

Für eine erfolgreiche Ausführung des C#-Programms müssen Sie den folgenden statischen Felder die entsprechenden Werte zuweisen. Diese statischen Felder agieren im Programm wie Parameter. Hier werden die Felder mit vorgetäuschten Werten angezeigt. Es werden auch die Speicherorte im Azure-Portal angezeigt, von denen Sie die richtigen Werte erhalten:


| Name des statischen Felds | Vorgetäuschter Wert | Wo im Azure-Portal |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | „my-favorite-sqldb-svr.database.windows.net“ | **SQL-Server** &gt; **Filtern nach Name** |
| AzureAD_UserID | "user9@abc.onmicrosoft.com" | **Azure Active Directory** &gt; **Benutzer** &gt; **Neuer Gastbenutzer** |
| Initial_DatabaseName | „master“ | **SQL Server-Instanzen** &gt; **SQL-Datenbanken** |
| ClientApplicationID | „a94f9c62-97fe-4d19-b06d-111111111111“ | **Azure Active Directory** &gt; **App-Registrierungen**<br /> &nbsp; &nbsp; &gt; **Nach Name suchen** &gt; **Anwendungs-ID** |
| RedirectUri | neuer Uri( „https://bing.com/“) | **Azure Active Directory** &gt; **App-Registrierungen**<br /> &nbsp; &nbsp; &gt; **Nach Name suchen** &gt; *[Ihre App-Registrierung]* &gt;<br /> &nbsp; &nbsp; **Einstellungen** &gt; **RedirectURIs**<br /><br />Im Rahmen dieses Artikels ist jeder gültige Wert für RedirectUri geeignet. Der Wert wird in unserem Fall nicht wirklich verwendet. |
| &nbsp; | &nbsp; | &nbsp; |


Je nach Ihrem speziellen Szenario benötigen Sie möglicherweise nicht die Werte aller Parameter in der obigen Tabelle.




## <a name="run-ssms-to-verify"></a>Ausführen von SSMS zur Überprüfung

Es ist hilfreich, SQL Server Management Studio (SSMS) vor dem C#-Programm auszuführen. Die SSMS-Ausführung stellt sicher, dass verschiedene Konfigurationen richtig sind. Dann kann jeder Fehler des C#-Programms auf seinen Quellcode eingegrenzt werden.


#### <a name="verify-sql-database-firewall-ip-addresses"></a>Überprüfen der SQL-Datenbank-Firewall-IP-Adressen

Führen Sie SSMS auf demselben Computer und im selben Gebäude aus wie später das C#-Programm. Sie können jeden **Authentifizierungsmodus** verwenden, den Sie für den einfachsten halten. Bei Anzeichen, dass die Datenbankserver-Firewall Ihre IP-Adresse nicht akzeptiert, können Sie dies wie in [Firewallregeln auf Server- und Datenbankebene für Azure SQL-Datenbank](sql-database-firewall-configure.md) gezeigt beheben.


#### <a name="verify-multi-factor-authentication-mfa-for-azure-ad"></a>Überprüfen der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für Azure AD

Führen Sie SSMS erneut aus, wobei Sie **Authentifizierung** dieses Mal auf **Active Directory: universell mit MFA-Unterstützung** festlegen. Für diese Option müssen Sie über SSMS Version 17.5 oder höher verfügen.

Weitere Informationen finden Sie unter [Konfigurieren der mehrstufigen Authentifizierung für SQL Server Management Studio und Azure AD](sql-database-ssms-mfa-authentication-configure.md).




## <a name="c-code-example"></a>C#-Codebeispiel

Um dieses C#-Beispiel zu kompilieren, müssen Sie einen Verweis auf die DLL-Assembly mit dem Namen **Microsoft.IdentityModel.Clients.ActiveDirectory** hinzufügen.


#### <a name="reference-documentation"></a>Referenzdokumentation

- **System.Data.SqlClient**-Namespace:
    - Suchen:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient)
    - Leiten:&nbsp; [System.Data.Client](https://docs.microsoft.com/dotnet/api/system.data.sqlclient)

- **Microsoft.IdentityModel.Clients.ActiveDirectory**-Namespace:
    - Suchen:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory](https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory)
    - Leiten:&nbsp; [Microsoft.IdentityModel.Clients.ActiveDirectory](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory)


#### <a name="c-source-code-in-two-parts"></a>C#-Quellcode, in zwei Teilen

&nbsp;

```csharp

using System;    // C# ,  part 1 of 2.

// Add a reference to assembly:  Microsoft.IdentityModel.Clients.ActiveDirectory.DLL
using Microsoft.IdentityModel.Clients.ActiveDirectory;

using DA = System.Data;
using SC = System.Data.SqlClient;
using AD = Microsoft.IdentityModel.Clients.ActiveDirectory;
using TX = System.Text;
using TT = System.Threading.Tasks;

namespace ADInteractive5
{
    class Program
    {
        // ASSIGN YOUR VALUES TO THESE STATIC FIELDS !!
        static public string Az_SQLDB_svrName = "<YOUR VALUE HERE>";
        static public string AzureAD_UserID = "<YOUR VALUE HERE>";
        static public string Initial_DatabaseName = "master";
        // Some scenarios do not need values for the following two fields:
        static public readonly string ClientApplicationID = "<YOUR VALUE HERE>";
        static public readonly Uri RedirectUri = new Uri("<YOUR VALUE HERE>");

        public static void Main(string[] args)
        {
            var provider = new ActiveDirectoryAuthProvider();

            SC.SqlAuthenticationProvider.SetProvider(
                SC.SqlAuthenticationMethod.ActiveDirectoryInteractive,
                //SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated,  // Alternatives.
                //SC.SqlAuthenticationMethod.ActiveDirectoryPassword,
                provider);

            Program.Connection();
        }

        public static void Connection()
        {
            SC.SqlConnectionStringBuilder builder = new SC.SqlConnectionStringBuilder();

            // Program._  static values that you set earlier.
            builder["Data Source"] = Program.Az_SQLDB_svrName;
            builder.UserID = Program.AzureAD_UserID;
            builder["Initial Catalog"] = Program.Initial_DatabaseName;

            // This "Password" is not used with .ActiveDirectoryInteractive.
            //builder["Password"] = "<YOUR PASSWORD HERE>";

            builder["Connect Timeout"] = 15;
            builder["TrustServerCertificate"] = true;
            builder.Pooling = false;

            // Assigned enum value must match the enum given to .SetProvider().
            builder.Authentication = SC.SqlAuthenticationMethod.ActiveDirectoryInteractive;
            SC.SqlConnection sqlConnection = new SC.SqlConnection(builder.ConnectionString);

            SC.SqlCommand cmd = new SC.SqlCommand(
                "SELECT '******** MY QUERY RAN SUCCESSFULLY!! ********';",
                sqlConnection);

            try
            {
                sqlConnection.Open();
                if (sqlConnection.State == DA.ConnectionState.Open)
                {
                    var rdr = cmd.ExecuteReader();
                    var msg = new TX.StringBuilder();
                    while (rdr.Read())
                    {
                        msg.AppendLine(rdr.GetString(0));
                    }
                    Console.WriteLine(msg.ToString());
                    Console.WriteLine(":Success");
                }
                else
                {
                    Console.WriteLine(":Failed");
                }
                sqlConnection.Close();
            }
            catch (Exception ex)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Connection failed with the following exception...");
                Console.WriteLine(ex.ToString());
                Console.ResetColor();
            }
        }
    } // EOClass Program .

```

&nbsp;

#### <a name="second-half-of-the-c-program"></a>Zweite Hälfte des C#-Programms

Zur besseren visuellen Darstellung wird das C#-Programm in zwei Codeblöcke aufgeteilt. Fügen Sie zum Ausführen des Programms die zwei Codeblöcke zusammen.

&nbsp;

```csharp

    // C# ,  part 2 of 2 ,  to concatenate below part 1.

    /// <summary>
    /// SqlAuthenticationProvider - Is a public class that defines 3 different Azure AD
    /// authentication methods.  The methods are supported in the new .NET 4.7.2 .
    ///  . 
    /// 1. Interactive,  2. Integrated,  3. Password
    ///  . 
    /// All 3 authentication methods are based on the Azure
    /// Active Directory Authentication Library (ADAL) managed library.
    /// </summary>
    public class ActiveDirectoryAuthProvider : SC.SqlAuthenticationProvider
    {
        // Program._ more static values that you set!
        private readonly string _clientId = Program.ClientApplicationID;
        private readonly Uri _redirectUri = Program.RedirectUri;

        public override async TT.Task<SC.SqlAuthenticationToken>
            AcquireTokenAsync(SC.SqlAuthenticationParameters parameters)
        {
            AD.AuthenticationContext authContext =
                new AD.AuthenticationContext(parameters.Authority);
            authContext.CorrelationId = parameters.ConnectionId;
            AD.AuthenticationResult result;

            switch (parameters.AuthenticationMethod)
            {
                case SC.SqlAuthenticationMethod.ActiveDirectoryInteractive:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,  // "https://database.windows.net/"
                        _clientId,
                        _redirectUri,
                        new AD.PlatformParameters(AD.PromptBehavior.Auto),
                        new AD.UserIdentifier(
                            parameters.UserId,
                            AD.UserIdentifierType.RequiredDisplayableId));
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_1 == '.ActiveDirectoryIntegrated'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserCredential());
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryPassword:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_2 == '.ActiveDirectoryPassword'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserPasswordCredential(
                            parameters.UserId,
                            parameters.Password));
                    break;

                default: throw new InvalidOperationException();
            }
            return new SC.SqlAuthenticationToken(result.AccessToken, result.ExpiresOn);
        }

        public override bool IsSupported(SC.SqlAuthenticationMethod authenticationMethod)
        {
            return authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryInteractive
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryPassword;
        }
    } // EOClass ActiveDirectoryAuthProvider .
} // EONamespace.  End of entire program source code.

```

&nbsp;

#### <a name="actual-test-output-from-c"></a>Tatsächliche Testausgabe in C#

```
[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>> ADInteractive5.exe
In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.
******** MY QUERY RAN SUCCESSFULLY!! ********

:Success

[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>>
```

&nbsp;


## <a name="next-steps"></a>Nächste Schritte

- [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlserveractivedirectoryadministrator)

