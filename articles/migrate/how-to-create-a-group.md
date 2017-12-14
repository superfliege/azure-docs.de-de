---
title: "Gruppieren von Computern für die Bewertung mit Azure Migrate | Microsoft-Dokumentation"
description: "Hier wird beschrieben, wie Sie Computer gruppieren, bevor Sie mit dem Azure Migrate-Dienst eine Bewertung ausführen."
author: rayne-wiselman
ms.service: azure-migrate
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 429a9150d1fbf50c0e3fa2046eb64affc8db8e5d
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2017
---
# <a name="group-machines-for-assessment"></a>Gruppieren von Computern für die Bewertung

In diesem Artikel wird beschrieben, wie Sie eine Gruppe mit Computern für die Bewertung durch [Azure Migrate](migrate-overview.md) erstellen. Azure Migrate bewertet Computer in der Gruppe, um zu ermitteln, ob sie sich für die Migration zu Azure eignen, und stellt Größen- und Kostenschätzungen für die Ausführung der Computer in Azure bereit.


## <a name="create-a-group"></a>Erstellen einer Gruppe

1. Klicken Sie im **Dashboard** des Azure Migrate-Projekts auf  **Gruppen** > **+Gruppe**, und geben Sie einen Gruppennamen an.
2. Fügen Sie Computer zur Gruppe hinzu, und klicken Sie auf  **Erstellen**. 
3. Sie können optional festlegen, dass eine neue Bewertung für die Gruppe ausgeführt wird. 

    ![Erstellen einer Gruppe](./media/how-to-create-a-group/create-group.png)

Nachdem die Gruppe erstellt wurde, können Sie sie ändern, indem Sie die Gruppe auf der Seite **Gruppen** auswählen und Computer hinzufügen bzw. entfernen.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen detaillierterer Gruppen mithilfe der [Zuordnung von Computerabhängigkeiten](how-to-create-group-machine-dependencies.md)
- Weitere Informationen zur [Berechnung von Bewertungen](concepts-assessment-calculation.md)
