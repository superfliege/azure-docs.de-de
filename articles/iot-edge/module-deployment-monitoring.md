---
title: Bereitstellen von Modulen für Azure IoT Edge | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Module auf Edge-Geräten bereitgestellt werden.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f64e6db576b7b1605cc070948a021184fc6ee8ad
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029259"
---
# <a name="understand-iot-edge-deployments-for-single-devices-or-at-scale"></a>Grundlegendes zu IoT Edge-Bereitstellungen für einzelne Geräte oder nach Bedarf

Für Azure IoT Edge-Geräte gilt ein [Gerätelebenszyklus][lnk-lifecycle], der weitgehend dem von anderen Typen von IoT-Geräten entspricht:

1. IoT Edge-Geräte werden bereitgestellt. Hierzu gehören die Imageerstellung auf einem Gerät mit einem Betriebssystem und das Installieren der [IoT Edge-Laufzeit][lnk-runtime].
1. Die Geräte werden zum Ausführen von [IoT Edge-Modulen][lnk-modules] konfiguriert, und ihre Integrität wird überwacht. 
1. Schließlich können Geräte außer Kraft gesetzt werden, wenn sie ersetzt werden oder veraltet sind.  

Azure IoT Edge bietet zwei Möglichkeiten zum Konfigurieren der Module, die auf IoT Edge-Geräten ausgeführt werden: eine für Entwicklungsarbeiten und schnelle Iterationen auf einem einzelnen Gerät (diese Methode wurde in den Azure IoT Edge-Tutorials verwendet) und eine für die Verwaltung umfangreicher IoT Edge-Gerätebestände. Beide Ansätze sind im Azure-Portal und programmgesteuert verfügbar.

Der Schwerpunkt dieses Artikels liegt auf der Konfigurations- und Überwachungsphase für größere Gerätebestände, die zusammen als automatische IoT Edge-Bereitstellungen bezeichnet werden. Die allgemeinen Schritte zur Bereitstellung lauten wie folgt:   

1. Ein Operator definiert eine Bereitstellung, in der ein Satz von Modulen sowie die Zielgeräte beschrieben werden. Jede Bereitstellung verfügt über ein Bereitstellungsmanifest, das diese Informationen enthält. 
1. Der IoT Hub-Dienst kommuniziert mit allen Zielgeräten, um sie mit den gewünschten Modulen zu konfigurieren. 
1. Der IoT Hub-Dienst ruft den Status von den IoT Edge-Geräten ab und gibt diese für den Operator zur Überwachung aus.  Beispielsweise kann ein Operator sehen, wenn ein Edge-Gerät nicht erfolgreich konfiguriert wurde oder wenn bei einem Modul während der Laufzeit ein Fehler auftritt. 
1. Zu jedem Zeitpunkt werden neue IoT Edge-Geräte, die die Zielbedingungen erfüllen, für die Bereitstellung konfiguriert. Eine Bereitstellung, die auf alle IoT Edge-Geräte in Washington State ausgerichtet ist, konfiguriert ein neues IoT Edge-Gerät beispielsweise automatisch, sobald es bereitgestellt und der Gerätegruppe für Washington State hinzugefügt wurde. 
 
In diesem Artikel werden die einzelnen Komponenten beschrieben, die beim Konfigurieren und Überwachen einer Bereitstellung verwendet werden. Eine exemplarische Vorgehensweise zum Erstellen und Aktualisieren einer Bereitstellung finden Sie unter [Deploy and monitor IoT Edge modules at scale][lnk-howto] (Bedarfsabhängiges Bereitstellen und Überwachen von IoT Edge-Modulen).

## <a name="deployment"></a>Bereitstellung

