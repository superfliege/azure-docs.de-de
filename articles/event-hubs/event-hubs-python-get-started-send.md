---
title: Senden von Ereignissen unter Verwendung von Python – Azure Event Hubs | Microsoft-Dokumentation
description: Dieser Artikel enthält eine exemplarische Vorgehensweise für die Erstellung einer Node.js-Anwendung, die Ereignisse an Azure Event Hubs sendet.
services: event-hubs
author: ShubhaVijayasarathy
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 11/16/2018
ms.author: shvija
ms.openlocfilehash: b7adf3976f5f7e028ffa9ffeb13db22d3d4bba8e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53102978"
---
# <a name="send-events-to-event-hubs-using-python"></a>Senden von Ereignissen an Event Hubs mithilfe von Python

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der pro Sekunde Millionen von Ereignissen empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Eine ausführliche Übersicht über Event Hubs finden Sie unter [Was ist Azure Event Hubs?](event-hubs-about.md) und [Event Hubs-Features im Überblick](event-hubs-features.md).

Dieses Tutorial beschreibt, wie Sie Ereignisse aus einer in Python geschriebenen Anwendung an einen Event Hub senden. 

> [!NOTE]
> Sie können diese Schnellstartvorlage als Beispiel aus [GitHub](https://github.com/Azure/azure-event-hubs-python/tree/master/examples) herunterladen, die Zeichenfolgen `EventHubConnectionString` und `EventHubName` durch die Werte für Ihren Event Hub ersetzen und das Beispiel dann ausführen. Alternativ können Sie auch die Schritte in diesem Tutorial ausführen, um eine eigene Vorlage zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.
- Python 3.4 oder höher.


## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs
Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Erstellen Sie anhand der Anleitung in [diesem Artikel](event-hubs-create.md) einen Namespace und einen Event Hub.

Gehen Sie wie im Artikel beschrieben vor, um den Wert des Zugriffsschlüssels für den Event Hub abzurufen: [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Sie verwenden den Zugriffsschlüssel im Code, den Sie später in diesem Tutorial schreiben. Der Standardschlüsselname lautet: **RootManageSharedAccessKey**.

Fahren Sie nun mit den folgenden Schritten in diesem Tutorial fort.

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

## <a name="run-application-to-send-events"></a>Ausführen der Anwendung zum Senden von Ereignissen

Um das Skript auszuführen, öffnen Sie eine Eingabeaufforderung, deren Pfad Python enthält, und führen Sie dann diesen Befehl aus:

```bash
start python send.py
```

Glückwunsch! Sie haben jetzt Nachrichten an einen Event Hub gesendet.
 
## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie Nachrichten mithilfe von Python an einen Event Hub gesendet. Informationen dazu, wie Sie Nachrichten unter Verwendung von Python von einem Event Hub empfangen, erhalten Sie unter [Empfangen von Ereignissen von Event Hubs mithilfe von Python](event-hubs-python-get-started-receive.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
