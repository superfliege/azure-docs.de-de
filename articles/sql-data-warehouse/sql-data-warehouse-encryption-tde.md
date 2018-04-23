---
title: Transparent Data Encryption in SQL Data Warehouse (Portal) | Microsoft Docs
description: Transparent Data Encryption (TDE) in SQL Data Warehouse
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 9c4abb0416acc656a4cfae332377c398260191de
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Erste Schritte mit Transparent Data Encryption (TDE) in SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Sicherheitsübersicht](sql-data-warehouse-overview-manage-security.md)
> * [Authentifizierung](sql-data-warehouse-authentication.md)
> * [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Erforderliche Berechtigungen
Sie müssen Administrator oder ein Mitglied der Rolle „dbmanager“ sein, um Transparent Data Encryption (TDE) zu aktivieren.

## <a name="enabling-encryption"></a>Aktivieren der Verschlüsselung
Führen Sie diese Schritte aus, um TDE für ein SQL Data Warehouse zu aktivieren:

1. Öffnen Sie die Datenbank im [Azure-Portal](https://portal.azure.com)
2. Klicken Sie im Datenbank-Blatt auf die Schaltfläche **Einstellungen** .
3. Wählen Sie die Option **Transparten Data Encryption** aus. ![][1]
4. Wählen Sie die Einstellung **Ein**. ![][2]
5. Wählen Sie **Speichern** aus. 
   ![][3]  

## <a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung
Führen Sie diese Schritte aus, um TDE für ein SQL Data Warehouse zu deaktivieren:

1. Öffnen Sie die Datenbank im [Azure-Portal](https://portal.azure.com)
2. Klicken Sie im Datenbank-Blatt auf die Schaltfläche **Einstellungen** .
3. Wählen Sie die Option **Transparten Data Encryption** aus. ![][1]
4. Wählen Sie die Einstellung **Aus**. ![][4]
5. Wählen Sie **Speichern** aus. 
   ![][5]  

## <a name="encryption-dmvs"></a>Verschlüsselung-DMVs
Die Verschlüsselung kann mit folgenden DMVs überprüft werden:

* [sys.databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
