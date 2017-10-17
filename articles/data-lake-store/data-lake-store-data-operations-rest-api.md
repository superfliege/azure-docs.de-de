---
title: "REST-API: Dateisystemvorgänge in Azure Data Lake Store | Microsoft-Dokumentation"
description: "Verwenden von WebHDFS-REST-APIs, um Dateisystemvorgänge in Data Lake Store auszuführen"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/28/2017
ms.author: nitinme
ms.openlocfilehash: 62cfc713c3b7ce8f7c0a7ff4dd5e80802f1843b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="filesystem-operations-on-azure-data-lake-store-using-rest-api"></a>Dateisystemvorgänge in Azure Data Lake Store per REST-API
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

In diesem Artikel erfahren Sie, wie Sie mit WebHDFS-REST-APIs und Data Lake Store-REST-APIs Dateisystemvorgänge in Azure Data Lake Store durchführen. Eine Anleitung zum Durchführen von Kontoverwaltungsvorgängen in Data Lake Store per REST-API finden Sie unter [Erste Schritte mit Azure Data Lake Store mithilfe von REST-APIs](data-lake-store-get-started-rest-api.md).

## <a name="prerequisites"></a>Voraussetzungen
* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake-Speicherkonto**. Befolgen Sie die Anweisungen unter [Erste Schritte mit Azure Data Lake Store über das Azure-Portal](data-lake-store-get-started-portal.md).

* **[cURL](http://curl.haxx.se/)**. Dieser Artikel zeigt mit cURL, wie Sie REST-API-Aufrufe eines Data Lake-Speicherkontos ausführen.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Wie authentifiziere ich mich mithilfe von Azure Active Directory?
Zur Authentifizierung mit Azure Active Directory können Sie zwischen zwei Ansätzen wählen.

* Informationen zur Authentifizierung von Endbenutzern für Ihre Anwendung (interaktiv) finden Sie unter [End-user authentication with Data Lake Store using .NET SDK](data-lake-store-end-user-authenticate-rest-api.md) (Authentifizierung von Endbenutzern mit Data Lake Store per .NET SDK).
* Informationen zur Dienst-zu-Dienst-Authentifizierung für Ihre Anwendung (nicht interaktiv) finden Sie unter [Service-to-service authentication with Data Lake Store using .NET SDK](data-lake-store-service-to-service-authenticate-rest-api.md) (Dienst-zu-Dienst-Authentifizierung mit Data Lake Store per .NET SDK).


## <a name="create-folders"></a>Erstellen Sie die Ordner.
Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory)definierten WebHDFS-REST-API-Aufruf.

Verwenden Sie den folgenden cURL-Befehl. Ersetzen Sie **\<yourstorename>** durch Ihren Data Lake Store-Namen.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

Ersetzen Sie im vorherigen Befehl \<`REDACTED`\> durch das Autorisierungstoken, das Sie zuvor abgerufen haben. Dieser Befehl erstellt das Verzeichnis **mytempdir** im Stammordner Ihres Data Lake Store-Kontos.

Wenn der Vorgang erfolgreich abgeschlossen wurde, sollte eine Antwort wie im folgenden Codeausschnitt angezeigt werden:

    {"boolean":true}

## <a name="list-folders"></a>Auflisten von Ordnern
Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory)definierten WebHDFS-REST-API-Aufruf.

Verwenden Sie den folgenden cURL-Befehl. Ersetzen Sie **\<yourstorename>** durch Ihren Data Lake Store-Namen.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

Ersetzen Sie im vorherigen Befehl \<`REDACTED`\> durch das Autorisierungstoken, das Sie zuvor abgerufen haben.

Wenn der Vorgang erfolgreich abgeschlossen wurde, sollte eine Antwort wie im folgenden Codeausschnitt angezeigt werden:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "<GUID>",
            "group": "<GUID>"
        }]
    }
    }

## <a name="upload-data"></a>Hochladen von Daten
Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File)definierten WebHDFS-REST-API-Aufruf.

Verwenden Sie den folgenden cURL-Befehl. Ersetzen Sie **\<yourstorename>** durch Ihren Data Lake Store-Namen.

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

In der vorherigen Syntax wird mit dem Parameter **-T** der Speicherort der hochgeladenen Datei angegeben.

Die Ausgabe sieht in etwa wie im folgenden Codeausschnitt aus:
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data"></a>Lesen von Daten
Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File)definierten WebHDFS-REST-API-Aufruf.

Lesen von Daten aus einem Data Lake-Speicherkonto ist ein zweistufiger Prozess.

* Sie müssen zuerst eine GET-Anforderung an den Endpunkt `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`senden. Mit diesem Aufruf wird ein Speicherort zurückgegeben, an den die nächste GET-Anforderung gesendet werden soll.
* Dann senden Sie die GET-Anforderung an den Endpunkt `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Mit diesem Aufruf wird der Inhalt der Datei angezeigt.

Da es in den Eingabeparametern keinen Unterschied zwischen dem ersten und zweiten Schritt gibt, können Sie mit dem `-L` -Parameter die erste Anforderung senden. Mit `-L` werden im Wesentlichen zwei Anforderungen in einer kombiniert, und es wird erreicht, dass cURL die Anforderung am neuen Speicherort noch einmal ausführt. Schließlich wird die Ausgabe aller Anforderungsaufrufe wie im folgenden Codeausschnitt dargestellt angezeigt. Ersetzen Sie **\<yourstorename>** durch Ihren Data Lake Store-Namen.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

Die Ausgabe sollte in etwa wie folgender Codeausschnitt aussehen:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file"></a>Umbenennen einer Datei
Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory)definierten WebHDFS-REST-API-Aufruf.

Verwenden Sie zum Umbenennen einer Datei den folgenden cURL-Befehl. Ersetzen Sie **\<yourstorename>** durch Ihren Data Lake Store-Namen.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

Die Ausgabe sollte in etwa wie folgender Codeausschnitt aussehen:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file"></a>Löschen von Dateien
Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory)definierten WebHDFS-REST-API-Aufruf.

Verwenden Sie zum Löschen einer Datei den folgenden cURL-Befehl. Ersetzen Sie **\<yourstorename>** durch Ihren Data Lake Store-Namen.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

Folgendes sollte angezeigt werden:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="next-steps"></a>Nächste Schritte
* [Erste Schritte mit Azure Data Lake Store mithilfe von REST-APIs](data-lake-store-get-started-rest-api.md)

## <a name="see-also"></a>Weitere Informationen
* [Azure Data Lake Store – REST-API-Referenz](https://docs.microsoft.com/rest/api/datalakestore/)
* [Open Source-Big Data-Anwendungen, die mit Azure Data Lake-Speicher funktionieren](data-lake-store-compatible-oss-other-applications.md)

