---
title: ".NET SDK: Dateisystemvorgänge in Azure Data Lake Store | Microsoft-Dokumentation"
description: "Verwenden Sie das Azure Data Lake Store .NET SDK, um Dateisystemvorgänge in Data Lake Store durchzuführen, z.B. das Erstellen von Ordnern usw."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2017
ms.author: nitinme
ms.openlocfilehash: a5d446986f810993d65c7e73eb95eeb2283c39a3
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2017
---
# <a name="filesystem-operations-on-azure-data-lake-store-using-net-sdk"></a>Dateisystemvorgänge in Azure Data Lake Store per .NET SDK
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
>

In diesem Artikel erfahren Sie, wie Sie Dateisystemvorgänge in Data Lake Store per .NET SDK durchführen. Zu den Dateisystemvorgängen gehören das Erstellen von Ordnern in einem Data Lake Store-Konto, das Hochladen von Dateien, das Herunterladen von Dateien usw.

Eine Anleitung zum Durchführen von Kontoverwaltungsvorgängen in Data Lake Store per .NET SDK finden Sie unter [Erste Schritte mit Azure Data Lake Store mithilfe des .NET SDK](data-lake-store-get-started-net-sdk.md).

## <a name="prerequisites"></a>Voraussetzungen
* **Visual Studio 2013, 2015 oder 2017** In der Anleitung unten wird Visual Studio 2017 verwendet.

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake-Speicherkonto**. Eine Anleitung zur Erstellung eines Kontos finden Sie unter [Erste Schritte mit Azure Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-get-started-portal.md)

## <a name="create-a-net-application"></a>Erstellen einer .NET-Anwendung
Das auf [GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) verfügbare Codebeispiel veranschaulicht Schritt für Schritt den Prozess zum Erstellen der Dateien im Speicher, zum Verketten von Dateien, zum Herunterladen einer Datei und zum Löschen einiger Dateien aus dem Speicher. In diesem Abschnitt des Artikels werden die wichtigsten Teile des Codes beschrieben.

1. Öffnen Sie Visual Studio, und erstellen Sie eine Konsolenanwendung.
2. Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**.
3. Unter **Neues Projekt** können Sie die folgenden Werte eingeben bzw. auswählen:

   | Eigenschaft | Wert |
   | --- | --- |
   | Kategorie |Vorlagen/Visual C#/Windows |
   | Vorlage |Konsolenanwendung |
   | Name |CreateADLApplication |

4. Klicken Sie auf **OK** , um das Projekt zu erstellen.

