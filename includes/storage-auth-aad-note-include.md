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
ms.openlocfilehash: fbc5ca4d433be65d43ae535703cc7f765dda0a1e
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292683"
---
> [!NOTE]
> - Die Vorschau der Azure AD-Authentifizierung für Blobs und Warteschlangen ist nur für die Verwendung außerhalb der Produktion bestimmt. Produktions-SLAs (Service Level Agreements, Vereinbarungen zum Servicelevel) sind derzeit nicht verfügbar. Wenn die Azure AD-Authentifizierung für Ihr Szenario noch nicht unterstützt wird, verwenden Sie in Ihren Anwendungen weiterhin die Autorisierung mit gemeinsam verwendetem Schlüssel oder SAS-Token.
>
> - In der Vorschauphase kann die Verteilung von RBAC-Rollenzuweisungen bis zu fünf Minuten dauern.
>
> - Um Blob- und Warteschlangenvorgänge mit einem OAuth-Token zu autorisieren, müssen Sie HTTPS verwenden.
>
> - Das Azure-Portal unterstützt nun im Rahmen der Vorschauversion die Verwendung von Azure AD-Anmeldeinformationen zum Lesen und Schreiben von Blob- und Warteschlangendaten.
> 
> - [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) verwendet derzeit Ihren Speicherkontoschlüssel für den Datenzugriff auf Blob- und Warteschlangendaten.
>
> - Azure Files unterstützt die Authentifizierung mit Azure AD über SMB nur für in die Domäne eingebundene virtuelle Computer (Vorschau). Informationen über die Verwendung von Azure AD über SMB für Azure Files finden Sie unter [Übersicht über die Azure Active Directory-Authentifizierung über SMB für Azure Files (Vorschau)](../articles/storage/files/storage-files-active-directory-overview.md).



