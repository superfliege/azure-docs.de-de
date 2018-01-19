---
title: "Geplante Ereignisse für Linux-VMs in Azure | Microsoft-Dokumentation"
description: "Planen Sie Ereignisse mit dem Azure-Metadatendienst für Ihre virtuellen Linux-Computer."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: ae9955253647f3277729e7905baf7bb07645de42
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2018
---
# <a name="azure-metadata-service-scheduled-events-preview-for-linux-vms"></a>Azure-Metadatendienst: Geplante Ereignisse (Vorschau) für Linux-VMs

> [!NOTE] 
> Die Vorschauen werden Ihnen zur Verfügung gestellt, wenn Sie die folgenden Nutzungsbedingungen akzeptieren. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Das Feature für geplante Ereignisse ist ein untergeordneter Dienst des Azure-Metadatendiensts, der Ihren Anwendungen Zeit zur Vorbereitung auf die Wartung virtueller Computer gibt. Er stellt Informationen zu geplanten Wartungsereignissen (z.B. Neustart) bereit, damit Ihre Anwendung sich darauf vorbereiten und Unterbrechungen begrenzen kann. Der Dienst steht für alle Azure-VM-Typen zur Verfügung, einschließlich PaaS und IaaS unter Windows und Linux. 

Informationen zu geplanten Ereignissen unter Windows finden Sie unter [Geplante Ereignisse für virtuelle Windows-Computer](../windows/scheduled-events.md).

## <a name="why-use-scheduled-events"></a>Was spricht für die Verwendung geplanter Ereignisse?

Viele Anwendungen können von der Vorbereitungszeit auf die VM-Wartung profitieren. Die Zeit kann genutzt werden, um anwendungsspezifische Aufgaben durchzuführen, die die Verfügbarkeit, Zuverlässigkeit und Wartungsfreundlichkeit verbessern. Dazu gehören u.a.: 

- Prüfpunkt und Wiederherstellung
- Verbindungsausgleich
- Failover des primären Replikats
- Entfernung aus einem Lastenausgleichspool
- Ereignisprotokollierung
- Ordnungsgemäßes Herunterfahren

Mit dem Feature für geplante Ereignisse kann Ihre Anwendung erkennen, wann eine Wartung erfolgt, und so Aufgaben auslösen, um die Auswirkungen abzumildern.  

Geplante Ereignisse umfasst Ereignisse in den folgenden Anwendungsfällen:

- Von der Plattform ausgelöste Wartungsaktionen (z.B. Update des Hostbetriebssystems)
- Benutzerinitiierte Wartung (z.B. Neustart oder erneute Bereitstellung eines virtuellen Computers durch den Benutzer)

## <a name="the-basics"></a>Grundlagen  

  Der Metadatendienst macht Informationen zu ausgeführten virtuellen Computern mithilfe eines innerhalb einer VM zugänglichen REST-Endpunkts verfügbar. Die Informationen stehen über eine nicht routingfähige IP-Adresse bereit, die außerhalb der VM nicht verfügbar gemacht wird.

### <a name="scope"></a>Umfang
Geplante Ereignisse werden übermittelt an:

- Alle VMs in einem Clouddienst
- Alle VMs in einer Verfügbarkeitsgruppe
- Alle VMs in einer Gruppe für die Skalierungsgruppenplatzierung 

Überprüfen Sie deshalb anhand des Felds `Resources` in einem Ereignis, welche VMs betroffen sein werden.

