## <a name="setting-up-powershell-for-resource-manager-templates"></a>Einrichten von PowerShell für Ressourcen-Manager-Vorlagen
Bevor Sie Azure PowerShell mit dem Ressourcen-Manager verwenden können, müssen Sie das Recht Windows PowerShell und die Azure PowerShell-Versionen verfügen.

### <a name="verify-powershell-versions"></a>Überprüfen von Versionen von PowerShell
Stellen Sie sicher, dass Sie Windows PowerShell, Version 3.0 oder 4.0 verfügen. Um die Version von Windows PowerShell zu suchen, geben Sie folgenden Befehl an einer Windows PowerShell-Eingabeaufforderung ein.

    $PSVersionTable

Sie erhalten folgende Art von Informationen:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Überprüfen Sie, ob der Wert der **PSVersion** 3.0 oder 4.0 ist. Falls nicht, siehe [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Legen Sie Ihr Azure-Konto und Ihr Abonnement
Wenn Sie ein Azure-Abonnement noch nicht haben, können Sie aktivieren die [MSDN-abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

Öffnen Sie ein Azure PowerShell-Eingabeaufforderung, und melden Sie sich bei Azure mit dem folgenden Befehl ein.

    Login-AzureRmAccount

Wenn Sie mehrere Azure-Abonnements verfügen, können Sie Ihre Azure-Abonnements mit dem folgenden Befehl aufführen.

    Get-AzureRmSubscription

Sie erhalten folgende Art von Informationen:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Sie können das aktuelle Azure-Abonnement festlegen, indem Sie diese Befehle an der Azure-PowerShell-Eingabeaufforderung ausführen. Ersetzen Sie alles innerhalb der Anführungszeichen, einschließlich der Zeichen, mit dem richtigen Namen < und >.

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Weitere Informationen zu Azure-Abonnements und Dienstkonten finden Sie unter [Vorgehensweise: Herstellen einer Verbindung mit Ihrem Abonnement](/powershell/azureps-cmdlets-docs#step-3-connect).

