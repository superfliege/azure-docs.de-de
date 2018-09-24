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
ms.openlocfilehash: 70128735aef64d273c63236a13b0ae28edb6077d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003782"
---
### <a name="sql-databases"></a>SQL-DATENBANKEN

|  |  |
|---------|---------|
| [Zulässige SQL-Datenbank-SKUs](../articles/governance/policy/samples/allowed-sql-db-skus.md) | Erfordert, dass SQL-Datenbanken eine genehmigte SKU verwenden. Sie geben ein Array von zulässigen SKU-IDs oder von zulässigen SKU-Namen an. |
| [Überwachen der Einstellung für die Bedrohungserkennung auf Datenbankebene](../articles/governance/policy/samples/audit-db-threat-det-setting.md) | Überwacht, ob die Richtlinien der Sicherheitswarnungen für SQL-Datenbanken nicht auf den angegebenen Status festgelegt sind. Sie geben einen Wert an, der angibt, ob die Bedrohungserkennung aktiviert oder deaktiviert ist.  |
| [Überwachen der SQL-Datenbankverschlüsselung](../articles/governance/policy/samples/sql-database-encryption-audit.md) | Überwacht, ob die transparente Datenverschlüsselung für die SQL-Datenbank deaktiviert ist. |
| [Überwachen von Einstellungen der Überwachung auf SQL-Datenbank-Ebene](../articles/governance/policy/samples/audit-sql-db-audit-setting.md) | Überwacht, ob die Einstellungen der SQL Datenbanküberwachung nicht mit einer bestimmten Einstellung übereinstimmen Sie geben einen Wert an, der angibt, ob Überwachungseinstellungen aktiviert oder deaktiviert sein sollen.  |
| [Überwachen des Status der transparenten Datenverschlüsselung](../articles/governance/policy/samples/audit-trans-data-enc-status.md) | Überwacht, ob die transparente Datenverschlüsselung von SQL-Datenbanken nicht aktiviert ist.  |