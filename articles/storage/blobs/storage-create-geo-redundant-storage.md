---
title: "Herstellen von Hochverfügbarkeit für Anwendungsdaten in Azure | Microsoft-Dokumentation"
description: "Verwenden von georedundantem Speicher mit Lesezugriff zur Herstellung von Hochverfügbarkeit für Anwendungsdaten"
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
ms.openlocfilehash: 547ca7843f53bd11fdb922af8e0ae77e38f813d9
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2017
---
# <a name="make-your-application-data-highly-available-with-azure-storage"></a>Herstellen von Hochverfügbarkeit für Anwendungsdaten mit Azure Storage

Dieses Tutorial ist der erste Teil einer Serie. In diesem Tutorial wird beschrieben, wie Sie Hochverfügbarkeit für Ihre Anwendungsdaten in Azure herstellen. Am Ende des Tutorials werden Sie über eine Konsolenanwendung verfügen, mit der Sie ein Blob in ein RA-GRS-Speicherkonto ([read-access geo-redundant storage, georedundanter Speicher mit Lesezugriff](../common/storage-redundancy.md#read-access-geo-redundant-storage)) hochladen bzw. von diesem abrufen können. RA-GRS funktioniert durch die Replikation von Transaktionen von der primären zur sekundären Region. Dieser Replikationsprozess garantiert, dass die Daten in der sekundären Region letztendlich konsistent sind. Für die Anwendung wird das Muster [Trennschalter](/azure/architecture/patterns/circuit-breaker.md) (Circuit Breaker) verwendet, um den Endpunkt zu ermitteln, mit dem eine Verbindung hergestellt werden soll. Die Anwendung verbindet sich mit dem sekundären Endpunkt, wenn ein Fehler simuliert wird.

Im ersten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen Sie ein Speicherkonto.
> * Herunterladen des Beispiels
> * Festlegen der Verbindungszeichenfolge
> * Ausführen der Konsolenanwendung

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit den folgenden Workloads:
  - **Azure-Entwicklung**

  ![Azure-Entwicklung (unter Web & Cloud)](media/storage-create-geo-redundant-storage/workloads.png)

* Laden Sie das Tool [Fiddler](https://www.telerik.com/download/fiddler) herunter, und installieren Sie es.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto.

Ein Speicherkonto stellt einen eindeutigen Namespace zum Speichern Ihrer Azure Storage-Datenobjekte sowie für den Zugriff darauf bereit.

Führen Sie die folgenden Schritte aus, um ein georedundantes Speicherkonto mit Lesezugriff zu erstellen:

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Klicken Sie zuerst auf der Seite **Neu** auf **Speicher** und anschließend unter **Empfohlen** auf **Speicherkonto – Blob, Datei, Tabelle, Warteschlange**.
3. Füllen Sie das Speicherkontoformular wie in der Abbildung dargestellt mit den folgenden Angaben aus, und klicken Sie auf **Erstellen**.

   | Einstellung       | Empfohlener Wert | Beschreibung |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Name** | mystorageaccount | ein eindeutiger Wert für Ihr Speicherkonto |
   | **Bereitstellungsmodell** | Ressourcen-Manager  | Azure Resource Manager enthält die neuesten Funktionen.  |
   | **Kontoart** | Allgemeiner Zweck | Weitere Informationen zu den unterschiedlichen Kontoarten finden Sie unter [Speicherkontentypen](../common/storage-introduction.md#types-of-storage-accounts). |
   | **Leistung** | Standard | Der Wert „Standard“ ist für das Beispielszenario ausreichend. |
   | **Replikation**| Georedundanter Speicher mit Lesezugriff (RA-GRS) | Diese Angabe ist erforderlich, damit das Beispiel funktioniert. |
   |**Sichere Übertragung erforderlich** | Deaktiviert| Für dieses Szenario ist keine sichere Übertragung notwendig. |
   |**Abonnement** | Ihr Abonnement |Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
   |**ResourceGroup** | myResourceGroup |Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
   |**Standort** | USA (Ost) | Wählen Sie einen Standort aus. |

![Erstellen eines Speicherkontos](media/storage-create-geo-redundant-storage/figure1.png)

## <a name="download-the-sample"></a>Herunterladen des Beispiels

[Laden Sie das Beispielprojekt](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) herunter.

Extrahieren (entzippen) Sie die Datei „storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip“.
Das Beispielprojekt enthält eine Konsolenanwendung.

## <a name="set-the-connection-string"></a>Festlegen der Verbindungszeichenfolge

Öffnen Sie die Konsolenanwendung *storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs* in Visual Studio.

Ersetzen Sie in der Datei **App.config** im XML-Element **appSettings** den Wert _StorageConnectionString_ mit der Verbindungszeichenfolge Ihres Speicherkontos. Dieser Wert wird abgerufen, wenn Sie im Azure-Portal in Ihrem Speicherkonto unter **Einstellungen** **Zugriffsschlüssel** auswählen. Kopieren Sie die **Verbindungszeichenfolge** aus dem primären oder sekundären Schlüssel, und fügen Sie sie in die Datei **App.config** ein. Klicken Sie auf **Speichern**, um die Datei abschließend zu speichern.

![Datei „App.config“](media/storage-create-geo-redundant-storage/figure2.png)

## <a name="run-the-console-application"></a>Ausführen der Konsolenanwendung

Drücken Sie in Visual Studio **F5**, oder wählen Sie **Starten** aus, um mit dem Debuggen der Anwendung zu beginnen. Visual Studio stellt fehlende NuGet-Pakete bei entsprechender Konfiguration automatisch wieder her. Weitere Informationen erhalten Sie unter [Installing and reinstalling packages with package restore (Installieren und Neuinstallieren von Paketen mit der Paketwiederherstellung)](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview). 

Ein Konsolenfenster wird geöffnet, und die Anwendung wird ausgeführt. Die Anwendung lädt das Bild **HelloWorld.png** Bild aus der Projektmappe in das Speicherkonto hoch. Anschließend wird von der Anwendung überprüft, ob das Bild repliziert wurde und sich am sekundären RA-GRS-Endpunkt befindet. Das Bild wird dann von der Anwendung maximal 999 Mal heruntergeladen. Jeder Lesevorgang wird dabei durch ein **P** oder **S** dargestellt. Während **P** für den primären Endpunkt steht, repräsentiert **S** den sekundären Endpunkt.

![Konsolenanwendung wird ausgeführt](media/storage-create-geo-redundant-storage/figure3.png)

Im Beispielcode wird die Aufgabe `RunCircuitBreakerAsync` in der Datei `Program.cs` verwendet, um mit der [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.downloadtofileasync?view=azure-dotnet)-Methode ein Bild aus dem Speicherkonto herunterzuladen. Vor dem Download wird ein [OperationContext](/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet)-Objekt initialisiert. Der Vorgangskontext definiert Ereignishandler, die aufgerufen werden, wenn ein Download erfolgreich abgeschlossen wurde oder wenn ein Download fehlschlägt und erneut versucht wird, den Download auszuführen.

### <a name="retry-event-handler"></a>Ereignishandler für erneuten Downloadversuch

Der Ereignishandler `Operation_context_Retrying` wird aufgerufen, wenn der Download des Bilds fehlschlägt und erneut ausgeführt werden soll. Wenn die maximale Anzahl von Wiederholungen erreicht wird, die in der Anwendung definiert ist, wird die [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode)-Eigenschaft der Anforderung in `SecondaryOnly` geändert. Durch diese Einstellung versucht die Anwendung, das Bild vom sekundären Endpunkt herunterzuladen. Diese Konfiguration reduziert die Anforderungszeit für das Bild, da die Anzahl der wiederholten Anforderungen an den primären Endpunkt begrenzt ist.

```csharp
private static void Operation_context_Retrying(object sender, RequestEventArgs e)
{
    retryCount++;
    Console.WriteLine("Retrying event because of failure reading the primary. RetryCount = " + retryCount);

    // Check if we have had more than n retries in which case switch to secondary.
    if (retryCount >= retryThreshold)
    {

        // Check to see if we can fail over to secondary.
        if (blobClient.DefaultRequestOptions.LocationMode != LocationMode.SecondaryOnly)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.SecondaryOnly;
            retryCount = 0;
        }
        else
        {
            throw new ApplicationException("Both primary and secondary are unreachable. Check your application's network connection. ");
        }
    }
}
```

### <a name="request-completed-event-handler"></a>Ereignishandler für abgeschlossene Anforderung

Der Ereignishandler `Operation_context_RequestCompleted` wird aufgerufen, wenn der Download des Bilds erfolgreich ist. Wenn die Anwendung den sekundären Endpunkt verwendet, stellt die Anwendung weiterhin bis zu 20 Anforderungen an diesen Endpunkt. Nach 20 Anforderungen legt die Anwendung für die [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode)-Eigenschaft erneut den Wert `PrimaryThenSecondary` fest und versucht nun wieder, Anforderungen an den primären Endpunkt zu senden. Wenn eine Anforderung erfolgreich ist, liest die Anwendung weiterhin aus dem primären Endpunkt.

```csharp
private static void Operation_context_RequestCompleted(object sender, RequestEventArgs e)
{
    if (blobClient.DefaultRequestOptions.LocationMode == LocationMode.SecondaryOnly)
    {
        // You're reading the secondary. Let it read the secondary [secondaryThreshold] times, 
        //    then switch back to the primary and see if it's available now.
        secondaryReadCount++;
        if (secondaryReadCount >= secondaryThreshold)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;
            secondaryReadCount = 0;
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Im ersten Teil dieser Reihe haben Sie erfahren, wie Sie mit RA-GRS-Speicherkonten Hochverfügbarkeit für eine Anwendung herstellen. Folgende Schritte wurden dabei erläutert:

> [!div class="checklist"]
> * Erstellen Sie ein Speicherkonto.
> * Herunterladen des Beispiels
> * Festlegen der Verbindungszeichenfolge
> * Ausführen der Konsolenanwendung

Im zweiten Teil der Reihe erfahren Sie, wie Sie einen Fehler simulieren, durch den Ihre Anwendung auf den sekundären RA-GRS-Endpunkt ausweichen muss.

> [!div class="nextstepaction"]
> [Simulate a failure in connection to your primary storage endpoint (Simulieren eines Fehlers bei einem primären Speicherendpunkt)](storage-simulate-failure-ragrs-account-app.md)
