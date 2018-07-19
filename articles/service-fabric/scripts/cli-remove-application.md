---
title: Azure Service Fabric-CLI-Skript (sfctl) – Entfernungsbeispiel
description: Entfernen einer Anwendung aus einem Azure Service Fabric-Cluster mithilfe der Azure Service Fabric-CLI
services: service-fabric
documentationcenter: ''
author: TylerMSFT
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: twhitney
ms.custom: mvc
ms.openlocfilehash: 24b645552212c502c06cc4df725e77c1d017340e
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069310"
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Entfernen einer Anwendung aus einem Service Fabric-Cluster

Dieses Beispielskript löscht eine ausgeführte Service Fabric-Anwendungsinstanz und hebt dann die Registrierung für einen Anwendungstyp und eine Version im Cluster auf.  Durch das Löschen der Anwendungsinstanz werden auch alle ausgeführten Dienstinstanzen gelöscht, die dieser Anwendung zugeordnet sind. Als Nächstes werden die Anwendungsdateien aus dem Imagespeicher gelöscht. 

Installieren Sie ggf. die [Service Fabric-CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Beispielskript

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Dokumentation der Service Fabric-Befehlszeilenschnittstelle](../service-fabric-cli.md).

Zusätzliche Service Fabric-CLI-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../samples-cli.md).
