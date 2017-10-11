---
title: "Erste Schritte mit der Azure-CDN-Bibliothek für .NET | Microsoft Docs"
description: Informationen Sie zum Verwalten von Azure-CDN mithilfe von Visual Studio schreiben.
services: cdn
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 63cf4101-92e7-49dd-a155-a90e54a792ca
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5379586355ece98af6295236d6cbd09cb31c742b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-cdn-development"></a>Erste Schritte mit Azure-CDN-Entwicklung
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

Sie können die [Azure CDN-Bibliothek für .NET](https://msdn.microsoft.com/library/mt657769.aspx) zum Automatisieren der Erstellung und Verwaltung von CDN-Profile und Endpunkten.  Dieses Lernprogramm führt Sie durch die Erstellung einer einfachen .NET Konsolenanwendung, die einige der verfügbaren Vorgänge veranschaulicht.  Dieses Lernprogramm ist nicht vorgesehen, um alle Aspekte der Azure-CDN-Bibliothek für .NET ausführlich zu beschreiben.

Sie benötigen Visual Studio 2015, um dieses Lernprogramm abzuschließen.  [Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) kann kostenlos herunterladen.

> [!TIP]
> Die [abgeschlossenes Projekt aus diesem Lernprogramm](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) steht zum Download auf MSDN.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Erstellen des Projekts und Hinzufügen von NuGet-Paketen
Jetzt, wir haben eine Ressourcengruppe für unsere CDN-Profile erstellt und mit der Berechtigung unsere Azure AD-Anwendung zum Verwalten von CDN-Profile und Endpunkte innerhalb dieser Gruppe, können wir mit dem Erstellen der Anwendung beginnen.

Klicken Sie in Visual Studio 2015 auf **Datei**, **neu**, **Projekt...**  um das Dialogfeld "Neues Projekt" zu öffnen.  Erweitern Sie **Visual C#-**, und wählen Sie dann **Windows** im Bereich auf der linken Seite.  Klicken Sie auf **Konsolenanwendung** im mittleren Bereich.  Namen für das Projekt, und klicken Sie auf **OK**.  

![Neues Projekt](./media/cdn-app-dev-net/cdn-new-project.png)

Unser Projekt wird in NuGet-Pakete enthalten einige Azure-Bibliotheken verwenden möchten.  Fügen Sie diese in das Projekt aus.

1. Klicken Sie auf die **Tools** Menü **Nuget Package Manager**, klicken Sie dann **Package Manager Console**.
   
    ![NuGet-Pakete verwalten](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. Führen Sie in der Paket-Manager-Konsole den folgenden Befehl zum Installieren der **Active Directory-Authentifizierungsbibliothek (ADAL)**:
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. Führen Sie die folgenden So installieren Sie die **Azure CDN-Verwaltungsbibliothek**:
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Direktiven, die Konstanten, die main-Methode und die Hilfsmethoden
Erfahren Sie die grundlegende Struktur unsere geschriebenes Programm an.

1. Ersetzen Sie in der Registerkarte "Datei" Program.cs "" die `using` -Direktiven am Anfang durch Folgendes:
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
2. Wir müssen einige Konstanten definieren, die Methoden verwenden.  In der `Program` Klasse, aber vor der `Main` -Methode, fügen Sie Folgendes hinzu.  Achten Sie darauf, dass Sie die Platzhalter, einschließlich ersetzen die  **&lt;spitzen Klammern&gt;**, durch Ihre eigenen Werte nach Bedarf.
   
    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";
   
    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. Auf Klassenebene, müssen Sie auch definieren Sie diese zwei Variablen.  Wir werden diese später verwenden, um festzustellen, ob unsere Profil und ein Endpunkt bereits vorhanden.
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. Ersetzen Sie die `Main` Methode wie folgt:
   
   ```csharp
   static void Main(string[] args)
   {
       //Get a token
       AuthenticationResult authResult = GetAccessToken();
   
       // Create CDN client
       CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
           { SubscriptionId = subscriptionId };
   
       ListProfilesAndEndpoints(cdn);
   
       // Create CDN Profile
       CreateCdnProfile(cdn);
   
       // Create CDN Endpoint
       CreateCdnEndpoint(cdn);
   
       Console.WriteLine();
   
       // Purge CDN Endpoint
       PromptPurgeCdnEndpoint(cdn);
   
       // Delete CDN Endpoint
       PromptDeleteCdnEndpoint(cdn);
   
       // Delete CDN Profile
       PromptDeleteCdnProfile(cdn);
   
       Console.WriteLine("Press Enter to end program.");
       Console.ReadLine();
   }
   ```
5. Einige unserer anderen Methoden sind im Begriff, fordert den Benutzer mit Fragen mit "Ja/Nein".  Fügen Sie die folgende Methode, die etwas vereinfachen:
   
    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Nun, dass die grundlegende Struktur unser Programm geschrieben wird, sollten wir erstellen, die vom aufgerufenen Methoden die `Main` Methode.

## <a name="authentication"></a>Authentifizierung
Bevor wir die Azure-CDN-Management-Bibliothek verwenden können, müssen wir unsere Dienstprinzipal zu authentifizieren und ein Authentifizierungstoken zu erhalten.  Diese Methode verwendet ADAL, um das Token abzurufen.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Bei Verwendung von Benutzerauthentifizierung für einzelne, die `GetAccessToken` Methode sieht etwas anders.

> [!IMPORTANT]
> Verwenden Sie in diesem Codebeispiel nur, wenn Sie einzelne Benutzer-Authentifizierung statt einem Dienstprinzipal auswählen.
> 
> 

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Achten Sie darauf, dass Sie ersetzen `<redirect URI>` mit den umleitungs-URI, die Sie eingegeben haben, wenn Sie die Anwendung in Azure AD registriert.

## <a name="list-cdn-profiles-and-endpoints"></a>CDN-Profile auflisten und Endpunkten
Nun können wir die CDN-Vorgänge ausführen.  Unsere Methode führt zunächst Liste sind alle Profile und Endpunkte in unserer Ressourcengruppe und findet eine Übereinstimmung für die Profile und Endpunkt in unserer Konstanten angegeben macht, die Sie sich für die spätere damit wir versucht nicht, um Duplikate zu erstellen.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Erstellen von CDN-Profile und Endpunkten
Als Nächstes erstellen wir ein Profil.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Sobald das Profil erstellt wurde, erstellen wir einen Endpunkt.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

> [!NOTE]
> Das obige Beispiel weist dem Endpunkt einen Ursprung mit dem Namen *Contoso* mit einem Hostnamen `www.contoso.com`.  Sie sollten diese Option, um eigene Ursprung Hostnamen zeigen ändern.
> 
> 

## <a name="purge-an-endpoint"></a>Löschen Sie einen Endpunkt
Vorausgesetzt, dass der Endpunkt erstellt wurde, ist eine häufige Aufgabe, die wir unser Programm ausführen möchten, möglicherweise den Inhalt in unsere Endpunkt löschen.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

> [!NOTE]
> In der oben gezeigten Beispiel wird die Zeichenfolge `/*` gibt an, dass ich alles in den Stamm des Pfads Endpunkt löschen möchten.  Dies entspricht dem Überprüfung **löschen alle** der Azure-Portal im Dialogfeld "löschen" ". In der `CreateCdnProfile` Methode ich erstellt habe unsere Profil als ein **Azure CDN von Verizon** erstellen ein Profil mit dem Code `Sku = new Sku(SkuName.StandardVerizon)`, sodass dies erfolgreich sein wird.  Allerdings **Azure CDN von Akamai** Profile unterstützen keine **löschen alle**, sodass ich ein Akamai-Profil für dieses Lernprogramm wurde verwendet, ich bestimmte Pfade zu bereinigenden enthalten muss würde.
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a>Löschen von CDN-Profile und Endpunkten
Die letzten Methoden unsere Endpunkt und Profil gelöscht.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Ausführen des Programms
Wir können jetzt kompilieren und führen Sie das Programm, indem Sie auf die **starten** Schaltfläche in Visual Studio.

![Programm ausgeführt wird](./media/cdn-app-dev-net/cdn-program-running-1.png)

Wenn das Programm die oben genannten Meldung erreicht wird, sollten Sie Lage für Ihre Ressourcengruppe im Azure-Portal zurück, und sehen, dass das Profil erstellt wurde.

![War erfolgreich!](./media/cdn-app-dev-net/cdn-success.png)

Wir können die Anweisungen, um den Rest des Programms führen dann bestätigen.

![Programm abschließen](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Nächste Schritte
Um das abgeschlossene Projekt aus dieser exemplarischen Vorgehensweise finden Sie unter [Herunterladen des Beispiels](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

Weiterführendes Dokumentationsmaterial zum Azure CDN Management-Bibliothek für .NET zeigen, um die [-Referenz auf MSDN](https://msdn.microsoft.com/library/mt657769.aspx).

Verwalten Sie Ihre CDN-Ressourcen mit [PowerShell](cdn-manage-powershell.md).

