---
title: 'Tutorial: Durchführen von ETL-Vorgängen mithilfe von Azure Databricks'
description: Hier erfahren Sie, wie Sie Daten aus Data Lake Store in Azure Databricks extrahieren, transformieren und anschließend in Azure SQL Data Warehouse laden.
services: azure-databricks
author: nitinme
ms.author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.custom: mvc
ms.topic: tutorial
ms.workload: Active
ms.date: 07/26/2018
ms.openlocfilehash: c416937f98f6bcb49f86fce18213ca4ed349c513
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902058"
---
# <a name="tutorial-extract-transform-and-load-data-using-azure-databricks"></a>Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Azure Databricks

In diesem Tutorial führen Sie mithilfe von Azure Databricks einen ETL-Vorgang (Extrahieren, Transformieren und Laden) für Daten durch. Sie extrahieren Daten aus Azure Data Lake Store in Azure Databricks, führen Transformationen für die Daten in Azure Databricks aus und laden die transformierten Daten anschließend in Azure SQL Data Warehouse. 

Bei den Schritten in diesem Tutorial wird für die Datenübertragung an Azure Databricks der SQL Data Warehouse-Connector für Azure Databricks verwendet. Dieser Connector verwendet wiederum Azure Blob Storage als temporären Speicher für die Daten, die zwischen einem Azure Databricks-Cluster und Azure SQL Data Warehouse übertragen werden.

In der folgenden Abbildung ist der Anwendungsfluss dargestellt:

![Azure Databricks mit Data Lake Store und SQL Data Warehouse](./media/databricks-extract-load-sql-data-warehouse/databricks-extract-transform-load-sql-datawarehouse.png "Azure Databricks mit Data Lake Store und SQL Data Warehouse")

Dieses Tutorial enthält die folgenden Aufgaben: 

> [!div class="checklist"]
> * Erstellen eines Azure Databricks-Arbeitsbereichs
> * Erstellen eines Spark-Clusters in Azure Databricks
> * Erstellen eines Azure Data Lake-Speicherkontos
> * Hochladen von Daten in Azure Data Lake Store
> * Erstellen eines Notebooks in Azure Databricks
> * Extrahieren von Daten aus Data Lake Store
> * Transformieren von Daten in Azure Databricks
> * Laden von Daten in Azure SQL Data Warehouse

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich zunächst, dass die folgenden Anforderungen erfüllt sind bzw. dass Folgendes vorhanden ist:
- Erstellen Sie eine Azure SQL Data Warehouse-Instanz, erstellen Sie eine Firewallregel auf Serverebene, und stellen Sie als Serveradministrator eine Verbindung mit dem Server her. Führen Sie dazu die unter [Schnellstart: Erstellen und Abfragen einer Azure SQL Data Warehouse-Instanz im Azure-Portal](../sql-data-warehouse/create-data-warehouse-portal.md) angegebenen Schritte aus.
- Erstellen Sie einen Datenbank-Hauptschlüssel für die Azure SQL Data Warehouse-Instanz. Führen Sie dazu die unter [Erstellen eines Datenbank-Hauptschlüssels](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key) angegebenen Schritte aus.
- Erstellen Sie ein Azure Blob Storage-Konto und einen Container darin. Rufen Sie außerdem den Zugriffsschlüssel für das Speicherkonto ab. Führen Sie dazu die unter [Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs über das Azure-Portal](../storage/blobs/storage-quickstart-blobs-portal.md) angegebenen Schritte aus.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-an-azure-databricks-workspace"></a>Erstellen eines Azure Databricks-Arbeitsbereichs

In diesem Abschnitt erstellen Sie einen Azure Databricks-Arbeitsbereich über das Azure-Portal. 

1. Klicken Sie im Azure-Portal auf **Ressource erstellen** > **Daten + Analysen** > **Azure Databricks**.

    ![Databricks im Azure-Portal](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-on-portal.png "Databricks im Azure-Portal")

