---
title: 'Übersicht über den Entwurfsprozess für das Projekt: Akustik'
titlesuffix: Azure Cognitive Services
description: In diesem Dokument wird beschrieben, wie Sie Ihre Entwurfsabsicht in allen drei Phasen des Project Acoustics-Workflows zum Ausdruck bringen können.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: bb5f309a96feac2caea85fbe81b7216eecfc4b79
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873936"
---
# <a name="design-process-overview"></a>Übersicht über den Entwurfsprozess
Sie können Ihre Entwurfsabsicht in allen drei Phasen des Project Acoustics-Workflows zum Ausdruck bringen: Szeneneinrichtung vor dem Bake-Vorgang, bei der Platzierung der Tonquelle und Entwurf nach dem Bake-Vorgang. Für den Prozess muss mit der Platzierung von Halllautstärken weniger Aufschlag verknüpft werden, während der Entwickler die Kontrolle darüber behält, wie eine Szene klingt.

## <a name="pre-bake-design"></a>Entwurf vor dem Bake-Vorgang
Beim Szeneneinrichtungsprozess vor dem Bake-Vorgang werden die Szene und Metadaten generiert, die für die Simulation von Akustikwellen verwendet werden. Hierzu gehört die Auswahl von Szenenelementen für die Simulation zur Bereitstellung von Okklusionen, Reflektionen und von Hall. Bei den Metadaten für die Szene handelt es sich um die Auswahl von akustischen Materialien für die einzelnen Szenenelemente. Die akustischen Materialien bestimmten die Menge an Schallenergie, die von der Oberfläche reflektiert wird.

Der Absorptionskoeffizient für alle Oberflächen beträgt standardmäßig 0,04, was für eine stark reflektierende Oberfläche steht. Ästhetische Effekte und Gameplay-Effekte können Sie erzielen, indem Sie die Absorptionskoeffizienten der unterschiedlichen Materialien in der gesamten Szene optimieren. Diese sind für die Zuhörer besonders auffällig, wenn sie die Übergänge von einem Bereich der Szene in einen anderen hören. Der Übergang von einem dunklen Raum mit Hall in eine helle Szene im Freien ohne Hall verstärkt die Wirkung des Übergangs beispielsweise. Um diesen Effekt zu erzielen, legen Sie für die Absorptionskoeffizienten für die Materialien der Szene im Freien höhere Werte fest.

Die Nachhallzeit eines bestimmten Materials in einem Raum ist umgekehrt proportional zum Absorptionskoeffizienten, wobei die meisten Materialien Absorptionswerte im Bereich zwischen 0,01 und 0,20 aufweisen. Materialien mit Absorptionskoeffizienten außerhalb dieses Bereichs sind extrem absorbierend.

![Zeitdiagramm für den Hall](media/ReverbTimeGraph.png)

In der [exemplarischen Vorgehensweise für Baking-UI](bake-ui-walkthrough.md) werden die Steuerelemente für den Vorgang vor dem Baking ausführlich beschrieben.

## <a name="sound-source-placement"></a>Platzierung der Tonquelle
Die Anzeige von Voxeln und Testpunkten zur Laufzeit kann beim Debuggen von Problemen mit Tonquellen helfen, die in der voxelisierten Geometrie feststecken. Um zwischen der Voxelraster- und der Testpunktansicht zu wechseln, klicken Sie auf das entsprechende Kontrollkästchen im Menü „Gizmos“ in der oberen rechten Ecke der Szenenansicht. Wenn sich die Tonquelle in einem Wandvoxel befindet, verschieben Sie sie in einen Luftvoxel.

![Das Menü „Gizmos“](media/GizmosMenu.png)  

Mithilfe der Voxelanzeige können Sie ermitteln, ob auf visuelle Komponenten im Spiel eine Transformation angewendet wurde. Ist dies der Fall, wenden Sie dieselbe Transformation auf das GameObject an, in dem der **Acoustics Manager** gehostet wird.

