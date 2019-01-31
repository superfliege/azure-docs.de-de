---
title: 'Was ist „Projekt: Akustik“?'
titlesuffix: Azure Cognitive Services
description: 'Das Unity-Plug-In für „Projekt: Akustik“ bietet Okklusion, Nachhall und Verräumlichung für Projekte, die auf VR und herkömmliche Bildschirme ausgerichtet sind.'
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 175203269f524f5c680345602e6382f5c26f1bd1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178901"
---
# <a name="what-is-project-acoustics"></a>Was ist „Projekt: Akustik“?
Das Unity-Plug-In für „Projekt: Akustik“ bietet Okklusion, Nachhall und Verräumlichung für Projekte, die auf VR und herkömmliche Bildschirme ausgerichtet sind. Es bietet eine Möglichkeit zum Entwickeln von Spielakustik, bei der die Absichten des Entwicklers über eine physikbasierte Wellensimulation gelagert werden.

## <a name="why-use-acoustics-in-virtual-environments"></a>Was spricht für die Verwendung von Akustik in virtuellen Umgebungen?
Menschen finden sich anhand von audiovisuellen Signalen in ihrer Umgebung zurecht. In virtuellen Welten ermöglicht das Kombinieren von räumlichem Klang mit Akustik ein tieferes Eintauchen der Benutzer. Das hier beschriebene Akustiktool analysiert virtuelle Welten, um eine realistische Akustiksimulation zu erstellen, und unterstützt einen Entwurfsprozess im Anschluss an die Simulation. Die Analyse beinhaltet sowohl die Geometrie als auch die Materialien für jede Oberfläche der Welt. Die Simulation umfasst Parameter wie die Eingangsrichtung (Portalisierung), Hallstärke, Abklingzeiten sowie Okklusions- und Obstruktionseffekte.

## <a name="how-does-this-approach-to-acoustics-work"></a>Wie funktioniert dieser Akustikansatz?
Das System basiert auf einer Offlineberechnung der virtuellen Welt, die eine komplexere Simulation als bei der Ausführung der Analyse zur Laufzeit ermöglicht. Die Offlineberechnung erzeugt eine Nachschlagetabelle von Akustikparametern. Ein Designer gibt Regeln an, die zur Laufzeit auf die Parameter angewendet werden. Durch Optimierung dieser Regeln können hyperrealistische Effekte für eine hohe emotionale Intensität oder hyporealistische Szenen für Klänge erzielt werden, die sich mehr im Hintergrund befinden.

## <a name="design-process-comparison"></a>Vergleich des Entwurfsprozesses
Das Plug-In für „Projekt: Akustik“ unterstützt einen neuen Entwurfsprozess für Akustik in Unity-Szenen. Um diesen neuen Entwurfsprozess zu erläutern, vergleichen wir ihn mit einer der heutzutage gebräuchlichen Herangehensweisen an Akustik.

### <a name="typical-approach-to-acoustics-today"></a>Typische derzeitige Herangehensweise an Akustik
Bei einem typischen heutigen Akustikansatz wird die Lautstärke des Halls gezeichnet:

![Entwurfsansicht](media/reverbZonesAltSPace2.png)

Dann werden die Parameter für die einzelnen Zonen optimiert:

![Entwurfsansicht](media/TooManyReverbParameters.png)

Zum Schluss wird Raytracing-Logik hinzugefügt, um in der gesamten Szene die richtige Okklusions- und Obstruktionsfilterung zu erzielen, sowie Pfadsuchlogik für Portalisierung. Dieser Code kann Laufzeitkosten hinzufügen. Außerdem liegen Probleme mit der Gleichmäßigkeit an Ecken sowie Grenzfälle bei unregelmäßig geformten Szenen vor.

### <a name="an-alternative-approach-with-physics-based-design"></a>Ein alternativer Ansatz mit physikbasiertem Entwurf
Mit dem Ansatz des Unity-Plug-ins für „Projekt: Akustik“ stellen Sie Form und Materialien einer statischen Szene bereit. Da die Szene voxelisiert ist und der Prozess kein Raytracing verwendet, müssen Sie kein vereinfachtes oder dichtes Akustiknetz bereitstellen. Ebenso ist es nicht erforderlich, die Szene mit der Lautstärke des Halls zu markieren. Das Plug-In lädt die Szene in die Cloud hoch, wo sie physikbasierte Wellensimulation verwendet. Das Ergebnis wird als Nachschlagetabelle in Ihr Projekt integriert und kann für Ästhetik- oder Gaming-Effekte angepasst werden.

![Entwurfsansicht](media/GearsWithVoxels.jpg)

## <a name="requirements"></a>Requirements (Anforderungen)
* Unity 2018.2+ für Akustikintegrationen und Unity 5.2+ für Sounddesign- und bereitstellung
* Unity Editor für Windows (64-Bit)
* Azure Batch-Abonnement für Akustikintegrationen
* Unity-Skriptlaufzeit muss auf „.NET 4.x Equivalent“ festgelegt sein.

## <a name="platform-support"></a>Plattformunterstützung
* Windows Desktop (x86 und AMD64)
* Windows UWP (x86, AMD64 und ARM)
* Android (x86 und ARM64)

## <a name="download"></a>Download
Wenn Sie an der Evaluierung des Akustik-Plug-Ins interessiert sind, registrieren Sie sich [hier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u), um die Designer-Vorschauversion nutzen zu können.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum [Entwurfsprozess](design-process.md)
* Erste Schritte mit der [Integration von Akustik in Ihr Unity-Projekt](getting-started.md)

