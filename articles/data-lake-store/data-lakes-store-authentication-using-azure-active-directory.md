---
title: Authentifizierung bei Data Lake Store mit Azure Active Directory | Microsoft-Dokumentation
description: Informationen zum Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory
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
ms.date: 10/05/2017
ms.author: nitinme
ms.openlocfilehash: 6130f1a7fd58a50fe5c0133452a945e30899b654
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="authentication-with-data-lake-store-using-azure-active-directory"></a>Authentifizierung bei Data Lake Store mithilfe von Azure Active Directory

Azure Data Lake Store verwendet Azure Active Directory für die Authentifizierung. Vor dem Erstellen einer Anwendung, die mit Azure Data Lake Store funktioniert, müssen Sie entscheiden, wie Sie Ihre Anwendung bei Azure Active Directory (Azure AD) authentifizieren. 

## <a name="authentication-options"></a>Authentifizierungsoptionen

* **Endbenutzerauthentifizierung**: Die Azure-Anmeldeinformationen eines Endbenutzers werden für die Authentifizierung bei Data Lake Store verwendet. Die Anwendung, die Sie zum Arbeiten mit Data Lake Store erstellen, fordert diese Benutzeranmeldeinformationen an. Folglich ist dieser Authentifizierungsmechanismus *interaktiv*, und die Anwendung wird im Kontext des angemeldeten Benutzers ausgeführt. Weitere Informationen und Anweisungen finden Sie unter [Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

* **Authentifizierung zwischen Diensten**: Verwenden Sie diese Option, wenn eine Anwendung sich selbst bei Data Lake Store authentifizieren soll. In solchen Fällen erstellen Sie eine Azure Active Directory-Anwendung (AD) und verwenden den Schlüssel aus der Azure AD-Anwendung für die Authentifizierung bei Data Lake Store. Folglich ist dieser Authentifizierungsmechanismus *nicht interaktiv*. Weitere Informationen und Anweisungen finden Sie unter [Service-to-service authentication with Data Lake Store using Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md) (Authentifizierung zwischen Diensten bei Data Lake Store mit Azure Active Directory).

Die folgende Tabelle verdeutlicht, wie Endbenutzerauthentifizierung und Authentifizierung zwischen Diensten für Data Lake Store unterstützt werden. Lesen Sie die Tabelle wie im Folgenden beschrieben.

* Das Symbol ✔* bedeutet, dass die Authentifizierungsoption unterstützt wird, und bietet einen Link zu einem Artikel, in dem die Authentifizierungsoption veranschaulicht wird. 
* Das Symbol ✔ bedeutet, dass die Authentifizierungsoption unterstützt wird. 
* Leere Zellen bedeuten, dass die Authentifizierungsoption nicht unterstützt wird.


|Verwenden Sie diese Authentifizierungsoption mit...                   |.NET         |Java     |PowerShell |CLI 2.0 | Python   |REST     |
|:---------------------------------------------|:------------|:--------|:----------|:-------------|:---------|:--------|
|Endbenutzer (ohne MFA**)                        |   ✔ |    ✔    |    ✔      |       ✔      |    **[✔ *](data-lake-store-end-user-authenticate-python.md#end-user-authentication-without-multi-factor-authentication)**(veraltet)     |    **[✔*](data-lake-store-end-user-authenticate-rest-api.md)**    |
|Endbenutzer (mit MFA)                           |    **[✔*](data-lake-store-end-user-authenticate-net-sdk.md)**        |    **[✔*](data-lake-store-end-user-authenticate-java-sdk.md)**     |    ✔      |       **[✔*](data-lake-store-get-started-cli-2.0.md)**      |    **[✔*](data-lake-store-end-user-authenticate-python.md#end-user-authentication-with-multi-factor-authentication)**     |    ✔    |
|Zwischen Diensten (mit Clientschlüssel)         |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-client-secret)** |    **[✔*](data-lake-store-service-to-service-authenticate-java.md)**    |    ✔      |       ✔      |    **[✔*](data-lake-store-service-to-service-authenticate-python.md#service-to-service-authentication-with-client-secret-for-account-management)**     |    **[✔*](data-lake-store-service-to-service-authenticate-rest-api.md)**    |
|Zwischen Diensten (mit Clientzertifikat) |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-certificate)**        |    ✔    |    ✔      |       ✔      |    **[✔*](data-lake-store-service-to-service-authenticate-python.md#service-to-service-authentication-with-certificate-for-account-management)**     |    ✔    |

<i>* Klicken Sie auf das Symbol <b>✔\* </b>. Es ist ein Link.</i><br>
<i>**MFA steht für Multi-Factor Authentication (mehrstufige Authentifizierung)</i>.

Weitere Informationen zur Authentifizierung mit Azure Active Directory finden Sie unter [Authentifizierungsszenarien für Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).

## <a name="next-steps"></a>Nächste Schritte

* [Authentifizierung von Endbenutzern](data-lake-store-end-user-authenticate-using-active-directory.md)
* [Dienst-zu-Dienst-Authentifizierung](data-lake-store-service-to-service-authenticate-using-active-directory.md)


