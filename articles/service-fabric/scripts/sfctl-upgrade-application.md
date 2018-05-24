---
title: Service Fabric CLI-Skriptbeispiel – Update einer Anwendung in einem Cluster
description: Service Fabric CLI-Skriptbeispiel – Update einer Anwendung mit einer neuen Version. In diesem Beispiel wird auch eine bereitgestellte Anwendung mit den neuen Bits aktualisiert.
services: service-fabric
documentationcenter: ''
author: Thraka
manager: timlt
editor: ''
tags: ''
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: adegeo
ms.custom: ''
ms.openlocfilehash: e14e65e365389b33891794a3f12b86b3a4705533
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204378"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Fügen Sie ein Anwendungszertifikat zu einem Service Fabric-Cluster hinzu

Dieses Beispielskript lädt eine neue Version einer vorhandenen Anwendung hoch und aktualisiert anschließend eine bereitgestellte Anwendung mit den neuen Bits.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Beispielskript

[!code-sh[main](../../../cli_scripts/service-fabric/upgrade-application/upgrade-application.sh "Upload and update an application on a Service Fabric cluster")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Dokumentation der Service Fabric-Befehlszeilenschnittstelle](../service-fabric-cli.md).

Zusätzliche Service Fabric-CLI-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../samples-cli.md).