### <a name="voxel-size-discrepancies"></a>Abweichungen der Voxel-Größe
Sie werden feststellen, dass die Größe der Voxel, die verwendet werden, um zu veranschaulichen, welche der Szenengittermodelle am Acoustics-Bake-Vorgang teilnehmen, in den Ansichten der Entwurfszeit und Laufzeit unterschiedlich ist. Dieser Unterschied hat keinen Einfluss auf die Qualität/Granularität der von Ihnen gewählten Simulationshäufigkeit, sondern ist vielmehr ein Nebenprodukt der Laufzeitnutzung der in Voxel konvertierten Szene. Zur Laufzeit werden die Simulationsvoxel „optimiert“, um die Interpolation zwischen den Quellspeicherorten zu unterstützen. Dies ermöglicht auch die Entwurfszeitpositionierung von Tonquellen, die näher an den Szenengittermodellen liegen, als es die Simulationsvoxelgröße zulässt – da Quellen innerhalb eines Voxels, die ein akustisch behandeltes Gittermodell enthalten, keinen Ton erzeugen.

Hier folgen zwei Bilder, die den Unterschied zwischen Entwurfszeitvoxel („vor dem Bake-Vorgang“) und Laufzeitvoxel („nach dem Bake-Vorgang“) zeigen, wie sie durch das Unity-Plug-In veranschaulicht werden:

Entwurfszeitvoxel:

![VoxelsDesignTime](media/VoxelsDesignTime.png)

Laufzeitvoxel:

![VoxelsRuntime](media/VoxelsRuntime.png)

Die Entscheidung, ob das Voxelgittermodell die Architektur-/Struktur-Szenengittermodell genau repräsentiert oder nicht, sollte mithilfe der Entwurfsmodusvoxel und nicht der Laufzeitdarstellung der optimierten Voxel getroffen werden.

## <a name="post-bake-design"></a>Entwurf nach dem Bake-Vorgang
Ergebnisse des Bake-Vorgangs werden in der ACE-Datei als Okklusions- und Hallparameter für alle Quellen- und Zuhörerpositionspaare in der gesamten Szene gespeichert. Dieses physisch exakte Ergebnis können Sie unverändert für Ihr Projekt verwenden. Es stellt ein hervorragender Ausgangspunkt für den Entwurf dar. Beim Entwurf nach dem Bake-Vorgang werden Regeln für die Transformation der Baking-Ergebnisparameter zur Laufzeit festgelegt.

### <a name="distance-based-attenuation"></a>Distanzabhängige Dämpfung
Der vom Spatializer-Plug-In von **Microsoft Acoustics** Unity bereitgestellte Audio-DSP berücksichtigt die im Unity-Editor integrierte distanzabhängige Dämpfung pro Quelle. Steuerelemente für die distanzabhängige Dämpfung befinden sich in der Komponente **Audioquelle** im Bereich **Inspektor** von Tonquellen unter **3D Sound Settings** (3D-Soundeinstellungen):

![Distanzdämpfung](media/distanceattenuation.png)

Acoustics führt die Berechnung in einem „Simulationsbereichsfeld“ durch, das um die Position des Players herum angeordnet ist. Wenn eine Tonquelle vom Player entfernt ist, die sich außerhalb dieses Simulationsbereichs befindet, beeinflusst nur die Geometrie innerhalb des Felds die Schallausbreitung (z. B. durch Okklusion), was recht gut funktioniert, wenn sich Okkluder in der Nähe des Players befinden. In Fällen, in denen sich der Player jedoch im offenen Raum befindet, sich die Okkluder aber in der Nähe der fernen Tonquelle befinden, kann der Ton unrealistisch disokkludiert werden. Als Umgehung wird empfohlen, in solchen Fällen sicherzustellen, dass die Schallschwächung bei ca. 45 m auf 0 absinkt, dem voreingestellten horizontalen Abstand des Players zum Feldrand.

