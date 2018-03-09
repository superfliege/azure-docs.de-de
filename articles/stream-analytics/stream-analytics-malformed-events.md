---
title: "Problembehandlung für falsch formatierte Eingabeereignisse in Azure Stream Analytics| Microsoft-Dokumentation"
description: Wie ermittle ich, welches Ereignis in meinen Eingabedaten ein Problem in einem Stream Analytics-Auftrag verursacht?
keywords: 
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: Kfile
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/19/2018
ms.author: sngun
ms.openlocfilehash: 2b4f82bae20c3cb398848a89715bfdc1316e1ba1
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
---
# <a name="troubleshoot-for-malformed-input-events"></a>Problembehandlung für falsch formatierte Eingabeereignisse

Wenn der Eingabedatenstrom Ihres Stream Analytics-Auftrags falsch formatierte Nachrichten enthält, werden Serialisierungsprobleme verursacht. Zu falsch formatierten Nachrichten zählen unter anderem Nachrichten mit fehlerhafter Serialisierung (also etwa mit fehlenden Klammern in einem JSON-Objekt oder mit einem falschen Zeitstempelformat). Wenn ein Stream Analytics-Auftrag eine falsch formatierte Nachricht empfängt, wird diese gelöscht und der Benutzer mit einer Warnung benachrichtigt. Auf der Kachel **Eingaben** des Stream Analytics-Auftrags wird ein Warnsymbol angezeigt:

![Kachel „Eingaben“](media/stream-analytics-malformed-events/inputs_tile.png)

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

1. Navigieren Sie zur Eingabekachel, und klicken Sie darauf, um Warnungen anzuzeigen.
2. Auf der Kachel „Eingabedetails“ werden Warnungen mit Details zum Problem angezeigt. Nachfolgend sehen Sie ein Beispiel für eine Warnmeldung. Diese Warnmeldung enthält die Partition, den Offset und die Sequenznummern mit falsch formatierten JSON-Daten. 

   ![Warnmeldung mit Offset](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Führen Sie zum Abrufen der JSON-Daten mit falschem Format den folgenden Codeabschnitt aus. Dieser Codeblock liest die Partitions-ID und den Offset, und gibt die Daten aus. Das vollständige Beispiel finden Sie im [GitHub-Beispielrepository](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Nach dem Lesen der Daten können Sie das Serialisierungsformat analysieren und korrigieren.

```csharp
static void PrintMessages(string partitionId, long offset, int numberOfEvents)
        {
            EventHubReceiver receiver;
            
            try
            {
                foreach (var e in receiver.Receive(numberOfEvents, TimeSpan.FromMinutes(1)))
                {
                    Console.WriteLine(Encoding.UTF8.GetString(e.GetBytes()));
                    Console.WriteLine("----");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.ToString());
                return;
            }

            receiver.Close();
        }
```
## <a name="next-steps"></a>Nächste Schritte

* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
