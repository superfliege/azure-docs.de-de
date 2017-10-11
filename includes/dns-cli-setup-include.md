## <a name="set-up-azure-cli-for-azure-dns"></a>Einrichten von Azure-CLI für Azure-DNS

### <a name="before-you-begin"></a>Vorbereitung

Stellen Sie sicher, dass Sie die folgenden Elemente aufweisen, bevor die Konfiguration.

* Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement noch nicht haben, können Sie aktivieren die [Vorteile für MSDN-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für eine [Konto frei](https://azure.microsoft.com/pricing/free-trial/).
* Installieren Sie die neueste Version der Azure-CLI für Windows, Linux oder Mac. Weitere Informationen finden Sie unter [installieren Sie die Azure-CLI](../articles/cli-install-nodejs.md).

### <a name="sign-in-to-your-azure-account"></a>Melden Sie sich beim Azure-Konto

Ein Konsolenfenster geöffnet und mit Ihren Anmeldeinformationen authentifiziert werden. Weitere Informationen finden Sie unter [melden Sie sich bei Azure an, aus der Azure-CLI](../articles/xplat-cli-connect.md)

```azurecli
azure login
```

### <a name="switch-cli-mode"></a>Switch-CLI-Modus

Azure DNS verwendet Azure Resource Manager. Stellen Sie sicher, dass Sie CLI-Modus zur Verwendung von Azure Resource Manager-Befehle wechseln.

```azurecli
azure config mode arm
```

### <a name="select-the-subscription"></a>Wählen Sie das Abonnement

Überprüfen Sie die Abonnements für das Konto ein.

```azurecli
azure account list
```

Wählen Sie das Azure-Abonnements verwenden.

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Azure-Ressourcen-Manager erfordert, dass alle Ressourcengruppen auf einen Speicherort anzugeben. Dies wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet werden. Da alle DNS-Ressourcen global, nicht Land/Region sind, hat die Auswahl der Speicherort der Ressourcengruppe allerdings keine Auswirkungen auf Azure DNS.

Sie können diesen Schritt überspringen, wenn Sie eine vorhandene Ressourcengruppe verwenden.

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a>Registrierung des Ressourcenanbieters

Der Azure DNS-Dienst wird vom Ressourcenanbieter Microsoft.Network verwaltet. Ihr Azure-Abonnement muss registriert sein, um diesen Ressourcenanbieter vor der Verwendung von Azure DNS zu verwenden. Dies ist ein Einmaliger Vorgang für jedes Abonnement.

```azurecli
azure provider register --namespace Microsoft.Network
```