### <a name="tuning-scene-parameters"></a>Optimieren von Szenenparametern
Klicken Sie zum Anpassen von Parametern für alle Quellen auf den Kanalstreifen im **Audiomixer** von Unity, und passen Sie die Parameter für den Effekt **Project Acoustics Mixer** (Projektakustikmixer) an.

![Mixeranpassung](media/MixerParameters.png)

* **Wetness Adjust** (Feuchtigkeitsanpassung): passt die Hallstärke (in dB) für alle Quellen in der Szene basierend auf der Entfernung des Hörers an. Positive Werte verstärken den Hall, während negative Werte für einen Ton mit weniger Hall sorgen.
* **RT60 Scale** (RT60-Skala): multiplikatives Skalar für die Hallzeit.
* **Use Panning** (Panorama verwenden): steuert, ob die Audioausgabe binaural (0) oder als Mehrkanalpanorama (1) erfolgt. Sämtliche Werte außer 1 stehen für eine binaurale Ausgabe. Die Räumlichkeit der binauralen Ausgabe wird für die Verwendung mit Kopfhörern mithilfe von HRTFs und bei der Mehrkanalausgabe für die Verwendung mit Mehrkanal-Surround-Lautsprechersystemen mithilfe von VBAP erreicht. Wenn Sie das Mehrkanalpanorama verwenden, wählen Sie einen Lautsprechermodus entsprechend Ihren Geräteeinstellungen aus, den Sie unter **Projekteinstellungen** > **Audio** finden.

![SpeakerMode](media/SpeakerMode.png)

### <a name="tuning-source-parameters"></a>Optimieren von Quellenparametern
Durch Anfügen des **AcousticsAdjust**-Skripts an eine Quelle können die Parameter für diese Quelle optimiert werden. Wenn Sie das Skript anfügen möchten, klicken Sie auf **Komponente hinzufügen** im unteren Bereich des Abschnitts **Inspektor** und navigieren zu **Skripts > Acoustics Adjust** (Anpassung der Akustik). Das Skript verfügt über sechs Steuerelemente:

![AcousticsAdjust](media/AcousticsAdjust.png)

