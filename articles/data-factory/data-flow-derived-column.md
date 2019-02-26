---
title: Azure Data Factory-Datenflusstransformation für abgeleitete Spalten
description: Azure Data Factory-Datenflusstransformation für abgeleitete Spalten
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: aee005aed52563e214e24148af2563fd7869de76
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271184"
---
# <a name="azure-data-factory-data-flow-derived-column-transformation"></a>Azure Data Factory-Datenflusstransformation für abgeleitete Spalten

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verwenden Sie die Transformation für abgeleitete Spalten, um in Ihrem Datenfluss neue Spalten zu generieren oder vorhandene Felder zu ändern.

![Spalte ableiten](media/data-flow/dc1.png "Abgeleitete Spalte")

Sie können in einer einzigen Transformation für abgeleitete Spalten mehrere Aktionen für abgeleitete Spalten ausführen. Klicken Sie auf „Spalte hinzufügen“, um beim Schritt einer Einzeltransformation mehr als eine Spalte zu transformieren.

Wählen Sie im Feld „Spalte“ eine vorhandene Spalte aus, die mit einem neuen abgeleiteten Wert überschrieben werden soll, oder klicken Sie auf „Neue Spalte erstellen“, um eine neue Spalte mit dem neu abgeleiteten Wert zu erstellen.

Über das Textfeld „Ausdruck“ wird der Ausdrucks-Generator geöffnet, in dem Sie den Ausdruck für die abgeleiteten Spalten mithilfe der Ausdrucksfunktionen erstellen können.
