---
title: Kopieren von Daten aus HDFS mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Daten aus einer Cloud- oder lokalen HDFS-Quelle mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: jingwang
ms.openlocfilehash: 6d28b82f892ac88c32ab3e04df19143d69a5a05b
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2017
---
# <a name="copy-data-from-and-to-hdfs-using-azure-data-factory"></a>Kopieren von Daten aus dem und in das HDFS mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – Allgemein verfügbar](v1/data-factory-hdfs-connector.md)
> * [Version 2 – Vorschau](connector-hdfs.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus dem und in das HDFS zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschauversion verfügbar ist. Wenn Sie Version 1 des Data Factory-Diensts verwenden, der allgemein verfügbar (General Availability, GA) ist, lesen Sie [HDFS-Connector in V1](v1/data-factory-hdfs-connector.md).


## <a name="supported-scenarios"></a>Unterstützte Szenarien

Sie können Daten aus HDFS in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der HDFS-Connector unterstützt insbesondere Folgendes:

- Kopieren von Dateien mithilfe der **Windows**-Authentifizierung (Kerberos) oder **anonymen** Authentifizierung
- Kopieren von Dateien mithilfe des **WebHDFS**-Protokolls oder der **integrierten DistCp**-Unterstützung
- Kopieren von Dateien im jeweiligen Zustand oder Analysieren bzw. Generieren von Dateien mit den [unterstützten Dateiformaten und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md)

## <a name="prerequisites"></a>Voraussetzungen

Zum Kopieren von Daten aus einem oder in ein HDFS, das nicht öffentlich zugänglich ist, müssen Sie eine selbstgehostete Integrationslaufzeit einrichten. Im Artikel [Selbstgehostete Integrationslaufzeit](concepts-integration-runtime.md) finden Sie Details.

## <a name="getting-started"></a>Erste Schritte
Sie können mit dem .NET SDK, Python SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](quickstart-create-data-factory-dot-net.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für HDFS verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit HDFS verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft muss auf **Hdfs** festgelegt werden. | Ja |
| url |Die URL für das HDFS. |Ja |
| authenticationType | Zulässige Werte sind **Anonymous** oder **Windows**. <br><br> Um Ihre lokale Umgebung zur Verwendung der **Kerberos-Authentifizierung** für den HDFS-Connector einzurichten, lesen Sie [diesen Abschnitt](#use-kerberos-authentication-for-hdfs-connector). |Ja |
| userName |Der Benutzername für die Windows-Authentifizierung. Geben Sie für die Kerberos-Authentifizierung `<username>@<domain>.com` an. |Ja (für die Windows-Authentifizierung) |
| password |Das Kennwort für die Windows-Authentifizierung. Legen Sie für dieses Feld „SecureString“ fest. |Ja (für die Windows-Authentifizierung) |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die selbstgehostete Integration Runtime oder Azure Integration Runtime verwenden (sofern Ihr Datenspeicher öffentlich zugänglich ist). Wenn keine Option angegeben ist, wird die standardmäßige Azure-Integrationslaufzeit verwendet. |Nein |

**Beispiel: Verwenden der anonymen Authentifizierung**

```json
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Anonymous",
            "userName": "hadoop"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel: Verwenden der Windows-Authentifizierung**

```json
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom HDFS-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus einem HDFS die type-Eigenschaft des Datasets auf **FileShare** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft des Datasets muss auf **FileShare** festgelegt werden. |Ja |
| folderPath | Pfad zum Ordner. Beispiel: „<Ordner>/<Unterordner>/“ |Ja |
| fileName | Geben Sie den Namen der Datei in der Eigenschaft **folderPath** an, wenn Sie einen Kopiervorgang aus einer bestimmten Datei durchführen möchten. Wenn Sie keine Werte für diese Eigenschaft angeben, verweist das Dataset auf alle Dateien im Ordner als Quelle. |Nein |
| format | Wenn Sie **Dateien unverändert zwischen dateibasierten Speichern kopieren** möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen.<br/><br/>Wenn Dateien mit einem bestimmten Format analysiert werden sollen, werden die folgenden Formattypen unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](supported-file-formats-and-compression-codecs.md#text-format), [JSON-Format](supported-file-formats-and-compression-codecs.md#json-format), [Avro-Format](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-Format](supported-file-formats-and-compression-codecs.md#orc-format) und [Parquet-Format](supported-file-formats-and-compression-codecs.md#parquet-format). |Nein (nur für Szenarien mit Binärkopien) |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Weitere Informationen finden Sie unter [Unterstützte Dateiformate und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Unterstützte Typen sind: **Gzip**, **Deflate**, **bzip2** und **ZipDeflate**.<br/>Unterstützte Grade sind: **Optimal** und **Schnellste**. |Nein |

**Beispiel:**

```json
{
    "name": "HDFSDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der HDFS-Quelle und -Senke unterstützt werden.

### <a name="hdfs-as-source"></a>HDFS als Quelle

Legen Sie zum Kopieren von Daten aus einem HDFS den Quelltyp in der Kopieraktivität auf **HdfsSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **HdfsSource** festgelegt werden. |Ja |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden.<br/>Zulässige Werte sind **true** (Standard) oder **false**. | Nein |
| distcpSettings | Eigenschaftengruppe bei Verwendung von HDFS DistCp | Nein |
| resourceManagerEndpoint | YARN-ResourceManager-Endpunkt | Ja, wenn DistCp verwendet wird |
| tempScriptPath | Ein Ordnerpfad zum Speichern von temporären DistCp-Befehlsskripts. Die Skriptdatei wird von ADF generiert und nach Abschluss des Kopierauftrags entfernt. | Ja, wenn DistCp verwendet wird |
| distcpOptions | Zusätzliche für den DistCp-Befehl bereitgestellte Optionen | Nein |

**Beispiel: HDFS-Quelle in der Kopieraktivität mit UNLOAD**

```json
"source": {
    "type": "HdfsSource",
    "distcpSettings": {
        "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
        "tempScriptPath": "/usr/hadoop/tempscript",
        "distcpOptions": "-m 100"
    }
}
```

Erfahren Sie mehr darüber, wie Sie mithilfe von DistCp effizient Daten aus einem HDFS aus dem nächsten Abschnitt kopieren.

## <a name="use-distcp-to-copy-data-from-hdfs"></a>Kopieren von Daten aus einem HDFS mithilfe von DistCp

[DistCp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) ist ein natives Hadoop-Befehlszeilentool, um verteilte Kopiervorgänge in einem Hadoop-Cluster auszuführen. Wenn ein DistCp-Befehl ausgeführt wird, werden zuerst alle zu kopierende Dateien aufgeführt und mehrere Zuordnungsaufträge im Hadoop-Cluster erstellt. Zudem wird für jede Zuordnung eine Binärkopie von der Quelle in der Senke erstellt.

Statt die selbstgehostete Integrationslaufzeit auszuführen, können bei der Kopieraktivität mithilfe von DistCp Dateien im jeweiligen Zustand im Azure-Blob (einschließlich der [bereitgestellten Kopie](copy-activity-performance.md)) oder Azure Data Lake Store kopiert werden, sodass Ihr Cluster optimal zum Einsatz kommt. Die Kopieraktivität bietet einen besseren Kopierdurchsatz, insbesondere dann, wenn Sie über einen äußerst umfangreichen Cluster verfügen. Basierend auf Ihrer Konfiguration in Azure Data Factory erstellt die Kopieraktivität automatisch einen DistCp-Befehl, sendet diesen an Ihren Hadoop-Cluster und überwacht den Kopierstatus.

### <a name="prerequsites"></a>Voraussetzungen

Um mithilfe von DistCp Dateien im jeweiligen Zustand aus HDFS im Azure-Blob (einschließlich der bereitgestellten Kopie) oder Azure Data Lake Store kopieren zu können, müssen Sie sicherstellen, dass Ihr Hadoop-Cluster die folgenden Anforderungen erfüllt:

1. Die Dienste MapReduce und YARN sind aktiviert.
2. Sie verfügen über YARN Version 2.5 oder höher.
3. Der HDFS-Server ist in Ihren Zieldatenspeicher integriert, d.h. Azure-Blob oder Azure Data Lake Store:

    - Das Azure-Blobdateisystem wird nativ ab Hadoop 2.7 unterstützt. Sie müssen lediglich den JAR-Pfad in der Hadoop-Umgebungskonfiguration angeben.
    - Das Azure Data Lake Store-Dateisystem wird ab Hadoop 3.0.0-alpha1 verpackt. Wenn Ihr Hadoop-Cluster eine niedrigere Version als diese aufweist, müssen Sie JAR-Pakete im Zusammenhang mit ADLS (azure-datalake-store.jar) [hierüber](https://hadoop.apache.org/releases.html) manuell in den Cluster importieren und den JAR-Pfad in der Hadoop-Umgebungskonfiguration angeben.

4. Bereiten Sie einen temporären Ordner in HDFS vor. In diesem temp-Ordner werden DistCp-Shellskripts gespeichert, sodass Speicherplatz im KB-Bereich belegt wird.
5. Stellen Sie sicher, dass das im verknüpften HDFS-Dienst bereitgestellte Benutzerkonto über Berechtigungen zum a) Übermitteln einer Anwendung in YARN; und b) Erstellen eines Unterordners sowie zum Lesen von bzw. Schreiben in Dateien im oben genannten temp-Ordner verfügt.

### <a name="configurations"></a>Configurations

Im Folgenden wird ein Beispiel für die Konfiguration der Kopieraktivität gezeigt, um Daten von HDFS im Azure-Blob mithilfe von DistCp zu kopieren:

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromHDFSToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "HDFSDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "BlobDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "HdfsSource",
                "distcpSettings": {
                    "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
                    "tempScriptPath": "/usr/hadoop/tempscript",
                    "distcpOptions": "-strategy dynamic -map 100"
                }
            },
            "sink": {
                "type": "BlobSink"
            }
        }
    }
]
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>Verwenden der Kerberos-Authentifizierung für den HDFS-Connector

Zur Einrichtung der lokalen Umgebung für die Verwendung der Kerberos-Authentifizierung im HDFS-Connector gibt es zwei Optionen. Wählen Sie die Option, die in Ihrem Fall besser passt.
* Option 1: [Beitreten zum Computer mit der selbstgehosteten Integrationslaufzeit im Kerberosbereich](#kerberos-join-realm)
* Option 2: [Aktivieren der gegenseitigen Vertrauensstellung zwischen der Windows-Domäne und dem Kerberos-Bereich](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Option 1: Beitreten zum Computer mit der selbstgehosteten Integrationslaufzeit im Kerberosbereich

#### <a name="requirements"></a>Requirements (Anforderungen)

* Der Computer mit der selbstgehosteten Integrationslaufzeit muss dem Kerberosbereich beitreten und darf in keine Windows-Domäne eingebunden sein.

#### <a name="how-to-configure"></a>Vorgehensweise zur Konfiguration

**Bezüglich der selbstgehosteten Integrationslaufzeit:**

1.  Führen Sie das Dienstprogramm **Ksetup** aus, um den Kerberos-KDC-Server und -Bereich zu konfigurieren.

    Der Computer muss als Mitglied einer Arbeitsgruppe konfiguriert werden, da sich Kerberos-Bereiche von Windows-Domänen unterscheiden. Sie erreichen dies, indem Sie den Kerberos-Bereich einrichten und einen KDC-Server hinzufügen, wie im Folgenden erläutert. Ersetzen Sie *REALM.COM* durch Ihren eigenen Bereich.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    Nach Ausführung dieser beiden Befehle **starten Sie den Computer neu**.

2.  Überprüfen Sie die Konfiguration mit dem Befehl **Ksetup**. Die Ausgabe sollte wie folgt sein:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**In Azure Data Factory:**

* Konfigurieren Sie den HDFS-Connector mithilfe der **Windows-Authentifizierung** zusammen mit dem Namen und Kennwort Ihres Kerberos-Prinzipals, um eine Verbindung mit der HDFS-Datenquelle herzustellen. Informationen zu den Konfigurationsdetails finden Sie im Abschnitt [Eigenschaften des mit HDFS verknüpften Diensts](#linked-service-properties).

### <a name="kerberos-mutual-trust"></a>Option 2: Aktivieren der gegenseitigen Vertrauensstellung zwischen der Windows-Domäne und dem Kerberos-Bereich

#### <a name="requirements"></a>Requirements (Anforderungen)

*   Der Computer mit der selbstgehosteten Integrationslaufzeit muss einer Windows-Domäne beitreten.
*   Sie benötigen die Berechtigung zum Aktualisieren der Einstellungen des Domänencontrollers.

#### <a name="how-to-configure"></a>Vorgehensweise zur Konfiguration

> [!NOTE]
> Ersetzen Sie im folgenden Tutorial die Zeichenfolgen REALM.COM und AD.COM durch Ihren eigenen Bereich und Domänencontroller.

**Auf dem KDC-Server:**

1.  Bearbeiten Sie die KDC-Konfiguration in der Datei **krb5.conf** mithilfe der folgenden Konfigurationsvorlage so, dass KDC der Windows-Domäne vertraut. Standardmäßig befindet sich die Konfiguration unter **/etc/krb5.conf**.

            [logging]
             default = FILE:/var/log/krb5libs.log
             kdc = FILE:/var/log/krb5kdc.log
             admin_server = FILE:/var/log/kadmind.log

            [libdefaults]
             default_realm = REALM.COM
             dns_lookup_realm = false
             dns_lookup_kdc = false
             ticket_lifetime = 24h
             renew_lifetime = 7d
             forwardable = true

            [realms]
             REALM.COM = {
              kdc = node.REALM.COM
              admin_server = node.REALM.COM
             }
            AD.COM = {
             kdc = windc.ad.com
             admin_server = windc.ad.com
            }

            [domain_realm]
             .REALM.COM = REALM.COM
             REALM.COM = REALM.COM
             .ad.com = AD.COM
             ad.com = AD.COM

            [capaths]
             AD.COM = {
              REALM.COM = .
             }

  Führen Sie nach der Konfiguration einen **Neustart** des KDC-Diensts aus.

2.  Bereiten Sie mit dem folgenden Befehl einen Prinzipal namens **krbtgt/REALM.COM@AD.COM** auf dem KDC-Server vor:

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  Fügen Sie in der HDFS-Dienstkonfigurationsdatei **hadoop.security.auth_to_local** diese Zeichenfolge hinzu: `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.

**Auf dem Domänencontroller:**

1.  Führen Sie die folgenden **Ksetup**-Befehle aus, um einen Bereichseintrag hinzuzufügen:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Richten Sie eine Vertrauensstellung zwischen der Windows-Domäne und dem Kerberos-Bereich ein. [password] ist das Kennwort für den Prinzipal **krbtgt/REALM.COM@AD.COM**.

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Wählen Sie den in Kerberos verwendeten Verschlüsselungsalgorithmus aus.

    1. Wechseln Sie zu „Server-Manager > Gruppenrichtlinienverwaltung > Domäne > Gruppenrichtlinienobjekte > Standard- oder aktive Domänenrichtlinie“, und wählen Sie „Bearbeiten“.

    2. Wechseln Sie im Popupfenster **Gruppenrichtlinienverwaltungs-Editor** zu „Computerkonfiguration > Richtlinien > Windows-Einstellungen > Sicherheitseinstellungen > Lokale Richtlinien > Sicherheitsoptionen“, und konfigurieren Sie die Einstellung **Netzwerksicherheit: Für Kerberos zulässige Verschlüsselungstypen konfigurieren**.

    3. Wählen Sie den Verschlüsselungsalgorithmus, der beim Herstellen der Verbindung mit KDC verwendet werden soll. Im Allgemeinen können Sie einfach alle Optionen auswählen.

        ![Konfigurieren von Verschlüsselungstypen für Kerberos](media/connector-hdfs/config-encryption-types-for-kerberos.png)

    4. Verwenden Sie den Befehl **Ksetup**, um den Verschlüsselungsalgorithmus anzugeben, der in dem bestimmten Bereich verwendet werden soll.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Erstellen Sie die Zuordnung zwischen dem Domänenkonto und dem Kerberos-Prinzipal, um den Kerberos-Prinzipal in der Windows-Domäne verwenden zu können.

    1. Starten Sie „Verwaltung > **Active Directory-Benutzer und -Computer**“.

    2. Konfigurieren Sie erweiterte Funktionen, indem Sie auf **Ansicht** > **Erweiterte Funktionen** klicken.

    3. Suchen Sie das Konto, für das Sie Zuordnungen erstellen möchten, und klicken Sie mit der rechten Maustaste, um **Namenszuordnungen** anzuzeigen. Klicken Sie dann auf die Registerkarte **Kerberos-Namen**.

    4. Fügen Sie einen Prinzipal aus dem Bereich hinzu.

        ![Sicherheitsidentität zuordnen](media/connector-hdfs/map-security-identity.png)

**Bezüglich der selbstgehosteten Integrationslaufzeit:**

* Führen Sie die folgenden **Ksetup**-Befehle aus, um einen Bereichseintrag hinzuzufügen.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**In Azure Data Factory:**

* Konfigurieren Sie den HDFS-Connector mithilfe der **Windows-Authentifizierung** zusammen mit Ihrem Domänenkonto oder Ihrem Kerberos-Prinzipal, um eine Verbindung mit der HDFS-Datenquelle herzustellen. Informationen zu den Konfigurationsdetails finden Sie im Abschnitt [Eigenschaften des mit HDFS verknüpften Diensts](#linked-service-properties).


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).