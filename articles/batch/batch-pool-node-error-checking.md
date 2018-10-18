---
title: Suchen nach Batch-Pool- und Knotenfehlern
description: Suchen nach Fehlern und Vermeiden dieser Fehler bei der Erstellung von Pools und Knoten
services: batch
author: mscurrell
ms.author: markscu
ms.date: 9/25/2018
ms.topic: conceptual
ms.openlocfilehash: bc09089fa9984e9042166938da19499afca21509
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435834"
---
# <a name="checking-for-pool-and-node-errors"></a>Suchen nach Pool- und Knotenfehlern

Beim Erstellen und Verwalten von Batch-Pools gibt es Vorgänge, die unmittelbar ausgeführt werden, und asynchrone Vorgänge, die nicht unmittelbar ausgeführt werden. Ihre Ausführung erfolgt im Hintergrund und kann einige Minuten in Anspruch nehmen.

Das Erkennen von Fehlern bei unmittelbar ausgeführten Vorgängen, ist einfach, da sämtliche Fehler direkt über die API, die CLI oder die UI zurückgegeben werden.

Dieser Artikel befasst sich mit den Hintergrundprozessen, die für Pools und Poolknoten ausgeführt werden können. Darin wird erläutert, wie Fehler erkannt und vermieden werden können.

## <a name="pool-errors"></a>Poolfehler

### <a name="resize-timeout-or-failure"></a>Timeout oder Fehler bei Größenänderung

