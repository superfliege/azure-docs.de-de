---
title: Tutorial zum Entwerfen einer mehrmandantenfähigen Datenbank mit Azure Database for PostgreSQL – Hyperscale (Citus) (Vorschau)
description: In diesem Tutorial erfahren Sie, wie Sie verteilte Tabellen in Azure Database for PostgreSQL Hyperscale (Citus) (Vorschau) erstellen, auffüllen und abfragen.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 05/06/2019
ms.openlocfilehash: b135baf73e21cd524b6e8fad35452362f36cf0c0
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079545"
---
# <a name="tutorial-design-a-multi-tenant-database-by-using-azure-database-for-postgresql--hyperscale-citus-preview"></a>Tutorial: Entwerfen einer mehrmandantenfähigen Datenbank mithilfe von Azure Database for PostgreSQL – Hyperscale (Citus) (Vorschau)

In diesem Tutorial verwenden Sie Azure Database for PostgreSQL – Hyperscale (Citus) (Vorschau), um Folgendes zu lernen:

> [!div class="checklist"]
> * Erstellen einer Hyperscale (Citus)-Servergruppe
> * Verwenden des psql-Hilfsprogramms zum Erstellen eines Schemas
> * Knotenübergreifende Shard-Tabellen
> * Erfassen von Beispieldaten
> * Abfragen von Mandantendaten
> * Teilen von Daten zwischen Mandanten
> * Mandantenweises Anpassen des Schemas

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
   - Name der Servergruppe: Geben Sie einen eindeutigen Namen für die neue Servergruppe ein, der ebenfalls für eine Serverunterdomäne verwendet wird.
   - Benutzername des Administrators: Geben Sie einen eindeutigen Benutzernamen ein, er wird später zum Herstellen der Datenbankverbindung verwendet.
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

   ![Azure-Datenbank für PostgreSQL – Azure Cloud Shell-Terminalsymbol](./media/tutorial-design-database-hyperscale-multi-tenant/psql-cloud-shell.png)

2. Die Azure Cloud Shell wird in Ihrem Browser geöffnet, sodass Sie Bash-Befehle eingeben können.

   ![Azure-Datenbank für PostgreSQL – Azure Shell-Bash-Eingabeaufforderung](./media/tutorial-design-database-hyperscale-multi-tenant/psql-bash.png)

3. Stellen Sie an der Cloud Shell-Eingabeaufforderung mit den psql-Befehlen eine Verbindung mit Ihrem Azure-Datenbank für PostgreSQL-Server her. Das folgende Format wird verwendet, um mit dem Hilfsprogramm [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) eine Verbindung mit einer Azure-Datenbank für PostgreSQL-Server herzustellen:
   ```bash
   psql --host=<myserver> --username=myadmin --dbname=citus
   ```

   Mit dem folgenden Befehl wird beispielsweise mit den Zugriffsanmeldeinformationen eine Verbindung mit der Standarddatenbank **citus** auf Ihrem PostgreSQL-Server **mydemoserver.postgres.database.azure.com** hergestellt. Geben Sie Ihr Serveradministrator-Kennwort ein, wenn Sie dazu aufgefordert werden.

   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --username=myadmin --dbname=citus
   ```

## <a name="use-psql-utility-to-create-a-schema"></a>Verwenden des psql-Hilfsprogramms zum Erstellen eines Schemas

Nach dem Herstellen der Verbindung mit der Azure Database for PostgreSQL – Hyperscale (Citus) (Vorschau) mithilfe von psql können Sie ein paar einfache Aufgaben ausführen. Dieses Tutorial führt Sie durch die Erstellung einer Web-App, die Werbetreibenden das Nachverfolgen ihrer Kampagnen ermöglicht.

Die App kann von mehreren Firmen verwendet werden, erstellen wir also eine Tabelle, die die Firmen enthält, und eine weitere für ihre Kampagnen. Führen Sie in der psql-Konsole diese Befehle aus:

```sql
CREATE TABLE companies (
  id bigserial PRIMARY KEY,
  name text NOT NULL,
  image_url text,
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL
);

