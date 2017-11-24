---
title: Grundlegendes zur Azure IoT Edge-Laufzeit | Microsoft-Dokumentation
description: "Hier erhalten Sie Informationen über die Azure IoT Edge-Laufzeit und wie Sie sie für Ihre Edgegeräte nutzen können."
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 7b37f9e103644d2492f69f4a4cc80d3fd57d4aa4
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture---preview"></a>Grundlegendes zur Azure IoT Edge-Laufzeit und ihre Architektur – Vorschau

Die IoT-Edge-Laufzeit ist eine Sammlung von Programmen, die auf einem Gerät installiert sein müssen, damit es als IoT Edge-Gerät gilt. Die Sammlung der Komponenten der IoT Edge-Laufzeit ermöglicht IoT Edge-Geräten den Empfang von Code für die Ausführung im Edgebereich und das Kommunizieren der Ergebnisse. 

Die IoT Edge-Laufzeit führt auf IoT Edge-Geräten die folgenden Funktionen aus:

* Installieren und Aktualisieren von Workloads auf dem Gerät
* Aufrechterhalten von Azure IoT Edge-Sicherheitsstandards auf dem Gerät
* Sicherstellen, dass die [IoT Edge-Module][Understand Azure IoT Edge modules - preview] immer ausgeführt werden
* Melden der Modulintegrität an die Cloud für die Remoteüberwachung
* Ermöglichen der Kommunikation zwischen nachgeschalteten Blattknotengeräten und dem IoT Edge-Gerät
* Ermöglichen der Kommunikation zwischen Modulen auf dem IoT Edge-Gerät
* Ermöglichen der Kommunikation zwischen dem IoT Edge-Gerät und der Cloud

![Die IoT Edge-Laufzeit übermittelt an IoT Hub Einblicke und Informationen zur Modulintegrität][1]

Die Aufgaben der IoT Edge-Laufzeit fallen in zwei Kategorien: Modulverwaltung und -kommunikation. Diese beiden Rollen werden von zwei Komponenten ausgeführt, aus denen die IoT Edge-Laufzeit besteht. Der IoT Edge-Hub ist für die Kommunikation verantwortlich, während der IoT Edge-Agent für die Bereitstellung und Überwachung der Module zuständig ist. 

Der Edge-Agent und der Edge-Hub sind Module wie jedes andere auf einem IoT Edge-Gerät ausgeführte Modul. Weitere Informationen zur Funktionsweise der Module finden Sie unter [Understand Azure IoT Edge modules - preview] (Grundlegendes zu den Azure IoT Edge-Modulen – Vorschau, in englischer Sprache) 

## <a name="iot-edge-hub"></a>IoT Edge-Hub

Der Edge-Hub ist eines der beiden Module, aus denen die Azure IoT Edge-Laufzeit besteht. Er fungiert als lokaler Proxy für IoT Hub, indem er die gleichen Protokollendpunkte wie IoT Hub verfügbar macht. Diese Konsistenz bedeutet, dass Clients (ob Geräte oder Module) auf die gleiche Weise eine Verbindung mit der IoT Edge-Laufzeit wie mit IoT Hub herstellen können. 

>[!NOTE]
> Während der öffentlichen Vorschau unterstützt der Edge-Hub nur Clients, die Verbindungen mithilfe von MQTT herstellen.

Der Edge-Hub ist keine vollständige Version des lokal ausgeführten IoT Hub. Einige Aufgaben werden vom Edge-Hub automatisch an IoT Hub delegiert. Beispielsweise leitet der Edge-Hub Authentifizierungsanforderungen an IoT Hub weiter, wenn ein Gerät zum ersten Mal eine Verbindung herzustellen versucht. Nachdem die erste Verbindung hergestellt wurde, werden die Sicherheitsinformationen vom Edge-Hub lokal zwischengespeichert. Nachfolgende Verbindungen von diesem Gerät sind zulässig, ohne dass eine Authentifizierung bei der Cloud erforderlich ist. 

>[!NOTE]
> Während der öffentlichen Vorschau muss die Laufzeit immer verbunden sein, wenn sie versucht, ein Gerät zu authentifizieren.

