---
title: Simulieren eines Fehlers bei Zugriff auf georedundanten Speicher mit Lesezugriff in Azure | Microsoft-Dokumentation
description: Simulieren eines Fehlers bei Zugriff auf georedundanten Speicher mit Lesezugriff
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 2919eb0e301000b53f4f63799e9c65aad45ca9f2
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2017
---
# <a name="simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Simulieren eines Fehlers bei Zugriff auf redundanten Speicher mit Lesezugriff

Dieses Tutorial ist der zweite Teil einer Reihe. In diesem Tutorial fügen Sie mit Fiddler eine fehlerhafte Antwort auf Anforderungen an ein RA-GRS-Speicherkonto ([read-access geographically redundant storage, georedundanter Speicher mit Lesezugriff](../common/storage-redundancy.md#read-access-geo-redundant-storage)) ein, um einen Fehler zu simulieren, durch den die Anwendung aus dem sekundären Endpunkt liest.

![Szenario-App](media/storage-simulate-failure-ragrs-account-app/scenario.png)

Im zweiten Teil der Reihe lernen Sie Folgendes:

> [!div class="checklist"]
> * Ausführen und Anhalten einer Anwendung
> * Simulieren eines Fehlers
> * Simulieren der Wiederherstellung des primären Endpunkts

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Laden Sie das Tool [Fiddler](https://www.telerik.com/download/fiddler) herunter, und installieren Sie es.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Zum Abschließen dieses Tutorials müssen Sie das vorherige Speichertutorial [Make your application data highly available with Azure storage (Herstellen von Hochverfügbarkeit für Anwendungsdaten mit Azure Storage)][previous-tutorial] abgeschlossen haben.

## <a name="launch-fiddler"></a>Starten von Fiddler

Öffnen Sie Fiddler, und wählen Sie zuerst **Rules** (Regeln) und anschließend **Customize Rules** (Regeln anpassen) aus.

![Anpassen von Fiddler-Regeln](media/storage-simulate-failure-ragrs-account-app/figure1.png)

Der Fiddler ScriptEditor wird gestartet und zeigt die Datei **SampleRules.js** an. Mit dieser Datei können Sie Fiddler anpassen. Fügen Sie das folgende Codebeispiel in die `OnBeforeResponse`-Funktion ein. Der neue Code ist auskommentiert, damit die damit erstellte Logik nicht unmittelbar implementiert wird. Wählen Sie zuerst **File** (Datei) und anschließend **Save** (Speichern) zum Speichern der Änderungen aus.

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error. 
        // When you're ready to stop sending back errors, comment these lines of script out again 
        //     and save the changes.

        if ((oSession.hostname == "contosoragrs.blob.core.windows.net") 
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;  
        }
    */
```

![Einfügen von benutzerdefinierten Regeln](media/storage-simulate-failure-ragrs-account-app/figure2.png)

## <a name="start-and-pause-the-application"></a>Starten und Anhalten der Anwendung

Drücken Sie in Visual Studio **F5**, oder wählen Sie **Starten** aus, um mit dem Debuggen der Anwendung zu beginnen. Drücken Sie im Konsolenfenster **eine Taste**, um die Anwendung anzuhalten, während diese aus dem primären Endpunkt liest.

## <a name="simulate-failure"></a>Simulieren eines Fehlers

Nach dem Anhalten der Anwendung können Sie die Auskommentierung für die benutzerdefinierte Regel aufheben, die Sie im vorherigen Schritt in Fiddler gespeichert haben. In diesem Codebeispiel wird nach Anforderungen an das RA-GRS-Speicherkonto gesucht. Wenn der Pfad den Namen des Bilds enthält (`HelloWorld`), wird der Antwortcode `503 - Service Unavailable` zurückgegeben.

Navigieren Sie zu Fiddler, und wählen Sie zuerst **Rules**  -> (Regeln) und anschließend **Customize Rules...** (Regeln anpassen...) aus.  Heben Sie die Auskommentierung für die folgenden Zeilen auf, und ersetzen Sie `STORAGEACCOUNTNAME` mit dem Namen Ihres Speicherkontos. Wählen Sie zuerst **File**  -> (Datei) und anschließend **Save** (Speichern) aus, um die Änderungen zu speichern.

```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

Drücken Sie zum Fortsetzen der Anwendung **eine Taste**.

Sobald die Anwendung erneut gestartet wird, treten beim Senden von Anforderungen an den primären Endpunkt Fehler auf. Die Anwendung versucht fünfmal, erneut eine Verbindung mit dem primären Endpunkt herzustellen. Wenn der Verbindungsaufbau mehr als fünfmal scheitert, fordert die Anwendung das Bild vom sekundären Endpunkt an, der schreibgeschützt ist. Nachdem die Anwendung das Bild erfolgreich 20 Mal vom sekundären Endpunkt abgerufen hat, versucht sie, eine Verbindung mit dem primären Endpunkt herzustellen. Wenn dieser weiterhin nicht erreichbar ist, beginnt die Anwendung erneut mit dem Lesen aus dem sekundären Endpunkt. Hierbei handelt es sich um das Muster [Trennschalter](/azure/architecture/patterns/circuit-breaker.md) (Circuit Breaker), das im vorherigen Tutorial beschrieben wurde.

![Einfügen einer benutzerdefinierten Regel](media/storage-simulate-failure-ragrs-account-app/figure3.png)

## <a name="simulate-primary-endpoint-restoration"></a>Simulieren der Wiederherstellung des primären Endpunkts

Durch die benutzerdefinierte Regel, die Sie im vorherigen Schritt in Fiddler festgelegt haben, treten beim Senden von Anforderungen an den primären Endpunkt Fehler auf. Um den primären Endpunkt in der Simulation wiederherzustellen, entfernen Sie die Logik, mit der der `503`-Fehler eingefügt wurde.

Drücken Sie **eine Taste**, um die Anwendung anzuhalten.

### <a name="remove-the-custom-rule"></a>Entfernen der benutzerdefinierten Regel

Navigieren Sie zu Fiddler, und wählen Sie zuerst **Rules** (Regeln) und anschließend **Customize Rules...** (Regeln anpassen...) aus.  Kommentieren Sie in der `OnBeforeResponse`-Funktion die benutzerdefinierte Logik aus, oder entfernen Sie diese, sodass die Standardfunktion wieder vorhanden ist. Wählen Sie **File** (Datei) und anschließend **Save** (Speichern) aus, um die Änderungen zu speichern.

![Entfernen der benutzerdefinierten Regel](media/storage-simulate-failure-ragrs-account-app/figure5.png)

Drücken Sie anschließend zum Fortsetzen der Anwendung **eine Taste**. Die Anwendung liest nun erneut aus dem primären Endpunkt, bis 999 Lesevorgänge erfasst wurden.

![Fortsetzen der Anwendung](media/storage-simulate-failure-ragrs-account-app/figure4.png)

## <a name="next-steps"></a>Nächste Schritte

Im zweiten Teil dieser Reihe haben Sie gelernt, wie Sie einen Fehler simulieren, um den georedundanten Speicher mit Lesezugriff zu testen. Folgende Schritte wurden dabei erläutert:

> [!div class="checklist"]
> * Ausführen und Anhalten einer Anwendung
> * Simulieren eines Fehlers
> * Simulieren der Wiederherstellung des primären Endpunkts

Unter diesem Link finden Sie vordefinierte Speicherbeispiele.

> [!div class="nextstepaction"]
> [Beispielskripts für Azure Storage](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md