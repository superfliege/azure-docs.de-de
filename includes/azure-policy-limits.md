---
title: Includedatei
description: Includedatei
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/16/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 0e5d1214a8e1af8299cb40d1a3b31ff6eafc8c5c
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "40184893"
---
Für jeden Objekttyp für Azure Policy gibt es eine maximale Anzahl. Ein _Scope_-Eintrag (Bereich) gilt entweder für das Abonnement oder die [Verwaltungsgruppe](../articles/azure-resource-manager/management-groups-overview.md).

| Hierbei gilt: | Was | Maximale Anzahl |
|---|---|---|
| Bereich | Richtliniendefinitionen | 250 |
| Bereich | Initiativdefinitionen | 100 |
| Tenant | Initiativdefinitionen | 1000 |
| Bereich | Richtlinie/Initiativenzuweisungen | 100 |
| Richtliniendefinition | Parameter | 20 |
| Initiativdefinition | Richtlinien | 100 |
| Initiativdefinition | Parameter | 100 |
| Richtlinie/Initiativenzuweisungen | Ausschlüsse (notScopes) | 100 |
| Richtlinienregel | Konditionelle Abschnitte | 512 |
