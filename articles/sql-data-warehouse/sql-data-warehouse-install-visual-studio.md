---
title: Installieren von Visual Studio und SSDT für SQL Data Warehouse | Microsoft-Dokumentation
description: Installieren von Visual Studio und SQL Server Data Tools (SSDT) für Azure SQL Data Warehouse
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: e27a3d4f2fe4b8421b637e485d77d6376d15bf9a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55461490"
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Installieren von Visual Studio und SSDT für SQL Data Warehouse
Für die Entwicklung von Anwendungen für SQL Data Warehouse empfiehlt es sich, die neueste Version von Visual Studio in Verbindung mit der neuesten Version von SQL Server Data Tools (SSDT) zu verwenden.  Visual Studio 2013 Update 5 mit SSDT wird auch für die Abwärtskompatibilität unterstützt.  

Die Verwendung von Visual Studio mit SSDT ermöglicht die Nutzung des SQL Server-Objekt-Explorers zum Visualisieren von Tabellen, Ansichten, gespeicherten Prozeduren und zahlreichen weiteren Objekten in SQL Data Warehouse sowie zum Ausführen von Abfragen.

> [!NOTE]
> SQL Data Warehouse unterstützt noch keine Visual Studio-Datenbankprojekte. Um regelmäßig Aktualisierungen zu dieser Funktion zu erhalten, stimmen Sie auf [UserVoice] ab.
> 
> 

## <a name="step-1-install-visual-studio"></a>Schritt 1: Installieren von Visual Studio
Verwenden Sie die folgenden Links, um Visual Studio herunterzuladen und zu installieren. Falls Sie bereits Visual Studio 2013 oder eine höhere Version installiert haben, fahren Sie mit Schritt 2 fort, um SSDT zu installieren.

1. [Laden Sie Visual Studio herunter][].
2. Führen Sie die Schritte der auf MSDN bereitgestellten Anleitung [Installieren von Visual Studio][Installing Visual Studio] aus, und wählen Sie die Standardkonfigurationen aus.

## <a name="step-2-install-ssdt"></a>Schritt 2: Installieren von SSDT
Führen Sie zum Installieren von SSDT für Visual Studio zuerst die folgenden Schritte aus, um in Visual Studio nach einem SSDT-Update zu suchen:

1. Klicken Sie in Visual Studio auf **Tools** / **Erweiterungen und Updates...**  / **Updates**
2. Wählen Sie **Produktupdates** aus, und suchen Sie nach **Microsoft SQL Server-Update für Datenbanktools**.

Wird kein Update gefunden, ist bereits die neueste Version installiert.  Klicken Sie auf **Hilfe** / **Info**, und suchen Sie in der Liste nach „SQL Server Data Tools“, um sich zu vergewissern, dass SSDT installiert ist. Die neueste Version von SSDT ist 14.0.60525.0. Falls die Option zur Installation in Visual Studio nicht verfügbar ist, können Sie SSDT auf der Seite für den [SSDT-Download][SSDT Download] manuell herunterladen und installieren.

## <a name="next-steps"></a>Nächste Schritte
Da Sie jetzt die neueste Version von SSDT verwenden, sind Sie bereit, eine [Verbindung][connect] mit SQL Data Warehouse herzustellen.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Laden Sie Visual Studio herunter]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
