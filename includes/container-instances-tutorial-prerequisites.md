---
title: Includedatei
description: Includedatei
services: container-instances
author: mmacy
ms.service: container-instances
ms.topic: include
ms.date: 03/20/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 95a8cf4eca7969631d069a31c9d08ba52c8b02b4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
Für dieses Tutorial müssen folgende Voraussetzungen erfüllt sein:

**Azure-Befehlszeilenschnittstelle:** Auf dem lokalen Computer muss mindestens Version 2.0.29 der Azure-Befehlszeilenschnittstelle installiert sein. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0][azure-cli-install].

**Docker:** In diesem Tutorial wird vorausgesetzt, dass zentrale Docker-Konzepte wie Container und Containerimages sowie grundlegende `docker`-Befehle bekannt sind. Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht][docker-get-started].

**Docker-Engine:** Für dieses Tutorial muss die Docker-Engine lokal installiert sein. Für die Docker-Umgebung stehen Konfigurationspakete für [macOS][docker-mac], [Windows][docker-windows] und [Linux][docker-linux] zur Verfügung.

> [!IMPORTANT]
> Da Azure Cloud Shell den Docker-Daemon nicht beinhaltet, *müssen* Sie sowohl die Azure-Befehlszeilenschnittstelle als auch die Docker-Engine auf Ihrem *lokalen Computer* installieren, um dieses Tutorial absolvieren zu können. Azure Cloud Shell kann für dieses Tutorial nicht verwendet werden.

<!-- LINKS - External -->
[docker-get-started]: https://docs.docker.com/engine/docker-overview/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli