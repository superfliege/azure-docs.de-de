---
title: Azure Cloud Shell-Features | Microsoft-Dokumentation
description: Übersicht über die Features von Bash in Azure Cloud Shell
services: Azure
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: juluk
ms.openlocfilehash: 1321645d97e7f6ff2faed1e61ddb608afcb7b413
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2018
ms.locfileid: "40038100"
---
# <a name="features--tools-for-azure-cloud-shell"></a>Features und Tools für Azure Cloud Shell

[!INCLUDE [features-introblock](../../includes/cloud-shell-features-introblock.md)]

Azure Cloud Shell wird unter `Ubuntu 16.04 LTS` ausgeführt.

## <a name="features"></a>Features

### <a name="secure-automatic-authentication"></a>Sichern der automatischen Authentifizierung

Cloud Shell authentifiziert den Zugriff auf Konten für Azure CLI 2.0 und Azure PowerShell sicher und automatisch.

### <a name="home-persistence-across-sessions"></a>Sitzungsübergreifende $Home-Persistenz

Damit Sie Dateien sitzungsübergreifend beibehalten können, wird Ihnen beim ersten Start von Cloud Shell das Anfügen einer Azure-Dateifreigabe gezeigt.
Anschließend fügt Cloud Shell Ihren Speicher (als `$Home\clouddrive` eingebunden) automatisch für alle zukünftigen Sitzungen an.
Darüber hinaus wird Ihr `$Home`-Verzeichnis als IMG-Datei in Ihrer Azure-Dateifreigabe gespeichert.
Dateien außerhalb von `$Home` und der Zustand des Computers werden nicht sitzungsübergreifend beibehalten. Verwenden Sie beim Speichern von Geheimnissen (z.B. SSH-Schlüssel) bewährte Methoden. Dienste wie [Azure Key Vault enthalten Tutorials für das Setup](https://docs.microsoft.com/azure/key-vault/key-vault-manage-with-cli2#prerequisites).

[Erfahren Sie mehr über das Beibehalten von Dateien in Cloud Shell.](persisting-shell-storage.md)

### <a name="azure-drive-azure"></a>Azure-Laufwerk (Azure:)

PowerShell in Cloud Shell (Vorschauversion) startet auf dem Azure-Laufwerk (`Azure:`).
Das Azure-Laufwerk ermöglicht die einfache Ermittlung und Navigation von Azure-Ressourcen wie Computing, Netzwerk, Speicher usw., die der Navigation im Dateisystem ähnelt.
Sie können weiterhin unabhängig vom Laufwerk, in dem Sie sich befinden, die vertrauten [Azure PowerShell-Cmdlets](https://docs.microsoft.com/powershell/azure) zur Verwaltung dieser Ressourcen verwenden.
Alle an den Azure-Ressourcen vorgenommenen Änderungen, die entweder direkt im Azure-Portal oder über Azure PowerShell-Cmdlets durchgeführt wurden, werden auf dem Azure-Laufwerk reflektiert.  Sie können `dir -Force` ausführen, um Ihre Ressourcen zu aktualisieren.

![](media/features-powershell/azure-drive.png)

### <a name="deep-integration-with-open-source-tooling"></a>Enge Integration in Open Source-Tools

Cloud Shell enthält eine vorkonfigurierte Authentifizierung für Open Source-Tools wie Terraform, Ansible oder Chef InSpec. Probieren Sie es mithilfe der exemplarischen Vorgehensweisen aus.

## <a name="tools"></a>Tools

|Category (Kategorie)   |NAME   |
|---|---|
|Linux-Tools            |Bash<br> zsh<br> sh<br> tmux<br> dig<br>               |
|Azure-Tools            |[Azure CLI 2.0](https://github.com/Azure/azure-cli) und [1.0](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [Service Fabric-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) |
|Text-Editoren           |Vim<br> Nano<br> Emacs       |
|Quellcodeverwaltung         |Git                    |
|Buildtools            |Make<br> Maven<br> npm<br> pip         |
|Container             |[Docker-CLI](https://github.com/docker/cli)/[Docker Machine](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Helm](https://github.com/kubernetes/helm)<br> [DC/OS-CLI](https://github.com/dcos/dcos-cli)         |
|Datenbanken              |MySQL-Client<br> PostgreSQL-Client<br> [SQLCMD-Hilfsprogramm](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli) |
|Andere                  |iPython-Client<br> [Cloud Foundry-CLI](https://github.com/cloudfoundry/cli)<br> [Terraform](https://www.terraform.io/docs/providers/azurerm/)<br> [Ansible](https://www.ansible.com/microsoft-azure)<br> [Chef InSpec](https://www.chef.io/inspec/)| 

## <a name="language-support"></a>Sprachunterstützung

|Sprache   |Version   |
|---|---|
|.NET Core  |2.0.0       |
|Go         |1.9        |
|Java       |1.8        |
|Node.js    |8.9.4      |
|PowerShell |[6.1.0-preview.4](https://github.com/PowerShell/powershell/releases)       |
|Python     |2.7 und 3.5 (Standard)|

## <a name="next-steps"></a>Nächste Schritte
[Bash in Cloud Shell – Schnellstart](quickstart.md) <br>
[Schnellstart für PowerShell in Cloud Shell (Vorschauversion)](quickstart-powershell.md) <br>
[Erfahren Sie mehr über Azure CLI 2.0](https://docs.microsoft.com/cli/azure/) <br>
[Informationen zu Azure PowerShell](https://docs.microsoft.com/powershell/azure/) <br>
