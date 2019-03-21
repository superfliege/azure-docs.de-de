---
title: Übersicht über Azure Data Factory Mapping Data Flow
description: Eine zusammenfassende Erläuterung von Mapping Data Flows in Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 5064f47444b4ca6d9194ed66144938e6e3d51a4e
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56732364"
---
# <a name="what-are-mapping-data-flows-in-azure-data-factory"></a>Was sind Mapping Data Flows in Azure Data Factory?

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Mit Mapping Data Flows können Data Engineers grafische Datentransformationslogik entwickeln, ohne Code schreiben zu müssen. Die daraus resultierenden Datenflüsse werden als Aktivitäten in Azure Data Factory-Pipelines mit horizontal skalierten Azure Databricks-Clustern ausgeführt.

Die Absicht von Azure Data Factory-Datenfluss ist es, eine vollständig visuelle Erfahrung ohne Codeerstellung zu ermöglichen. Ihre Datenflüsse werden in Ihrem eigenen Ausführungscluster für die horizontal skalierte Datenverarbeitung ausgeführt. Azure Data Factory übernimmt die gesamte Codeübersetzung, Pfadoptimierung und Ausführung Ihrer Datenflussaufträge.

Erstellen Sie zunächst Datenflüsse im Debugmodus, damit Sie Ihre Transformationslogik interaktiv überprüfen können. Fügen Sie anschließend Ihrer Pipeline eine Datenflussaktivität zum Ausführen und Testen Ihres Datenflusses in der Pipeline im Debugmodus hinzu, oder verwenden Sie in der Pipeline die Option „Trigger Now“ (Jetzt auslösen), um den Datenfluss in einer Pipelineaktivität zu testen.

Dann können Sie Ihre Datenflussaktivitäten mithilfe von Azure Data Factory-Pipelines planen und überwachen, die die Datenflussaktivität ausführen.

Der Umschalter „Debugmodus“ auf der Datenfluss-Entwurfsoberfläche ermöglicht die interaktive Erstellung von Datentransformationen. Der Debugmodus bietet eine Umgebung für die Datenvorbereitung zur Erstellung von Datenflüssen.