### <a name="discover-the-endpoint"></a>Ermitteln des Endpunkts
Für VMs, die für virtuelle Netzwerke aktiviert sind, lautet der vollständige Name des Endpunkts für die neueste Version der geplanten Ereignisse folgendermaßen: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01`

In Fällen, in denen ein virtueller Computer innerhalb eines virtuellen Netzwerks erstellt wird, ist der Metadatendienst über eine statische nicht routingfähige IP-Adresse verfügbar: `169.254.169.254`.
Wenn der virtuelle Computer nicht innerhalb eines virtuellen Netzwerks erstellt wird – Standard für Clouddienste und klassische virtuelle Computer –, ist zusätzliche Logik erforderlich, um die zu verwendende IP-Adresse zu ermitteln. In diesem Beispiel erfahren Sie, wie Sie [den Hostendpunkt ermitteln](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="versioning"></a>Versionsverwaltung 
Das Feature für geplante Ereignisse ist versionsspezifisch. Die Versionen sind obligatorisch, und die aktuelle Version ist `2017-08-01`.

| Version | Versionshinweise | 
| - | - | 
| 2017-08-01 | <li> Ein vorangestellter Unterstrich wurde aus Ressourcennamen virtueller Iaas-Computer entfernt.<br><li>Der Metadatenheader wird als Voraussetzung für alle Anforderungen erzwungen. | 
| 2017-03-01 | <li>Öffentliche Vorschauversion


> [!NOTE] 
> In früheren Vorschauversionen von geplanten Ereignissen wird {latest} als „api-version“ unterstützt. Dieses Format wird nicht mehr unterstützt und wird zukünftig veraltet sein.

### <a name="use-headers"></a>Verwenden von Headern
Beim Abfragen des Metadatendiensts müssen Sie den Header `Metadata:true` angeben, um sicherzustellen, dass die Anforderung nicht unbeabsichtigt umgeleitet wurde. Der Header `Metadata:true` ist für alle Anforderungen für Geplante Ereignisse erforderlich. Wird der Header nicht in die Anforderung eingefügt, erhalten Sie vom Metadatendienst die Antwort „Ungültige Anforderung“.

### <a name="enable-scheduled-events"></a>Aktivieren geplanter Ereignisse
Wenn Sie das Feature für geplante Ereignisse erstmals aufrufen, wird dieses Feature von Azure implizit auf Ihrer VM aktiviert. Daher ist beim ersten Aufruf eine um bis zu zwei Minuten verzögerte Antwort zu erwarten.

> [!NOTE]
> Geplante Ereignisse werden für Ihren Dienst automatisch deaktiviert, wenn dieser den Endpunkt einen Tag lang nicht aufruft. Wenn das Feature für geplante Ereignisse für Ihren Dienst deaktiviert wird, werden keine Ereignisse für die vom Benutzer initiierten Wartungen erstellt.

### <a name="user-initiated-maintenance"></a>Vom Benutzer ausgelöste Wartung
Eine vom Benutzer über das Azure-Portal, die API, die CLI oder PowerShell initiierte Wartung eines virtuellen Computers führt zu einem geplanten Ereignis. Sie können anschließend die Logik zur Vorbereitung auf Wartungsmaßnahmen in Ihrer Anwendung testen und Ihre Anwendung auf die vom Benutzer initiierte Wartung vorbereiten.

Wenn Sie eine VM neu starten, wird ein Ereignis vom Typ `Reboot` geplant. Wenn Sie eine VM neu bereitstellen, wird ein Ereignis vom Typ `Redeploy` geplant.

> [!NOTE] 
> Aktuell können maximal 100 vom Benutzer initiierte Wartungsvorgänge parallel geplant werden.

> [!NOTE] 
> Die vom Benutzer initiierte Wartung, die zu geplanten Ereignissen führt, kann zurzeit nicht konfiguriert werden. Die Konfigurierbarkeit ist für ein zukünftiges Release geplant.

## <a name="use-the-api"></a>Verwenden der API

### <a name="query-for-events"></a>Abfragen von Ereignissen
Sie können geplante Ereignisse abfragen, indem Sie den folgenden Aufruf ausführen:

#### <a name="bash"></a>Bash
```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

