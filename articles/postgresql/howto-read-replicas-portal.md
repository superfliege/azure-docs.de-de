---
title: Verwalten von Lesereplikaten für Azure Database for PostgreSQL über das Azure-Portal
description: Erfahren Sie, wie Sie Azure Database for PostgreSQL-Lesereplikate über das Azure-Portal verwalten.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/19/2019
ms.openlocfilehash: b34b103d3b710b90fd7b396f2c8d0e7adc27aaca
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56454666"
---
# <a name="create-and-manage-read-replicas-from-the-azure-portal"></a>Erstellen und Verwalten von Lesereplikaten über das Azure-Portal

In diesem Artikel erfahren Sie, wie Sie Lesereplikate in Azure Database for PostgreSQL über das Azure-Portal erstellen und verwalten. Weitere Informationen zu Lesereplikaten finden Sie in der [Übersicht](concepts-read-replicas.md).

> [!IMPORTANT]
> Das Feature für Lesereplikate befindet sich in der öffentlichen Vorschau.

## <a name="prerequisites"></a>Voraussetzungen
Ein [Azure Database for PostgreSQL-Server](quickstart-create-server-database-portal.md), der als Masterserver verwendet wird

## <a name="prepare-the-master-server"></a>Vorbereiten des Masterservers
Diese Schritte müssen durchgeführt werden, um einen Masterserver in den Tarifen „Universell“ und „Arbeitsspeicheroptimiert“ vorzubereiten.

Auf dem Masterserver muss der Parameter `azure.replication_support` auf **REPLICA** festgelegt werden. Bei Änderung dieses Parameters ist ein Neustart des Servers erforderlich, damit die Änderung wirksam wird.

1. Wählen Sie den vorhandenen Azure Database for PostgreSQL-Server, den Sie als Masterserver verwenden möchten, im Azure-Portal aus.

2. Wählen Sie im Menü auf der linken Seite **Serverparameter** aus.

3. Suchen Sie nach dem Parameter `azure.replication_support`.

   ![Suche nach dem Parameter „azure.replication_support“](./media/howto-read-replicas-portal/azure-replication-parameter.png)

4. Legen Sie die Parameterwert von `azure.replication_support` auf **REPLICA** fest. Wählen Sie **Speichern**, um Ihre Änderungen zu speichern.

   ![Festlegen des Parameters auf REPLICA und Speichern der Änderungen](./media/howto-read-replicas-portal/save-parameter-replica.png)

5. Nachdem Sie die Änderungen gespeichert haben, erhalten Sie eine Benachrichtigung:

   ![Benachrichtigung zu Speichervorgang](./media/howto-read-replicas-portal/parameter-save-notification.png)

6. Starten Sie den Server neu, um die Änderungen zu übernehmen. Die Vorgehensweise zum Neustart eines Servers finden Sie unter [Neustart eines Azure Database for PostgreSQL-Servers über das Azure-Portal](howto-restart-server-portal.md).


## <a name="create-a-read-replica"></a>Erstellen eines Lesereplikats
Führen Sie die folgenden Schritte aus, um ein Lesereplikat zu erstellen:

1.  Wählen Sie den vorhandenen Azure Database for PostgreSQL-Server aus, den Sie als Masterserver verwenden möchten. 

2.  Wählen Sie im Servermenü unter **EINSTELLUNGEN** die Option **Replikation** aus.

   Wenn Sie den Parameter `azure.replication_support` auf einem universellen oder arbeitsspeicheroptimierten Masterserver nicht auf **REPLICA** festgelegt und den Server nicht neu gestartet haben, erhalten Sie eine Benachrichtigung. Führen Sie diese Schritte aus, bevor Sie das Replikat erstellen.

3.  Wählen Sie **Replikat hinzufügen**.

   ![Hinzufügen eines Replikats](./media/howto-read-replicas-portal/add-replica.png)

4.  Geben Sie einen Namen für das Lesereplikat ein. Wählen Sie **OK**, um die Erstellung des Replikats zu bestätigen.

   ![Benennen des Replikats](./media/howto-read-replicas-portal/name-replica.png) 

Ein Replikat wird mit der gleichen Serverkonfiguration wie der Masterserver erstellt. Nachdem ein Replikat erstellt wurde, können mehrere Einstellungen unabhängig vom Masterserver geändert werden: die Computegeneration, die virtuellen Kerne, der Speicher und der Aufbewahrungszeitraum für Sicherungen. Auch der Tarif kann unabhängig geändert werden, allerdings nicht in den oder aus dem Tarif „Basic“.

> [!IMPORTANT]
> Bevor Sie die Konfiguration eines Masterservers mit neuen Werten aktualisieren, ändern Sie die Replikatkonfiguration in gleiche oder größere Werte. Durch diese Aktion wird sichergestellt, dass das Replikat mit allen Änderungen, die auf dem Masterserver durchgeführt werden, Schritt halten kann.

Nach der Erstellung des Lesereplikats kann dieses im Fenster **Replikation** angezeigt werden:

