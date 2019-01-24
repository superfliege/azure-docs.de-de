---
title: 'PowerShell: Aktivieren von BYOK-TDE: verwaltete Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation'
description: Erfahren Sie, wie eine verwaltete Azure SQL-Datenbank-Instanz mithilfe von PowerShell für die Verwendung von Transparent Data Encryption (TDE) zur Verschlüsselung ruhender Daten in einem BYOK-Szenario konfiguriert wird.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 01/17/2019
ms.openlocfilehash: fdf300d8aa288a80c88830e0a8d4cbe80acf0e28
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54389892"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>Verwalten von Transparent Data Encryption in einer verwalteten Instanz mithilfe eines eigenen Azure Key Vault-Schlüssels

In diesem PowerShell-Skriptbeispiel wird Transparent Data Encryption (TDE) in einem Bring Your Own Key-Szenario (BYOK) für eine verwaltete Azure SQL-Datenbank-Instanz unter Verwendung eines Schlüssels aus Azure Key Vault konfiguriert. Weitere Informationen zu TDE mit BYOK-Unterstützung finden Sie unter [TDE mit Bring Your Own Key-Unterstützung für Azure SQL](../transparent-data-encryption-byok-azure-sql.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.7.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

Wenn Sie PowerShell lokal installieren und verwenden möchten, wird für dieses Tutorial auch eine Vorschauversion des AzureRM.Sql-PowerShell-Pakets *4.11.6-preview* benötigt. Führen Sie den folgenden Befehl aus, um das Paket zu installieren: `Install-Module -Name AzureRM.Sql -RequiredVersion 4.11.6-preview -AllowPrerelease`.

## <a name="sample-scripts"></a>Beispielskripts

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche PowerShell-Skriptbeispiele für SQL-Datenbank finden Sie in den [Azure PowerShell samples for Azure SQL Database](../sql-database-powershell-samples.md) (Azure PowerShell-Beispiele für Azure SQL-Datenbank).
