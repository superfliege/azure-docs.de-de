---
title: Authentifizieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory (Vorschauversion) | Microsoft-Dokumentation
description: Enthält Informationen zum Authentifizieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory (Vorschauversion).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/13/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: dcf7d237c8cfbf52a804e428d84fff0bb328c7c8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58012103"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory-preview"></a>Authentifizieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory (Vorschauversion)

Azure Storage unterstützt Authentifizierung und Autorisierung mit Azure Active Directory (AD) für Blob- und Warteschlangendienste. Mit Azure AD können Sie mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) den Zugriff auf Benutzer-, Gruppen- oder Anwendungsdienstprinzipale gewährleisten. 

Die Authentifizierung von Benutzern oder Anwendungen mithilfe von Azure AD-Anmeldeinformationen bietet mehr Sicherheit und Benutzerfreundlichkeit als andere Autorisierungsmethoden. Während Sie weiterhin die Autorisierung mit gemeinsam verwendetem Schlüssel mit Ihren Anwendungen verwenden können, macht Azure AD das Speichern Ihrer Kontozugriffsschlüssel mit Ihrem Code überflüssig. Sie können auch weiterhin Shared Access Signatures für zum Gewähren eines differenzierten Zugriffs auf Ressourcen in Ihrem Speicherkonto verwenden. Azure AD bietet jedoch ähnliche Funktionen, bei denen Sie weder SAS-Token verwalten noch sich um das Widerrufen einer gefährdeten SAS kümmern müssen. Microsoft empfiehlt, nach Möglichkeit die Azure AD-Authentifizierung für Ihre Azure Storage-Anwendungen zu nutzen.

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="about-the-preview"></a>Informationen zur Vorschau

Berücksichtigen Sie bei der Vorschau Folgendes:

- Die Azure AD-Integration ist für die Blob- und Warteschlangendienste nur in der Vorschau verfügbar.
- Die Azure AD-Integration ist für GPv1, GPv2 und Blobspeicherkonten in allen öffentlichen Regionen verfügbar. 
- Nur Speicherkonten, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden, werden unterstützt. 
- Die Unterstützung für Aufruferidentitätsinformationen in der Azure Storage Analytics-Protokollierung wird bald verfügbar sein.
- Azure AD-Autorisierung des Zugriffs auf Ressourcen in Standardspeicherkonten wird derzeit unterstützt. Autorisierung des Zugriffs auf Seitenblobs in Premium-Speicherkonten wird bald unterstützt.
- Azure Storage unterstützt sowohl integrierte als auch benutzerdefinierte RBAC-Rollen. Sie können Rollen auf der Ebene des Abonnements, der Ressourcengruppe, des Speicherkontos oder eines einzelnen Containers oder einer Warteschlange zuweisen.
- Zu den Azure Storage-Clientbibliotheken, die derzeit Azure AD-Integration unterstützen, zählen:
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage)
    - [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)
    - Python
        - [Blob, Queue und Files](https://github.com/Azure/azure-storage-python)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs)

## <a name="overview-of-azure-ad-for-storage"></a>Übersicht über Azure AD für Azure Storage

Im ersten Schritt beim Verwenden der Azure AD-Integration mit Azure Storage weisen Sie RBAC-Rollen für Speicherdaten Ihrem Dienstprinzipal (einem Benutzer-, Gruppen- oder Anwendungsdienstprinzipal) oder den verwalteten Identitäten für Azure-Ressourcen zu. RBAC-Rollen umfassen gängige Sätze von Berechtigungen für Container und Warteschlangen. Weitere Informationen zum Zuweisen von RBAC-Rollen zu Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC (Vorschau)](storage-auth-aad-rbac.md).

Um Azure AD zum Autorisieren des Zugriffs auf Speicherressourcen in Ihren Anwendungen zu verwenden, müssen Sie ein OAuth 2.0-Zugriffstoken aus dem Code anfordern. Informationen zum Anfordern eines Zugriffstokens und seiner Verwendung zum Autorisieren der Anforderungen an Azure Storage finden Sie unter [Authentifizieren mit Azure AD über eine Azure Storage-Anwendung (Vorschau)](storage-auth-aad-app.md). Wenn Sie eine verwaltete Identität nutzen, helfen Ihnen die Informationen unter [Authentifizieren des Zugriffs auf Blobs und Warteschlangen mit verwalteten Azure-Identitäten für Azure-Ressourcen (Vorschau)](storage-auth-aad-msi.md) weiter.

