---
title: Abrufen der Verbindungszeichenfolge aus dem Azure-Portal
description: Abrufen der Verbindungszeichenfolge aus dem Azure-Portal
keywords: SQL-Verbindung,Verbindungszeichenfolge
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: include
ms.date: 07/13/2018
ms.author: ninarn
ms.openlocfilehash: 6ead2e0ea326b5c3f2e76e7aa9cc4ab3c50d4154
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66164282"
---
### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Abrufen der Verbindungszeichenfolge aus dem Azure-Portal
Nutzen Sie das [Azure-Portal](https://portal.azure.com/) zum Abrufen der Verbindungszeichenfolge, die für die Interaktion des Clientprogramms mit Azure SQL-Datenbank benötigt wird.

1. Wählen Sie **Alle Dienste** > **SQL-Datenbanken** aus.

2. Geben Sie in das Textfeld „Filter“ nahe der oberen linken Ecke des Blattes **SQL-Datenbanken** den Namen der Datenbank ein.

3. Wählen Sie die Zeile für die Datenbank aus.

4. Nachdem das Blatt für die Datenbank angezeigt wird, können Sie der visuellen Einfachheit halber die Schaltflächen zum **Minimieren** auswählen, um die Blätter auszublenden, die Sie zum Durchsuchen und Filtern verwendet haben.

5. Wählen Sie auf dem Blatt für die Datenbank **Datenbankverbindungszeichenfolgen anzeigen** aus.

6. Kopieren Sie die entsprechende Verbindungszeichenfolge. Wenn Sie also die ADO.NET-Verbindungsbibliothek verwenden möchten, kopieren Sie die entsprechende Zeichenfolge von der Registerkarte **ADO.NET**.

    ![Kopieren der ADO-Verbindungszeichenfolge für die Datenbank][20-CopyAdoConnectionString]

7. Bearbeiten Sie Verbindungszeichenfolge nach Bedarf. Fügen Sie Ihr Kennwort in die Verbindungszeichenfolge ein, oder entfernen Sie „@&lt;Servername&gt;“ aus dem Benutzernamen, wenn der Benutzer- oder Servername zu lang ist.

8. Fügen Sie in dem ein oder anderen Format die Informationen der Verbindungszeichenfolge in den Clientcode für die Anwendung ein.

Weitere Informationen finden Sie unter [Verbindungszeichenfolgen und Konfigurationsdateien](https://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->



[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
