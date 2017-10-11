## <a name="set-up-azure-powershell-for-azure-dns"></a>Einrichten von Azure PowerShell für Azure-DNS

### <a name="before-you-begin"></a>Vorbereitung

Stellen Sie sicher, dass Sie die folgenden Elemente aufweisen, bevor die Konfiguration.

* Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement noch nicht haben, können Sie aktivieren die [Vorteile für MSDN-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für eine [Konto frei](https://azure.microsoft.com/pricing/free-trial/).
* Sie müssen die neueste Version der Azure-Ressourcen-Manager-PowerShell-Cmdlets zu installieren. Weitere Informationen finden Sie unter [zum Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a name="sign-in-to-your-azure-account"></a>Melden Sie sich beim Azure-Konto

Öffnen Sie die PowerShell-Konsole, und Herstellen einer Verbindung mit Ihrem Konto. Weitere Informationen finden Sie unter [mithilfe von PowerShell mit dem Ressourcen-Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).

```powershell
Login-AzureRmAccount
```

### <a name="select-the-subscription"></a>Wählen Sie das Abonnement
 
Überprüfen Sie die Abonnements für das Konto ein.

```powershell
Get-AzureRmSubscription
```

Wählen Sie das Azure-Abonnements verwenden.

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Azure-Ressourcen-Manager erfordert, dass alle Ressourcengruppen auf einen Speicherort anzugeben. Dieser Speicherort wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen global, nicht Land/Region sind, hat die Auswahl der Speicherort der Ressourcengruppe allerdings keine Auswirkungen auf Azure DNS.

Sie können diesen Schritt überspringen, wenn Sie eine vorhandene Ressourcengruppe verwenden.

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a>Registrierung des Ressourcenanbieters

Der Azure DNS-Dienst wird vom Ressourcenanbieter Microsoft.Network verwaltet. Ihr Azure-Abonnement muss registriert sein, um diesen Ressourcenanbieter vor der Verwendung von Azure DNS zu verwenden. Dies ist ein Einmaliger Vorgang für jedes Abonnement.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```