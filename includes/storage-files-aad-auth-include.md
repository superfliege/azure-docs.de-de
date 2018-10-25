---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/04/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: c2dc0f2dea752dd44ab2805a0d6fc0e5517f5c6c
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48831703"
---
[Azure Files](../articles/storage/files/storage-files-introduction.md) unterstützt identitätsbasierte Authentifizierung über SMB (Server Message Block) (Vorschau) über [Azure Active Directory (Azure AD) Domain Services](../articles/active-directory-domain-services/active-directory-ds-overview.md). Ihre in die Domäne eingebundenen virtuellen Windows-Computer (VMs) können mit [Azure AD](../articles/active-directory/fundamentals/active-directory-whatis.md)-Anmeldeinformationen auf Azure-Dateifreigaben zugreifen. 

Azure AD authentifiziert, eine Identität, z. B. einen Benutzer, eine Gruppe oder einen Dienstprinzipal, mittels [rollenbasierter Zugriffssteuerung (RBAC)](../articles/role-based-access-control/overview.md). Sie können benutzerdefinierte RBAC-Rollen definieren, die gängige Berechtigungssätze für den Zugriff auf Azure Files enthalten. Wenn Sie Ihre benutzerdefinierte RBAC-Rolle einer Azure AD-Identität zuweisen, erhält diese Identität Zugriff auf eine Azure-Dateifreigabe gemäß diesen Berechtigungen.

Als Teil der Preview unterstützt Azure Files auch die Beibehaltung, Vererbung und Durchsetzung von [NTFS DACLs](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) für alle Dateien und Verzeichnisse in einer Dateifreigabe. Wenn Sie Daten von einer Dateifreigabe in Azure Files kopieren, oder umgekehrt, können Sie angeben, dass NTFS DACLs beibehalten werden sollen. Auf diese Weise können Sie Sicherungsszenarien unter Verwendung von Azure Files implementieren, wobei Ihre NTFS DACLS zwischen Ihrer lokalen Dateifreigabe und Ihrer Clouddateifreigabe erhalten bleiben. 

> [!NOTE]
> Azure AD-Authentifizierung über SMB wird in der Preview-Version nicht für Linux-VMs unterstützt. Nur Windows Server-VMs werden unterstützt.

> Azure AD-Authentifizierung über SMB wird nicht für lokale Computer unterstützt, die auf Azure Files mit AD- oder AAD-Anmeldeinformationen zugreifen.

> Azure AD-Authentifizierung ist nur für Speicherkonten verfügbar, die nach dem 24. September 2018 erstellt wurden.
