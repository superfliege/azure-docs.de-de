---
title: Azure App Service – Synchronisieren der Netzwerkkonfiguration | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie die Netzwerkkonfiguration für den Azure App Service-Hostingplan synchronisieren.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 03/07/2018
ms.openlocfilehash: d5de908166e8de1d45a36f97aee8934653e59623
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163164"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Synchronisieren der Netzwerkkonfiguration für den Azure App Service-Hostingplan

Möglicherweise können Sie trotz [Integrieren Ihrer App in ein Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md) keine Verbindung mit der verwalteten Instanz herstellen. Sie können dann versuchen, die Netzwerkkonfiguration für Ihren Dienstplan zu aktualisieren. 

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>Synchronisieren der Netzwerkkonfiguration für den App Service-Hostingplan

Gehen Sie dazu folgendermaßen vor:  

1. Navigieren Sie zum App Service-Plan für Ihre Web-Apps.
 
   ![App Service-Plan](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Klicken Sie auf **Netzwerk** und dann auf **Klicken Sie zur Verwaltung hier**.
 
   ![Verwalten des Dienstplans](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Wählen Sie Ihr **VNET** aus, und klicken Sie auf **Netzwerk synchronisieren**. 
 
   ![Netzwerk synchronisieren](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Warten Sie, bis die Synchronisierung abgeschlossen ist.
  
   ![Synchronisierung abgeschlossen](./media/sql-database-managed-instance-sync-networking/sync-done.png)

Nun können Sie versuchen, die Verbindung mit der verwalteten Instanz erneut herzustellen.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Konfigurieren Ihres VNET für die verwaltete Instanz finden Sie unter [Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-vnet-configuration.md).
