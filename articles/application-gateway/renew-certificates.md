---
title: Verlängern des Azure Application Gateway-Zertifikats
description: Es wird beschrieben, wie Sie ein Zertifikat verlängern, das einem Anwendungsgateway-Listener zugeordnet ist.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.date: 05/18/2018
ms.author: victorh
ms.openlocfilehash: b44a57fe8ebcc985d3ab66ea04936a1558d00863
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598265"
---
# <a name="renew-application-gateway-certificates"></a>Verlängern von Application Gateway-Zertifikaten

Sie müssen Ihre Zertifikate nach einiger Zeit verlängern, falls sie für Ihr Anwendungsgateway die SSL-Verschlüsselung konfiguriert haben.

Sie können ein Zertifikat, das einem Listener zugeordnet ist, verlängern, indem Sie entweder das Azure-Portal, Azure PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden:

## <a name="azure-portal"></a>Azure-Portal

Navigieren Sie zu Ihren Anwendungsgateway-Listenern, um ein Listenerzertifikat über das Portal zu verlängern. Klicken Sie auf den Listener, der über ein zu verlängerndes Zertifikat verfügt, und klicken Sie anschließend auf **Ausgewähltes Zertifikat erneuern oder bearbeiten**.

![Verlängern des Zertifikats](media/renew-certificate/ssl-cert.png)

Laden Sie Ihr neues PFX-Zertifikat hoch, geben Sie ihm einen Namen, geben Sie das Kennwort ein, und klicken Sie dann auf **Speichern**.

## <a name="azure-powershell"></a>Azure PowerShell

Verwenden Sie das folgende Cmdlet, um Ihr Zertifikat mit Azure PowerShell zu verlängern:

```azurepowershell-interactive
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-azureRmApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password
```
## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren der SSL-Auslagerung mit Azure Application Gateway finden Sie unter [Konfigurieren der SSL-Auslagerung](application-gateway-ssl-portal.md).
