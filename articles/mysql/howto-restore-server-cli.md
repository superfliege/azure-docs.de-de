---
title: Sichern und Wiederherstellen eines Servers in Azure Database for MySQL
description: Erfahren Sie, wie Sie einen Server in Azure Database for MySQL mit der Azure-Befehlszeilenschnittstelle sichern und wiederherstellen.
author: rachel-msft
ms.author: raagyema
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 04/01/2018
ms.openlocfilehash: f3850623f5918ea9405131edb1821b941019ac34
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2019
ms.locfileid: "66160436"
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-cli"></a>Sichern und Wiederherstellen eines Servers in Azure Database for MySQL mit der Azure CLI

## <a name="backup-happens-automatically"></a>Automatische Sicherung
Azure Database for MySQL-Server werden regelmäßig gesichert, um Wiederherstellungsfunktionen zu ermöglichen. Mithilfe dieses Features können Sie für den Server und alle dazugehörigen Datenbanken einen Zustand zu einem früheren Zeitpunkt auf einem neuen Server wiederherstellen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- Einen [Azure Database for MySQL-Server und eine Datenbank](quickstart-create-mysql-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Diese Anleitung setzt die Verwendung von Azure CLI-Version 2.0 oder höher voraus. Geben Sie zum Bestätigen der Version an der Eingabeaufforderung von Azure CLI `az --version` ein. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

## <a name="set-backup-configuration"></a>Festlegen der Sicherungskonfiguration

Beim Konfigurieren des Servers treffen Sie die Entscheidung, ob dafür lokal redundante oder georedundante Sicherungen erstellt werden sollen. 

> [!NOTE]
> Nachdem ein Server erstellt wurde, kann die Redundanzart (georedundant oder lokal redundant) nicht mehr gewechselt werden.
>

Beim Erstellen eines Servers über den `az mysql server create`-Befehl legt der `--geo-redundant-backup`-Parameter Ihre Sicherungsredundanzoption fest. Bei `Enabled` werden georedundante Sicherungen vorgenommen. Bei `Disabled` werden lokal redundante Sicherungen vorgenommen. 

Der Aufbewahrungszeitraum für Sicherungen wird durch den Parameter `--backup-retention` festgelegt. 

Weitere Informationen zum Festlegen dieser Werte während der Erstellung finden Sie unter [Erstellen einer Azure-Datenbank für MySQL-Server mithilfe der Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md).

Der Aufbewahrungszeitraum für Sicherungen kann für einen Server folgendermaßen geändert werden:

```azurecli-interactive
az mysql server update --name mydemoserver --resource-group myresourcegroup --backup-retention 10
```

Das obige Beispiel ändert den Aufbewahrungszeitraum für Sicherungen von „mydemoserver“ in 10 Tage.

Mit „Aufbewahrungszeit für Sicherung“ wird auch gesteuert, für welchen zurückliegenden Zeitraum eine Point-in-Time-Wiederherstellung durchgeführt werden kann, da dies auf den verfügbaren Sicherungen basiert. Die Point-in-Time-Wiederherstellung wird im folgenden Abschnitt näher beschrieben.

## <a name="server-point-in-time-restore"></a>Point-in-Time-Wiederherstellung des Servers
Sie können den Zustand des Servers zu einem früheren Zeitpunkt wiederherstellen. Die wiederhergestellten Daten werden auf einen neuen Server kopiert, und der vorhandene Server bleibt unverändert. Wenn zum Beispiel zur Mittagszeit am heutigen Tag eine Tabelle versehentlich gelöscht wird, können Sie den Zustand kurz vor Mittag wiederherstellen. Dann können Sie die fehlende Tabelle und die fehlenden Daten aus der wiederhergestellten Kopie des Servers abrufen. 

Verwenden Sie zur Wiederherstellung des Servers den Azure CLI-Befehl [az mysql server restore](/cli/azure/mysql/server#az-mysql-server-restore).

### <a name="run-the-restore-command"></a>Ausführen des Befehls „restore“

Geben Sie zum Wiederherstellen des Servers an der Azure CLI-Eingabeaufforderung den folgenden Befehl ein:

```azurecli-interactive
az mysql server restore --resource-group myresourcegroup --name mydemoserver-restored --restore-point-in-time 2018-03-13T13:59:00Z --source-server mydemoserver
```

Für den Befehl `az mysql server restore` sind folgende Parameter erforderlich:

| Einstellung | Empfohlener Wert | BESCHREIBUNG  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Die Ressourcengruppe, in der sich der Quellserver befindet.  |
| name | mydemoserver-restored | Der Name des neuen Servers, der durch den Befehl „restore“ erstellt wird. |
| restore-point-in-time | 2018-03-13T13:59:00Z | Wählen Sie einen Zeitpunkt aus, dessen Zustand wiederhergestellt werden soll. Datum und Zeit müssen innerhalb des Aufbewahrungszeitraums für Sicherungen des Quellservers liegen. Verwenden Sie das Datums- und Zeitformat nach ISO 8601. Beispielsweise können Sie Ihre eigene lokale Zeitzone wie `2018-03-13T05:59:00-08:00` verwenden. Sie können z.B. auch das UTC-Zulu-Format verwenden, `2018-03-13T13:59:00Z`. |
| source-server | mydemoserver | Der Name oder die ID des Quellservers, über den die Wiederherstellung durchgeführt wird. |

Wenn Sie den Zustand eines Servers zu einem früheren Zeitpunkt wiederherstellen, wird ein neuer Server erstellt. Der ursprüngliche Server und seine Datenbanken aus dem angegebenen Zeitpunkt werden auf den neuen Server kopiert.

Die Werte zum Standort und Tarif des wiederhergestellten Servers bleiben mit denen des ursprünglichen Servers identisch. 

Suchen Sie nach Abschluss der Wiederherstellung den neuen Server, um zu überprüfen, ob die Daten wie erwartet wiederhergestellt wurden.

## <a name="geo-restore"></a>Geowiederherstellung
Wenn Sie Ihren Server für georedundante Sicherungen konfiguriert haben, kann aus der Sicherung dieses vorhandenen Servers ein neuer Server erstellt werden. Dieser neue Server kann in allen Regionen erstellt werden, in denen Azure Database for MySQL verfügbar ist.  

Um einen Server mithilfe einer georedundanten Sicherung zu erstellen, verwenden Sie den Azure CLI-Befehl `az mysql server georestore`.

> [!NOTE]
> Für einen neu erstellten Server kann möglicherweise nicht sofort eine Geowiederherstellung durchgeführt werden. Es kann einige Stunden dauern, bis die erforderlichen Metadaten aufgefüllt wurden.
>

Geben Sie zur Geowiederherstellung des Servers an der Azure CLI-Eingabeaufforderung den folgenden Befehl ein:

```azurecli-interactive
az mysql server georestore --resource-group myresourcegroup --name mydemoserver-georestored --source-server mydemoserver --location eastus --sku-name GP_Gen5_8 
```
Dieser Befehl erstellt einen neuen Server namens *mydemoserver-georestored* in „USA, Osten“, der zu *myresourcegroup* gehört. Es ist ein Gen 5-Server vom Typ „Universell“ mit acht virtuellen Kernen. Der Server wird aus der georedundanten Sicherung von *mydemoserver* erstellt, die auch in der Ressourcengruppe *myresourcegroup* vorhanden ist.

Wenn Sie den neuen Server in einer anderen Ressourcengruppe als der, in der sich der vorhandene Server befindet, erstellen möchten, bestimmen Sie im `--source-server`-Parameter den Servernamen wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az mysql server georestore --resource-group newresourcegroup --name mydemoserver-georestored --source-server "/subscriptions/$<subscription ID>/resourceGroups/$<resource group ID>/providers/Microsoft.DBforMySQL/servers/mydemoserver" --location eastus --sku-name GP_Gen5_8

```

Für den Befehl `az mysql server georestore` sind folgende Parameter erforderlich:

| Einstellung | Empfohlener Wert | BESCHREIBUNG  |
| --- | --- | --- |
|resource-group| myresourcegroup | Der Name der Ressourcengruppe, zu der der neue Server gehören soll.|
|name | mydemoserver-georestored | Der Name des neuen Servers. |
|source-server | mydemoserver | Der Name des vorhandenen Servers, dessen georedundanten Sicherungen verwendet werden. |
|location | eastus | Der Speicherort des neuen Servers. |
|sku-name| GP_Gen5_8 | Dieser Parameter legt Tarif, Computegeneration und Anzahl der virtuellen Kerne des neuen Servers fest. GP_Gen5_8 wird einem Gen 5-Server vom Typ „Allgemein“ mit acht virtuellen Kernen zugeordnet.|


>[!Important]
>Beim Erstellen von einer Geowiederherstellung erbt ein neuer Server Speichergröße und Tarif vom Quellserver. Diese Werte können während der Erstellung nicht geändert werden. Sobald der neue Server erstellt ist, kann die Speichergröße zentral hochskaliert werden.

Suchen Sie nach Abschluss der Wiederherstellung den neuen Server, um zu überprüfen, ob die Daten wie erwartet wiederhergestellt wurden.

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich über die [Sicherungen](concepts-backup.md) des Diensts.
- Informieren Sie sich über die Optionen in Bezug auf die [Geschäftskontinuität](concepts-business-continuity.md).
