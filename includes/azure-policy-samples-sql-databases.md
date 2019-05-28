---
title: Includedatei
description: Includedatei
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 10/29/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 7bf885f2ab81e5d86878d2b0b53f4e98b8aedd9a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155697"
---
### <a name="sql-databases"></a>SQL-DATENBANKEN

|  |  |
|---------|---------|
| [Zulässige SQL-Datenbank-SKUs](../articles/governance/policy/samples/allowed-sql-db-skus.md) | Erfordert, dass SQL-Datenbanken eine genehmigte SKU verwenden. Sie geben ein Array von zulässigen SKU-IDs oder von zulässigen SKU-Namen an. |
| [Überwachen der Einstellung für die Bedrohungserkennung auf Datenbankebene](../articles/governance/policy/samples/audit-db-threat-detection-setting.md) | Überwacht, ob die Richtlinien der Sicherheitswarnungen für SQL-Datenbanken nicht auf den angegebenen Status festgelegt sind. Sie geben einen Wert an, der angibt, ob die Bedrohungserkennung aktiviert oder deaktiviert ist.  |
| [Überwachen der SQL-Datenbankverschlüsselung](../articles/governance/policy/samples/sql-database-encryption-audit.md) | Überwacht, ob die transparente Datenverschlüsselung für die SQL-Datenbank deaktiviert ist. |
| [Überwachen von Einstellungen der Überwachung auf SQL-Datenbank-Ebene](../articles/governance/policy/samples/audit-sql-db-audit-setting.md) | Überwacht, ob die Einstellungen der SQL Datenbanküberwachung nicht mit einer bestimmten Einstellung übereinstimmen Sie geben einen Wert an, der angibt, ob Überwachungseinstellungen aktiviert oder deaktiviert sein sollen.  |