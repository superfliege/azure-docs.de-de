---
title: 'Schnellstart: Azure Database for PostgreSQL – Hyperscale (Citus) (Vorschauversion)'
description: Schnellstart zum Erstellen und Abfragen verteilter Tabellen in Azure Database for PostgreSQL – Hyperscale (Citus) (Vorschauversion).
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: quickstart
ms.date: 05/06/2019
ms.openlocfilehash: 4271d94f07125a870cc4aa859b01db819d583f40
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406447"
---
# <a name="quickstart-create-an-azure-database-for-postgresql---hyperscale-citus-preview-in-the-azure-portal"></a>Schnellstart: Erstellen einer Azure Database for PostgreSQL – Hyperscale-Servergruppe (Citus) (Vorschauversion) im Azure-Portal

Azure-Datenbank für PostgreSQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare PostgreSQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal eine Azure Database for PostgreSQL – Hyperscale-Servergruppe (Citus) (Vorschauversion) erstellen. Dabei erkunden Sie verteilte Daten: knotenübergreifendes Sharding von Tabellen, Erfassen von Beispieldaten und Ausführen von Abfragen, die auf mehreren Knoten ausgeführt werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-an-azure-database-for-postgresql"></a>Erstellen einer Azure-Datenbank für PostgreSQL

