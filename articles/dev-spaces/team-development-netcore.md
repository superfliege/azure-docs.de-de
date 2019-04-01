---
title: Teamentwicklung mit Azure Dev Spaces mit .NET Core und Visual Studio Code
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 07/09/2018
ms.topic: tutorial
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s '
ms.openlocfilehash: 22f68b4350bfa0310a517bd36cda9763601aad2a
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438713"
---
[!INCLUDE [](../../includes/devspaces-team-development-1.md)]

### <a name="make-a-code-change"></a>Vornehmen einer Codeänderung
Wechseln Sie ins VS Code-Fenster für `mywebapi`, und ändern Sie den Code der `string Get(int id)`-Methode in `Controllers/ValuesController.cs` etwa wie folgt:

```csharp
[HttpGet("{id}")]
public string Get(int id)
{
    return "mywebapi now says something new";
}
```

[!INCLUDE [](../../includes/devspaces-team-development-2.md)]
