---
title: "Verwenden einer Windows-VM-MSI für den Zugriff auf Azure SQL"
description: "Dieses Tutorial erläutert, wie Sie eine Windows-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) verwenden, um auf Azure SQL zuzugreifen."
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/12/2017
ms.author: skwan
ms.openlocfilehash: f2cfef1c2aed90e111d06fc3090973e093fd7a4f
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2017
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-sql"></a>Verwenden einer Windows-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) für den Zugriff auf Azure SQL

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine verwaltete Dienstidentität (Managed Service Identity, MSI) für einen virtuellen Windows-Computer für den Zugriff auf einen Azure SQL-Server verwenden. Verwaltete Dienstidentitäten werden von Azure automatisch verwaltet und ermöglichen Ihnen die Authentifizierung für Dienste, die die Azure AD-Authentifizierung unterstützen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Aktivieren von MSI auf einem virtuellen Windows-Computer 
> * Gewähren des Zugriffs auf eine Azure SQL-Server durch Ihre VM
> * Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwenden dieses Zugriffstokens zum Aufrufen eines Azure SQL-Servers


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Windows-Computers in einer neuen Ressourcengruppe

In diesem Tutorial wird ein neuer virtueller Windows-Computer erstellt.  Sie können MSI auch auf einem vorhandenen virtuellen Computer aktivieren.

