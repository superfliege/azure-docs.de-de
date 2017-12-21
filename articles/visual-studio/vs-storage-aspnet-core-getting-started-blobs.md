---
title: Erste Schritte mit Azure-Blobspeicher und verbundenen Visual Studio-Diensten (ASP.NET Core) | Microsoft-Dokumentation
description: "Hier erfahren Sie etwas über die ersten Schritte mit Azure-Blobspeicher in einem ASP.NET Core-Projekt in Visual Studio, nachdem Sie mithilfe von verbundenen Visual Studio-Diensten eine Verbindung mit einem Speicherkonto hergestellt haben."
services: storage
documentationcenter: 
author: camsoper
manager: wpickett
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: casoper
ms.openlocfilehash: 889afa471eeb556662cddf8eb383a905f08b1f01
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Erste Schritte mit Azure-Blobspeicher und verbundenen Visual Studio-Diensten (ASP.NET Core)

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [ASP.NET Core](./vs-storage-aspnet-core-getting-started-blobs.md)

Der Azure-Blobspeicher ist ein Dienst, bei dem unstrukturierte Daten in der Cloud als Objekte/Blobs gespeichert werden. In Blob Storage können alle Arten von Text- oder Binärdaten gespeichert werden, z. B. ein Dokument, eine Mediendatei oder ein Installer einer Anwendung. Der Blobspeicher wird auch als Objektspeicher bezeichnet.

In diesem Tutorial wird gezeigt, wie Sie ASP.NET Core-Code für einige häufig verwendete Szenarien mit Azure-Blobspeicher schreiben. Zu den Szenarien zählen das Erstellen eines Blobcontainers sowie das Hochladen, Auflisten, Herunterladen und Löschen von Blobs.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

In diesem Abschnitt wird die Einrichtung Ihrer Entwicklungsumgebung erläutert. Diese umfasst das Erstellen einer ASP.NET MVC-App, das Hinzufügen einer Verbindung mit verbundenen Diensten, das Hinzufügen eines Controllers und das Angeben der erforderlichen Namespace-Direktiven.

### <a name="create-an-aspnet-mvc-app-project"></a>Erstellen einer ASP.NET MVC-App

1. Öffnen Sie Visual Studio.

1. Wählen Sie im Hauptmenü **Datei->Neu->Projekt**.

1. Legen Sie im Dialogfeld **Neues Projekt** die in der folgenden Abbildung hervorgehobenen Optionen fest:

    ![Erstellen eines ASP.NET Core-Projekts](./media/vs-storage-aspnet-core-getting-started-blobs/new-project.png)

1. Klicken Sie auf **OK**.

1. Legen Sie im Dialogfeld **Neues ASP.NET-Projekt** die in der folgenden Abbildung hervorgehobenen Optionen fest:

    ![Angeben von MVC](./media/vs-storage-aspnet-core-getting-started-blobs/new-mvc.png)

1. Klicken Sie auf **OK**.

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Herstellen einer Verbindung mit einem Azure-Speicherkonto mithilfe von Verbundene Dienste

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü **Hinzufügen->Verbundener Dienst** aus.

1. Wählen Sie im Dialogfeld **Verbundenen Dienst hinzufügen** die Option **Cloudspeicher mit Azure Storage** aus, und klicken Sie dann auf **Konfigurieren**.

    ![Dialogfeld „Verbundener Dienst“](./media/vs-storage-aspnet-core-getting-started-blobs/connected-services.png)

1. Wählen Sie im Dialogfeld **Azure Storage** das Azure Storage-Konto aus, das für dieses Tutorial verwendet werden soll.  Klicken Sie zum Erstellen eines neuen Azure Storage-Kontos auf **Neues Speicherkonto erstellen**, und füllen Sie das Formular aus.  Klicken Sie auf **Hinzufügen**, nachdem Sie entweder ein vorhandenes Speicherkonto ausgewählt oder ein neues Speicherkonto erstellt haben.  Visual Studio installiert das NuGet-Paket für Azure Storage und eine Speicherverbindungszeichenfolge in **appsettings.json**.