Der Edge-Hub optimiert die Anzahl der tatsächlich hergestellten Verbindungen mit der Cloud, um die von der IoT Edge-Lösung genutzte Bandbreite zu reduzieren. Der Edge-Hub fasst logische Verbindungen von Clients, z.B. Modulen oder Blattknotengeräten, zu einer einzelnen physischen Verbindung mit der Cloud zusammen. Die Details dieses Vorgangs sind für den Rest der Lösung transparent. Clients können nicht erkennen, dass statt einer eigenen Verbindung mit der Cloud für alle Clients eine gemeinsame Verbindung verwendet wird. 

![Der Edge-Hub fungiert als Gateway zwischen mehreren physischen Geräten und der Cloud.][2]

Der Edge-Hub kann bestimmen, ob er mit IoT Hub verbunden ist. Wenn die Verbindung unterbrochen wird, speichert der Edge-Hub Nachrichten oder Zwillingsaktualisierungen lokal. Sobald eine Verbindung wieder hergestellt wurde, werden alle Daten synchronisiert. Der Speicherort für diesen temporären Cache wird durch eine Eigenschaft des Modulzwillings des Edge-Hubs bestimmt. Die Größe des Caches wird nicht begrenzt, solange die Speicherkapazität des Geräts ausreicht. 

>[!NOTE]
>Vor der allgemeinen Verfügbarkeit des Produkts wird in dieses die Steuerung zusätzlicher Zwischenspeicherungsparameter integriert.

### <a name="module-communication"></a>Modulkommunikation

Der Edge-Hub ermöglicht die Kommunikation zwischen Modulen. Die Verwendung des Edge-Hubs als Nachrichtenbroker sorgt dafür, dass die Module unabhängig voneinander bleiben. Module müssen nur die Eingänge, an denen sie Nachrichten akzeptieren, und die Ausgänge, an die sie Nachrichten schreiben, angeben. Anschließend fügt ein Lösungsentwickler diese Eingänge und Ausgänge zusammen, damit die Module Daten in der speziellen Reihenfolge für die Lösung verarbeiten. 

![Der Edge-Hub ermöglicht die Kommunikation zwischen Modulen][3]

Ein Modul ruft zum Senden von Daten an den Edge-Hub die SendEventAsync-Methode auf. Das erste Argument gibt an, an welchen Ausgang die Nachricht gesendet werden soll. Mit dem folgenden Pseudocode wird eine Nachricht an output1 gesendet:

    DeviceClient client = new DeviceClient.CreateFromConnectionString(moduleConnectionString, settings); 
    await client.OpenAsync(); 
    await client.SendEventAsync(“output1”, message); 

Um eine Nachricht zu empfangen, registrieren Sie einen Rückruf, der eingehende Nachrichten an einem speziellen Eingang verarbeitet. Mit dem folgenden Pseudocode wird die Funktion „messageProcessor“ registriert, die für die Verarbeitung aller an input1 empfangenen Nachrichten verwendet werden soll:

    await client.SetEventHandlerAsync(“input1”, messageProcessor, userContext);
    
Der Lösungsentwickler muss die Regeln angeben, die bestimmen, wie Nachrichten vom Edge-Hub zwischen Modulen übergeben werden. Routingregeln werden in der Cloud definiert und an den Edge-Hub in dessen Gerätezwilling übertragen. Zum Definieren von Routen zwischen Modulen in Azure IoT Edge wird die gleiche Syntax wie für IoT Hub-Routen verwendet. 

<!--- For more info on how to declare routes between modules, see []. --->   

![Routing zwischen Modulen][4]

## <a name="iot-edge-agent"></a>IoT Edge-Agent

Der IoT Edge-Agent ist das zweite Modul, aus dem die Azure IoT Edge-Laufzeit besteht. Er ist für das Instanziieren von Modulen verantwortlich, stellt sicher, dass sie weiterhin ausgeführt werden, und meldet den Status der Module an IoT Hub. Der Edge-Agent verwendet den Modulzwilling wie jedes andere Modul zum Speichern der Konfigurationsdaten. 

