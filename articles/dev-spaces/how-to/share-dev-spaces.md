---
title: Freigeben von Azure Dev Spaces | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
manager: douge
ms.openlocfilehash: 140a487a0a605e0f03122868af0d2d12bcdba35b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34197656"
---
# <a name="share-azure-dev-spaces"></a>Freigeben von Azure Dev Spaces

Mit Azure Dev Spaces können Sie Ihren Dev Space (Entwicklungsbereich) für andere Personen in Ihrem Team freigeben. Jeder Entwickler kann in seinem eigenen Bereich arbeiten, ohne befürchten zu müssen, andere zu stören. Darüber hinaus ermöglicht die Zusammenarbeit in einem Bereich das durchgängige Testen (End-to-End-Testen) von Code, ohne Mocks erstellen oder Abhängigkeiten simulieren zu müssen. Im Leitfaden [Weitere Informationen zur Teamentwicklung](../get-started-nodejs.md#learn-about-team-development) können Sie mehr darüber erfahren.

Gehen Sie zum Einrichten einer Umgebung für mehrere Entwickler wie folgt vor:
1. Erstellen Sie einen Dev Space in Azure. Wählen Sie [.NET Core und VS Code](../get-started-netcore.md), [.NET Core und Visual Studio](../get-started-netcore-visualstudio.md) oder [Node.js und VS Code](../get-started-nodejs.md) aus. Sie müssen über Zugriffsrechte der Rolle „Besitzer“ oder „Mitwirkender“ für das ausgewählte Azure-Abonnement verfügen.
1. Konfigurieren Sie die Azure Dev Space-**Ressourcengruppe** so, dass jedem Teammitglied [Zugriffsrechte für Mitwirkende gewährt werden](/azure/active-directory/role-based-access-control-configure). Sie können die Ressourcengruppe einer Umgebung überprüfen, indem Sie den folgenden Befehl ausführen: `azds resource list`
1. Fordern Sie die Teammitglieder zum **Auswählen der Umgebung** auf, um darin entwickeln zu können.
     * **Befehlszeile oder VS Code**: Um vorhandene Azure Dev Spaces anzuzeigen, auf die Sie zugreifen können, verwenden Sie den Befehl `azds resource list`. Um eine Umgebung auszuwählen, verwenden Sie den Befehl `azds resource select`.
     * **Visual Studio-IDE**: Öffnen Sie ein Projekt in Visual Studio, und wählen Sie im Dropdownmenü „Starteinstellungen“ die Option **Azure Dev Spaces** aus. Wählen Sie im Dialogfeld, das daraufhin geöffnet wird, eine vorhandene Entwicklungsumgebung aus.

![Visual Studio-Dropdownmenü „Starteinstellungen“](../media/get-started-netcore-visualstudio/LaunchSettings.png)