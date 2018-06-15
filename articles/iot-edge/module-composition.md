---
title: Azure IoT Edge-Modulzusammenstellung | Microsoft Docs
description: Erfahren Sie, was in Azure IoT Edge-Module aufgenommen wird und wie sie wiederverwendet werden können.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/23/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: c886d1d9dea120a243693c12ae861a58126daadc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631682"
---
# <a name="understand-how-iot-edge-modules-can-be-used-configured-and-reused---preview"></a>Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können – Vorschau

Jedes IoT Edge-Gerät führt mindestens zwei Module aus, die die IoT Edge-Runtime bilden: „$edgeAgent“ und „$edgeHub“. Zusätzlich zu diesen zwei Standardmodulen kann jedes IoT Edge-Gerät mehrere Module ausführen, um eine beliebige Anzahl von Prozessen auszuführen. Wenn Sie all diese Module gleichzeitig auf einem Gerät bereitstellen, müssen Sie deklarieren, welche Module enthalten sind und wie sie miteinander interagieren. 

Das *Bereitstellungsmanifest* ist ein JSON-Dokument, das Folgendes beschreibt:

* Welche IoT Edge-Module bereitgestellt werden müssen sowie die zugehörigen Erstellungs- und Verwaltungsoptionen
* Die Konfiguration des Edge-Hubs, die u. a. definiert, wie Nachrichten zwischen Modulen und schließlich an IoT Hub gesendet werden
* Optional die Werte, die in den gewünschten Eigenschaften der Modulzwillinge festgelegt werden, um die einzelnen Modulanwendungen zu konfigurieren

Alle IoT Edge-Geräte müssen mit einem Bereitstellungsmanifest konfiguriert werden. Eine neu installierte IoT Edge-Runtime meldet einen Fehlercode, solange kein gültiges Manifest konfiguriert ist. 

In den Azure IoT Edge-Tutorials erstellen Sie ein Bereitstellungsmanifest mithilfe eines Assistenten im Azure IoT Edge-Portal. Sie können auch ein Bereitstellungsmanifest programmgesteuert mithilfe von REST oder des IoT Hub Service SDK anwenden. Weitere Informationen zu IoT Edge-Bereitstellungen finden Sie unter [Bereitstellen und Überwachen][lnk-deploy].

## <a name="create-a-deployment-manifest"></a>Erstellen eines Bereitstellungsmanifests

Allgemein beschrieben konfiguriert das Bereitstellungsmanifest die gewünschten Eigenschaften eines Modulzwillings für IoT Edge-Module, die auf einem IoT Edge-Gerät bereitgestellt werden. Zwei dieser Module sind immer vorhanden: der Edge-Agent und der Edge-Hub.

Ein Bereitstellungsmanifest, das nur die IoT Edge-Runtime (Agent und Hub) enthält, ist gültig.

Das Manifest folgt dieser Struktur:

```json
{
    "moduleContent": {
        "$edgeAgent": {
            "properties.desired": {
                // desired properties of the Edge agent
                // includes the image URIs of all modules
            }
        },
        "$edgeHub": {
            "properties.desired": {
                // desired properties of the Edge hub
                // includes the routing information between modules, and to IoT Hub
            }
        },
        "{module1}": {  // optional
            "properties.desired": {
                // desired properties of module with id {module1}
            }
        },
        "{module2}": {  // optional
            ...
        },
        ...
    }
}
```

## <a name="configure-modules"></a>Konfigurieren von Modulen

Zusätzlich zum Einrichten der gewünschten Eigenschaften aller Module, die Sie bereitstellen möchten, müssen Sie die IoT Edge-Runtime anweisen, wie die Module installiert werden sollen. Die Konfigurations- und Verwaltungsinformationen für alle Module werden in den gewünschten **$edgeAgent**-Eigenschaften festgelegt. Zu diesen Informationen zählen die Konfigurationsparameter für den Edge-Agent selbst. 

Eine vollständige Liste der Eigenschaften, die hinzugefügt werden können oder müssen, finden Sie unter [Properties of the Edge agent and Edge hub](module-edgeagent-edgehub.md) (Eigenschaften des Edge-Agents und Edge-Hubs).

Die $edgeAgent-Eigenschaften weisen die folgende Struktur auf:

```json
"$edgeAgent": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
        },
        "systemModules": {
            "edgeAgent": {
                // configuration and management details
            },
            "edgeHub": {
                // configuration and management details
            }
        },
        "modules": {
            "{module1}": { //optional
                // configuration and management details
            },
            "{module2}": { // optional
                // configuration and management details
            }
        }
    }
},
```

