---
title: Includedatei
description: Includedatei
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 12/20/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 40ab53c941a7ac619ebb09d381a4ae0450f26e8b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66162111"
---
`objectIdType` (oder **Objektbezeichnertyp**) bezieht sich auf den Typ der Identität, der einer Rolle zugewiesen wird. Mit Ausnahme der Typen `DeviceId` und `UserDefinedFunctionId` entsprechen Objektbezeichnertypen Eigenschaften von Azure Active Directory-Objekten.

Die folgende Tabelle enthält die unterstützten Objektbezeichnertypen in Azure Digital Twins:

| Type | BESCHREIBUNG |
| --- | --- |
| UserId | Weist einem Benutzer eine Rolle zu. |
| deviceId | Weist einem Gerät eine Rolle zu. |
| DomainName | Weist einem Domänennamen eine Rolle zu. Jeder Benutzer mit dem angegebenen Domänennamen hat die Zugriffsrechte der entsprechenden Rolle. |
| TenantId | Weist einem Mandanten eine Rolle zu. Jeder Benutzer, der zu der angegebenen Azure AD-Mandanten-ID gehört, hat die Zugriffsrechte der entsprechenden Rolle. |
| ServicePrincipalId | Weist einer Dienstprinzipalobjekt-ID eine Rolle zu. |
| UserDefinedFunctionId | Weist einer benutzerdefinierten Funktion (User-defined Function, UDF) eine Rolle zu. |