Bei der Erstellung eines neuen Pools oder der Änderung der Größe eines vorhandenen Pools wird die geplante Anzahl der Knoten angegeben.  Der Vorgang wird unmittelbar abgeschlossen, die tatsächliche Zuordnung der neuen Knoten bzw. das Entfernen vorhandener Knoten erfolgt jedoch im Hintergrund und kann einige Minuten in Anspruch nehmen.  In der API für die [Erstellung](https://docs.microsoft.com/rest/api/batchservice/pool/add) oder [Größenänderung](https://docs.microsoft.com/rest/api/batchservice/pool/resize) wird ein Timeout bei Größenänderung angegeben, wenn die geplante Anzahl der Knoten innerhalb des Zeitlimits für die Größenänderung nicht abgerufen werden kann. In diesem Fall wird der Vorgang mit einem Pool im stabilen Zustand und Fehlern bei der Größenänderung beendet.

Ein Timeout bei einer Größenänderung wird bei der letzten Auswertung von der Eigenschaft [ResizeError](https://docs.microsoft.com/rest/api/batchservice/pool/get#resizeerror) gemeldet, in der mindestens ein aufgetretener Fehler aufgeführt wird.

Zu den häufigen Ursachen für Timeouts bei Größenänderungen zählen die folgenden:
- Der Timeout für die Größenänderung ist zu kurz.
  - Der Standardtimeout beträgt 15 Minuten; normalerweise reicht diese Zeit für das Zuordnen oder Entfernen von Knoten aus.
  - Bei der Zuordnung einer Vielzahl von Knoten (über 1.000 Knoten über ein Marketplace-Image oder über 300 Knoten über ein benutzerdefiniertes Image), während der Erstellung oder Größenänderung eines Pools; in diesem Fall wird ein Timeout von 30 Minuten empfohlen.
- Kernkontingent nicht ausreichend
  - Ein Batch-Konto weist ein Kontingent für die Anzahl von Kernen auf, die poolübergreifend zugeordnet werden.  Sobald dieses Kontingent erschöpft ist, werden keine weiteren Knoten zugeordnet.  Das Kernkontingent [kann erhöht werden](https://docs.microsoft.com/azure/batch/batch-quota-limit), damit weitere Knoten zugeordnet werden können.
- Nicht genügend Subnetz-IP-Adressen, wenn sich ein [Pool in einem virtuellen Netzwerk](https://docs.microsoft.com/azure/batch/batch-virtual-network) befindet
  - Das Subnetz eines virtuellen Netzwerks muss über ausreichend nicht zugewiesene IP-Adressen verfügen, die allen angeforderten Poolknoten zugeordnet werden sollen. Andernfalls können die Knoten nicht erstellt werden.
- Nicht genügend Ressourcen, wenn sich ein [Pool in einem virtuellen Netzwerk](https://docs.microsoft.com/azure/batch/batch-virtual-network) befindet
  - Ressourcen, wie z.B. Lastenausgleichsmodule, öffentliche IP-Adressen und NSGs werden in dem Abonnement erstellt, mit dem das Batch-Konto erstellt wird.  Die Abonnementkontingente für diese Ressourcen müssen ausreichend sein.
- Verwenden eines benutzerdefinierten VM-Image für große Pools
  - Die Zuordnung großer Pools, die benutzerdefinierte Images verwenden, kann länger dauern und zu Timeouts bei Größenänderungen führen.  In einem [bestimmten Artikel](https://docs.microsoft.com/azure/batch/batch-custom-images) finden Sie Empfehlungen zu Einschränkungen und Konfigurationen. 

### <a name="auto-scale-failures"></a>Fehler bei der automatischen Skalierung

Anstatt die geplante Anzahl der Knoten für einen Pool bei der Erstellung oder Größenänderung eines Pools explizit festzulegen, kann die Anzahl der Knoten in einem Pool automatisch skaliert werden.  Für einen Pool kann eine [Formel für die automatische Skalierung](https://docs.microsoft.com/azure/batch/batch-automatic-scaling) erstellt werden, die in regelmäßigen konfigurierbaren Zeitabständen ausgewertet wird, um die geplante Anzahl der Knoten für den Pool festzulegen.  Die folgenden Arten von Problemen können auftreten und erkannt werden:

- Die Auswertung der automatischen Skalierung kann fehlschlagen.
- Die resultierende Größenänderung kann fehlschlagen und zu einem Timeout führen.
- Möglicherweise gibt es mit der Formel für die automatische Skalierung ein Problem, das zu falschen Werten für die geplanten Knoten führt. Dabei kann die Größenänderung funktionieren oder zu einem Timeout führen.

Mit der Eigenschaft [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/pool/get#autoscalerun) werden Informationen zu der letzten Auswertung der automatischen Skalierung abgerufen. Diese beinhalten den Zeitpunkt der Auswertung, die Werte und das Ergebnis der Auswertung sowie Fehler während der Durchführung der Auswertung.

Ein [Ereignis zum Abschluss der Größenänderung von Pools](https://docs.microsoft.com/azure/batch/batch-pool-resize-complete-event) erfasst automatisch Informationen zu sämtlichen Auswertungen.

### <a name="delete"></a>Löschen

Angenommen, ein Pool enthält Knoten, die anschließend durch einen Löschvorgang in dem Pool gelöscht werden, gefolgt von dem Poolobjekt selbst.  Es kann einige Minuten dauern, bis die Poolknoten gelöscht wurden.

Der [Poolstatus](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate) wird während des Löschvorgangs auf „Wird gelöscht“ festgelegt.  Die aufrufende Anwendung kann mithilfe der Eigenschaften „state“ und „stateTransitionTime“ erkennen, ob der Löschvorgang im Pool zu lange dauert.

## <a name="pool-compute-node-errors"></a>Fehler in Pool-Computeknoten

Knoten können in einem Pool erfolgreich zugeordnet werden. Einige Probleme können jedoch dazu führen, dass die Knoten fehlerhaft sind und nicht verwendet werden können.  Sobald Knoten in einem Pool zugeordnet wurden, werden entsprechend Gebühren berechnet. Daher ist das Erkennen von Problemen wichtig, um zu verhindern, dass für nicht verwendbare Knoten gezahlt wird.

### <a name="start-task-failure"></a>Fehler bei Starttask

Für einen Pool kann ein optionaler [Starttask](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask) angegeben werden.  Wie bei jedem Task können eine Befehlszeile und aus dem Speicher herunterzuladende Ressourcendateien angegeben werden.  Der Starttask wird für den Pool angegeben, jedoch auf jedem Knoten ausgeführt. Sobald alle Knoten gestartet wurden, wird der Starttask ausgeführt.  Eine weitere Eigenschaft des [Starttasks](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask), „waitForSuccess“ gibt an, ob Batch vor der Planung von Tasks für einen Knoten warten sollte, bis der Starttask erfolgreich abgeschlossen wurde.

Wenn ein Starttask fehlschlägt und bei der Konfiguration des Starttask angegeben wurde, dass auf den erfolgreichen Abschluss gewartet werden soll, dann ist der Knoten nicht verwendbar, und es fallen weiterhin Gebühren an.

Starttaskfehler können mithilfe der Eigenschaften [result](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskexecutionresult) und [failureInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskfailureinformation) der Knoteneigenschaft [startTaskInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#starttaskinformation) der obersten Ebene erkannt werden.

Ein fehlgeschlagener Starttask führt auch dazu, dass der Knoten [state](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) auf „starttaskfailed“ festgelegt wird, jedoch nur, wenn „waitForSuccess“ auf TRUE festgelegt wurde.

Wie bei jedem anderen Task kann es für das Fehlschlagen eines Starttasks viele Ursachen geben.  Zur Fehlerbehebung sollten „stdout“, „stderr“ und weitere taskspezifische Protokolldateien überprüft werden.

### <a name="application-package-download-failure"></a>Fehler beim Herunterladen des Anwendungspakets

Optional kann mindestens ein Anwendungspaket für einen Pool angegeben werden. Die angegebenen Paketdateien werden dann auf den einzelnen Knoten heruntergeladen und nach dem Starten der Knoten, aber vor der Planung der Tasks, dekomprimiert.  Normalerweise wird eine Befehlszeile für Starttasks in Verbindung mit Anwendungspaketen verwendet, um z.B. Dateien an einen anderen Speicherort zu kopieren oder ein Setup auszuführen.

Ein Fehler beim Herunterladen und Dekomprimieren eines Anwendungspakets wird von der Knoteneigenschaft [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) gemeldet.  Der Knotenstatus wird auf „Nicht verwendbar“ festgelegt.

### <a name="node-in-unusable-state"></a>Knoten mit dem Status „Nicht verwendbar“

Es kann verschiedene Ursachen dafür geben, dass der [Knotenstatus](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) auf „Nicht verwendbar“ festgelegt wird.  Wenn „Nicht verwendbar“ festgelegt ist, können für den Knoten zwar keine Tasks geplant werden, es fallen jedoch weiterhin Gebühren für den Knoten an.

Batch versucht immer, nicht verwendbare Knoten wiederherzustellen. Abhängig von der Ursache ist eine Wiederherstellung aber unter Umständen nicht möglich.

Wenn die Ursache ermittelt werden kann, wird diese von der Knoteneigenschaft [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) gemeldet.

Einige weitere Beispiele für Ursachen für „nicht verwendbare“ Knoten:

- Ungültiges benutzerdefiniertes Image; keine richtige Vorbereitung.
- Infrastrukturausfall oder Low-Level-Upgrade, der bzw. das zu einer Verschiebung des zugrunde liegenden virtuellen Computers führt; Batch stellt den Knoten wieder her.

### <a name="node-agent-log-files"></a>Protokolldateien des Knoten-Agents

Sollten Sie sich bei einem Problem mit Poolknoten an den Support wenden, werden möglicherweise Protokolldateien aus dem Batch-Agent-Prozess abgerufen, der auf den einzelnen Poolknoten ausgeführt wird.  Die Protokolldateien für einen Knoten können über das Azure-Portal, Batch Explorer oder eine [API](https://docs.microsoft.com/rest/api/batchservice/computenode/uploadbatchservicelogs) hochgeladen werden.  Das Hochladen und Speichern der Protokolldateien kann besonders nützlich sein, da der Knoten bzw. Pool gelöscht werden kann und auf diese Weise die Kosten für den ausgeführten Knoten gespart werden.

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie sicher, dass Ihre Anwendung eine umfassende Fehlerüberprüfung implementiert hat, insbesondere für asynchrone Vorgänge, damit Probleme schnell erkannt und diagnostiziert werden können.
