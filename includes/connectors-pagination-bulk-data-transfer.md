---
title: Includedatei
description: Includedatei
services: logic-apps
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 05/09/2018
ms.author: estfan
ms.custom: include file
ms.openlocfilehash: 524bc1d3a19ad8ecfc8d0d210e735d6a9ae0058b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34678275"
---
### <a name="set-up-pagination"></a>Einrichten der Paginierung

Für einige Connectors und ihre Aktionen, die mehrere Elemente abrufen, können die Ergebnisse die Standardseitengröße für den Connector überschreiten. In diesem Fall gibt die Aktion nur die erste Seite mit Ergebnissen zurück. Die Standardseitengröße für die Aktion **SQL Server – -Zeilen abrufen** ist z.B. 2.048, dieser Wert kann aber basierend auf anderen Einstellungen abweichen. Um sicherzustellen, dass Sie alle Datensätze erhalten, aktivieren Sie die Einstellung **Paginierung** für diese Aktion. Mit dieser Einstellung fragt Ihre Logik-App vom Connector die verbleibenden Einträge ab. Sämtliche Ergebnisse werden als eine einzelne Nachricht zurückgeben, wenn die Aktion abgeschlossen ist. 

Im Folgenden finden Sie einige der Connectors, bei denen Sie die Paginierung für bestimmte Aktionen aktivieren können: 

* <a href="https://docs.microsoft.com/connectors/db2/" target="_blank">DB2</a>
* <a href="https://docs.microsoft.com/connectors/dynamicscrmonline/" target="_blank">Dynamics 365 CRM Online</a>
* <a href="https://docs.microsoft.com/connectors/excel/" target="_blank">Excel</a>
* <a href="https://docs.microsoft.com/connectors/oracle/" target="_blank">Oracle-Datenbank</a>
* <a href="https://docs.microsoft.com/connectors/sharepointonline/" target="_blank">SharePoint</a>
* <a href="https://docs.microsoft.com/connectors/sql/" target="_blank">SQL Server</a> 

Dies ist ein Beispiel für die Aktion **Zeilen abrufen**:

1. Um zu erfahren, ob eine Aktion die Paginierung unterstützt, öffnen Sie die **Einstellungen** der Aktion. 

   ![Öffnen von „Einstellungen“ für die Aktion](./media/connectors-pagination-bulk-data-transfer/sql-action-settings.png)

2. Wenn die Aktion die Paginierung unterstützt, ändern Sie die Einstellung **Paginierung** von **Aus** in **Ein**. Um sicherzustellen, dass die Aktion einen Mindestsatz von Ergebnissen zurückgibt, geben Sie einen Wert für **Grenzwert** an.

   ![Festlegen, dass die Aktion eine Mindestanzahl von Ergebnissen zurückgibt](./media/connectors-pagination-bulk-data-transfer/sql-action-settings-pagination.png)

3. Wählen Sie abschließend **Fertig** aus.