> [!TIP]
> Informationen zum Erstellen eines Speicherkontos mit dem [Azure-Portal](https://portal.azure.com) finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md#create-a-storage-account).
>
> Ein Azure-Speicherkonto kann auch mit [Azure PowerShell](../storage/common/storage-powershell-guide-full.md), der [Azure CLI](../storage/common/storage-azure-cli.md) oder [Azure Cloud Shell](../cloud-shell/overview.md) erstellt werden.


### <a name="create-an-mvc-controller"></a>Erstellen eines MVC-Controllers 

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Controller**, und wählen Sie im Kontextmenü **Hinzufügen > Controller** aus.

    ![Hinzufügen eines Controllers zu einer ASP.NET Core MVC-App](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller-menu.png)

1. Wählen Sie im Dialogfeld **Gerüst hinzufügen** die Option **MVC-Controller – leer** und dann **Hinzufügen**.

    ![Angeben des MVC-Controllertyps](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller.png)

1. Benennen Sie den Controller im Dialogfeld **Controller hinzufügen** mit *BlobsController*, und wählen Sie **Hinzufügen**.

    ![Benennen des MVC-Controllers](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller-name.png)

1. Fügen Sie die folgenden *using*-Anweisungen der `BlobsController.cs`-Datei hinzu:

    ```csharp
    using System.IO;
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Herstellen einer Verbindung mit einem Speicherkonto und Abrufen eines Containerverweises

Ein Blobcontainer ist eine geschachtelte Hierarchie von Blobs und Ordnern.  Für die übrigen Schritte in diesem Dokument ist ein Verweis auf einen Blobcontainer erforderlich, daher muss der Code in einer eigenen Methode platziert werden, um wiederverwendet werden zu können.

Mit den folgenden Schritten wird eine Methode erstellt, um mithilfe der Verbindungszeichenfolge in **appsettings.json** eine Verbindung mit dem Speicherkonto herzustellen und einen Verweis auf einen Container zu erstellen.  Die Einstellung der Verbindungszeichenfolge in **appsettings.json** erhält das Format `<storageaccountname>_AzureStorageConnectionString`. 

1. Öffnen Sie die Datei `BlobsController.cs` .

1. Fügen Sie eine Methode mit dem Namen **GetCloudBlobContainer** hinzu, die ein **CloudBlobContainer**-Element zurückgibt.  Ersetzen Sie `<storageaccountname>_AzureStorageConnectionString` durch den tatsächlichen Namen des Schlüssels in **Web.config**.
    
    ```csharp
    private CloudBlobContainer GetCloudBlobContainer()
    {
        var builder = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("appsettings.json");
        IConfigurationRoot Configuration = builder.Build();
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.Parse(Configuration["ConnectionStrings:aspnettutorial_AzureStorageConnectionString"]);
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
        return container;
    }
    ```

> [!NOTE]
> Obwohl *test-blob-container* noch nicht vorhanden ist, erstellt dieser Code einen Verweis darauf, sodass der Container mit der im nächsten Schritt gezeigten `CreateIfNotExists`-Methode erstellt werden kann.

## <a name="create-a-blob-container"></a>Erstellen eines Blobcontainers

Die folgenden Schritte veranschaulichen, wie Sie einen Blobcontainer erstellen:

1. Fügen Sie eine Methode namens `CreateBlobContainer` hinzu, die `ActionResult` zurückgibt.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Rufen Sie ein `CloudBlobContainer`-Objekt ab, das einen Verweis auf den gewünschten Blobcontainernamen darstellt. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Rufen Sie die `CloudBlobContainer.CreateIfNotExists`-Methode auf, um den Container zu erstellen, wenn er noch nicht vorhanden ist. Die `CloudBlobContainer.CreateIfNotExists`-Methode gibt **true** zurück, wenn der Container nicht vorhanden war und erfolgreich erstellt wurde. Andernfalls wird **false** zurückgegeben.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExistsAsync().Result;
    ```

1. Aktualisieren Sie `ViewBag` mit dem Namen des Blobcontainers.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```
    
    Der folgende Code zeigt die abgeschlossene `CreateBlobContainer`-Methode:

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        ViewBag.Success = container.CreateIfNotExistsAsync().Result;
        ViewBag.BlobContainerName = container.Name;

        return View();
    }
    ```

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Ansichten**, und wählen Sie im Kontextmenü **Hinzufügen > Neuer Ordner** aus. Nennen Sie den neuen Ordner *Blobs*. 

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Ansichten**, klicken Sie mit der rechten Maustaste auf **Blobs**, und wählen Sie im Kontextmenü **Hinzufügen > Ansicht** aus.

1. Geben Sie im Dialogfeld **Ansicht hinzufügen** für den Ansichtsnamen **CreateBlobContainer** ein, und wählen Sie **Hinzufügen**.

1. Öffnen Sie `CreateBlobContainer.cshtml`, und ändern Sie die Datei so, dass sie folgendem Codeausschnitt entspricht:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>
    
    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Views > Shared**, und öffnen Sie `_Layout.cshtml`.

1. Suchen Sie nach der unsortierten Liste, die wie folgt aussieht: `<ul class="nav navbar-nav">`.  Fügen Sie nach dem letzten `<li>`-Element in der Liste den folgenden HTML-Code ein, um ein weiteres Navigationsmenüelement hinzuzufügen:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="CreateBlobContainer">Create blob container</a></li>
    ```

1. Führen Sie die Anwendung aus, und wählen Sie **Blobcontainer erstellen**, um ähnliche Ergebnisse wie im folgenden Screenshot zu sehen:
  
    ![Erstellen eines Blobcontainers](./media/vs-storage-aspnet-core-getting-started-blobs/create-blob-container-results.png)

    Wie bereits erwähnt, gibt die `CloudBlobContainer.CreateIfNotExists`-Methode nur dann **true** zurück, wenn der Container nicht vorhanden ist und erstellt wird. Aus diesem Grund gibt die Methode **false** zurück, wenn die App bei vorhandenem Container ausgeführt wird.

## <a name="upload-a-blob-into-a-blob-container"></a>Hochladen eines Blobs in einen Blobcontainer

Sobald der [Blobcontainer erstellt wurde](#create-a-blob-container), können Sie Dateien in diesen Container hochladen. Dieser Abschnitt führt Sie durch die Schritte zum Hochladen einer lokalen Datei in einen Blobcontainer. Die Schritte setzen voraus, dass ein Blobcontainer namens *test-blob-container* vorhanden ist. 

1. Öffnen Sie die Datei `BlobsController.cs` .

1. Fügen Sie eine Methode namens `UploadBlob` hinzu, die eine Zeichenfolge zurückgibt.

    ```csharp
    public string UploadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. Rufen Sie in der `UploadBlob`-Methode ein `CloudBlobContainer`-Objekt ab, das einen Verweis auf den gewünschten Blobcontainernamen darstellt. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Wie bereits erwähnt, unterstützt Azure-Speicher verschiedene Blobtypen. In diesem Tutorial werden Block-BLOBs verwendet.  Rufen Sie die `CloudBlobContainer.GetBlockBlobReference`-Methode auf, um einen Verweis auf einen Blockblob abzurufen.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > Der Blobname ist Teil der URL, die zum Abrufen eines Blobs verwendet wird. Er kann eine beliebige Zeichenfolge (einschließlich des Dateinamens) sein.

1. Sobald ein Blobverweis vorhanden ist, können Sie jeden Datenstrom in diesen hochladen, indem Sie die `UploadFromStream`-Methode des Blobverweisobjekts aufrufen. Die `UploadFromStream`-Methode erstellt das Blob, falls es nicht vorhanden ist, oder überschreibt es, falls es vorhanden ist. (Ändern Sie *&lt;file-to-upload>* in einen vollqualifizierten Pfad zu einer Datei, die hochgeladen werden soll.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(@"<file-to-upload>"))
    {
        blob.UploadFromStreamAsync(fileStream).Wait();
    }
    ```
    
    Das folgende Beispiel zeigt die abgeschlossene `UploadBlob`-Methode (mit einem vollqualifizierten Pfad für die hochzuladende Datei):

    ```csharp
    public string UploadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenRead(@"c:\src\sample.txt"))
        {
            blob.UploadFromStreamAsync(fileStream).Wait();
        }
        return "success!";
    }
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Views > Shared**, und öffnen Sie `_Layout.cshtml`.