CREATE TABLE campaigns (
  id bigserial,
  company_id bigint REFERENCES companies (id),
  name text NOT NULL,
  cost_model text NOT NULL,
  state text NOT NULL,
  monthly_budget bigint,
  blacklisted_site_urls text[],
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL,

  PRIMARY KEY (company_id, id)
);
```

Jede Kampagne zahlt für die Ausführung von Werbeeinblendungen. Fügen Sie außerdem eine Tabelle für Werbeeinblendungen hinzu, indem Sie nach dem Code oben den folgenden Code in psql ausführen:

```sql
CREATE TABLE ads (
  id bigserial,
  company_id bigint,
  campaign_id bigint,
  name text NOT NULL,
  image_url text,
  target_url text,
  impressions_count bigint DEFAULT 0,
  clicks_count bigint DEFAULT 0,
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, campaign_id)
    REFERENCES campaigns (company_id, id)
);
```

Schließlich verfolgen wir die Statistiken über Klicks und Eindrücke für die einzelnen Werbeeinblendungen nach:

```sql
CREATE TABLE clicks (
  id bigserial,
  company_id bigint,
  ad_id bigint,
  clicked_at timestamp without time zone NOT NULL,
  site_url text NOT NULL,
  cost_per_click_usd numeric(20,10),
  user_ip inet NOT NULL,
  user_data jsonb NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, ad_id)
    REFERENCES ads (company_id, id)
);

CREATE TABLE impressions (
  id bigserial,
  company_id bigint,
  ad_id bigint,
  seen_at timestamp without time zone NOT NULL,
  site_url text NOT NULL,
  cost_per_impression_usd numeric(20,10),
  user_ip inet NOT NULL,
  user_data jsonb NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, ad_id)
    REFERENCES ads (company_id, id)
);
```

Sie können jetzt die neu erstellten Tabellen in der Liste der Tabellen in psql anzeigen, indem Sie dies ausführen:

```postgres
\dt
```

Mehrmandantenfähige Anwendungen können Eindeutigkeit nur mandantenweise durchsetzen; dies ist der Grund dafür, warum alle Primär- und Fremdschlüssel die Firmen-ID enthalten.

## <a name="shard-tables-across-nodes"></a>Knotenübergreifende Shard-Tabellen

Eine Hyperscalebereitstellung speichert Tabellenzeilen basierend auf dem Wert einer vom Benutzer zugeordneten Spalte auf verschiedenen Knoten. Diese „Verteilungsspalte“ gibt an, welcher Mandant welche Zeilen besitzt.

Legen wir die Verteilungsspalte auf „company\_id“ fest, den Mandantenbezeichner. Führen Sie in psql diese Funktionen aus:

```sql
SELECT create_distributed_table('companies',   'id');
SELECT create_distributed_table('campaigns',   'company_id');
SELECT create_distributed_table('ads',         'company_id');
SELECT create_distributed_table('clicks',      'company_id');
SELECT create_distributed_table('impressions', 'company_id');
```

## <a name="ingest-sample-data"></a>Erfassen von Beispieldaten

Laden Sie jetzt außerhalb von psql, an einer normalen Befehlszeile, Beispieldatasets herunter:

```bash
for dataset in companies campaigns ads clicks impressions geo_ips; do
  curl -O https://examples.citusdata.com/mt_ref_arch/${dataset}.csv
done
```

Führen Sie – jetzt wieder in psql – ein Massenladen der Daten aus. Achten Sie darauf, psql in dem gleichen Verzeichnis auszuführen, in das Sie die Datendateien heruntergeladen hatten.

```sql
\copy companies from 'companies.csv' with csv
\copy campaigns from 'campaigns.csv' with csv
\copy ads from 'ads.csv' with csv
\copy clicks from 'clicks.csv' with csv
\copy impressions from 'impressions.csv' with csv
```

Diese Daten werden jetzt über die Workerknoten verteilt.

## <a name="query-tenant-data"></a>Abfragen von Mandantendaten

Wenn die Anwendung Daten für einen einzelnen Mandanten anfordert, kann die Datenbank die Abfrage auf einem einzelnen Workerknoten ausführen. Abfragen für einzelne Mandanten führen eine Filterung nach einer einzelnen Mandanten-ID durch. Beispielsweise filtert die folgende Abfrage `company_id = 5` nach Werbeeinblendungen und Eindrücken. Versuchen Sie, sie in psql auszuführen, um die Ergebnisse anzuzeigen.

```sql
SELECT a.campaign_id,
       RANK() OVER (
         PARTITION BY a.campaign_id
         ORDER BY a.campaign_id, count(*) desc
       ), count(*) as n_impressions, a.id
  FROM ads as a
  JOIN impressions as i
    ON i.company_id = a.company_id
   AND i.ad_id      = a.id
 WHERE a.company_id = 5
