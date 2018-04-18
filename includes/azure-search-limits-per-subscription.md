---
title: Includedatei
description: Includedatei
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 04/04/2018
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 8a4f794c8ef24a90498954629c131904621c5b43
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
Sie können mehrere Dienste in einem Abonnement erstellen, die jeweils für einen bestimmten Tarif bereitgestellt werden. Als einzige Einschränkung gilt hierbei die Anzahl von zulässigen Diensten des jeweiligen Tarifs. Beispielsweise können Sie in einem Abonnement bis zu 12 Dienste für den Basic-Tarif und weitere 12 Dienste für den S1-Tarif erstellen. Weitere Informationen zu Tarifen finden Sie unter [Auswählen einer SKU oder eines Tarifs für Azure Search](../articles/search/search-sku-tier.md).

Die Obergrenze für die maximale Anzahl von Diensten kann auf Anfrage angehoben werden. Wenden Sie sich an den Support von Azure, wenn Sie in einem Abonnement weitere Dienste benötigen.

| Ressource            | Free&nbsp;<sup>1</sup> | Basic | S1  | S2 | S3 | S3&nbsp;HD |
| ------------------- | ---- | ----- | --- | -- | -- | ----- |
| Maximale Anzahl von Diensten    |1     | 12    | 12  | 6  | 6  | 6     |
| Maximale Skalierung in SU&nbsp;<sup>2</sup> |N/V |3 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> „Free“ basiert auf gemeinsamen (nicht dedizierten) Ressourcen. Zentrales Hochskalieren wird für gemeinsame Ressourcen nicht unterstützt.

<sup>2</sup> Sucheinheiten (SU) sind Abrechnungseinheiten, die entweder als *Replikat* oder *Partition* zugeordnet werden. Sie benötigen beide Ressourcen für Speicher-, Indizierungs- und Abfragevorgänge. Weitere Informationen zu SU-Berechnungen finden Sie unter [Skalieren von Ressourcenebenen für Abfrage und Indizierung von Arbeitslasten in Azure Search](../articles/search/search-capacity-planning.md). 