1. Fügen Sie nach dem letzten `<li>`-Element in der Liste den folgenden HTML-Code ein, um ein weiteres Navigationsmenüelement hinzuzufügen:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="UploadBlob">Upload blob</a></li>
    ```

1. Führen Sie die Anwendung aus, und wählen Sie **Blob hochladen**.  Das Wort „success!“ sollte angezeigt werden.
    
    ![Erfolgsmeldung](./media/vs-storage-aspnet-core-getting-started-blobs/upload-blob.png)
  
Dieser Abschnitt – [Auflisten von Blobs in einem Blobcontainer](#list-the-blobs-in-a-blob-container) – veranschaulicht das Auflisten von Blobs in einem Blobcontainer.    

## <a name="list-the-blobs-in-a-blob-container"></a>Auflisten von Blobs in einem Blobcontainer

In diesem Abschnitt wird das Auflisten von Blobs in einem Blobcontainer veranschaulicht. Der Beispielcode verweist auf den *test-blob-container*, der im Abschnitt [Erstellen eines Blobcontainers](#create-a-blob-container) erstellt wurde.

1. Öffnen Sie die Datei `BlobsController.cs` .

1. Fügen Sie eine Methode namens `ListBlobs` hinzu, die `ActionResult` zurückgibt.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

    }
    ```
 
