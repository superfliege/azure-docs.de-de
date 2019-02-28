---
title: Service Fabric CLI-Skriptbeispiel – Auflisten der Anwendungen in einem Cluster
description: Service Fabric CLI-Beispielskript – Listen Sie die Anwendungen auf, die in einem Service Fabric-Cluster bereitgestellt werden.
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: timlt
editor: ''
tags: ''
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 04/13/2018
ms.author: aljo
ms.custom: ''
ms.openlocfilehash: 60cb5aaf93521a3e2f7c922949d112ad96a00180
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805636"
---
# <a name="list-applications-running-in-a-service-fabric-cluster"></a>Auflisten von Anwendungen, die in einem Service Fabric-Cluster ausgeführt werden

Dieses Beispielskript stellt eine Verbindung mit einem Service Fabric-Cluster her und listet dann alle bereitgestellten Anwendungen auf.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Beispielskript

[!code-sh[main](../../../cli_scripts/service-fabric/list-application/list-application.sh "List provisioned applications from a cluster")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Dokumentation der Service Fabric-Befehlszeilenschnittstelle](../service-fabric-cli.md).

Zusätzliche Service Fabric-CLI-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../samples-cli.md).
