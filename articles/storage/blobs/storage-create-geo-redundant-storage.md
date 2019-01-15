---
title: 'Tutorial: Herstellen von Hochverfügbarkeit für Anwendungsdaten in Azure | Microsoft-Dokumentation'
description: Verwenden von georedundantem Speicher mit Lesezugriff, um Ihre Anwendungsdaten hoch verfügbar zu machen
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 01/03/2019
ms.author: tamram
ms.custom: mvc
ms.component: blobs
ms.openlocfilehash: e6b64c7545e7a36b9178af84f88ee89b498077b0
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54019095"
---
# <a name="tutorial-make-your-application-data-highly-available-with-azure-storage"></a>Tutorial: Herstellen von Hochverfügbarkeit für Anwendungsdaten mit Azure Storage

Dieses Tutorial ist der erste Teil einer Serie. In diesem Teil erfahren Sie, wie Sie Hochverfügbarkeit für Ihre Anwendungsdaten in Azure herstellen.

Am Ende des Tutorials verfügen Sie über eine Konsolenanwendung, die ein Blob in ein RA-GRS-Speicherkonto ([georedundanter Speicher mit Lesezugriff](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage)) hochlädt und aus diesem abruft.

Georedundanter Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS) beruht auf der Replikation von Transaktionen von einer primären in eine sekundäre Region. Dieser Replikationsprozess garantiert, dass die Daten in der sekundären Region letztendlich konsistent sind. Die Anwendung ermittelt mithilfe des [Circuit-Breaker](/azure/architecture/patterns/circuit-breaker)-Musters, mit welchem Endpunkt eine Verbindung hergestellt werden soll, und wechselt automatisch zwischen Endpunkten, wenn Fehler und Wiederherstellungen simuliert werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Im ersten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Speicherkonto erstellen
> * Festlegen der Verbindungszeichenfolge
> * Ausführen der Konsolenanwendung

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit den folgenden Workloads:
  - **Azure-Entwicklung**

  ![Azure-Entwicklung (unter Web & Cloud)](media/storage-create-geo-redundant-storage/workloads.png)

# <a name="python-tabpython"></a>[Python] (#tab/python)

