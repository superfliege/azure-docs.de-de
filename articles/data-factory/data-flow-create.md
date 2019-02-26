---
title: Erstellen eines Azure Data Factory-Zuordnungsdatenflusses
description: Erstellen eines Azure Data Factory-Zuordnungsdatenflusses
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: b706e229bed48c821d5ca772450df320fd7e0b7f
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271155"
---
# <a name="create-azure-data-factory-data-flow"></a>Erstellen eines Azure Data Factory-Datenflusses

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Mit Mapping Data Flow in ADF können Sie Daten nach Maß transformieren, ohne Code schreiben zu müssen. Sie können einen Datentransformationsauftrag im Datenfluss-Designer entwerfen, indem Sie eine Reihe von Transformationen erstellen. Beginnen Sie mit einer beliebigen Anzahl von Quelltransformationen, gefolgt von Datentransformationsschritten. Vervollständigen Sie dann Ihren Datenfluss mit einer Senke, damit Ihre Ergebnisse an ein Ziel gelangen.

Erstellen Sie zuerst im Azure-Portal eine neue Data Factory (V2). Klicken Sie nach dem Erstellen der neuen Factory auf die Kachel „Erstellen und überwachen“, um die Data Factory-Benutzeroberfläche zu starten.

![Datenflussoptionen](media/data-flow/v2dataflowportal.png "Datenfluss erstellen")

Sobald Sie sich auf der Data Factory-Benutzeroberfläche befinden, können Sie die Beispieldatenflüsse verwenden. Die Beispiele sind im ADF-Vorlagenkatalog verfügbar. Erstellen Sie in ADF eine „Pipeline aus Vorlage“, und wählen Sie im Vorlagenkatalog die Kategorie „Datenfluss“ aus.

![Datenflussoptionen](media/data-flow/template.png "Datenfluss erstellen")

Sie werden zur Eingabe Ihrer Azure Blob Storage-Kontoinformationen aufgefordert.

![Datenflussoptionen](media/data-flow/template2.png "Datenfluss erstellen 2")

[Die für diese Beispiele verwendeten Daten finden Sie hier](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). Laden Sie die Beispieldaten herunter, und speichern Sie die Dateien in Ihren Azure Blob Storage-Konten, damit Sie die Beispiele ausführen können.

Verwenden Sie auf der ADF-Benutzeroberfläche die Schaltfläche „Ressource erstellen“ mit dem Pluszeichen (+), um Datenflüsse zu erstellen.

![Datenflussoptionen](media/data-flow/newresource.png "Neue Ressource")

