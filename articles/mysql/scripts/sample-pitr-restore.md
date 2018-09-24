---
title: Azure CLI-Skript – Wiederherstellen eines Azure Database for MySQL-Servers auf den Zustand zu einem früheren Zeitpunkt
description: Mit diesem CLI-Beispielskript wird für einen Azure Database for MySQL-Server der Zustand zu einem früheren Zeitpunkt wiederhergestellt.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: 83ff7b0a5f4abd6b8a1cddb80d5cf130df4d19f0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986749"
---
# <a name="restore-an-azure-database-for-mysql-server-using-azure-cli"></a>Wiederherstellen eines Azure Database for MySQL-Servers mithilfe der Azure-Befehlszeilenschnittstelle
Dieses CLI-Beispielskript stellt einen einzelnen Azure Database for MySQL-Server auf den Zustand auf einen früheren Zeitpunkt wieder her.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Wenn Sie es bevorzugen, die Befehlszeilenschnittstelle lokal zu installieren und zu verwenden, müssen Sie für dieses Beispiel die Azure-Befehlszeilenschnittstelle in Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli). 

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
