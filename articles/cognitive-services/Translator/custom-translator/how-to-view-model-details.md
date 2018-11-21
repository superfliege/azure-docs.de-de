---
title: Anzeigen der Modelldetails – Custom Translator
titleSuffix: Azure Cognitive Services
description: Die Registerkarte „Modelle“ zeigt unter jedem Projekt Details zu jedem Modell an, z.B. Modellname, Modellstatus, BLEU-Score, Training, Optimierung, Anzahl von Testsets.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 80ee99fc4490f1fdc591093f357c8db55a97abb9
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51626995"
---
# <a name="view-model-details"></a>Anzeigen der Modelldetails

Die Registerkarte „Modelle“ unter „Projekt“ zeigt alle Modelle in diesem Projekt an. Alle für dieses Projekt trainierten Modelle sind in dieser Registerkarte aufgelistet.

Für jedes Modell im Projekt werden folgende Modelldetails angezeigt.

1.  Modellname: Der Modellname eines bestimmten Modells.

2.  Status: Status eines bestimmten Modells. Ihr neues Training hat den Status „Übermittelt“, bis die Daten angenommen werden. Der Status ändert sich in „Datenverarbeitung“, wenn der Dienst den Inhalt Ihrer Dokumente auswertet. Wenn die Auswertung Ihrer Dokumente abgeschlossen ist, wechselt der Status zu „Wird ausgeführt“. Nun können Sie die Anzahl von Sätzen in Ihrem Training sehen, einschließlich der Optimierungs- und Testsets, die automatisch für Sie erstellt werden. Die folgende Liste beschreibt die unterschiedlichen Modellstatus.

    -  Submitted: Das Back-End verarbeitet die Dokumente für das Modell.

    -  TrainingQueued: Das Training für dieses Modell wurde der Warteschlange des MT-Systems hinzugefügt.

    -  Running: Das Training wird im MT-System für dieses Modell ausgeführt.

    -  Succeeded: Das Training war im MT-System erfolgreich, und ein Modell ist verfügbar. Bei diesem Status wird ein BLEU-Score (Bilingual Evaluation Understudy) für dieses Modell angezeigt.

    -  Deployed: Das erfolgreich trainierte Modell wird an das MT-System zum Bereitstellen übermittelt.

    -  Undeploying: Die Bereitstellung des Modells wird zurückgenommen.

    -  Undeployed: Die Bereitstellung eines Modells wurde erfolgreich zurückgenommen.

    -  Training Failed: Beim Trainieren ist ein Fehler aufgetreten. Wenn ein Trainingsfehler auftritt, versuchen Sie den Trainingsauftrag erneut. Wenden Sie sich an uns, wenn der Fehler weiterhin auftritt. Löschen Sie nicht das fehlerhafte Modell.

    - DataProcessingFailed: Die Datenverarbeitung ist für mindestens ein Dokument des Modells fehlgeschlagen.

    - DeploymentFailed: Die Modellimplementierung ist fehlgeschlagen.

    - MigratedDraft: Das Modell befindet sich nach der Migration von Hub zu Custom Translator im Entwurfsstatus.

4.  BLEU Score: BLEU-Score des Modells, der die Qualität Ihres Übersetzungssystems anzeigt. Er gibt Auskunft darüber, wie exakt die vom Übersetzungssystem anhand dieses Trainings angefertigten Übersetzungen mit den Referenzsätzen im Testdataset übereinstimmen. Der BLEU-Score wird angezeigt, wenn das Training erfolgreich abgeschlossen wurde. Wenn das Training nicht abgeschlossen wurde bzw. ein Fehler aufgetreten ist, wird kein BLEU-Score angezeigt.

5.  Anzahl von Trainingssätzen: Gesamtanzahl von Sätzen, die als Trainingsset verwendet werden.

6.  Anzahl von Optimierungssätzen: Gesamtanzahl von Sätzen, die als Optimierungsset verwendet werden.

7.  Anzahl von Testsätzen: Gesamtanzahl von Sätzen, die als Testset verwendet werden.

8.  Anzahl von Monosätzen: Gesamtanzahl von Sätzen, die als Monoset verwendet werden.

9.  Interaktive Schaltfläche „Bereitstellen“: Für erfolgreich trainierte Modelle, die noch nicht bereitgestellt wurden, wird die Schaltfläche „Bereitstellen“ angezeigt. Wenn ein Modell bereitgestellt wurde, wird die Schaltfläche „Bereitstellung zurücknehmen“ angezeigt.

10. Löschen: Mit dieser Schaltfläche können Sie das Modell löschen. Wenn Sie ein Modell löschen, werden nicht die Dokumente entfernt, die zum Erstellen dieses Modells verwendet wurden.

    ![Anzeigen der Modelldetails](media/how-to/how-to-view-model-details.png)

>[!Note]
>Zum Vergleichen aufeinanderfolgender Trainings für dieselben Systeme müssen das Optimierungs- und Testset konstant sein.

## <a name="view-model-training-details"></a>Anzeigen von Modelltrainingsdetails

Wenn Ihr Training abgeschlossen ist, werden auf der Detailseite die genauen Trainingsinformationen angezeigt. Wählen Sie ein Projekt aus, gehen Sie zur Registerkarte „Modelle“, und wählen Sie diese sowie ein Modell aus.

Die Modellseite hat zwei Registerkarten: „Trainingsdetails“ und „Test“.

1.  **Trainingsdetails**: Hier wird die Liste der Dokumente angezeigt, die zum Trainieren verwendet wurden:

    -  Dokumentname: Dieses Feld zeigt den Namen des Dokuments an.

    -  Dokumenttyp: Dieses Feld zeigt, ob dieses Dokument parallel/mono ist.

    -  Anzahl von Sätzen in der Ausgangssprache: Dieses Feld gibt die Anzahl von Sätzen in der Ausgangssprache an.

    -  Anzahl von Sätzen in der Zielsprache: Dieses Feld gibt die Anzahl von Sätzen in der Zielsprache an.

    -  Zugeordnete Sätze: Dieses Feld zeigt an, wie viele Sätze beim Alignment von Custom Translator zugeordnet wurden.

    -  Verwendete Sätze: Dieses Feld zeigt an, wie viele Sätze beim Trainieren von Custom Translator verwendet wurden.

    ![Modelltrainingsdetails](media/how-to/how-to-model-training-details.png)

2.  **Test**: Hier werden die Testdetails für ein erfolgreiches Training angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- Überprüfen und analysieren Sie die [Testergebnisse](how-to-view-system-test-results.md).
