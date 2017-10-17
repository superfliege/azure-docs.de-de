---
title: "Python: Dateisystemvorgänge in Azure Data Lake Store | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie das Python SDK für das Data Lake Store-Dateisystem verwenden."
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
ms.openlocfilehash: 0c3bda65bd40a5d24e4c4ab3dcbbbf27fbbb87c9
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

In diesem Artikel wird beschrieben, wie Sie das Python SDK verwenden, um Dateisystemvorgänge in Azure Data Lake Store durchzuführen. Eine Anleitung zum Durchführen von Kontoverwaltungsvorgängen in Data Lake Store per Python finden Sie unter [Erste Schritte mit Azure Data Lake Store mit Python](data-lake-store-get-started-python.md).

## <a name="prerequisites"></a>Voraussetzungen

* **Python**. Sie können Python [hier](https://www.python.org/downloads/) herunterladen. In diesem Artikel wird Python 3.6.2 verwendet.

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake-Speicherkonto**. Befolgen Sie die Anweisungen unter [Erste Schritte mit Azure Data Lake Store über das Azure-Portal](data-lake-store-get-started-portal.md).

## <a name="install-the-modules"></a>Installieren der Module

Zum Verwenden von Data Lake Store mit Python müssen Sie drei Module installieren.

* Das Modul `azure-mgmt-resource`, in dem Azure-Module für Active Directory usw. enthalten sind.
* Das Modul `azure-mgmt-datalake-store`, das die Kontoverwaltungsvorgänge für Azure Data Lake Store enthält. Weitere Informationen zu diesem Modul finden Sie in der [Referenz zum Verwaltungsmodul für Azure Data Lake Store](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).
* Das Modul `azure-datalake-store`, das die Dateisystemvorgänge für Azure Data Lake Store enthält. Weitere Informationen zu diesem Modul finden Sie in der [Referenz zum Dateisystemmodul für Azure Data Lake Store](http://azure-datalake-store.readthedocs.io/en/latest/).

Verwenden Sie die folgenden Befehle, um die Module zu installieren:

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

1. Erstellen Sie in der IDE Ihrer Wahl eine neue Python-Anwendung, z.B. **mysample.py**.

2. Fügen Sie die folgenden Zeilen hinzu, um die erforderlichen Module zu importieren:

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. Speichern Sie Ihre Änderungen an „mysample.py“.

## <a name="authentication"></a>Authentifizierung

In diesem Abschnitt werden die unterschiedlichen Möglichkeiten zur Authentifizierung mit Azure AD beschrieben. Die verfügbaren Optionen sind:

* Informationen zur Authentifizierung von Endbenutzern für Ihre Anwendung finden Sie unter [End-user authentication with Data Lake Store using Python](data-lake-store-end-user-authenticate-python.md) (Authentifizierung von Endbenutzern mit Data Lake Store per Python).
* Informationen zur Dienst-zu-Dienst-Authentifizierung für Ihre Anwendung finden Sie unter [Service-to-service authentication with Data Lake Store using Python](data-lake-store-service-to-service-authenticate-python.md) (Dienst-zu-Dienst-Authentifizierung mit Data Lake Store per Python).

## <a name="create-filesystem-client"></a>Erstellen des Dateisystemclients

Der folgende Codeausschnitt erstellt zunächst den Data Lake Store-Kontoclient. Er verwendet das Clientobjekt zum Erstellen eines Data Lake Store-Kontos. Schließlich erstellt der Codeausschnitt ein Dateisystem-Clientobjekt.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(token, store_name=adlsAccountName)

## <a name="create-a-directory"></a>Erstellen eines Verzeichnisses

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a>Hochladen einer Datei


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a>Herunterladen einer Datei

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a>Löschen eines Verzeichnisses

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="next-steps"></a>Nächste Schritte
* [Erste Schritte mit Azure Data Lake Store mit Python](data-lake-store-get-started-python.md)

## <a name="see-also"></a>Weitere Informationen
* [Azure Data Lake Store Python (Account management) Reference](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html) (Python-Referenz zu Azure Data Lake Store (Kontoverwaltung))
* [Azure Data Lake Store Python (Filesystem) Reference](http://azure-datalake-store.readthedocs.io/en/latest) (Python-Referenz zu Azure Data Lake Store (Dateisystem))
* [Open Source-Big Data-Anwendungen, die mit Azure Data Lake-Speicher funktionieren](data-lake-store-compatible-oss-other-applications.md)
