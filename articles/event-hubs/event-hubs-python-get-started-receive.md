---
title: Empfangen von Ereignissen von Azure Event Hubs mithilfe von Python | Microsoft-Dokumentation
description: Erste Schritte beim Empfangen von Ereignissen von Event Hubs mithilfe von Python
services: event-hubs
author: sethmanheim
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/26/2018
ms.author: sethm
ms.openlocfilehash: 9707f0cf5a76c0ac4006b53ca8166d3b42fc9af4
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240561"
---
# <a name="receive-events-from-event-hubs-using-python"></a>Empfangen von Ereignissen von Event Hubs mithilfe von Python

Azure Event Hubs ist ein hochgradig skalierbares Ereignisverwaltungssystem, das Millionen von Ereignissen pro Sekunde verarbeiten kann. Dadurch können Anwendungen die von verbundenen Geräten und anderen Systemen erzeugten immensen Datenmengen verarbeiten und analysieren. Sobald Ereignisse in einem Event Hub gesammelt wurden, können Sie sie über In-Process-Handler oder durch Weiterleiten an andere Analysesysteme empfangen und verarbeiten.

Weitere Informationen zu Event Hubs finden Sie unter [Übersicht über Event Hubs][Event Hubs overview].

Dieses Tutorial beschreibt, wie Sie Ereignisse aus einem Event Hub in einer in Python geschriebenen Anwendung empfangen. Informationen zum Senden von Ereignissen finden Sie im [entsprechenden Artikel](event-hubs-python-get-started-send.md).

Der Code in diesem Tutorial stammt aus [ diesen GitHub-Beispielen](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), die weitere Informationen zur voll funktionsfähigen Anwendung einschließlich Importanweisungen und Variablendeklarationen enthalten. Weitere Beispiele sind im selben GitHub-Ordner verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Python 3.4 oder höher.
- Einen vorhandenen Event Hubs-Namespace und einen vorhandenen Event Hub. Die Anweisungen [in diesem Artikel](event-hubs-create.md) zeigen Ihnen, wie Sie diese Entitäten erstellen. 

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]


## <a name="install-python-package"></a>Installieren des Python-Pakets

Um die Python-Pakete für Event Hubs zu installieren, öffnen Sie eine Eingabeaufforderung, deren Pfad Python enthält, und führen Sie dann diesen Befehl aus: 

```bash
pip install azure-eventhub
```

## <a name="create-a-python-script-to-receive-events"></a>Erstellen eines Python-Skripts zum Empfangen von Ereignissen

Erstellen Sie als Nächstes eine Python-Anwendung, die Ereignisse von einem Event Hub empfängt:

1. Öffnen Sie Ihren bevorzugten Python-Editor, z.B. [Visual Studio Code][Visual Studio Code].
2. Erstellen Sie ein Skript mit dem Namen **recv.py**.
3. Fügen Sie den folgenden Code in „recv.py“ ein, und ersetzen Sie dabei die Werte von „ADDRESS“, „USER“ und „KEY“ durch die Werte, die Sie im vorherigen Abschnitt im Azure-Portal abgerufen haben: 

```python
import os
import sys
import logging
import time
from azure.eventhub import EventHubClient, Receiver, Offset

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"
CONSUMER_GROUP = "$default"
OFFSET = Offset("-1")
PARTITION = "0"

total = 0
last_sn = -1
last_offset = "-1"
client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
try:
    receiver = client.add_receiver(CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)
    client.run()
    start_time = time.time()
    for event_data in receiver.receive(timeout=100):
        last_offset = event_data.offset
        last_sn = event_data.sequence_number
        print("Received: {}, {}".format(last_offset, last_sn))
        total += 1

    end_time = time.time()
    client.stop()
    run_time = end_time - start_time
    print("Received {} messages in {} seconds".format(total, run_time))

except KeyboardInterrupt:
    pass
finally:
    client.stop()
```

## <a name="receive-events"></a>Empfangen von Ereignissen

Um das Skript auszuführen, öffnen Sie eine Eingabeaufforderung, deren Pfad Python enthält, und führen Sie dann diesen Befehl aus:

```bash
start python recv.py
```
 
## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie eine Python-Anwendung erstellt, die Nachrichten von einem Event Hub empfängt. Informationen dazu, wie Sie Ereignisse unter Verwendung von Python an einen Event Hub senden, finden Sie unter [Erste Schritte beim Senden von Nachrichten an Azure Event Hubs: Python](event-hubs-python-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
