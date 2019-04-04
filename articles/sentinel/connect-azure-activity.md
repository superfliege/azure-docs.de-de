---
title: Sammeln von Azure-Aktivitätsdaten in Azure Sentinel Preview | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-Aktivitätsdaten in Azure Sentinel sammeln.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 8c25baa8-b93b-41da-9e6c-15bb7b5c5511
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 42ce8d06489cfacf7ba9bb3de1425224b5df95e7
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2019
ms.locfileid: "57240574"
---
# <a name="collect-data-from-azure-activity-log"></a>Sammeln von Daten aus dem Azure-Aktivitätsprotokoll

> [!IMPORTANT]
> Azure Sentinel ist aktuell als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können Protokolle aus dem [Azure-Aktivitätsprotokoll](../azure-monitor/platform/activity-logs-overview.md) mit einem einzigen Klick an Azure Sentinel streamen. Das Aktivitätsprotokoll ist ein Abonnementprotokoll, das Einblicke in Ereignisse auf Abonnementebene ermöglicht, die in Azure aufgetreten sind. Dies schließt einen Datenbereich von Azure Resource Manager-Betriebsdaten bis hin zu Aktualisierungen für Dienstintegritätsereignisse ein. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für jeden Schreibvorgang (PUT, POST, DELETE) ermitteln, der für die Ressourcen Ihres Abonnements ausgeführt wurde. Sie können auch den Status des Vorgangs und andere relevante Eigenschaften verstehen. Das Aktivitätsprotokoll umfasst keine Lesevorgänge (GET) oder Vorgänge für Ressourcen, die das klassische Modell/RDFE-Modell verwenden. 


## <a name="prerequisites"></a>Voraussetzungen

- Ein Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen.


## <a name="connect-to-azure-activity-log"></a>Herstellen einer Verbindung mit dem Aktivitätsprotokoll

1. Wählen Sie in Azure Sentinel die Option **Datenerfassung** aus, und klicken Sie dann auf die Kachel **Azure-Aktivitätsprotokoll**.

2. Wählen Sie im Bereich für das Azure-Aktivitätsprotokoll die Abonnements aus, die Sie an Azure Sentinel streamen möchten. 

3. Klicken Sie auf **Verbinden**.

4. Um das relevante Schema für die Azure-Aktivitätswarnungen in Log Analytics zu verwenden, suchen Sie nach **AzureActivity**.


 

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie das Azure-Aktivitätsprotokoll mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [ Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
