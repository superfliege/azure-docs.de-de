---
title: Migration zu Azure Database for PostgreSQL mit minimaler Ausfallzeit
description: In diesem Artikel wird beschrieben, wie Sie eine Migration mit minimaler Ausfallzeit einer PostgreSQL-Datenbank-Instanz zu Azure Database for PostgreSQL mit dem Azure Database Migration Service durchführen können.
services: postgresql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/11/2018
ms.openlocfilehash: 80e5d30677735b35d90fda6288d7bf6f2ea4aa1b
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093832"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Migration zu Azure Database for PostgreSQL mit minimaler Ausfallzeit
Mit der neu eingeführten **fortlaufenden Synchronisationsfunktion** für den [Azure Database Migration Service](https://aka.ms/get-dms) (DMS) können Sie PostgreSQL-Migrationen zu Azure Database for PostgreSQL mit minimaler Ausfallzeit ausführen. Diese Funktion beschränkt die Ausfallzeit der Anwendung.

## <a name="overview"></a>Übersicht
DMS führt einen ersten Ladevorgang Ihrer lokalen Datenbankinstanz nach Azure Database for PostgreSQL durch und synchronisiert dann kontinuierlich alle neuen Transaktionen mit Azure, während die Anwendung weiterhin ausgeführt wird. Nachdem die Daten auf der Azure-Zielseite erfasst wurden, stoppen Sie die Anwendung für einen kurzen Moment (minimale Ausfallzeit), warten auf den letzten Datenbatch (von dem Zeitpunkt an, an dem Sie die Anwendung stoppen, bis zu dem Moment, in dem die Anwendung tatsächlich nicht mehr verfügbar ist, um neuen Datenverkehr aufzunehmen), um die Erfassung im Ziel aufzuholen. Aktualisieren Sie dann Ihre Verbindungszeichenfolge, um auf Azure zu verweisen. Danach ist Ihre Anwendung auf Azure live geschaltet!

![Fortlaufende Synchronisierung mit dem Azure Database Migration Service](./media/howto-migrate-online/ContinuousSync.png)

DMS-Migration von PostgreSQL-Quellen ist derzeit als Vorschau verfügbar. Wenn Sie den Dienst zur Migration Ihrer PostgreSQL-Workloads testen möchten, registrieren Sie sich über die Azure DMS-[Vorschauseite](https://aka.ms/dms-preview). Ihr Feedback ist für die weitere Verbesserung des Diensts von großem Nutzen.

## <a name="next-steps"></a>Nächste Schritte
- Sehen Sie sich das Video [App Modernization with Microsoft Azure (App-Modernisierung mit Microsoft Azure)](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102) an, das eine Demo zum Migrieren von PostgreSQL-Apps zu Azure Database for PostgreSQL enthält.
- Sehen Sie sich das Tutorial [Migrieren von PostgreSQL zu Azure Database for PostgreSQL (online) mit DMS](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online) an.
