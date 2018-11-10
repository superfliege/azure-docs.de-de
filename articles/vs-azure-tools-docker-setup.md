---
title: Konfigurieren eines Docker-Hosts mit VirtualBox | Microsoft Docs
description: Schrittweise Anweisungen zum Konfigurieren einer Docker-Standardinstanz mithilfe von Docker Machine und VirtualBox
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: ''
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: d7549ac9888e9214280e5311aa5ef2123d97fd47
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50968498"
---
# <a name="configure-a-docker-host-with-virtualbox"></a>Konfigurieren eines Docker-Hosts mit VirtualBox
## <a name="overview"></a>Übersicht
Dieser Artikel führt Sie durch die Konfiguration einer Docker-Standardinstanz mithilfe von Docker Machine und VirtualBox. Bei Verwendung von [Docker für Windows](https://www.docker.com/products/docker-desktop) ist diese Konfiguration nicht erforderlich.

## <a name="prerequisites"></a>Voraussetzungen
Die folgenden Tools müssen installiert werden:

* [Docker Toolbox](https://github.com/docker/toolbox/releases)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Konfigurieren des Docker-Clients mit Windows PowerShell
Um einen Docker-Client zu konfigurieren, öffnen Sie einfach Windows PowerShell, und führen Sie die folgenden Schritte aus:

1. Erstellen Sie eine Standardinstanz des Docker-Hosts.
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. Stellen Sie sicher, dass die Standardinstanz konfiguriert ist und ausgeführt wird. (Für eine Instanz mit dem Namen „default“ sollte angezeigt werden, dass sie ausgeführt wird.)
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![docker-machine ls output][0]
3. Legen Sie „default“ als aktuellen Host fest, und konfigurieren Sie die Shell.
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. Zeigen Sie die aktiven Docker-Container an. Die Liste sollte leer sein.
   
    ```PowerShell
    docker ps
    ```
   
    ![docker ps output][1]

> [!NOTE]
> Bei jedem Neustart Ihres Entwicklungscomputers müssen Sie Ihren lokalen Docker-Host neu starten.
> Führen Sie dazu an einer Eingabeaufforderung den folgenden Befehl aus: `docker-machine start default`.
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
