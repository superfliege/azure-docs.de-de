---
title: Löschen von Daten aus dem Azure-Daten-Explorer
description: Dieser Artikel beschreibt Szenarien zum Massenlöschen im Azure-Daten-Explorer, einschließlich endgültiger und beibehaltungsbasierter Löschvorgänge.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 571a005dd3f50690f291a7ffa3c1174ea15cb0ed
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046285"
---
# <a name="delete-data-from-azure-data-explorer"></a>Löschen von Daten aus dem Azure-Daten-Explorer

Der Azure-Daten-Explorer unterstützt mehrere Ansätze zum Massenlöschen, die in diesem Artikel behandelt werden. Das Löschen einzelner Datensätze in Echtzeit wird nicht unterstützt, weil er für schnellen Lesezugriff optimiert ist.

* Wenn die Datenbank nicht mehr erforderlich ist, löschen Sie sie mit dem Befehl „drop database“.

    ```Kusto
    .drop database <DatabaseName>
    ```

* Wenn eine oder mehrere Tabellen nicht mehr benötigt werden, löschen Sie sie über den Befehl „drop table“ bzw. „drop tables“.

    ```Kusto
    .drop table <TableName>

    .drop tables (<TableName1>, <TableName2>,...)
    ```

* Wenn alte Daten nicht mehr erforderlich sind, löschen Sie sie, indem Sie die Beibehaltungsdauer auf Datenbank- oder Tabellenebene ändern.

    Stellen Sie sich eine Datenbank oder Tabelle vor, für die eine Beibehaltungsdauer von 90 Tagen festgelegt ist. Das Unternehmen braucht Veränderung, daher werden nur noch Daten aus den letzten 60 Tage benötigt. In diesem Fall löschen Sie die älteren Daten anhand einer der folgenden Methoden.

    ```Kusto
    .alter-merge database <DatabaseName> policy retention softdelete = 60d

    .alter-merge table <TableName> policy retention softdelete = 60d
    ```

    Weitere Informationen finden Sie unter [Aufbewahrungsrichtlinie](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy).

* Einzelne Datensätze können über den Vorgang *purge* basierend auf einem Prädikat wie `where CustomerName == 'contoso'` gelöscht werden. Demnach entspricht ein endgültiger Löschvorgang mit „purge“ einer Massenlöschung, die nicht auf das Löschen in Echtzeit ausgerichtet ist. Im folgenden Beispiel wird ein endgültiger Löschvorgang mit „purge“ dargestellt.

    ```Kusto
    .purge table Customer records
    | where CustomerName =='contoso'
    ```

Wenn Sie Hilfe bei Datenlöschvorgängen benötigen, erstellen Sie eine Supportanfrage im [Azure-Portal](https://portal.azure.com).