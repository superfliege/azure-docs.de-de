---
title: Abrufen von Azure Blockchain Workbench-Datenbankinformationen
description: Erfahren Sie, wie Sie Informationen zu Datenbank und Datenbankserver von Azure Blockchain Workbench abrufen.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/14/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 0d806491c58c4b1881adc2fd830de7c7b9f0859d
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2019
ms.locfileid: "54331927"
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Abrufen von Informationen zu Ihrer Azure Blockchain Workbench-Datenbank

In diesem Artikel wird erläutert, wie Sie ausführliche Informationen zu Ihrer Azure Blockchain Workbench-Datenbank abrufen.

## <a name="overview"></a>Übersicht

Informationen zu Anwendungen, Workflows und intelligenter Vertragsausführung werden unter Verwendung von Datenbanksichten in der Blockchain Workbench-SQL-Datenbank bereitgestellt. Entwickler können diese Informationen bei Verwendung von Tools wie Microsoft Excel, Power BI, Visual Studio und SQL Server Management Studio nutzen.

Bevor Entwickler eine Verbindung mit der Datenbank herstellen können, ist Folgendes erforderlich:

* In der Datenbankfirewall wird der externe Clientzugriff zugelassen. In diesem Artikel zum Konfigurieren eines Datenbankfirewallartikels wird erläutert, wie der Zugriff zugelassen wird.
* Datenbankservername und Datenbankname.

## <a name="connect-to-the-blockchain-workbench-database"></a>Herstellen einer Verbindung mit der Blockchain Workbench-Datenbank

So stellen Sie die Verbindung mit der Datenbank her:

1. Melden Sie sich beim Azure-Portal mit einem Konto an, das über die Berechtigungen **Besitzer** für die Azure Blockchain Workbench-Ressourcen verfügt.
2. Wählen Sie im linken Navigationsbereich die Option **Ressourcengruppen** aus.
3. Wählen Sie den Namen der Ressourcengruppe für die Blockchain Workbench-Bereitstellung aus.
4. Wählen Sie **Typ** aus, um die Ressourcenliste zu sortieren, und wählen Sie dann Ihren **SQL-Server** aus. Die sortierte Liste im nächsten Bildschirm zeigt zwei SQL-Datenbanken (eine "master"-Datenbank und eine weitere mit dem **Ressourcenpräfix** "lhgn").

   ![Sortierte Blockchain Workbench-Ressourcenliste](./media/getdb-details/sorted-workbench-resource-list.png)

5. Um ausführliche Informationen zur Blockchain Workbench-Datenbank anzuzeigen, wählen Sie den Link für die Datenbank mit dem für die Bereitstellung von Blockchain Workbench bereitgestellten **Ressourcenpräfix** aus.

   ![Datenbankdetails](./media/getdb-details/workbench-db-details.png)

Unter Verwendung des Datenbankservernamens und Datenbanknamens können Sie mit Ihrem Entwicklungs- oder Berichtstool die Verbindung zur Blockchain Workbench-Datenbank herstellen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Datenbanksichten in Azure Blockchain Workbench](database-views.md)