Bei einer automatischen IoT Edge-Bereitstellung werden IoT Edge-Modulimages zugewiesen, die als Instanzen auf einem bestimmten Satz von IoT Edge-Geräten ausgeführt werden. Hierfür wird ein IoT Edge-Bereitstellungsmanifest konfiguriert, das eine Liste von Modulen mit den entsprechenden Initialisierungsparametern enthält. Eine Bereitstellung kann einem einzelnen Gerät (basierend auf der Geräte-ID) oder einer Gruppe von Geräten (basierend auf Tags) zugewiesen werden. Nachdem ein IoT Edge-Gerät ein Bereitstellungsmanifest erhalten hat, lädt es die Modul-Containerimages aus den entsprechenden Containerrepositorys herunter, installiert und konfiguriert sie. Sobald eine Bereitstellung erstellt wurde, kann der Operator den Bereitstellungsstatus überwachen und so erkennen, ob die Zielgeräte ordnungsgemäß konfiguriert wurden.   

Geräte müssen als IoT Edge-Geräte bereitgestellt werden, um mit einer Bereitstellung konfiguriert werden zu können. Auf dem Gerät muss Folgendes vorhanden sein, um die Bereitstellung zu ermöglichen:
* Das Basisbetriebssystem
* Ein Containerverwaltungssystem (beispielsweise Moby oder Docker)
* Bereitstellung der IoT Edge-Laufzeit 

### <a name="deployment-manifest"></a>Bereitstellungsmanifest

Ein Bereitstellungsmanifest ist ein JSON-Dokument, in dem die Module beschrieben werden, die auf den IoT Edge-Zielgeräten konfiguriert werden sollen. Es enthält die Konfigurationsmetadaten für alle Module, einschließlich der erforderlichen Systemmodule (insbesondere IoT Edge-Agent und IoT Edge-Hub).  

Die Konfigurationsmetadaten für jedes Modul enthalten Folgendes: 
* Version 
* Typ 
* Status (z.B. „wird ausgeführt“ oder „beendet“) 
* Richtlinien für den Neustart 
* Image- und Containerregistrierung
* Routen für Dateneingabe und -ausgabe 

Wenn das Modulimage in einer privaten Containerregistrierung gespeichert ist, befinden sich die Anmeldeinformationen für die Registrierung im IoT Edge-Agent. 

### <a name="target-condition"></a>Zielbedingung

Die Zielbedingung wird während der Lebensdauer der Bereitstellung kontinuierlich ausgewertet, um jedes neue Gerät einzubeziehen, das die Anforderungen erfüllt, bzw. Geräte zu entfernen, die die Bedingung nicht mehr erfüllen. Die Bereitstellung wird reaktiviert, wenn der Dienst Änderungen an der Zielbedingung feststellt. 

Angenommen, Sie verfügen über eine Bereitstellung A mit der Zielbedingung „tags.environment = 'prod'“. Wenn Sie die Bereitstellung starten, sind zehn Produktionsgeräte vorhanden. Die Module werden erfolgreich auf diesen zehn Geräten installiert. Der Status des IoT Edge-Agents wird mit insgesamt zehn Geräten, zehn erfolgreichen Antworten, null Fehlerantworten und null ausstehenden Antworten angezeigt. Nun fügen Sie fünf weitere Geräte mit „tags.environment = 'prod'“ hinzu. Der Dienst erkennt die Änderung, und der Status des IoT Edge-Agents wird mit insgesamt 15 Geräten, zehn erfolgreichen Antworten, null Fehlerantworten und fünf ausstehenden Antworten angezeigt, wenn der Agent versucht, die fünf neuen Geräte bereitzustellen.

Verwenden Sie eine beliebige boolesche Bedingung auf Tags von Gerätezwillingen oder die Geräte-ID, um die Zielgeräte auszuwählen. Wenn Sie eine Bedingung mit Tags verwenden möchten, müssen Sie auf dem Gerätezwilling einen Abschnitt „"tags":{}“ auf der gleichen Ebene wie die Eigenschaften hinzufügen. [Weitere Informationen über Tags in Gerätezwillingen](../iot-hub/iot-hub-devguide-device-twins.md)

