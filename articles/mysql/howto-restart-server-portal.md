---
title: Neustarten eines Azure Database for MySQL-Servers mithilfe des Azure-Portals
description: In diesem Artikel wird beschrieben, wie Sie einen Azure Database for MySQL-Server über das Azure-Portal neu starten.
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 11/16/2018
ms.openlocfilehash: 278b535e57dc4d3e79c1ca4d08c89f3e4d833325
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52166706"
---
# <a name="restart-azure-database-for-mysql-server-using-azure-portal"></a>Neustarten eines Azure Database for MySQL-Servers mithilfe des Azure-Portals
In diesem Thema wird erläutert, wie Sie einen Azure Database for MySQL-Server neu starten. Es kann vorkommen, dass Sie Ihren Server zu Wartungszwecken neu starten müssen. In diesem Fall kommt es zu einem kurzen Ausfall, weil der Vorgang vom Server ausgeführt wird.

Der Neustart des Servers wird blockiert, wenn der Dienst ausgelastet ist. Beispielsweise kann der Dienst einen zuvor angeforderten Vorgang (z. B. das Skalieren von virtuellen Kernen) verarbeiten.

Die für einen Neustart benötigte Zeit hängt vom MySQL-Wiederherstellungsprozess ab. Um die Neustartzeit zu verkürzen, empfehlen wir, die Aktivitäten auf dem Server vor dem Neustart auf ein Minimum zu beschränken.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- Einen [Azure Database for MySQL-Server und eine Datenbank](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Ausführen des Serverneustarts

Mit den folgenden Schritten wird der MySQL-Server neu gestartet:

1. Wählen Sie im Azure-Portal Ihren Azure Database for MySQL-Server aus.

2. Klicken Sie auf der Seite **Übersicht** des Servers auf der Symbolleiste auf **Neu starten**.

   ![Azure Database for MySQL – Übersicht – Schaltfläche „Neu starten“](./media/howto-restart-server-portal/2-server.png)

3. Klicken Sie auf **Ja**, um den Neustart des Servers zu bestätigen. 

   ![Azure Database for MySQL – Neustart bestätigen ](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Beachten Sie, dass sich der Serverstatus in „Wird neu gestartet“ ändert.

   ![Azure Database for MySQL – Status „Wird neu gestartet“ ](./media/howto-restart-server-portal/4-restarting-status.png)

5. Vergewissern Sie sich, dass der Neustart des Servers erfolgreich war.

   ![Azure Database for MySQL – Erfolgreicher Neustart ](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erstellen eines Servers mit Azure-Datenbank für MySQL im Azure-Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)