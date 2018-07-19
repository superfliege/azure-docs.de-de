---
title: Service Fabric CLI-Skriptbeispiel – Update einer Anwendung in einem Cluster
description: Service Fabric CLI-Skriptbeispiel – Update einer Anwendung mit einer neuen Version. In diesem Beispiel wird auch eine bereitgestellte Anwendung mit den neuen Bits aktualisiert.
services: service-fabric
documentationcenter: ''
author: TylerMSFT
manager: timlt
editor: ''
tags: ''
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: twhitney
ms.custom: ''
ms.openlocfilehash: bd0941cc6e67a759dd5fcb2e8912f8fcd6e9b18c
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071163"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Fügen Sie ein Anwendungszertifikat zu einem Service Fabric-Cluster hinzu

Dieses Beispielskript lädt eine neue Version einer vorhandenen Anwendung hoch und aktualisiert anschließend eine bereitgestellte Anwendung mit den neuen Bits.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Beispielskript

[!code-sh[main](../../../cli_scripts/service-fabric/upgrade-application/upgrade-application.sh "Upload and update an application on a Service Fabric cluster")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Dokumentation der Service Fabric-Befehlszeilenschnittstelle](../service-fabric-cli.md).

Zusätzliche Service Fabric-CLI-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../samples-cli.md).
