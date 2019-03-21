---
title: 'Azure Data Factory Mapping Data Flow: Suchtransformation'
description: 'Azure Data Factory Mapping Data Flow: Suchtransformation'
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: b156917a9987b023a9bf94e51c0cc14aebb133c7
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56738384"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure Data Factory Mapping Data Flow: Suchtransformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verwenden Sie die Transformation für die Suche, um Ihrem Datenfluss Verweisdaten aus einer anderen Quelle hinzuzufügen. Für die Suchtransformation ist eine definierte Quelle erforderlich, die auf Ihre Verweistabelle verweist und mit Schlüsselfeldern übereinstimmt.

![Suchtransformation](media/data-flow/lookup1.png "Suche")

Wählen Sie die Schlüsselfelder aus, die zwischen den Feldern für den eingehenden Datenstrom und den Feldern der Referenzquelle abgeglichen werden sollen. Zuerst muss auf dem Datenfluss-Entwurfscanvas eine neue Quelle erstellt worden sein, die als rechte Seite für die Suche verwendet wird.

Werden Übereinstimmungen gefunden, werden Ihrem Datenfluss die resultierenden Zeilen und Spalten der Referenzquelle hinzugefügt. Sie können die relevanten Felder auswählen, die Sie in Ihre Senke am Ende des Datenflusses einschließen möchten.
