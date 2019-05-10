---
title: Tutorial zum Entwerfen eines Echtzeitdashboards mit Azure Database for PostgreSQL – Hyperscale (Citus) (Vorschau)
description: In diesem Tutorial erfahren Sie, wie Sie verteilte Tabellen in Azure Database for PostgreSQL Hyperscale (Citus) (Vorschau) erstellen, auffüllen und abfragen.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: tutorial
ms.date: 05/06/2019
ms.openlocfilehash: 7324ab1d7aa6e42100c9c6760c17b0ea6445f21d
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079455"
---
# <a name="tutorial-design-a-real-time-analytics-dashboard-by-using-azure-database-for-postgresql--hyperscale-citus-preview"></a>Tutorial: Entwerfen eines Dashboards zur Echtzeitanalyse mithilfe von Azure Database for PostgreSQL – Hyperscale (Citus) (Vorschau)

In diesem Tutorial verwenden Sie Azure Database for PostgreSQL – Hyperscale (Citus) (Vorschau), um Folgendes zu lernen:

> [!div class="checklist"]
> * Erstellen einer Hyperscale (Citus)-Servergruppe
> * Verwenden des psql-Hilfsprogramms zum Erstellen eines Schemas
> * Knotenübergreifende Shard-Tabellen
> * Generieren von Beispieldaten
> * Ausführen von Rollups
> * Abfragen von Rohdaten und aggregierten Daten
> * Ablauf von Daten

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-an-azure-database-for-postgresql"></a>Erstellen einer Azure-Datenbank für PostgreSQL

