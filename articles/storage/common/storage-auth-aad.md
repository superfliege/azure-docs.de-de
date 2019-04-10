---
title: Authentifizieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory | Microsoft-Dokumentation
description: Authentifizieren Sie den Zugriff auf Azure-Blobs und -Warteschlangen mit Azure Active Directory.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/28/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: e72400a759b976b1a2a6864b2fa7d7d91e16c62f
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58619287"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Authentifizieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory

Azure Storage unterstützt Authentifizierung und Autorisierung mit Azure Active Directory (AD) für Blob- und Warteschlangendienste. Mit Azure AD können Sie mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) den Zugriff auf Benutzer-, Gruppen- oder Anwendungsdienstprinzipale gewährleisten. 

Die Authentifizierung von Benutzern oder Anwendungen mithilfe von Azure AD-Anmeldeinformationen bietet mehr Sicherheit und Benutzerfreundlichkeit als andere Autorisierungsmethoden. Während Sie weiterhin die Autorisierung mit gemeinsam verwendetem Schlüssel mit Ihren Anwendungen verwenden können, macht Azure AD das Speichern Ihrer Kontozugriffsschlüssel mit Ihrem Code überflüssig. Sie können auch weiterhin Shared Access Signatures für zum Gewähren eines differenzierten Zugriffs auf Ressourcen in Ihrem Speicherkonto verwenden. Azure AD bietet jedoch ähnliche Funktionen, bei denen Sie weder SAS-Token verwalten noch sich um das Widerrufen einer gefährdeten SAS kümmern müssen. Microsoft empfiehlt, nach Möglichkeit die Azure AD-Authentifizierung für Ihre Azure Storage-Anwendungen zu nutzen.

Die Authentifizierung und Autorisierung mit Azure AD-Anmeldeinformationen ist für alle Speicherkonten vom Typ „Universell V2“ und vom Typ „Universell V1“ sowie alle Blob Storage-Konten in allen öffentlichen Regionen verfügbar. Die Azure AD-Autorisierung wird nur für Speicherkonten unterstützt, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden.

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>Übersicht über Azure AD für Blobs und Warteschlangen

Im ersten Schritt beim Verwenden der Azure AD-Integration mit Azure Storage weisen Sie RBAC-Rollen für Speicherdaten Ihrem Dienstprinzipal (einem Benutzer-, Gruppen- oder Anwendungsdienstprinzipal) oder den verwalteten Identitäten für Azure-Ressourcen zu. RBAC-Rollen umfassen gängige Sätze von Berechtigungen für Container und Warteschlangen. Weitere Informationen zum Zuweisen von RBAC-Rollen in Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC](storage-auth-aad-rbac.md).

Um Azure AD zum Autorisieren des Zugriffs auf Speicherressourcen in Ihren Anwendungen zu verwenden, müssen Sie ein OAuth 2.0-Zugriffstoken aus dem Code anfordern. Informationen zum Anfordern eines Zugriffstokens und seiner Verwendung zum Autorisieren der Anforderungen an Azure Storage finden Sie unter [Authentifizieren mit Azure AD über eine Azure Storage-Anwendung](storage-auth-aad-app.md). Wenn Sie eine verwaltete Identität nutzen, helfen Ihnen die Informationen unter [Authentifizieren des Zugriffs auf Blobs und Warteschlangen mit verwalteten Azure-Identitäten für Azure-Ressourcen](storage-auth-aad-msi.md) weiter.

Azure CLI und PowerShell unterstützen jetzt die Anmeldung mit einer Azure AD-Identität. Nachdem Sie sich mit einer Azure AD-Identität angemeldet haben, wird die Sitzung mit dieser Identität ausgeführt. Weitere Informationen finden Sie unter [Verwenden einer Azure AD-Identität für den Zugriff auf Azure Storage mit der Befehlszeilenschnittstelle oder PowerShell](storage-auth-aad-script.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC-Rollen für Blobs und Warteschlangen

Azure Active Directory (Azure AD) autorisiert Rechte für den Zugriff auf abgesicherte Ressourcen über die [rollenbasierte Zugriffssteuerung (RBAC)](../../role-based-access-control/overview.md). Azure Storage bietet eine Reihe integrierter RBAC-Rollen mit üblichen Berechtigungssätzen für den Zugriff auf Blob- und Warteschlangendaten. Außerdem können Sie benutzerdefinierte Rollen für den Zugriff auf Blob- und Warteschlangendaten definieren.

Wenn einem Azure AD-Sicherheitsprinzipal eine RBAC-Rolle zugewiesen wird, gewährt Azure diesem Sicherheitsprinzipal Zugriff auf diese Ressourcen. Der Zugriff kann auf die Ebene des Abonnements, der Ressourcengruppe, des Speicherkontos oder eines einzelnen Containers oder einer Warteschlange begrenzt werden. Eine Azure AD-Sicherheitsprinzipal kann ein Benutzer, eine Gruppe, ein Anwendungsdienstprinzipal oder eine [verwaltete Identität für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md) sein.

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Informationen zum Zuweisen einer integrierten RBAC-Rolle für Azure Storage-Ressourcen finden Sie unter den folgenden Themen:

- [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über das Azure-Portal](storage-auth-aad-rbac-portal.md)
- [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über die Azure CLI](storage-auth-aad-rbac-cli.md)
- [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über PowerShell](storage-auth-aad-rbac-powershell.md)

### <a name="access-permissions-granted-by-rbac-roles"></a>Von RBAC-Rollen erteilte Zugriffsberechtigungen 

Einzelheiten zu den Berechtigungen, die für das Aufrufen von Azure Storage-Vorgängen erforderlich sind, finden Sie unter [Berechtigungen für das Aufrufen von REST-Vorgängen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

## <a name="resource-scope"></a>Ressourcenumfang

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Azure Storage support for Azure Active Directory based access control generally available](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/) (Azure Storage-Unterstützung für Azure Active Directory-basierte Zugriffssteuerung allgemein verfügbar)
- [Authentifizieren mit Azure Active Directory aus einer Anwendung für den Zugriff auf Blobs und Warteschlangen](storage-auth-aad-app.md)
- [Authentifizieren des Zugriffs auf Blobs und Warteschlangen mit verwalteten Identitäten für Azure-Ressourcen](storage-auth-aad-msi.md)
- Azure Files unterstützt die Authentifizierung mit Azure AD über SMB nur für in die Domäne eingebundene virtuelle Computer (Vorschau). Informationen über die Verwendung von Azure AD über SMB für Azure Files finden Sie unter [Übersicht über die Azure Active Directory-Authentifizierung über SMB für Azure Files (Vorschau)](../files/storage-files-active-directory-overview.md).