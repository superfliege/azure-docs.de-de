---
title: Includedatei
description: Includedatei
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 76e161be1adca4aa2ec7b682a13b0a42e4b79412
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003736"
---
### <a name="sql-servers"></a>SQL-Server

|  |  |
|---------|---------|
| [Überwachen bei nicht vorhandenem Azure Active Directory-Administrator](../articles/governance/policy/samples/audit-no-aad-admin.md) | Lassen Sie eine Überwachung zu, wenn dem SQL-Server kein Azure Active Directory-Administrator zugewiesen ist. |
| [Überwachen von Einstellungen für die Bedrohungserkennung auf Serverebene](../articles/governance/policy/samples/audit-sql-ser-threat-det-setting.md) | Überwacht, ob die Richtlinien der Sicherheitswarnungen für SQL-Datenbanken nicht auf den angegebenen Status festgelegt sind. Sie geben einen Wert an, der angibt, ob die Bedrohungserkennung aktiviert oder deaktiviert ist.  |
| [Überwachen der SQL Server-Überwachungseinstellungen](../articles/governance/policy/samples/sql-server-audit.md) | Überwacht SQL Server abhängig davon, ob die Überwachungseinstellungen aktiviert sind. |
| [Überwachen von Einstellungen der Überwachung auf SQL-Server-Ebene](../articles/governance/policy/samples/audit-sql-ser-leve-audit-setting.md) | Überwacht, ob die SQL-Server-Überwachungseinstellungen nicht mit einer angegebenen Einstellung übereinstimmen. Sie geben einen Wert an, der angibt, ob Überwachungseinstellungen aktiviert oder deaktiviert sein sollen. |
| [Verlangen der SQL Server-Version 12.0](../articles/governance/policy/samples/req-sql-12.md) | Erfordert die Verwendung der SQL Server-Version 12.0.  |