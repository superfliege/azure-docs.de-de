---
title: Übersicht über den Entwurfsprozess für Acoustics – Cognitive Services
description: In diesem Dokument wird beschrieben, wie Sie Ihre Entwurfsabsicht in allen drei Phasen des Project Acoustics-Workflows zum Ausdruck bringen können.
services: cognitive-services
author: kegodin
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 3b35f78d66a6ae66b0a56818f5d4be455ce00de5
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181276"
---
# <a name="design-process-overview"></a>Übersicht über den Entwurfsprozess
Sie können Ihre Entwurfsabsicht in allen drei Phasen des Project Acoustics-Workflows zum Ausdruck bringen: Entwurf vor dem Bake-Vorgang, bei der Platzierung der Tonquelle und Entwurf nach dem Bake-Vorgang. Für den Prozess muss mit der Platzierung von Halllautstärken weniger Aufschlag verknüpft werden, während der Entwickler die Kontrolle darüber behält, wie eine Szene klingt.

## <a name="pre-bake-design"></a>Entwurf vor dem Bake-Vorgang
Beim Entwurfsprozess vor dem Bake-Vorgang werden die Szene und Metadaten generiert, die für die Simulation von Akustikwellen verwendet werden. Hierzu gehört die Auswahl von Szenenelementen für die Simulation zur Bereitstellung von Okklusionen, Reflektionen und von Hall. Bei den Metadaten für die Szene handelt es sich um die Auswahl von akustischen Materialien für die einzelnen Szenenelemente. Die akustischen Materialien bestimmten die Menge an Schallenergie, die von der Oberfläche reflektiert wird.

Der Absorptionskoeffizient für alle Oberflächen beträgt standardmäßig 0,04, was für eine stark reflektierende Oberfläche steht. Ästhetische Effekte und Gameplay-Effekte können Sie erzielen, indem Sie die Absorptionskoeffizienten der unterschiedlichen Materialien in der gesamten Szene optimieren. Diese sind für die Zuhörer besonders auffällig, wenn sie die Übergänge von einem Bereich der Szene in einen anderen hören. Der Übergang von einem dunklen Raum mit Hall in eine helle Szene im Freien ohne Hall verstärkt die Wirkung des Übergangs beispielsweise. Um diesen Effekt zu erzielen, legen Sie für die Absorptionskoeffizienten für die Materialien der Szene im Freien höhere Werte fest.

Die Nachhallzeit eines bestimmten Materials in einem Raum ist umgekehrt proportional zum Absorptionskoeffizienten, wobei die meisten Materialien Absorptionswerte im Bereich zwischen 0,01 und 0,20 aufweisen. Materialien mit Absorptionskoeffizienten außerhalb dieses Bereichs sind extrem absorbierend.

![Zeitdiagramm für den Hall](media/ReverbTimeGraph.png)

In der [exemplarischen Vorgehensweise für Baking-UI](bake-ui-walkthrough.md) werden die Steuerelemente für den Vorgang vor dem Baking ausführlich beschrieben.

## <a name="sound-source-placement"></a>Platzierung der Tonquelle
Die Anzeige von Voxeln und Testpunkten zur Laufzeit kann beim Debuggen von Problemen mit Tonquellen helfen, die in der voxelisierten Geometrie feststecken. Um zwischen der Voxelraster- und der Testpunktansicht zu wechseln, klicken Sie auf das entsprechende Kontrollkästchen im Menü „Gizmos“ in der oberen rechten Ecke der Szenenansicht. Wenn sich die Tonquelle in einem Wandvoxel befindet, verschieben Sie sie in einen Luftvoxel.

![Das Menü „Gizmos“](media/GizmosMenu.png)  

Mithilfe der Voxelanzeige können Sie ermitteln, ob auf visuelle Komponenten im Spiel eine Transformation angewendet wurde. Ist dies der Fall, wenden Sie dieselbe Transformation auf das GameObject an, in dem der **Acoustics Manager** gehostet wird.

