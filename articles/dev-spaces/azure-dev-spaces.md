---
title: Azure Dev Spaces | Microsoft-Dokumentation
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: tutorial
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
manager: douge
ms.openlocfilehash: 344947b7906d15e819e372e0affe4af3c34ba69b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34198757"
---
# <a name="azure-dev-spaces"></a>Azure Dev Spaces
Azure Dev Spaces unterstützt Sie bei der schnellen Entwicklung in Kubernetes. Mit Azure Dev Spaces fügen Sie Ihren Azure Kubernetes-Containern zudem umfassende Entwicklungsfunktionen (etwa Debuggen) hinzu, und Sie können mithilfe vertrauter Tools wie VS Code, Visual Studio oder der Befehlszeile Container iterativ in der Cloud entwickeln. Azure Dev Spaces ist insbesondere bei der Teamentwicklung von Bedeutung, bei der die Isolation einzelner Codebranches in eigenen Bereichen ein wichtiger Teil des Entwicklungslebenszyklus ist.

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>So vereinfacht Azure Dev Spaces die Kubernetes-Entwicklung 

Dieser Ansatz hat mehrere Vorteile:

* Erstellen einer Entwicklungsumgebung ohne Infrastruktur, die repräsentativ für die Produktion ist, mit Vollzugriff auf Cloudressourcen
* Debuggen von Node.js- und .NET Core-Containern direkt in Kubernetes mit VS Code oder Visual Studio. Alle anderen Sprachen können mit der Befehlszeilenschnittstelle entwickelt werden.
* Freigeben einer Kubernetes-Instanz für das gesamte Entwicklungsteam, um Kosten zu sparen und den Einrichtungsaufwand für lokale Computer neuer Teammitglieder zu minimieren
* Entwickeln von Code in Isolation und Ausführen von End-to-End-Tests mit anderen Komponenten ohne Replizieren oder Simulieren von Abhängigkeiten

[!INCLUDE[](includes/get-started.md)]

![](media/azure-dev-spaces/vscode-overview.png)