Eine Antwort enthält ein Array geplanter Ereignisse. Ein leeres Array bedeutet, dass derzeit keine Ereignisse geplant sind.
Sofern geplante Ereignisse vorliegen, enthält die Antwort ein Array mit Ereignissen. 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a>Ereigniseigenschaften
|Eigenschaft  |  BESCHREIBUNG |
| - | - |
| EventId | Global eindeutiger Bezeichner für dieses Ereignis <br><br> Beispiel: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| EventType | Auswirkungen dieses Ereignisses <br><br> Werte: <br><ul><li> `Freeze`: Es ist geplant, dass die VM mehrere Sekunden lang angehalten wird. Die CPU wird angehalten, dies hat jedoch keine Auswirkungen auf Arbeitsspeicher, geöffnete Dateien oder Netzwerkverbindungen. <li>`Reboot`: Die VM ist für einen Neustart geplant. (Nicht persistierte Arbeitsspeicherdaten gehen verloren.) <li>`Redeploy`: Die VM ist für eine Verschiebung auf einen anderen Knoten geplant. (Kurzlebige Datenträger gehen verloren.) |
| ResourceType | Typ der Ressource, auf die sich dieses Ereignis auswirkt. <br><br> Werte: <ul><li>`VirtualMachine`|
| Resources| Liste der Ressourcen, auf die sich dieses Ereignis auswirkt. Die Liste enthält garantiert Computer aus maximal einer [Updatedomäne](manage-availability.md), muss jedoch nicht alle Computer in dieser Domäne enthalten. <br><br> Beispiel: <br><ul><li> [„FrontEnd_IN_0“, „BackEnd_IN_0“] |
| EventStatus | Status dieses Ereignisses <br><br> Werte: <ul><li>`Scheduled`: Dieses Ereignis erfolgt nach dem in der `NotBefore`-Eigenschaft angegebenen Zeitpunkt.<li>`Started`: Dieses Ereignis wurde gestartet.</ul> `Completed` oder ein ähnlicher Status wird nie angegeben. Das Ergebnis wird nicht länger zurückgegeben, wenn es abgeschlossen wurde.
| NotBefore| Zeitpunkt, nach dem dieses Ereignis gestartet werden kann. <br><br> Beispiel: <br><ul><li> 2016-09-19T18:29:47Z  |

### <a name="event-scheduling"></a>Ereignisplanung
Jedes Ereignis wird, basierend auf dem Ereignistyp, mit einer minimalen Vorlaufzeit geplant. Diese Zeit ist in der `NotBefore`-Eigenschaft eines Ereignisses angegeben. 

|EventType  | Mindestzeitspanne |
| - | - |
| Freeze| 15 Minuten |
| Reboot | 15 Minuten |
| Erneute Bereitstellung | 10 Minuten |

### <a name="start-an-event"></a>Starten eines Ereignisses 

Nachdem Sie von einem anstehenden Ereignis erfahren und Ihre Logik für ein ordnungsgemäßes Herunterfahren vervollständigt haben, können Sie das ausstehende Ereignis genehmigen, indem Sie einen Aufruf des Typs `POST` an den Metadatendienst mit der `EventId` ausführen. Dieser Aufruf informiert Azure darüber, dass die minimale Benachrichtigungszeit verkürzt werden kann (falls möglich). 

Das folgende JSON-Beispiel wird im `POST`-Anforderungstext erwartet. Die Anforderung sollte eine Liste mit `StartRequests` enthalten. Jede `StartRequest` enthält die `EventId` für das Ereignis, das Sie beschleunigen möchten:
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

#### <a name="bash-sample"></a>Bash-Beispiel
```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

> [!NOTE] 
> Durch das Bestätigen eines Ereignisses kann dieses für alle `Resources` im Ereignis fortgesetzt werden, nicht nur für die VM, die das Ereignis bestätigt. Daher können Sie einen Leiter zur Koordinierung der Bestätigung auswählen, beispielsweise einfach den ersten Computer im Feld `Resources`.

## <a name="python-sample"></a>Python-Beispiel 

Im folgenden Beispiel fragt der Metadatenserver geplante Ereignisse ab und genehmigt jedes ausstehende Ereignis:

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host " + this_host + " is scheduled for " + eventtype + " not before " + notbefore
            # Add logic for handling events here


def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)

if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die Codebeispiele für geplante Ereignisse im [GitHub-Repository mit Azure-Instanzmetadaten für geplante Ereignisse](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm).
- Erfahren Sie mehr über die verfügbaren APIs im [Instanz-Metadatendienst](instance-metadata-service.md).
- Erfahren Sie mehr über [Geplante Wartung für virtuelle Linux-Computer in Azure](planned-maintenance.md).
