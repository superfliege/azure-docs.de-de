---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/09/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 2d641287bcf095f13719667a91b7f61295309b5d
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49430356"
---
> [!NOTE]
> - Die Vorschau der Azure AD-Authentifizierung für Blobs und Warteschlangen ist nur für die Verwendung außerhalb der Produktion bestimmt. Produktions-SLAs (Service Level Agreements, Vereinbarungen zum Servicelevel) sind derzeit nicht verfügbar. Wenn die Azure AD-Authentifizierung für Ihr Szenario noch nicht unterstützt wird, verwenden Sie in Ihren Anwendungen weiterhin die Autorisierung mit gemeinsam verwendetem Schlüssel oder SAS-Token.
>
> - In der Vorschauphase kann die Verteilung von RBAC-Rollenzuweisungen bis zu fünf Minuten dauern.
>
> - Beim Aufrufen von Blob- und Warteschlangenvorgängen müssen Sie HTTPS für die Authentifizierung über Azure AD verwenden.
>
> - Das Azure-Portal unterstützt nun im Rahmen der Vorschauversion die Verwendung von Azure AD-Anmeldeinformationen zum Lesen und Schreiben von Blobdaten.
> 
> - Im Azure-Portal werden gegenwärtig keine Azure AD-Anmeldeinformationen zum Lesen und Schreiben von Warteschlangendaten unterstützt. Der Zugriff auf Warteschlangendaten erfolgt über Ihre Speicherkontoschlüssel.
>
> - [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) verwendet derzeit Ihren Speicherkontoschlüssel für den Datenzugriff auf Blob- und Warteschlangendaten.
>
> - Azure Files unterstützt die Authentifizierung mit Azure AD über SMB nur für in die Domäne eingebundene virtuelle Computer (Vorschau). Informationen über die Verwendung von Azure AD über SMB für Azure Files finden Sie unter [Übersicht über die Azure Active Directory-Authentifizierung über SMB für Azure Files (Vorschau)](../articles/storage/files/storage-files-active-directory-overview.md).



