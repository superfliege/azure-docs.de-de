---
title: Herunterladen von Azure Stack-Tools von GitHub | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Tools herunterladen, die für die Arbeit mit Azure Stack benötigt werden."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 20cac6eace89d86f8fa6d7640357e19c8d7359b3
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="download-azure-stack-tools-from-github"></a>Herunterladen von Azure Stack-Tools von GitHub

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

„AzureStack-Tools“ ist ein GitHub-Repository, das PowerShell-Module hostet, die Sie zum Verwalten und Bereitstellen von Ressourcen für Azure Stack verwenden können. Sie können diese PowerShell-Module in das Azure Stack Development Kit oder in einen Windows-basierten externen Client herunterladen und verwenden, wenn Sie planen, die VPN-Konnektivität einzurichten. Um diese Tools zu erhalten, klonen Sie das GitHub-Repository, oder laden Sie den Ordner „AzureStack-Tools“ herunter. 

Um das Repository zu klonen, laden Sie [Git](https://git-scm.com/download/win) für Windows herunter, öffnen Sie ein Befehlszeilenfenster, und führen Sie das folgende Skript aus:

```PowerShell
# Change directory to the root directory 
cd \

# clone the repository
git clone https://github.com/Azure/AzureStack-Tools.git --recursive

# Change to the tools directory
cd AzureStack-Tools
```

Um den Ordner „Tools“ herunterzuladen, führen Sie das folgende Skript aus:

```PowerShell
# Change directory to the root directory 
cd \

# Download the tools archive
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory
cd AzureStack-Tools-master

```

## <a name="functionalities-provided-by-the-modules"></a>Von den Modulen bereitgestellte Funktionen

Das Repository „AzureStack-Tools“ umfasst PowerShell-Module, die die folgenden Funktionen für Azure Stack unterstützen:  

| Funktionalität | Beschreibung | Wer kann dieses Modul verwenden? |
| --- | --- | --- |
| [Cloudfunktionen](user/azure-stack-validate-templates.md) | Verwenden Sie dieses Modul, um die Cloudfunktionen einer Cloud zu nutzen. Beispielsweise können Sie mithilfe dieses Moduls Cloudfunktionen für Azure Stack und Azure-Clouds nutzen. Zu diesen Funktionen zählen z.B. die API-Version, Azure Resource Manager-Ressourcen, VM-Erweiterungen usw. | Cloudoperatoren und Benutzer. |
| [Azure Stack-Computeverwaltung](azure-stack-add-vm-image.md) | Mit diesem Modul können Sie dem Azure Stack-Marketplace ein VM-Image hinzufügen bzw. dieses daraus entfernen. | Cloudoperatoren. |
| [Verwaltung der Azure Stack-Infrastruktur](https://github.com/Azure/AzureStack-Tools/blob/master/Infrastructure/README.md) | Verwenden Sie dieses Modul zum Verwalten von VMs, Warnungen, Updates usw. der Azure Stack-Infrastruktur. |  Cloudoperatoren.|
| [Resource Manager-Richtlinie für Azure Stack](user/azure-stack-policy-module.md) | Verwenden Sie dieses Modul zum Konfigurieren eines Azure-Abonnements oder einer Azure-Ressourcengruppe mit der gleichen Versionsverwaltung und Dienstverfügbarkeit wie Azure Stack. | Cloudoperatoren und Benutzer |
| [Registrieren bei Azure](azure-stack-register.md) | Verwenden Sie dieses Modul zum Registrieren Ihrer Development Kit-Instanz bei Azure. Nach der Registrierung können Sie die Marketplace-Elemente von Azure herunterladen und in Azure Stack verwenden. | Cloudoperatoren |
| [Azure Stack-Bereitstellung](azure-stack-run-powershell-script.md) | Verwenden Sie dieses Modul zum Vorbereiten des Azure Stack-Hostcomputers auf die Bereitstellung und die erneute Bereitstellung mithilfe des Azure Stack-VHD-Images (virtuelle Festplatte). | Cloudoperatoren. |
| [Herstellen einer Verbindung mit Azure Stack](azure-stack-connect-powershell.md) | Verwenden Sie dieses Modul zum Herstellen einer Verbindung mit einer Azure Stack-Instanz über PowerShell und zum Konfigurieren der VPN-Konnektivität mit Azure Stack. | Cloudoperatoren und Benutzer |
| [Azure Stack-Dienstverwaltung](azure-stack-create-offer.md) | Azure Stack-Administratoren können dieses Modul verwenden, um ein Standardmandantenangebot mit unbegrenztem Kontingent in Compute-, Storage-, Netzwerk- und Key Vault-Diensten zu erstellen.   | Cloudoperatoren.|
| [Validierungssteuerelement für Vorlagen](user/azure-stack-validate-templates.md) | Verwenden Sie dieses Modul, um zu überprüfen, ob eine vorhandene oder eine neue Vorlage für Azure Stack bereitgestellt werden kann. | Cloudoperatoren und Benutzer |


## <a name="next-steps"></a>Nächste Schritte
* [Konfigurieren Sie die PowerShell-Umgebung des Azure Stack-Benutzers.](user/azure-stack-powershell-configure-user.md)   
* [Connect to Azure Stack Development Kit over a VPN (Herstellen einer Verbindung mit dem Azure Stack Development Kit über VPN)](azure-stack-connect-azure-stack.md)  

