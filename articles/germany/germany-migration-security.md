---
title: Migration von Security-Ressourcen von Azure Deutschland zu Azure weltweit
description: Dieser Artikel bietet Unterstützung bei der Migration von Security-Ressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
---

# <a name="migration-of-security-resources-from-azure-germany-to-global-azure"></a>Migration von Security-Ressourcen von Azure Deutschland zu Azure weltweit

Dieser Artikel unterstützt Sie bei der Migration von Azure Security-Ressourcen von Azure Deutschland zu Azure weltweit.

## <a name="azure-active-directory"></a>Azure Active Directory

Dieser Dienst wird unter [Migration von Identitäten](./germany-migration-identity.md#azure-active-directory) behandelt.





## <a name="key-vault"></a>Key Vault

### <a name="encryption-keys"></a>Verschlüsselungsschlüssel

Verschlüsselungsschlüssel können nicht migriert werden. Erstellen Sie neue Schlüssel in der Zielregion, und verwenden Sie diese, um die Zielressource (Storage, SQL-Datenbank usw.) zu schützen. Migrieren Sie dann Daten sicher aus der alten Region zur neuen Region.

### <a name="application-secrets"></a>Anwendungsgeheimnisse

Anwendungsgeheimnisse sind Zertifikate, Speicherkontoschlüssel und andere anwendungsbezogene Geheimnisse.

- Erstellen Sie einen neuen KeyVault in Azure weltweit.
- Erstellen Sie neue Anwendungsgeheimnisse, **oder**
- lesen Sie die aktuellen Geheimnisse in Azure Deutschland, und geben Sie den Wert in den neuen Tresor ein.

```powershell
Get-AzureKeyVaultSecret -vaultname mysecrets -name Deploydefaultpw
```

### <a name="next-steps"></a>Nächste Schritte

Frischen Sie Ihre Kenntnisse zu Key Vault anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/key-vault/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Key Vault: Übersicht](../key-vault/key-vault-overview.md)
- [KeyVault-PowerShell-Cmdlets](/powershell/module/azurerm.keyvault/?view=azurermps-6.5.0)















## <a name="vpn-gateway"></a>VPN Gateway

Die Migration von VPN Gateways zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Es wird empfohlen, ein neues VPN Gateway zu erstellen und zu konfigurieren.

Erfassen Sie Informationen zu Ihrer aktuellen VPN Gatewaykonfiguration über das Portal oder mithilfe von PowerShell. Es gibt eine Reihe von Cmdlets, die mit `Get-AzureRmVirtualNetworkGateway*` beginnen.

Vergessen Sie nicht, Ihre lokale Konfiguration zu aktualisieren und vorhandene Regeln für die alten IP-Bereiche zu löschen, sobald Sie Ihre Azure-Netzwerkumgebung aktualisiert haben.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu VPN Gateways anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/vpn-gateway/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Erstellen einer Site-to-Site-Verbindung](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) mit dem VPN Gateway
- PowerShell-Cmdlets [Get-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/get-azurermvirtualnetworkgateway?view=azurermps-6.5.0)
- Blog: [Erstellen einer Site-to-Site-Verbindung](https://blogs.technet.microsoft.com/ralfwi/2017/02/02/connecting-clouds/) zwischen Azure Deutschland und Azure weltweit
 






## <a name="application-gateway"></a>Application Gateway

Die Migration von Application Gateways zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Es wird empfohlen, ein neues Gateway zu erstellen und zu konfigurieren.

Erfassen Sie Informationen zu Ihrer aktuellen Gatewaykonfiguration über das Portal oder mithilfe von PowerShell. Es gibt eine Reihe von Cmdlets, die mit `Get-AzureRmApplicationGateway*` beginnen.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Application Gateways anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/application-gateway/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Erstellen eines Application Gateways](../application-gateway/quick-create-portal.md)
- PowerShell-Cmdlets [Get-AzureRmApplicationGateway](/powershell/module/azurerm.network/get-azurermapplicationgateway?view=azurermps-6.5.0)

