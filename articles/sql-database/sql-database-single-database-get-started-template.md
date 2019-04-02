---
title: 'Azure Resource Manager: Erstellen einer Einzeldatenbank – Azure SQL-Datenbank | Microsoft-Dokumentation'
description: Erstellen Sie mithilfe der Azure Resource Manager-Vorlage eine Einzeldatenbank in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 03/22/2019
ms.openlocfilehash: 01e14f86b16db6d998d60e74211ae5ad77381461
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372945"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-resource-manager-template"></a>Schnellstart: Erstellen einer Einzeldatenbank in Azure SQL-Datenbank mithilfe der Azure Resource Manager-Vorlage

Die Erstellung einer [Einzeldatenbank](sql-database-single-database.md) ist die schnellste und einfachste Bereitstellungsoption zum Erstellen einer Datenbank in Azure SQL-Datenbank. In dieser Schnellstartanleitung wird gezeigt, wie Sie mit der Azure Resource Manager-Vorlage eine Einzeldatenbank erstellen. Weitere Informationen finden Sie in der [Dokumentation zu Azure Resource Manager](/azure/azure-resource-manager/).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).

## <a name="create-a-single-database"></a>Erstellen einer Einzeldatenbank

Eine Einzeldatenbank enthält einen definierten Satz von Compute-, Arbeitsspeicher-, EA- und Speicherressourcen und verwendet eins von zwei möglichen [Erwerbsmodellen](sql-database-purchase-models.md). Wenn Sie eine Einzeldatenbank erstellen, legen Sie auch einen [SQL-Datenbankserver](sql-database-servers.md) für ihre Verwaltung fest und platzieren ihn in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) in einer bestimmten Region.

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/201-sql-threat-detection-server-policy-optional-db/). Die folgende JSON-Datei ist die Vorlage, die in diesem Artikel verwendet wird. Weitere Vorlagenbeispiele für Azure SQL-Datenbank finden Sie [hier](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular).

[!code-json[create-azure-sql-database](~/quickstart-templates/201-sql-threat-detection-server-policy-optional-db/azuredeploy.json)]

1. Klicken Sie auf das folgende Bild, um sich bei Azure anzumelden und eine Vorlage zu öffnen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-sql-threat-detection-server-policy-optional-db%2Fazuredeploy.json"><img src="./media/sql-database-single-database-get-started-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Wählen Sie die folgenden Werte aus, bzw. geben Sie sie ein.  

    ![Resource Manager-Vorlage: Erstellen einer Azure SQL-Datenbank](./media/sql-database-single-database-get-started-template/create-azure-sql-database-resource-manager-template.png)

    Verwenden Sie die Standardwerte, sofern nichts anderes angegeben ist.

    * **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
    * **Ressourcengruppe**: Wählen Sie die Option **Neu erstellen** aus, geben Sie einen eindeutigen Namen für die Ressourcengruppe ein, und klicken Sie dann auf **OK**. 
    * **Standort**: Wählen Sie einen Standort aus.  Beispiel: **USA, Mitte**.
    * **Administratorbenutzer**: Geben Sie den Benutzernamen des Serveradministrators für SQL-Datenbank an.
    * **Administratorkennwort**: Geben Sie ein Administratorkennwort ein. 
    * **Ich stimme den oben genannten Geschäftsbedingungen zu**: Aktivieren Sie dieses Kontrollkästchen.
3. Wählen Sie die Option **Kaufen**.

## <a name="query-the-database"></a>Abfragen der Datenbank

Informationen zum Abfragen der Datenbank finden Sie unter [Abfragen der Datenbank](./sql-database-single-database-get-started.md#query-the-database).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Behalten Sie die Ressourcengruppe, den Datenbankserver und die Einzeldatenbank, wenn Sie mit [Nächste Schritte](#next-steps) fortfahren möchten. Die nächsten Schritte zeigen, wie Sie mithilfe verschiedener Methoden eine Verbindung mit Ihrer Datenbank herstellen und die Datenbank abfragen.

Löschen Sie die Ressourcengruppe wie folgt mit der Azure CLI oder mit Azure Powershell:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName 
```

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName 
```

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie eine Firewallregel auf Serverebene, um über lokale Tools oder Remotetools eine Verbindung mit der Einzeldatenbank herstellen zu können. Weitere Informationen finden Sie unter [Erstellen einer Firewallregel auf Serverebene](sql-database-server-level-firewall-rule.md).
- Nachdem Sie eine Firewallregel auf Serverebene erstellt haben, können Sie mit verschiedenen Tools und Programmiersprachen eine [Verbindung mit Ihrer Datenbank herstellen und Abfragen ausführen](sql-database-connect-query.md).
  - [Verbinden und Abfragen mit SQL Server Management Studio (SSMS)](sql-database-connect-query-ssms.md)
  - [Verbinden und Abfragen mit Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Informationen zum Erstellen von Einzeldatenbanken mit der Azure-Befehlszeilenschnittstelle finden Sie unter [Azure CLI-Beispiele für Azure SQL-Datenbank](sql-database-cli-samples.md).
- Informationen zum Erstellen von Einzeldatenbanken mit Azure PowerShell finden Sie unter [Azure PowerShell-Beispiele für Azure SQL-Datenbank](sql-database-powershell-samples.md).
