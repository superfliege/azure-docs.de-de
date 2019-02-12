---
title: Teamentwicklung mit Azure Dev Spaces mit Node.js und VS Code | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 07/09/2018
ms.topic: tutorial
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: bd2c58860ca51d6a137df656a4098dea1cfc48c1
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2019
ms.locfileid: "55658554"
---
[!INCLUDE [](../../includes/devspaces-team-development-1.md)]

### <a name="make-a-code-change"></a>Vornehmen einer Codeänderung
Wechseln Sie ins VS Code-Fenster für `mywebapi`, und nehmen Sie eine Codeänderung am GET-Standardhandler für `/` vor. Beispiel:

```javascript
app.get('/', function (req, res) {
    res.send('mywebapi now says something new');
});
```

[!INCLUDE [](../../includes/devspaces-team-development-2.md)]