* [Installieren Sie Python.](https://www.python.org/downloads/)
* Laden Sie das [Azure Storage SDK für Python](https://github.com/Azure/azure-storage-python) herunter, und installieren Sie es.

# <a name="java-v7-sdk--tabjava-v7"></a>[Java V7 SDK ] (#tab/java-v7)

* Installieren Sie [Maven](http://maven.apache.org/download.cgi), und konfigurieren Sie es so, dass es über die Befehlszeile verwendet werden kann.
* Installieren und konfigurieren Sie ein [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

# <a name="java-v10-sdk-tabjava-v10"></a>[Java V10 SDK] (#tab/java-v10)

* Installieren Sie [Maven](http://maven.apache.org/download.cgi), und konfigurieren Sie es so, dass es über die Befehlszeile verwendet werden kann.
* Installieren und konfigurieren Sie ein [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

---

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Ein Speicherkonto stellt einen eindeutigen Namespace zum Speichern Ihrer Azure Storage-Datenobjekte sowie für den Zugriff auf diese Objekte bereit.

Führen Sie die folgenden Schritte aus, um ein georedundantes Speicherkonto mit Lesezugriff zu erstellen:

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Ressource erstellen**.
2. Klicken Sie auf der Seite **Neu** auf **Storage**.
3. Wählen Sie unter **Ausgewählte** die Option **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** aus.
4. Füllen Sie das Speicherkontoformular wie in der Abbildung dargestellt mit den folgenden Angaben aus, und klicken Sie auf **Erstellen**.

   | Einstellung       | Empfohlener Wert | Beschreibung |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Name** | mystorageaccount | ein eindeutiger Wert für Ihr Speicherkonto |
   | **Bereitstellungsmodell** | Ressourcen-Manager  | Azure Resource Manager enthält die neuesten Funktionen.|
   | **Kontoart** | StorageV2 | Weitere Informationen zu den unterschiedlichen Kontoarten finden Sie unter [Speicherkontentypen](../common/storage-introduction.md#types-of-storage-accounts). |
   | **Leistung** | Standard | Der Wert „Standard“ ist für das Beispielszenario ausreichend. |
   | **Replikation**| Georedundanter Speicher mit Lesezugriff (RA-GRS) | Diese Angabe ist erforderlich, damit das Beispiel funktioniert. |
   |**Abonnement** | Ihr Abonnement |Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
   |**ResourceGroup** | myResourceGroup |Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
   |**Location** | USA (Ost) | Wählen Sie einen Standort aus. |

![Erstellen eines Speicherkontos](media/storage-create-geo-redundant-storage/createragrsstracct.png)

## <a name="download-the-sample"></a>Herunterladen des Beispiels

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

[Laden Sie das Beispielprojekt herunter](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip), und extrahieren (entzippen) Sie die Datei „storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip“. Sie können auch [Git](https://git-scm.com/) verwenden, um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. Das Beispielprojekt enthält eine Konsolenanwendung.

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.git 
```

# <a name="python-tabpython"></a>[Python] (#tab/python)

[Laden Sie das Beispielprojekt herunter](https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip), und extrahieren (entzippen) Sie die Datei „storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.zip“. Sie können auch [Git](https://git-scm.com/) verwenden, um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. Das Beispielprojekt enthält eine einfache Python-Anwendung.

```bash
git clone https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="java-v7-sdk--tabjava-v7"></a>[Java V7 SDK ] (#tab/java-v7)

[Laden Sie ein Beispielprojekt herunter](https://github.com/Azure-Samples/storage-java-ha-ra-grs), und extrahieren Sie die Datei „storage-java-ragrs.zip“. Sie können auch [Git](https://git-scm.com/) verwenden, um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. Das Beispielprojekt enthält eine einfache Java-Anwendung.

```bash
git clone https://github.com/Azure-Samples/storage-java-ha-ra-grs.git
```

# <a name="java-v10-sdk-tabjava-v10"></a>[Java V10 SDK] (#tab/java-v10)

[Laden Sie ein Beispielprojekt herunter](https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs), und extrahieren Sie die Datei „storage-java-ragrs.zip“. Sie können auch [Git](https://git-scm.com/) verwenden, um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. Das Beispielprojekt enthält eine einfache Java-Anwendung.

```bash
git clone https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs
```

---

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

Sie müssen in der Anwendung die Verbindungszeichenfolge für das Speicherkonto angeben. Es wird empfohlen, diese Verbindungszeichenfolge auf dem lokalen Computer, auf dem die Anwendung ausgeführt wird, in einer Umgebungsvariablen zu speichern. Befolgen Sie je nach Betriebssystem die Schritte für eines der unten angegebenen Beispiele, um die Umgebungsvariable zu erstellen.

Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto. Wählen Sie in Ihrem Speicherkonto unter **Einstellungen** die Option **Zugriffsschlüssel** aus. Kopieren Sie die **Verbindungszeichenfolge** aus dem primären oder sekundären Schlüssel. Ersetzen Sie \<yourconnectionstring\> durch Ihre tatsächliche Verbindungszeichenfolge, indem Sie basierend auf Ihrem Betriebssystem einen der folgenden Befehle ausführen. Mit diesem Befehl wird eine Umgebungsvariable auf dem lokalen Computer gespeichert. Unter Windows ist die Umgebungsvariable erst verfügbar, wenn Sie die **Eingabeaufforderung** oder die verwendete Shell neu laden. Ersetzen Sie **\<storageConnectionString\>** im folgenden Beispiel:

### <a name="linux"></a>Linux

```
export storageconnectionstring=\<yourconnectionstring\> 
```
### <a name="windows"></a>Windows

```PowerShell
setx storageconnectionstring "\<yourconnectionstring\>"
```

# <a name="python-tabpython"></a>[Python] (#tab/python)

Sie müssen in der Anwendung die Verbindungszeichenfolge für das Speicherkonto angeben. Es wird empfohlen, diese Verbindungszeichenfolge auf dem lokalen Computer, auf dem die Anwendung ausgeführt wird, in einer Umgebungsvariablen zu speichern. Befolgen Sie je nach Betriebssystem die Schritte für eines der unten angegebenen Beispiele, um die Umgebungsvariable zu erstellen.

Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto. Wählen Sie in Ihrem Speicherkonto unter **Einstellungen** die Option **Zugriffsschlüssel** aus. Kopieren Sie die **Verbindungszeichenfolge** aus dem primären oder sekundären Schlüssel. Ersetzen Sie \<yourconnectionstring\> durch Ihre tatsächliche Verbindungszeichenfolge, indem Sie basierend auf Ihrem Betriebssystem einen der folgenden Befehle ausführen. Mit diesem Befehl wird eine Umgebungsvariable auf dem lokalen Computer gespeichert. Unter Windows ist die Umgebungsvariable erst verfügbar, wenn Sie die **Eingabeaufforderung** oder die verwendete Shell neu laden. Ersetzen Sie **\<storageConnectionString\>** im folgenden Beispiel:

### <a name="linux"></a>Linux

```
export storageconnectionstring=\<yourconnectionstring\> 
```
### <a name="windows"></a>Windows

```PowerShell
setx storageconnectionstring "\<yourconnectionstring\>"
```

# <a name="java-v7-sdk--tabjava-v7"></a>[Java V7 SDK ] (#tab/java-v7)

Sie müssen in der Anwendung die Verbindungszeichenfolge für das Speicherkonto angeben. Es wird empfohlen, diese Verbindungszeichenfolge auf dem lokalen Computer, auf dem die Anwendung ausgeführt wird, in einer Umgebungsvariablen zu speichern. Befolgen Sie je nach Betriebssystem die Schritte für eines der unten angegebenen Beispiele, um die Umgebungsvariable zu erstellen.

Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto. Wählen Sie in Ihrem Speicherkonto unter **Einstellungen** die Option **Zugriffsschlüssel** aus. Kopieren Sie die **Verbindungszeichenfolge** aus dem primären oder sekundären Schlüssel. Ersetzen Sie \<yourconnectionstring\> durch Ihre tatsächliche Verbindungszeichenfolge, indem Sie basierend auf Ihrem Betriebssystem einen der folgenden Befehle ausführen. Mit diesem Befehl wird eine Umgebungsvariable auf dem lokalen Computer gespeichert. Unter Windows ist die Umgebungsvariable erst verfügbar, wenn Sie die **Eingabeaufforderung** oder die verwendete Shell neu laden. Ersetzen Sie **\<storageConnectionString\>** im folgenden Beispiel:

### <a name="linux"></a>Linux

```
export storageconnectionstring=\<yourconnectionstring\> 
```
### <a name="windows"></a>Windows

```PowerShell
setx storageconnectionstring "\<yourconnectionstring\>"
```

# <a name="java-v10-sdk-tabjava-v10"></a>[Java V10 SDK] (#tab/java-v10)

Für dieses Beispiel müssen der Name und Schlüssel Ihres Speicherkontos sicher gespeichert werden. Speichern Sie sie in lokalen Umgebungsvariablen des Computers, auf dem das Beispiel ausgeführt wird. Folgen Sie je nach Betriebssystem den Schritten für das Linux- oder Windows-Beispiel, um die Umgebungsvariablen zu erstellen. Unter Windows ist die Umgebungsvariable erst verfügbar, wenn Sie die **Eingabeaufforderung** oder die verwendete Shell neu laden.

### <a name="linux-example"></a>Linux-Beispiel

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="windows-example"></a>Windows-Beispiel

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

---

## <a name="run-the-console-application"></a>Ausführen der Konsolenanwendung

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

Drücken Sie in Visual Studio **F5**, oder wählen Sie **Starten** aus, um mit dem Debuggen der Anwendung zu beginnen. Visual Studio stellt fehlende NuGet-Pakete bei entsprechender Konfiguration automatisch wieder her. Weitere Informationen finden Sie unter [Installieren und Neuinstallieren von Paketen mit der Paketwiederherstellung](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview).

Ein Konsolenfenster wird geöffnet, und die Anwendung wird ausgeführt. Die Anwendung lädt das Bild **HelloWorld.png** Bild aus der Projektmappe in das Speicherkonto hoch. Anschließend wird von der Anwendung überprüft, ob das Bild repliziert wurde und sich am sekundären RA-GRS-Endpunkt befindet. Das Bild wird dann von der Anwendung maximal 999 Mal heruntergeladen. Jeder Lesevorgang wird dabei durch ein **P** oder ein **S** dargestellt. Während **P** für den primären Endpunkt steht, repräsentiert **S** den sekundären Endpunkt.

![Konsolenanwendung wird ausgeführt](media/storage-create-geo-redundant-storage/figure3.png)

Im Beispielcode wird die Aufgabe `RunCircuitBreakerAsync` in der Datei `Program.cs` verwendet, um mit der [DownloadToFileAsync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_DownloadToFileAsync_System_String_System_IO_FileMode_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_)-Methode ein Bild aus dem Speicherkonto herunterzuladen. Vor dem Download wird ein [OperationContext](/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet)-Objekt initialisiert. Der Vorgangskontext definiert Ereignishandler, die aufgerufen werden, wenn ein Download erfolgreich abgeschlossen wurde oder wenn ein Download fehlschlägt und erneut versucht wird, den Download auszuführen.

# <a name="python-tabpython"></a>[Python] (#tab/python)

Navigieren Sie zum Ausführen der Anwendung in einem Terminal oder über eine Eingabeaufforderung zum Verzeichnis **circuitbreaker.py**, und geben Sie `python circuitbreaker.py` ein. Die Anwendung lädt das Bild **HelloWorld.png** Bild aus der Projektmappe in das Speicherkonto hoch. Anschließend wird von der Anwendung überprüft, ob das Bild repliziert wurde und sich am sekundären RA-GRS-Endpunkt befindet. Das Bild wird dann von der Anwendung maximal 999 Mal heruntergeladen. Jeder Lesevorgang wird dabei durch ein **P** oder ein **S** dargestellt. Während **P** für den primären Endpunkt steht, repräsentiert **S** den sekundären Endpunkt.

![Konsolenanwendung wird ausgeführt](media/storage-create-geo-redundant-storage/figure3.png)

Im Beispielcode wird die `run_circuit_breaker`-Methode in der Datei `circuitbreaker.py` verwendet, um mit der [get_blob_to_path](https://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html)-Methode ein Bild aus dem Speicherkonto herunterzuladen. 

Die retry-Funktion des Storage-Objekts wird auf eine lineare Wiederholungsrichtlinie festgelegt. Mit der retry-Funktion wird ermittelt, ob für eine Anforderung ein Wiederholungsversuch durchgeführt werden soll. Außerdem wird angegeben, wie viele Sekunden lang gewartet werden soll, bevor der Versuch gestartet wird. Legen Sie den Wert **retry\_to\_secondary** auf „true“ fest, wenn für die Anforderung ein Wiederholungsversuch für den sekundären Endpunkt durchgeführt werden soll, falls die erste Anforderung an den primären Endpunkt fehlschlägt. In der Beispielanwendung wird in der `retry_callback`-Funktion des Storage-Objekts eine benutzerdefinierte Wiederholungsrichtlinie definiert.

Vor dem Herunterladen werden die Funktionen [retry_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) und [response_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) für das Service-Objekt definiert. Mit diesen Funktionen werden die Ereignishandler definiert, die aufgerufen werden, wenn ein Download erfolgreich abgeschlossen wurde oder wenn ein Download fehlschlägt und erneut versucht wird, den Download durchzuführen.  

# <a name="java-v7-sdk-tabjava-v7"></a>[Java V7 SDK] (#tab/java-v7)

Sie können die Anwendung ausführen, indem Sie ein Terminal oder eine Eingabeaufforderung für den heruntergeladenen Anwendungsordner öffnen. Geben Sie dort `mvn compile exec:java` ein, um die Anwendung auszuführen. Daraufhin lädt die Anwendung das Bild **HelloWorld.png** aus dem Verzeichnis in Ihr Speicherkonto hoch und überprüft, ob es am sekundären RA-GRS-Endpunkt repliziert wurde. Nach dieser Überprüfung beginnt die Anwendung mit dem wiederholten Herunterladen des Bilds und meldet dabei jeweils den Endpunkt, von dem das Bild heruntergeladen wird.

Die retry-Funktion des Storage-Objekts ist auf eine lineare Wiederholungsrichtlinie festgelegt. Die retry-Funktion bestimmt, ob für eine Anforderung ein Wiederholungsversuch durchgeführt und wie viele Sekunden zwischen den einzelnen Versuchen gewartet werden soll. Die Eigenschaft **LocationMode** von **BlobRequestOptions** ist auf **PRIMARY\_THEN\_SECONDARY** festgelegt. Dadurch kann die Anwendung automatisch auf den sekundären Standort ausweichen, wenn sie den primären Standort beim Herunterladen von **HelloWorld.png** nicht erreicht.

# <a name="java-v10-sdk-tabjava-v10"></a>[Java V10 SDK] (#tab/java-v10)

Führen Sie das Beispiel mithilfe von Maven über die Befehlszeile aus.

1. Öffnen Sie eine Shell, und navigieren Sie im geklonten Verzeichnis zu **storage-blobs-java-v10-quickstart**.
2. Geben Sie `mvn compile exec:java` ein.

In diesem Beispiel wird eine Testdatei im Standardverzeichnis erstellt. Für Windows-Benutzer ist dies das Verzeichnis **AppData\Local\Temp**. Im Beispiel werden dann die folgenden Optionen für Befehle angezeigt, die Sie eingeben können:

- Geben Sie **P** ein, um einen „Put Blob“-Vorgang zum Hochladen einer temporären Datei in Ihr Speicherkonto auszuführen.
- Geben Sie **L** ein, um einen „List Blob“-Vorgang zum Auflisten der aktuell im Container enthaltenen Blobs auszuführen.
- Geben Sie **G** ein, um einen „Get Blob“-Vorgang zum Herunterladen einer Datei aus Ihrem Speicherkonto auf den lokalen Computer auszuführen.
- Geben Sie **D** ein, um einen „Delete Blob“-Vorgang zum Löschen des Blobs aus Ihrem Speicherkonto auszuführen.
- Geben Sie **E** ein, um das Beispiel zu schließen und gleichzeitig alle Ressourcen zu löschen, die im Rahmen des Beispiels erstellt wurden.

In diesem Beispiel ist die Ausgabe gezeigt, wenn Sie die Anwendung unter Windows ausführen.

```
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

Da Sie die Kontrolle über das Beispiel haben, geben Sie Befehle ein, damit der Code ausgeführt wird. Bei Eingaben muss die Groß-/Kleinschreibung beachtet werden.

---

## <a name="understand-the-sample-code"></a>Grundlagen des Beispielcodes

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

### <a name="retry-event-handler"></a>Ereignishandler für erneuten Downloadversuch

Der Ereignishandler `OperationContextRetrying` wird aufgerufen und für einen Neuversuch festgelegt, wenn es beim Herunterladen des Bilds zu einem Fehler kommt. Wenn die maximale Anzahl von Wiederholungen erreicht ist, die in der Anwendung definiert ist, wird die [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode)-Eigenschaft der Anforderung in `SecondaryOnly` geändert. Durch diese Einstellung versucht die Anwendung, das Bild vom sekundären Endpunkt herunterzuladen. Diese Konfiguration reduziert die Anforderungszeit für das Bild, da die Anzahl der wiederholten Anforderungen an den primären Endpunkt begrenzt ist.
 
```csharp
private static void OperationContextRetrying(object sender, RequestEventArgs e)
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

Der Ereignishandler `OperationContextRequestCompleted` wird aufgerufen, wenn der Download des Bilds erfolgreich ist. Wenn die Anwendung den sekundären Endpunkt verwendet, stellt die Anwendung weiterhin bis zu 20 Anforderungen an diesen Endpunkt. Nach 20 Anforderungen legt die Anwendung für die [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode)-Eigenschaft auf den Wert `PrimaryThenSecondary` zurück und versucht erneut, Anforderungen an den primären Endpunkt zu senden. Wenn eine Anforderung erfolgreich ist, liest die Anwendung weiterhin aus dem primären Endpunkt.

```csharp
private static void OperationContextRequestCompleted(object sender, RequestEventArgs e)
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

# <a name="python-tabpython"></a>[Python] (#tab/python) 

### <a name="retry-event-handler"></a>Ereignishandler für erneuten Downloadversuch

Der Ereignishandler `retry_callback` wird aufgerufen und für einen Neuversuch festgelegt, wenn es beim Herunterladen des Bilds zu einem Fehler kommt. Wenn die maximale Anzahl von Wiederholungen erreicht ist, die in der Anwendung definiert ist, wird die [LocationMode](https://docs.microsoft.com/python/api/azure.storage.common.models.locationmode?view=azure-python)-Eigenschaft der Anforderung in `SECONDARY` geändert. Durch diese Einstellung versucht die Anwendung, das Bild vom sekundären Endpunkt herunterzuladen. Diese Konfiguration reduziert die Anforderungszeit für das Bild, da die Anzahl der wiederholten Anforderungen an den primären Endpunkt begrenzt ist.  

```python
def retry_callback(retry_context):
    global retry_count
    retry_count = retry_context.count
    sys.stdout.write("\nRetrying event because of failure reading the primary. RetryCount= {0}".format(retry_count))
    sys.stdout.flush()

    # Check if we have more than n-retries in which case switch to secondary
    if retry_count >= retry_threshold:

        # Check to see if we can fail over to secondary.
        if blob_client.location_mode != LocationMode.SECONDARY:
            blob_client.location_mode = LocationMode.SECONDARY
            retry_count = 0
        else:
            raise Exception("Both primary and secondary are unreachable. "
                            "Check your application's network connection.")
```

### <a name="request-completed-event-handler"></a>Ereignishandler für abgeschlossene Anforderung

Der Ereignishandler `response_callback` wird aufgerufen, wenn der Download des Bilds erfolgreich ist. Wenn die Anwendung den sekundären Endpunkt verwendet, stellt die Anwendung weiterhin bis zu 20 Anforderungen an diesen Endpunkt. Nach 20 Anforderungen legt die Anwendung für die [LocationMode](https://docs.microsoft.com/python/api/azure.storage.common.models.locationmode?view=azure-python)-Eigenschaft auf den Wert `PRIMARY` zurück und versucht erneut, Anforderungen an den primären Endpunkt zu senden. Wenn eine Anforderung erfolgreich ist, liest die Anwendung weiterhin aus dem primären Endpunkt.

```python
def response_callback(response):
    global secondary_read_count
    if blob_client.location_mode == LocationMode.SECONDARY:

        # You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        # then switch back to the primary and see if it is available now.
        secondary_read_count += 1
        if secondary_read_count >= secondary_threshold:
            blob_client.location_mode = LocationMode.PRIMARY
            secondary_read_count = 0
```

# <a name="java-v7-sdk--tabjava-v7"></a>[Java V7 SDK ] (#tab/java-v7)

Bei Verwendung von Java müssen keine Rückrufhandler definiert werden, wenn die Eigenschaft **LocationMode** von **BlobRequestOptions** auf **PRIMARY\_THEN\_SECONDARY** festgelegt ist. Dadurch kann die Anwendung automatisch auf den sekundären Standort ausweichen, wenn sie den primären Standort beim Herunterladen von **HelloWorld.png** nicht erreicht.

```java
    BlobRequestOptions myReqOptions = new BlobRequestOptions();
    myReqOptions.setRetryPolicyFactory(new RetryLinearRetry(deltaBackOff, maxAttempts));
    myReqOptions.setLocationMode(LocationMode.PRIMARY_THEN_SECONDARY);
    blobClient.setDefaultRequestOptions(myReqOptions);

    blob.downloadToFile(downloadedFile.getAbsolutePath(),null,blobClient.getDefaultRequestOptions(),opContext);
```

# <a name="java-v10-sdk-tabjava-v10"></a>[Java V10 SDK] (#tab/java-v10)

Beim Java V10 SDK ist das Definieren von Rückrufhandlern nach wie vor nicht erforderlich. Zudem unterscheidet sich das SDK jetzt in einigen Punkten grundlegend vom V7 SDK. Anstelle von „LocationMode“ wird jetzt eine sekundäre **Pipeline** verwendet. Eine sekundäre Pipeline kann über die **RequestRetryOptions** definiert werden. Ist sie definiert, kann die Anwendung automatisch zur sekundären Pipeline wechseln, wenn sie Ihre Daten nicht über die primäre Pipeline erreichen kann.

```java
// We create pipeline options here so that they can be easily used between different pipelines
PipelineOptions myOptions = new PipelineOptions();
myOptions.withRequestRetryOptions(new RequestRetryOptions(RetryPolicyType.EXPONENTIAL, 3, 10, 500L, 1000L, accountName + "-secondary.blob.core.windows.net"));
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("https://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, myOptions));
```
---

## <a name="next-steps"></a>Nächste Schritte

Im ersten Teil dieser Reihe haben Sie erfahren, wie Sie mit RA-GRS-Speicherkonten Hochverfügbarkeit für eine Anwendung herstellen.

Im zweiten Teil der Reihe erfahren Sie, wie Sie einen Fehler simulieren, durch den Ihre Anwendung auf den sekundären RA-GRS-Endpunkt ausweichen muss.

> [!div class="nextstepaction"]
> [Simulate a failure in connection to your primary storage endpoint (Simulieren eines Fehlers bei einem primären Speicherendpunkt)](storage-simulate-failure-ragrs-account-app.md)