---
title: 'Tutorial: Verwenden von Azure Database Migration Service zur Offlinemigration von MongoDB zur Mongo-API von Azure Cosmos DB | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie mit Azure Database Migration Service eine Offlinemigration von einer lokalen MongoDB-Instanz zur Mongo-API von Azure Cosmos DB durchführen.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 12/11/2018
ms.openlocfilehash: 46e8ca97411e9dce77c0c82c4e0f5dd164bce01b
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53320726"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-db-mongo-api-offline-using-dms"></a>Tutorial: Offlinemigration von MongoDB zur Mongo-API von Azure Cosmos DB mit DMS
Sie können Azure Database Migration Service zum Durchführen einer Offlinemigration (einmalig) von Datenbanken aus einer lokalen oder Cloudinstanz von MongoDB zur Mongo-API von Azure Cosmos DB verwenden.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen Sie ein Migrationsprojekt mithilfe von Azure Database Migration Service.
> * Ausführen der Migration
> * Überwachen der Migration

In diesem Tutorial migrieren Sie das **Wingtips**-Dataset in MongoDB, das auf einem virtuellen Azure-Computer gehostet wird, mit Azure Database Migration Service zur Mongo-API von Azure Cosmos DB. Wenn Sie noch keine MongoDB-Quelle eingerichtet haben, lesen Sie den Artikel [Installieren und Konfigurieren von MongoDB auf einem virtuellen Windows-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/install-mongodb).

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:
- [Erstellen eines Kontos für die Azure Cosmos DB-MongoDB-API](https://ms.portal.azure.com/#create/Microsoft.DocumentDB).
- Erstellen Sie ein VNET für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das entweder über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) Standort-zu-Standort-Konnektivität für Ihre lokalen Quellserver bietet.
- Stellen Sie sicher, dass die Netzwerksicherheitsgruppen-Regeln Ihrer Azure Virtual Network-Instanz (VNET) nicht die folgenden Kommunikationsports blockieren: 443, 53, 9354, 445 und 12000. Weitere Details zur Datenverkehrsfilterung mit NSG in Azure VNET finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Öffnen Sie Ihre Windows-Firewall, damit Azure Database Migration Service auf die MongoDB-Server-Quellinstanz zugreifen kann (standardmäßig TCP-Port 27017).
- Wenn Sie eine Firewall-Appliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration zugreifen kann.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrieren des Ressourcenanbieters „Microsoft.DataMigration“
1. Melden Sie sich beim Azure-Portal an, und klicken Sie auf **Alle Dienste** und anschließend auf **Abonnements**.

   ![Abonnements im Portal anzeigen](media/tutorial-mongodb-to-cosmosdb/portal-select-subscription1.png)
       
2. Wählen Sie das Abonnement aus, in dem Sie die Azure Database Migration Service-Instanz erstellen möchten, und klicken Sie auf **Ressourcenanbieter**.
 
    ![Ressourcenanbieter anzeigen](media/tutorial-mongodb-to-cosmosdb/portal-select-resource-provider.png)
    
