---
title: 'Azure Data Factory Mapping Data Flow: Suchtransformation'
description: 'Azure Data Factory Mapping Data Flow: Suchtransformation'
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 28c8f2b0005641934f7fcd9549f1cf004b206d80
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271161"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure Data Factory Mapping Data Flow: Suchtransformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verwenden Sie die Transformation für die Suche, um Ihrem Datenfluss Verweisdaten aus einer anderen Quelle hinzuzufügen. Für die Suchtransformation ist eine definierte Quelle erforderlich, die auf Ihre Verweistabelle verweist und mit Schlüsselfeldern übereinstimmt.

![Suchtransformation](media/data-flow/lookup1.png "Suche")

Wählen Sie die Schlüsselfelder aus, die zwischen den Feldern für den eingehenden Datenstrom und den Feldern der Referenzquelle abgeglichen werden sollen. Zuerst muss auf dem Datenfluss-Entwurfscanvas eine neue Quelle erstellt worden sein, die als rechte Seite für die Suche verwendet wird.

Werden Übereinstimmungen gefunden, werden Ihrem Datenfluss die resultierenden Zeilen und Spalten der Referenzquelle hinzugefügt. Sie können die relevanten Felder auswählen, die Sie in Ihre Senke am Ende des Datenflusses einschließen möchten.