## <a name="post-bake-design"></a>Entwurf nach dem Bake-Vorgang
Ergebnisse des Bake-Vorgangs werden in der ACE-Datei als Okklusions- und Hallparameter für alle Quellen- und Zuhörerpositionspaare in der gesamten Szene gespeichert. Dieses physisch exakte Ergebnis können Sie unverändert für Ihr Projekt verwenden. Es stellt ein hervorragender Ausgangspunkt für den Entwurf dar. Beim Entwurf nach dem Bake-Vorgang werden Regeln für die Transformation der Baking-Ergebnisparameter zur Laufzeit festgelegt.

### <a name="distance-based-attenuation"></a>Distanzabhängige Dämpfung
Der vom Spatializer-Plug-In von **Microsoft Acoustics** Unity bereitgestellte Audio-DSP berücksichtigt die im Unity-Editor integrierte distanzabhängige Dämpfung pro Quelle. Steuerelemente für die distanzabhängige Dämpfung befinden sich in der Komponente **Audioquelle** im Bereich **Inspektor** von Tonquellen unter **3D Sound Settings** (3D-Soundeinstellungen):

![Distanzdämpfung](media/distanceattenuation.png)

### <a name="tuning-scene-parameters"></a>Optimieren von Szenenparametern
Klicken Sie zum Anpassen von Parametern für alle Quellen auf den Kanalstreifen im **Audiomixer** von Unity, und passen Sie die Parameter für den Effekt **Acoustics Mixer** (Akustikmixer) an.

![Mixeranpassung](media/MixerParameters.png)

### <a name="tuning-source-parameters"></a>Optimieren von Quellenparametern
Durch Anfügen des **AcousticsSourceCustomization**-Skripts an eine Quelle können die Parameter für diese Quelle optimiert werden. Wenn Sie das Skript anfügen möchten, klicken Sie auf **Komponente hinzufügen** im unteren Bereich des Abschnitts **Inspektor** und navigieren zu **Skripts > Acoustics Source Customization** (Anpassung der akustischen Quelle). Das Skript enthält drei Parameter:

![Anpassen der Quelle](media/SourceCustomization.png)

* **Reverb Power Adjust** (Anpassung der Hallstärke): Passt die Hallstärke in dB an. Positive Werte verstärken den Hall, während negative Werte für einen Ton mit weniger Hall sorgen.
* **Decay Time Scale** (Abklingzeitskala): Passt den Multiplikator für die Abklingzeit an. Wenn im Ergebnis des Bake-Vorgangs beispielsweise eine Abklingzeit von 750 Millisekunden angegeben ist, dieser Wert jedoch auf 1,5 festgelegt wurde, beträgt die auf die Quelle angewendete Abklingzeit 1,125 Millisekunden.
* **Enable Acoustics** (Akustik aktivieren): Bestimmt, ob auf diese Quelle Akustikeffekte angewendet werden. Wenn diese Option nicht aktiviert ist, wird die Quelle mit HRTFs, jedoch ohne akustische Effekte, d.h. ohne Verzögerungs-, Okklusions- und dynamische Hallparameter wie Pegel und Abklingzeit verräumlicht. Halleffekte werden dennoch angewendet, jedoch mit einem festen Pegel und einer festen Abklingzeit.

Für unterschiedliche Quellen sind möglicherweise unterschiedliche Einstellungen erforderlich, um bestimmte ästhetische Effekte oder Gameplay-Effekte zu erzielen. Dialoge sind ein gutes Beispiel. Das menschliche Gehör ist durchaus auf den Nachhall in der Sprache eingestellt, jedoch müssen Dialoge bei Spielen in erster Linie verständlich sein. Sie können diesen Umstand berücksichtigen, ohne Dialoge nicht-diegetisch werden zu lassen, indem Sie die Hallstärke nach unten anpassen.
