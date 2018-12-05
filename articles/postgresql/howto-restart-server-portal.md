---
title: Neustart eines Azure Database for PostgreSQL-Servers über das Azure-Portal
description: In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal einen Azure Database for PostgreSQL-Server neu starten können.
services: postgresql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: postgresql
ms.topic: article
ms.date: 11/16/2018
ms.openlocfilehash: 9a60f46f71a3d8e6f6d218c9a0ebd3194b6ab39f
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52166868"
---
# <a name="restart-azure-database-for-postgresql-server-using-azure-portal"></a>Neustart eines Azure Database for PostgreSQL-Servers über das Azure-Portal
In diesem Thema wird erläutert, wie Sie einen Azure Database for PostgreSQL-Server neu starten können. Möglicherweise müssen Sie Ihren Server aus Wartungsgründen neu starten, was zu einem kurzen Ausfall führt, da der Server den Vorgang ausführt.

Der Neustart des Servers wird blockiert, wenn der Dienst ausgelastet ist. Zum Beispiel kann der Dienst einen zuvor angeforderten Vorgang (z. B. das Skalieren von virtuellen Kernen) verarbeiten.
 
Die für einen Neustart benötigte Zeit hängt von dem PostgreSQL-Wiederherstellungsprozess ab. Um die Neustartzeit zu verkürzen, empfehlen wir, die Aktivitäten auf dem Server vor dem Neustart auf ein Minimum zu beschränken.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- [Einen Server und eine Datenbank für Azure-Datenbank für PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="perform-server-restart"></a>Ausführen des Serverneustarts

Mit den folgenden Schritten wird der PostgreSQL-Server neu gestartet:

1. Wählen Sie im Azure-Portal Ihren Azure Database for PostgreSQL-Server aus.

2. Klicken Sie auf der Seite **Übersicht** des Servers auf der Symbolleiste auf **Neu starten**.

   ![Azure Database for PostgreSQL – Übersicht – Schaltfläche „Neu starten“](./media/howto-restart-server-portal/2-server.png)

3. Klicken Sie auf **Ja**, um den Neustart des Servers zu bestätigen.

   ![Azure Database for PostgreSQL – Neustart bestätigen ](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Beachten Sie, dass der Serverstatus zu „Wird neu gestartet“ wechselt.

   ![Azure Database for PostgreSQL – Status „Wird neu gestartet“ ](./media/howto-restart-server-portal/4-restarting-status.png)

5. Vergewissern Sie sich, das der Neustart des Servers erfolgreich ist.

   ![Azure Database for PostgreSQL – Erfolgreicher Neustart ](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erstellen eines Azure Database for PostgreSQL-Servers über das Azure-Portal](./quickstart-create-server-database-portal.md)