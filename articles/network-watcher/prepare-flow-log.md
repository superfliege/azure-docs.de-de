---
title: Änderungen in den Flowprotokollen von Azure-Netzwerksicherheitsgruppen | Microsoft-Dokumentation
description: Erfahren Sie mehr über Änderungen in den Flowprotokollen von Azure-Netzwerksicherheitsgruppen.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2018
ms.author: jdial
ms.openlocfilehash: b3a5ca929c83f70c31d667c2d9c811623691cff8
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180563"
---
# <a name="network-security-group-flow-logs--version-2--upcoming-changes"></a>Flowprotokolle von Netzwerksicherheitsgruppen (Version 2): Anstehende Änderungen

Das Format für Flowprotokolle von Netzwerksicherheitsgruppen wird ab dem 15. September 2018 geändert. Neu hinzugefügte Felder stellen Informationen zum Flowzustand und zu inkrementellen Anzahlen von Bytes und Paketen, die in jede Richtung übertragen werden, bereit. Anwendungen, die Flowprotokolle analysieren, sind von dieser Änderung betroffen und müssen entsprechend aktualisiert werden. In diesem Artikel wird die Änderung detailliert beschrieben.

## <a name="what-is-changing"></a>Was ändert sich?

Die Version der Flowprotokolle von Netzwerksicherheitsgruppen ändert sich von „Version: 1“ in „Version: 2“, außerdem werden zusätzliche Felder zur *flowTuples*-Eigenschaft in den Protokollen hinzugefügt. Weitere Informationen zum Format finden Sie unter [Einführung in die Datenflussprotokollierung](#how-is-a-flow-modeled-in-version-2).

### <a name="version-1-flow-tuple-format"></a>Format der Flowtupel (Version 1)

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,D"
```

### <a name="version-2-flow-tuple-format"></a>Format der Flowtupel (Version 2)

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1,103,1,186"
```

## <a name="when-will-this-change-take-place"></a>Wann werden diese Änderungen wirksam?

Diese Änderung wird ab dem **15. September 2018** in der Region „USA, Westen-Mitte“ wirksam. Das Rollout der Änderung in die öffentlichen Cloudregionen ist am 31. November 2018 abgeschlossen. Danach sind nur Protokolle von Version 2 verfügbar.

## <a name="am-i-affected-by-the-change"></a>Bin ich von der Änderung betroffen?

Sie sind möglicherweise von dieser Änderung betroffen, wenn Sie Anwendungen erstellen oder verwenden, die Flowprotokolldaten von Netzwerksicherheitsgruppen verarbeiten.

### <a name="if-i-use-traffic-analytics"></a>Bin ich betroffen, wenn ich Traffic Analytics verwende?

Nein. Traffic Analytics ist vom Übergang von Version 1 zu Version 2 der Flowprotokollierung nicht betroffen. Erweiterungen, die in Kürze verfügbar sind, können die zusätzlichen Sitzungs- und Bandbreiteinformationen nutzen, die in den Flowprotokollen von Version 2 bereitgestellt werden.

### <a name="if-im-using-third-party-tooling"></a>Bin ich betroffen, wenn ich Drittanbietertools verwende?

Die Änderung im Protokollformat wurde im Voraus allen [Network Watcher-Partnern](https://azure.microsoft.com/services/network-watcher) mitgeteilt. Wenden Sie sich für weitere Informationen an Ihren Anbieter.

### <a name="if-i-have-built-a-custom-application-or-integration"></a>Bin ich betroffen, wenn ich eine benutzerdefinierte Anwendung oder Integration erstellt habe?

**Ja**, Sie müssen Ihre Anwendung anpassen, damit Flowprotokolle von Netzwerksicherheitsgruppen im neuen Format verarbeitet werden können.

## <a name="what-is-the-new-flow-logging-format"></a>Was ist das neue Format für die Flowprotokollierung?

Version 2 der Flowprotokolle enthält Flowtupel im folgenden Format:

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1,103,1,186".
```

Folgende Tabelle enthält Eigenschaftennamen und -beschreibungen für Version 2 der Flowtupel von Netzwerksicherheitsgruppen. Vollständige Beispieldatensätze finden Sie im [Beispielereignis für Version2](#version2sampleevent).

| Eigenschaftenname                        | Wert           | Beschreibung                                                                                                                                                 |
| ------------------------------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zeitstempel                           | 1493763938      | Der Zeitstempel, der dem Floweintrag entspricht. Das UNIX EPOCH-Format wird verwendet.                                                                                       |
| Quell-IP-Adresse                    | 185.170.185.105 |                                                                                                                                                             |
| IP-Zieladresse               | 10.2.0.4        |                                                                                                                                                             |
| Quellport                          | 35370           |                                                                                                                                                             |
| Zielport                     | 23              |                                                                                                                                                             |
| Datenverkehrprotokoll                     | T               | **T**: TCP, **U**: UDP                                                                                                                                  |
| Richtung des Datenverkehrs               | I               | **I**: eingehender Datenverkehr, **O**: ausgehender Datenverkehr                                                                                                         |
| Entscheidung zum Datenverkehr                     | A               | **A**: Flow wurde zugelassen, **D**: Flow wurde verweigert.                                                                                                         |
| Flowzustand                           | C               | **B** (Begin/Anfang): Erstellung eines Flows, Statistiken werden nicht bereitgestellt. **C** (Continue/Fortsetzung): Ein laufender Flow wird weiter fortgesetzt. Statistiken werden in Intervallen von 5 Minuten bereitgestellt. **E** (End/Beendung): Beendung eines Flows, Statistiken werden bereitgestellt.                                                                                                                                                        |
| Pakete – Quelle zu Ziel      | 1               | Die Gesamtanzahl von TCP- und UDP-Paketen, die seit dem letzten Update von der Quelle zum Ziel gesendet wurden.                                                                  |
| Gesendete Bytes – Quelle zu Ziel   | 103             | Die Gesamtanzahl von TCP- und UDP-Paketbytes, die seit dem letzten Update von der Quelle zum Ziel gesendet wurden. Paketbytes enthalten den Paketheader und die Nutzlast.         |
| Gesendete Pakete – Ziel zu Quelle | 1               | Die Gesamtanzahl von TCP- und UDP-Paketen, die seit dem letzten Update vom Ziel zur Quelle gesendet wurden.                                                                  |
| Gesendete Bytes – Ziel zu Quelle   | 186             | Die Gesamtanzahl von TCP- und UDP-Paketbytes, die seit dem letzten Update vom Ziel zur Quelle gesendet wurden. Paketbytes enthalten den Paketheader und die Nutzlast.             |

## <a name="how-is-a-flow-modeled-in-version-2"></a>Wie wird ein Flow in Version 2 modelliert?

In Version 2 der Protokollierung wird der Flowzustand eingeführt. Der Flowzustand *B* wird aufgezeichnet, wenn ein Flow initiiert wird. Die Flowzustände *C* und *E* markieren die Fortsetzung bzw. die Beendung eines Flows. Die Zustände *C* und *E* enthalten Informationen zur Bandbreite des Datenverkehrs.

**Beispiel:** Flowtupel von einer TCP-Konversation zwischen 185.170.185.105:35370 und 10.2.0.4:23:

"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,B,,,," "1493695838,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1021,588096,8005,4610880" "1493696138,185.170.185.105,10.2.0.4,35370,23,T,I,A,E,52,29952,47,27072"

### <a name="how-does-the-format-differ-from-version-1"></a>Wie unterscheidet sich das Format von Version 1?

In Version 1 wird ein Flowtupel ("1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,D") immer dann erstellt, wenn ein Flow hergestellt wird oder versucht wird, einen Flow herzustellen (Verweigerung).

### <a name="how-are-bytes-and-packets-accounted-for"></a>Wie werden Bytes und Pakete berücksichtigt?

Für die Flowzustände *C* (Fortsetzung) und *E* (Beendung) wird die Anzahl von Bytes und Paketen vom Zeitpunkt des vorherigen Flowtupeldatensatzes an aggregiert. Entsprechend der vorherigen Beispielkonversation ist die Gesamtanzahl der übertragenen Pakete 1021+52+8005+47 = 9125. Die Gesamtanzahl der übertragenen Bytes ist 588096+29952+4610880+27072 = 5256000.

### <a name="if-my-application-doesnt-understand-the-traffic-bandwidth-fields-how-does-version-2-affect-the-application"></a>Wie wirkt sich Version 2 auf meine Anwendung aus, wenn diese die Felder für die Bandbreite des Datenverkehrs nicht auswerten kann?

Anwendungen, die Version 1 der Flowprotokollierung von Netzwerksicherheitsgruppen verwenden, erfassen üblicherweise die Anzahl der eindeutigen Flows. Wenn für den Flowzustand keine Änderung an der Analyse berücksichtigt wird, wird Ihnen möglicherweise eine falsche Erhöhung der Anzahl der gemeldeten Flows angezeigt. Sie können eindeutige Flows zählen, indem Sie die Flowtupels in den Flowzuständen *C* und *E* ignorieren.

## <a name="can-i-control-the-version-format-i-receive"></a>Kann ich das Versionsformat steuern, das ich erhalte?

Nein. Das Aktivieren oder Deaktivieren von Flowprotokollen wirkt sich nicht auf das Ausgabeformat der Protokolle aus. Die Änderung der Protokolle von Version 1 in Version 2 erfolgt Region für Region. Wenn ein Upgrade von Version 1 zu Version 2 für eine Region durchgeführt wird, werden die Flowprotokolle von Netzwerksicherheitsgruppen möglicherweise in beiden Formaten angezeigt. Nach dem Rollout sind nur Protokolle von Version 2 verfügbar.

## <a name="while-the-change-occurs-can-i-see-both-formats-for-the-same-network-security-group"></a>Werden beide Formate für dieselbe Netzwerksicherheitsgruppe angezeigt, während die Änderung durchgeführt wird?

Ja. Innerhalb einer Region erfolgt der Übergang pro MAC-Adresse. Da eine Netzwerksicherheitsgruppe auf mehrere Computer angewendet werden kann, werden möglicherweise Protokolle in beiden Formaten angezeigt, die in den Speicher geschrieben werden. Die Protokolle entsprechen entweder Version 1 oder Version 2.

## <a name="will-i-see-duplicate-data"></a>Werden doppelte Daten angezeigt?

Die Flowprotokollierungsdaten werden in den verschiedenen Formaten nicht dupliziert. Ein Flow wird entweder im Format von Version 1 oder Version 2 aufgezeichnet, während die Änderung vorgenommen wird.

## <a name="how-can-i-test-the-new-format-ahead-of-time"></a>Kann ich das neue Format vorab testen?

Ja. Sie können ein Beispiel für eine Flowprotokolldatei von Version 2 [herunterladen](https://aka.ms/nsgflowlogsv2blobsample), um Ihre Lösung zu testen.

## <a name="are-there-changes-to-configuration-and-management-of-network-security-group-flow-logging"></a>Gibt es Änderungen an der Konfiguration und Verwaltung der Flowprotokollierung von Netzwerksicherheitsgruppen?

Nein. Die Unterstützung für Azure Storage-Konten für Abonnements, die denselben Azure Active Directory-Mandanten verwenden, wurde in diesem Jahr bereits [veröffentlicht](https://azure.microsoft.com/blog/new-azure-network-watcher-integrations-and-network-security-group-flow-logging-updates/). Durch diese Änderung kann die Anzahl der erforderlichen Speicherkonten bei der Verwaltung von Flowprotokollen von Netzwerksicherheitsgruppen reduziert werden.

## <a name="questions-or-feedback"></a>Haben Sie Fragen oder Feedback?

Wir freuen uns, mehr über Ihre Erfahrungen mit Flowprotokollen von Netzwerksicherheitsgruppen und Network Watcher zu erfahren. Sie können Feedback oder Vorschläge online oder per E-Mail unter AzureNetworkWatcher@microsoft.com einreichen.

## <a name="version-2-sample"></a>Beispiel für Version 2

```json
{

"records": [

{

"time": "2018-08-03T17:00:54.5657764Z",

"systemId": "bbd48473-8ce0-4834-99bb-2e13b9b23ff8",

"category": "NetworkSecurityGroupFlowEvent",

"resourceId":
"/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FLOWLOGSV2DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MULTITIERAPP2-NSG",

"operationName": "NetworkSecurityGroupFlowEvents",

"properties": {

"Version": 2,

"flows": [

{

"rule": "DefaultRule_AllowInternetOutBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315592,10.1.1.6,204.79.197.200,62375,80,T,O,A,B,,,,",

"1533315595,10.1.1.6,204.79.197.200,62373,80,T,O,A,E,30,1784,92,123631",

"1533315597,10.1.1.6,204.79.197.200,62376,80,T,O,A,B,,,,",

"1533315601,10.1.1.6,204.79.197.200,62374,80,T,O,A,E,13,866,87,123079",

"1533315603,10.1.1.6,204.79.197.200,62377,80,T,O,A,B,,,,",

"1533315604,10.1.1.6,204.79.197.200,62375,80,T,O,A,E,33,1946,90,123247",

"1533315608,10.1.1.6,204.79.197.200,62378,80,T,O,A,B,,,,",

"1533315610,10.1.1.6,204.79.197.200,62376,80,T,O,A,E,20,1244,92,123355",

"1533315613,10.1.1.6,204.79.197.200,62379,80,T,O,A,B,,,,",

"1533315616,10.1.1.6,204.79.197.200,62377,80,T,O,A,E,24,1460,92,123355",

"1533315619,10.1.1.6,204.79.197.200,62380,80,T,O,A,B,,,,",

"1533315622,10.1.1.6,204.79.197.200,62378,80,T,O,A,E,23,1406,93,123409",

"1533315624,10.1.1.6,204.79.197.200,62381,80,T,O,A,B,,,,",

"1533315628,10.1.1.6,204.79.197.200,62379,80,T,O,A,E,16,1028,88,123133",

"1533315630,10.1.1.6,204.79.197.200,62382,80,T,O,A,B,,,,",

"1533315631,10.1.1.6,204.79.197.200,62380,80,T,O,A,E,13,866,87,123079",

"1533315635,10.1.1.6,204.79.197.200,62384,80,T,O,A,B,,,,",

"1533315637,10.1.1.6,204.79.197.200,62381,80,T,O,A,E,15,974,86,123025",

"1533315640,10.1.1.6,204.79.197.200,62385,80,T,O,A,B,,,,",

"1533315643,10.1.1.6,204.79.197.200,62382,80,T,O,A,E,16,1028,88,123139",

"1533315646,10.1.1.6,204.79.197.200,62386,80,T,O,A,B,,,,",

"1533315649,10.1.1.6,204.79.197.200,62384,80,T,O,A,E,21,1298,92,123355",

"1533315651,10.1.1.6,204.79.197.200,62387,80,T,O,A,B,,,,"

]

}

]

}

]

}

},

{

"time": "2018-08-03T17:01:54.5668880Z",

"systemId": "bbd48473-8ce0-4834-99bb-2e13b9b23ff8",

"category": "NetworkSecurityGroupFlowEvent",

"resourceId":
"/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FLOWLOGSV2DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MULTITIERAPP2-NSG",

"operationName": "NetworkSecurityGroupFlowEvents",

"properties": {

"Version": 2,

"flows": [

{

"rule": "DefaultRule_DenyAllInBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315685,80.211.83.37,10.1.1.6,45321,81,T,I,D,B,,,,"

]

}

]

},

{

"rule": "DefaultRule_AllowInternetOutBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315655,10.1.1.6,204.79.197.200,62385,80,T,O,A,E,20,1244,87,123079",

"1533315657,10.1.1.6,204.79.197.200,62388,80,T,O,A,B,,,,",

"1533315658,10.1.1.6,204.79.197.200,62386,80,T,O,A,E,26,1568,92,123355",

"1533315662,10.1.1.6,204.79.197.200,62389,80,T,O,A,B,,,,",

"1533315664,10.1.1.6,204.79.197.200,62387,80,T,O,A,E,15,974,88,123133",

"1533315667,10.1.1.6,204.79.197.200,62390,80,T,O,A,B,,,,",

"1533315670,10.1.1.6,204.79.197.200,62388,80,T,O,A,E,18,1136,88,123139",

"1533315673,10.1.1.6,204.79.197.200,62391,80,T,O,A,B,,,,",

"1533315676,10.1.1.6,204.79.197.200,62389,80,T,O,A,E,14,920,87,123079",

"1533315678,10.1.1.6,204.79.197.200,62392,80,T,O,A,B,,,,",

"1533315679,10.1.1.6,204.79.197.200,62390,80,T,O,A,E,31,1838,94,123739",

"1533315684,10.1.1.6,204.79.197.200,62393,80,T,O,A,B,,,,",

"1533315685,10.1.1.6,204.79.197.200,62391,80,T,O,A,E,28,1676,101,141199",

"1533315689,10.1.1.6,204.79.197.200,62394,80,T,O,A,B,,,,",

"1533315691,10.1.1.6,204.79.197.200,62392,80,T,O,A,E,21,1298,93,123409",

"1533315694,10.1.1.6,204.79.197.200,62395,80,T,O,A,B,,,,",

"1533315697,10.1.1.6,204.79.197.200,62393,80,T,O,A,E,26,1568,91,123393",

"1533315700,10.1.1.6,204.79.197.200,62396,80,T,O,A,B,,,,",

"1533315703,10.1.1.6,204.79.197.200,62394,80,T,O,A,E,14,920,89,123187",

"1533315705,10.1.1.6,204.79.197.200,62397,80,T,O,A,B,,,,",

"1533315706,10.1.1.6,204.79.197.200,62395,80,T,O,A,E,13,866,87,123079",

"1533315711,10.1.1.6,204.79.197.200,62398,80,T,O,A,B,,,,"

]

}

]

}

]

}

}

]

}
```

## <a name="version-1-sample"></a>Beispiel für Version 1

```json
{ 

    "records": [ 

        { 

            "time": "2017-02-16T22:00:32.8950000Z", 

            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434", 

            "category": "NetworkSecurityGroupFlowEvent", 

            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG", 

            "operationName": "NetworkSecurityGroupFlowEvents", 

            "properties": { 

                "Version": 1, 

                "flows": [ 

                    { 

                        "rule": "DefaultRule_DenyAllInBound", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D" 

                                ] 

                            } 

                        ] 

                    }, 

                    { 

                        "rule": "UserRule_default-allow-rdp", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A", 

                                    "1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A", 

                                    "1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A", 

                                    "1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A" 

                                ] 

                            } 

                        ] 

                    } 

                ] 

            } 

        }, 

        { 

            "time": "2017-02-16T22:01:32.8960000Z", 

            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434", 

            "category": "NetworkSecurityGroupFlowEvent", 

            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG", 

            "operationName": "NetworkSecurityGroupFlowEvents", 

            "properties": { 

                "Version": 1, 

                "flows": [ 

                    { 

                        "rule": "DefaultRule_DenyAllInBound", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D" 

                                ] 

                            } 

                        ] 

                    }, 

                    { 

                        "rule": "UserRule_default-allow-rdp", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A",

                                    "1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A",

                                    "1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"

                                ] 

                            } 

                        ] 

                    } 

                ] 

            } 

        } 

    ] 
}
```