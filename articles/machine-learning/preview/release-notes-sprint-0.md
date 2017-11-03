---
title: Anmerkungen zu dieser Version von Azure ML Workbench (Sprint 0, Oktober 2017)
description: "In diesem Dokument werden die Updates für die Version „Sprint 0“ von Azure ML detailliert beschrieben."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 10/12/2017
ms.openlocfilehash: 37e0a4461e8a0de631a6194a1eb8cc16b610954f
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="sprint-0---october-2017"></a>Sprint 0 – Oktober 2017 

**Versionsnummer: 0.1.1710.04013**

Willkommen beim ersten Update für Azure Machine Learning Workbench, das auf unsere erste, auf der Microsoft Ignite 2017-Konferenz vorgestellte Public Preview-Version folgt. Die wichtigsten Änderungen in dieser Version sind Korrekturen für mehr Zuverlässigkeit und Stabilität.  Einige der von uns aufgegriffenen kritischen Punkte sind u.a:

## <a name="new-features"></a>Neue Funktionen
- macOS High Sierra wird nun unterstützt.

## <a name="bug-fixes"></a>Fehlerbehebungen
### <a name="workbench-experience"></a>Workbench-Oberfläche
- Beim Ziehen und Ablegen einer Datei in Workbench stürzt Workbench ab.
- Das Terminalfenster in Visual Studio Code, das als IDE für Workbench konfiguriert ist, erkennt keine Befehle des Typs _az ml_.

### <a name="workbench-authentication"></a>Workbench-Authentifizierung
Wir haben eine Reihe von Änderungen vorgenommen, um verschiedene gemeldete Anmelde- und Authentifizierungsprobleme zu korrigieren.
- Das Authentifizierungsfenster wird ständig eingeblendet, insbesondere wenn die Internetverbindung nicht stabil ist.
- Korrigierte Zuverlässigkeitsprobleme im Zusammenhang mit dem Ablauf von Authentifizierungstoken.
- In einigen Fällen wird das Authentifizierungsfenster zweimal angezeigt.
- Im Workbench-Hauptfenster wird immer noch die Meldung zur laufenden Authentifizierung angezeigt, wenn der Authentifizierungsvorgang tatsächlich abgeschlossen ist und das Popupdialogfeld bereits geschlossen wurde.
- Wenn keine Internetverbindung besteht, wird der Authentifizierungsdialog mit einem leeren Bildschirm angezeigt.

### <a name="data-preparation"></a>Vorbereitung der Daten 
- Wenn ein bestimmter Wert gefiltert wird, werden auch Fehler und fehlende Werte herausgefiltert.
- Wenn Sie eine Stichprobenstrategie ändern, werden nachfolgende vorhandene Join-Vorgänge entfernt.
- Beim Ersetzen der Transformation „Fehlender Wert“ wird NaN nicht berücksichtigt.
- Datumstypableitung bewirkt eine Ausnahme, wenn NULL-Wert gefunden wird.

### <a name="job-execution"></a>Auftragsausführung
- Es gibt keine eindeutige Fehlermeldung, wenn die Auftragsausführung den Projektordner nicht hochladen kann, weil die Größenbeschränkung überschritten wurde.
- Wenn das Python-Skript des Benutzers das Arbeitsverzeichnis ändert, werden die Dateien, die in die Ausgabeordner geschrieben wurden, nicht nachverfolgt. 
- Wenn sich das aktive Azure-Abonnement von dem des aktuellen Projekts unterscheidet, führt die Auftragsübermittlung zum Fehler 403.
- Wenn Docker nicht vorhanden ist, wird keine eindeutige Fehlermeldung zurückgegeben, sobald der Benutzer versucht, Docker als Ausführungsziel zu verwenden.
- Die RUNCONFIG-Datei wird nicht automatisch gespeichert, wenn der Benutzer auf die Schaltfläche _Ausführen_ klickt.

### <a name="jupyter-notebook"></a>Jupyter Notebook
- Notebook-Server kann nicht gestartet werden, wenn der Benutzer diesen mit bestimmten Anmeldungstypen verwendet.
- Notebook-Serverfehlermeldungen werden in den für Benutzer sichtbaren Protokollen nicht angezeigt.

### <a name="azure-portal"></a>Azure-Portal
- Wenn Sie das Design „Dunkel“ des Azure-Portals auswählen, wird das Blatt „Modellverwaltung“ als schwarzes Feld angezeigt.

### <a name="operationalization"></a>Operationalisierung
- Die Wiederverwendung eines Manifests zur Aktualisierung eines Webdiensts führt zu einem neu erstellten Docker-Image mit einem Zufallsnamen.
- Webdienstprotokolle können nicht aus einem Kubernetes-Cluster abgerufen werden.
- Irreführende Fehlermeldung wird ausgegeben, wenn der Benutzer versucht, ein Modellverwaltungskonto oder ein ML-Computekonto zu erstellen. Zudem treten Berechtigungsprobleme auf.
