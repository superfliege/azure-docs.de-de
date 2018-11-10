---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/25/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 874643c5149cf6a222ab6d244dd561e5c4da5a84
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50251395"
---
In diesem Artikel werden PowerShell-Cmdlets verwendet. Um die Cmdlets auszuführen, können Sie Azure Cloud Shell verwenden, eine kostenlose interaktive Shell. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Klicken Sie einfach auf **Kopieren**, um den Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen. Cloud Shell kann auf mehrere Arten gestartet werden:

|  |   |
|-----------------------------------------------|---|
| Klicken Sie in der rechten oberen Ecke eines Codeblocks auf **Ausprobieren**. | ![Cloud Shell in diesem Artikel](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-powershell-try-it.png) |
| Öffnen Sie Cloud Shell in Ihrem Browser. | [![https://shell.azure.com/powershell](./media/vpn-gateway-cloud-shell-powershell/launchcloudshell.png)](https://shell.azure.com/powershell) |
| Klicken Sie auf die Schaltfläche **Cloud Shell** im Menü oben rechts im Azure-Portal. | [![Cloud Shell im Portal](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-menu.png)](https://portal.azure.com) |
|  |  |

Wenn Sie Azure Cloud Shell nicht verwenden möchten, können Sie PowerShell lokal installieren. Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager installieren. PowerShell-Cmdlets werden häufig aktualisiert, sodass Sie Ihre PowerShell-Cmdlets normalerweise aktualisieren müssen, um die neuesten Features und Funktionen nutzen zu können. Wenn Sie Ihre PowerShell-Cmdlets nicht aktualisieren, tritt für die angegebenen Werte unter Umständen ein Fehler auf. 

Um die Version von PowerShell zu finden, die Sie lokal ausführen, verwenden Sie das Cmdlet „Get-Module -ListAvailable AzureRM“. Sollte ein Upgrade erforderlich sein, lesen Sie [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps). Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).