Um die Ausführung des Edge-Agents zu starten, führen Sie den Startbefehl „azure-iot-edge-runtime-ctl.py“ aus. Der Agent ruft den Modulzwilling von IoT Hub ab und überprüft das Modulwörterbuch. Das Modulwörterbuch ist die Auflistung der Module, die gestartet werden müssen. 

Jedes Element im Modulwörterbuch enthält spezielle Informationen zu einem Modul, und es wird vom Edge-Agent zum Steuern des Lebenszyklus des Moduls verwendet. Einige der wichtigeren Eigenschaften lauten: 

* **settings.image** – Der Container, der vom Edge-Agent zum Starten des Moduls verwendet wird. Der Edge-Agent muss mit den Anmeldeinformationen für die Containerregistrierung konfiguriert werden, wenn das Image mit einem Kennwort geschützt ist. Verwenden Sie zum Konfigurieren des Edge-Agents den folgenden Befehl: `azure-iot-edge-runtime-ctl.py –configure`
* **settings.createOptions** – Eine Zeichenfolge, die beim Starten des Containers eines Moduls direkt an den Docker-Daemon übergeben wird. Das Hinzufügen von Docker-Optionen in dieser Eigenschaft ermöglicht erweiterte Optionen, z.B. Portweiterleitung oder das Bereitstellen von Volumes im Container eines Moduls.  
* **status** – Der Status, in den der Edge-Agent das Modul versetzt. Dieser Wert wird in der Regel auf *running* festgelegt, da die meisten Personen möchten, dass der Edge-Agent alle Module auf dem Gerät sofort startet. Sie können jedoch festlegen, dass der Anfangsstatus eines Moduls „Beendet“ lautet, und den Edge-Agent zu einem zukünftigen Zeitpunkt zum Starten eines Moduls auffordern. Der Edge-Agent meldet der Cloud den Status jedes Moduls in den gemeldeten Eigenschaften. Ein Unterschied zwischen der gewünschten Eigenschaft und der gemeldeten Eigenschaft ist ein Indikator oder ein fehlerhaftes Gerät. Die unterstützten Status lauten:
   * Herunterladen
   * Wird ausgeführt
   * Fehlerhaft
   * Fehler
   * Beendet
* **restartPolicy** – Legt fest, wie der Edge-Agent ein Modul neu startet. Mögliche Werte sind:
   * Nie – Der Edge-Agent führt niemals einen Neustart des Moduls aus.
   * Bei Fehler – Wenn das Modul abstürzt, wird es vom Edge-Agent neu gestartet. Wenn das Modul ordnungsgemäß heruntergefahren wird, führt der Edge-Agent keinen Neustart des Moduls aus.
   * Fehlerhaft – Wenn das Modul abstürzt oder als fehlerhaft angesehen wird, startet es der Edge-Agent neu.
   * Immer – Wenn das Modul abstürzt, als fehlerhaft angesehen wird oder auf irgend eine Weise heruntergefahren wird, führt der Edge-Agent den Neustart des Moduls aus. 
   
### <a name="security"></a>Sicherheit

Der IoT Edge-Agent hat eine wichtige Funktion für die Sicherheit eines IoT Edge-Geräts. Er überprüft beispielsweise das Image eines Moduls, bevor er es startet. Diese Features werden bei allgemeiner Verfügbarkeit von V2-Features hinzugefügt. 

<!-- For more information about the Azure IoT Edge security framework, see []. -->

## <a name="next-steps"></a>Nächste Schritte

- [Understand Azure IoT Edge modules - preview][Understand Azure IoT Edge modules - preview] (Grundlegendes zu den Azure IoT Edge-Modulen – Vorschau, in englischer Sprache)

<!-- Images -->
[1]: ./media/iot-edge-runtime/pipeline.png
[2]: ./media/iot-edge-runtime/gateway.png
[3]: ./media/iot-edge-runtime/ModuleEndpoints.png
[4]: ./media/iot-edge-runtime/ModuleEndpointsWithRoutes.png

<!-- Links -->
[Understand Azure IoT Edge modules - preview]: iot-edge-modules.md