---
title: "Azure CLI-Skript – Wiederherstellen eines Azure Database for MySQL-Servers auf den Zustand zu einem früheren Zeitpunkt"
description: "Mit diesem CLI-Beispielskript wird für einen Azure Database for MySQL-Server der Zustand zu einem früheren Zeitpunkt wiederhergestellt."
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: 090447e21a1428d1c0ebd526ec2f48d616b39b82
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="restore-an-azure-database-for-mysql-server-using-azure-cli"></a>Wiederherstellen eines Azure Database for MySQL-Servers mithilfe der Azure-Befehlszeilenschnittstelle
Dieses CLI-Beispielskript stellt einen einzelnen Azure Database for MySQL-Server auf den Zustand auf einen früheren Zeitpunkt wieder her.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Wenn Sie es bevorzugen, die Befehlszeilenschnittstelle lokal zu installieren und zu verwenden, müssen Sie für dieses Beispiel die Azure-Befehlszeilenschnittstelle in Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript
Ändern Sie in diesem Beispielskript die hervorgehobenen Zeilen, um den Benutzernamen und das Kennwort des Administrators anzupassen. Ersetzen Sie die Abonnement-ID, die in den az monitor-Befehlen verwendet wird, durch Ihre eigene Abonnement-ID.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/backup-restore-pitr/backup-restore.sh?highlight=18-19 "Restore Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/backup-restore-pitr/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Erläuterung des Skripts
Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| **Befehl** | **Hinweise** |
|---|---|
| [az group create](/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az mysql server create](/cli/azure/mysql/server#create) | Erstellt einen MySQL-Server, der die Datenbanken hostet. |
| [az mysql server restore](/cli/azure/mysql/server#restore) | Stellt einen Server aus einer Sicherung wieder her |
| [az group delete](/cli/azure/group#delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie weitere Informationen zur Azure CLI: [Azure CLI-Dokumentation](/cli/azure).
- Probieren Sie weitere Skripts aus: [Azure CLI-Beispiele für Azure-Datenbank für MySQL](../sample-scripts-azure-cli.md)
