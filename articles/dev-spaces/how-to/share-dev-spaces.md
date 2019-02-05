---
title: Freigeben von Azure Dev Spaces | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: article
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: 86a9400aca099bb79ca95dfc1c4ac2c4c241a6b2
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55466420"
---
# <a name="share-azure-dev-spaces"></a>Freigeben von Azure Dev Spaces

Mit Azure Dev Spaces können Sie Ihren Dev Space (Entwicklungsbereich) für andere Personen in Ihrem Team freigeben. Jeder Entwickler kann in seinem eigenen Bereich arbeiten, ohne befürchten zu müssen, andere zu stören. Darüber hinaus ermöglicht die Zusammenarbeit in einem Bereich das durchgängige Testen (End-to-End-Testen) von Code, ohne Mocks erstellen oder Abhängigkeiten simulieren zu müssen. Im Leitfaden [Weitere Informationen zur Teamentwicklung](../team-development-nodejs.md) können Sie mehr darüber erfahren.

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Einrichten eines Entwicklungsbereichs für mehrere Entwickler

1. Erstellen Sie einen Dev Space in Azure. Wählen Sie [.NET Core und VS Code](../get-started-netcore.md), [.NET Core und Visual Studio](../get-started-netcore-visualstudio.md) oder [Node.js und VS Code](../get-started-nodejs.md) aus. Sie müssen über Zugriffsrechte der Rolle „Besitzer“ oder „Mitwirkender“ für das ausgewählte Azure-Abonnement verfügen.
1. Konfigurieren Sie die Azure Dev Space-**Ressourcengruppe** so, dass jedem Teammitglied [Zugriffsrechte für Mitwirkende gewährt werden](/azure/active-directory/role-based-access-control-configure). Sie können die Ressourcengruppe eines Entwicklungsbereichs überprüfen, indem Sie den folgenden Befehl ausführen: `azds list-up`
1. Fordern Sie Teammitglieder zum **Auswählen des Entwicklungsbereichs** auf, um ihn für die Entwicklung zu verwenden.
     * **Befehlszeile oder VS Code**: Verwenden Sie den Befehl `azds space list`, um vorhandene Azure Dev Spaces-Instanzen anzuzeigen, auf die Sie zugreifen können. So wählen Sie einen Entwicklungsbereich aus: `azds space select`.
     * **Visual Studio-IDE**: Öffnen Sie ein Projekt in Visual Studio, und wählen Sie im Dropdownmenü „Starteinstellungen“ die Option **Azure Dev Spaces** aus. Wählen Sie im daraufhin angezeigten Dialogfeld einen vorhandenen Cluster aus.

    ![Visual Studio-Dropdownmenü „Starteinstellungen“](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Nächste Schritte

Unter [Weitere Informationen zur Teamentwicklung](../team-development-nodejs.md) können Sie mehr darüber erfahren.
