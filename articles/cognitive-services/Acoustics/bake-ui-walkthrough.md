---
title: Durchführen eines Akustik-Bake-Vorgangs mit Project Acoustics
titlesuffix: Azure Cognitive Services
description: In diesem Dokument wird das Übermitteln eines Akustik-Bake-Vorgangs mithilfe der Unity-Editor-Erweiterung erläutert.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.component: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: c37e050cd762cb173d64f78b5267e4ad252d17a9
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902245"
---
# <a name="bake-acoustics"></a>Baken von Akustik

In diesem Dokument wird das Übermitteln eines Akustik-Bake-Vorgangs mithilfe der Unity-Editor-Erweiterung erläutert. Weitere Informationen zu Akustik finden Sie unter [Was ist „Projekt: Akustik“?](what-is-acoustics.md). Eine Anleitung für den schnellen Einstieg finden Sie unter [Erste Schritte](getting-started.md).

## <a name="import-the-plugin"></a>Importieren des Plug-Ins

Importieren Sie das Akustik-Plug-In-Paket in Ihr Projekt. Öffnen Sie anschließend die Acoustics-Benutzeroberfläche, indem Sie im Unity-Menü auf **Window > Acoustics** (Fenster > Akustik) klicken:

![Öffnen des Fensters „Acoustics“ (Akustik)](media/WindowAcoustics.png)

## <a name="principles"></a>Prinzipien

Das Toolfenster „Acoustics“ (Akustik) sammelt die Informationen, die die Akustik-Engine benötigt, um die Akustik für Ihre Szene zu berechnen. Für einen Bake-Vorgang sind fünf Schritte erforderlich:

1. Erstellen oder markieren Sie Ihr Navigationsgittermodell für den Spieler.
2. Markieren Sie die Geometrie der Akustik.
3. Weisen Sie der Geometrie Akustikmaterialeigenschaften zu.
4. Zeigen Sie eine Vorschau der Testplatzierung an.
5. Führen Sie den Bake-Vorgang durch.

Nach dem Bake-Vorgang finden Sie unter [Design Process Overview for Acoustics (Übersicht über den Entwurfsprozess für Akustik)](design-process.md) weitere Informationen zu optionalen Entwurfsschritten.

## <a name="create-or-mark-a-navigation-mesh"></a>Erstellen oder Markieren eines Navigationsgittermodells

Ein Navigationsgittermodell wird verwendet, um Testpunkte für die Simulation zu platzieren. Sie können den [Navigationsgittermodell-Workflow](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html) von Unity verwenden oder ein eigenes Navigationsgittermodell angeben. Navigationsgittermodelle, die mit dem Unity-Workflow erstellt werden, werden vom Akustiksystem übernommen. Wenn Sie eigene Gittermodelle verwenden möchten, markieren Sie diese auf der Registerkarte **Objects** (Objekte). Dies wird im nächsten Schritt beschrieben.

## <a name="objects-tab"></a>Registerkarte „Objects“ (Objekte)

