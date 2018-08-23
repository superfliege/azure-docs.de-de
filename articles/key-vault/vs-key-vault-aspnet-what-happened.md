---
title: Änderungen an einem ASP.NET-Projekt beim Herstellen einer Verbindung mit Azure Key Vault | Microsoft-Dokumentation
description: Hier wird beschrieben, was mit dem ASP.NET-Projekt geschieht, wenn Sie mithilfe von verbundenen Visual Studio-Diensten eine Verbindung mit Key Vault herstellen.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: a74e4e10681f6b91e028067d8985408b0745dcd2
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "42146489"
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-key-vault-connected-service"></a>Was ist mit dem ASP.NET-Projekt passiert (verbundener Visual Studio-Dienst für Key Vault)?

> [!div class="op_single_selector"]
> - [Erste Schritte](vs-key-vault-aspnet-get-started.md)
> - [Was ist passiert?](vs-key-vault-aspnet-what-happened.md)

In diesem Artikel werden die genauen Änderungen identifiziert, die an einem ASP.NET-Projekt vorgenommen werden, wenn der [verbundene Key Vault-Dienst mit Visual Studio](vs-key-vault-add-connected-service.md) hinzugefügt wird.

Informationen zur Verwendung des verbundenen Diensts finden Sie unter [Erste Schritte](vs-key-vault-aspnet-get-started.md).

## <a name="added-references"></a>Hinzugefügte Verweise

Betrifft die *.NET-Verweise der Projektdatei und `packages.config` (NuGet-Verweise).

| Typ | Verweis |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

## <a name="added-files"></a>Hinzugefügte Dateien

- „ConnectedService.json“ wurde hinzugefügt. Enthält Informationen zum Anbieter des verbundenen Diensts und zur Version sowie einen Link zur Dokumentation.

## <a name="project-file-changes"></a>Änderungen an der Projektdatei

- Die Elementgruppe des verbundenen Diensts und die Datei „ConnectedServices.json“ wurden hinzugefügt.
- Verweise auf die .NET-Assemblys werden im Abschnitt [Hinzugefügte Verweise](#added-references) beschrieben.

## <a name="webconfig-or-appconfig-changes"></a>Änderungen an „web.config“ oder „app.config“

- Die folgenden Konfigurationseinträge wurden hinzugefügt:

    ```xml
    <appSettings>
       <add key="vaultName" value="<your Key Vault name>" />
       <add key="vaultUri" value="<the URI to your Key Vault in Azure>" />
    </appSettings>
    ```

## <a name="changes-on-azure"></a>Änderungen in Azure

- Eine Ressourcengruppe wurde erstellt (oder eine bereits vorhandene wurde verwendet).
- Eine Key Vault-Instanz wurde in der angegebenen Ressourcengruppe erstellt.

