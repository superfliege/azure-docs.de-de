---
title: Änderungen an einem ASP.NET Core-Projekt beim Herstellen einer Verbindung mit Azure Key Vault | Microsoft-Dokumentation
description: Hier wird beschrieben, was mit dem ASP.NET Core-Projekt geschieht, wenn Sie mithilfe von verbundenen Visual Studio-Diensten eine Verbindung mit Key Vault herstellen.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: 8b6c590344db2997c1a987da14cabba76cdca83b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="what-happened-to-my-aspnet-core-project-visual-studio-key-vault-connected-service"></a>Was ist mit dem ASP.NET Core-Projekt passiert (verbundener Visual Studio-Dienst für Key Vault)?

> [!div class="op_single_selector"]
> - [Erste Schritte](vs-key-vault-aspnet-core-get-started.md)
> - [Was ist passiert?](vs-key-vault-aspnet-core-what-happened.md)

In diesem Artikel werden die genauen Änderungen identifiziert, die an einem ASP.NET-Projekt vorgenommen werden, wenn der [verbundene Key Vault-Dienst mit Visual Studio](vs-key-vault-add-connected-service.md) hinzugefügt wird.

Informationen zur Verwendung des verbundenen Diensts finden Sie unter [Erste Schritte](vs-key-vault-aspnet-core-get-started.md).

## <a name="added-references"></a>Hinzugefügte Verweise

Betrifft die *.NET-Verweise der Projektdatei und NuGet-Verweise.

| Typ | Verweis |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

## <a name="added-files"></a>Hinzugefügte Dateien

- „ConnectedService.json“ wurde hinzugefügt. Enthält Informationen zum Anbieter des verbundenen Diensts und zur Version sowie einen Link zur Dokumentation.

## <a name="project-file-changes"></a>Änderungen an der Projektdatei

- Die Elementgruppe des verbundenen Diensts und die Datei „ConnectedServices.json“ wurden hinzugefügt.

## <a name="launchsettingsjson-changes"></a>Änderungen an „launchsettings.json“

- Dem IIS Express-Profil und dem Profil, das Ihrem Webprojektnamen entspricht, wurde die folgende Umgebungsvariable hinzugefügt:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

## <a name="changes-on-azure"></a>Änderungen in Azure

- Eine Ressourcengruppe wurde erstellt (oder eine bereits vorhandene wurde verwendet).
- Eine Key Vault-Instanz wurde in der angegebenen Ressourcengruppe erstellt.