1. Rufen Sie in der `ListBlobs`-Methode ein `CloudBlobContainer`-Objekt ab, das einen Verweis auf den Blobcontainer darstellt. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```
   
1. Verwenden Sie zum Auflisten der Blobs in einem Blobcontainer die Methode `CloudBlobContainer.ListBlobsSegmentedAsync`. Die Methode `CloudBlobContainer.ListBlobsSegmentedAsync` gibt ein `BlobResultSegment`-Element zurück, das `IListBlobItem`-Objekte enthält, die in `CloudBlockBlob`-, `CloudPageBlob`- oder `CloudBlobDirectory`-Objekte umgewandelt werden können. Der folgende Codeausschnitt listet alle Blobs im Blobcontainer auf. Jedes Blob wird dem Typ gemäß in das entsprechende Objekt umgewandelt, und sein Name (oder URI im Fall von `CloudBlobDirectory`) wird einer Liste hinzugefügt.

    ```csharp
    List<string> blobs = new List<string>();
    BlobResultSegment resultSegment = container.ListBlobsSegmentedAsync(null).Result;
    foreach (IListBlobItem item in resultSegment.Results)
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```
    Der folgende Code zeigt die abgeschlossene `ListBlobs`-Methode:

    ```csharp
    public ActionResult ListBlobs()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        List<string> blobs = new List<string>();
        BlobResultSegment resultSegment = container.ListBlobsSegmentedAsync(null).Result;
        foreach (IListBlobItem item in resultSegment.Results)
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob blob = (CloudPageBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory dir = (CloudBlobDirectory)item;
                blobs.Add(dir.Uri.ToString());
            }
        }

        return View(blobs);
    }
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Ansichten**, klicken Sie mit der rechten Maustaste auf **Blobs**, und wählen Sie im Kontextmenü **Hinzufügen > Ansicht** aus.

1. Geben Sie im Dialogfeld **Ansicht hinzufügen** für den Ansichtsnamen `ListBlobs` ein, und wählen Sie **Hinzufügen**.

1. Öffnen Sie `ListBlobs.cshtml`, und ersetzen Sie den Inhalt durch den folgenden Code:

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
            <li>@item</li>
        }
    </ul>
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Views > Shared**, und öffnen Sie `_Layout.cshtml`.

