---
title: Azure IoT Edge-Modulzusammenstellung | Microsoft Docs
description: "Erfahren Sie, was in Azure IoT Edge-Module aufgenommen wird und wie sie wiederverwendet werden können."
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5de67b6f1ce79934a3a6aab623d2e77a56a8ce76
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="understand-how-iot-edge-modules-can-be-used-configured-and-reused---preview"></a>Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können – Vorschau

Mit Azure IoT Edge können Sie mehrere IoT Edge-Module vor dem Bereitstellen auf IoT Edge-Geräten zusammenstellen. Dieser Artikel beschreibt die wichtigsten Konzepte in Bezug auf die Zusammenstellung mehrere IoT Edge-Module vor der Bereitstellung. 

## <a name="the-deployment-manifest"></a>Das Bereitstellungsmanifest
Das *Bereitstellungsmanifest* ist ein JSON-Dokument, das Folgendes beschreibt:

* Welche IoT Edge-Module bereitgestellt werden müssen und welche Erstellungs- und Verwaltungsoptionen vorhanden sind
* Die Konfiguration des Edge-Hubs, die beschreibt, wie Nachrichten zwischen Modulen sowie zwischen Modulen und IoT Hub übergeben werden müssen
* Optional die Werte, die in den gewünschten Eigenschaften der Modulzwillinge festgelegt werden, um die einzelnen Modulanwendungen zu konfigurieren

In den Azure IoT Edge-Tutorials erstellen Sie ein Bereitstellungsmanifest mithilfe eines Assistenten im Azure IoT Edge-Portal. Sie können auch ein Bereitstellungsmanifest programmgesteuert mithilfe von REST oder des IoT Hub Service SDK anwenden. Weitere Informationen zu IoT Edge-Bereitstellungen finden Sie unter [Bereitstellen und Überwachen][lnk-deploy].

Allgemein beschrieben konfiguriert das Bereitstellungsmanifest die gewünschten Eigenschaften eines Modulzwillings für IoT Edge-Module, die auf einem IoT Edge-Gerät bereitgestellt werden. Zwei dieser Module sind immer vorhanden: der Edge-Agent und der Edge-Hub.

