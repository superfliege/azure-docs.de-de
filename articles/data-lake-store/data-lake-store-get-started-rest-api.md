---
title: "REST-API: Kontoverwaltungsvorgänge in Azure Data Lake Store | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Azure Data Lake Store und die WebHDFS-REST-API verwenden, um in Data Lake Store Kontoverwaltungsvorgänge durchzuführen."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/28/2017
ms.author: nitinme
ms.openlocfilehash: 6c43f2b341280731707e486ba6f22f11560102c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="account-management-operations-on-azure-data-lake-store-using-rest-api"></a>Kontoverwaltungsvorgänge in Azure Data Lake Store mit der REST-API
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

In diesem Artikel wird beschrieben, wie Sie Kontoverwaltungsvorgänge in Data Lake Store mit der REST-API durchführen. Zu den Kontoverwaltungsvorgängen gehören das Erstellen eines Data Lake Store-Kontos, Löschen eines Data Lake Store-Kontos usw. Eine Anleitung, wie Sie Dateisystemvorgänge in Data Lake Store mit der REST-API durchführen, finden Sie unter [Filesystem operations on Data Lake Store using REST API](data-lake-store-data-operations-rest-api.md) (Dateisystemvorgänge in Data Lake Store mit der REST-API).

## <a name="prerequisites"></a>Voraussetzungen
* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **[cURL](http://curl.haxx.se/)**. Dieser Artikel zeigt mit cURL, wie Sie REST-API-Aufrufe eines Data Lake-Speicherkontos ausführen.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Wie authentifiziere ich mich mithilfe von Azure Active Directory?
Zur Authentifizierung mit Azure Active Directory können Sie zwischen zwei Ansätzen wählen.

* Informationen zur Authentifizierung von Endbenutzern für Ihre Anwendung (interaktiv) finden Sie unter [End-user authentication with Data Lake Store using .NET SDK](data-lake-store-end-user-authenticate-rest-api.md) (Authentifizierung von Endbenutzern mit Data Lake Store per .NET SDK).
* Informationen zur Dienst-zu-Dienst-Authentifizierung für Ihre Anwendung (nicht interaktiv) finden Sie unter [Service-to-service authentication with Data Lake Store using .NET SDK](data-lake-store-service-to-service-authenticate-rest-api.md) (Dienst-zu-Dienst-Authentifizierung mit Data Lake Store per .NET SDK).


## <a name="create-a-data-lake-store-account"></a>Erstellen eines Data Lake-Speicherkontos
Dieser Vorgang basiert auf dem [hier](https://msdn.microsoft.com/library/mt694078.aspx)definierten REST-API-Aufruf.

Verwenden Sie den folgenden cURL-Befehl. Ersetzen Sie **\<yourstorename>** durch Ihren Data Lake Store-Namen.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Ersetzen Sie im Befehl oben \<`REDACTED`\> durch das Autorisierungstoken, das Sie zuvor abgerufen haben. Die Anforderungsnutzlast für diesen Befehl ist in der Datei **input.json** enthalten, die für den oben genannten `-d`-Parameter bereitgestellt wird. Der Inhalt der Datei „input.json“ ähnelt dem folgenden Codeausschnitt:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="delete-a-data-lake-store-account"></a>Löschen eines Data Lake-Speicherkontos
Dieser Vorgang basiert auf dem [hier](https://msdn.microsoft.com/library/mt694075.aspx)definierten REST-API-Aufruf.

Verwenden Sie zum Löschen eines Data Lake-Speicherkontos den folgenden cURL-Befehl. Ersetzen Sie **\<yourstorename>** durch Ihren Data Lake Store-Namen.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Die Ausgabe sollte in etwa wie im folgenden Codeausschnitt aussehen:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="next-steps"></a>Nächste Schritte
* [Filesystem operations on Data Lake Store using REST API](data-lake-store-data-operations-rest-api.md) (Dateisystemvorgänge in Data Lake Store mit der REST-API)

## <a name="see-also"></a>Weitere Informationen
* [Azure Data Lake Store – REST-API-Referenz](https://docs.microsoft.com/rest/api/datalakestore/)
* [Open Source-Big Data-Anwendungen, die mit Azure Data Lake-Speicher funktionieren](data-lake-store-compatible-oss-other-applications.md)

