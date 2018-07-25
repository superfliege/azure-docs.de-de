---
title: Aktivieren der Public Preview von Azure Firewall
description: Verwenden von Azure PowerShell zum Aktivieren der Public Preview von Azure Firewall
author: vhorne
ms.service: firewall
services: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: 263b16a419b5ff20a9b6d62860385f92c2a18f9c
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992386"
---
# <a name="enable-the-azure-firewall-public-preview"></a>Aktivieren der Public Preview von Azure Firewall

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

## <a name="enable-using-azure-powershell"></a>Aktivieren mithilfe von Azure PowerShell

Verwenden Sie zum Aktivieren der Public Preview von Azure Firewall die folgenden Azure PowerShell-Befehle:

```PowerShell
Register-AzureRmProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Register-AzureRmProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```

Es dauert bis zu 30 Minuten, bis die Featureregistrierung abgeschlossen ist. Sie können den Registrierungsstatus überprüfen, indem Sie die folgenden Azure PowerShell-Befehle ausführen:

```PowerShell

Get-AzureRmProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Get-AzureRmProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```
Führen Sie im Anschluss an die Registrierung den folgenden Befehl aus:

```PowerShell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal](tutorial-firewall-deploy-portal.md)

