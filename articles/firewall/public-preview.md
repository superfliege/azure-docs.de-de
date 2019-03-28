---
title: Aktivieren der Public Preview von Azure Firewall
description: Verwenden von Azure PowerShell zum Aktivieren der Public Preview von Azure Firewall
author: vhorne
ms.service: firewall
services: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: fe1b8f9d56b0f4faa0baa25463b2aa29a59715cb
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499589"
---
# <a name="enable-the-azure-firewall-public-preview"></a>Aktivieren der Public Preview von Azure Firewall

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

## <a name="enable-using-azure-powershell"></a>Aktivieren mithilfe von Azure PowerShell

Verwenden Sie zum Aktivieren der Public Preview von Azure Firewall die folgenden Azure PowerShell-Befehle:

```PowerShell
Register-AzProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network
Register-AzProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```

Es dauert bis zu 30 Minuten, bis die Featureregistrierung abgeschlossen ist. Sie können den Registrierungsstatus überprüfen, indem Sie die folgenden Azure PowerShell-Befehle ausführen:

```powershell

Get-AzProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Get-AzProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```
Führen Sie im Anschluss an die Registrierung den folgenden Befehl aus:

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal](tutorial-firewall-deploy-portal.md)