* **Enable Acoustics** (Akustik aktivieren): Bestimmt, ob auf diese Quelle Akustikeffekte angewendet werden. Ist diese Option deaktiviert, wird die Quelle mithilfe von HRTFs oder der Panoramafunktion räumlich gemacht, es sind dann aber keine Akustikeinstellungen möglich. Dies bedeutet, dass keine Parameter für Hindernisse, verdeckende Elemente oder dynamischen Hall, wie z.B. der Stärke und Abfallzeit, verfügbar sind. Halleffekte werden dennoch angewendet, jedoch mit einem festen Pegel und einer festen Abklingzeit.
* **Occlusion** (Okklusion): Wenden Sie einen Multiplikator auf den vom Akustiksystem berechneten dB-Wert der Okklusion an. Wenn dieser Multiplikator größer als 1 ist, wird die Okklusion überspitzt, während Werte kleiner als 1 zu einem subtileren Okklusionseffekt führen, und ein Wert von 0 deaktiviert die Okklusion.
* **Transmission (dB)** (Übertragung): Legen Sie die Schallschwächung (in dB) fest, die durch die Übertragung durch die Geometrie verursacht wird. Stellen Sie diesen Schieberegler auf die niedrigste Stufe, um die Übertragung zu deaktivieren. Die Akustik legt das anfänglich trockene Audio räumlich so fest, dass es um die Szenengeometrie herum eintrifft (Portalisierung). Die Übertragung bietet einen zusätzlichen trockenen Eingang, der in Sichtrichtung verräumlicht ist. Beachten Sie, dass auch die Distanzdämpfungskurve für die Quelle angewendet wird.
* **Wetness (dB)** (Verwässerung): Stellt die Hallleistung in dB entsprechend der Entfernung zur Quelle ein. Positive Werte verstärken den Hall, während negative Werte für einen Ton mit weniger Hall sorgen. Klicken Sie auf das Kurvensteuerelement (grüne Linie), um den Kurveneditor aufzurufen. Ändern Sie die Kurve, indem Sie mit der linken Maustaste klicken, um Punkte hinzuzufügen, und diese Punkte ziehen, um die gewünschte Funktion zu bilden. Die x-Achse ist der Abstand zur Quelle und die y-Achse die Hallkorrektur in dB. Weitere Informationen zur Bearbeitung von Kurven finden Sie in diesem [Unity-Leitfaden](https://docs.unity3d.com/Manual/EditingCurves.html). Um die Kurve wieder auf den Standardwert zurückzusetzen, klicken Sie mit der rechten Maustaste auf **Wetness** (Verwässerung) und wählen Sie **Reset** (Zurücksetzen) aus.
* **Decay Time Scale** (Abklingzeitskala): Passt den Multiplikator für die Abklingzeit an. Wenn im Ergebnis des Bake-Vorgangs beispielsweise eine Abklingzeit von 750 Millisekunden angegeben ist, dieser Wert jedoch auf 1,5 festgelegt wurde, beträgt die auf die Quelle angewendete Abklingzeit 1,125 Millisekunden.
* **Outdoorness** („im Freien-Wert“): Eine additive Anpassung der Schätzung des Akustiksystems, wie stark der Hall auf einer Quelle nach „im Freien“ klingen soll. Wenn Sie diesen Wert auf 1 festlegen, klingt eine Quelle immer vollständig so, als ob sie im Freien wäre, während eine Quelle bei der Einstellung -1 nach Innenbereich klingt.

Für unterschiedliche Quellen sind möglicherweise unterschiedliche Einstellungen erforderlich, um bestimmte ästhetische Effekte oder Gameplay-Effekte zu erzielen. Dialoge sind ein gutes Beispiel. Das menschliche Gehör ist durchaus auf den Nachhall in der Sprache eingestellt, jedoch müssen Dialoge bei Spielen in erster Linie verständlich sein. Sie können dies berücksichtigen, ohne den Dialog nicht-diegetisch zu machen, indem Sie den **Wetness-Wert** (Verwässerung) nach verringern, den unten beschriebenen Parameter **Perceptual Distance Warp** (Wahrnehmungsdistanzverzerrung) anpassen, einige **Übertragungen** für einen trockenen Audioanstieg hinzufügen, der sich durch Wände ausbreitet, und/oder die **Occlusion** (Okklusion) von 1 verringern, damit mehr Sound über Portale ankommt.

Durch Anfügen des **AcousticsAdjustExperimental**-Skripts an eine Quelle können die zusätzlichen experimentellen Parameter für diese Quelle optimiert werden. Wenn Sie das Skript anfügen möchten, klicken Sie auf **Komponente hinzufügen** im unteren Bereich des Abschnitts **Inspektor** und navigieren zu **Skripts > Acoustics Adjust Experimental** (experimentelle Anpassung der Akustik). Es gibt derzeit ein experimentelles Steuerelement:

![AcousticsAdjustExperimental](media/AcousticsAdjustExperimental.png)

* **Perceptual Distance Warp** (Wahrnehmungsdistanzverzerrung): Wenden Sie eine exponentielle Verzerrung auf die Entfernung an, die zur Berechnung des Dry-Wet-Verhältnisses (Verhältnis von Trockenheit zur Verwässerung) verwendet wird. Das Akustiksystem berechnet Verwässerungspegel im gesamten Raum, die mit der Entfernung stufenlos variieren und wahrnehmbare Entfernungshinweise liefern. Verzerrungswerte größer als 1 verstärken diesen Effekt noch, da die entfernungsbedingte Hallstärke erhöht wird, sodass der Sound „entfernt“ klingt. Verzerrungswerte kleiner als 1 verringern den entfernungsbasierten Hall, sodass der Sound „präsenter“ klingt.

