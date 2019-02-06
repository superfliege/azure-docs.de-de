---
title: Verwalten von Lesereplikaten für Azure Database for PostgreSQL im Azure-Portal
description: Dieser Artikel beschreibt die Verwaltung von Azure Database for PostgreSQL-Lesereplikaten im Azure-Portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/23/2019
ms.openlocfilehash: bf6e9947c21e5b07b2adc99de585c77444447c04
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54902654"
---
# <a name="how-to-create-and-manage-read-replicas-in-the-azure-portal"></a>Erstellen und Verwalten von Lesereplikaten über das Azure-Portal

> [!IMPORTANT]
> Das Feature für Lesereplikate befindet sich in der Public Preview-Phase.


In diesem Artikel erfahren Sie, wie Sie über das Azure-Portal Lesereplikate im Azure Database for PostgreSQL-Dienst erstellen und verwalten. Weitere Informationen zu Lesereplikaten finden Sie in der [Dokumentation der Konzepte](concepts-read-replicas.md).

## <a name="prerequisites"></a>Voraussetzungen
- Ein [Azure Database for PostgreSQL-Server](quickstart-create-server-database-portal.md), der als Masterserver verwendet wird

## <a name="prepare-the-master-server"></a>Vorbereiten des Masterservers
Diese Schritte zur Vorbereitung des Masters gelten nur für universelle und arbeitsspeicheroptimierte Server.

Der Parameter **azure.replication_support** muss auf dem Masterserver auf REPLICA festgelegt werden. Damit die Änderung an diesem Parameter angewandt wird, ist ein Neustart des Servers erforderlich.

1. Wählen Sie den vorhandenen Azure Database for PostgreSQL-Server, den Sie als Masterserver verwenden möchten, im Azure-Portal aus.

2. Wählen Sie im Menü auf der linken Seite **Serverparameter** aus.

3. Suchen Sie nach **azure.replication_support**.

   ![Azure Database for PostgreSQL – azure.replication_support](./media/howto-read-replicas-portal/azure-replication-parameter.png)

4. Legen Sie **azure.replication_support** auf REPLICA fest. **Speichern** Sie die Änderungen.

   ![Azure Database for PostgreSQL – REPLICA und Neustart](./media/howto-read-replicas-portal/save-parameter-replica.png)

5. Sie erhalten nach dem Speichern eine Benachrichtigung.

   ![Azure Database for PostgreSQL – Speicherbenachrichtigung](./media/howto-read-replicas-portal/parameter-save-notification.png)

6. Starten Sie den Server neu, um die Änderung nach dem Speichern zu übernehmen. Weitere Informationen zum Neustarten eines Servers finden Sie in der [Dokumentation zu Neustarts](howto-restart-server-portal.md).


## <a name="create-a-read-replica"></a>Erstellen eines Lesereplikats
Lesereplikate können anhand der folgenden Schritte erstellt werden:
1.  Wählen Sie den vorhandenen Azure Database for PostgreSQL-Server aus, den Sie als Masterserver verwenden möchten. 

2.  Wählen Sie im Menü unter „EINSTELLUNGEN“ die Option „Replikation“ aus.

   Wenn Sie **azure.replication_support** für den universellen oder arbeitsspeicheroptimierten Master nicht auf REPLICA festgelegt und den Server neu gestartet haben, werden Sie in einer Meldung dazu aufgefordert. Führen Sie dies aus, bevor Sie mit dem Erstellen fortfahren.

3.  Wählen Sie „Replikat hinzufügen“ aus.

   ![Azure Database for PostgreSQL – Hinzufügen eines Replikats](./media/howto-read-replicas-portal/add-replica.png)

4.  Geben Sie einen Namen für den Replikatserver ein, und wählen Sie „OK“ aus, um die Erstellung des Replikats zu bestätigen.

   ![Azure Database for PostgreSQL – Benennen des Replikats](./media/howto-read-replicas-portal/name-replica.png) 

> [!IMPORTANT]
> Lesereplikate werden mit der gleichen Serverkonfiguration wie der Masterserver erstellt. Nachdem ein Replikat erstellt wurde, können Sie den Tarif (Ausnahme: Wechsel zu oder aus Basic), die Computegeneration, die virtuellen Kerne, den Speicher und den Aufbewahrungszeitraum für Sicherungen unabhängig vom Masterserver ändern.

> [!IMPORTANT]
> Bevor Sie die Konfiguration eines Masterservers mit neuen Werten aktualisieren, sollten Sie die Konfiguration der Replikate in gleiche oder größere Werte ändern. Andernfalls wird ein Fehler verursacht. Dadurch wird sichergestellt, dass die Replikate die Änderungen am Master einhalten können. 


Nach der Erstellung des Replikatservers kann dieser im Fenster „Replikation“ angezeigt werden.

