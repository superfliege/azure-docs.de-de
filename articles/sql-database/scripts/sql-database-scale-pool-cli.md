---
title: CLI-Beispielskript zum Skalieren eines Pools für elastische SQL-Datenbanken in Azure SQL-Datenbank | Microsoft-Dokumentation
description: Azure CLI-Beispielskript zum Skalieren eines Pools für elastische Datenbanken in Azure SQL-Datenbank
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: d492259a114d6574c4a81d1f9b97a0dcfcd8c5d3
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "56004042"
---
# <a name="use-cli-to-scale-an-elastic-pool-in-azure-sql-database"></a>Verwenden der Befehlszeilenschnittstelle zum Skalieren eines Pools für elastische Datenbanken in Azure SQL-Datenbank

Dieses Azure CLI-Skriptbeispiel erstellt Pools für elastische Datenbanken, verschiebt in Pools zusammengefasste Datenbanken und ändert die Computegrößen für Pools für elastische Datenbanken. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI in Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "Move database between pools")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um eine Ressourcengruppe, einen SQL-Datenbankserver, eine Einzeldatenbank und SQL-Datenbank-Firewallregeln zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#az-sql-server-create) | Erstellt einen SQL-Datenbankserver, der Einzeldatenbanken und Pools für elastische Datenbanken hostet. |
| [az sql elastic-pools create](https://docs.microsoft.com/cli/azure/sql/elastic-pool#az-sql-elastic-pool-create) | Erstellt einen Pool für elastische Datenbanken. |
| [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create) | Erstellt eine Einzel- oder Pooldatenbank. |
| [az sql elastic-pools update](https://docs.microsoft.com/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update) | Aktualisiert einen Pool für elastische Datenbanken. In diesem Beispiel wird die zugewiesene eDTU (Transaktionseinheit in elastischer Datenbank) geändert. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../sql-database-cli-samples.md).