Das Manifest folgt dieser Struktur:

    {
        "moduleContent": {
            "$edgeAgent": {
                "properties.desired": {
                    // desired properties of the Edge agent
                }
            },
            "$edgeHub": {
                "properties.desired": {
                    // desired properties of the Edge hub
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

Ein Beispiel für ein Bereitstellungsmanifest wird am Ende dieses Abschnitts gezeigt.

> [!IMPORTANT]
> Alle IoT Edge-Geräte müssen mit einem Bereitstellungsmanifest konfiguriert werden. Eine neu installierte IoT Edge-Runtime meldet einen Fehlercode, solange kein gültiges Manifest konfiguriert ist. 

### <a name="specify-the-modules"></a>Angeben der Module
Die gewünschten Eigenschaften des Modulzwillings des Edge-Agents enthalten die gewünschten Module, ihre Konfigurations- und Verwaltungsoptionen sowie Konfigurationsparameter für den Edge-Agent.

Allgemein beschrieben umfasst dieser Abschnitt des Manifests die Verweise auf die Modulimages und Verwaltungsoptionen für die IoT Edge-Runtimemodule (Edge-Agent und Edge-Hub) sowie die benutzerspezifischen Module.

Unter [Gewünschte Eigenschaften des Edge-Agents][lnk-edgeagent-desired] finden Sie eine ausführliche Beschreibung dieses Manifestabschnitts.

> [!NOTE]
> Auch ein Bereitstellungsmanifest, dass nur die IoT Edge-Runtime (Agent und Hub) enthält, ist gültig.


### <a name="specify-the-routes"></a>Angeben der Routen
Der Edge-Hub bietet eine Möglichkeit, Nachrichten deklarativ zwischen mehreren Modulen sowie zwischen Modulen und IoT Hub zu übertragen.

Routen haben die folgende Syntax:

        FROM <source>
        [WHERE <condition>]
        INTO <sink>

Als *Quelle* kommen alle folgenden Nachrichten infrage:

| Quelle | BESCHREIBUNG |
| ------ | ----------- |
| `/*` | Alle Gerät-zu-Cloud-Nachrichten von beliebigen Geräten oder Modulen |
| `/messages/*` | Alle Gerät-zu-Cloud-Nachrichten, die von einem Gerät oder einem Modul über eine beliebige oder keine Ausgabe versendet wurden |
| `/messages/modules/*` | Alle Gerät-zu-Cloud-Nachrichten, die von einem Modul über eine beliebige oder keine Ausgabe versendet wurden |
| `/messages/modules/{moduleId}/*` | Alle Gerät-zu-Cloud-Nachrichten, die von {moduleId} über keine Ausgabe versendet wurden |
| `/messages/modules/{moduleId}/outputs/*` | Alle Gerät-zu-Cloud-Nachrichten, die von {moduleId} über eine beliebige Ausgabe versendet wurden |
| `/messages/modules/{moduleId}/outputs/{output}` | Alle Gerät-zu-Cloud-Nachrichten, die von {moduleId} über {output} versendet wurden |

Jede Bedingung, die von der [IoT Hub-Abfragesprache][lnk-iothub-query] für IoT Hub-Routingregeln unterstützt wird, kommt als Bedingung infrage.

Als Senke wird eines der folgenden Elemente verwendet:

| Senke | BESCHREIBUNG |
| ---- | ----------- |
| `$upstream` | Sendet die Nachricht an IoT Hub |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Sendet die Nachricht an Eingang `{input}` von Modul `{moduleId}` |

Es ist wichtig zu beachten, dass der Edge-Hub so genannte At-Least-Once-Zusicherungen enthält, d.h., Nachrichten werden lokal für den Fall gespeichert, dass eine Route die Nachricht nicht an die vorgesehene Senkel übertragen kann, wenn beispielsweise der Edge-Hub keine Verbindung mit IoT Hub herstellen kann oder das Zielmodul nicht angeschlossen ist.

Der Edge-Hub speichert Nachrichten bis zu dem Zeitpunkt, der in der `storeAndForwardConfiguration.timeToLiveSecs`-Eigenschaft in den gewünschten Edge-Hubeigenschaften angegeben ist.

### <a name="specifying-the-desired-properties-of-the-module-twin"></a>Angeben der gewünschten Eigenschaften des Modulzwillings

Das Bereitstellungsmanifest kann für jedes im Edge-Agent-Abschnitt angegebene Benutzermodul die gewünschten Eigenschaften des Modulzwillings angeben.

Wenn die gewünschten Eigenschaften im Bereitstellungsmanifest angegeben sind, überschreiben sie alle gewünschten Eigenschaften, die gegenwärtig im Modulzwilling vorhanden sind.

Wenn Sie die gewünschten Eigenschaften eines Modulzwillings nicht im Bereitstellungsmanifest angeben, ändert IoT Hub den Modulzwilling nicht ab, und Sie können die gewünschten Eigenschaften programmgesteuert festlegen.

Die gleichen Verfahren, mit denen Sie Gerätezwillinge ändern, werden auch zum Ändern von Modulzwillingen verwendet. Weitere Informationen finden Sie im [Entwicklerhandbuch für Gerätezwillinge](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins).   

### <a name="deployment-manifest-example"></a>Beispiel für ein Bereitstellungsmanifest

Dies ist ein Beispiel für das JSON-Dokument eines Bereitstellungsmanifests.

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

## <a name="reference-edge-agent-module-twin"></a>Referenz: Edge-Agent-Modulzwilling

Der Modulzwilling für den Edge-Agent heißt `$edgeAgent` und koordiniert die Kommunikationsvorgänge zwischen dem auf einem Gerät ausgeführten Edge-Agent und IoT Hub.
Die gewünschten Eigenschaften werden festgelegt, wenn ein Bereitstellungsmanifest auf einem bestimmten Gerät im Zuge einer Einzelgerät- oder einer umfassenden Bereitstellung angewendet wird. Weitere Informationen zur Bereitstellung von Modulen auf IoT Edge-Geräten finden Sie unter [Bereitstellung und Überwachung][lnk-deploy].

### <a name="edge-agent-twin-desired-properties"></a>Edge-Agent, gewünschte Eigenschaften von Gerätezwillingen

| Eigenschaft | BESCHREIBUNG | Erforderlich |
| -------- | ----------- | -------- |
| Schemaversion | Muss „1.0“ sein. | Ja |
| runtime.type | Muss „Docker“ sein. | Ja |
| runtime.settings.minDockerVersion | Legen Sie hier die für dieses Bereitstellungsmanifest mindestens erforderliche Docker-Version fest. | Ja |
| runtime.settings.loggingOptions | In eine Zeichenfolge umgewandelter JSON-Code mit den Protokollierungsoptionen für den Edge-Agent-Container. [Docker-Protokollierungsoptionen][lnk-docker-logging-options] | Nein  |
| systemModules.edgeAgent.type | Muss „Docker“ sein. | Ja |
| systemModules.edgeAgent.settings.image | Der URI des Edge-Agent-Image. Der Edge-Agent ist gegenwärtig nicht in der Lage, sich selbst zu aktualisieren. | Ja |
| systemModules.edgeAgent.settings.createOptions | In eine Zeichenfolge umgewandelter JSON-Code, der die Optionen für die Erstellung des Edge-Agent-Containers enthält. [Optionen zum Erstellen von Docker][lnk-docker-create-options] | Nein  |
| systemModules.edgeAgent.configuration.id | Die ID der Bereitstellung, von der dieses Modul bereitgestellt wurde. | Sie wird von IoT Hub festgelegt, wenn dieses Manifest mithilfe einer Bereitstellung angewendet wird. Nicht Teil eines Bereitstellungsmanifests. |
| systemModules.edgeHub.type | Muss „Docker“ sein. | Ja |
| systemModules.edgeHub.type | Muss „running“ sein. | Ja |
| systemModules.edgeHub.restartPolicy | Muss „always“ sein. | Ja |
| systemModules.edgeHub.settings.image | Der URI des Image des Edge-Hubs. | Ja |
| systemModules.edgeHub.settings.createOptions | In eine Zeichenfolge umgewandelter JSON-Code mit den Optionen für die Erstellung des Edge-Hubcontainers. [Optionen zum Erstellen von Docker][lnk-docker-create-options] | Nein  |
| systemModules.edgeHub.configuration.id | Die ID der Bereitstellung, von der dieses Modul bereitgestellt wurde. | Sie wird von IoT Hub festgelegt, wenn dieses Manifest mithilfe einer Bereitstellung angewendet wird. Nicht Teil eines Bereitstellungsmanifests. |
| modules.{moduleId}.version | Benutzerdefinierte Zeichenfolge, die die Version des Moduls darstellt. | Ja |
| modules.{moduleId}.type | Muss „Docker“ sein. | Ja |
| modules.{moduleId}.restartPolicy | {"never" \| "on-failed" \| "on-unhealthy" \| "always"} | Ja |
| modules.{moduleId}.settings.image | URI des Modulimage. | Ja |
| modules.{moduleId}.settings.createOptions | In eine Zeichenfolge umgewandelter JSON-Code mit den Optionen für die Erstellung des Modulcontainers. [Optionen zum Erstellen von Docker][lnk-docker-create-options] | Nein  |
| modules.{moduleId}.configuration.id | Die ID der Bereitstellung, von der dieses Modul bereitgestellt wurde. | Sie wird von IoT Hub festgelegt, wenn dieses Manifest mithilfe einer Bereitstellung angewendet wird. Nicht Teil eines Bereitstellungsmanifests. |

### <a name="edge-agent-twin-reported-properties"></a>Vom Edge-Agent-Zwilling gemeldete Eigenschaften

Zu den vom Edge-Agent gemeldeten Eigenschaften gehören im Wesentlichen drei Informationen:

1. Status der Anwendung der zuletzt erkannten gewünschten Eigenschaften
2. Status der Module, die nach Angaben des Edge-Agents derzeit auf dem Gerät ausgeführt werden
3. Kopie der gewünschten Eigenschaften, die derzeit auf dem Gerät ausgeführt werden

Diese letzte Information ist nützlich für den Fall, dass die letzten gewünschten Eigenschaften nicht erfolgreich von der Runtime angewendet wurden und das Gerät demzufolge noch ein vorheriges Bereitstellungsmanifest ausführt.

> [!NOTE]
> Die gemeldeten Eigenschaften des Edge-Agents sind hilfreich, da sie mit der [IoT Hub-Abfragesprache][lnk-iothub-query] abgefragt werden können, um den Status der Bereitstellungen großflächig zu untersuchen. Weitere Informationen zur Verwendung dieses Features finden Sie unter [Bereitstellungen][lnk-deploy].

In der folgenden Tabelle sind die aus den gewünschten Eigenschaften kopierten Informationen nicht enthalten.

| Eigenschaft | BESCHREIBUNG |
| -------- | ----------- |
| lastDesiredVersion | Dieser ganzzahlige Wert bezeichnet die letzte vom Edge-Agent verarbeitete Version der gewünschten Eigenschaften. |
| lastDesiredStatus.code | Dieser Statuscode nennt die letzten gewünschten Eigenschaften, die der Edge-Agent erkannt hat. Zulässige Werte: `200` Success (Erfolg), `400` Invalid Configuration (Ungültige Konfiguration), `412` Invalid Schema Version (Ungültige Schemaversion), `417` The Desired Properties are Empty (Die gewünschten Eigenschaften sind leer), `500` Failed (Fehler) |
| lastDesiredStatus.description | Textbeschreibung des Status |
| deviceHealth | `healthy`, wenn der Runtimestatus aller Module entweder `running` oder `stopped` ist, andernfalls `unhealthy` |
| configurationHealth.{deploymentId}.health | `healthy`, wenn der von der Bereitstellung {deploymentId} festgelegte Runtimestatus aller Module `running` oder `stopped` ist, andernfalls `unhealthy` |
| runtime.platform.OS | Meldung des auf dem Gerät ausgeführten Betriebssystems |
| runtime.platform.architecture | Meldung der CPU-Architektur auf dem Gerät |
| systemModules.edgeAgent.runtimeStatus | Gemeldeter Status des Edge-Agents: {"running" \| "unhealthy"} |
| systemModules.edgeAgent.statusDescription | Textbeschreibung des gemeldeten Status des Edge-Agents. |
| systemModules.edgeHub.runtimeStatus | Aktueller Status des Edge-Hubs: { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| systemModules.edgeHub.statusDescription | Textbeschreibung des aktuellen Status des Edge-Hub, falls „fehlerhaft“ |
| systemModules.edgeHub.exitCode | Der vom Edge-Hubcontainer gemeldete Exitcode beim Beenden |
| systemModules.edgeHub.startTimeUtc | Zeitpunkt des letzten Starts des Edge-Hubs |
| systemModules.edgeHub.lastExitTimeUtc | Zeitpunkt des letzten Beendens des Edge-Hubs |
| systemModules.edgeHub.lastRestartTimeUtc | Zeitpunkt des letzten Neustarts des Edge-Hubs |
| systemModules.edgeHub.lastRestartTimeUtc | Neustarthäufigkeit dieses Moduls aufgrund der Neustartrichtlinie |
| modules.{moduleId}.runtimeStatus | Aktueller Status des Moduls: { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| modules.{moduleId}.statusDescription | Textbeschreibung des aktuellen Modulstatus, falls fehlerhaft |
| modules.{moduleId}.exitCode | Der vom Modulcontainer gemeldete Exitcode beim Beenden |
| modules.{moduleId}.startTimeUtc | Zeitpunkt des letzten Modulstarts |
| modules.{moduleId}.lastExitTimeUtc | Zeitpunkt der letzten Modulbeendigung |
| modules.{moduleId}.lastRestartTimeUtc | Zeitpunkt des letzten Modulneustarts |
| modules.{moduleId}.restartCount | Neustarthäufigkeit dieses Moduls aufgrund der Neustartrichtlinie |

## <a name="reference-edge-hub-module-twin"></a>Referenz: Edge-Hubmodulzwilling

Der Modulzwilling für den Edge-Hub wird als `$edgeHub` bezeichnet und koordiniert die Kommunikationsvorgänge zwischen dem auf einem Gerät ausgeführten Edge-Hub und IoT Hub.
Die gewünschten Eigenschaften werden festgelegt, wenn ein Bereitstellungsmanifest auf einem bestimmten Gerät im Zuge einer Einzelgeräte- oder einer umfassenden Bereitstellung angewendet wird. Weitere Informationen zur Bereitstellung von Modulen auf IoT Edge-Geräten finden Sie unter [Bereitstellungen][lnk-deploy].

### <a name="edge-hub-twin-desired-properties"></a>Edge-Hub, gewünschte Eigenschaften von Zwillingen

| Eigenschaft | BESCHREIBUNG | Im Bereitstellungsmanifest erforderlich |
| -------- | ----------- | -------- |
| Schemaversion | Muss „1.0“ sein. | Ja |
| routes.{routeName} | Eine Zeichenfolge, die eine Edge-Hubroute darstellt. | Das `routes`-Element kann vorhanden, aber leer sein. |
| storeAndForwardConfiguration.timeToLiveSecs | Zeit in Sekunden, für die der Edge-Hub Nachrichten nach einer Trennung von Routingendpunkten aufbewahrt, beispielsweise nach einer Trennung von IoT Hub oder einem lokalen Modul | Ja |

### <a name="edge-hub-twin-reported-properties"></a>Edge-Hub, gemeldete Eigenschaften von Zwillingen

| Eigenschaft | BESCHREIBUNG |
| -------- | ----------- |
| lastDesiredVersion | Dieser ganzzahlige Wert gibt die letzte Version der gewünschten Eigenschaften an, die vom Edge-Hub verarbeitet wurde. |
| lastDesiredStatus.code | Dieser Statuscode nennt die letzten gewünschten Eigenschaften, die der Edge-Hub erkannt hat. Zulässige Werte: `200` Success (Erfolg), `400` Invalid Configuration (ungültige Konfiguration), `500` Failed (Fehler) |
| lastDesiredStatus.description | Textbeschreibung des Status |
| clients.{device or module identity}.status | Konnektivitätsstatus dieses Geräts oder Moduls. Mögliche Werte: {"connected" \| "disconnected"}. Nur Modulidentitäten können den Status „disconnected“ aufweisen. Nachgeschaltete Geräte, die eine Verbindung mit dem Edge-Hub herstellen, erscheinen nur bei erfolgreicher Verbindung. |
| clients.{device or module identity}.lastConnectTime | Letzter Verbindungszeitpunkt des Geräts oder Moduls |
| clients.{device or module identity}.lastDisconnectTime | Letzter Trennungszeitpunkt des Geräts oder Moduls |

## <a name="next-steps"></a>Nächste Schritte

Sie wissen jetzt, wie IoT Edge-Module verwendet werden. Im nächsten Schritt lernen Sie [die Anforderungen und Tools für die Entwicklung von IoT Edge-Modulen][lnk-module-dev] kennen.

[lnk-deploy]: module-deployment-monitoring.md
[lnk-edgeagent-desired]: #edge-agent-twin-desired-properties
[lnk-edgeagent-reported]: #edge-agent-twin-reported-properties
[lnk=edgehub-desired]: #edge-hub-twin-desired-properties
[lnk-edgehub-reported]: #edge-hub-twin-reported-properties
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-module-dev]: module-development.md
