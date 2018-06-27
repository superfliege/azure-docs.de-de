---
title: Migration nach Azure Database for MySQL mit minimaler Ausfallzeit
description: In diesem Artikel wird beschrieben, wie Sie eine Migration einer MySQL-Datenbank-Instanz nach Azure Database for MySQL mit dem Azure Database Migration Service durchführen können.
services: mysql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: ecbd35bd45bd11292bbe4a032329d704858d4c77
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293920"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migration nach Azure Database for MySQL mit minimaler Ausfallzeit
Mit der neu eingeführten **fortlaufenden Synchronisationsfunktion** für den [Azure Database Migration Service](https://aka.ms/get-dms) (DMS) können Sie MySQL-Migrationen nach Azure Database for MySQL mit minimaler Ausfallzeit ausführen. Diese Funktion beschränkt die Ausfallzeit der Anwendung.

## <a name="overview"></a>Übersicht
DMS führt einen ersten Ladevorgang Ihrer lokalen Datenbankinstanz nach Azure Database for MySQL durch und synchronisiert dann kontinuierlich alle neuen Transaktionen mit Azure, während die Anwendung weiterhin ausgeführt wird. Nachdem die Daten auf der Azure-Zielseite erfasst wurden, stoppen Sie die Anwendung für einen kurzen Moment (minimale Ausfallzeit), warten auf den letzten Datenbatch (von dem Zeitpunkt an, an dem Sie die Anwendung stoppen, bis zu dem Moment, in dem die Anwendung tatsächlich nicht mehr verfügbar ist, um neuen Datenverkehr aufzunehmen), um die Erfassung im Ziel aufzuholen. Aktualisieren Sie dann Ihre Verbindungszeichenfolge, um auf Azure zu verweisen. Danach ist Ihre Anwendung auf Azure live geschaltet.

![Fortlaufende Synchronisierung mit dem Azure Database Migration Service](./media/howto-migrate-online/ContinuousSync.png)

DMS-Migration von MySQL-Quellen ist derzeit als Vorschau verfügbar. Wenn Sie den Dienst zur Migration Ihrer MySQL-Workloads testen möchten, registrieren Sie sich über die Azure DMS-[Vorschauseite](https://aka.ms/dms-preview). Ihr Feedback ist für die weitere Verbesserung des Diensts von großem Nutzen.

## <a name="next-steps"></a>Nächste Schritte
- Sehen Sie sich das Video [Easily migrate MySQL/PostgreSQL apps to Azure managed service](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201) (in englischer Sprache) an, das Ihnen anhand einer Demo erläutert, wie man MySQL-Anwendungen nach Azure Database for MySQL migriert.
- Registrieren Sie sich für eine begrenzte Vorschau von Migrationen von MySQL nach Azure Database for MySQL mit minimaler Ausfallzeit über die Azure DMS-[Vorschau-Seite](https://aka.ms/dms-preview) an.
