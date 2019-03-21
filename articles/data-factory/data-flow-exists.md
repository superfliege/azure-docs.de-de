---
title: 'Azure Data Factory Mapping Data Flow: Exists-Transformation'
description: 'Azure Data Factory Mapping Data Flow: Exists-Transformation'
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 18d8a0e231e8b4dbe33911dd6267966674366904
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56734488"
---
# <a name="azure-data-factory-mapping-data-flow-exists-transformation"></a>Azure Data Factory Mapping Data Flow: Exists-Transformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Die Exists-Transformation ist eine Zeilenfilterungstransformation, die das Durchlaufen von Zeilen in Ihren Daten beendet oder zulässt. Die Exists-Transform ist mit ```SQL WHERE EXISTS``` und ```SQL WHERE NOT EXISTS``` vergleichbar. Nach einer Filtertransformation enthalten die resultierenden Zeilen aus Ihrem Datenstrom entweder alle Zeilen, in denen die Spaltenwerte aus Quelle 1 in Quelle 2 vorhanden oder in Quelle 2 nicht vorhanden sind.

![Exists-Einstellungen](media/data-flow/exsits.png "ist vorhanden 1")

Wählen Sie die zweite Quelle für Exists aus, damit Datenfluss die Werte aus Datenstrom 1 mit Datenstrom 2 vergleichen kann.

Wählen Sie die Spalte aus Quelle 1 und aus Quelle 2 aus, deren Werte Sie auf das Vorhandensein oder Nichtvorhandensein überprüfen möchten.
