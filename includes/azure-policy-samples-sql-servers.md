---
title: Includedatei
description: Includedatei
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 298b92205e73b3623db02fb1dd803edac8379700
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664596"
---
### <a name="sql-servers"></a>SQL-Server

|  |  |
|---------|---------|
| [Überwachen bei nicht vorhandenem Azure Active Directory-Administrator](../articles/azure-policy/scripts/audit-no-aad-admin.md) | Lassen Sie eine Überwachung zu, wenn dem SQL-Server kein Azure Active Directory-Administrator zugewiesen ist. |
| [Überwachen von Einstellungen für die Bedrohungserkennung auf Serverebene](../articles/azure-policy/scripts/audit-sql-ser-threat-det-setting.md) | Überwacht, ob die Richtlinien der Sicherheitswarnungen für SQL-Datenbanken nicht auf den angegebenen Status festgelegt sind. Sie geben einen Wert an, der angibt, ob die Bedrohungserkennung aktiviert oder deaktiviert ist.  |
| [Überwachen der SQL Server-Überwachungseinstellungen](../articles/azure-policy/scripts/sql-server-audit.md) | Überwacht SQL Server abhängig davon, ob die Überwachungseinstellungen aktiviert sind. |
| [Überwachen von Einstellungen der Überwachung auf SQL-Server-Ebene](../articles/azure-policy/scripts/audit-sql-ser-leve-audit-setting.md) | Überwacht, ob die SQL-Server-Überwachungseinstellungen nicht mit einer angegebenen Einstellung übereinstimmen. Sie geben einen Wert an, der angibt, ob Überwachungseinstellungen aktiviert oder deaktiviert sein sollen. |
| [Verlangen der SQL Server-Version 12.0](../articles/azure-policy/scripts/req-sql-12.md) | Erfordert die Verwendung der SQL Server-Version 12.0.  |