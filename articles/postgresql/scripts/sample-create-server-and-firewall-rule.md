---
title: "Azure CLI-Skript: Erstellen einer Azure-Datenbank für PostgreSQL-Instanz | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Erstellt einen Azure-Datenbank für PostgreSQL-Server und konfiguriert eine Firewallregel auf Serverebene."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: e545b568cd57fdcf28ab33a5ebfa34a495111c7f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>Erstellen eines Azure-Datenbank für PostgreSQL-Servers und Konfigurieren einer Firewallregel mit der Azure CLI
Dieses Azure CLI-Beispielskript erstellt einen Azure-Datenbank für PostgreSQL-Server und konfiguriert eine Firewallregel auf Serverebene. Nach erfolgreicher Ausführung des Skripts können alle Azure-Dienste und die konfigurierte IP-Adresse auf den PostgreSQL-Server zugreifen.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript
Bearbeiten Sie in diesem Beispielskript die hervorgehobenen Zeilen, um den Benutzernamen und das Kennwort des Administrators anzupassen.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Erläuterung des Skripts
Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| **Befehl** | **Hinweise** |
|---|---|
| [az group create](/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az postgres server create](/cli/azure/postgres/server#create) | Erstellt einen PostgreSQL-Server, der die Datenbanken hostet. |
| [az postgres server firewall create](/cli/azure/postgres/server/firewall-rule#create) | Erstellt eine Firewallregel, die vom eingegebenen IP-Adressbereich aus den Zugriff auf alle Server und darauf befindlichen Datenbanken ermöglicht. |
| [az group delete](/cli/azure/group#delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie weitere Informationen zur Azure CLI: [Azure CLI-Dokumentation](/cli/azure/overview)
- Probieren Sie weitere Skripts aus: [Azure CLI-Beispiele für Azure-Datenbank für PostgreSQL](../sample-scripts-azure-cli.md)
