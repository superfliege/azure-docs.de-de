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
ms.openlocfilehash: 0182df40a4e7815560a85e60fe9062ccd8001c18
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52978845"
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
> - [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) verwendet derzeit Ihren Speicherkontoschlüssel für den Datenzugriff auf Blob- und Warteschlangendaten. OAuth-Zugriff wird für Blobs unterstützt.
>
> - Azure Files unterstützt die Authentifizierung mit Azure AD über SMB nur für in die Domäne eingebundene virtuelle Computer (Vorschau). Informationen über die Verwendung von Azure AD über SMB für Azure Files finden Sie unter [Übersicht über die Azure Active Directory-Authentifizierung über SMB für Azure Files (Vorschau)](../articles/storage/files/storage-files-active-directory-overview.md).