1. Fügen Sie nach dem letzten `<li>`-Element in der Liste den folgenden HTML-Code ein, um ein weiteres Navigationsmenüelement hinzuzufügen:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="ListBlobs">List blobs</a></li>
    ```

1. Führen Sie die Anwendung aus, und wählen Sie **Blobs auflisten**, um ähnliche Ergebnisse wie im folgenden Screenshot zu sehen:
  
    ![Blobauflistung](./media/vs-storage-aspnet-core-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Herunterladen von Blobs

Dieser Abschnitt veranschaulicht, wie Sie ein Blob herunterladen und entweder im lokalen Speicher beibehalten oder den Inhalt in eine Zeichenfolge lesen. Der Beispielcode verweist auf den *test-blob-container*, der im Abschnitt [Erstellen eines Blobcontainers](#create-a-blob-container) erstellt wurde.

1. Öffnen Sie die Datei `BlobsController.cs` .

1. Fügen Sie eine Methode namens `DownloadBlob` hinzu, die eine Zeichenfolge zurückgibt.

    ```csharp
    public string DownloadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. Rufen Sie in der `DownloadBlob`-Methode ein `CloudBlobContainer`-Objekt ab, das einen Verweis auf den Blobcontainer darstellt.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Rufen Sie ein Blobverweisobjekt durch Aufrufen der `CloudBlobContainer.GetBlockBlobReference`-Methode ab. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Verwenden Sie zum Herunterladen eines Blobs die `CloudBlockBlob.DownloadToStream`-Methode. Im folgenden Code wird der Blobinhalt in ein Datenstromobjekt übertragen, das danach in einer lokalen Datei gespeichert wird. (Ändern Sie *&lt;local-file-name>* in den vollständig qualifizierten Dateinamen, der angibt, wohin das Blob heruntergeladen werden soll.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStreamAsync(fileStream).Wait();
    }
    ```
    
    Das folgende Beispiel zeigt die abgeschlossene `ListBlobs`-Methode (mit einem vollqualifizierten Pfad für die lokale Datei, die erstellt wird):
    
    ```csharp
    public string DownloadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenWrite(@"c:\src\downloadedBlob.txt"))
        {
            blob.DownloadToStreamAsync(fileStream).Wait();
        }
        return "success!";
    }
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Views > Shared**, und öffnen Sie `_Layout.cshtml`.

1. Fügen Sie nach dem letzten `<li>`-Element in der Liste den folgenden HTML-Code ein, um ein weiteres Navigationsmenüelement hinzuzufügen:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="DownloadBlob">Download blob</a></li>
    ```

1. Führen Sie die Anwendung aus, und wählen Sie **Blob herunterladen** zum Herunterladen des Blobs. Das im `CloudBlobContainer.GetBlockBlobReference`-Methodenaufruf angegebene Blob wird an den Speicherort heruntergeladen, den Sie im Aufruf der `File.OpenWrite`-Methode angegeben haben.  Der Text "success!" sollte im Browser angezeigt werden. 

## <a name="delete-blobs"></a>Löschen von Blobs

Die folgenden Schritte veranschaulichen, wie Sie ein Blob löschen:

1. Öffnen Sie die Datei `BlobsController.cs` .

1. Fügen Sie eine Methode namens `DeleteBlob` hinzu, die eine Zeichenfolge zurückgibt.

    ```csharp
    public string DeleteBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```

1. Rufen Sie in der `DeleteBlob`-Methode ein `CloudBlobContainer`-Objekt ab, das einen Verweis auf den Blobcontainer darstellt.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Rufen Sie ein Blobverweisobjekt durch Aufrufen der `CloudBlobContainer.GetBlockBlobReference`-Methode ab. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Verwenden Sie zum Löschen eines Blobs die Methode `Delete`.

    ```csharp
    blob.DeleteAsync().Wait();
    ```
    
    Die abgeschlossene `DeleteBlob`-Methode sollte wie folgt aussehen:
    
    ```csharp
    public string DeleteBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        blob.DeleteAsync().Wait();
        return "success!";
    }
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Views > Shared**, und öffnen Sie `_Layout.cshtml`.

1. Fügen Sie nach dem letzten `<li>`-Element in der Liste den folgenden HTML-Code ein, um ein weiteres Navigationsmenüelement hinzuzufügen:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="DeleteBlob">Delete blob</a></li>
    ```

1. Führen Sie die Anwendung aus, und wählen Sie **Blob löschen** zum Löschen des Blobs, das Sie im Aufruf der `CloudBlobContainer.GetBlockBlobReference`-Methode angegeben haben.  Der Text "success!" sollte im Browser angezeigt werden.  Klicken Sie im Browser auf die Schaltfläche **Zurück**, und klicken Sie dann auf **Blobs auflisten**, um zu bestätigen, dass sich der Blob nicht mehr im Container befindet.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie Blobs in Azure Storage mithilfe von ASP.NET Core speichern, auflisten und abrufen.  Weitere Informationen zu zusätzlichen Optionen für das Speichern von Daten in Azure finden Sie in den anderen Featureleitfäden.

  * [Erste Schritte mit Tabellenspeicher und verbundenen Visual Studio-Diensten (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Erste Schritte mit Azure-Warteschlangenspeicher und verbundenen Visual Studio-Diensten](vs-storage-aspnet-getting-started-queues.md)