3. Geben Sie unter **Azure Databricks-Dienst** die Werte für die Erstellung eines Databricks-Arbeitsbereichs an.

    ![Erstellen eines Azure Databricks-Arbeitsbereichs](./media/databricks-extract-load-sql-data-warehouse/create-databricks-workspace.png "Erstellen eines Azure Databricks-Arbeitsbereichs")

    Geben Sie außerdem die folgenden Werte an: 
     
    |Eigenschaft  |BESCHREIBUNG  |
    |---------|---------|
    |**Arbeitsbereichsname**     | Geben Sie einen Namen für Ihren Databricks-Arbeitsbereich an.        |
    |**Abonnement**     | Wählen Sie in der Dropdownliste Ihr Azure-Abonnement aus.        |
    |**Ressourcengruppe**     | Geben Sie an, ob Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden möchten. Eine Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. Weitere Informationen finden Sie in der [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). |
    |**Location**     | Wählen Sie **USA, Osten 2** aus. Informationen zu weiteren verfügbaren Regionen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).        |
    |**Tarif**     |  Wählen Sie zwischen **Standard** und **Premium**. Weitere Informationen zu diesen Tarifen, finden Sie unter [Azure Databricks – Preise](https://azure.microsoft.com/pricing/details/databricks/).       |

    Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie anschließend auf **Erstellen**.

4. Die Kontoerstellung dauert einige Minuten. Während der Erstellung des Kontos wird im Portal auf der rechten Seite die Kachel **Submitting deployment for Azure Databricks** (Bereitstellung für Azure Databricks wird übermittelt) angezeigt. Möglicherweise müssen Sie im Dashboard nach rechts scrollen, um die Kachel zu sehen. Am oberen Bildschirmrand wird auch eine Statusanzeige angezeigt. Sie können den Status beider Bereiche beobachten.

    ![Kachel zur Bereitstellung von Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-deployment-tile.png "Kachel zur Bereitstellung von Databricks")

## <a name="create-a-spark-cluster-in-databricks"></a>Erstellen eines Spark-Clusters in Databricks

1. Navigieren Sie im Azure-Portal zum erstellten Databricks-Arbeitsbereich, und klicken Sie auf **Launch Workspace** (Arbeitsbereich starten).

2. Sie werden zum Azure Databricks-Portal weitergeleitet. Klicken Sie im Portal auf **Cluster**.

    ![Databricks in Azure](./media/databricks-extract-load-sql-data-warehouse/databricks-on-azure.png "Databricks in Azure")

3. Geben Sie auf der Seite **Neuer Cluster** die erforderlichen Werte an, um einen Cluster zu erstellen.

    ![Erstellen eines Databricks-Spark-Clusters in Azure](./media/databricks-extract-load-sql-data-warehouse/create-databricks-spark-cluster.png "Erstellen eines Databricks-Spark-Clusters in Azure")

    Übernehmen Sie alle anderen Standardwerte bis auf die folgenden Werte:

    * Geben Sie einen Namen für den Cluster ein.
    * Erstellen Sie im Rahmen dieses Artikels einen Cluster mit der Runtime **4.0**. 
    * Aktivieren Sie das Kontrollkästchen **Terminate after ____ minutes of inactivity** (Nach ___ Minuten Inaktivität beenden). Geben Sie an, nach wie vielen Minuten der Cluster beendet werden soll, wenn er nicht verwendet wird.
    
    Klicken Sie auf **Cluster erstellen**. Sobald der Cluster ausgeführt wird, können Sie Notizbücher an den Cluster anfügen und Spark-Aufträge ausführen.

## <a name="create-an-azure-data-lake-store-account"></a>Erstellen eines Azure Data Lake-Speicherkontos

In diesem Abschnitt erstellen Sie ein Data Lake Store-Konto und ordnen ihm einen Azure Active Directory-Dienstprinzipal zu. Dieser Dienstprinzipal wird im weiteren Verlauf des Tutorials in Azure Databricks für den Zugriff auf Azure Data Lake Store verwendet. 

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Ressource erstellen** > **Speicher** > **Data Lake Store**.
3. Geben Sie auf dem Blatt **Neuer Data Lake Store** die Werte wie im folgenden Screenshot gezeigt an:
   
    ![Erstellen eines neuen Azure Data Lake Store-Kontos](./media/databricks-extract-load-sql-data-warehouse/create-new-datalake-store.png "Erstellen eines neuen Azure Data Lake-Kontos")

    Geben Sie außerdem die folgenden Werte an: 
     
    |Eigenschaft  |BESCHREIBUNG  |
    |---------|---------|
    |**Name**     | Geben Sie einen eindeutigen Namen für das Data Lake Store-Konto ein.        |
    |**Abonnement**     | Wählen Sie in der Dropdownliste Ihr Azure-Abonnement aus.        |
    |**Ressourcengruppe**     | Wählen Sie für dieses Tutorial die gleiche Ressourcengruppe aus, die Sie auch beim Erstellen des Azure Databricks-Arbeitsbereichs verwendet haben.  |
    |**Location**     | Wählen Sie **USA, Osten 2** aus.  |
    |**Preispaket**     |  Klicken Sie auf **Nutzungsbasierte Bezahlung**. |
    | **Verschlüsselungseinstellungen** | Behalten Sie die Standardeinstellungen bei. |

    Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie anschließend auf **Erstellen**.

Als Nächstes erstellen Sie einen Azure Active Directory-Dienstprinzipal und ordnen ihn dem erstellten Data Lake Store-Konto zu.

### <a name="create-an-azure-active-directory-service-principal"></a>Erstellen eines Azure Active Directory-Dienstprinzipals
   
1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Alle Dienste**, und suchen Sie nach **Azure Active Directory**.

2. Wählen Sie **App-Registrierungen** aus.

   ![App-Registrierungen auswählen](./media/databricks-extract-load-sql-data-warehouse/select-app-registrations.png)

3. Wählen Sie **Registrierung einer neuen Anwendung** aus.

   ![App hinzufügen](./media/databricks-extract-load-sql-data-warehouse/select-add-app.png)

4. Geben Sie einen Namen und eine URL für die Anwendung an. Wählen Sie als Typ für die zu erstellende Anwendung **Web-App/API** aus. Geben Sie eine Anmelde-URL an, und klicken Sie auf **Erstellen**.

   ![Anwendung benennen](./media/databricks-extract-load-sql-data-warehouse/create-app.png)

Um in Azure Databricks auf das Data Lake Store-Konto zugreifen zu können, benötigen Sie folgende Werte für den erstellten Azure Active Directory Dienstprinzipal:
- Anwendungs-ID
- Authentifizierungsschlüssel
- Mandanten-ID

In den folgenden Abschnitten rufen Sie diese Werte für den zuvor erstellten Azure Active Directory-Dienstprinzipal ab.

### <a name="get-application-id-and-authentication-key-for-the-service-principal"></a>Abrufen der Anwendungs-ID und des Authentifizierungsschlüssels für den Dienstprinzipal

Beim programmgesteuerten Anmelden benötigen Sie die ID für Ihre Anwendung und einen Authentifizierungsschlüssel. Führen Sie die folgenden Schritte aus, um diese Werte abzurufen:

1. Wählen Sie in Azure Active Directory unter **App-Registrierungen** Ihre Anwendung aus.

   ![Anwendung auswählen](./media/databricks-extract-load-sql-data-warehouse/select-app.png)

2. Kopieren Sie die **Anwendungs-ID**, und speichern Sie sie in Ihrem Anwendungscode. In einigen [Beispielanwendungen](#log-in-as-the-application) wird dieser Wert als „Client-ID“ bezeichnet.

   ![Client-ID](./media/databricks-extract-load-sql-data-warehouse/copy-app-id.png)

3. Klicken Sie zum Generieren eines Authentifizierungsschlüssels auf **Einstellungen**.

   ![Klicken auf „Einstellungen“](./media/databricks-extract-load-sql-data-warehouse/select-settings.png)

4. Wählen Sie zum Generieren eines Authentifizierungsschlüssels die Option **Schlüssel** aus.

   ![Schlüssel auswählen](./media/databricks-extract-load-sql-data-warehouse/select-keys.png)

5. Geben Sie eine Beschreibung des Schlüssels und eine Dauer für den Schlüssel ein. Wählen Sie dann die Option **Schließen**.

   ![Schlüssel speichern](./media/databricks-extract-load-sql-data-warehouse/save-key.png)

   Nach dem Speichern des Schlüssels wird der Wert des Schlüssels angezeigt. Kopieren Sie diesen Wert jetzt, da Sie ihn später nicht mehr abrufen können. Sie geben den Schlüsselwert zusammen mit der Anwendungs-ID ein, um sich als Anwendung anzumelden. Speichern Sie die Schlüsselwert an einem Ort, von dem Ihre Anwendung ihn abrufen kann.

   ![gespeicherter Schlüssel](./media/databricks-extract-load-sql-data-warehouse/copy-key.png)

### <a name="get-tenant-id"></a>Abrufen der Mandanten-ID

Beim programmgesteuerten Anmelden müssen Sie mit Ihrer Authentifizierungsanforderung die Mandanten-ID übergeben.

1. Wählen Sie **Azure Active Directory**.

   ![Azure Active Directory auswählen](./media/databricks-extract-load-sql-data-warehouse/select-active-directory.png)

1. Wählen Sie zum Abrufen der Mandanten-ID die Option **Eigenschaften** für Ihren Azure AD-Mandanten aus.

   ![Auswählen von Azure AD-Eigenschaften](./media/databricks-extract-load-sql-data-warehouse/select-ad-properties.png)

1. Kopieren Sie die **-Verzeichnis-ID**. Dieser Wert ist Ihre Mandanten-ID.

   ![tenant ID](./media/databricks-extract-load-sql-data-warehouse/copy-directory-id.png) 

## <a name="upload-data-to-data-lake-store"></a>Hochladen von Daten in den Data Lake-Speicher

In diesem Abschnitt laden Sie eine Beispieldatendatei in Data Lake Store hoch. Für diese Datei werden später in Azure Databricks einige Transformationen ausgeführt. Die in diesem Tutorial verwendeten Beispieldaten (**small_radio_json.json**) stehen in [diesem GitHub-Repository](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) zur Verfügung.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) das erstellte Data Lake Store-Konto aus.

2. Klicken Sie auf der Registerkarte **Übersicht** auf **Daten-Explorer**.

    ![Öffnen des Daten-Explorers](./media/databricks-extract-load-sql-data-warehouse/open-data-explorer.png "Öffnen des Daten-Explorers")

3. Klicken Sie im Daten-Explorer auf **Hochladen**.

    ![Option „Hochladen“](./media/databricks-extract-load-sql-data-warehouse/upload-to-data-lake-store.png "Option „Hochladen“")

4. Navigieren Sie unter **Dateien hochladen** zum Speicherort Ihrer Beispieldatendatei, und klicken Sie anschließend auf **Ausgewählte Dateien hinzufügen**.

    ![Option „Hochladen“](./media/databricks-extract-load-sql-data-warehouse/upload-data.png "Option „Hochladen“")

5. In diesem Tutorial haben Sie die Datendatei in das Stammverzeichnis von Data Lake Store hochgeladen. Die Datei befindet sich daher unter `adl://<YOUR_DATA_LAKE_STORE_ACCOUNT_NAME>.azuredatalakestore.net/small_radio_json.json`.

## <a name="associate-service-principal-with-azure-data-lake-store"></a>Zuordnen des Dienstprinzipals zu Azure Data Lake Store

In diesem Abschnitt ordnen Sie die Daten des Data Lake Store-Kontos dem erstellten Azure Active Directory-Dienstprinzipal zu. Dadurch wird sichergestellt, dass Sie von Azure Databricks aus auf das Data Lake Store-Konto zugreifen können. Für das Szenario in diesem Artikel lesen Sie die Daten in Data Lake Store aus, um damit eine Tabelle in SQL Data Warehouse aufzufüllen. Laut [Übersicht über die Zugriffssteuerung in Data Lake Store](../data-lake-store/data-lake-store-access-control.md#common-scenarios-related-to-permissions) benötigen Sie Folgendes, um über Lesezugriff auf eine Datei in Data Lake Store zu verfügen:

- **Ausführen**-Berechtigungen für alle Ordner der Ordnerstruktur bis zum Speicherort der Datei.
- **Lesen**-Berechtigungen für die Datei.

Führen Sie die folgenden Schritte aus, um diese Berechtigungen zu gewähren.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) das Data Lake Store-Konto aus, das Sie erstellt haben, und wählen Sie anschließend **Daten-Explorer**.

    ![Starten von Daten-Explorer](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-data-explorer.png "Starten von Daten-Explorer")

2. Da sich die Datei mit den Beispieldaten im Stammverzeichnis der Ordnerstruktur befindet, müssen Sie nur **Ausführen**-Berechtigungen für das Stammverzeichnis zuweisen. Wählen Sie hierzu im Stammverzeichnis des Daten-Explorers die Option **Zugriff**.

    ![Hinzufügen von ACLs für den Ordner](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-1.png "Hinzufügen von ACLs für den Ordner")

3. Wählen Sie unter **Zugriff** die Option **Hinzufügen**.

    ![Hinzufügen von ACLs für den Ordner](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-2.png "Hinzufügen von ACLs für den Ordner")

4. Klicken Sie unter **Berechtigungen zuweisen** auf **Benutzer oder Gruppe auswählen**, und suchen Sie nach dem Azure Active Directory-Dienstprinzipal, den Sie zuvor erstellt haben.

    ![Hinzufügen von Data Lake Store-Zugriff](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-3.png "Hinzufügen von Data Lake Store-Zugriff")

    Wählen Sie den AAD-Dienstprinzipal aus, den Sie zuweisen möchten, und klicken Sie auf **Auswählen**.

5. Klicken Sie unter **Berechtigungen zuweisen** auf **Berechtigungen auswählen** > **Ausführen**. Behalten Sie die anderen Standardwerte bei, und wählen Sie unter **Berechtigungen auswählen** und dann unter **Berechtigungen zuweisen** jeweils **OK**.

    ![Hinzufügen von Data Lake Store-Zugriff](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-4.png "Hinzufügen von Data Lake Store-Zugriff")

6. Wechseln Sie zurück zum Daten-Explorer, und klicken Sie nun auf die Datei, die Sie der Leseberechtigung zuweisen möchten. Wählen Sie unter **Dateivorschau** die Option **Zugriff**.

    ![Hinzufügen von Data Lake Store-Zugriff](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-file-1.png "Hinzufügen von Data Lake Store-Zugriff")

7. Wählen Sie unter **Zugriff** die Option **Hinzufügen**. Klicken Sie unter **Berechtigungen zuweisen** auf **Benutzer oder Gruppe auswählen**, und suchen Sie nach dem Azure Active Directory-Dienstprinzipal, den Sie zuvor erstellt haben.

    ![Hinzufügen von Data Lake Store-Zugriff](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-3.png "Hinzufügen von Data Lake Store-Zugriff")

    Wählen Sie den AAD-Dienstprinzipal aus, den Sie zuweisen möchten, und klicken Sie auf **Auswählen**.

8. Klicken Sie unter **Berechtigungen zuweisen** auf **Berechtigungen auswählen** > **Lesen**. Wählen Sie unter **Berechtigungen auswählen** und dann unter **Berechtigungen zuweisen** jeweils **OK**.

    ![Hinzufügen von Data Lake Store-Zugriff](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-file-2.png "Hinzufügen von Data Lake Store-Zugriff")

    Der Dienstprinzipal verfügt jetzt über ausreichende Berechtigungen zum Lesen der Datei mit den Beispieldaten aus Azure Data Lake Store.

## <a name="extract-data-from-data-lake-store"></a>Extrahieren von Daten aus Data Lake Store

In diesem Abschnitt erstellen Sie ein Notebook im Azure Databricks-Arbeitsbereich und führen anschließend Codeausschnitte aus, um Daten aus Data Lake Store in Azure Databricks zu extrahieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum erstellten Azure Databricks-Arbeitsbereich, und klicken Sie auf **Launch Workspace** (Arbeitsbereich starten).

2. Klicken Sie im linken Bereich auf **Arbeitsbereich**. Wählen Sie in der Dropdownliste **Arbeitsbereich** die Option **Erstellen** > **Notebook** aus.

    ![Erstellen eines Notizbuchs in Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-create-notebook.png "Erstellen eines Notizbuchs in Databricks")

2. Geben Sie im Dialogfeld **Notizbuch erstellen** einen Namen für das Notebook ein. Wählen Sie **Scala** als Sprache und anschließend den zuvor erstellten Spark-Cluster aus.

    ![Erstellen eines Notizbuchs in Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-notebook-details.png "Erstellen eines Notizbuchs in Databricks")

    Klicken Sie auf **Erstellen**.

3. Fügen Sie den folgenden Codeausschnitt in eine leere Codezelle ein, und ersetzen Sie die Platzhalterwerte durch die Werte, die Sie zuvor für den Azure Active Directory-Dienstprinzipal gespeichert haben:

        spark.conf.set("dfs.adls.oauth2.access.token.provider.type", "ClientCredential")
        spark.conf.set("dfs.adls.oauth2.client.id", "<APPLICATION-ID>")
        spark.conf.set("dfs.adls.oauth2.credential", "<AUTHENTICATION-KEY>")
        spark.conf.set("dfs.adls.oauth2.refresh.url", "https://login.microsoftonline.com/<TENANT-ID>/oauth2/token")

    Drücken Sie **UMSCHALT+EINGABE**, um die Codezelle auszuführen.

4. Nun können Sie die JSON-Beispieldatei in Data Lake Store als Datenrahmen in Azure Databricks laden. Fügen Sie den folgenden Codeausschnitt in eine neue Codezelle ein, ersetzen Sie den Platzhalterwert, und drücken Sie anschließend**UMSCHALT+EINGABE**.

        val df = spark.read.json("adl://<DATA LAKE STORE NAME>.azuredatalakestore.net/small_radio_json.json")

5. Führen Sie den folgenden Codeausschnitt aus, um den Inhalt des Datenrahmens anzuzeigen:

        df.show()

    Es wird ungefähr folgender Codeausschnitt angezeigt:

        +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
        |               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
        +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
        | El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
        | Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
        | Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
        ...
        ...

Sie haben die Daten aus Azure Data Lake Store in Azure Databricks extrahiert.

## <a name="transform-data-in-azure-databricks"></a>Transformieren von Daten in Azure Databricks

Die unformatierten Beispieldaten aus **small_radio_json.json** dienen zur Erfassung der Zuhörer eines Radiosenders und enthalten eine Reihe von Spalten. In diesem Abschnitt transformieren Sie die Daten, um nur bestimmte Spalten aus dem Dataset abzurufen. 

1. Rufen Sie zunächst nur die Spalten *firstName*, *lastName*, *gender*, *location* und *level* aus dem bereits erstellten Datenrahmen ab.

        val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
        specificColumnsDf.show()

    Daraufhin erhalten Sie eine Ausgabe wie die folgende:

        +---------+----------+------+--------------------+-----+
        |firstname|  lastname|gender|            location|level|
        +---------+----------+------+--------------------+-----+
        | Annalyse|Montgomery|     F|  Killeen-Temple, TX| free|
        |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
        |     Liam|     Watts|     M|New York-Newark-J...| paid|
        |     Tess|  Townsend|     F|Nashville-Davidso...| free|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
        |     Alan|     Morse|     M|Chicago-Napervill...| paid|
        |Gabriella|   Shelton|     F|San Jose-Sunnyval...| free|
        |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
        |     Tess|  Townsend|     F|Nashville-Davidso...| free|
        |     Alan|     Morse|     M|Chicago-Napervill...| paid|
        |     Liam|     Watts|     M|New York-Newark-J...| paid|
        |     Liam|     Watts|     M|New York-Newark-J...| paid|
        |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
        |     Alan|     Morse|     M|Chicago-Napervill...| paid|
        |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
        |     Alan|     Morse|     M|Chicago-Napervill...| paid|
        |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
        +---------+----------+------+--------------------+-----+

2.  Diese Daten können noch weiter transformiert werden, um die Spalte **level** in **subscription_type** umzubenennen:

        val renamedColumnsDf = specificColumnsDf.withColumnRenamed("level", "subscription_type")
        renamedColumnsDf.show()

    Daraufhin erhalten Sie eine Ausgabe wie die folgende:

        +---------+----------+------+--------------------+-----------------+
        |firstname|  lastname|gender|            location|subscription_type|
        +---------+----------+------+--------------------+-----------------+
        | Annalyse|Montgomery|     F|  Killeen-Temple, TX|             free|
        |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
        |     Liam|     Watts|     M|New York-Newark-J...|             paid|
        |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
        |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
        |Gabriella|   Shelton|     F|San Jose-Sunnyval...|             free|
        |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
        |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
        |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
        |     Liam|     Watts|     M|New York-Newark-J...|             paid|
        |     Liam|     Watts|     M|New York-Newark-J...|             paid|
        |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
        |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
        |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
        |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
        |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
        +---------+----------+------+--------------------+-----------------+

## <a name="load-data-into-azure-sql-data-warehouse"></a>Laden von Daten in Azure SQL Data Warehouse

In diesem Abschnitt laden Sie die transformierten Daten in Azure SQL Data Warehouse hoch. Mithilfe des Azure SQL Data Warehouse-Connectors für Azure Databricks können Sie einen Datenrahmen direkt als Tabelle in SQL Data Warehouse hochladen.

Der SQL Date Warehouse-Connector verwendet, wie bereits erwähnt, Azure Blob Storage als temporären Speicherort, um Daten zwischen Azure Databricks und Azure SQL Data Warehouse hochzuladen. Geben Sie daher zunächst die Konfiguration für die Verbindungsherstellung mit dem Speicherkonto an. Das Konto muss im Rahmen der Vorbereitung für diesen Artikel bereits erstellt worden sein.

1. Geben Sie die Konfiguration für den Zugriff auf das Azure Storage-Konto über Azure Databricks an.

        val blobStorage = "<STORAGE ACCOUNT NAME>.blob.core.windows.net"
        val blobContainer = "<CONTAINER NAME>"
        val blobAccessKey =  "<ACCESS KEY>"

2. Geben Sie einen temporären Ordner an, der beim Verschieben von Daten zwischen Azure Databricks und Azure SQL Data Warehouse verwendet werden soll.

        val tempDir = "wasbs://" + blobContainer + "@" + blobStorage +"/tempDirs"

3. Führen Sie den folgenden Codeausschnitt aus, um Azure Blob-Speicherzugriffsschlüssel in der Konfiguration zu speichern. Dadurch wird sichergestellt, dass Sie den Zugriffsschlüssel im Notebook nicht als Klartext speichern müssen.

        val acntInfo = "fs.azure.account.key."+ blobStorage
        sc.hadoopConfiguration.set(acntInfo, blobAccessKey)

4. Geben Sie die Werte für die Verbindungsherstellung mit der Azure SQL Data Warehouse-Instanz an. Im Rahmen der Vorbereitung muss bereits eine SQL Data Warehouse-Instanz erstellt worden sein.

        //SQL Data Warehouse related settings
        val dwDatabase = "<DATABASE NAME>"
        val dwServer = "<DATABASE SERVER NAME>" 
        val dwUser = "<USER NAME>"
        val dwPass = "<PASSWORD>"
        val dwJdbcPort =  "1433"
        val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
        val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
        val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass

5. Führen Sie den folgenden Codeausschnitt aus, um den transformierten Datenrahmen (**renamedColumnsDf**) als Tabelle in SQL Data Warehouse zu laden. Dieser Codeausschnitt erstellt in der SQL-Datenbank eine Tabelle namens **SampleTable**. Beachten Sie, dass für Azure SQL DW ein Masterschlüssel erforderlich ist.  Sie können einen Hauptschlüssel erstellen, indem Sie in SQL Server Management Studio den Befehl „CREATE MASTER KEY;“ ausführen.

        spark.conf.set(
          "spark.sql.parquet.writeLegacyFormat",
          "true")
        
        renamedColumnsDf.write
            .format("com.databricks.spark.sqldw")
            .option("url", sqlDwUrlSmall) 
            .option("dbtable", "SampleTable")
            .option( "forward_spark_azure_storage_credentials","True")
            .option("tempdir", tempDir)
            .mode("overwrite")
            .save()

6. Stellen Sie eine Verbindung mit der SQL-Datenbank her, und vergewissern Sie sich, dass eine Tabelle namens **SampleTable** vorhanden ist.

    ![Überprüfen der Beispieltabelle](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table.png "Überprüfen der Beispieltabelle")

7. Führen Sie zur Überprüfung des Tabelleninhalts eine SELECT-Abfrage aus. Es sollten die gleichen Daten wie im Datenrahmen **renamedColumnsDf** enthalten sein.

    ![Überprüfen des Inhalts der Beispieltabelle](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table-content.png "Überprüfen des Inhalts der Beispieltabelle")

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nach Abschluss des Tutorials können Sie den Cluster beenden. Klicken Sie hierzu im linken Bereich des Azure Databricks-Arbeitsbereichs auf **Cluster**. Bewegen Sie den Cursor zum Beenden des Clusters auf die Auslassungspunkte in der Spalte **Aktionen**, und klicken Sie auf das Symbol **Beenden**.

![Beenden eines Databricks-Clusters](./media/databricks-extract-load-sql-data-warehouse/terminate-databricks-cluster.png "Beenden eines Databricks-Clusters")

Wenn Sie den Cluster nicht manuell beenden, wird er automatisch beendet, sofern Sie bei der Erstellung des Clusters das Kontrollkästchen **Terminate after __ minutes of inactivity** (Nach __ Minuten Inaktivität beenden) aktiviert haben. Der Cluster wird dann automatisch beendet, wenn er für den angegebenen Zeitraum inaktiv war.

## <a name="next-steps"></a>Nächste Schritte 
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Azure Databricks-Arbeitsbereichs
> * Erstellen eines Spark-Clusters in Azure Databricks
> * Erstellen eines Azure Data Lake-Speicherkontos
> * Hochladen von Daten in Azure Data Lake Store
> * Erstellen eines Notebooks in Azure Databricks
> * Extrahieren von Daten aus Data Lake Store
> * Transformieren von Daten in Azure Databricks
> * Laden von Daten in Azure SQL Data Warehouse

Das nächste Tutorial beschäftigt sich mit dem Streamen von Echtzeitdaten an Azure Databricks unter Verwendung von Azure Event Hubs.

> [!div class="nextstepaction"]
>[Stream data into Azure Databricks using Event Hubs](databricks-stream-from-eventhubs.md) (Streamen von Daten an Azure Databricks unter Verwendung von Event Hubs)
