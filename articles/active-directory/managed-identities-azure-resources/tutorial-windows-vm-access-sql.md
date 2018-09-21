---
title: Verwenden der systemseitig zugewiesenen verwalteten Identität eines virtuellen Windows-Computers für den Zugriff auf Azure SQL
description: In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Identität eines virtuellen Windows-Computers verwenden, um auf Azure SQL zuzugreifen.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: bryanla
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: 57f9def09f498c3fc644cbee979d5ae552f2206c
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46369392"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-sql"></a>Tutorial: Verwenden einer systemseitig zugewiesenen verwalteten Identität eines virtuellen Windows-Computers für den Zugriff auf Azure SQL

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene Identität für einen virtuellen Windows-Computer verwenden, um auf eine Azure SQL Server-Instanz zuzugreifen. Verwaltete Dienstidentitäten werden von Azure automatisch verwaltet und ermöglichen Ihnen die Authentifizierung für Dienste, die die Azure AD-Authentifizierung unterstützen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Gewähren des Zugriffs auf eine Azure SQL-Server durch Ihre VM
> * Erstellen einer Gruppe in Azure AD und Hinzufügen der systemseitig zugewiesenen verwalteten Identität des virtuellen Computers zu dieser Gruppe
> * Aktivieren der Azure AD-Authentifizierung für den SQL-Server
> * Erstellen eines in der Datenbank enthaltenen Benutzers, der die Azure AD-Gruppe darstellt
> * Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwenden dieses Zugriffstokens zum Aufrufen eines Azure SQL-Servers

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [Anmelden beim Azure-Portal](https://portal.azure.com)

- [Erstellen eines virtuellen Windows-Computers](/azure/virtual-machines/windows/quick-create-portal)

- [Aktivieren einer systemseitig zugewiesenen verwalteten Identität für Ihren virtuellen Computer](/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm#enable-system-assigned-identity-on-an-existing-vm)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Gewähren des Zugriffs auf eine Datenbank auf einem Azure SQL-Server durch Ihre VM

Nun können Sie Ihrer VM den Zugriff auf eine Datenbank auf einem Azure SQL-Server gewähren.  Für diesen Schritt können Sie einen vorhandenen SQL-Server verwenden oder einen neuen erstellen.  Zum Erstellen eines neuen Servers und einer Datenbank mithilfe des Azure-Portals befolgen Sie diesen [Azure SQL-Schnellstart](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal). Es gibt in der [Azure SQL-Dokumentation](https://docs.microsoft.com/azure/sql-database/) auch Schnellstarts, in denen die Azure-Befehlszeilenschnittstelle und Azure PowerShell verwendet werden.

Das Gewähren des Zugriffs durch eine VM auf eine Datenbank umfasst drei Schritte:
1.  Erstellen einer Gruppe in Azure AD und Hinzufügen der systemseitig zugewiesenen verwalteten Identität des virtuellen Computers zu dieser Gruppe
2.  Aktivieren der Azure AD-Authentifizierung für den SQL-Server
3.  Erstellen eines in der Datenbank **enthaltenen Benutzers**, der die Azure AD-Gruppe darstellt

> [!NOTE]
> Normalerweise würden Sie einen enthaltenen Benutzer erstellen, der direkt der systemseitig zugewiesenen verwalteten Identität des virtuellen Computers zugeordnet ist.  Mit Azure SQL ist es derzeit nicht möglich, den Azure AD-Dienstprinzipal, der die systemseitig zugewiesene verwaltete Identität des virtuellen Computers darstellt, einem enthaltenen Benutzer zuzuordnen.  Zur Umgehung dieser Einschränkung machen Sie die systemseitig zugewiesene verwaltete Identität des virtuellen Computers zu einem Mitglied einer Azure AD-Gruppe und erstellen dann einen enthaltenen Benutzer in der Datenbank, der die Gruppe darstellt.


## <a name="create-a-group-in-azure-ad-and-make-the-vms-system-assigned-managed-identity-a-member-of-the-group"></a>Erstellen einer Gruppe in Azure AD und Hinzufügen der systemseitig zugewiesenen verwalteten Identität des virtuellen Computers zu dieser Gruppe

Sie können eine vorhandene Azure AD-Gruppe verwenden oder mithilfe von Azure AD PowerShell eine neue erstellen.  

Installieren Sie zunächst das [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2)-Modul. Melden Sie sich mit `Connect-AzureAD` an, und führen Sie den folgenden Befehl aus, um die Gruppe zu erstellen und in einer Variable zu speichern:

```powershell
$Group = New-AzureADGroup -DisplayName "VM managed identity access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

Die Ausgabe ähnelt der folgenden, die auch den Wert der Variable untersucht:

```powershell
$Group = New-AzureADGroup -DisplayName "VM managed identity access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM managed identity access to SQL
```

Fügen Sie als Nächstes die systemseitig zugewiesene verwaltete Identität des virtuellen Computers der Gruppe hinzu.  Sie benötigen die Objekt-ID (**ObjectId**) der systemseitig zugewiesenen verwalteten Identität. Diese ID können Sie mit Azure PowerShell abrufen.  Laden Sie zunächst [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) herunter. Melden Sie sich anschließend mit `Connect-AzureRmAccount` an, und führen Sie die folgenden Befehle aus:
- Wenn Sie über mehrere Azure-Abonnements verfügen, stellen Sie sicher, dass der Sitzungskontext auf das richtige festgelegt ist.
- Listen Sie die verfügbaren Ressourcen in Ihrem Azure-Abonnement auf, und überprüfen Sie die Ressourcengruppe und die Namen der virtuellen Computer.
- Rufen Sie die Eigenschaften der systemseitig zugewiesenen verwalteten Identität des virtuellen Computers ab, und verwenden Sie dabei die passenden Werte für `<RESOURCE-GROUP>` und `<VM-NAME>`.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

Die Ausgabe sieht in etwa wie die folgende Ausgabe aus, in der auch die Objekt-ID des Dienstprinzipals der systemseitig zugewiesenen verwalteten Identität des virtuellen Computers untersucht wird:
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Fügen Sie nun die systemseitig zugewiesene verwaltete Identität des virtuellen Computers der Gruppe hinzu.  Sie können einen Dienstprinzipal nur mit Azure AD PowerShell einer Gruppe hinzufügen.  Führen Sie den folgenden Befehl aus:
```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
```

Wenn Sie danach auch die Gruppenmitgliedschaft untersuchen, sieht die Ausgabe wie folgt aus:

```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
Get-AzureAdGroupMember -ObjectId $Group.ObjectId

ObjectId                             AppId                                DisplayName
--------                             -----                                -----------
b83305de-f496-49ca-9427-e77512f6cc64 0b67a6d6-6090-4ab4-b423-d6edda8e5d9f DevTestWinVM
```

## <a name="enable-azure-ad-authentication-for-the-sql-server"></a>Aktivieren der Azure AD-Authentifizierung für den SQL-Server

Nachdem Sie die Gruppe erstellt und ihr die systemseitig zugewiesene verwaltete Identität des virtuellen Computers als Mitglied hinzugefügt haben, können Sie die [Azure AD-Authentifizierung für den SQL-Server konfigurieren](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-azure-sql-server). Gehen Sie dazu wie folgt vor:

1.  Wählen Sie im Azure-Portal im Navigationsbereich links **SQL-Server** aus.
2.  Klicken Sie auf den SQL-Server, für den die Azure AD-Authentifizierung aktiviert werden soll.
3.  Klicken Sie auf dem Blatt im Bereich **Einstellungen** auf **Active Directory-Administrator**.
4.  Klicken Sie auf der Befehlsleiste auf **Administrator festlegen**.
5.  Wählen Sie ein Azure AD-Benutzerkonto als Administrator des Servers aus, und klicken Sie auf **Auswählen**.
6.  Klicken Sie auf der Befehlsleiste auf **Speichern**.

## <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Erstellen eines in der Datenbank enthaltenen Benutzers, der die Azure AD-Gruppe darstellt

Für den nächsten Schritt benötigen Sie [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Bevor Sie beginnen, sollten Sie die folgenden Artikel mit Hintergrundinformationen zur Azure AD-Integration lesen:

- [Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication)
- [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit SQL-Datenbank oder SQL Data Warehouse](/azure/sql-database/sql-database-aad-authentication-configure)

1.  Starten Sie SQL Server Management Studio.
2.  Geben Sie im Dialogfeld **Mit Server verbinden** den Namen Ihres SQL-Servers im Feld **Servername** ein.
3.  Wählen Sie im Feld **Authentifizierung** die Option **Active Directory: universell mit MFA-Unterstützung** aus.
4.  Geben Sie im Feld **Benutzername** den Namen des Azure AD-Kontos ein, das Sie als Serveradministrator festlegen möchten, z.B. helen@woodgroveonline.com.
5.  Klicken Sie auf **Options**.
6.  Geben Sie im Feld **Mit Datenbank verbinden** den Namen der systemfremden Datenbank ein, die Sie konfigurieren möchten.
7.  Klicken Sie auf **Verbinden**.  Schließen Sie den Anmeldevorgang ab.
8.  Erweitern Sie im **Objekt-Explorer** den Ordner **Datenbanken**.
9.  Klicken Sie mit der rechten Maustaste auf eine Benutzerdatenbank, und klicken Sie dann auf **Neue Abfrage**.
10.  Geben Sie im Abfragefenster die folgende Zeile ein, und klicken Sie auf der Symbolleiste auf **Ausführen**:
    
     ```
     CREATE USER [VM managed identity access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     Der Befehl sollte erfolgreich abgeschlossen werden und den enthaltenen Benutzer für die Gruppe erstellen.
11.  Leeren Sie das Abfragefenster, geben Sie die folgende Zeile ein, und klicken Sie auf der Symbolleiste auf **Ausführen**:
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM managed identity access to SQL]
     ```

     Der Befehl sollte erfolgreich abgeschlossen werden und dem enthaltenen Benutzer die Möglichkeit gewähren, die gesamte Datenbank zu lesen.

Auf dem virtuellen Computer ausgeführter Code kann jetzt unter Verwendung der systemseitig zugewiesenen verwalteten Identität ein Token abrufen und damit die Authentifizierung bei der SQL Server-Instanz durchführen.

## <a name="get-an-access-token-using-the-vms-system-assigned-managed-identity-and-use-it-to-call-azure-sql"></a>Abrufen eines Zugriffstokens mithilfe der systemseitig zugewiesenen verwalteten Identität des virtuellen Computers und Verwenden dieses Zugriffstokens zum Aufrufen von Azure SQL 

Azure SQL unterstützt die Azure AD-Authentifizierung nativ, sodass Zugriffstoken, die mit verwalteten Identitäten für Azure-Ressourcen abgerufen wurden, direkt angenommen werden können.  Verwenden Sie das Verfahren für **Zugriffstoken**, um eine Verbindung mit SQL herzustellen.  Dieses ist Teil des Azure SQL-Integration in Azure AD und unterscheidet sich vom Bereitstellen von Anmeldeinformationen in der Verbindungszeichenfolge.

Mit dem folgenden .NET-Codebeispiel wird eine Verbindung mit SQL mithilfe eines Zugriffstokens hergestellt.  Dieser Code muss auf dem virtuellen Computer ausgeführt werden, um auf den Endpunkt der systemseitig zugewiesenen verwalteten Identität des virtuellen Computers zugreifen zu können.  Für die Verwendung des Verfahrens mit Zugriffstoken ist **.NET Framework 4.6** oder höher erforderlich.  Ersetzen Sie die Werte für AZURE-SQL-SERVERNAME und DATABASE entsprechend.  Beachten Sie, dass die Ressourcen-ID für Azure SQL „https://database.windows.net/“ ist.

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call managed identities for Azure resources endpoint.
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader and extract access token.
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

//
// Open a connection to the SQL server using the access token.
//
if (accessToken != null) {
    string connectionString = "Data Source=<AZURE-SQL-SERVERNAME>; Initial Catalog=<DATABASE>;";
    SqlConnection conn = new SqlConnection(connectionString);
    conn.AccessToken = accessToken;
    conn.Open();
}
```

Eine andere schnelle Möglichkeit zum Testen der umfassenden Einrichtung ohne Schreiben und Bereitstellen einer App auf dem virtuellen Computer bietet PowerShell.

1.  Navigieren Sie im Portal zu **Virtuelle Computer**, wechseln Sie dann zu Ihrem virtuellen Windows-Computer, und klicken Sie in der **Übersicht** auf **Verbinden**. 
2.  Geben Sie Ihren **Benutzernamen** und Ihr **Kennwort** ein, das Sie beim Erstellen des virtuellen Windows-Computers hinzugefügt haben. 
3.  Sie haben nun eine **Remotedesktopverbindung** mit dem virtuellen Computer erstellt. Öffnen Sie jetzt **PowerShell** in der Remotesitzung. 
4.  Erstellen Sie mithilfe des PowerShell-Befehls `Invoke-WebRequest` eine Anforderung an den lokalen Endpunkt der verwalteten Identität, um ein Zugriffstoken für Azure SQL zu erhalten.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fdatabase.windows.net%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    Konvertieren Sie die Antwort aus einem JSON-Objekt in ein PowerShell-Objekt. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Extrahieren Sie das Zugriffstoken aus der Antwort.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5.  Öffnen Sie eine Verbindung mit dem SQL-Server. Denken Sie daran, die Werte für AZURE-SQL-SERVERNAME und DATABASE zu ersetzen.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Als Nächstes erstellen Sie eine Abfrage und senden diese an den Server.  Denken Sie daran, den Wert für TABLE zu ersetzen.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Überprüfen Sie den Wert von `$DataSet.Tables[0]` zum Anzeigen der Ergebnisse der Abfrage.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine systemseitig zugewiesene verwaltete Identität für den Zugriff auf Azure SQL Server verwenden.  Weitere Informationen zu Azure SQL Server:

> [!div class="nextstepaction"]
>[Azure SQL-Datenbankdienst](/azure/sql-database/sql-database-technical-overview)