Führen Sie die folgenden Schritte aus, um eine Azure-Datenbank für PostgreSQL-Server zu erstellen:
1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **Azure-Datenbank für PostgreSQL** aus.
3. Klicken Sie als Bereitstellungsoption auf die Schaltfläche **Erstellen** unter **Hyperscale-Servergruppe (Citus) – VORSCHAU**.
4. Geben Sie im Formular für den neuen Server folgende Informationen an:
   - Ressourcengruppe: Klicken Sie auf den Link **Neue erstellen** unterhalb des Textfelds für dieses Feld. Geben Sie einen Namen ein, z.B. **myresourcegroup**.
   - Servergruppenname: Geben Sie einen eindeutigen Namen für die neue Servergruppe ein, der ebenfalls für eine Serverunterdomäne verwendet wird.
   - Administratorbenutzername: Geben Sie einen eindeutigen Benutzernamen ein. Er wird später zum Herstellen der Datenbankverbindung verwendet.
   - Kennwort: Muss mindestens 8 Zeichen lang sein und Zeichen aus drei der folgenden Kategorien enthalten: Großbuchstaben des englischen Alphabets, Kleinbuchstaben des englischen Alphabets, Ziffern (0–9) und nicht alphanumerische Zeichen (!, $, #, % usw.)
   - Standort: Verwenden Sie den Standort, der Ihren Benutzern am nächsten ist, damit sie möglichst schnell auf die Daten zugreifen können.

   > [!IMPORTANT]
   > Der hier angegebene Benutzername bzw. das Kennwort für den Serveradministrator sind erforderlich, um später in diesem Schnellstart die Anmeldung am Server und an den dazugehörigen Datenbanken durchzuführen. Behalten Sie diese Angaben im Kopf, oder notieren Sie sie zur späteren Verwendung.

5. Klicken Sie auf **Servergruppe konfigurieren**. Übernehmen Sie die Einstellungen in diesem Abschnitt unverändert, und klicken Sie auf **Speichern**.
6. Klicken Sie auf **Bewerten + erstellen** und dann auf **Erstellen**, um den Server bereitzustellen. Die Bereitstellung dauert einige Minuten.
7. Die Seite leitet zur Überwachung der Bereitstellung um. Wenn sich der Livestatus von **Ihre Bereitstellung wird ausgeführt** in **Ihre Bereitstellung wurde abgeschlossen** ändert, klicken Sie links auf der Seite auf das Menüelement **Ausgaben**.
8. Die Seite „Ausgaben“ enthält einen Koordinatorhostnamen mit einer Schaltfläche daneben, um den Wert in die Zwischenablage zu kopieren. Notieren Sie diese Informationen zur späteren Verwendung.

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene

Der Dienst Azure Database for PostgreSQL – Hyperscale (Citus) (Vorschauversion) verwendet eine Firewall auf der Serverebene. Standardmäßig hindert die Firewall alle externen Anwendungen und Tools daran, eine Verbindung mit dem Koordinatorknoten und mit internen Datenbanken herzustellen. Sie müssen eine Regel zum Öffnen der Firewall für einen bestimmten IP-Adressbereich hinzufügen.

1. Klicken Sie im Abschnitt **Ausgaben**, in dem Sie zuvor den Hostnamen des Koordinatorknotens kopiert hatten, zurück zum Menüelement **Übersicht**.

2. Dem Namen der Skalierungsgruppe Ihrer Bereitstellung ist „sg-“ vorangestellt. Suchen Sie in der Liste der Ressourcen danach, und klicken Sie darauf.

3. Klicken Sie im Menü auf der linken Seite unter **Sicherheit** auf **Firewall**.

4. Klicken Sie auf den Link **+ Firewallregel für die aktuelle Client-IP-Adresse hinzufügen**. Klicken Sie abschließend auf die Schaltfläche **Speichern**.

5. Klicken Sie auf **Speichern**.

   > [!NOTE]
   > Der Azure-PostgreSQL-Server kommuniziert über Port 5432. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 5432 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbank-Server herstellen, wenn Ihre IT-Abteilung Port 5432 öffnet.
   >

## <a name="connect-to-the-database-using-psql-in-cloud-shell"></a>Herstellen einer Verbindung mit der Datenbank mithilfe von psql in Cloud Shell

Stellen Sie jetzt mit dem Befehlszeilen-Hilfsprogramm [psql](https://www.postgresql.org/docs/current/app-psql.html) eine Verbindung mit dem Azure Database for PostgreSQL-Server her.
1. Starten Sie die Azure Cloud Shell über das Terminalsymbol im oberen Navigationsbereich.

   ![Azure-Datenbank für PostgreSQL – Azure Cloud Shell-Terminalsymbol](./media/quickstart-create-hyperscale-portal/psql-cloud-shell.png)

2. Die Azure Cloud Shell wird in Ihrem Browser geöffnet, sodass Sie Bash-Befehle eingeben können.

   ![Azure-Datenbank für PostgreSQL – Azure Shell-Bash-Eingabeaufforderung](./media/quickstart-create-hyperscale-portal/psql-bash.png)

3. Stellen Sie an der Cloud Shell-Eingabeaufforderung mit den psql-Befehlen eine Verbindung mit Ihrem Azure-Datenbank für PostgreSQL-Server her. Das folgende Format wird verwendet, um mit dem Hilfsprogramm [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) eine Verbindung mit einer Azure-Datenbank für PostgreSQL-Server herzustellen:
   ```bash
   psql --host=<myserver> --username=myadmin --dbname=citus
   ```

   Mit dem folgenden Befehl wird beispielsweise mit den Zugriffsanmeldeinformationen eine Verbindung mit der Standarddatenbank **citus** auf Ihrem PostgreSQL-Server **mydemoserver.postgres.database.azure.com** hergestellt. Geben Sie Ihr Serveradministrator-Kennwort ein, wenn Sie dazu aufgefordert werden.

   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --username=myadmin --dbname=citus
   ```

## <a name="create-and-distribute-tables"></a>Erstellen und Verteilen von Tabellen

Nachdem Sie mithilfe von psql eine Verbindung mit dem Hyperscale-Koordinatorknoten hergestellt haben, können Sie einige grundlegende Aufgaben ausführen.

Innerhalb der Hyperscale-Server gibt es drei Arten von Tabellen:

- Verteilte oder Shardtabellen (zur besseren Leistungsskalierung und Parallelisierung aufgeteilt)
- Verweistabellen (mehrere Kopien beibehalten)
- Lokale Tabellen (häufig für interne Administrationstabellen verwendet)

In diesem Schnellstart liegt der Schwerpunkt in erster Linie auf verteilten Tabellen, mit denen Sie sich hier vertraut machen.

Bei dem Datenmodell, mit dem Sie arbeiten, handelt es sich einfach um Benutzer- und Ereignisdaten aus GitHub. Die Ereignisse umfassen das Erstellen einer Verzweigung, Git-Commits im Zusammenhang mit einer Organisation und vieles mehr.

Sobald Sie über psql eine Verbindung hergestellt haben, können Sie die Tabellen erstellen. Führen Sie in der psql-Konsole Folgendes aus:

```sql
CREATE TABLE github_events
(
    event_id bigint,
    event_type text,
    event_public boolean,
    repo_id bigint,
    payload jsonb,
    repo jsonb,
    user_id bigint,
    org jsonb,
    created_at timestamp
);

CREATE TABLE github_users
(
    user_id bigint,
    url text,
    login text,
    avatar_url text,
    gravatar_id text,
    display_login text
);
```

Das Feld `payload` von `github_events` weist den Datentyp JSONB auf. JSONB ist der JSON-Datentyp im Binärformat in Postgres. Dadurch kann auf einfache Weise ein flexibleres Schema in einer einzelnen Spalte gespeichert werden.

Postgres kann einen `GIN`-Index für diesen Typ erstellen, durch den jeder darin enthaltene Schlüssel und Wert indiziert wird. Mit einem Index kann die Nutzlast mit verschiedenen Bedingungen schnell und einfach abgefragt werden. Erstellen Sie nun eine Reihe von Indizes, bevor Sie die Daten laden. In psql:

```sql
CREATE INDEX event_type_index ON github_events (event_type);
CREATE INDEX payload_index ON github_events USING GIN (payload jsonb_path_ops);
```

Als Nächstes verwenden Sie diese Postgres-Tabellen auf dem Koordinatorknoten und weisen Hyperscale an, diese per Sharding auf die Worker zu verteilen. Zu diesem Zweck führen Sie eine Abfrage für jede Tabelle aus und geben dabei den Schlüssel für das Sharding an. Im aktuellen Beispiel wird ein Sharding sowohl für die Ereignis- als auch die Benutzertabelle nach `user_id` durchgeführt:

```sql
SELECT create_distributed_table('github_events', 'user_id');
SELECT create_distributed_table('github_users', 'user_id');
```

Jetzt können Daten geladen werden. Laden Sie die beiden Beispieldateien [users.csv](https://examples.citusdata.com/users.csv) und [events.csv](https://examples.citusdata.com/events.csv) herunter. Nach dem Herunterladen der Dateien stellen Sie mithilfe von psql eine Verbindung mit der Datenbank her. Achten Sie darauf, dass Sie psql aus dem Verzeichnis ausführen, das die heruntergeladenen Dateien enthält. Laden Sie die Daten mit dem Befehl `\copy`:

```sql
\copy github_events from 'events.csv' WITH CSV
\copy github_users from 'users.csv' WITH CSV
```

## <a name="run-queries"></a>Ausführen von Abfragen

Nun kommt der interessante Teil – das Ausführen von Abfragen. Beginnen Sie mit einer einfachen Abfrage vom Typ `count (*)`, um festzustellen, wie viele Daten geladen wurden:

```sql
SELECT count(*) from github_events;
```

Das hat gut funktioniert. Auf diese Art der Aggregation wird in Kürze noch genauer eingegangen, doch jetzt sehen Sie sich erst einmal ein paar andere Abfragen an. In der JSONB-Spalte `payload` sind so einige Daten enthalten, doch variieren sie je nach Ereignistyp. `PushEvent`-Ereignisse enthalten eine Größe, die die Anzahl der eindeutigen Commits für den Pushvorgang beinhaltet. Hiermit kann die Gesamtzahl von Commits pro Stunde ermittelt werden:

```sql
SELECT date_trunc('hour', created_at) AS hour,
       sum((payload->>'distinct_size')::int) AS num_commits
FROM github_events
WHERE event_type = 'PushEvent'
GROUP BY hour
ORDER BY hour;
```

Bisher waren bei den Abfragen ausschließlich „github\_events“ einbezogen, doch können diese Informationen auch mit „github\_users“ kombiniert werden. Da ein Sharding sowohl für Benutzer als auch Ereignisse mit demselben Bezeichner (`user_id`) durchgeführt wurde, werden die Zeilen beider Tabellen mit übereinstimmenden Benutzer-IDs auf denselben Datenbankknoten [zusammengestellt](https://docs.citusdata.com/en/stable/sharding/data_modeling.html#colocation) und können auf einfache Weise verknüpft werden.

Bei einer Verknüpfung basierend auf `user_id` kann Hyperscale deren Ausführung per Pushvorgang in Shards für die parallele Ausführung auf Workerknoten übertragen. Suchen Sie beispielsweise nach den Benutzern, die die größte Anzahl von Repositorys erstellt haben:

```sql
SELECT login, count(*)
FROM github_events ge
JOIN github_users gu
ON ge.user_id = gu.user_id
WHERE event_type = 'CreateEvent' AND
      payload @> '{"ref_type": "repository"}'
GROUP BY login
ORDER BY count(*) DESC;
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den vorherigen Schritten haben Sie Azure-Ressourcen in einer Servergruppe erstellt. Wenn Sie diese Ressourcen nicht mehr benötigen, löschen Sie die Servergruppe. Klicken Sie auf der Seite **Übersicht** für Ihre Servergruppe auf die Schaltfläche **Löschen**. Wenn Sie auf einer Popupseite dazu aufgefordert werden, bestätigen Sie den Namen der Servergruppe, und klicken Sie abschließend auf die Schaltfläche **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie eine Hyperscale-Servergruppe (Citus) bereitstellen. Sie haben mithilfe von psql eine Verbindung mit ihr hergestellt, haben ein Schema erstellt und Daten verteilt.

Als Nächstes arbeiten Sie ein Tutorial zum Erstellen skalierbarer mehrinstanzenfähiger Anwendungen durch.
> [!div class="nextstepaction"]
> [Entwerfen einer mehrinstanzenfähigen Datenbank](https://aka.ms/hyperscale-tutorial-multi-tenant)
