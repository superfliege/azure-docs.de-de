## <a name="prepare-for-akv-integration"></a>Vorbereitung der AKV-Integration
Um Azure Key Vault-Integration verwenden, um Ihre virtuellen SQL Server-Computer konfigurieren, stehen Ihnen verschiedene Voraussetzungen: 

1. [Installieren Sie Azure Powershell](#install-azure-powershell)
2. [Erstellen Sie eine Azure Active Directory](#create-an-azure-active-directory)
3. [Erstellen eines schlüsseltresors](#create-a-key-vault)

In den folgenden Abschnitten werden diese erforderlichen Komponenten und die Informationen, die zu sammelnden führen, dass später die PowerShell-Cmdlets beschrieben.

### <a name="install-azure-powershell"></a>Installieren Sie Azure PowerShell
Stellen Sie sicher, dass Sie das neueste Azure PowerShell SDK installiert haben. Weitere Informationen finden Sie unter [zum Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a name="create-an-azure-active-directory"></a>Erstellen Sie eine Azure Active Directory
Zunächst müssen Sie haben eine [Azure Active Directory](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) in Ihrem Abonnement. Für viele Vorteile haben können Sie diese Berechtigung gewähren, Ihren schlüsseltresor für bestimmte Benutzer und Anwendungen.

Registrieren Sie als Nächstes eine Anwendung in AAD. Sie erhalten ein dienstprinzipalkonto, die Zugriff auf Ihren schlüsseltresor besitzt, die Ihren virtuellen Computer benötigen. In den Azure Key Vault-Artikel finden Sie folgende Schritte in der [Registrieren einer Anwendung in Azure Active Directory](../articles/key-vault/key-vault-get-started.md#register) Abschnitt, oder Sie können finden Sie unter den Schritten und Screenshots in der **erhalten Sie eine Identität für den Anwendungsabschnitt** von [diesem Blogbeitrag](http://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx). Beachten Sie bevor Sie diese Schritte ausführen, dass Sie die folgenden Informationen ein, während diese Registrierung zu sammeln, die später benötigt wird, wenn Sie Azure Key Vault-Integration für die SQL-VM aktivieren müssen.

* Nachdem die Anwendung hinzugefügt wurde, suchen die **Clientkennung** auf die **konfigurieren** Registerkarte. 
    ![Azure Active Directory-Client-ID](./media/virtual-machines-sql-server-akv-prepare/aad-client-id.png)
  
    Die Client-ID zugewiesen ist, später die **$spName** (Service Principal Name)-Parameter in der PowerShell-Skript für Azure Key Vault-Integration zu aktivieren. 
* Während der folgenden Schritte aus, wenn Sie Ihren Schlüssel erstellen, kopieren Sie außerdem den geheimen Schlüssel für Ihren Schlüssel wie im folgenden Screenshot gezeigt wird. Dieser Schlüssel geheim später zugewiesen ist die **$spSecret** (Service Principal geheimer Schlüssel)-Parameter in der PowerShell-Skript.  
    ![Azure Active Directory geheimen Schlüssel](./media/virtual-machines-sql-server-akv-prepare/aad-sp-secret.png)
* Autorisieren Sie diese neue Client-ID haben die folgenden Zugriffsberechtigungen: **verschlüsseln**, **entschlüsseln**, **WrapKey**, **UnwrapKey**, **Anmeldung**, und **überprüfen**. Dies erfolgt mit der [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625.aspx) Cmdlet. Weitere Informationen finden Sie unter [Autorisieren der Anwendung auf den Schlüssel oder des geheimen Schlüssels](../articles/key-vault/key-vault-get-started.md#authorize).

### <a name="create-a-key-vault"></a>Erstellen eines schlüsseltresors
Um Azure Key Vault verwenden, um die Schlüssel zu speichern, die Sie für die Verschlüsselung in Ihrem virtuellen Computer verwenden, benötigen Sie Zugriff auf einen schlüsseltresor. Wenn Sie Ihren schlüsseltresor noch nicht eingerichtet haben, erstellen Sie anhand der Schritte in der [erste Schritte mit Azure Key Vault](../articles/key-vault/key-vault-get-started.md) Thema. Bevor Sie diese Schritte ausführen, wird Beachten Sie, dass es einige Informationen, die Sie während dieser einrichten, die gesammelt werden müssen später benötigt, wenn Sie Azure Key Vault-Integration für die SQL-VM aktivieren.

Wenn Sie einen schlüsseltresor Schritt das Erstellen erhalten, beachten Sie das zurückgegebene **VaultUri** -Eigenschaft, die die schlüsseltresor-URL ist. In dem Beispiel in diesem Schritt, der weiter unten gezeigte schlüsseltresorname ContosoKeyVault, ist daher wäre die schlüsseltresor-URL https://contosokeyvault.vault.azure.net/.

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Die schlüsseltresor-URL hat zu einem späteren Zeitpunkt die **$akvURL** Parameter in der PowerShell-Skript für Azure Key Vault-Integration zu aktivieren.

