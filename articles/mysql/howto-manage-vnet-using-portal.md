---
title: Erstellen und Verwalten von VNET-Dienstendpunkten und -Regeln für Azure Database for MySQL mithilfe des Azure-Portals | Microsoft-Dokumentation
description: Erstellen und Verwalten von VNET-Dienstendpunkten und -Regeln für Azure Database for MySQL mithilfe des Microsoft Azure-Portals
author: mbolz
ms.author: mbolz
ms.service: mysql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 9aac664ae78fa0995f39677a362f1b23b9c4af35
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53535300"
---
# <a name="create-and-manage-azure-database-for-mysql-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Erstellen und Verwalten von VNET-Dienstendpunkten und VNET-Regeln für Azure Database for MySQL mithilfe des Microsoft Azure-Portals
Über Dienstendpunkte und Regeln für ein virtuelles Netzwerk (VNET) wird der private Adressraum eines virtuellen Netzwerks auf Ihren Azure Database for MySQL-Server ausgeweitet. Einen Überblick über VNET-Dienstendpunkte für Azure Database for MySQL – einschließlich der Einschränkungen – finden Sie unter [VNET-Dienstendpunkte für Azure Database for MySQL Server](concepts-data-access-and-security-vnet.md). VNET-Dienstendpunkte sind in allen unterstützten Regionen für Azure Database for MySQL verfügbar.

> [!NOTE]
> VNET-Dienstendpunkte werden nur für Server vom Typ „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.

## <a name="create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal"></a>Erstellen einer VNET-Regel und Aktivieren von Dienstendpunkten im Azure-Portal

1. Klicken Sie auf der Seite des MySQL-Servers unter der Überschrift „Einstellungen“ auf **Verbindungssicherheit**, um den Bereich „Verbindungssicherheit“ für Azure Database for MySQL zu öffnen. Klicken Sie dann auf **+ Vorhandenes virtuelles Netzwerk wird hinzugefügt**. Wenn kein VNET vorhanden ist, können Sie auf **+ Neues virtuelles Netzwerk erstellen** klicken, um eines zu erstellen. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines virtuellen Netzwerks im Azure-Portal](../virtual-network/quick-create-portal.md)

   ![Azure-Portal – Klicken auf „Verbindungssicherheit“](./media/howto-manage-vnet-using-portal/1-connection-security.png)

2. Geben Sie einen VNET-Regelnamen ein, wählen Sie das Abonnement, das virtuelle Netzwerk sowie den Subnetznamen aus, und klicken Sie dann auf **Aktivieren**. Dadurch werden automatisch VNET-Dienstendpunkte im Subnetz mithilfe des **Microsoft.SQL**-Diensttags aktiviert.

   ![Azure-Portal – Konfigurieren von VNET](./media/howto-manage-vnet-using-portal/2-configure-vnet.png)

   Das Konto muss über die erforderlichen Berechtigungen zum Erstellen eines virtuellen Netzwerks und eines Dienstendpunkts verfügen.

   Dienstendpunkte können unabhängig für virtuelle Netzwerke konfiguriert werden, der Benutzer benötigt hierzu Schreibzugriff auf das virtuelle Netzwerk.
    
   Um Azure-Dienstressourcen in einem VNET zu sichern, muss der Benutzer für die hinzuzufügenden Subnetze über die Berechtigung für „Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/“ verfügen. Diese Berechtigung ist standardmäßig in die integrierten Dienstadministratorrollen integriert und kann durch die Erstellung von benutzerdefinierten Rollen geändert werden.
    
   Erfahren Sie mehr über [integrierte Rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).
    
   VNETs und Ressourcen von Azure-Diensten können sich in demselben oder in unterschiedlichen Abonnements befinden. Wenn das VNET und die Ressourcen von Azure-Diensten in unterschiedlichen Abonnements enthalten sind, sollten sich die Ressourcen unter demselben Active Directory-Mandanten (AD) befinden.

   > [!IMPORTANT]
   > Es wird dringend empfohlen, diesen Artikel zu Dienstendpunktkonfigurationen und Überlegungen zu lesen, bevor Sie Dienstendpunkte konfigurieren. **Dienstendpunkt des virtuellen Netzwerks**: Ein [Dienstendpunkt des virtuellen Netzwerks](../virtual-network/virtual-network-service-endpoints-overview.md) ist ein Subnetz, dessen Eigenschaftswerte mindestens einen formalen Azure-Diensttypnamen enthalten. VNET-Dienstendpunkte verwenden den Diensttypnamen **Microsoft.Sql**, der auf den Azure-Dienst „SQL-Datenbank“ verweist. Dieses Diensttag gilt auch für die Dienste Azure SQL-Datenbank, Azure Database for PostgreSQL und Azure Database for MySQL. Beim Anwenden des Diensttags **Microsoft.Sql** auf einen VNET-Dienstendpunkt muss beachtet werden, dass auf diese Weise der Dienstendpunkt-Datenverkehr für alle Azure-Datenbankdienste konfiguriert wird. Dies schließt Azure SQL-Datenbank-, Azure Database for PostgreSQL- und Azure Database for MySQL-Server im Subnetz ein. 
   > 

3. Wenn Sie nach der Aktivierung auf **OK** klicken, sehen Sie, dass die VNET-Dienstendpunkte gemeinsam mit einer VNET-Regel aktiviert sind.

   ![Aktivierte VNET-Dienstendpunkte und erstellte VNET-Regel](./media/howto-manage-vnet-using-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Nächste Schritte
- Auf ähnliche Weise können Sie zum [Aktivieren von VNET-Dienstendpunkten und Erstellen einer VNET-Regel für Azure Database for MySQL unter Verwendung von Azure CLI](howto-manage-vnet-using-cli.md) Skripts erstellen.
- Wenn Sie Unterstützung beim Herstellen einer Verbindung mit einem Server für Azure-Datenbank für MySQL benötigen, lesen Sie die Informationen unter [Connection libraries for Azure Database for MySQL](./concepts-connection-libraries.md) (Verbindungsbibliotheken für Azure-Datenbank für MySQL).