GROUP BY a.campaign_id, a.id
ORDER BY a.campaign_id, n_impressions desc;
```

## <a name="share-data-between-tenants"></a>Teilen von Daten zwischen Mandanten

Bis jetzt wurden alle Tabellen nach `company_id` verteilt, aber einige Daten „gehören“ nicht selbstverständlich zu einem bestimmten Mandanten und können geteilt werden. Beispielsweise ist es denkbar, dass alle Firmen auf der Plattform für Beispielwerbeeinblendungen geografische Informationen für ihre Zielgruppe auf der Grundlage der IP-Adressen abrufen möchten.

Erstellen Sie eine Tabelle zum Speichern von gemeinsam verwendeten geografischen Informationen. Führen Sie dies in psql aus:

```sql
CREATE TABLE geo_ips (
  addrs cidr NOT NULL PRIMARY KEY,
  latlon point NOT NULL
    CHECK (-90  <= latlon[0] AND latlon[0] <= 90 AND
           -180 <= latlon[1] AND latlon[1] <= 180)
);
CREATE INDEX ON geo_ips USING gist (addrs inet_ops);
```

Machen Sie als Nächstes `geo_ips` zu einer „Verweistabelle“, und speichern Sie eine Kopie der Tabelle auf jedem Workerknoten.

```sql
SELECT create_reference_table('geo_ips');
```

Laden Sie sie mit Beispieldaten. Achten Sie darauf, dies in psql in dem Verzeichnis auszuführen, in das Sie das Dataset heruntergeladen hatten.

```sql
\copy geo_ips from 'geo_ips.csv' with csv
```

Das Verknüpfen der Klicktabelle mit „geo\_ips“ ist auf allen Knoten wirksam.
Hier ist eine Verknüpfung, mit der Sie die Standorte aller Personen finden, die auf eine Werbeeinblendung geklickt haben:
290. Versuchen Sie, die Abfrage in psql auszuführen.

```sql
SELECT c.id, clicked_at, latlon
  FROM geo_ips, clicks c
 WHERE addrs >> c.user_ip
   AND c.company_id = 5
   AND c.ad_id = 290;
```

## <a name="customize-the-schema-per-tenant"></a>Mandantenweises Anpassen des Schemas

Für jeden Mandanten kann es erforderlich sein, besondere Informationen zu speichern, die von anderen nicht benötigt werden. Allerdings verwenden alle Mandanten eine gemeinsame Infrastruktur mit einem identischen Datenbankschema. Wo können die zusätzlichen Daten untergebracht werden?

Ein Trick besteht darin, einen Spaltentyp mit offenem Ende zu verwenden, wie JSONB von PostgreSQL.  Unser Schema weist ein JSONB-Feld in `clicks` mit dem Namen `user_data` auf.
Eine Firma (z.B. Firma 5) kann die Spalte verwenden, um nachzuverfolgen, ob der Benutzer von einem mobilen Gerät aus zugreift.

Mit dieser Abfrage finden Sie heraus, wer öfter klickt – mobile oder herkömmliche Benutzer:

```sql
SELECT
  user_data->>'is_mobile' AS is_mobile,
  count(*) AS count
FROM clicks
WHERE company_id = 5
GROUP BY user_data->>'is_mobile'
ORDER BY count DESC;
```

Wir können diese Abfrage für eine einzelne Firma optimieren, indem wir einen [Teilindex](https://www.postgresql.org/docs/current/static/indexes-partial.html) erstellen.

```sql
CREATE INDEX click_user_data_is_mobile
ON clicks ((user_data->>'is_mobile'))
WHERE company_id = 5;
```

Allgemeiner ausgedrückt können wir [GIN-Indizes](https://www.postgresql.org/docs/current/static/gin-intro.html) für alle Schlüssel und Wert in der Spalte erstellen.

```sql
CREATE INDEX click_user_data
ON clicks USING gin (user_data);

-- this speeds up queries like, "which clicks have
-- the is_mobile key present in user_data?"

SELECT id
  FROM clicks
 WHERE user_data ? 'is_mobile'
   AND company_id = 5;
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den vorherigen Schritten haben Sie Azure-Ressourcen in einer Servergruppe erstellt. Wenn Sie diese Ressourcen nicht mehr benötigen, löschen Sie die Servergruppe. Drücken Sie die Schaltfläche *Löschen* auf der Seite *Übersicht* für Ihre Servergruppe. Wenn Sie auf einer Popupseite dazu aufgefordert werden, bestätigen Sie den Namen der Servergruppe, und klicken Sie abschließend auf die Schaltfläche *Löschen*.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie eine Hyperscale (Citus)-Servergruppe bereitstellen. Sie haben mithilfe von psql eine Verbindung mit ihr hergestellt, haben ein Schema erstellt und Daten verteilt. Sie haben gelernt, Daten sowohl innerhalb von als auch zwischen Mandanten abzufragen und das Schema mandantenweise anzupassen.

Lernen Sie als Nächstes die Konzepte von Hyperscale kennen.
> [!div class="nextstepaction"]
> [Hyperscale-Knotentypen](https://aka.ms/hyperscale-concepts)