Azure CLI und PowerShell unterstützen jetzt die Anmeldung mit einer Azure AD-Identität. Nachdem Sie sich mit einer Azure AD-Identität angemeldet haben, wird die Sitzung mit dieser Identität ausgeführt. Weitere Informationen finden Sie unter [Verwenden einer Azure AD Identity für den Zugriff auf Azure Storage mit der Befehlszeilenschnittstelle oder PowerShell (Vorschau)](storage-auth-aad-script.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC-Rollen für Blobs und Warteschlangen

Azure Active Directory (Azure AD) autorisiert Rechte für den Zugriff auf abgesicherte Ressourcen über die [rollenbasierte Zugriffssteuerung (RBAC)](../../role-based-access-control/overview.md). Azure Storage bietet eine Reihe integrierter RBAC-Rollen mit üblichen Berechtigungssätzen für den Zugriff auf Container und Warteschlangen. 

Wenn einem Azure AD-Sicherheitsprinzipal eine RBAC-Rolle zugewiesen wird, gewährt Azure diesem Sicherheitsprinzipal Zugriff auf diese Ressourcen. Der Zugriff kann auf die Ebene des Abonnements, der Ressourcengruppe, des Speicherkontos oder eines einzelnen Containers oder einer Warteschlange begrenzt werden. Eine Azure AD-Sicherheitsprinzipal kann ein Benutzer, eine Gruppe, ein Anwendungsdienstprinzipal oder eine [verwaltete Identität für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md) sein. 

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Weitere Informationen zum Zuweisen einer integrierten Rolle im Azure-Portal finden Sie unter [Grant access to Azure containers and queues with RBAC in the Azure portal (preview)](storage-auth-aad-rbac.md) (Gewähren des Zugriff auf Azure-Container- und Warteschlangendaten im Azure-Portal mithilfe von RBAC [Vorschau]).

### <a name="access-permissions-granted-by-rbac-roles"></a>Von RBAC-Rollen erteilte Zugriffsberechtigungen 

In der folgenden Tabelle sind die von den integrierten Rollen erteilten Zugriffsrechte für verschiedene Geltungsbereichsebenen zusammengefasst:

|Bereich|Blobdatenbesitzer|Mitwirkender an Blobdaten|Leser von Blobdaten|Mitwirkender an Warteschlangendaten|Leser von Warteschlangendaten|
|---|---|---|---|---|---|
|Abonnementebene|Lese-/Schreibzugriff und Verwaltung der POSIX-Zugriffsteuerung für alle Container und Blobs im Abonnement|Lese-/Schreibzugriff auf alle Container und Blobs im Abonnement| Lesezugriff auf alle Container und Blobs im Abonnement|Lese-/Schreibzugriff auf alle Warteschlangen im Abonnement|Lesezugriff auf alle Warteschlangen im Abonnement|
|Ressourcengruppenebene|Lese-/Schreibzugriff und Verwaltung der POSIX-Zugriffsteuerung für alle Container und Blobs in der Ressourcengruppe|Lese-/Schreibzugriff auf alle Container und Blobs in der Ressourcengruppe|Lesezugriff auf alle Container und Blobs in der Ressourcengruppe|Lese-/Schreibzugriff auf alle Warteschlangen in der Ressourcengruppe|Lesezugriff auf alle Warteschlangen in der Ressourcengruppe|
|Speicherkontoebene|Lese-/Schreibzugriff und Verwaltung der POSIX-Zugriffsteuerung für alle Container und Blobs im Speicherkonto|Lese-/Schreibzugriff auf alle Container und Blobs im Speicherkonto|Lesezugriff auf alle Container und Blobs im Speicherkonto|Lese-/Schreibzugriff auf alle Warteschlangen im Speicherkonto|Lesezugriff auf alle Warteschlangen im Speicherkonto|
|Container-/Warteschlangenebene|Lese-/Schreibzugriff und Verwaltung der POSIX-Zugriffsteuerung für die angegebenen Container und die dazugehörigen Blobs|Lese-/Schreibzugriff auf den angegebenen Container und seine Blobs|Lesezugriff auf den angegebenen Container und seine Blobs|Lese-/Schreibzugriff auf die angegebene Warteschlange|Lesezugriff auf die angegebene Warteschlange|

Einzelheiten zu den Berechtigungen, die für das Aufrufen von Azure Storage-Vorgängen erforderlich sind, finden Sie unter [Berechtigungen für das Aufrufen von REST-Vorgängen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure AD-Integration für Azure-Blobs und -Warteschlangen finden Sie im Blogbeitrag des Azure Storage-Teams [Announcing the Preview of Azure AD Authentication for Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/) (Ankündigung der Vorschau der Azure AD-Authentifizierung für Azure Storage).
