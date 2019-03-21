---
title: So deinstallieren Sie das Tool für elastische Datenbankaufträge
description: Erfahren Sie, wie Sie Komponenten der Aufträge für die elastische Datenbank mithilfe des Azure-Portals oder PowerShell deinstallieren.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 12f923724616378a6ea6c83a947bd5362840a697
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57768863"
---
# <a name="uninstall-elastic-database-jobs-components"></a>Deinstallieren von Komponenten der Aufträge für die elastische Datenbank


[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]


**Aufträge für die elastische Datenbank** können wahlweise mithilfe des Azure-Portals oder von PowerShell deinstalliert werden.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Deinstallieren von Komponenten der Aufträge für die elastische Datenbank mithilfe des Azure-Portals
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/).
2. Navigieren Sie zu dem Abonnement, das die Komponenten der **Aufträge für die elastische Datenbank** enthält, also dem Abonnement, in dem die Komponenten der Aufträge für die elastische Datenbank installiert wurden.
3. Klicken Sie auf **Durchsuchen** und dann auf **Ressourcengruppen**.
4. Wählen Sie die Ressourcengruppe mit dem Namen "__ElasticDatabaseJob".
5. Löschen Sie die Ressourcengruppe.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Deinstallieren von Komponenten der Aufträge für die elastische Datenbank mithilfe von PowerShell
1. Öffnen Sie ein Microsoft Azure PowerShell-Befehlsfenster, und navigieren Sie zum Unterverzeichnis „tools“ im Ordner „Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x“: Geben Sie **cd tools** ein.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools
2. Führen Sie das PowerShell-Skript „.\UninstallElasticDatabaseJobs.ps1“ aus.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1

Oder führen Sie alternativ einfach das folgende Skript aus, unter der Voraussetzung, dass bei der Installation der Komponenten die Standardwerte übernommen wurden:

```powershell
$ResourceGroupName = "__ElasticDatabaseJob"
Switch-AzureMode AzureResourceManager

$resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
if(!$resourceGroup)
{
     Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
     return
}

Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
Remove-AzureResourceGroup -Name $ResourceGroupName -Force
Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job components are now uninstalled."
```

## <a name="next-steps"></a>Nächste Schritte
Informationen zur erneuten Installation von Aufträgen für die elastische Datenbank finden Sie unter [Installieren des Diensts für Aufträge für die elastische Datenbank](sql-database-elastic-jobs-service-installation.md)

Weitere Informationen zu Aufträgen für die elastische Datenbank finden Sie unter [Übersicht über Aufträge für die elastische Datenbank](sql-database-elastic-jobs-overview.md).
