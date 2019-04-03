---
title: 'Projekt Akustik: Unity-Integration und Bereitstellung'
titlesuffix: Azure Cognitive Services
description: In dieser Vorgehensweise wird die Integration des Unity-Plug-Ins von Projekt Akustik in Ihr Unity-Projekt erläutert.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: how-to
ms.date: 03/14/2019
ms.author: kegodin
ms.openlocfilehash: 7d8edd7b092ee1ed4f953d753b2bbe864e075378
ms.sourcegitcommit: f68b0e128f0478444740172f54e92b453df696be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/18/2019
ms.locfileid: "58137718"
---
# <a name="project-acoustics-unity-integration"></a>Projekt Akustik: Unity-Integration
In dieser Vorgehensweise wird die Integration des Unity-Plug-Ins von Projekt Akustik in Ihr Unity-Projekt erläutert.

Softwareanforderungen:
* [Unity 2018.2 oder höher](http://unity3d.com) für Windows
* [Projekt Akustik: Unity-Paket](https://www.microsoft.com/en-us/download/details.aspx?id=57346)

## <a name="import-the-plugin"></a>Importieren des Plug-Ins
Importieren Sie das Akustik-Unity-Paket in Ihr Projekt. 
* Wechseln Sie in Unity zu **Assets (Medienobjekte) > Import Package (Paket importieren) > Custom Package... (Benutzerdefiniertes Paket...)**.

    ![Importieren des Pakets](media/import-package.png)  

* Wählen Sie **ProjectAcoustics.unitypackage** aus.

Wenn Sie das Plug-In in ein vorhandenes Projekt importieren, ist in Ihrem Projekt möglicherweise bereits eine **mcs.rsp**-Datei im Projektstammverzeichnis vorhanden, in dem Optionen für den C#-Compiler festgelegt werden. Sie müssen diese Datei mit der Datei „mcs.rsp“, die im Plug-In Project Acoustics enthalten ist, zusammenführen.

## <a name="enable-the-plugin"></a>Aktivieren des Plug-Ins
Für den Bakevorgang des Akustiktoolkits ist die Skriptlaufzeitversion .NET 4.x erforderlich. Durch den Paketimport werden die Einstellungen des Unity-Players aktualisiert. Starten Sie Unity neu, damit die Einstellungen übernommen werden.

![Playereinstellungen](media/player-settings.png)

![.NET 4.5](media/net45.png)

## <a name="set-up-audio-dsp"></a>Audio-DSP einrichten
Projekt Akustik enthält die DSP-Audioruntime, die in das Spatializer-Framework der Unity-Audioengine integriert ist. Sie bietet sowohl HRTF-basierte als auch Schwenkverräumlichung. Um DSP von Projekt Akustik zu aktivieren, öffnen Sie die Unity-Audioeinstellungen über **Edit > Project Settings > Audio** (Bearbeiten > Projekteinstellungen > Audio), und wählen Sie dann **Projekt Akustik** als **Spatializer-Plug-In** für Ihr Projekt aus. Stellen Sie sicher, dass **DSP Buffer Size** (DSP-Puffergröße) auf „Best Performance“ (Optimale Leistung) festgelegt ist.

![„Project Settings“ (Projekteinstellungen)](media/project-settings.png)  

![Spatializer für Project Acoustics](media/choose-spatializer.png)

Öffnen Sie dann den Audiomixer (**Window > Audio Mixer** (Fenster > Audiomixer)). Stellen Sie sicher, dass mindestens ein Mixer mit einer Gruppe festgelegt ist. Wenn dies noch nicht der Fall ist, klicken Sie auf die Schaltfläche „+“ rechts neben **Mixers** (Mixer). Klicken Sie mit der rechten Maustaste unten in der Kanalleiste auf den Effektbereich, und fügen Sie den Effekt **Project Acoustics Mixer** hinzu. Beachten Sie, dass in Project Acoustics nicht mehrere Mixer gleichzeitig verwendet werden können.

![Audiomixer](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Aktivieren der Akustik für Soundquellen
Erstellen Sie eine Audioquelle. Klicken Sie auf das Kontrollkästchen neben **Spatialize** (Verräumlichen) im unteren Inspectorbereich der Audioquelle. Stellen Sie sicher, dass **Spatial Blend** (Räumliche Überlagerung) auf „Full 3D“ festgelegt ist.  

![Audioquelle](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Aktivieren des Akustikdesigns
Fügen Sie das Skript **AcousticsAdjust** einer Soundquelle an Ihre Szene an, um zusätzliche Quelldesignparameter zu aktivieren. Klicken Sie dazu auf **Add Component** (Komponente hinzufügen), und wählen Sie **Scripts > Acoustics Adjust** (Skripts > Sound anpassen) aus:

![AcousticsAdjust](media/acoustics-adjust.png)

## <a name="next-steps"></a>Nächste Schritte
* [Ausführen des Bakings für Ihre Szene mit Projekt Akustik für Unity](unity-baking.md)
* [Erstellen eines Azure Batch-Kontos](create-azure-account.md) für das Baking Ihrer Szene in der Cloud
* Erkunden des [Unity-Entwurfsvorgangs in Projekt Akustik](unity-workflow.md)

