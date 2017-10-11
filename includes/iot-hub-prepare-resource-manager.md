## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Vorbereiten der Azure Resource Manager-Anforderungen zu authentifizieren
Sie müssen alle Vorgänge, die Sie ausführen, auf Ressourcen mit Authentifizieren der [Azure Resource Manager] [ lnk-authenticate-arm] mit Azure Active Directory (AD). Die einfachste Möglichkeit, diese Konfiguration ist die Verwendung von PowerShell oder Azure-CLI.

Installieren der [Azure PowerShell-Cmdlets] [ lnk-powershell-install] bevor Sie fortfahren.

Die folgenden Schritte zeigen, wie Sie die Kennwort-Authentifizierung für eine AD-Anwendung mithilfe von PowerShell einrichten. Sie können diese Befehle in einer standard-PowerShell-Sitzung ausführen.

1. Melden Sie sich mit Ihrem Azure-Abonnement mit dem folgenden Befehl aus:

    ```powershell
    Login-AzureRmAccount
    ```

1. Wenn Sie mehrere Azure-Abonnements verfügen, gewährt die sich bei Azure anmelden Zugriff auf alle Azure Abonnements, die Ihre Anmeldeinformationen zugeordnet. Verwenden Sie den folgenden Befehl, um die Azure-Abonnements für Ihre Verwendung aufzulisten:

    ```powershell
    Get-AzureRMSubscription
    ```

    Verwenden Sie den folgenden Befehl Abonnement auswählen, die zum Ausführen der Befehle zum Verwalten von Ihren IoT Hubs verwendet werden sollen. Sie können den Abonnementnamen oder die ID aus der Ausgabe des vorherigen Befehls verwenden:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. Notieren Sie sich Ihre **"tenantid"** und **"subscriptionId"**. Sie benötigen sie später erneut.
3. Erstellen einer neuen Azure Active Directory-Anwendung, die mit dem folgenden Befehl ein, und Ersetzen Sie dabei die Platzhaltern:
   
   * **{Anzeigename}:** einen Anzeigenamen für Ihre Anwendung z. B. **MySampleApp**
   * **{URL der Homepage}:** die URL der Startseite der app wie z. B. **http://mysampleapp/home**. Diese URL muss nicht auf einer realen Anwendung verweisen.
   * **{ID}:** ein eindeutiger Bezeichner, wie z. B. **http://mysampleapp**. Diese URL muss nicht auf einer realen Anwendung verweisen.
   * **{Password}:** ein Kennwort, das Sie mit der app zur Authentifizierung verwenden.
     
     ```powershell
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
     ```
4. Notieren Sie sich die **ApplicationId** der Anwendung, die Sie erstellt haben. Dies ist später erforderlich.
5. Erstellen Sie einen neuen Dienstprinzipal mithilfe des folgenden Befehls, und Ersetzen Sie dabei **{MyApplicationId}** mit der **ApplicationId** aus dem vorherigen Schritt:
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. Richten Sie eine rollenzuweisung, die mithilfe des folgenden Befehls, und Ersetzen Sie dabei **{MyApplicationId}** mit Ihrem **ApplicationId**.
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

Sie haben nun erstellen die Azure AD-Anwendung, die Ihnen ermöglicht, die von der benutzerdefinierten C#-Anwendung zu authentifizieren. Sie benötigen die folgenden Werte später in diesem Lernprogramm:

* TenantId
* "SubscriptionId"
* ApplicationId
* Kennwort

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