## <a name="declare-routes"></a>Deklarieren von Routen

Der Edge-Hub bietet eine Möglichkeit, Nachrichten deklarativ zwischen mehreren Modulen sowie zwischen Modulen und IoT Hub zu übertragen. Der Edge-Hub verwaltet die gesamte Kommunikation. Die Routeninformationen werden daher in den gewünschten **$edgeHub**-Eigenschaften festgelegt. Sie können innerhalb einer Bereitstellung mehrere Routen verwenden.

Routen werden in den gewünschten **$edgeHub**-Eigenschaften mit der folgenden Syntax deklariert:

```json
"$edgeHub": {
    "properties.desired": {
        "routes": {
            "{route1}": "FROM <source> WHERE <condition> INTO <sink>",
            "{route2}": "FROM <source> WHERE <condition> INTO <sink>"
        },
    }
}
```

Für jede Route sind eine Quelle und eine Senke erforderlich, die Bedingung ist jedoch optional. Sie kann bei Bedarf zum Filtern von Nachrichten verwendet werden. 


### <a name="source"></a>Quelle
Die Quelle gibt an, woher die Nachrichten stammen. Dabei kann es sich um einen der folgenden Werte handeln:

| Quelle | BESCHREIBUNG |
| ------ | ----------- |
| `/*` | Alle Gerät-zu-Cloud-Nachrichten von beliebigen Geräten oder Modulen |
| `/messages/*` | Alle Gerät-zu-Cloud-Nachrichten, die von einem Gerät oder einem Modul über eine beliebige oder keine Ausgabe versendet wurden |
| `/messages/modules/*` | Alle Gerät-zu-Cloud-Nachrichten, die von einem Modul über eine beliebige oder keine Ausgabe versendet wurden |
| `/messages/modules/{moduleId}/*` | Alle Gerät-zu-Cloud-Nachrichten, die von {moduleId} über keine Ausgabe versendet wurden |
| `/messages/modules/{moduleId}/outputs/*` | Alle Gerät-zu-Cloud-Nachrichten, die von {moduleId} über eine beliebige Ausgabe versendet wurden |
| `/messages/modules/{moduleId}/outputs/{output}` | Alle Gerät-zu-Cloud-Nachrichten, die von {moduleId} über {output} versendet wurden |

### <a name="condition"></a>Bedingung
Die Bedingung ist in einer Routendeklaration optional. Wenn Sie alle Nachrichten von der Senke an die Quelle übergeben möchten, lassen Sie die **WHERE**-Klausel ganz weg. Alternativ können Sie die [IoT Hub-Abfragesprache][lnk-iothub-query] verwenden, um nach bestimmten Nachrichten oder Nachrichtentypen zu filtern, die die Bedingung erfüllen.

Das Format der Nachrichten, die zwischen Modulen in IoT Edge übermittelt werden, entspricht dem Format der Nachrichten, die zwischen Ihren Geräten und Azure IoT Hub übermittelt werden. Alle Nachrichten liegen im JSON-Format vor und verfügen über die Parameter **systemProperties**, **appProperties** und **body**. 

Sie können Abfragen für alle drei Parameter erstellen. Verwenden Sie dazu die folgende Syntax: 

* Systemeigenschaften: `$<propertyName>` oder `{$<propertyName>}`
* Anwendungseigenschaften: `<propertyName>`
* Texteigenschaften: `$body.<propertyName>` 

