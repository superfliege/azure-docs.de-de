
## <a name="start-your-powershell-session"></a>Starten Sie Ihre PowerShell-Sitzung
Zunächst müssen Sie das aktuellste [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) installiert ist und ausgeführt. Ausführliche Informationen finden Sie unter [zum Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> In den Beispielen in diesem Thema [Modell zur Bereitstellung von Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md), damit die Beispiele verwenden die [Azure Resource Manager-Cmdlets](http://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Führen Sie die [ **hinzufügen AzureRmAccount** ](http://msdn.microsoft.com/library/mt619267.aspx) -Cmdlet, und Sie werden mit einem Bildschirmen zum Anmelden Ihre Anmeldeinformationen eingeben angezeigt werden. Verwenden Sie die gleichen Anmeldeinformationen, mit denen Sie zum Azure-Portal anmelden.

    Add-AzureRmAccount

Bei Verwendung mehrerer Abonnements verwenden die [ **Satz AzureRmContext** ](http://msdn.microsoft.com/library/mt619263.aspx) Cmdlet, um welche Abonnement auswählen, sollten Ihre PowerShell-Sitzung verwenden. Um anzuzeigen, welches Abonnement die aktuellen PowerShell-Sitzung verwendet wird, führen Sie [ **Get-AzureRmContext**](http://msdn.microsoft.com/library/mt619265.aspx). Um alle Ihre Abonnements anzuzeigen, führen Sie [ **Get-AzureRmSubscription**](http://msdn.microsoft.com/library/mt619284.aspx).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

