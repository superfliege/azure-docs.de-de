---
title: Azure CLI-Skript – Erstellen einer Azure Database for MySQL-Instanz
description: Dieses CLI-Beispielskript erstellt einen einzelnen Azure-Datenbank für MySQL-Server und konfiguriert eine Firewallregel auf Serverebene.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 02/28/2018
ms.openlocfilehash: 2cf21abcee43f74e858dd1381baf43d65ac4c185
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971687"
---
# <a name="create-a-mysql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>Erstellen eines MySQL-Servers und Konfigurieren einer Firewallregel mit der Azure CLI
Dieses CLI-Beispielskript erstellt einen einzelnen Azure-Datenbank für MySQL-Server und konfiguriert eine Firewallregel auf Serverebene. Nach der erfolgreichen Ausführung des Skripts ist der MySQL-Server für alle Azure-Dienste und die konfigurierte IP-Adresse erreichbar.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittelle lokal ausführen, ist für diesen Artikel Azure CLI Version 2.0 oder höher erforderlich. Überprüfen Sie die Version, indem Sie `az --version` ausführen. Informationen zum Installieren oder Aktualisieren Ihrer Version der Azure CLI finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Beispielskript
Bearbeiten Sie in diesem Beispielskript die hervorgehobenen Zeilen, um den Benutzernamen und das Kennwort des Administrators zu aktualisieren und auf Ihre eigenen Daten festzulegen.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=18-19 "Create an Azure Database for MySQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle damit verbundenen Ressourcen zu entfernen, nachdem das Skript ausgeführt wurde. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Erläuterung des Skripts
In diesem Skript werden die Befehle verwendet, die in der folgenden Tabelle aufgeführt sind:

| **Befehl** | **Hinweise** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az mysql server create](/cli/azure/mysql/server#az-msql-server-create) | Erstellt einen MySQL-Server, der die Datenbanken hostet. |
| [az mysql server firewall create](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-create) | Erstellt eine Firewallregel, die vom eingegebenen IP-Adressbereich aus den Zugriff auf alle Server und darauf befindlichen Datenbanken ermöglicht. |
| [az group delete](/cli/azure/group#az-group-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie weitere Informationen zur Azure CLI: [Azure CLI-Dokumentation](/cli/azure).
- Probieren Sie weitere Skripts aus: [Azure CLI-Beispiele für Azure-Datenbank für MySQL](../sample-scripts-azure-cli.md)