Führen Sie die folgenden Schritte aus, um eine Azure-Datenbank für PostgreSQL-Server zu erstellen:
1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **Azure-Datenbank für PostgreSQL** aus.
3. Klicken Sie als Bereitstellungsoption auf die Schaltfläche **Erstellen** unter **Hyperscale (Citus)-Servergruppe – VORSCHAU.**
4. Geben Sie im Formular für den neuen Server folgende Informationen an:
   - Ressourcengruppe: Klicken Sie auf den Link **Neue erstellen** unterhalb des Textfelds für dieses Feld. Geben Sie einen Namen ein, z.B. **myresourcegroup**.
   - Servergruppenname: **mydemoserver** (der Name eines Servers, der dem DNS-Namen zugeordnet wird und global eindeutig sein muss).
   - Benutzername des Administrators: **myadmin** (er wird später zum Herstellen der Datenbankverbindung verwendet).
   - Kennwort: muss mindestens 8 Zeichen lang sein und Zeichen aus drei der folgenden Kategorien enthalten: Großbuchstaben des englischen Alphabets, Kleinbuchstaben des englischen Alphabets, Ziffern (0–9) und nicht-alphanumerische Zeichen (!, $, #, % usw.)
   - Standort: Verwenden Sie den Standort, der Ihren Benutzern am nächsten ist, damit sie möglichst schnell auf die Daten zugreifen können.

   > [!IMPORTANT]
   > Der hier angegebene Benutzername und das Kennwort für den Serveradministrator sind erforderlich, um später in diesem Tutorial die Anmeldung am Server und bei den zugehörigen Datenbanken auszuführen. Behalten Sie diese Angaben im Kopf, oder notieren Sie sie zur späteren Verwendung.

5. Klicken Sie auf **Servergruppe konfigurieren**. Übernehmen Sie die Einstellungen in diesem Abschnitt unverändert, und klicken Sie auf **Speichern**.
6. Klicken Sie auf **Bewerten + erstellen** und dann auf **Erstellen**, um den Server bereitzustellen. Die Bereitstellung dauert einige Minuten.
7. Die Seite leitet zur Überwachung der Bereitstellung um. Wenn sich der Livestatus von **Ihre Bereitstellung wird ausgeführt** in **Ihre Bereitstellung wurde abgeschlossen** ändert, klicken Sie links auf der Seite auf das Menüelement **Ausgaben**.
8. Die Seite „Ausgaben“ enthält einen Koordinatorhostnamen mit einer Schaltfläche daneben, um den Wert in die Zwischenablage zu kopieren. Notieren Sie diese Informationen zur späteren Verwendung.

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene

Der Azure Database for PostgreSQL-Dienst verwendet eine Firewall auf der Serverebene. Standardmäßig hindert die Firewall alle externen Anwendungen und Tools daran, eine Verbindung mit dem Server und mit Datenbanken auf dem Server herzustellen. Wir müssen eine Regel zum Öffnen der Firewall für einen bestimmten IP-Adressbereich hinzufügen.

1. Klicken Sie im Abschnitt **Ausgaben**, in dem Sie zuvor den Hostnamen des Koordinatorknotens kopiert hatten, zurück zum Menüelement **Übersicht**.

2. Suchen Sie die Skalierungsgruppe für Ihre Bereitstellung in der Liste der Ressourcen, und klicken Sie darauf. (Ihrem Namen ist „sg-“ vorangestellt.)

3. Klicken Sie im Menü auf der linken Seite unter **Sicherheit** auf **Firewall**.

4. Klicken Sie auf den Link **+ Firewallregel für die aktuelle Client-IP-Adresse hinzufügen**. Klicken Sie abschließend auf die Schaltfläche **Speichern**.

5. Klicken Sie auf **Speichern**.

   > [!NOTE]
   > Der Azure-PostgreSQL-Server kommuniziert über Port 5432. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 5432 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbank-Server herstellen, wenn Ihre IT-Abteilung Port 5432 öffnet.
   >

## <a name="connect-to-the-database-using-psql-in-cloud-shell"></a>Herstellen einer Verbindung mit der Datenbank mithilfe von psql in Cloud Shell

Stellen Sie jetzt mit dem Befehlszeilen-Hilfsprogramm [psql](https://www.postgresql.org/docs/current/app-psql.html) eine Verbindung mit dem Azure Database for PostgreSQL-Server her.
1. Starten Sie die Azure Cloud Shell über das Terminalsymbol im oberen Navigationsbereich.

   ![Azure-Datenbank für PostgreSQL – Azure Cloud Shell-Terminalsymbol](./media/tutorial-design-database-hyperscale-realtime/psql-cloud-shell.png)

2. Die Azure Cloud Shell wird in Ihrem Browser geöffnet, sodass Sie Bash-Befehle eingeben können.

   ![Azure-Datenbank für PostgreSQL – Azure Shell-Bash-Eingabeaufforderung](./media/tutorial-design-database-hyperscale-realtime/psql-bash.png)

3. Stellen Sie an der Cloud Shell-Eingabeaufforderung mit den psql-Befehlen eine Verbindung mit Ihrem Azure-Datenbank für PostgreSQL-Server her. Das folgende Format wird verwendet, um mit dem Hilfsprogramm [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) eine Verbindung mit einer Azure-Datenbank für PostgreSQL-Server herzustellen:
   ```bash
   psql --host=<myserver> --username=myadmin --dbname=citus
   ```

   Mit dem folgenden Befehl wird beispielsweise mit den Zugriffsanmeldeinformationen eine Verbindung mit der Standarddatenbank **citus** auf Ihrem PostgreSQL-Server **mydemoserver.postgres.database.azure.com** hergestellt. Geben Sie Ihr Serveradministrator-Kennwort ein, wenn Sie dazu aufgefordert werden.

   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --username=myadmin --dbname=citus
   ```

## <a name="use-psql-utility-to-create-a-schema"></a>Verwenden des psql-Hilfsprogramms zum Erstellen eines Schemas

Nach dem Herstellen der Verbindung mit der Azure Database for PostgreSQL – Hyperscale (Citus) (Vorschau) mithilfe von psql können Sie ein paar einfache Aufgaben ausführen. Dieses Tutorial führt Sie schrittweise durch das Erfassen von Verkehrsdaten aus Webanalysen und den anschließenden Rollup der Daten, um Echtzeitdashboards auf der Grundlage dieser Daten bereitzustellen.

Erstellen wir eine Tabelle, die unsere gesamten Webdatenverkehrs-Rohdaten nutzt. Führen Sie die folgenden Befehle im psql-Terminal aus:

```sql
CREATE TABLE http_request (
  site_id INT,
  ingest_time TIMESTAMPTZ DEFAULT now(),

  url TEXT,
  request_country TEXT,
  ip_address TEXT,

  status_code INT,
  response_time_msec INT
);
```

Wir erstellen darüber hinaus eine Tabelle, die unsere minütlichen Aggregationen aufnimmt und eine Tabelle, die die Position unseres letzten Rollups verwaltet. Führen Sie in psql außerdem Folgendes aus:

```sql
CREATE TABLE http_request_1min (
  site_id INT,
  ingest_time TIMESTAMPTZ, -- which minute this row represents

  error_count INT,
  success_count INT,
  request_count INT,
  average_response_time_msec INT,
  CHECK (request_count = error_count + success_count),
  CHECK (ingest_time = date_trunc('minute', ingest_time))
);

CREATE INDEX http_request_1min_idx ON http_request_1min (site_id, ingest_time);

CREATE TABLE latest_rollup (
  minute timestamptz PRIMARY KEY,

  CHECK (minute = date_trunc('minute', minute))
);
```

Sie können jetzt die neu erstellten Tabellen in der Liste der Tabellen mit diesem psql-Befehl anzeigen:

```postgres
\dt
```

## <a name="shard-tables-across-nodes"></a>Knotenübergreifende Shard-Tabellen

Eine Hyperscalebereitstellung speichert Tabellenzeilen basierend auf dem Wert einer vom Benutzer zugeordneten Spalte auf verschiedenen Knoten. Die „Verteilungsspalte“ gibt an, wie die Daten auf die Knoten verteilt sind.

Legen wir die Verteilungsspalte auf „site\_id“ fest, den Shardschlüssel. Führen Sie in psql diese Funktionen aus:

  ```sql
SELECT create_distributed_table('http_request',      'site_id');
SELECT create_distributed_table('http_request_1min', 'site_id');
```

## <a name="generate-sample-data"></a>Generieren von Beispieldaten

Jetzt sollte unsere Servergruppe für die Erfassung einiger Daten bereit sein. Wir können den folgenden Code lokal aus unserer `psql`-Verbindung ausführen, um fortlaufend Daten einzufügen.

```sql
DO $$
  BEGIN LOOP
    INSERT INTO http_request (
      site_id, ingest_time, url, request_country,
      ip_address, status_code, response_time_msec
    ) VALUES (
      trunc(random()*32), clock_timestamp(),
      concat('http://example.com/', md5(random()::text)),
      ('{China,India,USA,Indonesia}'::text[])[ceil(random()*4)],
      concat(
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2)
      )::inet,
      ('{200,404}'::int[])[ceil(random()*2)],
      5+trunc(random()*150)
    );
    PERFORM pg_sleep(random() * 0.25);
  END LOOP;
END $$;
```

Die Abfrage fügt ungefähr jede Viertelsekunde eine Zeile hinzu. Die Zeilen werden auf verschiedenen Workerknoten gespeichert, gemäß Anweisung durch die Verteilungsspalte `site_id`.

   > [!NOTE]
   > Führen Sie die Abfrage zur Datengenerierung weiter aus, und öffnen Sie für die verbleibenden Befehle in diesem Tutorial eine zweite psql-Verbindung.
   >

## <a name="query"></a>Abfragen

Die Hyperscale-Hostingoption ermöglicht es aus Geschwindigkeitsgründen, dass Abfragen von mehreren Knoten parallel verarbeitet werden. Beispielsweise berechnet die Datenbank Aggregate wie SUM und COUNT auf Workerknoten und kombiniert die Ergebnisse in einer endgültigen Antwort.

Die folgende Abfrage zählt die Webanforderungen pro Minute zusammen mit einer Reihe von Statistikangaben.
Versuchen Sie, sie in psql auszuführen, und beobachten Sie die Ergebnisse.

```sql
SELECT
  site_id,
  date_trunc('minute', ingest_time) as minute,
  COUNT(1) AS request_count,
  SUM(CASE WHEN (status_code between 200 and 299) THEN 1 ELSE 0 END) as success_count,
  SUM(CASE WHEN (status_code between 200 and 299) THEN 0 ELSE 1 END) as error_count,
  SUM(response_time_msec) / COUNT(1) AS average_response_time_msec
FROM http_request
WHERE date_trunc('minute', ingest_time) > now() - '5 minutes'::interval
GROUP BY site_id, minute
ORDER BY minute ASC;
```

## <a name="performing-rollups"></a>Ausführen von Rollups

Die Abfrage oben funktioniert gut in den frühen Phasen, aber mit zunehmender Datenmenge nimmt ihre Leistung ab. Selbst bei verteilter Verarbeitung ist es schneller, diese Daten vorab zu berechnen als sie wiederholt neu zu berechnen.

Wir können sicherstellen, dass unser Dashboard schnell bleibt, indem wir regelmäßig einen Rollup der Rohdaten in eine Aggregattabelle ausführen. In diesem Fall führen wir den Rollup in unsere 1-Minuten-Aggregattabelle durch, wir können aber auch Aggregationen von 5 Minuten, 15 Minuten, einer Stunde usw. verwenden.

Da wir diesen Rollup kontinuierlich ausführen, erstellen wir eine Funktion, die das übernimmt. Führen Sie diese Befehle in psql aus, um die Funktion `rollup_http_request` zu erstellen.

```sql
-- initialize to a time long ago
INSERT INTO latest_rollup VALUES ('10-10-1901');

-- function to do the rollup
CREATE OR REPLACE FUNCTION rollup_http_request() RETURNS void AS $$
DECLARE
  curr_rollup_time timestamptz := date_trunc('minute', now());
  last_rollup_time timestamptz := minute from latest_rollup;
BEGIN
  INSERT INTO http_request_1min (
    site_id, ingest_time, request_count,
    success_count, error_count, average_response_time_msec
  ) SELECT
    site_id,
    date_trunc('minute', ingest_time),
    COUNT(1) as request_count,
    SUM(CASE WHEN (status_code between 200 and 299) THEN 1 ELSE 0 END) as success_count,
    SUM(CASE WHEN (status_code between 200 and 299) THEN 0 ELSE 1 END) as error_count,
    SUM(response_time_msec) / COUNT(1) AS average_response_time_msec
  FROM http_request
  -- roll up only data new since last_rollup_time
  WHERE date_trunc('minute', ingest_time) <@
          tstzrange(last_rollup_time, curr_rollup_time, '(]')
  GROUP BY 1, 2;

  -- update the value in latest_rollup so that next time we run the
  -- rollup it will operate on data newer than curr_rollup_time
  UPDATE latest_rollup SET minute = curr_rollup_time;
END;
$$ LANGUAGE plpgsql;
```

Wenn unsere Funktion an Ort und Stelle ist, führen Sie sie aus, um den Rollup der Daten auszuführen:

```sql
SELECT rollup_http_request();
```

Und wenn unsere Daten in voraggregierter Form vorliegen, können wir die Rolluptabelle abfragen, um den gleichen Bericht wie zuvor zu erhalten. Führen Sie Folgendes aus:

```sql
SELECT site_id, ingest_time as minute, request_count,
       success_count, error_count, average_response_time_msec
  FROM http_request_1min
 WHERE ingest_time > date_trunc('minute', now()) - '5 minutes'::interval;
 ```

## <a name="expiring-old-data"></a>Ablauf alter Daten

Durch die Rollups werden die Abfragen schneller, wir müssen aber trotzdem alte Daten ablaufen lassen, um aus dem Ruder laufende Speicherkosten zu vermeiden. Entscheiden Sie einfach, wie lange Sie Daten für die einzelnen Granularitäten aufbewahren möchten, und verwenden Sie Standardabfragen, um abgelaufene Daten zu löschen. Im folgenden Beispiel haben wir beschlossen, Rohdaten einen Tag lang und minütliche Aggregationen einen Monat lang aufzubewahren:

```sql
DELETE FROM http_request WHERE ingest_time < now() - interval '1 day';
DELETE FROM http_request_1min WHERE ingest_time < now() - interval '1 month';
```

In einer Produktionsumgebung könnten Sie diese Abfragen in einer Funktion verpacken und sie jede Minute in einem cron-Auftrag aufrufen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den vorherigen Schritten haben Sie Azure-Ressourcen in einer Servergruppe erstellt. Wenn Sie diese Ressourcen nicht mehr benötigen, löschen Sie die Servergruppe. Drücken Sie die Schaltfläche *Löschen* auf der Seite *Übersicht* für Ihre Servergruppe. Wenn Sie auf einer Popupseite dazu aufgefordert werden, bestätigen Sie den Namen der Servergruppe, und klicken Sie abschließend auf die Schaltfläche *Löschen*.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie eine Hyperscale (Citus)-Servergruppe bereitstellen. Sie haben mithilfe von psql eine Verbindung mit ihr hergestellt, haben ein Schema erstellt und Daten verteilt. Sie haben sowohl gelernt, Daten in Rohform abzurufen als auch diese Daten regelmäßig zu aggregieren, die aggregierten Tabellen abzufragen und alte Daten ablaufen zu lassen.

Lernen Sie als Nächstes die Konzepte von Hyperscale kennen.
> [!div class="nextstepaction"]
> [Hyperscale-Knotentypen](https://aka.ms/hyperscale-concepts)