---
title: Regeln für die Benennung von Azure Data Factory-Entitäten | Microsoft-Dokumentation
description: Beschreibt die Benennungsregeln für Data Factory-Entitäten.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 0311c2e2223c60861d2b6c87bfa7747dc079856d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory – Benennungsregeln
> [!NOTE]
> Dieser Artikel bezieht sich auf Version 1 von Data Factory, die allgemein verfügbar (GA) ist. Wenn Sie Version 2 des Data Factory-Diensts verwenden, die sich derzeit in der Vorschauphase befindet, finden Sie weitere Informationen unter [naming rules in Data Factory version 2 (Benennungsregeln in Version 2 von Data Factory)](../naming-rules.md).

Die folgende Tabelle enthält Benennungsregeln für Data Factory-Artefakte.

| NAME | Eindeutigkeit des Namens | Überprüfungen |
|:--- |:--- |:--- |
| Data Factory |Für Microsoft Azure eindeutig. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt, sodass `MyDF` und `mydf` auf die gleiche Data Factory verweisen. |<ul><li>Jede Data Factory ist an genau ein Azure-Abonnement gebunden.</li><li>Objektnamen müssen mit einem Buchstaben oder einer Zahl beginnen und dürfen nur Buchstaben, Zahlen und Bindestriche (-) enthalten.</li><li>Jedem Bindestrich (-) muss unmittelbar ein Buchstabe oder eine Ziffer vorangestellt werden und er muss von diesen gefolgt sein. Aufeinanderfolgende Bindestriche sind in Containernamen nicht zulässig.</li><li>Namen können 3 bis 63 Zeichen lang sein.</li></ul> |
| Verknüpfte Dienste/Tabellen/Pipelines |Innerhalb einer Data Factory eindeutig. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt. |<ul><li>Maximale Anzahl von Zeichen in einem Tabellennamen: 260.</li><li>Objektnamen müssen mit einem Buchstaben, einer Zahl oder einem Unterstrich (_) beginnen.</li><li>Folgende Zeichen sind nicht zulässig: „.“, „+“, „?“, „/“, „<“, „>“, „*“, „%“, „&“, „:“, „\\“.</li></ul> |
| Ressourcengruppe |Für Microsoft Azure eindeutig. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt. |<ul><li>Maximale Anzahl von Zeichen: 1000.</li><li>Der Name kann Buchstaben, Zahlen und die folgenden Zeichen enthalten: „-“, „_“, „,“ und „.“.</li></ul> |

