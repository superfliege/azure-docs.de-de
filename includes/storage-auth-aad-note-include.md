---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/06/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 3a3ee0cda3643ac73581f868e47905841d9f1563
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095590"
---
> [!IMPORTANT]
> - Die Vorschau der Azure AD-Authentifizierung für Blobs und Warteschlangen ist nur für die Verwendung außerhalb der Produktion bestimmt. Produktions-SLAs (Service Level Agreements, Vereinbarungen zum Servicelevel) sind derzeit nicht verfügbar. Wenn die Azure AD-Authentifizierung für Ihr Szenario noch nicht unterstützt wird, verwenden Sie in Ihren Anwendungen weiterhin die Autorisierung mit gemeinsam verwendetem Schlüssel oder SAS-Token.
>
> - In der Vorschauphase kann die Verteilung von RBAC-Rollenzuweisungen bis zu fünf Minuten dauern.
>
> - Beim Aufrufen von Blob- und Warteschlangenvorgängen müssen Sie HTTPS für die Authentifizierung über Azure AD verwenden.
>
> - In der Vorschauversion werden im Azure-Portal keine Azure AD-Anmeldeinformationen zum Lesen und Schreiben von Blob- und Warteschlangendaten verwendet. Stattdessen wird im Portal weiterhin der Zugriffsschlüssel für das Konto unterstützt. Zum Anzeigen oder Aktualisieren von Blob- oder Warteschlangendaten im Portal muss dem Benutzer eine RBAC-Rolle zugewiesen sein, die [Microsoft.Storage/storageAccounts/listkeys/action](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role) umfasst, wodurch Berechtigungen zum Aufrufen von [Speicherkonten – Schlüssel auflisten](https://docs.microsoft.com/rest/api/storagerp/storageaccounts/listkeys) erteilt werden. Die Rollen „Mitwirkender“ und „Leser“ für Blobs und Warteschlangen beinhalten derzeit nicht die Aktion **listkeys** als Teil der Vorschauversion und bieten somit keinen Zugriff auf Daten über das Azure-Portal. Den identitätsbasierten Zugriff auf Blob- und Warteschlangendaten können Sie in der Vorschauversion über PowerShell oder die Azure-Befehlszeilenschnittstelle testen.
