---
title: Visualisierung von Abhängigkeiten in Azure Migrate | Microsoft-Dokumentation
description: Bietet eine Übersicht über Bewertungsberechnungen im Azure Migrate-Dienst.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 94d4db078175309444db5113506020eae54dda70
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36218403"
---
# <a name="dependency-visualization"></a>Visualisierung von Abhängigkeiten

Der [Azure Migrate](migrate-overview.md)-Dienst bewertet Gruppen von lokalen Computern für die Migration zu Azure. Computer können unter Verwendung der Abhängigkeitsvisualisierung gruppiert werden. Dieser Artikel enthält Informationen zu diesem Feature.


## <a name="overview"></a>Übersicht

Mit der Abhängigkeitsvisualisierung in Azure Migrate können Sie Gruppen für die Migrationsbewertung mit höherer Zuverlässigkeit erstellen. Die Abhängigkeitsvisualisierung ermöglicht die Darstellung von Netzwerkabhängigkeiten bestimmter Computer oder Computergruppen. Dadurch wird sichergestellt, dass bei der Migration keine Funktionen verloren gehen (oder Computer vergessen werden), wenn Apps und Workloads über mehrere Computer hinweg ausgeführt werden.  

## <a name="how-does-it-work"></a>Wie funktioniert dies?

Azure Migrate verwendet für die Abhängigkeitsvisualisierung die [Dienstzuordnung](../operations-management-suite/operations-management-suite-service-map.md) in [Log Analytics](../log-analytics/log-analytics-overview.md).
- Wenn Sie ein Azure Migrate-Projekt erstellen, wird in Ihrem Abonnement ein Log Analytics-Arbeitsbereich erstellt.
- Der Arbeitsbereich wird mit dem Namen des Migrationsprojekts und dem Präfix **migrate-** versehen. Optional wird noch eine Zahl als Suffix angehängt. 
- Navigieren Sie auf der Übersichtsseite des Projekts im Abschnitt **Zusammenfassung** zu dem Log Analytics-Arbeitsbereich.
- Der erstellte Arbeitsbereich ist mit dem Schlüssel **MigrateProject** und dem Wert **Projektname** gekennzeichnet. Diese Werte können für die Suche im Azure-Portal verwendet werden.  

    ![Log Analytics-Arbeitsbereich](./media/concepts-dependency-visualization/oms-workspace.png)

Zur Verwendung der Abhängigkeitsvisualisierung müssen Sie Agents auf alle lokalen Computer, die Sie analysieren möchten, herunterladen und dort installieren.  

## <a name="do-i-need-to-pay-for-it"></a>Fallen dadurch Kosten an?

Azure Migrate ist ohne Aufpreis erhältlich. Für die Nutzung der Abhängigkeitsvisualisierungsfeatures von Azure Migrate ist die Dienstzuordnung erforderlich. Bei der Erstellung eines Azure Migrate-Projekts wird von Azure Migrate in Ihrem Namen automatisch ein neuer Log Analytics-Arbeitsbereich erstellt.

> [!NOTE]
> Für das Feature für die Abhängigkeitsvisualisierung wird die Dienstzuordnung über einen Log Analytics-Arbeitsbereich verwendet. Seit dem 28. Februar 2018 ist das Feature mit der Ankündigung der allgemeinen Verfügbarkeit von Azure Migrate jetzt ohne weitere Kosten verfügbar. Sie müssen ein neues Projekt erstellen, um den kostenlosen Arbeitsbereich nutzen zu können. Für Arbeitsbereiche, die bereits vor der allgemeinen Verfügbarkeit vorhanden waren, können weiterhin Kosten anfallen. Daher empfehlen wir Ihnen, die Umstellung auf ein neues Projekt durchzuführen.

1. Wenn Sie eine andere Lösung als die Dienstzuordnung in diesem Log Analytics-Arbeitsbereich verwenden, fallen die Log Analytics-Standardgebühren an. 
2. Zur Unterstützung von Migrationsszenarien ohne weitere Kosten fallen für die Dienstzuordnungslösung in den ersten 180 Tagen nach der Erstellung des Azure Migrate-Projekts keine Kosten an. Danach werden die Standardgebühren berechnet.
3. Nur der Arbeitsbereich, der im Rahmen der Projekterstellung erstellt wurde, kann kostenlos genutzt werden.

Verwenden Sie beim Registrieren von Agents für den Arbeitsbereich die ID und den Schlüssel, die bzw. der auf der Seite mit Schritten für die Agent-Installation für das Projekt angegeben wurde. Es ist nicht möglich, einen vorhandenen Arbeitsbereich zu verwenden und dem Azure Migrate-Projekt zuzuordnen.

Wenn das Azure Migrate-Projekt gelöscht wurde, wird der Arbeitsbereich nicht zusammen mit dem Projekt gelöscht. Nach dem Löschen des Projekts ist die Nutzung der Dienstzuordnung nicht mehr kostenlos, und für jeden Knoten werden Kosten gemäß des kostenpflichtigen Tarifs für den Log Analytics-Arbeitsbereich berechnet.

Weitere Informationen zu den Preisen von Azure Migrate finden Sie [hier](https://azure.microsoft.com/pricing/details/azure-migrate/). 

## <a name="how-do-i-manage-the-workspace"></a>Wie verwalte ich den Arbeitsbereich?

Sie können den Log Analytics-Arbeitsbereich außerhalb von Azure Migrate verwalten. Er wird nicht gelöscht, wenn Sie das Migrationsprojekt löschen, in dem er erstellt wurde. Wenn Sie den Arbeitsbereich nicht mehr benötigen, [löschen Sie ihn](../log-analytics/log-analytics-manage-access.md) manuell.

Löschen den von Azure Migrate erstellten Arbeitsbereich nur, wenn Sie auch das Migrationsprojekt löschen. Andernfalls funktionieren die Abhängigkeiten nicht mehr ordnungsgemäß.

## <a name="next-steps"></a>Nächste Schritte

[Group machines using machine dependency mapping](how-to-create-group-machine-dependencies.md) (Gruppieren von Computern mithilfe der Computerabhängigkeitszuordnung)