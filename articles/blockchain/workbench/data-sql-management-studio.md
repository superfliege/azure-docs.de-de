---
title: Verwenden von Azure Blockchain Workbench-Daten mit SQL Server Management Studio
description: Informationen Sie zum Herstellen einer Verbindung mit der SQL-Datenbank von Azure Blockchain Workbench aus SQL Server Management Studio heraus.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/14/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: a5554f1fe0a14799a100aefc975a110d3a9a6311
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58088722"
---
# <a name="using-azure-blockchain-workbench-data-with-sql-server-management-studio"></a>Verwenden von Azure Blockchain Workbench-Daten mit SQL Server Management Studio

Microsoft SQL Server Management Studio bietet die Möglichkeit zum schnellen Schreiben und Testen von Abfragen für die Azure Blockchain Workbench Azure-SQL-Datenbank. In diesem Abschnitt finden Sie eine schrittweise exemplarische Vorgehensweise zum Herstellen einer Verbindung mit der SQL-Datenbank von Azure Blockchain Workbench aus SQL Server Management Studio heraus.

## <a name="prerequisites"></a>Voraussetzungen

* Laden Sie [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) herunter.

## <a name="connecting-sql-server-management-studio-to-data-in-azure-blockchain-workbench"></a>Verbinden von SQL Server Management Studio mit Daten in Azure Blockchain Workbench

1. Öffnen Sie SQL Server Management Studio, und wählen Sie **Verbinden** aus.
2. Wählen Sie **Datenbank-Engine** aus.

    ![Datenbank-Engine](./media/data-sql-management-studio/database-engine.png)

3. Geben Sie im Dialogfeld **Mit Server verbinden** den Servernamen und die Anmeldeinformationen für Ihre Datenbank ein.

    Wenn Sie die beim Azure Blockchain Workbench-Bereitstellungsprozess erstellten Anmeldeinformationen verwenden, lautet der Benutzername **dbadmin**, und Sie müssen das Kennwort angeben, das Sie während der Bereitstellung festgelegt haben.

    ![Eingeben der SQL-Anmeldeinformationen](./media/data-sql-management-studio/sql-creds.png)

   1. SQL Server Management Studio zeigt die Liste der Datenbanken, Datenbanksichten und gespeicherten Prozeduren in der Azure Blockchain Workbench-Datenbank an.

      ![Datenbankliste](./media/data-sql-management-studio/db-list.png)

5. Um die den Datenbanksichten zugeordneten Daten anzuzeigen, können Sie mithilfe der folgenden Schritte automatisch eine SELECT-Anweisung generieren.
6. Klicken Sie mit der rechten Maustaste auf eine der Datenbanksichten im Objekt-Explorer.
7. Klicken Sie auf **Skript für Sicht als**.
8. Wählen Sie **SELECT nach** aus.
9. Wählen Sie **Neues Abfrage-Editor-Fenster** aus.
10. Sie können eine neue Abfrage erstellen, indem Sie **Neue Abfrage** auswählen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Datenbanksichten in Azure Blockchain Workbench](database-views.md)