Beispiel für die Zielbedingung:
* deviceId ='linuxprod1'
* tags.environment ='prod'
* tags.environment = 'prod' AND tags.location = 'westus'
* tags.environment = 'prod' OR tags.location = 'westus'
* tags.operator = 'John' AND tags.environment = 'prod' NOT deviceId = 'linuxprod1'

Beim Erstellen einer Zielbedingung gelten die folgenden Einschränkungen:

* Sie können auf einem Gerätezwilling nur eine Zielbedingung mithilfe von Tags oder der Geräte-ID erstellen.
* Doppelte Anführungszeichen sind in keinem Teil der Zielbedingung zulässig. Verwenden Sie einfache Anführungszeichen.
* Einfache Anführungszeichen werden für die Werte der Zielbedingung verwendet. Daher müssen Sie für ein einfaches Anführungszeichen ein weiteres einfaches Anführungszeichen als Escapezeichen verwenden, wenn es Teil des Gerätenamens ist. Beispielsweise müsste die Zielbedingung für „operator'sDevice“ in der Form „deviceId='operator''sDevice'“ geschrieben werden.
* In Werten für Zielbedingungen sind Zahlen, Buchstaben und folgende Zeichen zulässig: `-:.+%_#*?!(),=@;$`.

### <a name="priority"></a>Priorität

Eine Priorität definiert, ob eine Bereitstellung auf einem Zielgerät relativ zu anderen Bereitstellungen angewendet werden soll. Bei der Bereitstellungspriorität handelt es sich um eine positive ganze Zahl, wobei höhere Werte eine höhere Priorität angeben. Wenn ein IoT Edge-Gerät Ziel mehrerer Bereitstellungen ist, wird die Bereitstellung mit der höchsten Priorität angewendet.  Bereitstellungen mit niedrigeren Prioritäten werden nicht angewendet, und Bereitstellungen werden nicht zusammengeführt.  Wenn ein Gerät Ziel von mehreren Bereitstellungen mit gleicher Priorität ist, gilt die zuletzt erstellte Bereitstellung (entsprechend dem Erstellungszeitstempel).

### <a name="labels"></a>Bezeichnungen 

Bei Bezeichnungen handelt es sich um Schlüssel/Wert-Paare als Zeichenfolgen, mit denen Gruppen von Bereitstellungen gefiltert und gruppiert werden können. Eine Bereitstellung kann mehrere Bezeichnungen aufweisen. Bezeichnungen sind optional und haben keine Auswirkungen auf die eigentliche Konfiguration der IoT Edge-Geräte. 

### <a name="deployment-status"></a>Bereitstellungsstatus

Eine Bereitstellung kann überwacht werden, um zu bestimmen, ob sie erfolgreich auf allen IoT Edge-Zielgeräte angewendet wurde.  Ein Edge-Zielgerät wird in einer oder mehreren der folgenden Statuskategorien angezeigt: 
* **Ziel** enthält die IoT Edge-Geräte, die den Zielbedingungen für die Bereitstellung entsprechen.
* **Tatsächlich** gibt die IoT Edge-Zielgeräte an, für die keine andere Bereitstellung mit höherer Priorität vorhanden ist.
* **Fehlerfrei** zeigt die IoT Edge-Geräte, die an den Dienst gemeldet haben, dass die Module erfolgreich bereitgestellt wurden. 
* **Fehlerhaft** zeigt die IoT Edge-Geräte, die an den Dienst gemeldet haben, dass eines oder mehrere Module nicht erfolgreich bereitgestellt wurden. Stellen Sie zum weiteren Untersuchen des Fehlers eine Remoteverbindung mit den Geräten her, und zeigen Sie die Protokolldateien an.
* **Unbekannt** enthält die IoT Edge-Geräte, die keinen Status für diese Bereitstellung gemeldet haben. Zeigen Sie zur weiteren Untersuchung Dienstinformationen und Protokolldateien an.

## <a name="phased-rollout"></a>Schrittweises Rollout 

