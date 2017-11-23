---
title: "Bereitstellen von Modulen für Azure IoT Edge | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Module auf Edge-Geräten bereitgestellt werden."
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: d8688ab2daefd400e9c0948853459dd238fa0d43
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="understand-iot-edge-deployments-for-single-devices-or-at-scale---preview"></a>Grundlegendes zu IoT Edge-Bereitstellungen für einzelne Geräte oder bedarfsabhängig (Vorschau)

Für Azure IoT Edge-Geräte gilt ein [Gerätelebenszyklus][lnk-lifecycle], der weitgehend dem von anderen Typen von IoT-Geräten entspricht:

1. IoT Edge-Geräte werden bereitgestellt. Hierzu gehören die Imageerstellung auf einem Gerät mit einem Betriebssystem und das Installieren der [IoT Edge-Laufzeit][lnk-runtime].
1. Die Geräte werden zum Ausführen von [IoT Edge-Modulen][lnk-modules] konfiguriert, und ihre Integrität wird überwacht. 
1. Schließlich können Geräte außer Kraft gesetzt werden, wenn sie ersetzt werden oder veraltet sind.  

Azure IoT Edge bietet zwei Möglichkeiten zum Konfigurieren der Module, die auf IoT Edge-Geräten ausgeführt werden: eine für die Entwicklung und schnelle Iterationen auf einem einzelnen Gerät (die Sie in den Azure IoT Edge-Lernprogrammen verwendet haben) und eine zum Verwalten großer Bestände von IoT Edge-Geräten. Beide Ansätze sind im Azure-Portal und programmgesteuert verfügbar.

Der Schwerpunkt dieses Artikels liegt auf der Konfigurations- und Überwachungsphase für größere Gerätebestände, die zusammen als IoT Edge-Bereitstellungen bezeichnet werden. Die allgemeinen Schritte zur Bereitstellung lauten wie folgt:   

1. Ein Operator definiert eine Bereitstellung, in der ein Satz von Modulen sowie die Zielgeräte beschrieben werden. Jede Bereitstellung verfügt über ein Bereitstellungsmanifest, das diese Informationen enthält. 
1. Der IoT Hub-Dienst kommuniziert mit allen Zielgeräten, um sie mit den gewünschten Modulen zu konfigurieren. 
1. Der IoT Hub-Dienst ruft den Status von den IoT Edge-Geräten ab und gibt diese für den Operator zur Überwachung aus.  Beispielsweise kann ein Operator sehen, wenn ein Edge-Gerät nicht erfolgreich konfiguriert wurde oder wenn bei einem Modul während der Laufzeit ein Fehler auftritt. 
1. Zu jedem Zeitpunkt werden neue IoT Edge-Geräte, die die Zielbedingungen erfüllen, für die Bereitstellung konfiguriert. Eine Bereitstellung, die auf alle IoT Edge-Geräte in Washington State ausgerichtet ist, konfiguriert ein neues IoT Edge-Gerät beispielsweise automatisch, sobald es bereitgestellt und der Gerätegruppe für Washington State hinzugefügt wurde. 
 
In diesem Artikel werden die einzelnen Komponenten erläutert, die beim Konfigurieren und Überwachen einer Bereitstellung verwendet werden. Eine exemplarische Vorgehensweise zum Erstellen und Aktualisieren einer Bereitstellung finden Sie unter [Deploy and monitor IoT Edge modules at scale][lnk-howto] (Bedarfsabhängiges Bereitstellen und Überwachen von IoT Edge-Modulen).

## <a name="deployment"></a>Bereitstellung

Bei der Bereitstellung werden IoT Edge-Modulimages zugewiesen, die als Instanzen auf einem bestimmten Satz von IoT Edge-Geräten ausgeführt werden. Hierfür wird ein IoT Edge-Bereitstellungsmanifest konfiguriert, das eine Liste von Modulen mit den entsprechenden Initialisierungsparametern enthält. Eine Bereitstellung kann einem einzelnen Gerät (in der Regel basierend auf der Geräte-ID) oder einer Gruppe von Geräten (basierend auf Tags) zugewiesen werden. Nachdem ein IoT Edge-Gerät ein Bereitstellungsmanifest erhalten hat, lädt es die Modul-Containerimages aus den entsprechenden Containerrepositorys herunter, installiert und konfiguriert sie. Sobald eine Bereitstellung erstellt wurde, kann der Operator den Bereitstellungsstatus überwachen und so erkennen, ob die Zielgeräte ordnungsgemäß konfiguriert wurden.   

Geräte müssen als IoT Edge-Geräte bereitgestellt werden, um mit einer Bereitstellung konfiguriert werden zu können. Folgende Komponenten werden vorausgesetzt und sind nicht in der Bereitstellung enthalten:
* Das Basisbetriebssystem
* Docker 
* Bereitstellung der IoT Edge-Laufzeit 

### <a name="deployment-manifest"></a>Bereitstellungsmanifest

Ein Bereitstellungsmanifest ist ein JSON-Dokument, in dem die Module beschrieben werden, die auf den IoT Edge-Zielgeräten konfiguriert werden sollen. Es enthält die Konfigurationsmetadaten für alle Module, einschließlich der erforderlichen Systemmodule (insbesondere IoT Edge-Agent und IoT Edge-Hub).  

Die Konfigurationsmetadaten für jedes Modul enthalten Folgendes: 
* Version 
* Typ 
* Status (z.B. „wird ausgeführt“ oder „beendet“) 
* Richtlinien für den Neustart 
* Image- und Containerrepository 
* Routen für Dateneingabe und -ausgabe 

### <a name="target-condition"></a>Zielbedingung

Die Zielbedingungen geben an, ob ein IoT Edge-Gerät im Umfang einer Bereitstellung enthalten sein soll. Zielbedingungen basieren auf Gerätezwillingstags. 

### <a name="priority"></a>Priority

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