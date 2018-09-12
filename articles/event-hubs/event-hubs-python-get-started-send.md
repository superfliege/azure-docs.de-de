---
title: Senden von Ereignissen an Azure Event Hubs mithilfe von Python | Microsoft-Dokumentation
description: Erste Schritte beim Senden von Ereignissen an Event Hubs mithilfe von Python
services: event-hubs
author: sethmanheim
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/26/2018
ms.author: sethm
ms.openlocfilehash: 762e21cfc7d16b614eb637c569f8bfc5b6115db1
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43703406"
---
# <a name="send-events-to-event-hubs-using-python"></a>Senden von Ereignissen an Event Hubs mithilfe von Python

Azure Event Hubs ist ein hochgradig skalierbares Ereignisverwaltungssystem, das Millionen von Ereignissen pro Sekunde verarbeiten kann. Dadurch können Anwendungen die von verbundenen Geräten und anderen Systemen erzeugten immensen Datenmengen verarbeiten und analysieren. Sobald Ereignisse in einem Event Hub gesammelt wurden, können Sie sie über In-Process-Handler oder durch Weiterleiten an andere Analysesysteme empfangen und verarbeiten.

Weitere Informationen zu Event Hubs finden Sie unter [Übersicht über Event Hubs][Event Hubs overview].

Dieses Tutorial beschreibt, wie Sie Ereignisse aus einer in Python geschriebenen Anwendung an einen Event Hub senden. Weitere Informationen zum Empfangen von Ereignissen finden Sie im [entsprechenden Artikel](event-hubs-python-get-started-receive.md).

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

## <a name="create-a-python-script-to-send-events"></a>Erstellen eines Python-Skripts zum Senden von Ereignissen

Erstellen Sie als Nächstes eine Python-Anwendung, die Ereignisse an einen Event Hub sendet:

1. Öffnen Sie Ihren bevorzugten Python-Editor, z.B. [Visual Studio Code][Visual Studio Code].
2. Erstellen Sie ein Skript mit dem Namen **send.py**. Dieses Skript sendet 100 Ereignisse an Ihren Event Hub.
3. Fügen Sie den folgenden Code in „send.py“ ein, und ersetzen Sie dabei die Werte von „ADDRESS“, „USER“ und „KEY“ durch die Werte, die Sie im vorherigen Abschnitt im Azure-Portal abgerufen haben: 

```python
import sys
import logging
import datetime
import time
import os

from azure.eventhub import EventHubClient, Sender, EventData

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"

try:
    if not ADDRESS:
        raise ValueError("No EventHubs URL supplied.")

    # Create Event Hubs client
    client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
    sender = client.add_sender(partition="0")
    client.run()
    try:
        start_time = time.time()
        for i in range(100):
            print("Sending message: {}".format(i))
            sender.send(EventData(str(i)))
    except:
        raise
    finally:
        end_time = time.time()
        client.stop()
        run_time = end_time - start_time
        logger.info("Runtime: {} seconds".format(run_time))

except KeyboardInterrupt:
    pass
```

## <a name="send-events"></a>Senden von Ereignisse

Um das Skript auszuführen, öffnen Sie eine Eingabeaufforderung, deren Pfad Python enthält, und führen Sie dann diesen Befehl aus:

```bash
start python send.py
```
 
## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun Ereignisse an einen Event Hub mithilfe von Python gesendet haben, informieren Sie sich im [entsprechenden Artikel](event-hubs-python-get-started-receive.md) über das Empfangen von Ereignissen.

Weitere Informationen zu Event Hubs finden Sie auf den folgenden Seiten:

* [Übersicht über Event Hubs][Event Hubs overview]
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