1.  Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
2.  Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**. 
3.  Geben Sie die Informationen zum virtuellen Computer ein. Der **Benutzername** und das **Kennwort**, die hier erstellt werden, sind die Anmeldeinformationen, die Sie für die Anmeldung beim virtuellen Computer verwenden.
4.  Wählen Sie in der Dropdownliste das richtige **Abonnement** für den virtuellen Computer aus.
5.  Um eine neue **Ressourcengruppe** auszuwählen, in der der virtuelle Computer erstellt werden soll, wählen Sie **Neu erstellen**. Klicken Sie zum Abschluss auf **OK**.
6.  Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. Behalten Sie auf der Seite „Einstellungen“ die Standardwerte bei, und klicken Sie auf **OK**.

    ![Alternativer Bildtext](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Aktivieren von MSI auf dem virtuellen Computer 

Eine VM-MSI ermöglicht es Ihnen, Zugriffstoken aus Azure AD abzurufen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen. Durch das Aktivieren von MSI weisen Sie Azure an, eine verwaltete Identität für den virtuellen Computer zu erstellen. Im Hintergrund werden durch das Aktivieren von MSI zwei Vorgänge ausgelöst: Auf dem virtuellen Computer wird die MSI-VM-Erweiterung installiert, und MSI wird im Azure Resource Manager aktiviert.

1.  Wählen Sie den **virtuellen Computer** aus, auf dem Sie MSI aktivieren möchten.  
2.  Klicken Sie in der links angezeigten Navigationsleiste auf **Konfiguration**. 
3.  Die Option **Verwaltete Dienstidenität** wird angezeigt. Wählen Sie zum Registrieren und Aktivieren von MSI die Option **Ja** oder zum Deaktivieren „Nein“. 
4.  Achten Sie darauf, zum Speichern der Konfiguration auf **Speichern** zu klicken.  
    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Wenn Sie überprüfen möchten, welche Erweiterungen sich auf diesem virtuellen Computer befinden, klicken Sie auf **Erweiterungen**. Wenn MSI aktiviert ist, wird **ManagedIdentityExtensionforWindows** in der Liste angezeigt.

    ![Alternativer Bildtext](media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Gewähren des Zugriffs auf eine Datenbank auf einem Azure SQL-Server durch Ihre VM

Nun können Sie Ihrer VM den Zugriff auf eine Datenbank auf einem Azure SQL-Server gewähren.  Für diesen Schritt können Sie einen vorhandenen SQL-Server verwenden oder einen neuen erstellen.  Zum Erstellen eines neuen Servers und einer Datenbank mithilfe des Azure-Portals befolgen Sie diesen [Azure SQL-Schnellstart](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal). Es gibt in der [Azure SQL-Dokumentation](https://docs.microsoft.com/azure/sql-database/) auch Schnellstarts, in denen die Azure-Befehlszeilenschnittstelle und Azure PowerShell verwendet werden.

Das Gewähren des Zugriffs durch eine VM auf eine Datenbank umfasst drei Schritte:
1.  Erstellen einer Gruppe in Azure AD und Hinzufügen der VM-MSI als Mitglied dieser Gruppe
2.  Aktivieren der Azure AD-Authentifizierung für den SQL-Server
3.  Erstellen eines in der Datenbank **enthaltenen Benutzers**, der die Azure AD-Gruppe darstellt

> [!NOTE]
> Normalerweise würden Sie einen enthaltenen Benutzer erstellen, der direkt der MSI des virtuellen Computers zugeordnet ist.  Azure SQL erlaubt es derzeit nicht, den Azure AD-Dienstprinzipal, der die VM-MSI darstellt, einem enthaltenen Benutzer zuzuordnen.  Zum Umgehen dieser Einschränkung machen Sie die VM-MSI zu einem Mitglied einer Azure AD-Gruppe und erstellen dann einen enthaltenen Benutzer in der Datenbank, die die Gruppe darstellt.


### <a name="create-a-group-in-azure-ad-and-make-the-vm-msi-a-member-of-the-group"></a>Erstellen einer Gruppe in Azure AD und Hinzufügen der VM-MSI als Mitglied dieser Gruppe

Sie können eine vorhandene Azure AD-Gruppe verwenden oder mithilfe von Azure AD PowerShell eine neue erstellen.  

Installieren Sie zunächst das [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2)-Modul. Melden Sie sich mit `Connect-AzureAD` an, und führen Sie den folgenden Befehl aus, um die Gruppe zu erstellen und in einer Variable zu speichern:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

Die Ausgabe ähnelt der folgenden, die auch den Wert der Variable untersucht:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM MSI access to SQL
```

Fügen Sie anschließend die VM-MSI der Gruppe hinzu.  Sie benötigen die **ObjectId** der MSI, die Sie mit Azure PowerShell abrufen können.  Laden Sie zunächst [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) herunter. Melden Sie sich anschließend mit `Login-AzureRmAccount` an, und führen Sie die folgenden Befehle aus:
- Wenn Sie über mehrere Azure-Abonnements verfügen, stellen Sie sicher, dass der Sitzungskontext auf das richtige festgelegt ist.
- Listen Sie die verfügbaren Ressourcen in Ihrem Azure-Abonnement auf, und überprüfen Sie die Ressourcengruppe und die Namen der virtuellen Computer.
- Rufen Sie die Eigenschaften der VM-MSI ab, und verwenden Sie dabei die richtigen Werte für `<RESOURCE-GROUP>` und `<VM-NAME>`.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

Die Ausgabe ähnelt der folgenden, die auch die Objekt-ID des Dienstprinzipals der VM-MSI untersucht:
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Fügen Sie nun die VM-MSI der Gruppe hinzu.  Sie können einen Dienstprinzipal nur mit Azure AD PowerShell einer Gruppe hinzufügen.  Führen Sie den folgenden Befehl aus:
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

### <a name="enable-azure-ad-authentication-for-the-sql-server"></a>Aktivieren der Azure AD-Authentifizierung für den SQL-Server

Nachdem Sie die Gruppe erstellt und ihr die VM-MSI als Mitglied hinzugefügt haben, können Sie die [Azure AD-Authentifizierung für den SQL-Server konfigurieren](/azure/sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-server). Gehen Sie dazu wie folgt vor:

1.  Wählen Sie im Azure-Portal im Navigationsbereich links **SQL-Server** aus.
2.  Klicken Sie auf den SQL-Server, für den die Azure AD-Authentifizierung aktiviert werden soll.
3.  Klicken Sie auf dem Blatt im Bereich **Einstellungen** auf **Active Directory-Administrator**.
4.  Klicken Sie auf der Befehlsleiste auf **Administrator festlegen**.
5.  Wählen Sie ein Azure AD-Benutzerkonto als Administrator des Servers aus, und klicken Sie auf **Auswählen**.
6.  Klicken Sie auf der Befehlsleiste auf **Speichern**.

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Erstellen eines in der Datenbank enthaltenen Benutzers, der die Azure AD-Gruppe darstellt

Für den nächsten Schritt benötigen Sie [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Bevor Sie beginnen, sollten Sie die folgenden Artikel mit Hintergrundinformationen zur Azure AD-Integration lesen:

- [Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication.md)
- [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit SQL-Datenbank oder SQL Data Warehouse](/azure/sql-database/sql-database-aad-authentication-configure.md)

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
     CREATE USER [VM MSI access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     Der Befehl sollte erfolgreich abgeschlossen werden und den enthaltenen Benutzer für die Gruppe erstellen.
11.  Leeren Sie das Abfragefenster, geben Sie die folgende Zeile ein, und klicken Sie auf der Symbolleiste auf **Ausführen**:
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM MSI access to SQL]
     ```

     Der Befehl sollte erfolgreich abgeschlossen werden und dem enthaltenen Benutzer die Möglichkeit gewähren, die gesamte Datenbank zu lesen.

Auf dem virtuellen Computer ausgeführter Code kann jetzt ein Token von der MSI abrufen und damit die Authentifizierung auf dem SQL-Server durchführen.

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-sql"></a>Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwenden dieses Zugriffstokens zum Aufrufen von Azure SQL 

Azure SQL unterstützt die Azure AD-Authentifizierung nativ, sodass Zugriffstoken, die mit der MSI abgerufen wurden, direkt angenommen werden können.  Verwenden Sie das Verfahren für **Zugriffstoken**, um eine Verbindung mit SQL herzustellen.  Dieses ist Teil des Azure SQL-Integration in Azure AD und unterscheidet sich vom Bereitstellen von Anmeldeinformationen in der Verbindungszeichenfolge.

Mit dem folgenden .NET-Codebeispiel wird eine Verbindung mit SQL mithilfe eines Zugriffstokens hergestellt.  Dieser Code muss auf dem virtuellen Computer ausgeführt werden, um auf den VM-MSI-Endpunkt zugreifen zu können.  Für die Verwendung des Verfahrens mit Zugriffstoken ist **.NET Framework 4.6** oder höher erforderlich.  Ersetzen Sie die Werte für AZURE-SQL-SERVERNAME und DATABASE entsprechend.  Beachten Sie, dass die Ressourcen-ID für Azure SQL „https://database.windows.net/“ lautet.

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://localhost:50342/oauth2/token?resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call MSI endpoint.
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
4.  Erstellen Sie mithilfe des PowerShell-Befehls `Invoke-WebRequest` eine Anforderung an den lokalen MSI-Endpunkt, um ein Zugriffstoken für Azure SQL zu erhalten.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://database.windows.net/"} -Headers @{Metadata="true"}
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

Überprüfen Sie den Wert von `$DataSet.Tables[0]` zum Anzeigen der Ergebnisse der Abfrage.  Herzlichen Glückwunsch, Sie haben die Datenbank mithilfe einer VM-MSI abgefragt, ohne dafür Anmeldeinformationen anzugeben!

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](../active-directory/msi-overview.md).
- Erfahren Sie mehr über das [Verwenden der Azure Active Directory-Authentifizierung für die Authentifizierung mit SQL-Datenbank oder SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).
- Erfahren Sie mehr über das [Konfigurieren der Azure Active Directory-Authentifizierung mit SQL-Datenbank oder SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure).
- Erfahren Sie mehr über [Erste Schritte mit Berechtigungen für das Datenbankmodul](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.