![Anzeigen des neuen Replikats im Fenster „Replikation“](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Beenden der Replikation
Sie können die Replikation zwischen einem Masterserver und einem Lesereplikat beenden.

> [!IMPORTANT]
> Das Beenden der Replikation zwischen einem Masterserver und einem Lesereplikat kann nicht mehr rückgängig gemacht werden. Das Lesereplikat wird zu einem eigenständigen Server, der sowohl Lese- als auch Schreibvorgänge unterstützt. Der eigenständige Server kann nicht wieder in ein Replikat umgewandelt werden.

Führen Sie die folgenden Schritte aus, um die Replikation zwischen einem Masterserver und einem Lesereplikat über das Azure-Portal zu beenden:

1.  Wählen Sie im Azure-Portal Ihren Azure Database for PostgreSQL-Masterserver aus.

2.  Wählen Sie im Servermenü unter **EINSTELLUNGEN** die Option **Replikation** aus.

3.  Wählen Sie den Replikatserver aus, für den Sie die Replikation beenden möchten.

   ![Auswählen des Replikats](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Wählen Sie **Replikation beenden** aus.

   ![Auswählen von „Replikation beenden“](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5.  Wählen Sie **OK**, um die Replikation zu beenden.

   ![Bestätigen der Beendigung der Replikation](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master-server"></a>Löschen eines Masterservers
Für das Löschen eines Masterservers führen Sie die gleichen Schritte wie für einen eigenständigen Azure Database for PostgreSQL-Server durch. 

> [!IMPORTANT]
> Wenn Sie einen Masterserver löschen, wird die Replikation auf allen Lesereplikaten beendet. Die Lesereplikate werden zu eigenständigen Servern, die nun Lese- und Schreibvorgänge unterstützen.

Um einen Server über das Azure-Portal zu löschen, gehen Sie folgendermaßen vor:

1.  Wählen Sie im Azure-Portal Ihren Azure Database for PostgreSQL-Masterserver aus.

2.  Öffnen Sie die Seite **Übersicht** für den Server. Klicken Sie auf **Löschen**.

   ![Auswählen des Löschvorgangs für den Masterserver auf der Übersichtsseite des Servers](./media/howto-read-replicas-portal/delete-server.png)
 
3.  Geben Sie den Namen des zu löschenden Masterservers ein. Wählen Sie **Löschen**, um das Löschen des Masterservers zu bestätigen.

   ![Bestätigen des Löschvorgangs für den Masterserver](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Löschen eines Replikats
Sie können ein Lesereplikat auf ähnliche Weise löschen wie einen Masterserver.

- Öffnen Sie im Azure-Portal die Seite **Übersicht** für das Lesereplikat. Klicken Sie auf **Löschen**.

   ![Auswählen des Löschvorgangs für das Replikat auf der Übersichtsseite des Replikats](./media/howto-read-replicas-portal/delete-replica.png)
 
Sie können das Lesereplikat auch über das Fenster **Replikation** löschen, indem Sie folgendermaßen vorgehen:

1.  Wählen Sie im Azure-Portal Ihren Azure Database for PostgreSQL-Masterserver aus.

2.  Wählen Sie im Servermenü unter **EINSTELLUNGEN** die Option **Replikation** aus.

3.  Wählen Sie das zu löschende Lesereplikat aus.

   ![Auswahl des zu löschenden Lesereplikats](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Wählen Sie **Replikat löschen** aus.

   ![Auswahl von „Replikat löschen“](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5.  Geben Sie den Namen des zu löschenden Replikats ein. Wählen Sie **Löschen**, um das Löschen des Replikats zu bestätigen.

   ![Bestätigen des Löschvorgangs für das Replikat](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Überwachen eines Replikats
Für die Überwachung von Lesereplikaten stehen zwei Metriken zur Verfügung.

### <a name="max-lag-across-replicas-metric"></a>Metrik „Maximale Verzögerung zwischen Replikaten“
Die Metrik **Maximale Verzögerung zwischen Replikaten** zeigt die Verzögerung in Byte zwischen dem Masterserver und dem Replikat mit der größten Verzögerung. 

1.  Wählen Sie im Azure-Portal den Azure Database for PostgreSQL-Masterserver aus.

2.  Klicken Sie auf **Metriken**. Wählen Sie im Fenster **Metriken** die Option **Maximale Verzögerung zwischen Replikaten** aus.

    ![Überwachen der maximalen Verzögerung zwischen Replikaten](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  Wählen Sie für Ihre **Aggregation** den Wert **Max** aus.


### <a name="replica-lag-metric"></a>Metrik „Replikatverzögerung“
Die Metrik **Replikatverzögerung** zeigt die Zeit seit der letzten wiedergegebenen Transaktion in einem Replikat an. Wenn keine Transaktionen auf dem Masterserver stattfinden, gibt die Metrik diese Verzögerungszeit wieder.

1.  Wählen Sie im Azure-Portal das Azure Database for PostgreSQL-Lesereplikat aus.

2.  Klicken Sie auf **Metriken**. Wählen Sie im Fenster **Metriken** die Option **Replikatverzögerung** aus.

   ![Überwachen der Replikatverzögerung](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3.  Wählen Sie für Ihre **Aggregation** den Wert **Max** aus. 
 
## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [Lesereplikate in Azure Database for PostgreSQL](concepts-read-replicas.md).