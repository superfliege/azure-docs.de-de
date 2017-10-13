---
title: Features von Bash in Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation
description: "Übersicht über die Features von Bash in Azure Cloud Shell"
services: Azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: juluk
ms.openlocfilehash: 158b0b65ba53630050275c910f2a7acfc39b8759
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="features--tools-for-bash-in-azure-cloud-shell"></a>Features und Tools für Bash in Azure Cloud Shell

[!include [features-introblock](../../includes/cloud-shell-features-introblock.md)]

> [!TIP]
> Features und Tools sind auch in [PowerShell](features-powershell.md) verfügbar.

Bash in Cloud Shell wird unter `Ubuntu 16.04 LTS` ausgeführt.

## <a name="features"></a>Features

### <a name="secure-automatic-authentication"></a>Sichern der automatischen Authentifizierung

Bash in Cloud Shell authentifiziert Zugriff auf Konten für Azure CLI 2.0 sicher und automatisch.

### <a name="ssh-into-azure-linux-virtual-machines"></a>SSH-Verbindungen zu virtuellen Linux-Computer in Azure

Das Erstellen eines virtuellen Linux-Computers über Azure-CLI 2.0 kann einen SSH-Standardschlüssel erstellen und ihn in Ihrem `$Home`-Verzeichnis platzieren. Das Platzieren von SSH-Schlüsseln in `$Home` ermöglicht direkte SSH-Verbindungen von Cloud Shell zu virtuellen Linux-Computern in Azure. Schlüssel werden in Ihrer Dateifreigabe in „acc_<user>.img“ gespeichert. Nutzen Sie bewährte Methoden bei der Verwendung oder Freigabe des Zugriffs auf Ihre Dateifreigabe oder Schlüssel.

### <a name="home-persistence-across-sessions"></a>Sitzungsübergreifende $Home-Persistenz

Damit Sie Dateien sitzungsübergreifend beibehalten können, wird Ihnen beim ersten Start von Cloud Shell das Anfügen einer Azure-Dateifreigabe gezeigt.
Anschließend fügt Cloud Shell Ihren Speicher (als `$Home\clouddrive` eingebunden) automatisch für alle zukünftigen Sitzungen an.
Darüber hinaus wird Ihr `$Home`-Verzeichnis in Bash in Cloud Shell als IMG-Datei in Ihrer Azure-Dateifreigabe gespeichert.
Dateien außerhalb von `$Home` und der Zustand des Computers werden nicht sitzungsübergreifend beibehalten.

[Erfahren Sie mehr über das Beibehalten von Dateien in Bash in Cloud Shell.](persisting-shell-storage.md)

## <a name="tools"></a>Tools

|Kategorie   |Name   |
|---|---|
|Linux-Shellinterpreter|Bash<br> sh               |
|Azure-Tools            |[Azure CLI 2.0](https://github.com/Azure/azure-cli) und [1.0](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [Batch Shipyard](https://github.com/Azure/batch-shipyard) <br>    |
|Text-Editoren           |Vim<br> Nano<br> Emacs       |
|Quellcodeverwaltung         |Git                    |
|Buildtools            |Make<br> Maven<br> npm<br> pip         |
|Container             |[Docker-CLI](https://github.com/docker/cli)/[Docker Machine](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Helm](https://github.com/kubernetes/helm)<br> [DC/OS-CLI](https://github.com/dcos/dcos-cli)         |
|Datenbanken              |MySQL-Client<br> PostgreSQL-Client<br> [SQLCMD-Hilfsprogramm](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli) |
|Sonstige                  |iPython-Client<br> [Cloud Foundry-CLI](https://github.com/cloudfoundry/cli)<br> [Terraform](https://www.terraform.io/docs/providers/azurerm/)<br> |

## <a name="language-support"></a>Sprachunterstützung

|Sprache   |Version   |
|---|---|
|.NET       |1.01       |
|Go         |1.7        |
|Java       |1.8        |
|Node.js    |6.9.4      |
|PowerShell  |[6.0 (Beta)](https://github.com/PowerShell/powershell/releases)       |
|Python     |2.7 und 3.5 (Standard)|

## <a name="next-steps"></a>Nächste Schritte
[Bash in Cloud Shell – Schnellstart](quickstart.md) <br>
[Erfahren Sie mehr über Azure CLI 2.0](https://docs.microsoft.com/cli/azure/).