5. Fügen Sie Ihrem Projekt die NuGet-Pakete hinzu.

   1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen und dann auf **NuGet-Pakete verwalten**.
   2. Stellen Sie auf der Registerkarte **NuGet-Paket-Manager** sicher, dass **Paketquelle** auf **nuget.org** festgelegt und das Kontrollkästchen **Vorabversion einbeziehen** aktiviert ist.
   3. Suchen und installieren Sie die folgenden NuGet-Pakete:

      * `Microsoft.Azure.DataLake.Store`: In diesem Tutorial wird „v1.0.0“ verwendet.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication`: In diesem Tutorial wird „v2.3.1“ verwendet.
    
    Schließen Sie den **NuGet-Paket-Manager**.

6. Öffnen Sie **Program.cs**, löschen Sie den vorhandenen Code, und fügen Sie dann die folgenden Anweisungen ein, um Verweise auf Namespaces hinzuzufügen.

        using System;
        using System.IO;using System.Threading;
        using System.Linq;
        using System.Text;
        using System.Collections.Generic;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
                
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.DataLake.Store;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

7. Deklarieren Sie die Variablen wie unten dargestellt, und geben Sie die Werte für die Platzhalter an. Stellen Sie außerdem sicher, dass der hier angegebene lokale Pfad und der Dateiname auf dem Computer vorhanden sind.

        namespace SdkSample
        {
            class Program
            {
                private static string _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; //Replace this value with the name of your existing Data Lake Store account.        
            }
        }

In den restlichen Abschnitten dieses Artikels erfahren Sie, wie Sie die verfügbaren .NET-Methoden verwenden, um Vorgänge wie Authentifizierung, Dateiupload usw. durchzuführen.

## <a name="authentication"></a>Authentifizierung

* Informationen zur Authentifizierung von Endbenutzern für Ihre Anwendung finden Sie unter [End-user authentication with Data Lake Store using .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md) (Authentifizierung von Endbenutzern mit Data Lake Store per .NET SDK).
* Informationen zur Dienst-zu-Dienst-Authentifizierung für Ihre Anwendung finden Sie unter [Service-to-service authentication with Data Lake Store using .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md) (Dienst-zu-Dienst-Authentifizierung mit Data Lake Store per .NET SDK).


## <a name="create-client-object"></a>Erstellen des Clientobjekts
Mit dem folgenden Codeausschnitt wird das Data Lake Store-Dateisystem-Clientobjekt erstellt, das zum Ausführen von Anforderungen für den Dienst genutzt wird.

    // Create client objects
    AdlsClient client = AdlsClient.CreateClient(_adlsAccountName, adlCreds);

## <a name="create-a-file-and-directory"></a>Erstellen einer Datei und eines Verzeichnisses
Fügen Sie Ihrer Anwendung den unten angegebenen Codeausschnitt hinzu. Mit diesem Codeausschnitt werden eine Datei und ggf. nicht vorhandene übergeordnete Verzeichnisse hinzugefügt.

    // Create a file - automatically creates any parent directories that don't exist
    
    string fileName = "/Test/testFilename1.txt";
    using (var streamWriter = new StreamWriter(client.CreateFile(fileName, IfExists.Overwrite)))
    {
        streamWriter.WriteLine("This is test data to write");
        streamWriter.WriteLine("This is line 2");
    }

## <a name="append-to-a-file"></a>Anfügen an eine Datei
Mit dem folgenden Codeausschnitt werden Daten an eine vorhandene Datei im Data Lake Store-Konto angefügt.

    // Append to existing file
    using (var streamWriter = new StreamWriter(client.GetAppendStream(fileName)))
    {
        streamWriter.WriteLine("This is the added line");
    }

## <a name="read-a-file"></a>Lesen einer Datei
Mit dem folgenden Codeausschnitt wird der Inhalt einer Datei in Data Lake Store gelesen:

    //Read file contents
    using (var readStream = new StreamReader(client.GetReadStream(fileName)))
    {
        string line;
        while ((line = readStream.ReadLine()) != null)
        {
            Console.WriteLine(line);
        }
    }

## <a name="get-file-properties"></a>Abrufen von Dateieigenschaften
Mit dem folgenden Codeausschnitt werden die Eigenschaften zurückgegeben, die einer Datei oder einem Verzeichnis zugeordnet sind:

    // Get file properties
    var directoryEntry = client.GetDirectoryEntry(fileName);
    PrintDirectoryEntry(directoryEntry);

Die Definition der `PrintDirectoryEntry`-Methode ist als Teil des Beispiels [auf GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) verfügbar. 

## <a name="rename-a-file"></a>Umbenennen einer Datei
Im folgenden Codeausschnitt wird eine vorhandene Datei in einem Data Lake Store-Konto umbenannt.

    // Rename a file
    string destFilePath = "/Test/testRenameDest3.txt";
    client.Rename(fileName, destFilePath, true);

## <a name="enumerate-a-directory"></a>Aufzählen eines Verzeichnisses
Mit dem folgenden Codeausschnitt werden Verzeichnisse in einem Data Lake Store-Konto aufgezählt:

    // Enumerate directory
    foreach (var entry in client.EnumerateDirectory("/Test"))
    {
        PrintDirectoryEntry(entry);
    }

Die Definition der `PrintDirectoryEntry`-Methode ist als Teil des Beispiels [auf GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) verfügbar.

## <a name="delete-directories-recursively"></a>Rekursives Löschen von Verzeichnissen
Mit dem folgenden Codeausschnitt werden ein Verzeichnis und alle Unterverzeichnisse rekursiv gelöscht.

    // Delete a directory and all it's subdirectories and files
    client.DeleteRecursive("/Test");

## <a name="samples"></a>Beispiele
Hier sind zwei Beispiele für die Nutzung des Data Lake Store-Dateisystem-SDK angegeben.
* [Einfaches Beispiel auf GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)
* [Erweitertes Beispiel auf GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-samples)

## <a name="see-also"></a>Weitere Informationen
* [Kontoverwaltungsvorgänge in Azure Data Lake Store mit dem .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Data Lake Store .NET SDK Reference (Referenz zum Data Lake Store .NET SDK)](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Nächste Schritte
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)
