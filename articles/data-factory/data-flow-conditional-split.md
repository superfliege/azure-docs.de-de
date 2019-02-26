---
title: 'Azure Data Factory Mapping Data Flow: Transformation für bedingtes Teilen'
description: Azure Data Factory-Datenflusstransformation für bedingtes Teilen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 9fe650465160ab837d8c8c191887d0f952976682
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271178"
---
# <a name="azure-data-factory-mapping-data-flow-conditional-split-transformation"></a>Azure Data Factory Mapping Data Flow: Transformation für bedingtes Teilen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Die Transformation für bedingtes Teilen kann Datenzeilen je nach Inhalt der Daten an verschiedene Datenströme routen. Die Implementierung der Transformation für bedingtes Teilen ist mit einer CASE-Entscheidungsstruktur in einer Programmiersprache vergleichbar. Die Transformation wertet Ausdrücke aus und leitet dann die Datenzeile basierend auf den Ergebnissen an den angegebenen Datenstrom weiter. Diese Transformation stellt zudem eine Standardausgabe bereit, damit eine Zeile, die mit keinem Ausdruck übereinstimmt, an die Standardausgabe weitergeleitet wird.

![Bedingtes Teilen](media/data-flow/cd1.png "Bedingtes Teilen")

Wenn Sie weitere Bedingungen hinzufügen möchten, wählen Sie im unteren Konfigurationsbereich die Option „Datenstrom hinzufügen“ aus, und klicken Sie in das Ausdrucks-Generator-Textfeld, um Ihren Ausdruck zu erstellen.
