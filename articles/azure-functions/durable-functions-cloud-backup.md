---
title: "Auffächerungsszenarios (nach innen und außen) in Durable Functions: Azure"
description: "In diesem Artikel wird beschrieben, wie ein Auffächerungsszenario (nach innen und außen) in der Erweiterung Durable Functions für Azure Functions implementiert wird."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: aa7c0738120ecda8d43669725748585e1ad5a581
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="fan-outfan-in-scenario-in-durable-functions---cloud-backup-example"></a>Auffächerungsszenario (nach innen und außen) in Durable Functions – Beispiel der Cloudsicherung

*Auffächern/Auffächern nach innen* bezieht sich auf das Muster, bei dem mehrere Funktionen gleichzeitig ausgeführt werden und die Ergebnisse im Anschluss zusammengefasst werden. In diesem Artikel wird ein Beispiel beschrieben, in dem [Durable Functions](durable-functions-overview.md) zum Implementieren eines Auffächerungsszenarios (nach innen und außen) verwendet wird. Das Beispiel stellt eine langlebige Funktion dar, die alle oder einige der Websiteinhalte in Azure Storage sichert.

## <a name="prerequisites"></a>Voraussetzungen

* Befolgen Sie die Anweisungen unter [Install Durable Functions](durable-functions-install.md) (Installieren von Durable Functions), um das Beispiel einzurichten.
* In diesem Artikel wird vorausgesetzt, dass Sie das [Hello Sequence](durable-functions-sequence.md)-Beispiel bereits Schritt für Schritt durchgegangen sind.

## <a name="scenario-overview"></a>Übersicht über das Szenario

In diesem Beispiel laden die Funktionen alle Dateien unter einem angegebenen Verzeichnis rekursiv in den Blobspeicher hoch. Zudem zählen sie die Gesamtzahl der hochgeladenen Bytes.

Es ist möglich, eine einzelne Funktion zu schreiben, die alles übernimmt. Das Hauptproblem, mit dem Sie sich auseinandersetzen müssten, ist die **Skalierbarkeit**. Eine einzelne Funktion kann nur auf einem einzelnen virtuellen Computer ausgeführt werden. Der Durchsatz wird folglich durch den Durchsatz dieses virtuellen Computers beschränkt. Ein weiteres Problem stellt die **Zuverlässigkeit** dar. Wenn während des Vorgangs ein Fehler auftritt oder der gesamte Prozess mehr als 5 Minuten in Anspruch nimmt, könnte die Sicherung in einem teilweise abgeschlossenen Zustand fehlschlagen. Sie müsste anschließend neu gestartet werden.

Ein stabilerer Ansatz bestünde darin, zwei reguläre Funktionen zu schreiben: eine zum Aufzählen der Dateien und zum Hinzufügen der Dateinamen zu einer Warteschlange, und die andere zum Lesen aus der Warteschlange und zum Hochladen der Dateien in den Blobspeicher. Im Hinblick auf den Durchsatz und die Zuverlässigkeit ist dies besser, Sie müssen hierfür jedoch eine Warteschlange bereitstellen und verwalten. Im Hinblick auf die **Zustandsverwaltung** und die **Koordinierung** bringt dies vor allem eine hohe Komplexität mit sich, falls Sie weitere Funktionen ausführen möchten, wie z.B. die Gesamtzahl der hochgeladenen Bytes melden.

Ein Durable Functions-Ansatz bietet Ihnen alle genannten Vorteile mit sehr geringem Mehraufwand.

## <a name="the-functions"></a>Die Funktionen

In diesem Artikel werden die folgenden Funktionen in der Beispiel-App beschrieben:

* `E2_BackupSiteContent`
* `E2_GetFileList`
* `E2_CopyFileToBlob`

In den folgenden Abschnitten werden die Konfiguration und der Code beschrieben, die für die Entwicklung im Azure-Portal verwendet werden. Der Code für die Visual Studio-Entwicklung ist am Ende des Artikels angegeben.

## <a name="the-cloud-backup-orchestration"></a>Die Orchestrierung der Cloudsicherung

Die Funktion `E2_BackupSiteContent` verwendet die Standarddatei *function.json* für Orchestratorfunktionen.

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/function.json)]

Im Folgenden wird der Code dargestellt, der die Orchestratorfunktion implementiert:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/run.csx)]

Diese Orchestratorfunktion führt im Wesentlichen Folgendes aus:

1. Sie akzeptiert einen Wert des Typs `rootDirectory` als Eingabeparameter.
2. Sie ruft eine Funktion auf, um unter `rootDirectory` eine rekursive Liste mit Dateien abzurufen.
3. Sie führt mehrere parallele Funktionsaufrufe durch, um die einzelnen Dateien in Azure Blob Storage hochzuladen.
4. Sie wartet, bis alle Uploads abgeschlossen wurden.
5. Sie gibt die Gesamtzahl der Bytes zurück, die in Azure Blob Storage hochgeladen wurden.

Beachten Sie die Zeile `await Task.WhenAll(tasks);`. Alle Aufrufe der Funktion `E2_CopyFileToBlob` wurden *nicht* erwartet. Dies ist beabsichtigt, damit sie parallel ausgeführt werden können. Wenn dieses Array von Aufgaben an `Task.WhenAll` übergeben wird, wird eine Aufgabe zurückgegeben, die erst *nach Abschluss aller Kopiervorgänge* abgeschlossen wird. Wenn Sie mit der Task Parallel Library (TPL) in .NET vertraut sind, ist dies nicht neu für Sie. Der Unterschied besteht darin, dass diese Aufgaben auf mehreren virtuellen Computern gleichzeitig ausgeführt werden könnten. Die Erweiterung stellt dabei sicher, dass die End-to-End-Ausführung robust ist für die Prozesswiederverwendung.

