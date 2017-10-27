---
title: "Python: Kontoverwaltungsvorgänge in Azure Data Lake Store | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie das Python SDK für Data Lake Store-Kontoverwaltungsvorgänge verwenden."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2017
ms.author: nitinme
ms.openlocfilehash: 76e84687815ca6f4b031e5f7143ba0079fb053db
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2017
---
# <a name="account-management-operations-on-azure-data-lake-store-using-python"></a>Kontoverwaltungsvorgänge in Azure Data Lake Store mit Python
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Es wird beschrieben, wie Sie das Python SDK für Azure Data Lake Store verwenden, um grundlegende Kontoverwaltungsvorgänge durchzuführen, z.B. Data Lake Store-Konto erstellen, Data Lake Store-Konto auflisten usw. Eine Anleitung, wie Sie Dateisystemvorgänge in Data Lake Store mit Python durchführen, finden Sie unter [Filesystem operations on Data Lake Store using Python](data-lake-store-data-operations-python.md) (Dateisystemvorgänge in Data Lake Store mit Python).

## <a name="prerequisites"></a>Voraussetzungen

* **Python**. Sie können Python [hier](https://www.python.org/downloads/) herunterladen. In diesem Artikel wird Python 3.6.2 verwendet.

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Eine Azure-Ressourcengruppe**. Anweisungen finden Sie unter [Verwalten von Azure-Ressourcen über das Portal](../azure-resource-manager/resource-group-portal.md).

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

2. Fügen Sie den folgenden Codeausschnitt hinzu, um die erforderlichen Module zu importieren:

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

## <a name="create-client-and-data-lake-store-account"></a>Erstellen eines Clients und eines Data Lake Store-Kontos

Der folgende Codeausschnitt erstellt zunächst den Data Lake Store-Kontoclient. Er verwendet das Clientobjekt zum Erstellen eines Data Lake Store-Kontos. Schließlich erstellt der Codeausschnitt ein Dateisystem-Clientobjekt.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'

    ## Create data lake store account management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(armCreds, subscriptionId)

    ## Create a Data Lake Store account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    
## <a name="list-the-data-lake-store-accounts"></a>Auflisten der Data Lake Store-Konten

    ## List the existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="delete-the-data-lake-store-account"></a>Löschen des Data Lake Store-Kontos

    ## Delete the existing Data Lake Store accounts
    adlsAcctClient.account.delete(adlsAccountName)
    

## <a name="next-steps"></a>Nächste Schritte
* [Filesystem operations on Data Lake Store using Python](data-lake-store-data-operations-python.md) (Dateisystemvorgänge in Data Lake Store mit Python)

## <a name="see-also"></a>Weitere Informationen
* [Azure Data Lake Store Python (Account management) Reference](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html) (Python-Referenz zu Azure Data Lake Store (Kontoverwaltung))
* [Azure Data Lake Store Python (Filesystem) Reference](http://azure-datalake-store.readthedocs.io/en/latest) (Python-Referenz zu Azure Data Lake Store (Dateisystem))
* [Open Source-Big Data-Anwendungen, die mit Azure Data Lake-Speicher funktionieren](data-lake-store-compatible-oss-other-applications.md)
