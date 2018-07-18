---
title: Authentifizieren des Zugriffs auf Azure Storage mit Azure Active Directory (Vorschau) | Microsoft-Dokumentation
description: Authentifizieren des Zugriffs auf Azure Storage mit Azure Active Directory (Vorschau).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 06/01/2018
ms.author: tamram
ms.openlocfilehash: 9a0782b96b45d27c9b7e603959ecadf5b2632064
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34737645"
---
# <a name="authenticate-access-to-azure-storage-using-azure-active-directory-preview"></a>Authentifizieren des Zugriffs auf Azure Storage mit Azure Active Directory (Vorschau)

Azure Storage unterstützt Authentifizierung und Autorisierung mit Azure Active Directory (AD) für Blob- und Warteschlangendienste. Mit Azure AD können Sie mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) den Zugriff auf Benutzer-, Gruppen- oder Anwendungsdienstprinzipale gewährleisten. 

Das Autorisieren von Anwendungen, die mithilfe von Azure AD auf Azure Storage zugreifen, bietet gegenüber anderen Autorisierungsoptionen herausragende Sicherheit und Benutzerfreundlichkeit. Während Sie weiterhin die Autorisierung mit gemeinsam verwendetem Schlüssel mit Ihren Anwendungen verwenden können, macht Azure AD das Speichern Ihrer Kontozugriffsschlüssel mit Ihrem Code überflüssig. Ebenso können Sie weiterhin Shared Access Signatures (SAS) für einen präzisen Zugriff auf Ressourcen in Ihrem Speicherkonto verwenden, aber Azure AD bietet ähnliche Funktionen ohne die Notwendigkeit, SAS-Token zu verwalten oder sich um das Widerrufen einer gefährdeten SAS kümmern zu müssen.

## <a name="about-the-preview"></a>Informationen zur Vorschau

Berücksichtigen Sie bei der Vorschau Folgendes:

- Die Azure AD-Integration ist für die Blob- und Warteschlangendienste nur in der Vorschau verfügbar.
- Die Azure AD-Integration ist für GPv1, GPv2 und Blobspeicherkonten in allen öffentlichen Regionen verfügbar. 
- Nur Speicherkonten, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden, werden unterstützt. 
- Die Unterstützung für Aufruferidentitätsinformationen in der Azure Storage Analytics-Protokollierung wird bald verfügbar sein.
- Azure AD-Autorisierung des Zugriffs auf Ressourcen in Standardspeicherkonten wird derzeit unterstützt. Autorisierung des Zugriffs auf Seitenblobs in Premium-Speicherkonten wird bald unterstützt.
- Azure Storage unterstützt sowohl integrierte als auch benutzerdefinierte RBAC-Rollen. Sie können Rollen auf der Ebene des Abonnements, der Ressourcengruppe, des Speicherkontos oder eines einzelnen Containers oder einer Warteschlange zuweisen.
- Zu den Azure Storage-Clientbibliotheken, die derzeit Azure AD-Integration unterstützen, zählen:
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0)
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) (7.1.x-Vorschau verwenden)
    - Python
        - [Blob](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-blob)
        - [Warteschlange](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-queue)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs))

> [!IMPORTANT]
> Diese Vorschau ist nur für die Verwendung außerhalb der Produktion bestimmt. Produktions-SLAs (Service Level Agreements, Vereinbarungen zum Servicelevel) sind erst dann verfügbar, wenn die Azure AD-Integration für Azure Storage als allgemein verfügbar deklariert wird. Wenn die Azure AD-Integration für Ihr Szenario noch nicht unterstützt wird, verwenden Sie in Ihren Anwendungen weiterhin die Autorisierung mit gemeinsam verwendetem Schlüssel oder SAS-Token.
>
> In der Vorschauphase kann die Verteilung von RBAC-Rollenzuweisungen bis zu fünf Minuten dauern.
>
> Für die Azure AD-Integration mit Azure Storage müssen Sie HTTPS für Azure Storage-Vorgänge verwenden.

## <a name="get-started-with-azure-ad-for-storage"></a>Erste Schritte mit Azure AD für Storage

Der erste Schritt beim Verwenden der Azure AD-Integration mit Azure Storage ist, dass Sie RBAC-Rollen für Speicherdaten Ihrem Dienstprinzipal (einem Benutzer-, Gruppen- oder Anwendungsdienstprinzipal) oder der verwalteten Dienstidentität (Managed Service Identity, MSI) zuweisen. RBAC-Rollen umfassen gängige Sätze von Berechtigungen für Container und Warteschlangen. Weitere Informationen zu RBAC-Rollen für Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC (Vorschau)](storage-auth-aad-rbac.md).

Um Azure AD zum Autorisieren des Zugriffs auf Speicherressourcen in Ihren Anwendungen zu verwenden, müssen Sie ein OAuth 2.0-Zugriffstoken aus dem Code anfordern. Informationen zum Anfordern eines Zugriffstokens und seiner Verwendung zum Autorisieren der Anforderungen an Azure Storage finden Sie unter [Authentifizieren mit Azure AD über eine Azure Storage-Anwendung (Vorschau)](storage-auth-aad-app.md). Wenn Sie eine verwaltete Dienstidentität (Managed Service Identity, MSI) von Azure verwenden, lesen Sie [Authentifizieren mit Azure AD über eine verwaltete Azure-VM-Dienstidentität (Vorschau)](storage-auth-aad-msi.md).

Azure CLI und PowerShell unterstützen jetzt die Anmeldung mit einer Azure AD-Identität. Nachdem Sie sich mit einer Azure AD-Identität angemeldet haben, wird die Sitzung mit dieser Identität ausgeführt. Weitere Informationen finden Sie unter [Verwenden einer Azure AD Identity für den Zugriff auf Azure Storage mit der Befehlszeilenschnittstelle oder PowerShell (Vorschau)](storage-auth-aad-script.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure AD-Integration für Azure-Blobs und -Warteschlangen finden Sie im Blogbeitrag des Azure Storage-Teams [Announcing the Preview of Azure AD Authentication for Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/) (Ankündigung der Vorschau der Azure AD-Authentifizierung für Azure Storage).
