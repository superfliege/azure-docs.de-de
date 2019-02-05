---
title: Verwenden eines benutzerdefinierten NuGet-Feeds in Azure Dev Spaces | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: johnsta
ms.author: johnsta
ms.date: 05/11/2018
ms.topic: article
description: Verwenden Sie einen benutzerdefinierten NuGet-Feed für den Zugriff auf und die Verwendung von NuGet-Paketen in Azure Dev Spaces.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, Container
manager: ghogen
ms.openlocfilehash: 21a70100fe186e176dfe8eb7c247d83a5d4705bd
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55466403"
---
#  <a name="use-a-custom-nuget-feed-in-an-azure-dev-space"></a>Verwenden eines benutzerdefinierten NuGet-Feeds in Azure Dev Spaces

Ein NuGet-Feed stellt eine einfache Möglichkeit dar, Paketquellen zu einem Projekt hinzuzufügen. Azure Dev Spaces muss auf diesen Feed zugreifen können, damit die Abhängigkeiten im Docker-Container korrekt installiert werden.

## <a name="set-up-a-nuget-feed"></a>Einrichten eines NuGet-Feeds

So richten Sie einen NuGet-Feed ein:
1. Fügen Sie einen [Paketverweis](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) zur `*.csproj`-Datei unter dem `PackageReference`-Knoten hinzu.

   ```xml
   <ItemGroup>
       <!-- ... -->
       <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
       <!-- ... -->
   </ItemGroup>
   ```

2. Erstellen Sie eine [NuGet.Config](https://docs.microsoft.com/nuget/reference/nuget-config-file)-Datei im Projektordner.
     * Verwenden Die den Abschnitt `packageSources`, um auf den Speicherort Ihres NuGet-Feeds zu verweisen. Wichtig: Der NuGet-Feed muss öffentlich zugänglich sein.
     * Verwenden Sie den Abschnitt `packageSourceCredentials`, um die Anmeldeinformationen (Benutzernamen und Kennwort) zu konfigurieren. 

   ```xml
   <packageSources>
       <add key="Contoso" value="https://contoso.com/packages/" />
   </packageSources>

   <packageSourceCredentials>
       <Contoso>
           <add key="Username" value="user@contoso.com" />
           <add key="ClearTextPassword" value="33f!!lloppa" />
       </Contoso>
   </packageSourceCredentials>
   ```

3. Wenn Sie Quellcodeverwaltung verwenden:
    - Verweisen Sie auf `NuGet.Config` in Ihrer `.gitignore`-Datei, damit Sie nicht versehentlich Anmeldeinformationen in Ihr Quellrepository committen.
    - Öffnen Sie die `azds.yaml`-Datei in Ihrem Projekt, suchen Sie den Abschnitt `build`, und fügen Sie den folgenden Codeausschnitt hinzu, um sicherzustellen, dass die `NuGet.Config`-Datei mit Azure synchronisiert wird und so während des Containerimage-Buildprozesses verwendet werden kann. (Standardmäßig synchronisiert Azure Dev Spaces keine Dateien, auf die `.gitignore`- und `.dockerignore`-Regeln zutreffen.)

        ```yaml
        build:
        useGitIgnore: true
        ignore:
        - “!NuGet.Config”
        ```


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die oben genannten Schritte ausgeführt haben und `azds up` das nächste Mal ausführen (oder in VS Code oder Visual Studio das nächste Mal `F5` drücken), synchronisiert Azure Dev Spaces die `NuGet.Config`-Datei mit Azure, die dann von `dotnet restore` zum Installieren der Paketabhängigkeiten im Container verwendet wird.

