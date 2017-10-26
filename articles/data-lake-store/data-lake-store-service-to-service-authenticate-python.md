---
title: 'Authentifizierung zwischen Diensten: Python mit Data Lake Store mit Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie die Authentifizierung zwischen Diensten mit Data Lake Store mithilfe von Azure Active Directory und Python implementieren.
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2017
ms.author: nitinme
ms.openlocfilehash: 9f23a5e24067dab29a21a44404a912588b46d3b0
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2017
---
# <a name="service-to-service-authentication-with-data-lake-store-using-python"></a>Authentifizierung zwischen Diensten mit Data Lake Store mithilfe von Python
> [!div class="op_single_selector"]
> * [Verwenden von Java](data-lake-store-service-to-service-authenticate-java.md)
> * [Verwenden des .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Verwenden von Python](data-lake-store-service-to-service-authenticate-python.md)
> * [Verwenden der REST-API](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

In diesem Artikel erfahren Sie, wie Sie mithilfe des Python-SDK die Authentifizierung zwischen Diensten bei Azure Data Lake Store durchführen. Informationen zur Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Python finden Sie unter [End-user authentication with Data Lake Store using Python](data-lake-store-end-user-authenticate-python.md) (Authentifizierung von Endbenutzern bei Data Lake Store per Python).


## <a name="prerequisites"></a>Voraussetzungen

* **Python**. Sie können Python [hier](https://www.python.org/downloads/) herunterladen. In diesem Artikel wird Python 3.6.2 verwendet.

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Erstellen einer Azure Active Directory-Webanwendung.** Sie müssen die Schritte unter [Dienst-zu-Dienst-Authentifizierung bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md) ausgeführt haben.

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
    ## Use this for Azure AD authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    import adal
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. Speichern Sie Ihre Änderungen an „mysample.py“.

## <a name="service-to-service-authentication-with-client-secret-for-account-management"></a>Dienst-zu-Dienst-Authentifizierung mit Clientgeheimnis für die Kontoverwaltung

Verwenden Sie diesen Codeausschnitt zur Authentifizierung bei Azure AD für Kontoverwaltungsvorgänge bei Data Lake Store, z.B. Erstellen oder Löschen eines Data Lake Store-Kontos. Mit dem folgenden Codeausschnitt können Sie unter Verwendung des Clientgeheimnisses für eine Anwendung bzw. einen Dienstprinzipal einer bestehenden Azure AD-„Web-App“-Anwendung eine nicht interaktive Authentifizierung Ihrer Anwendung durchführen.

    authority_host_uri = 'https://login.microsoftonline.com'
    tenant = '<TENANT>'
    authority_uri = authority_host_uri + '/' + tenant
    RESOURCE = 'https://management.core.windows.net/'
    client_id = '<CLIENT_ID>'
    client_secret = '<CLIENT_SECRET>'
    
    context = adal.AuthenticationContext(authority_uri, api_version=None)
    mgmt_token = context.acquire_token_with_client_credentials(RESOURCE, client_id, client_secret)
    armCreds = AADTokenCredentials(mgmt_token, client_id, resource=RESOURCE)

## <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a>Dienst-zu-Dienst-Authentifizierung mit Clientgeheimnis für Dateisystemvorgänge

Verwenden Sie den folgenden Codeausschnitt für die Authentifizierung mit Azure AD für Dateisystemvorgänge in Data Lake Store wie Erstellen von Ordnern, Hochladen von Dateien usw. Mit dem folgenden Codeausschnitt können Sie unter Verwendung des Clientgeheimnisses für eine Anwendung bzw. einen Dienstprinzipal eine nicht interaktive Authentifizierung Ihrer Anwendung durchführen. Verwenden Sie diese Option mit einer vorhandenen Azure AD-Anwendung vom Typ „Web-App“.

    adlCreds = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE', resource = 'https://datalake.azure.net/')

<!-- ## Service-to-service authentication with certificate for account management

Use this snippet to authenticate with Azure AD for account management operations on Data Lake Store such as create Data Lake Store account, delete Data Lake Store account, etc. The following snippet can be used to authenticate your application non-interactively, using the certificate of an existing Azure AD "Web App" application. For instructions on how to create an Azure AD application, see [Create service principal with certificates](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).

    authority_host_uri = 'https://login.microsoftonline.com'
    tenant = '<TENANT>'
    authority_uri = authority_host_uri + '/' + tenant
    resource_uri = 'https://management.core.windows.net/'
    client_id = '<CLIENT_ID>'
    client_cert = '<CLIENT_CERT>'
    client_cert_thumbprint = '<CLIENT_CERT_THUMBPRINT>'

    context = adal.AuthenticationContext(authority_uri, api_version=None)
    mgmt_token = context.acquire_token_with_client_certificate(resource_uri, client_id, client_cert, client_cert_thumbprint)
    credentials = AADTokenCredentials(mgmt_token, client_id) -->

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie die Authentifizierung zwischen Diensten verwenden, um sich mit Python bei Azure Data Lake Store zu authentifizieren. In den folgenden Artikeln wird erörtert, wie Sie Python mit Azure Data Lake Store verwenden.

* [Erste Schritte mit Azure Data Lake Store mit Python](data-lake-store-get-started-python.md)
* [Dateisystemvorgänge in Azure Data Lake Store per REST-API](data-lake-store-data-operations-python.md)


