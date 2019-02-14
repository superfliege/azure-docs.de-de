---
title: Erste Schritte mit Project Acoustics
titlesuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie das Plug-In in Ihr Unity-Projekt integrieren, die Szene hineinrendern („baken“) und Akustikparameter auf Soundquellen anwenden.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: a9cc8c7b4cdcc05d38bc0d68561fc9d86305b0cb
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55879913"
---
# <a name="getting-started-with-project-acoustics"></a>Erste Schritte mit Project Acoustics
In dieser Schnellstartanleitung erfahren Sie, wie Sie das Plug-In in Ihr Unity-Projekt integrieren, die Szene hineinrendern („baken“) und Akustikparameter auf Soundquellen anwenden. Für dieses Schnellstarttutorial müssen Sie zunächst ein [Azure Batch-Konto](create-azure-account.md) erstellen. Grundlegende Unity-Kenntnisse werden in dieser Anleitung vorausgesetzt.

## <a name="download-the-plugin"></a>Herunterladen des Plug-Ins
Registrieren Sie sich [hier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u), um die Designer-Vorschauversion nutzen zu können.

## <a name="supported-platforms-for-quickstart"></a>Unterstützte Plattformen für das Schnellstarttutorial
* [Unity 2018.2 oder höher](http://www.unity3d.com)
  * Skriptlaufzeitversion **.NET 4.x Equivalent** erforderlich 
  * Windows-basierter Unity-Editor erforderlich

## <a name="import-the-plugin"></a>Importieren des Plug-Ins
Importieren Sie das Akustik-Unity-Paket in Ihr Projekt. 
* Wechseln Sie in Unity zu **Assets (Medienobjekte) > Import Package (Paket importieren) > Custom Package... (Benutzerdefiniertes Paket...)**.

![Importieren des Pakets](media/ImportPackage.png)  

* Wählen Sie **MicrosoftAcoustics.unitypackage** aus.

Wenn Sie das Plug-In in ein vorhandenes Projekt importieren, ist in Ihrem Projekt möglicherweise bereits eine **mcs.rsp**-Datei im Projektstammverzeichnis vorhanden, in dem Optionen für den C#-Compiler festgelegt werden. Sie müssen diese Datei mit der Datei „mcs.rsp“, die im Plug-In Project Acoustics enthalten ist, zusammenführen.

## <a name="enable-the-plugin"></a>Aktivieren des Plug-Ins
Für den Bakevorgang des Akustiktoolkits ist die Skriptlaufzeitversion .NET 4.x erforderlich. Durch den Paketimport werden die Einstellungen des Unity-Players aktualisiert. Starten Sie Unity neu, damit die Einstellungen übernommen werden.

![Playereinstellungen](media/PlayerSettings.png)

![.NET 4.5](media/Net45.png)

## <a name="create-a-navigation-mesh"></a>Erstellen eines Navigationsgittermodells
Erstellen Sie mithilfe des [Standard-Unity-Workflows](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html) ein Navigationsgittermodell für Ihr Projekt. Informationen zur Verwendung eigener Navigationsgittermodelle finden Sie unter [Tutorial: Bake-Benutzeroberfläche](bake-ui-walkthrough.md).

## <a name="mark-static-meshes-for-acoustics"></a>Markieren von statischen Akustikgittermodellen
Rufen Sie das Akustikfenster über **Window (Fenster) > Acoustics (Akustik)** in Unity auf. Dieses Fenster kann an den Inspector angedockt werden.

![Öffnen des Fensters „Acoustics“ (Akustik)](media/WindowAcoustics.png)

Heben Sie im Unity-Fenster „Hierarchy“ (Hierarchie) die Markierung für alle ausgewählten Elemente auf. Aktivieren Sie auf der Registerkarte **Object** (Objekt) des Akustikfensters das Kontrollkästchen „Acoustics Geometry“ (Geometrie für Akustik), um alle Gittermodelle und Geländeobjekte in Ihrer Szene als Geometrieobjekte für die Akustik festzulegen.

Weisen Sie auf der Registerkarte **Materials** (Materialien) die Akustikmaterialien den Materialien zu, die in Ihrer Szene verwendet werden. Das Standardmaterial **Default** weist einen Absorptionskoeffizienten auf, der dem von Zement entspricht. Weitere Informationen zum Festlegen eigener Materialeigenschaften finden Sie auf der Seite [Design process (Designvorgang)](design-process.md).

![Registerkarte „Materials“ (Materialien)](media/MaterialsTab.png)

## <a name="preview-the-probes"></a>Vorschau für kugelförmige Objekte zur Sounderfassung („Probes“)
Klicken Sie auf der Registerkarte **Probes** auf **Calculate** (Berechnen). Die Berechnung kann je nach Größe der Szene einige Minuten in Anspruch nehmen. Wenn die Berechnung abgeschlossen ist, werden in der Szenenansicht Kugeln angezeigt, die die Stellen markieren, an denen die Akustiksimulation durchgeführt wird. Diese Stellen werden auch als „Probe“-Punkte bezeichnet. Wenn der Abstand zum Objekt im Szenenfenster klein genug ist, können Sie die Voxeldarstellung der Szene sehen. Die grünen Voxel sollten für die Objekte angezeigt werden, die Sie als geometrische Objekte festgelegt haben. Sie können oben rechts in der Szenenansicht über das Menü „Gizmos“ zwischen der Ansicht für „Probe“-Punkte und Voxel wechseln.

![Gizmos-Vorschau](media/BakePreviewWithGizmos.png)

## <a name="bake-the-scene"></a>Durchführen des Bakevorgangs für die Szene
Geben Sie auf der Registerkarte **Bake** Ihre Azure-Anmeldeinformationen ein, und klicken Sie auf **Bake**. Wenn Sie nicht über ein Azure Batch-Konto verfügen, finden Sie in [diesem Tutorial Hinweise zur empfohlenen Kontoeinrichtung](create-azure-account.md).
Wenn der Bakevorgang abgeschlossen ist, wird die Datendatei automatisch in das Verzeichnis **Assets/AcousticsData** (Medienobjekte/Akustikdaten) Ihres Projekts heruntergeladen.

## <a name="set-up-audio-runtime-dsp"></a>Einrichten der Audiolaufzeit-DSP
Die Audiolaufzeit-DSP für die Akustik wird in das Unity-Spatializer-Framework eingebettet und in die HRTF-basierte Klangverräumlichung integriert. Gehen Sie wie folgt vor, um die Soundverarbeitung zu aktivieren: Wechseln Sie zum Spatializer **Microsoft Acoustics**, indem Sie **Edit (Bearbeiten) > Project Settings (Projekteinstellungen) > Audio** aufrufen und anschließend **Microsoft Acoustics** als **Spatializer-Plug-In** für Ihr Projekt festlegen. Stellen Sie außerdem sicher, dass **DSP Buffer Size** (DSP-Puffergröße) auf „Best Performance“ (Optimale Leistung) festgelegt ist.

![„Project Settings“ (Projekteinstellungen)](media/ProjectSettings.png)  

![Spatializer für Project Acoustics](media/ChooseSpatializer.png)

Öffnen Sie den Audiomixer (**Window (Fenster) > Audio Mixer (Audiomixer)**). Stellen Sie sicher, dass mindestens ein Mixer mit einer Gruppe festgelegt ist. Wenn dies noch nicht der Fall ist, klicken Sie auf die Schaltfläche „+“ rechts neben **Mixers** (Mixer). Klicken Sie mit der rechten Maustaste unten in der Kanalleiste auf den Effektbereich, und fügen Sie den Effekt **Microsoft Acoustics Mixer** hinzu. Beachten Sie, dass in Project Acoustics nicht mehrere Mixer gleichzeitig verwendet werden können.

![Audiomixer](media/AudioMixer.png)

## <a name="set-up-the-acoustics-lookup-table"></a>Einrichten der Akustik-Nachschlagetabelle
Ziehen Sie das Prefab **Microsoft Acoustics** per Drag & Drop von Ihrem Projektbereich in Ihre Szene:

![Akustikprefab](media/AcousticsPrefab.png)

Klicken Sie auf das Spielobjekt **ProjectAcoustics**, und wechseln Sie zum zugehörigen Inspectorbereich. Geben Sie den Speicherort Ihres Bakeergebnisses (die ACE-Datei unter **Assets/AcousticsData**) an, indem Sie die Datei per Drag & Drop in das Skript „Acoustics Manager“ (Akustikverwalter) ziehen oder auf den Kreis neben dem Textfeld klicken.

![„Acoustics Manager“ (Akustikverwalter)](media/AcousticsManager.png)  

## <a name="apply-acoustics-to-sound-sources"></a>Festlegen von Akustikparametern für Soundquellen
Erstellen Sie eine Audioquelle. Klicken Sie auf das Kontrollkästchen neben **Spatialize** (Verräumlichen) im unteren Inspectorbereich der Audioquelle. Stellen Sie sicher, dass **Spatial Blend** (Räumliche Überlagerung) auf „Full 3D“ festgelegt ist.  

![Audioquelle](media/AudioSource.png)

## <a name="apply-post-bake-design"></a>Anwenden eines Designs nach dem Bakevorgang
Sie können das Skript **AcousticsAdjust** einer Soundquelle Ihrer Szene hinzufügen, um zusätzliche Quelldesignparameter zu aktivieren. Klicken Sie dazu auf **Add Component** (Komponente hinzufügen), und wählen Sie **Scripts (Skripts) > Acoustics Adjust (Sound anpassen)** aus:

![AcousticsAdjust](media/AcousticsAdjust.png)

Auch für den **Microsoft Acoustics Mixer** sind Parameter verfügbar. Weitere Informationen zum Design nach dem Bakevorgang finden Sie unter [Designparameter](design-process.md).

## <a name="next-steps"></a>Nächste Schritte
* Probieren Sie die [Beispielszene](sample-walkthrough.md) aus.
* Machen Sie sich mit allen [Bakefeatures](bake-ui-walkthrough.md) vertraut.
* Setzen Sie sich mit umfassenderen [Designparametern](design-process.md) auseinander.