![Azure Database for PostgreSQL – Neues Replikat](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Beenden der Replikation

> [!IMPORTANT]
> Das Beenden der Replikation auf einem Server kann nicht rückgängig gemacht werden. Wenn die Replikation zwischen einem Master und dem Replikat beendet wurde, kann dies nicht rückgängig gemacht werden. Der Replikatserver wird zu einem eigenständigen Server und unterstützt nun Lese- und Schreibvorgänge. Der Server kann nicht wieder in ein Replikat umgewandelt werden.

Führen Sie die folgenden Schritte aus, um die Replikation zwischen einem Masterserver und einem Replikat im Azure-Portal zu beenden:
1.  Wählen Sie im Azure-Portal Ihren Azure Database for PostgreSQL-Masterserver aus.

2.  Wählen Sie im Menü unter „EINSTELLUNGEN“ die Option „Replikation“ aus.

3.  Wählen Sie den Replikatserver aus, für den Sie die Replikation beenden möchten.

   ![Azure Database for PostgreSQL – Auswählen eines Replikats](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Wählen Sie „Replikation beenden“ aus.

   ![Azure Database for PostgreSQL – Auswählen von „Replikation beenden“](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5.  Klicken Sie auf „OK“, um zu bestätigen, dass Sie die Replikation beenden möchten.

   ![Azure Database for PostgreSQL – Bestätigen der Beendigung der Replikation](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master"></a>Löschen eines Masters

> [!IMPORTANT]
> Wenn Sie einen Masterserver löschen, wird die Replikation auf allen Replikatservern beendet. Replikatserver werden zu eigenständigen Servern, die nun Lese- und Schreibvorgänge unterstützen.
Für das Löschen eines Masters sind die gleichen Schritte wie für einen eigenständigen Azure Database for PostgreSQL-Server erforderlich. Gehen Sie wie folgt vor, um einen Server im Azure-Portal zu löschen:

1.  Wählen Sie im Azure-Portal Ihren Azure Database for PostgreSQL-Masterserver aus.

2.  Wählen Sie unter „Übersicht“ die Option „Löschen“ aus.

   ![Azure Database for PostgreSQL – Löschen eines Servers](./media/howto-read-replicas-portal/delete-server.png)
 
3.  Geben Sie den Namen des Masterservers ein, und wählen Sie „Löschen“ aus, um das Löschen des Masterservers zu bestätigen.

   ![Azure Database for PostgreSQL – Bestätigen des Löschvorgangs](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Löschen eines Replikats
Um ein Lesereplikat zu löschen, führen Sie die gleichen Schritte wie oben zum Löschen eines Masterservers aus. Öffnen Sie zunächst die Seite „Übersicht“ des Replikats, und wählen Sie dann „Löschen“ aus.

   ![Azure Database for PostgreSQL – Löschen eines Replikats](./media/howto-read-replicas-portal/delete-replica.png)
 
Alternativ können Sie es im Fenster „Replikation“ löschen.
1.  Wählen Sie im Azure-Portal Ihren Azure Database for PostgreSQL-Masterserver aus.

2.  Wählen Sie im Menü unter „EINSTELLUNGEN“ die Option „Replikation“ aus.

3.  Wählen Sie den Replikatserver aus, den Sie löschen möchten. 

   ![Azure Database for PostgreSQL – Auswählen eines Replikats](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Wählen Sie „Replikat löschen“ aus.

   ![Azure Database for PostgreSQL – Auswählen von „Replikat löschen“](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5.  Geben Sie den Namen des Replikats ein, und wählen Sie „Löschen“ aus, um das Löschen des Replikats zu bestätigen.

   ![Azure Database for PostgreSQL – Bestätigen des Löschvorgangs für ein Replikat](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Überwachen eines Replikats
### <a name="max-lag-across-replicas"></a>Maximale Verzögerung zwischen Replikaten
Die Metrik **Max lag across replicas** (Maximale Verzögerung zwischen Replikaten) zeigt den Zeitversatz zwischen dem Master und dem Replikat mit der größten Verzögerung. 

1.  Wählen Sie im Azure-Portal den Azure Database for PostgreSQL-**Masterserver** aus.

2.  Wählen Sie „Metriken“ aus. Wählen Sie im Fenster „Metriken“ die Option **Max Lag Across Replicas** (Maximale Verzögerung zwischen Replikaten) aus.

    ![Azure Database for PostgreSQL – Überwachen der maximalen Verzögerung zwischen Replikaten](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  Wählen Sie als Aggregation **Max** aus. 

### <a name="replica-lag"></a>Replikatverzögerung
Die Metrik **Replica Lag** (Replikatverzögerung) zeigt die Zeit seit der letzten wiedergegebenen Transaktion in diesem Replikat an. Wenn keine Transaktionen auf dem Masterserver stattfinden, gibt die Metrik diese Verzögerungszeit wieder.

1.  Wählen Sie im Azure-Portal einen Azure Database for PostgreSQL-**Replikatserver** aus.

2.  Wählen Sie „Metriken“ aus. Wählen Sie im Fenster „Metriken“ die Option **Replica Lag** (Replikatverzögerung) aus.

   ![Azure Database for PostgreSQL – Überwachen der Replikatverzögerung](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3.  Wählen Sie als Aggregation **Max** aus. 
 
## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Lesereplikate in Azure Database for PostgreSQL](concepts-read-replicas.md).