---
title: Neustarten eines Azure Database for MySQL-Servers mithilfe des Azure-Portals
description: In diesem Artikel wird beschrieben, wie Sie einen Azure Database for MySQL-Server über das Azure-Portal neu starten.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 11/16/2018
ms.openlocfilehash: 83e862aea5b1f2de5a3f80970c2331fc9d81704e
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540246"
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

[Schnellstart: Erstellen eines Azure Database for MySQL-Servers mithilfe des Azure-Portals](./quickstart-create-mysql-server-database-using-azure-portal.md)