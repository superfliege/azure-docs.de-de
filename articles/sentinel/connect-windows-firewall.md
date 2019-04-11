---
title: Sammeln von Windows-Firewalldaten in Azure Sentinel Preview | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Windows-Firewalldaten in Azure Sentinel sammeln.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/28/2019
ms.author: rkarlin
ms.openlocfilehash: 3839d81f70b8bc6dcb1da3c4dd77f52443294707
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574839"
---
# <a name="connect-windows-firewall"></a>Herstellen einer Verbindung mit der Windows-Firewall

> [!IMPORTANT]
> Azure Sentinel ist aktuell als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Der Windows-Firewall-Connector ermöglicht es Ihnen, einfach eine Verbindung mit Ihren Windows-Firewall-Protokollen herzustellen, wenn diese mit Ihrem Azure Sentinel-Arbeitsbereich verbunden sind. Diese Verbindung ermöglicht es Ihnen, Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Auf diese Weise erhalten Sie einen besseren Einblick in das Netzwerk Ihrer Organisation und werden Ihre Möglichkeiten für Sicherheitsvorgänge verbessert.  


> [!NOTE]
> 
> - Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="enable-the-connector"></a>Aktivieren des Connectors 

1. Wählen Sie im Azure Sentinel-Portal die Option **Datensammlung** aus, und klicken Sie dann auf die **Windows-Firewall**-Kachel. 
1. Wählen Sie die Datentypen aus, die Sie streamen möchten.
1. Klicken Sie auf **Installieren**.
6. Um das relevante Schema für die Windows-Firewall in Log Analytics zu verwenden, suchen Sie nach **SecurityEvent**.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 



## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie die Windows-Firewall mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [ Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).

