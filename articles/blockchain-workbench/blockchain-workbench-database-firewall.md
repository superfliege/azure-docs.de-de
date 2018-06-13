---
title: Konfigurieren der Firewall der SQL-Datenbank von Azure Blockchain Workbench
description: Erfahren Sie, wie die Firewall der SQL-Datenbank von Azure Blockchain Workbench konfiguriert wird.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/4/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: afeea143f73fa4f7d3e373535007846a668616ab
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34074948"
---
# <a name="configure-the-azure-blockchain-workbench-database-firewall"></a>Konfigurieren der Firewall der Datenbank von Azure Blockchain Workbench

In diesem wird Artikel gezeigt, wie eine Firewallregel mit dem Azure-Portal konfiguriert wird. Firewallregeln ermöglichen, dass externe Clients oder Clientanwendungen eine Verbindung mit Ihrer Azure Blockchain Workbench-Datenbank herstellen können.

## <a name="connect-to-the-blockchain-workbench-database"></a>Herstellen einer Verbindung mit der Blockchain Workbench-Datenbank

So stellen Sie die Verbindung mit der Datenbank her, für die Sie eine Regel konfigurieren möchten:

1. Melden Sie sich am Azure-Portal mit einem Konto an, das über die Berechtigungen **Besitzer** für die Azure Blockchain Workbench-Ressourcen verfügt.
2. Wählen Sie im linken Navigationsbereich die Option **Ressourcengruppen** aus.
3. Wählen Sie den Namen der Ressourcengruppe für die Blockchain Workbench-Bereitstellung aus.
4. Wählen Sie **Typ** aus, um die Liste der Ressourcen zu sortieren, und wählen Sie dann Ihren **SQL-Server** aus.
5. Das Beispiel für die Ressourcenliste im folgenden Screenshot zeigt zwei Datenbanken: *master* und *lsgn-sdk*. Sie konfigurieren die Firewallregel für *lsgn-sdk*.

![Auflisten der Blockchain Workbench-Ressourcen](media/blockchain-workbench-database-firewall/list-database-resources.png)

## <a name="create-a-database-firewall-rule"></a>Erstellen einer Datenbankfirewallregel

So erstellen Sie eine Firewallregel:

1. Wählen Sie den Link zur Datenbank „lsgn-sdk“ aus.
2. Wählen Sie auf der Menüleiste **Serverfirewall festlegen** aus.

   ![Festlegen der Serverfirewall](media/blockchain-workbench-database-firewall/configure-server-firewall.png)

3. So erstellen Sie eine Regel für Ihre Organisation:

   * Geben Sie einen **REGELNAMEN** ein.
   * Geben Sie eine IP-Adresse für die **START-IP** des Adressbereichs ein.
   * Geben Sie eine IP-Adresse für die **END-IP** des Adressbereichs ein.

   ![Erstellen einer Firewallregel](media/blockchain-workbench-database-firewall/create-firewall-rule.png)

    > [!NOTE]
    > Wenn Sie nur die IP-Adresse Ihres Computers hinzufügen möchten, wählen Sie **+ Client-IP hinzufügen** aus.
        
1. Wählen Sie zum Speichern der Firewallkonfiguration **Speichern** aus.
2. Testen Sie den IP-Adressbereich, den Sie für die Datenbank konfiguriert haben, durch Herstellen einer Verbindung von einer Anwendung oder einem Tool aus. Beispielsweise aus SQL Server Management Studio.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Datenbanksichten in Azure Blockchain Workbench](blockchain-workbench-database-views.md)