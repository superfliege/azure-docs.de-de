---
title: 'Tutorial: Simulieren eines Fehlers bei Zugriff auf georedundanten Speicher mit Lesezugriff in Azure | Microsoft-Dokumentation'
description: Simulieren eines Fehlers bei Zugriff auf georedundanten Speicher mit Lesezugriff
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 01/03/2019
ms.author: tamram
ms.openlocfilehash: 2d86c0c4838fb2ae9d839e64a067824019133d8b
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54024432"
---
# <a name="tutorial-simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Tutorial: Simulieren eines Fehlers bei Zugriff auf redundanten Speicher mit Lesezugriff

Dieses Tutorial ist der zweite Teil einer Reihe. In diesem Teil erfahren Sie, welchen Nutzen ein [georedundanter Speicher mit Lesezugriff](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (Read-Access Geographically Redundant Storage, RA-GRS) bietet, indem Sie einen Fehler simulieren.

Zum Simulieren eines Fehlers können Sie [Fiddler](#simulate-a-failure-with-fiddler) oder [statisches Routing](#simulate-a-failure-with-an-invalid-static-route) verwenden. Beide Methoden ermöglichen es Ihnen, einen Fehler für Anforderungen an den primären Endpunkt Ihres RA-GRS-Speicherkontos ([georedundanter Speicher mit Lesezugriff](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage)) zu simulieren, sodass die Anwendung stattdessen vom sekundären Endpunkt liest.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Im zweiten Teil der Reihe lernen Sie Folgendes:

> [!div class="checklist"]
> * Ausführen und Anhalten einer Anwendung
> * Simulieren eines Fehlers mit [Fiddler](#simulate-a-failure-with-fiddler) oder [einer ungültigen statischen Route](#simulate-a-failure-with-an-invalid-static-route) 
> * Simulieren der Wiederherstellung des primären Endpunkts

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie vor Beginn dieses Tutorials das vorherige Tutorial durch: [Herstellen von Hochverfügbarkeit für Anwendungsdaten mit Azure Storage][previous-tutorial].

Gehen Sie wie folgt vor, um mit Fiddler einen Fehler zu simulieren: 

* Laden Sie das Tool [Fiddler](https://www.telerik.com/download/fiddler) herunter, und installieren Sie es.

## <a name="simulate-a-failure-with-fiddler"></a>Simulieren eines Fehlers mit Fiddler

Zum Simulieren eines Fehlers mit Fiddler injizieren Sie eine fehlerhafte Antwort für Anforderungen an den primären Endpunkt Ihres RA-GRS-Speicherkontos.

In den folgenden Abschnitten wird gezeigt, wie Sie mit Fiddler einen Fehler simulieren und die Wiederherstellung des primären Endpunkts durchführen.

### <a name="launch-fiddler"></a>Starten von Fiddler

Öffnen Sie Fiddler, und wählen Sie zuerst **Rules** (Regeln) und anschließend **Customize Rules** (Regeln anpassen) aus.

![Anpassen von Fiddler-Regeln](media/storage-simulate-failure-ragrs-account-app/figure1.png)

Der Fiddler ScriptEditor wird gestartet und zeigt die Datei **SampleRules.js** an. Mit dieser Datei können Sie Fiddler anpassen.

Fügen Sie das folgende Codebeispiel in die `OnBeforeResponse`-Funktion ein. Der neue Code ist auskommentiert, damit die damit erstellte Logik nicht unmittelbar implementiert wird.

Wählen Sie anschließend **File** (Datei) und dann **Save** (Speichern) aus, um Ihre Änderungen zu speichern.

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

![Einfügen einer benutzerdefinierten Regel](media/storage-simulate-failure-ragrs-account-app/figure2.png)

### <a name="interrupting-the-application"></a>Unterbrechen der Anwendung

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET, Python und Java v7] (#tab/dotnet-python-java-v7)

Führen Sie die Anwendung in Ihrer integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) oder in einer Shell aus.

Drücken Sie im Konsolenfenster **eine Taste**, um die Anwendung anzuhalten, während diese aus dem primären Endpunkt liest.

![Szenario-App](media/storage-simulate-failure-ragrs-account-app/scenario.png)

# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

Führen Sie die Anwendung in Ihrer integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) oder in einer Shell aus.

Da Sie das Beispiel steuern, müssen Sie es nicht unterbrechen, um einen Fehler zu simulieren. Sie müssen nur sicherstellen, dass die Datei in Ihr Speicherkonto hochgeladen wurde, indem Sie das Beispiel ausführen und **P** eingeben.

![Szenario-App](media/storage-simulate-failure-ragrs-account-app/Java-put-list-output.png)

---

### <a name="simulate-failure"></a>Simulieren eines Fehlers

Während die Anwendung angehalten wird, heben Sie die Auskommentierung der benutzerdefinierten Regel auf, die Sie in Fiddler gespeichert haben.

Im Codebeispiel wird nach Anforderungen an das RA-GRS-Speicherkonto gesucht, und wenn der Pfad den Namen der Datei (`HelloWorld`) enthält, wird der Antwortcode `503 - Service Unavailable` zurückgegeben.

Navigieren Sie zu Fiddler, und wählen Sie zuerst **Rules**  -> (Regeln) und anschließend **Customize Rules...** (Regeln anpassen...) aus.

Heben Sie die Auskommentierung der folgenden Zeilen auf, und ersetzen Sie `STORAGEACCOUNTNAME` durch den Namen Ihres Speicherkontos. Wählen Sie zuerst **File**  -> (Datei) und anschließend **Save** (Speichern) aus, um die Änderungen zu speichern. 

> [!NOTE]
> Wenn Sie die Beispielanwendung unter Linux ausführen, müssen Sie Fiddler jeweils neu starten, wenn Sie die Datei **CustomRule.js** bearbeiten, damit die benutzerdefinierte Logik für Fiddler installiert wird.

```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET, Python und Java v7] (#tab/dotnet-python-java-v7)

Drücken Sie zum Fortsetzen der Anwendung eine **beliebige Taste**.

Sobald die Anwendung erneut gestartet wird, treten beim Senden von Anforderungen an den primären Endpunkt Fehler auf. Die Anwendung versucht fünfmal, erneut eine Verbindung mit dem primären Endpunkt herzustellen. Wenn der Verbindungsaufbau mehr als fünfmal scheitert, fordert die Anwendung das Bild vom sekundären Endpunkt an, der schreibgeschützt ist. Wenn die Anwendung das Bild 20-mal erfolgreich vom sekundären Endpunkt abgerufen hat, versucht sie, eine Verbindung mit dem primären Endpunkt herzustellen. Wenn dieser weiterhin nicht erreichbar ist, beginnt die Anwendung erneut mit dem Lesen aus dem sekundären Endpunkt.

Hierbei handelt es sich um das Muster [Trennschalter](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker) (Circuit Breaker), das im vorherigen Tutorial beschrieben wurde.

![Einfügen einer benutzerdefinierten Regel](media/storage-simulate-failure-ragrs-account-app/figure3.png)

# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

Nachdem Sie den Fehler eingeleitet haben, geben Sie **G** ein, um ihn zu testen.

Sie werden darüber informiert, dass die sekundäre Pipeline anstelle der primären Pipeline verwendet wird.

---

### <a name="simulate-primary-endpoint-restoration"></a>Simulieren der Wiederherstellung des primären Endpunkts

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET, Python und Java v7] (#tab/dotnet-python-java-v7)

Durch die benutzerdefinierte Regel, die Sie im vorherigen Schritt in Fiddler festgelegt haben, treten beim Senden von Anforderungen an den primären Endpunkt Fehler auf.

Um den primären Endpunkt in der Simulation wiederherzustellen, entfernen Sie die Logik, mit der der `503`-Fehler eingefügt wurde.

Drücken Sie **eine Taste**, um die Anwendung anzuhalten.

Navigieren Sie zu Fiddler, und wählen Sie zuerst **Rules** (Regeln) und anschließend **Customize Rules...** (Regeln anpassen...) aus. 

Kommentieren Sie in der `OnBeforeResponse`-Funktion die benutzerdefinierte Logik aus, oder entfernen Sie diese, sodass die Standardfunktion wieder vorhanden ist.

Wählen Sie **File** (Datei) und anschließend **Save** (Speichern) aus, um die Änderungen zu speichern.

![Entfernen der benutzerdefinierten Regel](media/storage-simulate-failure-ragrs-account-app/figure5.png)

Drücken Sie anschließend zum Fortsetzen der Anwendung **eine Taste**. Die Anwendung liest nun erneut aus dem primären Endpunkt, bis 999 Lesevorgänge erfasst wurden.

![Fortsetzen der Anwendung](media/storage-simulate-failure-ragrs-account-app/figure4.png)

# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

Durch die benutzerdefinierte Regel, die Sie im vorherigen Schritt in Fiddler festgelegt haben, treten beim Senden von Anforderungen an den primären Endpunkt Fehler auf.

Um den primären Endpunkt in der Simulation wiederherzustellen, entfernen Sie die Logik, mit der der `503`-Fehler eingefügt wurde.

Navigieren Sie zu Fiddler, und wählen Sie zuerst **Rules** (Regeln) und anschließend **Customize Rules...** (Regeln anpassen...) aus.  Kommentieren Sie in der `OnBeforeResponse`-Funktion die benutzerdefinierte Logik aus, oder entfernen Sie diese, sodass die Standardfunktion wieder vorhanden ist.

Wählen Sie **File** (Datei) und anschließend **Save** (Speichern) aus, um die Änderungen zu speichern.

Geben Sie anschließend **G** ein, um den Download zu testen. Die Anwendung meldet, dass sie jetzt wieder die primäre Pipeline verwendet.

---

## <a name="simulate-a-failure-with-an-invalid-static-route"></a>Simulieren eines Fehlers mit einer ungültigen statischen Route

Sie können eine ungültige statische Route für alle Anforderungen an den primären Endpunkt Ihres Speicherkontos vom Typ [Georedundanter Speicher mit Lesezugriff](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) erstellen. In diesem Tutorial wird der lokale Host als Gateway für das Weiterleiten von Anforderungen an das Speicherkonto verwendet. Die Nutzung des lokalen Hosts als Gateway bewirkt, dass für alle Anforderungen an den primären Endpunkt Ihres Speicherkontos eine Schleife zurück zum Host erfolgt. Dies führt schließlich zu einem Fehler. Führen Sie die folgenden Schritte aus, um einen Fehler zu simulieren und die Wiederherstellung des primären Endpunkts mit einer ungültigen statischen Route durchzuführen. 

### <a name="start-and-pause-the-application"></a>Starten und Anhalten der Anwendung

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET, Python und Java v7] (#tab/dotnet-python-java-v7)

Führen Sie die Anwendung in Ihrer integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) oder in einer Shell aus. Drücken Sie im Konsolenfenster **eine Taste**, um die Anwendung anzuhalten, während diese aus dem primären Endpunkt liest.

# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

Da Sie das Beispiel steuern, müssen Sie es nicht unterbrechen, um den Fehler zu testen.

Stellen Sie sicher, dass die Datei in Ihr Speicherkonto hochgeladen wurde, indem Sie das Beispiel ausführen und **P** eingeben.

---

### <a name="simulate-failure"></a>Simulieren eines Fehlers

Starten Sie die Eingabeaufforderung bei angehaltener Anwendung unter Windows als Administrator, oder führen Sie das Terminal unter Linux als root-Benutzer aus.

Geben Sie an einer Eingabeaufforderung oder in einem Terminal den folgenden Befehl ein, um Informationen zur Domäne des primären Endpunkts des Speicherkontos abzurufen.

```
nslookup STORAGEACCOUNTNAME.blob.core.windows.net
``` 
 Ersetzen Sie `STORAGEACCOUNTNAME` durch den Namen Ihres Speicherkontos. Kopieren Sie die IP-Adresse Ihres Speicherkontos zur späteren Verwendung in einen Text-Editor.

Geben Sie zum Abrufen der IP-Adresse Ihres lokalen Hosts an der Windows-Eingabeaufforderung `ipconfig` bzw. im Linux-Terminal `ifconfig` ein. 

Geben Sie den folgenden Befehl an einer Windows-Eingabeaufforderung oder einem Linux-Terminal ein, um eine statische Route für einen Zielhost hinzuzufügen. 

#### <a name="linux"></a>Linux

`route add <destination_ip> gw <gateway_ip>`

#### <a name="windows"></a>Windows

`route add <destination_ip> <gateway_ip>`

Ersetzen Sie `<destination_ip>` durch die IP-Adresse Ihres Speicherkontos und `<gateway_ip>` durch die IP-Adresse Ihres lokalen Hosts.

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET, Python und Java v7] (#tab/dotnet-python-java-v7)

Drücken Sie zum Fortsetzen der Anwendung eine **beliebige Taste**.

Sobald die Anwendung erneut gestartet wird, treten beim Senden von Anforderungen an den primären Endpunkt Fehler auf. Die Anwendung versucht fünfmal, erneut eine Verbindung mit dem primären Endpunkt herzustellen. Wenn der Verbindungsaufbau mehr als fünfmal scheitert, fordert die Anwendung das Bild vom sekundären Endpunkt an, der schreibgeschützt ist. Nachdem die Anwendung das Bild erfolgreich 20 Mal vom sekundären Endpunkt abgerufen hat, versucht sie, eine Verbindung mit dem primären Endpunkt herzustellen. Wenn dieser weiterhin nicht erreichbar ist, beginnt die Anwendung erneut mit dem Lesen aus dem sekundären Endpunkt. Hierbei handelt es sich um das Muster [Trennschalter](/azure/architecture/patterns/circuit-breaker) (Circuit Breaker), das im vorherigen Tutorial beschrieben wurde.

# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

Nachdem Sie den Fehler eingeleitet haben, geben Sie **G** ein, um ihn zu testen. Sie werden darüber informiert, dass die sekundäre Pipeline anstelle der primären Pipeline verwendet wird.

---

### <a name="simulate-primary-endpoint-restoration"></a>Simulieren der Wiederherstellung des primären Endpunkts

Löschen Sie die statische Route des primären Endpunkts aus der Routingtabelle, um zu simulieren, dass der primäre Endpunkt wieder funktioniert. Dies ermöglicht, dass alle Anforderungen an den primären Endpunkt über das Standardgateway geleitet werden.

Geben Sie den folgenden Befehl an einer Windows-Eingabeaufforderung oder in einem Linux-Terminal ein, um die statische Route eines Zielhosts (Speicherkonto) zu löschen.

#### <a name="linux"></a>Linux

`route del <destination_ip> gw <gateway_ip>`

#### <a name="windows"></a>Windows

`route delete <destination_ip>`

# <a name="net-python-and-java-v7-tabdotnet-python-java-v7"></a>[.NET, Python und Java v7] (#tab/dotnet-python-java-v7)

Drücken Sie eine **beliebige Taste**, um die Ausführung der Anwendung fortzusetzen. Die Anwendung liest nun erneut aus dem primären Endpunkt, bis 999 Lesevorgänge erfasst wurden.

![Fortsetzen der Anwendung](media/storage-simulate-failure-ragrs-account-app/figure4.png)


# <a name="java-v10-tabjava-v10"></a>[Java v10] (#tab/Java-v10)

Geben Sie **G** ein, um den Download zu testen. Die Anwendung meldet, dass sie jetzt wieder die primäre Pipeline verwendet.

![Fortsetzen der Anwendung](media/storage-simulate-failure-ragrs-account-app/java-get-pipeline-example-v10.png)

---

## <a name="next-steps"></a>Nächste Schritte

Im zweiten Teil dieser Reihe haben Sie gelernt, wie Sie einen Fehler simulieren, um den georedundanten Speicher mit Lesezugriff zu testen.

Im folgenden Artikel finden Sie weitere Informationen zur Funktionsweise von georedundantem Speicher mit Lesezugriff (RA-GRS) und zu den damit verbundenen Risiken:

> [!div class="nextstepaction"]
> [Entwerfen hochverfügbarer Anwendungen mithilfe von RA-GRS](../common/storage-designing-ha-apps-with-ragrs.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md