Beispiele zum Erstellen von Abfragen für Nachrichteneigenschaften finden Sie unter [D2C-Nachrichtenrouten-Abfrageausdrücke](../iot-hub/iot-hub-devguide-query-language.md#device-to-cloud-message-routes-query-expressions).

Ein spezifisches Beispiel für IoT Edge ist das Filtern nach Nachrichten, die von einem Blattgerät gesendet wurden und bei einem Gatewaygerät eingegangen sind. Nachrichten von Modulen enthalten eine spezifische Systemeigenschaft namens **connectionModuleId**. Wenn Sie Nachrichten von Blattgeräten direkt an IoT Hub weiterleiten möchten, verwenden Sie die folgende Route, um Modulnachrichten auszuschließen:

```sql
FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO $upstream
```

### <a name="sink"></a>Senke
Die Senke definiert, wohin die Nachrichten gesendet werden. Dabei kann es sich um einen der folgenden Werte handeln:

| Senke | BESCHREIBUNG |
| ---- | ----------- |
| `$upstream` | Sendet die Nachricht an IoT Hub |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Sendet die Nachricht an Eingang `{input}` von Modul `{moduleId}` |

Beachten Sie, dass der Edge-Hub At-Least-Once-Garantien bietet. Dies bedeutet, dass Nachrichten lokal gespeichert werden, wenn eine Route die Nachricht nicht an ihre Senke übermitteln kann. Dies kann beispielsweise der Fall sein, wenn der Edge-Hub keine Verbindung mit IoT Hub herstellen kann oder das Zielmodul nicht verbunden ist.

Der Edge-Hub speichert die Nachrichten bis zu dem Zeitpunkt, der in der `storeAndForwardConfiguration.timeToLiveSecs`-Eigenschaft in den gewünschten [Edge-Hubeigenschaften](module-edgeagent-edgehub.md) angegeben ist.

## <a name="define-or-update-desired-properties"></a>Definieren oder Aktualisieren der gewünschten Eigenschaften 

Im Bereitstellungsmanifest können gewünschte Eigenschaften für den Modulzwilling oder für einzelne auf dem IoT Edge-Gerät bereitgestellte Module festgelegt werden. Wenn die gewünschten Eigenschaften im Bereitstellungsmanifest angegeben sind, überschreiben sie alle gewünschten Eigenschaften, die gegenwärtig im Modulzwilling vorhanden sind.

Wenn Sie die gewünschten Eigenschaften eines Modulzwillings nicht im Bereitstellungsmanifest angeben, ändert IoT Hub den Modulzwilling nicht ab, und Sie können die gewünschten Eigenschaften programmgesteuert festlegen.

Die gleichen Verfahren, mit denen Sie Gerätezwillinge ändern, werden auch zum Ändern von Modulzwillingen verwendet. Weitere Informationen finden Sie im [Entwicklerhandbuch für Gerätezwillinge](../iot-hub/iot-hub-devguide-device-twins.md).   

## <a name="deployment-manifest-example"></a>Beispiel für ein Bereitstellungsmanifest

Dies ist ein Beispiel für das JSON-Dokument eines Bereitstellungsmanifests.

```json
{
"moduleContent": {
    "$edgeAgent": {
        "properties.desired": {
            "schemaVersion": "1.0",
            "runtime": {
                "type": "docker",
                "settings": {
                    "minDockerVersion": "v1.25",
                    "loggingOptions": ""
                }
            },
            "systemModules": {
                "edgeAgent": {
                    "type": "docker",
                    "settings": {
                    "image": "microsoft/azureiotedge-agent:1.0-preview",
                    "createOptions": ""
                    }
                },
                "edgeHub": {
                    "type": "docker",
                    "status": "running",
                    "restartPolicy": "always",
                    "settings": {
                    "image": "microsoft/azureiotedge-hub:1.0-preview",
                    "createOptions": ""
                    }
                }
            },
            "modules": {
                "tempSensor": {
                    "version": "1.0",
                    "type": "docker",
                    "status": "running",
                    "restartPolicy": "always",
                    "settings": {
                    "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
                    "createOptions": "{}"
                    }
                },
                "filtermodule": {
                    "version": "1.0",
                    "type": "docker",
                    "status": "running",
                    "restartPolicy": "always",
                    "settings": {
                    "image": "myacr.azurecr.io/filtermodule:latest",
                    "createOptions": "{}"
                    }
                }
            }
        }
    },
    "$edgeHub": {
        "properties.desired": {
            "schemaVersion": "1.0",
            "routes": {
                "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
                "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
            },
            "storeAndForwardConfiguration": {
                "timeToLiveSecs": 10
            }
        }
    }
}
}
```

## <a name="next-steps"></a>Nächste Schritte

* Eine vollständige Liste der Eigenschaften, die in „$edgeAgent“ und „$edgeHub“ hinzugefügt werden können oder müssen, finden Sie unter [Properties of the Edge agent and Edge hub](module-edgeagent-edgehub.md) (Eigenschaften des Edge-Agents und Edge-Hubs).

* Sie wissen jetzt, wie IoT Edge-Module verwendet werden. Im nächsten Schritt lernen Sie [die Anforderungen und Tools für die Entwicklung von IoT Edge-Modulen][lnk-module-dev] kennen.

[lnk-deploy]: module-deployment-monitoring.md
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-module-dev]: module-development.md
