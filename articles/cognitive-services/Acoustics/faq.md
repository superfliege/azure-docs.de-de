---
title: Häufig gestellte Fragen zu Project Acoustics
titlesuffix: Azure Cognitive Services
description: Diese Seite enthält Antworten auf häufig gestellte Fragen zu Project Acoustics, darunter auch Fragen zu Downloadanweisungen sowie zum Bake-Vorgang.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 07e65e6ff544d6372197010a2b9d7f3f647eeb0f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55166559"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

## <a name="what-is-project-acoustics"></a>Was ist „Projekt: Akustik“?

Beim Unity-Plug-In „Project Acoustics“ handelt es sich um ein Akustiksystem, mit dem das Verhalten von Akustikwellen ähnlich wie bei der statischen Beleuchtung vor der Laufzeit berechnet wird. Der Großteil der Wellenphysikberechnungen wird von der Cloud erledigt, sodass die CPU-Kosten zur Laufzeit niedrig sind.  

## <a name="where-can-i-download-the-plugin"></a>Wo kann ich das Plug-In herunterladen?

Wenn Sie an der Evaluierung des Akustik-Plug-Ins interessiert sind, registrieren Sie sich [hier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u), um die Designer-Vorschauversion nutzen zu können.

## <a name="is-azure-used-at-runtime"></a>Wird Azure zur Laufzeit verwendet?

Nein, die Cloudintegration wird nur während der Vorausberechnungsphase als Teil der Szeneneinrichtung verwendet.
 
## <a name="what-is-simulation-input"></a>Was ist eine Simulationseingabe? 

Die Simulationseingabe ist Ihre 3D-Szene, die virtuelle Umgebung oder die Spielstufe. Von Project Acoustics werden volumetrische Wellensimulationen in 3D durchgeführt, mit denen die Schallphysik wie etwa eine weiche Okklusion und Streuung genau modelliert wird.
 
## <a name="what-is-the-runtime-cost"></a>Was sind Laufzeitkosten?

Acoustics belegt etwa 0,01 % der CPU pro Quelle pro Frame. Die Speicherauslastung hängt von der Größe der Szene ab und kann zwischen 10 und 100 MB liegen.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>Muss ich die Pegelgeometrie vereinfachen? Muss ich die Anzahl der Dreiecke steuern? Muss ich Gittermodelle wasserdicht machen?

 Nein. Das System erfasst die detaillierte Pegelgeometrie direkt. Sie wird zur internen Verarbeitung in Voxels konvertiert.
 
## <a name="whats-in-the-runtime-lookup-table"></a>Was befindet sich in der Nachschlagetabelle für die Runtime?

Bei der ACE-Datei handelt es sich um eine Tabelle mit akustischen Parametern zwischen zahlreichen Quellen- und Zuhörerpositionspaaren.
 
## <a name="can-it-handle-moving-sources"></a>Kann sie mit beweglichen Quellen umgehen?

Ja, das Unity-Spatializer-Plug-In von **Microsoft Acoustics** schlägt bei jedem Audioverarbeitungstakt mit den aktuellen Quellen- und Zuhörerpositionen in der Nachschlagetabelle nach. Der DSP des Spatializers aktualisiert die akustischen Verarbeitungsparameter bei jedem Takt reibungslos.
 
## <a name="can-it-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>Kann er dynamische Geometrie verarbeiten? Oder schließende Türen? Oder Wände, die entfernt werden?

 Nein. Die akustischen Parameter werden basierend auf dem statischen Zustand einer Spielstufe vorausberechnet. Wir empfehlen, die Türgeometrie aus der Akustik auszuklammern und anschließend basierend auf dem Zustand von zerstörbaren und beweglichen Spielobjekten mithilfe bewährter Verfahren zusätzliche Okklusion anzuwenden.
 
## <a name="does-it-handle-materials"></a>Kann er Materialien verarbeiten?

Ja. Materialien werden zur Steuerung des Absorptionsgrads aus den physischen Materialnamen in Ihrer Stufe ausgewählt.
 
## <a name="what-do-the-probes-represent"></a>Was bedeuten die „Tests“?

Tests sind Stichproben von möglichen Spielerpositionen. Jeder Test stellt eine separate Wellensimulation der Szene dar, die an der Testposition entsteht. Zur Laufzeit werden akustische Parameter für die Zuhörerposition aus nahegelegenen Testpositionen interpoliert.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>Warum sollen so viele Computestunden in die Cloud verlegt werden? Was bringt mir das?

Project Acoustics stellt exakte und zuverlässige akustische Parameter selbst für extrem komplexe virtuelle Umgebungen dar und berücksichtigt dabei sämtliche architektonischen Aspekte. Das Plug-In stellt eine weiche Okklusion/Verzögerung ohne manuelles Eingreifen und dynamische Hallvariationen ohne Volumenabzug bereit. Und dabei belastet es die CPU während der Laufzeit kaum.

## <a name="what-exactly-happens-during-baking"></a>Was geschieht beim „Bake-Vorgang“ genau?

Das System zieht mögliche Spielerpositionen in Betracht, um eine Reihe von Beispielpositionen von „Tests“ mit einheitlichem Abstand zu generieren. Ein Bake-Vorgang für eine Stufe besteht aus unabhängigen Aufgaben für jeden Test: Das System berücksichtigt einen kubischen „Simulationsbereich“ in der Mitte des Tests und führt eine ausführliche Wellensimulation mit einer Auflösung von bis zu 25 cm in diesem Bereich durch.

## <a name="next-steps"></a>Nächste Schritte
* Untersuchen der [Beispielszene](sample-walkthrough.md)

