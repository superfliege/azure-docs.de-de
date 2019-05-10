---
title: Verknüpfen von Symantec ICDX-Daten mit der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Symantec ICDX-Daten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d068223f-395e-46d6-bb94-7ca1afd3503c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 0410b052f17a868aed70ce407b9c9fdefbe023df
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233634"
---
# <a name="connect-your-symantec-icdx-appliance"></a>Herstellen einer Verbindung zu Ihrer Symantec ICDX-Anwendung 

> [!IMPORTANT]
> Azure Sentinel ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit dem Connector für Symantec ICDX können Sie alle Protokolle Ihrer Symantec-Sicherheitslösungen einfach mit Azure Sentinel verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Dadurch erhalten Sie einen besseren Einblick in das Netzwerk Ihrer Organisation und bessere Möglichkeiten für Sicherheitsvorgänge. Die Integration von Symantec ICDX und Azure Sentinel stützt sich auf REST-API.


> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-symantec-icdx"></a>Konfigurieren von und Herstellen einer Verbindung mit Symantec ICDX 

Symantec ICDX kann sich in Azure Sentinel integrieren und Protokolle direkt an Azure Sentinel exportieren.

1. Öffnen Sie die Verwaltungskonsole für ICDX.
2. Wählen Sie im linken Navigationsmenü  **Konfiguration** und dann die Registerkarte  **Forwarder** aus.
3. Klicken Sie in der Microsoft Azure Log Analytics-Zeile auf **Mehr** und anschließend auf **Bearbeiten**. 
4. Legen Sie im Fenster **Microsoft Azure Log Analytics Forwarder** Folgendes fest:
    - Belassen Sie für „Name des benutzerdefinierten Protokolls“ den Standardwert „SymantecICDX“.
    - Kopieren Sie die Arbeitsbereichs-ID, und fügen Sie sie in das Feld  **Kundenbezeichner** ein. Kopieren Sie den **Primärschlüssel**, und fügen Sie ihn in das Feld „Gemeinsam verwendeter Schlüssel“ ein. Sie können diese Werte aus dem Azure Sentinel-Portal kopieren, indem Sie **Datenconnectors** und dann **Symantec ICDX** auswählen.
6. Um in Log Analytics das relevante Schema für die Symantec ICDX-Ereignisse zu verwenden, suchen Sie nach **SymantecICDX_CL**.


## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 



## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Symantec ICDX mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).

