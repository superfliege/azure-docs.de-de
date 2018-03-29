---
title: Simulieren eines Fehlers bei Zugriff auf georedundanten Speicher mit Lesezugriff in Azure | Microsoft-Dokumentation
description: Simulieren eines Fehlers bei Zugriff auf georedundanten Speicher mit Lesezugriff
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: ''
ms.topic: tutorial
ms.date: 12/23/2017
ms.author: tamram
ms.openlocfilehash: 57296d828156184aa36532cd649fbec0c81b5e27
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Simulieren eines Fehlers bei Zugriff auf redundanten Speicher mit Lesezugriff

Dieses Tutorial ist der zweite Teil einer Reihe.  In diesem Tutorial können Sie entweder [Fiddler](#simulate-a-failure-with-fiddler) oder [Statisches Routing](#simulate-a-failure-with-an-invalid-static-route) verwenden, um für Anforderungen an den primären Endpunkt Ihres Speicherkontos vom Typ [Georedundanter Speicher mit Lesezugriff](../common/storage-redundancy.md#read-access-geo-redundant-storage) (RA-GRS) Fehler zu simulieren und zu erreichen, dass die Anwendung vom sekundären Endpunkt liest.

![Szenario-App](media/storage-simulate-failure-ragrs-account-app/scenario.png)

Zum Abschließen dieses Tutorials müssen Sie das vorherige Speichertutorial [Make your application data highly available with Azure storage (Herstellen von Hochverfügbarkeit für Anwendungsdaten mit Azure Storage)][previous-tutorial] abgeschlossen haben.

Im zweiten Teil der Reihe lernen Sie Folgendes:

> [!div class="checklist"]
> * Ausführen und Anhalten einer Anwendung
> * Simulieren eines Fehlers mit [Fiddler](#simulate-a-failure-with-fiddler) oder [einer ungültigen statischen Route](#simulate-a-failure-with-an-invalid-static-route) 
> * Simulieren der Wiederherstellung des primären Endpunkts


## <a name="prerequisites"></a>Voraussetzungen

Gehen Sie wie folgt vor, um mit Fiddler einen Fehler zu simulieren: 

* Laden Sie das Tool [Fiddler](https://www.telerik.com/download/fiddler) herunter, und installieren Sie es.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="simulate-a-failure-with-fiddler"></a>Simulieren eines Fehlers mit Fiddler

Zum Simulieren eines Fehlers mit Fiddler injizieren Sie eine fehlgeschlagene Antwort für Anforderungen an den primären Endpunkt Ihres RA-GRS-Speicherkontos.

Führen Sie die folgenden Schritte aus, um einen Fehler zu simulieren und die Wiederherstellung des primären Endpunkts mit Fiddler durchzuführen.

### <a name="launch-fiddler"></a>Starten von Fiddler

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

### <a name="start-and-pause-the-application"></a>Starten und Anhalten der Anwendung

Führen Sie die Anwendung in Ihrer IDE oder im Text-Editor aus. Drücken Sie im Konsolenfenster **eine Taste**, um die Anwendung anzuhalten, während diese aus dem primären Endpunkt liest.

### <a name="simulate-failure"></a>Simulieren eines Fehlers

Nach dem Anhalten der Anwendung können Sie die Auskommentierung für die benutzerdefinierte Regel aufheben, die Sie in einem vorherigen Schritt in Fiddler gespeichert haben. Im Codebeispiel wird nach Anforderungen an das RA-GRS-Speicherkonto gesucht. Wenn der Pfad den Namen des Bilds enthält (`HelloWorld`), wird der Antwortcode `503 - Service Unavailable` zurückgegeben.

Navigieren Sie zu Fiddler, und wählen Sie zuerst **Rules**  -> (Regeln) und anschließend **Customize Rules...** (Regeln anpassen...) aus.  Heben Sie die Auskommentierung für die folgenden Zeilen auf, und ersetzen Sie `STORAGEACCOUNTNAME` mit dem Namen Ihres Speicherkontos. Wählen Sie zuerst **File**  -> (Datei) und anschließend **Save** (Speichern) aus, um die Änderungen zu speichern. 

> [!NOTE]
> Wenn Sie die Beispielanwendung unter Linux ausführen, müssen Sie Fiddler jeweils neu starten, wenn Sie die Datei **CustomRule.js** bearbeiten, damit die benutzerdefinierte Logik für Fiddler installiert wird. 
> 
> 


```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

Drücken Sie zum Fortsetzen der Anwendung eine **beliebige Taste**.

Sobald die Anwendung erneut gestartet wird, treten beim Senden von Anforderungen an den primären Endpunkt Fehler auf. Die Anwendung versucht fünfmal, erneut eine Verbindung mit dem primären Endpunkt herzustellen. Wenn der Verbindungsaufbau mehr als fünfmal scheitert, fordert die Anwendung das Bild vom sekundären Endpunkt an, der schreibgeschützt ist. Nachdem die Anwendung das Bild erfolgreich 20 Mal vom sekundären Endpunkt abgerufen hat, versucht sie, eine Verbindung mit dem primären Endpunkt herzustellen. Wenn dieser weiterhin nicht erreichbar ist, beginnt die Anwendung erneut mit dem Lesen aus dem sekundären Endpunkt. Hierbei handelt es sich um das Muster [Trennschalter](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker) (Circuit Breaker), das im vorherigen Tutorial beschrieben wurde.

![Einfügen einer benutzerdefinierten Regel](media/storage-simulate-failure-ragrs-account-app/figure3.png)

### <a name="simulate-primary-endpoint-restoration"></a>Simulieren der Wiederherstellung des primären Endpunkts

Durch die benutzerdefinierte Regel, die Sie im vorherigen Schritt in Fiddler festgelegt haben, treten beim Senden von Anforderungen an den primären Endpunkt Fehler auf. Um den primären Endpunkt in der Simulation wiederherzustellen, entfernen Sie die Logik, mit der der `503`-Fehler eingefügt wurde.

Drücken Sie **eine Taste**, um die Anwendung anzuhalten.

Navigieren Sie zu Fiddler, und wählen Sie zuerst **Rules** (Regeln) und anschließend **Customize Rules...** (Regeln anpassen...) aus.  Kommentieren Sie in der `OnBeforeResponse`-Funktion die benutzerdefinierte Logik aus, oder entfernen Sie diese, sodass die Standardfunktion wieder vorhanden ist. Wählen Sie **File** (Datei) und anschließend **Save** (Speichern) aus, um die Änderungen zu speichern.

![Entfernen der benutzerdefinierten Regel](media/storage-simulate-failure-ragrs-account-app/figure5.png)

Drücken Sie anschließend zum Fortsetzen der Anwendung **eine Taste**. Die Anwendung liest nun erneut aus dem primären Endpunkt, bis 999 Lesevorgänge erfasst wurden.

![Fortsetzen der Anwendung](media/storage-simulate-failure-ragrs-account-app/figure4.png)


## <a name="simulate-a-failure-with-an-invalid-static-route"></a>Simulieren eines Fehlers mit einer ungültigen statischen Route 
Sie können eine ungültige statische Route für alle Anforderungen an den primären Endpunkt Ihres Speicherkontos vom Typ [Georedundanter Speicher mit Lesezugriff](../common/storage-redundancy.md#read-access-geo-redundant-storage) (RA-GRS) erstellen. In diesem Tutorial wird der lokale Host als Gateway für das Weiterleiten von Anforderungen an das Speicherkonto verwendet. Die Nutzung des lokalen Hosts als Gateway bewirkt, dass für alle Anforderungen an den primären Endpunkt Ihres Speicherkontos eine Schleife zurück zum Host erfolgt. Dies führt schließlich zu einem Fehler. Führen Sie die folgenden Schritte aus, um einen Fehler zu simulieren und die Wiederherstellung des primären Endpunkts mit einer ungültigen statischen Route durchzuführen. 

### <a name="start-and-pause-the-application"></a>Starten und Anhalten der Anwendung

Führen Sie die Anwendung in Ihrer IDE oder im Text-Editor aus. Drücken Sie im Konsolenfenster **eine Taste**, um die Anwendung anzuhalten, während diese aus dem primären Endpunkt liest. 

### <a name="simulate-failure"></a>Simulieren eines Fehlers

Starten Sie die Eingabeaufforderung bei angehaltener Anwendung unter Windows als Administrator, oder führen Sie das Terminal unter Linux als root-Benutzer aus. Geben Sie den folgenden Befehl in einer Eingabeaufforderung oder einem Terminal ein, um Informationen zur Domäne des primären Endpunkts für das Speicherkonto zu erhalten.

```
nslookup STORAGEACCOUNTNAME.blob.core.windows.net
``` 
 Ersetzen Sie `STORAGEACCOUNTNAME` durch den Namen Ihres Speicherkontos. Kopieren Sie die IP-Adresse Ihres Speicherkontos zur späteren Verwendung in einen Text-Editor. Geben Sie zum Abrufen der IP-Adresse Ihres lokalen Hosts an der Windows-Eingabeaufforderung `ipconfig` bzw. im Linux-Terminal `ifconfig` ein. 

Geben Sie den folgenden Befehl an einer Windows-Eingabeaufforderung oder einem Linux-Terminal ein, um eine statische Route für einen Zielhost hinzuzufügen. 


# <a name="linuxtablinux"></a>[Linux](#tab/linux)

  route add <destination_ip> gw <gateway_ip>

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

  route add <destination_ip> <gateway_ip>

---
 
Ersetzen Sie `<destination_ip>` durch die IP-Adresse Ihres Speicherkontos und `<gateway_ip>` durch die IP-Adresse Ihres lokalen Hosts. Drücken Sie zum Fortsetzen der Anwendung eine **beliebige Taste**.

Sobald die Anwendung erneut gestartet wird, treten beim Senden von Anforderungen an den primären Endpunkt Fehler auf. Die Anwendung versucht fünfmal, erneut eine Verbindung mit dem primären Endpunkt herzustellen. Wenn der Verbindungsaufbau mehr als fünfmal scheitert, fordert die Anwendung das Bild vom sekundären Endpunkt an, der schreibgeschützt ist. Nachdem die Anwendung das Bild erfolgreich 20 Mal vom sekundären Endpunkt abgerufen hat, versucht sie, eine Verbindung mit dem primären Endpunkt herzustellen. Wenn dieser weiterhin nicht erreichbar ist, beginnt die Anwendung erneut mit dem Lesen aus dem sekundären Endpunkt. Hierbei handelt es sich um das Muster [Trennschalter](/azure/architecture/patterns/circuit-breaker.md) (Circuit Breaker), das im vorherigen Tutorial beschrieben wurde.

### <a name="simulate-primary-endpoint-restoration"></a>Simulieren der Wiederherstellung des primären Endpunkts

Löschen Sie die statische Route des primären Endpunkts aus der Routingtabelle, um zu simulieren, dass der primäre Endpunkt wieder funktioniert. Dies ermöglicht, dass alle Anforderungen an den primären Endpunkt über das Standardgateway geleitet werden. 

Geben Sie den folgenden Befehl an einer Windows-Eingabeaufforderung oder in einem Linux-Terminal ein, um die statische Route eines Zielhosts (Speicherkonto) zu löschen. 
 
# <a name="linuxtablinux"></a>[Linux](#tab/linux)

route del <destination_ip> gw <gateway_ip>

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

route delete <destination_ip>

---

Drücken Sie eine **beliebige Taste**, um die Ausführung der Anwendung fortzusetzen. Die Anwendung liest nun erneut aus dem primären Endpunkt, bis 999 Lesevorgänge erfasst wurden.

![Fortsetzen der Anwendung](media/storage-simulate-failure-ragrs-account-app/figure4.png)


## <a name="next-steps"></a>Nächste Schritte

Im zweiten Teil dieser Reihe haben Sie gelernt, wie Sie einen Fehler simulieren, um den georedundanten Speicher mit Lesezugriff zu testen. Folgende Schritte wurden dabei erläutert:

> [!div class="checklist"]
> * Ausführen und Anhalten einer Anwendung
> * Simulieren eines Fehlers mit [Fiddler](#simulate-a-failure-with-fiddler) oder [einer ungültigen statischen Route](#simulate-a-failure-with-an-invalid-static-route) 
> * Simulieren der Wiederherstellung des primären Endpunkts

Unter diesem Link finden Sie vordefinierte Speicherbeispiele.

> [!div class="nextstepaction"]
> [Beispielskripts für Azure Storage](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md