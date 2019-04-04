---
title: Sammeln von Windows-Sicherheitsereignisdaten in der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten zu Windows-Sicherheitsereignissen in Azure Sentinel sammeln.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 3c79747bf33e1769af5f8d3589904ba15105f216
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58087600"
---
# <a name="connect-windows-security-events"></a>Herstellen einer Verbindung mit Windows-Sicherheitsereignissen 

> [!IMPORTANT]
> Azure Sentinel ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können alle Sicherheitsereignisse von den mit dem Azure Sentinel-Arbeitsbereich verbundenen Windows-Servern streamen. Diese Verbindung ermöglicht es Ihnen, Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Dadurch erhalten Sie einen besseren Einblick in das Netzwerk Ihrer Organisation und bessere Möglichkeiten für Sicherheitsvorgänge.  Sie können die Ereignisse zum Streamen auswählen:

- **Alle Ereignisse:** alle Windows-Sicherheits- und AppLocker-Ereignisse.
- **Allgemein:** ein Standardsatz von Ereignissen zu Überwachungszwecken.
- **Minimal:** ein kleiner Satz von Ereignissen, die auf potenzielle Bedrohungen hinweisen können. Wenn Sie diese Option aktivieren, wird kein vollständiger Überwachungspfad erstellt.
- **Keine:** keine Sicherheits- oder AppLocker-Ereignisse.

> [!NOTE]
> 
> - Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="set-up-the-windows-security-events-connector"></a>Einrichten des Connectors für Windows-Sicherheitsereignisse

So integrieren Sie Ihre Windows-Sicherheitsereignisse vollständig in Azure Sentinel

1. Wählen Sie im Azure Sentinel-Portal die Option **Datensammlung** aus, und klicken Sie dann auf die Kachel **Windows-Sicherheitsereignisse**. 
1. Wählen Sie die Datentypen aus, die Sie streamen möchten.
1. Klicken Sie auf **Aktualisieren**.


## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann etwa 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 



## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Windows-Sicherheitsereignisse mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [ Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).

