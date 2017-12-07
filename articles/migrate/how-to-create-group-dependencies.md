---
title: "Verfeinern einer Bewertungsgruppe per Mapping von Gruppenabhängigkeiten in Azure Migrate | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie eine Bewertung verfeinern, indem Sie für den Azure Migrate-Dienst das Mapping von Gruppenabhängigkeiten verwenden."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0527e34e-a078-405e-aeb9-c91a5808112a
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: b4d6861f147fbb6e65a9d529f17f78b54075eb90
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2017
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Verfeinern einer Gruppe per Mapping von Gruppenabhängigkeiten

In diesem Artikel wird beschrieben, wie Sie das Mapping von Gruppenabhängigkeiten für die [Azure Migrate](migrate-overview.md)-Bewertung einrichten. Sie verwenden diese Methode normalerweise, wenn Sie die Einstellungen für eine vorhandene Gruppe durch eine Überprüfung der Gruppenabhängigkeiten vor dem Durchführen einer Bewertung verfeinern möchten. Gruppen, für die Sie das Mapping von Gruppenabhängigkeiten durchführen möchten, sollten nicht mehr als zehn Computer enthalten.  

## <a name="modify-a-group"></a>Ändern einer Gruppe

1. Klicken Sie im Azure Migrate-Projekt unter **Verwalten** auf  **Gruppen**, und wählen Sie eine Gruppe aus.
2. Klicken Sie auf der Seite der Gruppe auf  **Abhängigkeiten anzeigen**, um die Übersicht mit den Gruppenabhängigkeiten zu öffnen. 

     ![Anzeigen der Gruppe](./media/how-to-create-group-dependencies/create-group.png)

3. Klicken Sie auf den Zeitraum, und ändern Sie ihn, um ausführlichere Informationen zu den Abhängigkeiten zu erhalten. Standardmäßig ist ein Bereich von einer Stunde ausgewählt. Sie können den Zeitraum ändern oder das Start- und Enddatum und die Dauer angeben.
4. Klicken Sie bei gedrückter STRG-TASTE, um die Computer in der Übersicht auszuwählen. Fügen Sie in der Übersicht Computer hinzu, oder entfernen Sie sie.
    - Sie können nur Computer hinzufügen, die ermittelt wurden.
    - Wenn Sie Computer für eine Gruppe hinzufügen und entfernen, werden die vorherigen Bewertungen dafür ungültig.
    - Sie können optional eine neue Bewertung erstellen, wenn Sie die Gruppe ändern.
5. Klicken Sie auf **OK**, um die Gruppe zu speichern.

    ![Hinzufügen und Entfernen](./media/how-to-create-group-dependencies/add-remove.png)

Wenn Sie die Abhängigkeiten eines bestimmten Computers überprüfen möchten, der in der Übersicht mit den Gruppenabhängigkeiten angezeigt wird, können Sie eine [Übersicht mit den Computerabhängigkeiten einrichten](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [weiteren Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen.