Öffnen Sie die Registerkarte **Objects** des Fensters **Acoustics**. Verwenden Sie diese Registerkarte, um Objekte in Ihrer Szene zu markieren. Dadurch werden die Komponenten **AcousticsGeometry** oder **AcousticsNavigation** zum Objekt hinzugefügt. Sie können ebenfalls den [Unity-Standardworkflow für Komponenten](https://docs.unity3d.com/Manual/UsingComponents.html) verwenden, um Objekte zu markieren oder Markierungen aufzuheben.

Wählen Sie ein oder mehrere Objekte im Szenenfenster oder in der Hierarchie aus, und markieren Sie diese wie im Folgenden beschrieben für **AcousticsGeometry** oder **AcousticsNavigation**, oder heben Sie die Markierung auf. Wenn kein Element ausgewählt ist, können Sie alle Elemente in der Szene auf einmal markieren (oder die Markierung für alle aufheben).

Nur Gittermodellrenderer und Geländeobjekte können markiert werden. Alle anderen Objekttypen werden ignoriert. Über die Kontrollkästchen werden alle entsprechenden Objekte markiert (oder die Markierung wird aufgehoben).

Wenn in Ihrer Szene nichts ausgewählt ist, sieht diese folgendermaßen aus:

![Registerkarte „Objects“ ohne Auswahl](media/ObjectsTabNoSelectionDetail.png)

Wenn in Ihrem Szenen- oder Hierarchiefenster Elemente ausgewählt sind, sehen diese folgendermaßen aus:

![Registerkarte „Objects“ ohne Auswahl](media/ObjectsTabWithSelectionDetail.png)

### <a name="objects-tab-parts"></a>Bestandteile der Registerkarte „Objects“ (Objekte)

Die Registerkartenseite weist folgende Bestandteile auf:

1. Schaltflächen für die Registerkartenauswahl (Registerkarte **Objects** ist ausgewählt). Verwenden Sie diese Schaltflächen, um die Schritte eines Bake-Vorgangs für Akustik von links nach rechts zu durchlaufen.
2. Eine kurze Beschreibung, welche Vorgänge Sie mit dieser Seite durchführen müssen.
3. Verfügbare Filter für das Hierarchiefenster. Verwenden Sie diese, um das Hierarchiefenster nach Objekten des angegebenen Typs zu filtern, um diese einfacher markieren zu können. Wenn Sie bisher nichts für Akustik ausgewählt haben, wird nichts angezeigt, wenn Sie die letzten beiden Optionen auswählen. Diese können jedoch nützlich sein, um bei Bedarf markierte Objekte zu suchen.
4. Wenn keine Objekte ausgewählt sind, zeigt dieser Abschnitt den Status aller Objekte in der Szene an:
    * Total (Gesamt): Die Gesamtanzahl der aktiven, nicht ausgeblendeten Objekte in der Szene.
    * Ignored (Ignoriert): Die Anzahl der Objekte, die keine Gittermodellrenderer oder Geländeobjekte sind.
    * Mesh (Gittermodell): Die Anzahl der Gittermodellrendererobjekte in der Szene
    * Terrain (Gelände): Die Anzahl der Geländeobjekte in der Szene
    * Geometry (Geometrie): Die Anzahl der Gittermodell- oder Geländeobjekte in der Szene, die als „Acoustics Geometry“ (Akustikgeometrie) markiert sind.
    * Navigation: Die Anzahl der Gittermodell- oder Geländeobjekte in der Szene, die als „Acoustics Navigation“ (Akustiknavigation) markiert sind. In dieser Anzahl ist das Navigationsgittermodell von Unity (NavMesh) nicht eingeschlossen.
5. Die Gesamtanzahl der „markierbaren“ Objekte in der Szene (nur Gittermodellrenderer und Geländeobjekte). Die Kontrollkästchen, die Sie zum Markieren (d.h. zum Hinzufügen der entsprechenden Komponente) dieser Objekte als „Geometrie“ oder „Navigation“ für Akustik verwenden können.
6. Wenn keine Objekte ausgewählt sind, erinnert dieser Hinweis Sie daran, bei Bedarf Objekte zu markieren. Sie können ebenfalls ein Kontrollkästchen (oder beide) aktivieren, um alle Objekte in der Szene zu markieren, ohne eine Auswahl vorzunehmen.
7. Wenn Objekte ausgewählt sind, zeigt dieser Abschnitt nur den Status der ausgewählten Objekte an.
8. Die Gesamtanzahl der „markierbaren“ ausgewählten Objekte. Durch das Aktivieren oder Deaktivieren der Kontrollkästchen werden nur die ausgewählten Objekte markiert (bzw. die Markierung wird aufgehoben).

Wenn einige Objekte markiert sind und einige nicht, zeigt das entsprechende Kontrollkästchen einen „gemischten“ Wert an:

![Kontrollkästchen mit gemischtem Wert](media/MixedObjectSelectionDetail.png)

Wenn Sie das Kontrollkästchen aktivieren, werden alle Objekte markiert. Wenn Sie es deaktivieren, wird die Markierung für alle Objekte aufgehoben.

Objekte können für Geometrie und Navigation markiert werden.

### <a name="guidelines-for-marking-objects"></a>Richtlinien zum Markieren von Objekten

Stellen Sie sicher, dass Sie alle Objekte als **Acoustics Geometry** (Akustikgeometrie) markieren, die Sounds verdecken, reflektieren oder absorbieren sollen. Objekte für die Akustikgeometrie können z.B. Boden, Wände, Dächer, Fenster, Fensterglas, Teppiche oder große Möbelstücke umfassen. Es ist kein Problem, kleinere Objekte wie Lampen, Dekorationsgegenstände und Lichtinstallationen hinzuzufügen, da diese den Aufwand für den Bake-Vorgang nicht nennenswert erhöhen. Achten Sie unbedingt auf wichtige Elemente wie den Boden oder die Decke. Sie sollten ebenfalls keine Objekte hinzufügen, die sich nicht auf die Akustik auswirken, z.B. Gittermodelle für Kollisionen.

Die Transformation eines Objekts zum Zeitpunkt der Testberechnung (über die Registerkarte **Probes** (Tests)) wird in den Ergebnissen des Bake-Vorgangs festgelegt. Wenn Sie eines der markierten Objekte in der Szene bewegen, müssen Sie die Testberechnung und den Bake-Vorgang für die Szene wiederholen.

## <a name="materials-tab"></a>Registerkarte „Materials“ (Materialien)

Wenn Ihre Objekte markiert sind, klicken Sie auf die Schaltfläche **Materials** (Materialien), um zur Registerkarte „Materials“ (Materialien) zu gelangen.

### <a name="parts-of-the-materials-tab"></a>Bestandteile der Registerkarte „Materials“ (Materialien)

![Details der Registerkarte „Materials“ (Materialien)](media/MaterialsTabDetail.png)

1. Die Schaltfläche **Materials** (Materialien), die zum Aufrufen dieser Seite verwendet wird.
2. Eine kurze Beschreibung, welche Vorgänge Sie mit dieser Seite durchführen müssen.
3. Wenn dieses Kontrollkästchen aktiviert ist, werden nur Materialien aufgeführt, die von Objekten verwendet werden, die als **Acoustics Geometry** (Akustikgeometrie) markiert sind. Andernfalls werden alle Materialien aufgeführt, die in dieser Szene verwendet werden.
4. Verwenden Sie diese Optionen, um die Reihenfolge des Dropdownmenüs zu ändern, das angezeigt wird, wenn Sie auf einen Dropdownpfeil in der unten dargestellten Acoustics-Spalte (6) klicken. **Name** sortiert die Akustikmaterialien nach Name. „Absorptivity“ (Absorptionsgrad) sortiert diese aufsteigend nach Absorptionsgrad.
5. Die Liste der in dieser Szene verwendeten Materialien (alphabetisch sortiert). Wenn das Kontrollkästchen **Show Marked Only** (Nur markierte anzeigen) (3) aktiviert ist, werden nur Materialien angezeigt, die von Objekten verwendet werden, die als **Acoustics Geometry** (Akustikgeometrie) markiert sind. Wenn Sie hier auf eines der Materialien klicken, werden alle Objekte in der Szene ausgewählt, die dieses Material verwenden.
6. Anzeige der Akustikmaterialien, denen die Szenenmaterialien zugewiesen wurden. Klicken Sie auf eine Dropdownliste, um ein Szenenmaterial einem anderen Akustikmaterial zuzuweisen. Sie können die Sortierreihenfolge des anzeigten Menüs ändern, wenn Sie mithilfe der Optionen **Sort Acoustics By:** (Akustik sortieren nach:) (4) auf ein Element klicken.
7. Zeigt den Absorptionskoeffizienten des ausgewählten Akustikmaterials an, das in der vorherigen Spalte ausgewählt wurde. Ein Wert von „0“ bedeutet „perfekte Reflektion“ (keine Absorption), während ein Wert von „1“ „perfekte Absorption“ (keine Reflektion) bedeutet. Der Absorptionskoeffizient kann nicht geändert werden, sofern das ausgewählte Material nicht vom Typ „Custom“ (Benutzerdefiniert) ist.
8. Für ein auf „Custom“ (Benutzerdefiniert) festgelegtes Material ist der Schieberegler nicht mehr deaktiviert, und Sie können den Absorptionskoeffizienten mithilfe des Schiebereglers oder durch Eingeben eines Werts auswählen. Weitere Informationen zu Materialeigenschaften finden Sie unter [Design Process Overview for Acoustics (Übersicht über den Entwurfsprozess für Akustik)](design-process.md).

### <a name="guidelines-for-assigning-materials-or-absorption-values"></a>Richtlinien für die Zuweisung von Materialien (oder Absorptionswerten)

Diese Registerkarte schätzt die Absorption Ihrer Materialien anhand des Namens. Wenn der Name Ihres Szenenmaterials beispielsweise „LivingRoom_WoodTable“ ist, wird das Akustikmaterial zunächst „wood“ (Holz) zugewiesen. Materialien, bei denen kein bekanntes Material bestimmt werden kann, wird der Typ „Standard“ zugewiesen, dessen Absorption ähnlich wie bei „Concrete“ (Beton) ist.

Sie können Akustikmaterialien jedem Szenenmaterial neu zuordnen. Wenn der Hall in einem Raum beispielsweise zu stark ist, können Sie das Akustikmaterial der Wände, Böden und Decken in ein Material mit höherer Absorption ändern. Die Zuweisung des Akustikmaterials gilt für alle Objekte, die dieses Szenenmaterial verwenden.

## <a name="probes-tab"></a>Registerkarte „Probes“ (Tests)

Wenn Sie die Materialien zugewiesen haben, wechseln Sie zur Registerkarte **Probes** (Tests).

### <a name="parts-of-the-probes-tab"></a>Bestandteile der Registerkarte „Probes“ (Tests)

![Details der Registerkarte „Probes“ (Tests)](media/ProbesTabDetail.png)

1. Die Schaltfläche **Probes** (Tests), die zum Aufrufen dieser Seite verwendet wird.
2. Eine kurze Beschreibung, welche Vorgänge Sie mit dieser Seite durchführen müssen.
3. Verwenden Sie diese Kontrollkästchen, um „Coarse“ (Niedrig) oder „Fine“ (Hoch) als Auflösung für die Simulation auszuwählen. Eine niedrige Auflösung ist schneller, hat jedoch einige Nachteile. Weitere Informationen finden Sie im Verlauf des Artikels unter [Niedrige und hohe Auflösung im Vergleich](#Coarse-vs-Fine-Resolution).
4. Wählen Sie mithilfe dieses Felds den Speicherort für die Akustikdatendateien aus. Klicken Sie auf die Schaltfläche mit den Auslassungspunkten (...), um die Ordnerauswahl zu verwenden. Der Standardordner ist **Assets/AcousticsData**. An diesem Speicherort wird ebenfalls ein **Editor**-Unterordner erstellt. Weitere Informationen zu Datendateien finden Sie im Verlauf dieses Artikels unter [Datendateien](#Data-Files).
5. Die Datendateien für diese Szene werden mithilfe des hier bereitgestellten Präfixes benannt. Der Standardname ist „Acoustics_[Scene Name]“.
6. Nachdem die Tests berechnet wurden, werden die oben genannten Steuerelemente deaktiviert. Klicken Sie auf die Schaltfläche **Clear** (Löschen), um die Berechnungen zu löschen und die Steuerelemente zu aktivieren, damit Sie unter Verwendung der neuen Einstellungen eine Neuberechnung vornehmen können.
7. Klicken Sie auf die Schaltfläche **Calculate...** (Berechnen...), um die Szene in Voxels zu konvertieren und die Positionen der Testpunkte zu berechnen. Dies wird lokal auf Ihrem Computer durchgeführt und muss daher vor einem Bake-Vorgang erfolgen.

In dieser Version von Project Acoustics können Tests nicht manuell platziert werden, sondern müssen über einen automatisierten Prozess platziert werden, der auf der Registerkarte **Probes** (Tests) bereitgestellt wird.

### <a name="what-the-calculate-button-calculates"></a>Berechnungen der Schaltfläche „Calculate...“ (Berechnen...)

Die Schaltfläche **Calculate...** (Berechnen...) verwendet alle bisher bereitgestellten Daten (Geometrie, Navigation, Materialien und die Einstellung „Niedrige/Hohe Auflösung“) und durchläuft mehrere Schritte:

1. Zunächst wird die Geometrie aus dem Szenengittermodell verwendet und ein Voxelvolumen berechnet. Das Voxelvolumen ist ein dreidimensionales Volumen, das die gesamte Szene umfasst und aus kleinen, kubischen „Voxeln“ besteht. Die Größe der Voxels richtet sich nach der Häufigkeit der Simulation, die durch die Einstellung **Simulation Resolution** (Simulationsauflösung) festgelegt wird. Für jedes Voxel wird markiert, ob es „frei“ ist oder Szenengeometrie enthält. Wenn ein Voxel Geometrie enthält, wird es mit dem Absorptionskoeffizienten des Materials markiert, das dieser Geometrie zugewiesen ist.
2. Anschließend werden die Navigationsdaten verwendet, um die für die Akustik relevanten Positionen zu berechnen, an die der Spieler sich möglicherweise bewegt. Es wird versucht, kleine Gruppen dieser Positionen zu suchen, die kleinere Bereiche wie Eingänge oder Flure bis hin zu Räumen und offenen Flächen enthalten. Für kleine Szene sind dies meist weniger als 100 Positionen, während große Szenen bis zu 1000 enthalten können.
3. Für jede der endgültigen Listenerpositionen, für die eine Berechnung durchgeführt wird, werden einige Parameter bestimmt (z.B., wie „offen“ die Fläche ist oder die Größe des Raums, in dem sich die Fläche befindet).
4. Die Ergebnisse dieser Berechnungen werden an dem von Ihnen festgelegten Speicherort als Datei gespeichert. Dies wird im Verlauf des Artikels unter [Datendateien](#Data-Files) erläutert.

Je nach Größe Ihrer Szene und der Geschwindigkeit Ihres Computers können die Berechnungen mehrere Minuten dauern.

Sobald die Berechnungen abgeschlossen wurden, können Sie eine Vorschau der Voxeldaten und der Position der Testpunkte anzeigen, um sicherzugehen, dass die Ergebnisse des Bake-Vorgangs gut sind. Ein ungültiges Navigationsgittermodell oder fehlende bzw. zusätzliche Geometrie sind in der Vorschau üblicherweise schnell zu erkennen, sodass Sie diese Fehler korrigieren können.

### <a name="scene-rename"></a>Umbenennung von Szenen

Der Szenenname wird verwendet, um die Szene mit den Dateien zu verbinden, in denen die Position und die Voxels des Testpunkts gespeichert sind. Wenn die Szene nach der Berechnung der Testpunkte umbenannt wird, gehen die Zuweisungs- und Platzierungsdaten des Materials verloren, und die Berechnung sollte erneut ausgeführt werden.

## <a name="debug-display-through-gizmos"></a>Debuggen von Anzeigen über Gizmos

Standardmäßig sind die Gizmos-Optionen **Probes** (Tests) und **Voxels** aktiviert. Diese zeigen die Voxels und die Positionen der Testpunkte an, die berechnet wurden. Sie können über das Gizmos-Menü aktiviert oder deaktiviert werden:

![Gizmos-Menü](media/GizmosMenu.png)

### <a name="voxels"></a>Voxels

Voxels werden im Szenenfenster als grüne Würfel um die beteiligte Geometrie angezeigt. Voxels, die nur „Luft“ enthalten, werden nicht angezeigt. Ihre gesamte Szene ist von einem großen grünen Kasten umschlossen, der das vollständige Voxelvolumen angibt, das während der Simulation verwendet wird.
Bewegen Sie die Szene, und stellen Sie sicher, dass alle Geometrie-Objekte Voxels enthalten. Die Szenenkamera muss sich in einem Radius von etwa 5 Metern um das Objekt befinden, damit die Voxels angezeigt werden.

Wenn Sie die erstellten Voxels mit niedriger und hoher Auflösung vergleichen, sehen Sie, dass die Voxels mit niedriger Auflösung zweimal so groß sind.

![Voxelvorschau](media/VoxelCubesPreview.png)

### <a name="probe-points"></a>Testpunkte

Die Testpunkte entsprechen den möglichen Positionen des Spielers (Listeners). Wenn Sie einen Bake-Vorgang durchführen, wird die Akustik für eine Soundquelle an einer beliebigen Position in der Szene für jeden dieser Testpunkte berechnet. Wenn der Spieler sich nicht direkt an der Position des Testpunkts befindet, werden die Daten von den Testpunkten verwendet, die sich am nächsten beim Spieler befinden, um die Parameter an dieser Position zu interpolieren.

Deshalb ist es wichtig, sicherzustellen, dass die Testpunkte überall dort vorhanden sind, wo der Spieler sich wahrscheinlich in der Szene bewegt, und dass die die kleinen Bereiche und Eingänge richtig dargestellt werden. Testpunkte werden von der Engine für Project Acoustics je nach Interpretation der Geometrie der Szene platziert und können in dieser Designer-Vorschau nicht verschoben oder bearbeitet werden. Verwenden Sie diese stattdessen, um die Richtigkeit Ihrer Geometriemarkierung und der Hinweisdaten der Navigation zu überprüfen.

![Testvorschau](media/ProbesPreview.png)

### <a name="Coarse-vs-Fine-Resolution"></a>Niedrige und hohe Auflösung im Vergleich

Der einzige Unterschied zwischen den Auflösungseinstellungen „Coarse“ (Niedrig) und „Fine“ (Hoch) ist die Häufigkeit, mit der die Simulation ausgeführt wird. Die Einstellung „Fine“ (Hoch) verwendet eine doppelt so hohe Häufigkeit wie „Coarse“ (Niedrig).
Dieses Prinzip erscheint einfach, hat jedoch einige Auswirkungen auf die Akustiksimulation:

* Die Wellenlänge für „Coarse“ (Niedrig) ist doppelt so lang wie bei „Fine“ (Hoch), deshalb sind die Voxels doppelt so groß.
* Die Simulationszeit hängt direkt von der Voxelgröße ab, deshalb ist ein Bake-Vorgang mit der Einstellung „Coarse“ (Niedrig) bis zu 16-mal schneller als mit der Einstellung „Fine“ (Hoch).
* „Portale“ (z.B. Türen oder Fenster), die kleiner als die Voxelgröße sind, können nicht simuliert werden. Die Einstellung „Coarse“ (Niedrig) kann dazu führen, dass einige der kleineren Portale nicht simuliert werden, deshalb übergeben diese zur Laufzeit keinen Sound. Sie können feststellen, ob dieser Fall eintritt, indem Sie die Voxels anzeigen.
* Die niedrigere Simulationshäufigkeit führt zu einer verringerten Beugung von Ecken und Kanten.
* Soundquellen können nicht in „aufgefüllten“ Voxels (also Voxels, die Geometrie enthalten) platziert werden, da ansonsten keine Sounds wiedergegeben werden. Es ist schwieriger, Soundquellen so zu platzieren, dass diese sich nicht in den größeren Voxels der Einstellung „Coarse“ (Niedrig) befinden, als es bei der Einstellung „Fine“ (Hoch) der Fall ist.
* Die größeren Voxels überschneiden sich wie im Folgenden dargestellt mehr mit Portalen. Das erste Bild dieses Eingangs wurde mit der Auflösung „Coarse“ (Niedrig) erstellt, das zweite mit der Auflösung „Fine“ (Hoch). An den roten Markierungen ist erkennbar, dass die Überschneidung mit der Einstellung „Fine“ (Hoch) wesentlich geringer ist. Die blaue Linie stellt den durch die Geometrie definierten Eingang dar, während die rote Linie das Akustikportal darstellt, das von der Voxelgröße definiert wird. Wie diese Überschneidung sich in einer bestimmten Situation auswirkt, hängt vollständig davon ab, wie die Voxels und die Geometrie des Portals ausgerichtet sind. Dies wird von der Größe und Position der Objekte in der Szene bestimmt.

![Eingang mit niedriger Auflösung](media/CoarseVoxelDoorway.png)

![Eingang mit hoher Auflösung](media/FineVoxelDoorway.png)

## <a name="bake-tab"></a>Registerkarte „Bake“

Sobald Sie mit den Vorschaudaten zufrieden sind, können Sie die Registerkarte **Bake** verwenden, um Ihren Bake-Vorgang an die Cloud zu übermitteln.

### <a name="parts-of-the-bake-tab"></a>Bestandteile der Registerkarte „Bake“

![Details der Registerkarte „Bake“](media/BakeTabDetails.png)

1. Die Schaltfläche Bake, die zum Aufrufen dieser Seite verwendet wird.
2. Eine kurze Beschreibung, welche Vorgänge Sie auf dieser Seite durchführen müssen.
3. Felder zum Eingeben Ihrer Azure-Anmeldeinformationen, sobald Ihr Azure-Konto erstellt wurde. Weitere Informationen finden Sie unter [Create an Azure Batch Account (Erstellen eines Azure Batch-Kontos)](create-azure-account.md).
4. Docker-Imagetag für das Akustiktoolset.
5. Starten Sie das Azure-Portal, um Ihre Abonnements zu verwalten, die Nutzung zu überwachen und Abrechnungsinformationen anzuzeigen usw. 
6. Der Azure Batch-Computeknotentyp, der für die Berechnung verwendet werden soll. Der Knotentyp muss vom Standort Ihres Azure-Rechenzentrums unterstützt werden. Wenn Sie sich nicht sicher sind, behalten Sie den Wert **Standard_F8s_v2** bei.
7. Die Anzahl der Knoten, die für diese Berechnung verwendet werden soll. Die Anzahl, die Sie hier eingeben, wirkt sich auf die Zeit aus, die für den Bake-Vorgang erforderlich ist, und wird von der Zuteilung Ihres Azure Batch-Kerns eingeschränkt. Die Standardzuteilung ermöglicht nur zwei Knoten mit acht Kernen oder einen Knoten mit 16 Kernen, kann jedoch erweitert werden. Weitere Informationen zu Einschränkungen bei der Kernzuteilung finden Sie unter [Create an Azure Batch Account (Erstellen eines Azure Batch-Kontos)](create-azure-account.md).
8. Aktivieren Sie dieses Kontrollkästchen, um Ihren Computepool so zu konfigurieren, dass er [Knoten mit niedriger Priorität](https://docs.microsoft.com/azure/batch/batch-low-pri-vms) verwendet. Computeknoten mit niedriger Priorität sind deutlich kostengünstiger, aber sie sind möglicherweise nicht immer verfügbar oder können jederzeit vorzeitig entfernt werden.
9. Die Anzahl der Tests für Ihre Szene, die auf der Registerkarte **Probe** (Tests) berechnet wurde. Die Anzahl der Tests bestimmt die Anzahl der Simulationen, die in der Cloud ausgeführt werden müssen. Sie können nicht mehr Knoten als Tests angeben.
10. Die Zeit, die voraussichtlich verstreicht, während Ihr Auftrag in der Cloud ausgeführt wird. Diese schließt die Startzeit der Knoten nicht ein. Sobald der Auftrag ausgeführt wird, bestimmt diese Zeit, wie lange die Ausführung dauern sollte, bis Sie die Ergebnisse erhalten. Beachten Sie, dass es sich dabei nur um eine Schätzung handelt.
11. Die gesamte Computezeit, die für die Ausführung der Simulationen erforderlich ist. Diese entspricht der Computezeit für die Knoten, die in Azure verwendet werden. Weitere Informationen zur Verwendung dieses Werts finden Sie im Verlauf dieses Artikels unter [Schätzen der Bake-Kosten](#Estimating-bake-cost).
12. Diese Meldung teilt Ihnen mit, wo die Ergebnisse des Bake-Vorgangs nach Abschluss des Auftrags gespeichert werden.
13. (Nur für fortgeschrittene Benutzer) Wenn es erforderlich ist, dass Unity einen übermittelten Bake-Vorgang „vergisst“ (z.B., wenn Sie die Ergebnisse mit einem anderen Computer heruntergeladen haben), klicken Sie auf die Schaltfläche **Clear State** (Status löschen), um den übermittelten Auftrag zu vergessen. Das bedeutet, dass die Ergebnisdatei **nicht** heruntergeladen wird, wenn diese bereit ist, und dass dies **nicht dem Abbrechen des Auftrags entspricht**. Wenn der Auftrag bereits ausgeführt wird, wird er weiterhin in der Cloud ausgeführt.
14. Klicken Sie auf die Schaltfläche „Bake“, um den Bake-Vorgang an die Cloud zu übermitteln. Während ein Auftrag ausgeführt wird, zeigt diese stattdessen **Cancel Job** (Auftrag abbrechen) an.
15. Bereitet die Verarbeitung der Akustiksimulation auf einem lokalen Computer vor. Weitere Informationen finden Sie unter [Lokaler Bake-Vorgang](#Local-bake).  
16. Dieser Bereich zeigt den Status des Bake-Vorgangs an. Wenn dieser abgeschlossen ist, sollte dieser **Downloaded** (Heruntergeladen) anzeigen.

Vollständige Informationen zu aktiven Aufträgen, Computepools und Speicherplatz können Sie jederzeit im [Azure-Portal](https://portal.azure.com) abrufen.

Während ein Auftrag ausgeführt wird, ändert sich der Text der Schaltfläche **Bake** in **Cancel Job** (Auftrag abbrechen). Verwenden Sie diese Schaltfläche, um einen ausgeführten Auftrag abzubrechen. Sie werden zur Bestätigung aufgefordert, bevor der Auftrag abgebrochen wird. Das Abbrechen eines Auftrags kann nicht rückgängig gemacht werden, es sind keine Ergebnisse verfügbar, und die verwendete Azure-Computezeit wird dennoch in Rechnung gestellt.

Sobald Sie einen Bake-Vorgang gestartet haben, können Sie Unity schließen. Je nach Projekt, Knotentyp und Anzahl der Knoten kann ein Bake-Vorgang in die Cloud mehrere Stunden dauern. Der Status des Bake-Auftrags wird aktualisiert, wenn Sie das Projekt neu laden und das Acoustics-Fenster öffnen. Wenn der Auftrag abgeschlossen ist, wird die Ausgabedatei heruntergeladen.

Die Azure-Anmeldeinformationen werden sicher auf Ihrem lokalen Computer gespeichert und Ihrem Unity-Editor zugeordnet. Sie werden nur verwendet, um eine sichere Verbindung mit Azure herzustellen.

### <a name="Estimating-bake-cost"></a> Schätzen der Bake-Kosten für Azure

Wenn Sie einschätzen möchten, wie viel ein bestimmter Bake-Vorgang kosten wird, multiplizieren Sie den angezeigten Wert für **Estimated Compute Cost** (Geschätzte Computekosten) mit den stündlichen Kosten des ausgewählten **VM-Knotentyps** (in Ihrer lokalen Währung). Das Ergebnis umfasst nicht die Zeit, die erforderlich ist, um die Knoten bereit zu machen und auszuführen. Wenn Sie z. B. **Standard_F8s_v2** als Knotentyp ausgewählt haben (Kosten: 0,40 $/Stunde) und die geschätzte Computezeit 3 Stunden und 57 Minuten beträgt, betragen die geschätzten Kosten für die Ausführung des Auftrags „0,40 $ × ~4 h = ~1,60 $“. Die tatsächlichen Kosten sind aufgrund der zusätzlichen Zeit für das Starten der Knoten wahrscheinlich etwas höher. Die stündlichen Kosten für Knoten finden Sie auf der Seite [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/linux) (klicken Sie auf die Kategorie „Computeoptimiert“ oder „High Performance Computing“).

### <a name="reviewing-the-bake-results"></a>Überprüfen der Bake-Ergebnisse

Überprüfen Sie nach dem Bake-Vorgang, ob die Voxels und Testpunkte sich an den erwarteten Positionen befinden, indem Sie das Runtime-Plug-In ausführen. Weitere Informationen finden Sie unter [Design Process Overview for Acoustics (Übersicht über den Entwurfsprozess für Akustik)](design-process.md).

## <a name="Local-bake"></a>Lokaler Bake-Vorgang
Der lokale Bake-Vorgang führt die Akustiksimulation auf Ihrem eigenen PC aus, anstatt sie in den Azure Batch-Computecluster zu verlagern. Dies kann eine geeignete Option zum Experimentieren mit der Akustik sein, ohne dass ein Azure-Abonnement erforderlich ist. Beachten Sie, dass die Akustiksimulation rechenintensiv ist und je nach Szenengröße, Simulationskonfiguration und Rohrechenleistung des verarbeitenden Computers lange dauern kann.

### <a name="minimum-hardware-requirements"></a>Minimale Hardwareanforderungen
64-Bit-Intel-Prozessor mit mindestens 8 Prozessorkernen und 32 GB RAM.

Beispiel: Ein Computer mit Intel Xeon-Prozessor E5-1660 mit 8 Prozessorkernen, 3 GHz und 32 GB RAM:
* Kleine Szene mit 100 Tests nimmt etwa 2 Stunden für einen einfachen Bake-Vorgang und etwa 32 Stunden für einen Bake-Vorgang mit hoher Auflösung in Anspruch.
* Größere Szene mit 1000 Tests kann bis zu 20 Stunden für einen einfachen Bake-Vorgang und etwa 21 Tage für einen Bake-Vorgang mit hoher Auflösung in Anspruch nehmen.

### <a name="setup-docker"></a>Einrichten von Docker
Installieren und Konfigurieren von Docker auf dem PC, auf dem die Simulation verarbeitet werden soll –
1. Installieren Sie das [Docker-Toolset](https://www.docker.com/products/docker-desktop).
2. Starten Sie die Docker-Einstellungen, navigieren Sie zu den Optionen unter „Erweitert“ und konfigurieren Sie die Ressourcen so, dass diese über mindestens 8 GB RAM verfügen. Je mehr CPUs Sie Docker zuweisen können, desto schneller wird der Bake-Vorgang abgeschlossen. ![Beispiel zu Docker-Einstellungen](media/DockerSettings.png)
3. Navigieren Sie zu „Freigegebene Laufwerke“ und aktivieren Sie die Freigabe für das Laufwerk, das für die Verarbeitung verwendet wird.![DockerDriveSharing](media/DockerSharedDrives.png)

### <a name="run-local-bake"></a>Ausführen eines lokalen Bake-Vorgangs
1. Klicken Sie auf die Registerkarte zum Vorbereiten des lokalen Bake-Vorgangs und wählen Sie einen Ordner aus, in dem die Eingabedateien und Ausführungsskripts gespeichert werden. Sie können den Bake-Vorgang dann auf jedem beliebigen Computer ausführen, solange er die minimalen Hardwareanforderungen erfüllt und Docker installiert ist, indem Sie den Ordner auf diesen Computer kopieren.
2. Starten Sie die Simulation mit dem Skript „runlocalbake.bat“. Dieses Skript ruft das Project Acoustics-Docker-Image mit dem für die Simulationsbearbeitung notwendigen Toolset ab und startet die Simulation. 
3. Nach Abschluss der Simulation kopieren Sie die resultierende ACE-Datei zurück in Ihr Unity-Projekt. Um sicherzustellen, dass Unity dies als Binärdatei erkennt, fügen Sie „.bytes“ an die Dateiendung an (z. B. „Scene1.ace.bytes“). Die ausführlichen Protokolle für die Simulation werden in der Datei „AcousticsLog.txt“ gespeichert. Wenn Probleme auftreten, teilen Sie diese Datei, um die Diagnose zu erleichtern.

## <a name="Data-Files"></a>Datendateien

Es gibt vier Datendateien, die von diesem Plug-In an verschiedenen Speicherorten erstellt wurden. Nur eine davon wird zur Laufzeit benötigt, deshalb befinden die anderen drei sich in Ordnern namens „Editor“, damit Sie nicht in das Projekt kompiliert werden.

* **Assets/Editor/[SceneName]\_AcousticsParameters.asset:** Diese Datei speichert die Daten, die Sie in die Felder auf der Acoustics-Benutzeroberfläche eingeben. Der Speicherort und der Name dieser Datei können nicht geändert werden. In dieser Datei werden weitere Werte gespeichert, die sich auf den Bake-Vorgang auswirken, diese sind jedoch für fortgeschrittene Benutzer gedacht und sollten nicht geändert werden.
* **Assets/AcousticsData/Acoustics\_[SceneName].ace.bytes:** Diese Datei wurde während der Bake-Simulation erstellt und enthält die Suchdaten, die von der Runtime verwendet wurden, um die Akustik der Szene zu rendern. Der Speicherort und der Name dieser Datei können mithilfe der Felder auf der Registerkarte **Probes** (Tests) geändert werden.
* **Assets/AcousticsData/Editor/Acoustics_[SceneName].vox:** Diese Datei speichert die in Voxels konvertierte Akustikgeometrie und die Materialeigenschaften. Diese wurden mithilfe der Schaltfläche **Calculate...** (Berechnen...) auf der Registerkarte „Probes“ (Tests) berechnet. Der Speicherort und der Name dieser Datei können mithilfe der Felder auf der Registerkarte **Probes** (Tests) geändert werden.
* **Assets/AcousticsData/Editor/Acoustics\_[SceneName]\_config.xml:** Diese Datei speichert die Parameter, die mithilfe der Schaltfläche **Calculate...** (Berechnen...) auf der Registerkarte **Probes** (Tests) berechnet wurden. Der Speicherort und der Name dieser Datei können mithilfe der Felder auf der Registerkarte **Probes** (Tests) geändert werden.

Achten Sie darauf, die Datei „*.ace.bytes“ nicht zu löschen, die vom Bake-Vorgang heruntergeladen wurde. Diese Datei kann nicht wiederhergestellt werden, sondern kann nur durch eine Wiederholung des Bake-Vorgangs erneut generiert werden.

## <a name="next-steps"></a>Nächste Schritte
* Wenden Sie die Bake-Ergebnisse auf Soundquellen an. Weitere Informationen dazu finden Sie unter [Design Process Overview for Acoustics (Übersicht über den Entwurfsprozess für Akustik)](design-process.md).

