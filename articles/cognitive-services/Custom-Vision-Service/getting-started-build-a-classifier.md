---
title: Erstellen einer Klassifizierung – Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mithilfe der Custom Vision-Website ein Bildklassifizierungsmodell erstellen.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: anroth
ms.openlocfilehash: f2cd8f5074f815e84caaedb01335406657f29088
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58088008"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Erstellen einer Klassifizierung mit Custom Vision

Zur Verwendung des Custom Vision Service müssen Sie zuerst ein Klassifizierungsmodell erstellen. In diesem Leitfaden erfahren Sie, wie eine Klassifizierung über die Custom Vision-Website erstellt wird.

## <a name="prerequisites"></a>Voraussetzungen

- Ein gültiges Azure-Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/).
- Eine Reihe von Bildern zum Trainieren Ihrer Klassifizierung. Tipps zum Auswählen von Bildern finden Sie unten.


## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Erstellen von Custom Vision-Ressourcen im Azure-Portal
Für die Verwendung des Custom Vision Service müssen Sie im [Azure-Portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) Custom Vision-Trainings- und -Vorhersageressourcen erstellen. Bei diesem Vorgang wird sowohl eine Trainings- als auch eine Vorhersageressource erstellt. 

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts

Navigieren Sie im Webbrowser zur [Custom Vision-Webseite](https://customvision.ai), und wählen Sie __Sign in__ (Anmelden). Melden Sie sich mit demselben Konto an, mit dem Sie sich auch beim Azure-Portal angemeldet haben.

![Abbildung der Anmeldeseite](./media/browser-home.png)


1. Um Ihr erstes Projekt zu erstellen, wählen Sie **New Project** (Neues Projekt) aus. Das Dialogfeld **Create new project** (Neues Projekt erstellen) wird angezeigt.

    ![Das Dialogfeld „New Project“ enthält Felder für den Namen, eine Beschreibung und Domänen.](./media/getting-started-build-a-classifier/new-project.png)

1. Geben Sie einen Namen und eine Beschreibung für das Projekt ein. Wählen Sie dann eine Ressourcengruppe aus. Wenn Ihr angemeldetes Konto einem Azure-Konto zugeordnet ist, zeigt die Dropdownliste „Ressourcengruppe“ alle Ihre Azure-Ressourcengruppen an, die eine Custom Vision Service-Ressource enthalten. 

> [!NOTE]
> Wenn keine Ressourcengruppe verfügbar ist, sollten Sie sich vergewissern, dass Sie sich an [customvision.ai](https://customvision.ai) mit demselben Konto angemeldet haben, das Sie auch für die Anmeldung am [Azure-Portal](https://portal.azure.com/) verwendet haben. Stellen Sie darüber hinaus sicher, dass Sie im Custom Vision-Portal dasselbe Verzeichnis wie im Azure-Portal gewählt haben, in dem sich Ihre Custom Vision-Ressourcen befinden. An beiden Orten können Sie Ihr Verzeichnis jeweils oben rechts im Dropdownmenü mit den Konten auswählen. 

1. Wählen Sie unter __Projekttypen__ die Option __Klassifizierung__ aus. Wählen Sie unter __Klassifizierungstypen__ je nach Ihrem Anwendungsfall entweder **Multilabel** (Mehrere Bezeichnungen) oder **Multiclass** (Mehrere Klassen) aus. Die Klassifizierung mit mehreren Bezeichnungen wendet eine beliebige Anzahl Ihrer Tags (null oder mehr) auf ein Bild an, während die Klassifizierung mit mehreren Klassen Bilder in einzelne Kategorien sortiert (jedes übermittelte Bild wird dem wahrscheinlichsten Tag zugeteilt). Sie können den Klassifizierungstyp später bei Bedarf ändern.

1. Wählen Sie dann eine der verfügbaren Domänen aus. Jede Domäne optimiert die Klassifizierung für bestimmte Bildtypen, wie in der folgenden Tabelle beschrieben. Sie können die Domäne später bei Bedarf ändern.

    |Domäne|Zweck|
    |---|---|
    |__Allgemein__| Für eine breite Palette von Aufgaben in der Bildklassifizierung optimiert. Wenn keine der anderen Domänen geeignet erscheint oder Sie unsicher sind, welche Domäne Sie wählen sollen, verwenden Sie die Domäne „Generic“. |
    |__Food (Lebensmittel)__|Für Fotos von Gerichten optimiert, wie sie beispielsweise auf der Speisekarte von Restaurants abgebildet werden. Wenn Sie Fotos von einzelnen Früchten oder Gemüsen klassifizieren möchten, verwenden Sie die Domäne „Food“.|
    |__Landmarks (Wahrzeichen)__|Für erkennbare Wahrzeichen (Naturdenkmäler oder künstliche Wahrzeichen) optimiert. Diese Domäne funktioniert am besten, wenn das Wahrzeichen im Foto deutlich zu sehen ist. Die Domäne funktioniert auch, wenn das Wahrzeichen etwas von Personen im Vordergrund verdeckt wird.|
    |__Retail (Einzelhandel)__|Für Bilder optimiert, wie man sie in einem Einkaufskatalog oder auf einer Einkaufswebsite findet. Wenn Sie eine präzise Klassifizierung zwischen Kleidern, Hosen und Hemden wünschen, verwenden Sie diese Domäne.|
    |__Adult (Erwachsen)__|Optimiert für die bessere Unterscheidung zwischen nicht jugendfreien Inhalten und jugendfreien Inhalten. Wenn Sie also beispielsweise Bilder von Personen in Badebekleidung blockieren möchten, können Sie mithilfe dieser Domäne eine entsprechende benutzerdefinierte Klassifizierung erstellen.|
    |__Kompaktdomänen__| Für die Bedingungen der Echtzeitklassifizierung auf Mobilgeräten optimiert. Die von Kompaktdomänen generierten Modelle können für die lokale Ausführung exportiert werden.|
    
1. Wählen Sie schließlich __Create Project__ (Projekt erstellen) aus.

## <a name="choose-training-images"></a>Auswählen von Trainingsbildern

Es wird empfohlen, im ersten Trainingssatz mindestens 30 Bilder pro Tag zu verwenden. Sie sollten auch einige zusätzliche Bilder sammeln, um Ihr Modell zu testen, nachdem es trainiert wurde.

Verwenden Sie zum effektiven Trainieren Ihres Modells Bilder mit optischer Vielfalt. Wählen Sie Bilder aus, die sich nach folgenden Aspekten unterscheiden:
* Kamerawinkel
* Belichtung
* background
* Visueller Stil
* Einzelne/gruppierte Motive
* size
* type

Stellen Sie außerdem sicher, dass alle Ihre Trainingsbilder die folgenden Kriterien erfüllen:
* JPG-, PNG- oder BMP-Format
* Höchstens 6 MB groß (4 MB für Vorhersagebilder)
* Mindestens 256 Pixel an der kürzesten Seite; kürzere Bilder werden von Custom Vision Service automatisch hochskaliert.

## <a name="upload-and-tag-images"></a>Hochladen und Kennzeichnen von Bildern

In diesem Abschnitt laden Sie Bilder hoch und kennzeichnen diese manuell, um die Klassifizierung zu trainieren. 

1. Klicken Sie zum Hinzufügen von Bildern auf die Schaltfläche __Bilder hinzufügen__, und wählen Sie dann __Lokale Dateien durchsuchen__ aus. Wählen Sie __Öffnen__ aus, um zur Kategorisierung zu gelangen. Ihre Tag-Auswahl wird auf die gesamte Gruppe von Bildern angewendet, die Sie zum Hochladen ausgewählt. Daher ist es einfacher, Bildern entsprechend den gewünschten Tags in separaten Gruppen hochzuladen. Sie können die Kategorie für einzelne Bilder auch nach dem Hochladen ändern.

    ![Das Steuerelement zum Hinzufügen von Bildern wird oben links und als Schaltfläche unten in der Mitte angezeigt.](./media/getting-started-build-a-classifier/add-images01.png)


1. Zum Erstellen eines Tags geben Sie Text in das Feld __My Tags__ (Meine Tags) ein, und drücken Sie die EINGABETASTE. Wenn das Tag bereits vorhanden ist, wird es in einem Dropdownmenü angezeigt. In einem Projekt mit mehreren Bezeichnungen können Sie Ihren Bildern mehrere Tags hinzufügen. In einem Projekt mit mehreren Klassen können Sie jedoch nur eins hinzufügen. Verwenden Sie die Schaltfläche __Upload [number] files ([Anzahl] Dateien hochladen)__, um den Upload der Bilder abzuschließen. 

    ![Bild der Seite zum Hochladen und Kategorisieren](./media/getting-started-build-a-classifier/add-images03.png)

1. Wählen Sie __Fertig__ aus, nachdem die Bilder hochgeladen wurden.

    ![Die Statusleiste zeigt alle Aufgaben als abgeschlossen an.](./media/getting-started-build-a-classifier/add-images04.png)

Zum Hochladen eines weiteren Bildersatzes kehren Sie zum Anfang dieses Abschnitts zurück und wiederholen die Schritte. Möglicherweise müssen Sie in Ihrem Projekt irgendwann _Negativbeispiele_ hinzufügen, damit Ihre Klassifizierung genauer wird. Negativbeispiele sind jene, die mit keinen der anderen Tags übereinstimmen. Wenn Sie diese Bilder hochladen, wenden Sie die spezielle **Negativ**-Bezeichnung auf sie an.

> [!NOTE]
> Der Custom Vision Service unterstützt teilweise die automatische Verarbeitung von negativen Bildern. Wenn Sie beispielsweise eine Klassifizierung zur Unterscheidung von Trauben und Bananen erstellen und ein Bild eines Schuhs für die Vorhersage übermitteln, sollte die Klassifizierung dieses Bild in Bezug auf Trauben und Bananen mit nahezu 0 % bewerten.
> 
> Andererseits ist es in Fällen, in denen die negativen Bilder nur eine Variation der im Training verwendeten Bilder sind, wahrscheinlich, dass das Modell die negativen Bilder aufgrund der großen Ähnlichkeiten als eine bezeichnete Klasse klassifiziert. Falls Sie eine Klassifizierung zur Unterscheidung von Orangen und Grapefruits verwenden und ein Bild einer Clementine übermitteln, wird die Clementine ggf. als Orange eingestuft, da sich die Merkmale von Clementinen und Orangen stark ähneln. Wenn Ihre negativen Bilder dieser Art sind, raten wir Ihnen, mindestens ein zusätzliches Tag (z. B. **Sonstiges**) zu erstellen und die negativen Bilder während des Trainings mit diesem Tag zu bezeichnen, damit das Modell besser zwischen diesen Klassen unterscheiden kann.

## <a name="train-the-classifier"></a>Trainieren des Klassifizierers

Um die Klassifizierung zu trainieren, wählen Sie die Schaltfläche **Trainieren** aus. Die Klassifizierung verwendet alle aktuellen Bilder zum Erstellen eines Modells, das die visuellen Merkmale der einzelnen Tags identifiziert.

![Die Schaltfläche „Trainieren“ oben rechts auf der Header-Symbolleiste der Webseite](./media/getting-started-build-a-classifier/train01.png)

Der Trainingsprozess sollte nur wenige Minuten dauern. Während dieser Zeit werden auf der Registerkarte **Leistung** Informationen über den Trainingsprozess angezeigt.

![Das Browserfenster mit einem Trainingsdialogfeld im Hauptteil](./media/getting-started-build-a-classifier/train02.png)

## <a name="evaluate-the-classifier"></a>Bewerten der Klassifizierung

Nach Abschluss des Trainings wird die Leistung des Modells geschätzt und angezeigt. Custom Vision Service verwendet die Bilder, die Sie zum Training gesendet haben, um Genauigkeit und Trefferquote mithilfe eines Verfahrens namens [k-fache Kreuzvalidierung](https://en.wikipedia.org/wiki/Cross-validation_(statistics)) zu berechnen. Genauigkeit und Trefferquote sind zwei unterschiedliche Messungen der Wirksamkeit einer Klassifizierung:

- Die **Genauigkeit** gibt den Anteil der richtig identifizierten Klassifizierungen an. Beispiel: Wenn das Modell 100 Bilder als Hunde identifiziert hat und 99 davon tatsächlich Hunde zeigten, beträgt die Genauigkeit 99 %.
- Die **Trefferquote** gibt den Anteil der tatsächlichen Klassifizierungen an, die richtig identifiziert wurden. Beispiel: Wenn tatsächlich 100 Bilder von Äpfeln vorhanden sind und das Modell 80 davon als Äpfel identifiziert, beträgt die Trefferquote 80 %.

![Die Trainingsergebnisse zeigen die Gesamtgenauigkeit und die Erinnerung für jede Kategorie der Klassifizierung an.](./media/getting-started-build-a-classifier/train03.png)

### <a name="probability-threshold"></a>Wahrscheinlichkeitsschwellenwert

Beachten Sie den Schieberegler für den **Wahrscheinlichkeitsschwellenwert** im linken Bereich der Registerkarte **Leistung**. Dies ist der Schwellenwert für eine vorhergesagte Wahrscheinlichkeit, die bei der Berechnung von Genauigkeit und Trefferquote als richtig gelten soll.

Das Interpretieren von Vorhersageaufrufen mit einem hohen Wahrscheinlichkeitsschwellenwert gibt tendenziell Ergebnisse mit hoher Genauigkeit zurück. Dies geht allerdings zu Lasten der Trefferquote (die gefunden Klassifizierungen sind richtig, viele wurden jedoch nicht gefunden). Ein niedriger Wahrscheinlichkeitsschwellenwert bewirkt das Gegenteil (die meisten der tatsächlichen Klassifizierungen wurden gefunden, aber der Satz enthält falsch positive Ergebnisse). In diesem Sinne empfiehlt es sich, den Wahrscheinlichkeitsschwellenwert gemäß den spezifischen Anforderungen Ihres Projekts festzulegen. Später sollten Sie auf Clients den gleichen Wahrscheinlichkeitsschwellenwert als Filter festlegen, wenn Vorhersageergebnisse vom Modell empfangen werden.

## <a name="manage-training-iterations"></a>Verwalten von Trainingsiterationen

Bei jedem Trainingsvorgang für Ihre Klassifizierung erstellen Sie eine neue _Iteration_ mit eigenen aktualisierten Leistungsmetriken. Sie können alle Ihre Iterationen im linken Bereich der Registerkarte **Leistung** anzeigen. Wenn Sie eine auswählen, haben Sie die Möglichkeit, sie durch Klicken auf die Schaltfläche **Standard** oben als _Standarditeration_ festzulegen. Die _Standarditeration_ ist das Modell, das standardmäßig verwendet wird, wenn Sie es über die Vorhersage-API abfragen (z.B. in einer App). Wenn Sie die _Standarditeration_ nicht aktualisieren, können Sie Ihr Modell ohne Auswirkungen auf das aktuelle Verhalten Ihrer App weiter trainieren. Sobald Sie mit dem verbesserten Modell zufrieden sind, können Sie die Standarditeration aktualisieren.

Im linken Bereich befindet sich auch die Schaltfläche **Löschen**, mit der Sie veraltete Iterationen löschen können. Beim Löschen einer Iteration werden auch alle Bilder gelöscht, die dieser eindeutig zugeordnet sind.

## <a name="next-steps"></a>Nächste Schritte

In diesem Handbuch haben Sie erfahren, wie ein Bildklassifizierungsmodell mithilfe der Custom Vision-Website erstellt und trainiert wird. Informieren Sie sich als Nächstes über den iterativen Prozess zur Verbesserung Ihres Modells.

[Testen und erneutes Trainieren eines Modells mit Custom Vision Service](test-your-model.md)

