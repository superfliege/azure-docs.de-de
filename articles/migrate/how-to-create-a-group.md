---
title: Gruppieren von Computern für die Bewertung mit Azure Migrate | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie Computer gruppieren, bevor Sie mit dem Azure Migrate-Dienst eine Bewertung ausführen.
author: rayne-wiselman
ms.service: azure-migrate
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: 5c5b5dc97c0faf43c9543422406c4fa1c30ed679
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34203698"
---
# <a name="group-machines-for-assessment"></a>Gruppieren von Computern für die Bewertung

In diesem Artikel wird beschrieben, wie Sie eine Gruppe mit Computern für die Bewertung durch [Azure Migrate](migrate-overview.md) erstellen. Azure Migrate bewertet Computer in der Gruppe, um zu ermitteln, ob sie sich für die Migration zu Azure eignen, und stellt Größen- und Kostenschätzungen für die Ausführung der Computer in Azure bereit.


## <a name="create-a-group"></a>Erstellen einer Gruppe

1. Klicken Sie in der **Übersicht** des Azure Migrate-Projekts unter „Verwalten“ auf  **Gruppen** > **+Gruppe**, und geben Sie einen Gruppennamen an.
2. Fügen Sie Computer zur Gruppe hinzu, und klicken Sie auf  **Erstellen**. 
3. Sie können optional festlegen, dass eine neue Bewertung für die Gruppe ausgeführt wird. 

    ![Erstellen einer Gruppe](./media/how-to-create-a-group/create-group.png)

Nachdem die Gruppe erstellt wurde, können Sie sie ändern, indem Sie die Gruppe auf der Seite **Gruppen** auswählen und Computer hinzufügen bzw. entfernen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das Erstellen von Gruppen mit hoher Vertrauenswürdigkeit mithilfe der [Zuordnung von Computerabhängigkeiten](how-to-create-group-machine-dependencies.md).
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
