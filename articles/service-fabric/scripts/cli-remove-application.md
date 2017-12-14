---
title: "Azure Service Fabric-CLI-Skript – Entfernungsbeispiel"
description: Entfernen einer Anwendung aus einem Azure Service Fabric-Cluster mithilfe der Azure Service Fabric-CLI
services: service-fabric
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 95a6bac7ee79e3490584305e78d410104363b780
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Entfernen einer Anwendung aus einem Service Fabric-Cluster

Dieses Beispielskript löscht eine ausgeführte Service Fabric-Anwendungsinstanz und hebt die Registrierung für einen Anwendungstyp und eine Version im Cluster auf.  Durch das Löschen der Anwendungsinstanz werden auch alle ausgeführten Dienstinstanzen gelöscht, die dieser Anwendung zugeordnet sind. Als Nächstes werden die Anwendungsdateien aus dem Imagespeicher gelöscht. 

Installieren Sie ggf. die [Service Fabric-CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Beispielskript

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Dokumentation der Service Fabric-Befehlszeilenschnittstelle](../service-fabric-cli.md).

Zusätzliche Service Fabric-CLI-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../samples-cli.md).
