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
ms.openlocfilehash: 50a6388a95bce14bc9af7e0c9a5387e148f6fa73
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664608"
---
### <a name="sql-databases"></a>SQL-DATENBANKEN

|  |  |
|---------|---------|
| [Zulässige SQL-Datenbank-SKUs](../articles/azure-policy/scripts/allowed-sql-db-skus.md) | Erfordert, dass SQL-Datenbanken eine genehmigte SKU verwenden. Sie geben ein Array von zulässigen SKU-IDs oder von zulässigen SKU-Namen an. |
| [Überwachen der Einstellung für die Bedrohungserkennung auf Datenbankebene](../articles/azure-policy/scripts/audit-db-threat-det-setting.md) | Überwacht, ob die Richtlinien der Sicherheitswarnungen für SQL-Datenbanken nicht auf den angegebenen Status festgelegt sind. Sie geben einen Wert an, der angibt, ob die Bedrohungserkennung aktiviert oder deaktiviert ist.  |
| [Überwachen der SQL-Datenbankverschlüsselung](../articles/azure-policy/scripts/sql-database-encryption-audit.md) | Überwacht, ob die transparente Datenverschlüsselung für die SQL-Datenbank deaktiviert ist. |
| [Überwachen von Einstellungen der Überwachung auf SQL-Datenbank-Ebene](../articles/azure-policy/scripts/audit-sql-db-audit-setting.md) | Überwacht, ob die Einstellungen der SQL Datenbanküberwachung nicht mit einer bestimmten Einstellung übereinstimmen Sie geben einen Wert an, der angibt, ob Überwachungseinstellungen aktiviert oder deaktiviert sein sollen.  |
| [Überwachen des Status der transparenten Datenverschlüsselung](../articles/azure-policy/scripts/audit-trans-data-enc-status.md) | Überwacht, ob die transparente Datenverschlüsselung von SQL-Datenbanken nicht aktiviert ist.  |