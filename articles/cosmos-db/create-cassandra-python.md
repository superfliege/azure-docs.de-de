---
title: 'Schnellstart: Cassandra-API mit Python – Azure Cosmos DB'
description: In diesem Schnellstart erfahren Sie, wie Sie mithilfe der Apache Cassandra-API von Azure Cosmos DB eine Profilanwendung mit Python erstellen.
services: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.custom: quick start connect, mvc
ms.devlang: python
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: 4712c0b40209cd6d40703176f95a80f491d0364c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46979099"
---
# <a name="quickstart-build-a-cassandra-app-with-python-and-azure-cosmos-db"></a>Schnellstartanleitung: Erstellen einer Cassandra-App mit Python und Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](create-cassandra-dotnet.md)
> * [Java](create-cassandra-java.md)
> * [Node.js](create-cassandra-nodejs.md)
> * [Python](create-cassandra-python.md)
>  

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Python und der [Cassandra-API](cassandra-introduction.md) von Azure Cosmos DB eine Profil-App erstellen, indem Sie ein Beispiel von GitHub klonen. Außerdem wird in dieser Schnellstartanleitung gezeigt, wie Sie das webbasierte Azure-Portal verwenden, um ein Azure Cosmos DB-Konto zu erstellen.

Azure Cosmos DB ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Tabellen-, Schlüssel-Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Azure Cosmos DB bietet.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] Alternativ können Sie [Azure Cosmos DB ohne Azure-Abonnement testen](https://azure.microsoft.com/try/cosmosdb/) – kostenlos und ohne Verpflichtung.

Zudem benötigen Sie:
* [Python](https://www.python.org/downloads/) (Version 2.7.14)
* [Git-Client](http://git-scm.com/)
* [Python-Treiber für Apache Cassandra](https://github.com/datastax/python-driver)

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

Vor dem Erstellen einer Dokumentdatenbank müssen Sie mit Azure Cosmos DB ein Cassandra-Konto erstellen.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

In diesem Schritt klonen wir eine Cassandra-API-App aus GitHub, legen die Verbindungszeichenfolge fest und führen die App aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie eine Eingabeaufforderung. Erstellen Sie einen neuen Ordner namens `git-samples`. Schließen Sie dann die Eingabeaufforderung.

    ```bash
    md "C:\git-samples"
    ```

2. Öffnen Sie ein Git-Terminalfenster (z.B. git bash), und verwenden Sie den Befehl `cd`, um in den neuen Ordner zu gelangen und dort die Beispiel-App zu installieren.

    ```bash
    cd "C:\git-samples"
    ```

3. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git
    ```

## <a name="review-the-code"></a>Überprüfen des Codes

Dieser Schritt ist optional. Wenn Sie erfahren möchten, wie der Code die Datenbankressourcen erstellt, können Sie sich die folgenden Codeausschnitte ansehen. Die Codeausschnitte stammen alle aus der Datei „pyquickstart.py“. Andernfalls können Sie mit [Aktualisieren der Verbindungszeichenfolge](#update-your-connection-string) fortfahren. 

* Die Werte für Benutzername und Kennwort wurden auf der Seite „Verbindungszeichenfolge“ des Azure-Portals festgelegt. `path\to\cert` gibt den Pfad für das X.509-Zertifikat an. 

   ```python
    ssl_opts = {
            'ca_certs': 'path\to\cert',
            'ssl_version': ssl.PROTOCOL_TLSv1_2
            }
    auth_provider = PlainTextAuthProvider( username=cfg.config['username'], password=cfg.config['password'])
    cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider, ssl_options=ssl_opts)
    session = cluster.connect()
   
   ```

* `cluster` wird mit contactPoint-Informationen initialisiert. Der Kontaktpunkt (contactPoint) wird aus dem Azure-Portal abgerufen.

    ```python
   cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider)
    ```

* `cluster` stellt eine Verbindung mit der Cassandra-API von Azure Cosmos DB her.

    ```python
    session = cluster.connect()
    ```

* Ein neuer Keyspace wird erstellt.

    ```python
   session.execute('CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter1\' : \'1\' }')
    ```

* Eine neue Tabelle wird erstellt.

   ```
   session.execute('CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)');
   ```

* Schlüssel-Wert-Entitäten werden eingefügt.

    ```Python
    insert_data = session.prepare("INSERT INTO  uprofile.user  (user_id, user_name , user_bcity) VALUES (?,?,?)")
    batch = BatchStatement()
    batch.add(insert_data, (1, 'LyubovK', 'Dubai'))
    batch.add(insert_data, (2, 'JiriK', 'Toronto'))
    batch.add(insert_data, (3, 'IvanH', 'Mumbai'))
    batch.add(insert_data, (4, 'YuliaT', 'Seattle'))
    ....
    session.execute(batch)
    ```

* Abfrage zum Abrufen aller Schlüsselwerte.

    ```Python
    rows = session.execute('SELECT * FROM uprofile.user')
    ```  
    
* Abfrage zum Abrufen eines Schlüsselwerts.

    ```Python
    
    rows = session.execute('SELECT * FROM uprofile.user where user_id=1')
    ```  

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren. Die Angabe der Verbindungszeichenfolge ermöglicht Ihrer App die Kommunikation mit Ihrer gehosteten Datenbank.

1. Wählen Sie im [Azure-Portal](http://portal.azure.com/) die Option **Verbindungszeichenfolge** aus. 

    Verwenden Sie die Schaltfläche ![Schaltfläche „Kopieren“](./media/create-cassandra-python/copy.png) auf der rechten Seite des Bildschirms, um den obersten Wert (den Kontaktpunkt) zu kopieren.

    ![Anzeigen und Kopieren eines Benutzernamens, Kennworts und Kontaktpunkts für den Zugriff im Azure-Portal auf dem Blatt „Verbindungszeichenfolge“](./media/create-cassandra-python/keys.png)

2. Öffnen Sie die Datei `config.py` . 

3. Ersetzen Sie `<FILLME>` in Zeile 10 durch den Kontaktpunktwert aus dem Portal.

    Zeile 10 sollte nun in etwa wie folgt aussehen: 

    `'contactPoint': 'cosmos-db-quickstarts.cassandra.cosmosdb.azure.com:10350'`

4. Kopieren Sie den Wert für den Benutzernamen aus dem Portal, und überschreiben Sie damit `<FILLME>` in Zeile 6.

    Zeile 6 sollte nun in etwa wie folgt aussehen: 

    `'username': 'cosmos-db-quickstart',`
    
5. Kopieren Sie den Wert für das Kennwort aus dem Portal, und überschreiben Sie damit `<FILLME>` in Zeile 8.

    Zeile 8 sollte nun in etwa wie folgt aussehen:

    `'password' = '2Ggkr662ifxz2Mg==`';`

6. Speichern Sie die Datei „config.py“.
    
## <a name="use-the-x509-certificate"></a>Verwenden des X.509-Zertifikats

1. Laden Sie das Zertifikat von „Baltimore CyberTrust Root“ unter [https://cacert.omniroot.com/bc2025.crt](https://cacert.omniroot.com/bc2025.crt) lokal herunter. Benennen Sie die Datei mit der Dateierweiterung `.cer` um.

   Das Zertifikat weist die Seriennummer `02:00:00:b9` und den SHA1-Fingerabdruck `d4🇩🇪20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` auf.

2. Öffnen Sie `pyquickstart.py`, und ändern Sie `path\to\cert` in einen Verweis auf das neue Zertifikat.

3. Speichern Sie `pyquickstart.py`.

## <a name="run-the-python-app"></a>Ausführen der Python-App

1. Verwenden Sie den Befehl cd im Git-Terminal, um in den Ordner `azure-cosmos-db-cassandra-python-getting-started` zu wechseln. 

2. Führen Sie die folgenden Befehle aus, um die erforderlichen Module zu installieren:

    ```python
    python -m pip install cassandra-driver
    python -m pip install prettytable
    python -m pip install requests
    python -m pip install pyopenssl
    ```

2. Führen Sie den folgenden Befehl aus, um Ihre Node-Anwendung zu starten:

    ```
    python pyquickstart.py
    ```

3. Überprüfen Sie die Ergebnisse über die Befehlszeile.

    Drücken Sie STRG+C, um die Programmausführung zu beenden und das Konsolenfenster zu schließen. 

    ![Anzeigen und Überprüfen der Ausgabe](./media/create-cassandra-python/output.png)
    
4. Öffnen Sie im Azure-Portal den **Daten-Explorer**, um diese neuen Daten abzufragen, zu ändern und zu verwenden. 

    ![Anzeigen der Daten im Daten-Explorer](./media/create-cassandra-python/data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto erstellen, einen Container mit dem Daten-Explorer erstellen und eine App ausführen. Jetzt können Sie zusätzliche Daten in Ihr Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB: Import Cassandra data](cassandra-import-data.md) (Azure Cosmos DB: Importieren von Cassandra-Daten)

