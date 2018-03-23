---
title: Azure App Service – Synchronisieren der Netzwerkkonfiguration | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie die Netzwerkkonfiguration für den Azure App Service-Hostingplan synchronisieren.
ms.service: sql-database
author: srdjan-bozovic
manager: cguyer
ms.custom: managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 1968f736dbfc24495e4e932f9c8c5955dc607133
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
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
