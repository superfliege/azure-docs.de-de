---
title: Installieren Sie den DC/OS CLI | Microsoft Docs
description: Installieren Sie den DC/OS CLI.
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Container "," Micro-Dienste, DC/OS, Azure
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2016
ms.author: rogardle
ms.openlocfilehash: a8ea47f158c0d666340815d2e039995c7483257f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
> [!NOTE]
> Dies ist für die Arbeit mit ACS DC/OS-basierten Clustern. Es ist nicht erforderlich für ACS Punktschwarms-basierten Cluster dazu.
> 
> 

Erstens [mit Ihrem DC/OS-basierten ACS-Cluster verbinden](../articles/container-service/container-service-connect.md). Sobald Sie dies getan haben, können Sie den DC/OS-CLI auf dem Clientcomputer mithilfe der unten aufgeführten Befehle installieren:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Wenn Sie eine alte Version von Python verwenden, wird Sie möglicherweise einige "InsecurePlatformWarnings" fest. Sie können diese ignorieren.

Führen Sie zum Einstieg ohne Neustart der Shell:

```bash
source ~/.bashrc
```

Dieser Schritt ist nicht erforderlich, wenn Sie neue Shells starten.

Jetzt können Sie sicherstellen, dass die CLI installiert ist:

```bash
dcos --help
```