---
title: Trainieren eines Modells – Custom Translator
titleSuffix: Azure Cognitive Services
description: Das Trainieren eines Modells ist ein wichtiger Schritt beim Erstellen eines Übersetzungsmodells. Das Training erfolgt basierend auf Dokumenten, die Sie für diese Trainingsvorgänge auswählen.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 60e0485c28d90050a6ff775db41f8696a09fe033
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53165757"
---
# <a name="train-a-model"></a>Trainieren eines Modells

Das Trainieren ist ein wichtiger Schritt beim Erstellen eines Übersetzungsmodells, da die Erstellung des Modells ohne Training nicht möglich ist. Das Trainieren erfolgt basierend auf Dokumenten, die Sie für die Trainingsvorgänge auswählen.

Trainieren Sie ein Modell wie folgt:

1.  Wählen Sie das Projekt aus, für das Sie ein Modell erstellen möchten.

2.  Auf der Registerkarte „Daten“ für das Projekt werden alle relevanten Dokumente für das Sprachpaar des Projekts angezeigt. Wählen Sie die Dokumente, die Sie zum Trainieren Ihres Modells verwenden möchten, manuell aus. Sie können auf diesem Bildschirm Dokumente für das Trainieren, Optimieren und Testen auswählen. Außerdem können Sie einfach den Trainingssatz auswählen und Custom Translator die Erstellung des Optimierungs- und Testsatzes überlassen.

    -  Dokumentname: Name des Dokuments.

    -  Kopplung: Gibt an, ob es sich um ein paralleles oder einsprachiges Dokument handelt. Einsprachige Dokumente werden für das Training derzeit nicht verwendet.

    -  Dokumenttyp: Hier kann Trainieren, Optimieren, Testen oder Wörterbuch als Typ angegeben werden.

    -  Sprachpaar: Gibt die Ausgangs- und Zielsprache für das Projekt an.

    -  Ausgangssätze: Gibt die Anzahl von Sätzen an, die aus der Quelldatei extrahiert werden.

    -  Zielsätze: Gibt die Anzahl von Sätzen an, die aus der Zieldatei extrahiert werden.

    ![Trainieren eines Modells](media/how-to/how-to-train-model.png)

3.  Klicken Sie auf die Schaltfläche „Trainieren“.

4.  Geben Sie im Dialogfeld einen Namen für Ihr Modell ein.

5.  Klicken Sie auf „Modell trainieren“.

    ![Dialogfeld „Modell trainieren“](media/how-to/how-to-train-model-2.png)

6.  Custom Translator übermittelt das Training und zeigt den zugehörigen Status auf der Registerkarte „Modelle“ an.

    ![Seite „Modell trainieren“](media/how-to/how-to-train-model-3.png)


## <a name="edit-a-model"></a>Bearbeiten eines Modells

Sie können ein Modell bearbeiten, indem Sie auf der Seite „Modelldetails“ den Link „Bearbeiten“ verwenden.

1.  Klicken Sie auf das Stiftsymbol.

    ![Bearbeiten des Modells](media/how-to/how-to-edit-model.png)

2.  Ändern Sie im Dialogfeld

    1.  Modellname (erforderlich): Geben Sie Ihrem Modell einen aussagekräftigen Namen.

        ![Dialogfeld für Bearbeitung](media/how-to/how-to-edit-model-dialog.png)

3.  Klicken Sie auf Speichern.


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich, [wie Sie Modelldetails anzeigen](how-to-view-model-details.md).
