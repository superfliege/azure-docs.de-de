---
title: "Visualisierung von Abhängigkeiten in Azure Migrate | Microsoft-Dokumentation"
description: "Bietet eine Übersicht über Bewertungsberechnungen im Azure Migrate-Dienst."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 78e52157-edfd-4b09-923f-f0df0880e0e0
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: a8a8cee327dac8adfb0ae53d101c382ef20599d2
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
# <a name="dependency-visualization"></a>Visualisierung von Abhängigkeiten

Der [Azure Migrate](migrate-overview.md)-Dienst bewertet Gruppen von lokalen Computern für die Migration zu Azure. Computer können unter Verwendung der Abhängigkeitsvisualisierung gruppiert werden. Dieser Artikel enthält Informationen zu diesem Feature.


## <a name="overview"></a>Übersicht

Mit der Abhängigkeitsvisualisierung in Azure Migrate können Sie Gruppen für die Migrationsbewertung mit höherer Zuverlässigkeit erstellen. Die Abhängigkeitsvisualisierung ermöglicht die Darstellung von Netzwerkabhängigkeiten bestimmter Computer oder Computergruppen. Dadurch wird sichergestellt, dass bei der Migration keine Funktionen verloren gehen (oder Computer vergessen werden), wenn Apps und Workloads über mehrere Computer hinweg ausgeführt werden.  

## <a name="how-does-it-work"></a>Funktionsweise

Azure Migrate verwendet für die Abhängigkeitsvisualisierung die [Dienstzuordnung](../operations-management-suite/operations-management-suite-service-map.md) in [Log Analytics](../log-analytics/log-analytics-overview.md).
- Wenn Sie ein Azure Migrate-Projekt erstellen, wird in Ihrem Abonnement ein OMS Log Analytics-Arbeitsbereich erstellt.
- Der Arbeitsbereich wird mit dem Namen des Migrationsprojekts und dem Präfix **migrate-** versehen. Optional wird noch eine Zahl als Suffix angehängt. 
- Navigieren Sie auf der Übersichtsseite des Projekts im Abschnitt **Zusammenfassung** zu dem Log Analytics-Arbeitsbereich.
- Der erstellte Arbeitsbereich ist mit dem Schlüssel **MigrateProject** und dem Wert **Projektname** gekennzeichnet. Diese Werte können für die Suche im Azure-Portal verwendet werden.  

    ![Log Analytics-Arbeitsbereich](./media/concepts-dependency-visualization/oms-workspace.png)

Zur Verwendung der Abhängigkeitsvisualisierung müssen Sie Agents auf alle lokalen Computer, die Sie analysieren möchten, herunterladen und dort installieren.  

## <a name="do-i-need-to-pay-for-it"></a>Fallen dadurch Kosten an?

Ja. Der Log Analytics-Arbeitsbereich wird standardmäßig erstellt. Er wird aber nur verwendet, wenn Sie die Abhängigkeitsvisualisierung in Azure Migrate nutzen. Wenn Sie die Abhängigkeitsvisualisierung nutzen oder den Arbeitsbereich außerhalb von Azure Migrate verwenden, werden Ihnen die Kosten für die Verwendung des Arbeitsbereichs berechnet.  Weitere Informationen zur Preisgestaltung der Dienstzuordnungslösung finden Sie [hier](https://azure.microsoft.com/pricing/details/insight-analytics/). 

## <a name="how-do-i-manage-the-workspace"></a>Wie verwalte ich den Arbeitsbereich?

Sie können den Log Analytics-Arbeitsbereich außerhalb von Azure Migrate verwalten. Er wird nicht gelöscht, wenn Sie das Migrationsprojekt löschen, in dem er erstellt wurde. Wenn Sie den Arbeitsbereich nicht mehr benötigen, [löschen Sie ihn](../log-analytics/log-analytics-manage-access.md) manuell.

Löschen den von Azure Migrate erstellten Arbeitsbereich nur, wenn Sie auch das Migrationsprojekt löschen. Andernfalls funktionieren die Abhängigkeiten nicht mehr ordnungsgemäß.

## <a name="next-steps"></a>Nächste Schritte

[Group machines using machine dependency mapping](how-to-create-group-machine-dependencies.md) (Gruppieren von Computern mithilfe der Computerabhängigkeitszuordnung)