Bei einem schrittweisen Rollout handelt es sich um ein Verfahren, bei dem ein Operator Änderungen für einen größeren Satz von IoT Edge-Geräten bereitstellt. Hierbei werden Änderungen nach und nach vorgenommen, um das Risiko durch umfangreiche wichtige Änderungen zu reduzieren.  

Ein schrittweises Rollout wird in den folgenden Phasen und Schritten ausgeführt: 
1. Richten Sie eine Testumgebung mit IoT Edge-Geräten durch Bereitstellung und Festlegen eines Gerätezwillingstags wie `tag.environment='test'` ein. Die Testumgebung sollte die Produktionsumgebung spiegeln, auf die die Bereitstellung später abzielt. 
1. Erstellen Sie eine Bereitstellung, einschließlich der gewünschten Module und Konfigurationen. Die Zielbedingung sollte die Testumgebung für IoT Edge-Geräte umfassen.   
1. Überprüfen Sie die neue Modulkonfiguration in der Testumgebung.
1. Aktualisieren Sie die Bereitstellung mit einer Teilmenge der IoT Edge-Produktionsgeräte, indem Sie der Zielbedingung ein neues Tag hinzufügen. Stellen Sie außerdem sicher, dass die Priorität für die Bereitstellung höher als bei anderen Bereitstellungen ist, die zurzeit für diese Geräte verwendet werden. 
1. Stellen Sie sicher, dass die Bereitstellung auf den IoT-Zielgeräten erfolgreich war, indem Sie den Bereitstellungsstatus anzeigen.
1. Aktualisieren Sie die Bereitstellung so, dass sie auf alle verbleibenden IoT Edge-Produktionsgeräte abzielt.

## <a name="rollback"></a>Rollback

Für Bereitstellungen kann im Fall von Fehlern oder Fehlkonfigurationen ein Rollback ausgeführt werden.  Da bei einer Bereitstellung die absolute Modulkonfiguration für ein IoT Edge-Gerät definiert wird, muss eine zusätzliche Bereitstellung auch auf das gleiche Gerät mit niedrigerer Priorität abzielen, auch wenn alle Module entfernt werden sollen.  

Führen Sie die Schritte für Rollbacks in der folgenden Reihenfolge aus: 
1. Vergewissern Sie sich, dass eine zweite Bereitstellung für die gleiche Gruppe von Geräten vorhanden ist. Wenn bei dem Rollback alle Module entfernt werden sollen, sollte die zweite Bereitstellung keine Module enthalten. 
1. Ändern oder entfernen Sie den Ausdruck für die Zielbedingung bei der Bereitstellung, für die ein Rollback ausgeführt werden soll, sodass die Geräte die Zielbedingung nicht mehr erfüllen.
1. Stellen Sie sicher, dass das Rollback erfolgreich war, indem Sie den Bereitstellungsstatus anzeigen.
   * Die Bereitstellung, für die das Rollback ausgeführt wurde, sollte keinen Status für die Geräte mehr anzeigen, auf die sich das Rollback bezogen hat.
   * Die zweite Bereitstellung sollte den Bereitstellungsstatus für die Geräte enthalten, für die das Rollback ausgeführt wurde.


## <a name="next-steps"></a>Nächste Schritte

* Führen Sie die Schritte zum Erstellen, Aktualisieren oder Löschen einer Bereitstellung in [Deploy and monitor IoT Edge modules at scale][lnk-howto] (Bedarfsabhängiges Bereitstellen und Überwachen von IoT Edge-Modulen) aus.
* Lesen Sie weitere Informationen zu anderen IoT Edge-Konzepten wie der [IoT Edge-Laufzeit][lnk-runtime] und [IoT-Edge-Modulen][lnk-modules].

<!-- Links -->
[lnk-lifecycle]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-runtime]: iot-edge-runtime.md
[lnk-modules]: iot-edge-modules.md
[lnk-howto]: how-to-deploy-monitor.md