Nach dem Warten von `Task.WhenAll` aus wissen wir, dass alle Funktionsaufrufe abgeschlossen wurden und Werte an uns zurückgegeben haben. Jeder Aufruf von `E2_CopyFileToBlob` gibt die Anzahl von hochgeladenen Bytes zurück. Die Summe der Gesamtbytezahl wird folglich berechnet, indem alle diese Rückgabewerte addiert werden.

## <a name="helper-activity-functions"></a>Hilfsaktivitätsfunktionen

Bei den Hilfsaktivitätsfunktionen handelt es sich, wie bei anderen Beispielen, um normale Funktionen, die die Triggerbindung `activityTrigger` verwenden. Die Datei *function.json* für `E2_GetFileList` sieht wie folgt aus:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/function.json)]

Und hier ist die Implementierung:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/run.csx)]

> [!NOTE]
> Sie wundern sich vielleicht, weshalb Sie diesen Code nicht einfach direkt in die Orchestratorfunktion einfügen konnten. Das können Sie, aber damit würden Sie eine der Grundregeln der Orchestratorfunktionen brechen. Diese besagt, dass die Funktionen niemals eine Eingabe/Ausgabe vornehmen dürfen, den lokalen Dateisystemzugriff inbegriffen.

Die Datei *function.json* für `E2_CopyFileToBlob` ist ebenso einfach:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/function.json)]

Die Implementierung ist auch ziemlich unkompliziert. Es werden einige erweiterte Features von Azure Functions-Bindungen verwendet (d.h. die Verwendung des Parameters `Binder`). Sie müssen sich zum Zwecke dieser ausführlichen Anleitung jedoch nicht um diese Details kümmern.

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/run.csx)]

Bei der Implementierung wird die Datei vom Datenträger geladen. Gleichzeitig werden die Inhalte in ein Blob mit dem gleichen Namen gestreamt. Der Rückgabewert ist die Anzahl der Bytes, die in den Speicher kopiert und anschließend von der Orchestratorfunktion für die Berechnung der Summe verwendet werden.

> [!NOTE]
> Dies ist ein gutes Beispiel für das Verschieben von Eingabe-/Ausgabevorgängen in die Funktion `activityTrigger`. Die Arbeit kann nicht nur auf viele verschiedene virtuelle Computer verteilt werden, sondern Sie können auch den Fortschritt überprüfen. Wenn der Hostprozess aus irgendeinem Grund beendet wird, wissen Sie, welche Uploads bereits abgeschlossen wurden.

## <a name="running-the-sample"></a>Ausführen des Beispiels

Wenn Sie die über HTTP ausgelösten Funktionen verwenden, die im Beispiel enthalten sind, können Sie mit der Orchestrierung beginnen, indem Sie folgende HTTP POST-Anforderung verwenden.

```
POST http://{host}/orchestrators/E2_BackupSiteContent HTTP/1.1
Content-Type: application/json
Content-Length: 20

"D:\\home\\LogFiles"
```

> [!NOTE]
> Die Funktion `HttpStart`, die Sie aufrufen, kann nur mit JSON-formatiertem Inhalt verwendet werden. Aus diesem Grund ist der Header `Content-Type: application/json` erforderlich und der Verzeichnispfad als JSON-Zeichenfolge codiert.

Dies löst den Orchestrator `E2_BackupSiteContent` aus und übergibt die Zeichenfolge `D:\home\LogFiles` als Parameter. Die Antwort enthält einen Link, über den der Status dieses Sicherungsvorgangs abgerufen werden kann:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Je nachdem, wie viele Protokolldateien in Ihrer Funktions-App enthalten sind, könnte dieser Vorgang einige Minuten in Anspruch nehmen. Sie können den aktuellen Status abrufen, indem Sie die URL im Header `Location` der vorherigen HTTP 202-Antwort abfragen.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

```
HTTP/1.1 202 Accepted
Content-Length: 148
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":"D:\\home\\LogFiles","output":null,"createdTime":"2017-06-29T18:50:55Z","lastUpdatedTime":"2017-06-29T18:51:16Z"}
```

In diesem Fall wird die Funktion weiterhin ausgeführt. Sie können die Eingabe, die in den Orchestratorzustand gespeichert wurde, sowie die Zeit der letzten Aktualisierung anzeigen. Sie können weiterhin die `Location`-Headerwerte zum Abfragen der Beendigung verwenden. Wenn der Status „Abgeschlossen“ lautet, wird Ihnen ein HTTP-Antwortwert wie der folgende angezeigt:

```
HTTP/1.1 200 OK
Content-Length: 152
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"D:\\home\\LogFiles","output":452071,"createdTime":"2017-06-29T18:50:55Z","lastUpdatedTime":"2017-06-29T18:51:26Z"}
```

Jetzt können Sie sehen, dass die Orchestrierung abgeschlossen ist und wie viel Zeit für den Abschluss benötigt wurde. Zudem wird Ihnen ein Wert für das Feld `output` angezeigt, der angibt, dass etwa 450 KB Protokolle hochgeladen wurden.

## <a name="visual-studio-sample-code"></a>Visual Studio-Beispielcode

So sieht die Orchestrierung als einzelne C#-Datei in einem Visual Studio-Projekt aus:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs)]

## <a name="next-steps"></a>Nächste Schritte

Jetzt sollten Sie über ein besseres Verständnis der wesentlichen Orchestrierungsfunktionen von Durable Functions verfügen. Die nachfolgenden Beispiele beinhalten erweiterte Features und Szenarios.

> [!div class="nextstepaction"]
> [Ausführen des zustandsbehafteten Singleton-Beispiels](durable-functions-counter.md)