3.  Suchen Sie nach „Migration“, und wählen Sie rechts neben **Microsoft.DataMigration** die Option **Registrieren** aus.
 
    ![Registrieren des Ressourcenanbieters](media/tutorial-mongodb-to-cosmosdb/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Erstellen einer Instanz
1.  Wählen Sie im Azure-Portal die Option **+ Ressource erstellen**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus der Dropdownliste aus.

    ![Azure Marketplace](media/tutorial-mongodb-to-cosmosdb/portal-marketplace.png)

2.  Wählen Sie auf dem Bildschirm **Azure Database Migration Service** die Schaltfläche **Erstellen** aus.
 
    ![Erstellen einer Instanz von Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb/dms-create1.png)
  
3.  Geben Sie auf dem Bildschirm **Migrationsdienst erstellen** einen Namen für den Dienst, das Abonnement und eine neue oder vorhandene Ressourcengruppe an.

4. Wählen Sie den Standort aus, an dem Sie die Azure Database Migration Service-Instanz erstellen möchten. 

5. Wählen Sie ein vorhandenes virtuelles Netzwerk (VNET) aus, oder erstellen Sie ein neues.

    Das VNET erteilt Azure Database Migration Service Zugriff auf die SQL Server-Quellinstanz und die Azure SQL-Datenbank-Zielinstanz.

    Weitere Informationen zum Erstellen des VNET im Azure-Portal finden Sie im Artikel [Erstellen eines virtuellen Netzwerks im Azure Portal](https://aka.ms/DMSVnet).

6. Wählen Sie einen Tarif.

    Weitere Informationen zu Kosten und Tarifen finden Sie in der [Preisübersicht](https://aka.ms/dms-pricing).

    Wenn Sie Hilfe bei der Wahl des richtigen Azure Database Migration Service-Tarifs benötigen, beachten Sie die Empfehlungen in [diesem Blogbeitrag](https://go.microsoft.com/fwlink/?linkid=861067).  

     ![Konfigurieren der Einstellungen einer Azure Database Migration Service-Instanz](media/tutorial-mongodb-to-cosmosdb/dms-settings2.png)

7.  Wählen Sie **Erstellen**, um den Dienst zu erstellen.

## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts
Nachdem der Dienst erstellt wurde, suchen Sie diesen im Azure-Portal, öffnen Sie ihn, und erstellen Sie anschließend ein neues Migrationsprojekt.

1. Wählen Sie im Azure-Portal **Alle Dienste**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus.
 
      ![Suchen aller Instanzen von Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb/dms-search.png)

2. Suchen Sie im Bildschirm **Azure Database Migration Services** nach dem Namen der von Ihnen erstellten Azure Database Migration Service-Instanz, und wählen Sie die Instanz aus.

3. Wählen Sie **+ Neues Migrationsprojekt** aus.

4. Geben Sie im Bildschirm **Neues Migrationsprojekt** einen Projektnamen ein, und wählen Sie im Textfeld **Quellservertyp** die Option **MongoDB** und im Textfeld **Zielservertyp** die Option **CosmosDB (MongoDB-API)** aus. Wählen Sie außerdem für **Aktivitätstyp auswählen** die Option **Offlinedatenmigration** aus. 

    ![Erstellen eines Database Migration Service-Projekts](media/tutorial-mongodb-to-cosmosdb/dms-create-project.png)

5.  Klicken Sie auf **Aktivität erstellen und ausführen**, um das Projekt zu erstellen und die Migrationsaktivität auszuführen.

## <a name="specify-source-details"></a>Angeben von Quelldetails
1. Geben Sie im Bildschirm **Source details** (Quelldetails) die Verbindungsdetails für die MongoDB-Server-Quellinstanz an.
    
   Sie können auch den Verbindungszeichenfolgen-Modus verwenden und einen Speicherort für einen Blogspeicherdatei-Container angeben, in dem Sie die Sammlungsdaten gesichert haben, die Sie migrieren möchten.

   > [!NOTE]
   > Der Azure Database Migration Service kann auch Bson- oder JSON-Dokumente zu Azure Cosmos DB-Mongo-API-Sammlungen migrieren.
    
   Sie können auch die IP-Adresse für Situationen verwenden, in denen DNS-Namensauflösung nicht möglich ist.

   ![Angeben von Quelldetails](media/tutorial-mongodb-to-cosmosdb/dms-specify-source.png)

2. Wählen Sie **Speichern**aus.

## <a name="specify-target-details"></a>Angeben von Zieldetails
1. Geben Sie auf dem Bildschirm **Zieldetails für die Migration** die Verbindungsdetails für das Azure Cosmos DB-Zielkonto an (also für das vorab bereitgestellte Azure Cosmos DB-MongoDB-Konto, zu dem Sie die MongoDB-Daten migrieren).

    ![Angeben von Zieldetails](media/tutorial-mongodb-to-cosmosdb/dms-specify-target.png)

2. Wählen Sie **Speichern**aus.

## <a name="map-to-target-databases"></a>Zuordnen zu Zieldatenbanken
1. Ordnen Sie im Bildschirm **Map to target databases** (Zu Zieldatenbanken zuordnen) die Quell- und die Zieldatenbank für die Migration einander zu.

    Wenn die Zieldatenbank denselben Datenbanknamen wie die Quelldatenbank enthält, wählt Azure Database Migration Service die Zieldatenbank standardmäßig aus.

    Wenn die Zeichenfolge **Erstellen** neben dem Datenbanknamen angezeigt wird, hat der Azure Database Migration Service die Zieldatenbank nicht gefunden, und der Dienst erstellt die Datenbank für Sie.

    An diesem Punkt der Migration können Sie [Durchsatz bereitstellen](https://docs.microsoft.com/azure/cosmos-db/set-throughput). In Cosmos DB können Sie den Durchsatz entweder auf Datenbankebene oder für jede Sammlung individuell bereitstellen. Der Durchsatz wird in [Anforderungseinheiten](https://docs.microsoft.com/azure/cosmos-db/request-units) (RUs) gemessen. Weitere Informationen: [Azure Cosmos DB – Preise](https://azure.microsoft.com/pricing/details/cosmos-db/).

    ![Zuordnen zu Zieldatenbanken](media/tutorial-mongodb-to-cosmosdb/dms-map-target-databases.png)

2. Wählen Sie **Speichern**aus.
3. Erweitern Sie auf dem Einstellungsbildschirm **Sammlung** die Auflistung der Sammlungen, und überprüfen Sie dann die Liste der Sammlungen, die migriert werden.

    Beachten Sie, dass der Azure Database Migration Service automatisch alle Sammlungen auswählt, die in der MongoDB-Quellinstanz vorhanden sind, jedoch nicht auf dem Azure Cosmos DB-Zielkonto. Wenn Sie Sammlungen, die bereits Daten enthalten, erneut migrieren möchten, müssen Sie die Sammlungen auf diesem Blatt explizit auswählen.

    Sie können die Anzahl an RUs angeben, die die Sammlungen verwenden sollen. Der Azure Database Migration Service schlägt intelligente Standardwerte basierend auf der Sammlungsgröße vor.

    Sie können auch einen Shardschlüssel angeben, um die [Partitionierung in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview) für eine optimale Skalierbarkeit zu nutzen. Überprüfen Sie unbedingt die [bewährten Methoden für die Auswahl eines Shard-/Partitionsschlüssels](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey).

    ![Auswählen von Sammlungstabellen](media/tutorial-mongodb-to-cosmosdb/dms-collection-setting.png)

4. Wählen Sie **Speichern**aus.

5. Geben Sie auf dem Bildschirm **Migrationszusammenfassung** im Textfeld **Aktivitätsname** einen Namen für die Migrationsaktivität an.

    ![Migrationszusammenfassung](media/tutorial-mongodb-to-cosmosdb/dms-migration-summary.png)

## <a name="run-the-migration"></a>Ausführen der Migration
- Wählen Sie **Migration ausführen** aus.

    Das Fenster „Migrationsaktivität“ wird angezeigt, und der **Status** der Aktivität lautet **Nicht gestartet**.

    ![Aktivitätsstatus](media/tutorial-mongodb-to-cosmosdb/dms-activity-status.png)

## <a name="monitor-the-migration"></a>Überwachen der Migration
- Klicken Sie auf dem Bildschirm „Migrationsaktivität“ auf **Aktualisieren**, um die Anzeige zu aktualisieren, bis der **Status** der Migration **Abgeschlossen** lautet.

   > [!NOTE]
   > Sie können die Aktivität zum Abrufen von Details der Migrationsmetriken auf Datenbank- und Sammlungsebene auswählen.

    ![Aktivitätsstatus: Abgeschlossen](media/tutorial-mongodb-to-cosmosdb/dms-activity-completed.png)

## <a name="verify-data-in-cosmos-db"></a>Überprüfen von Daten in Cosmos DB

- Nachdem die Migration abgeschlossen ist, können Sie Ihr Cosmos DB-MongoDB-API-Konto überprüfen, um sicherzustellen, dass alle Sammlungen erfolgreich migriert wurden.

    ![Aktivitätsstatus: Abgeschlossen](media/tutorial-mongodb-to-cosmosdb/dms-cosmosdb-data-explorer.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

 * [Cosmos DB-Dienstinformationen](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich in der Migrationsanleitung im [Leitfaden zur Datenbankmigration von Microsoft](https://datamigration.microsoft.com/) über weitere Szenarios.