---
title: Erstellen einer Klassifizierung mit dem Custom Vision Service – Azure Cognitive Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe des Custom Vision Service eine Klassifizierung erstellen können, die Objekte in Fotos unterscheiden kann.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: c5183078d2f9d5eb16abef4f5df240f77eea6b8b
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223368"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Erstellen einer Klassifizierung mit Custom Vision

Um den Custom Vision Service zu verwenden, müssen Sie zuerst eine Klassifizierung erstellen. In diesem Dokument erfahren Sie, wie Sie mithilfe Ihres Webbrowsers eine Klassifizierung erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Um eine Klassifizierung zu erstellen, müssen Sie über Folgendes verfügen:

- Ein gültiges [Microsoft-Konto](https://account.microsoft.com/account) oder eine Azure Active Directory-OrgID („Geschäfts-, Schul- oder Unikonto“) für die Anmeldung bei „customvision.ai“ und den Einstieg.

    > [!IMPORTANT] 
    > Die OrgID-Anmeldung für Azure AD-Benutzer (Azure Active Directory) von [nationalen Clouds](https://www.microsoft.com/en-us/trustcenter/cloudservices/nationalcloud) wird derzeit nicht unterstützt.

- Eine Reihe von Bildern zum Trainieren Ihrer Klassifizierung (mit mindestens 30 Bildern pro Kategorie).

- Einige Bilder zum Testen Ihrer Klassifizierung nach dem Trainieren.

- Optional: Ein Azure-Abonnement, das Ihrem Microsoft-Konto oder Ihrer OrgID zugeordnet ist. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

    > [!IMPORTANT]
    > Ohne Azure-Abonnement können Sie lediglich __eingeschränkte Testprojekte__ erstellen. Wenn Sie über ein Azure-Abonnement verfügen, werden Sie während der Projekterstellung aufgefordert, Trainings- und Vorhersageressourcen für den Custom Vision Service im [Azure-Portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) zu erstellen.   

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts

Führen Sie die folgenden Schritte aus, um ein neues Projekt zu erstellen:

1. Navigieren Sie im Webbrowser zur [Custom Vision-Webseite](https://customvision.ai). Wählen Sie __Anmelden__ aus, um mit der Verwendung des Diensts zu beginnen.

    ![Abbildung der Anmeldeseite](./media/getting-started-build-a-classifier/custom-vision-web-ui.png)

    > [!NOTE]
    > Nach der Anmeldung beim Custom Vision Service wird Ihnen eine Liste mit Projekten angezeigt. Abgesehen von zwei „eingeschränkten Testprojekten“ für Testzwecke sind Projekte mit einer Azure-Ressource verknüpft. Wenn Sie Azure-Benutzer sind, können Sie alle Projekte sehen, die [Azure-Ressourcen](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#grant-access-to-resources) zugeordnet sind, auf die Sie Zugriff haben. 

2. Um Ihr erstes Projekt zu erstellen, wählen Sie **New Project** (Neues Projekt) aus. Bei Ihrem ersten Projekt werden Sie aufgefordert, den Nutzungsbedingungen zuzustimmen. Aktivieren Sie das Kontrollkästchen, und wählen Sie dann die Schaltfläche **I agree** (Ich stimme zu) aus. Das Dialogfeld **New Project**  (Neues Projekt) wird angezeigt.

    ![Das Dialogfeld „New Project“ enthält Felder für den Namen, eine Beschreibung und Domänen.](./media/getting-started-build-a-classifier/new-project.png)

3. Geben Sie einen Namen und eine Beschreibung für das Projekt ein. Wählen Sie dann eine der verfügbaren Domänen aus. Jede Domäne optimiert die Klassifizierung für bestimmte Bildtypen, wie in der folgenden Tabelle beschrieben:

    |Domäne|Zweck|
    |---|---|
    |__Allgemein__| Für eine breite Palette von Aufgaben in der Bildklassifizierung optimiert. Wenn keine der anderen Domänen geeignet erscheint oder Sie unsicher sind, welche Domäne Sie wählen sollen, verwenden Sie die Domäne „Generic“. |
    |__Food (Lebensmittel)__|Für Fotos von Gerichten optimiert, wie sie beispielsweise auf der Speisekarte von Restaurants abgebildet werden. Wenn Sie Fotos von einzelnen Früchten oder Gemüsen klassifizieren möchten, verwenden Sie die Domäne „Food“.|
    |__Landmarks (Wahrzeichen)__|Für erkennbare Wahrzeichen (Naturdenkmäler oder künstliche Wahrzeichen) optimiert. Diese Domäne funktioniert am besten, wenn das Wahrzeichen im Foto deutlich zu sehen ist. Die Domäne funktioniert auch, wenn das Wahrzeichen etwas von Personen im Vordergrund verdeckt wird.|
    |__Retail (Einzelhandel)__|Für Bilder optimiert, wie man sie in einem Einkaufskatalog oder auf einer Einkaufswebsite findet. Wenn Sie eine präzise Klassifizierung zwischen Kleidern, Hosen und Hemden wünschen, verwenden Sie diese Domäne.|
    |__Adult (Erwachsen)__|Optimiert für die bessere Unterscheidung zwischen nicht jugendfreien Inhalten und jugendfreien Inhalten. Wenn Sie also beispielsweise Bilder von Personen in Badebekleidung blockieren möchten, können Sie mithilfe dieser Domäne eine entsprechende benutzerdefinierte Klassifizierung erstellen.|
    |__Kompaktdomänen__| Für die Bedingungen der Echtzeitklassifizierung auf Mobilgeräten optimiert. Die von Kompaktdomänen generierten Modelle können für die lokale Ausführung exportiert werden.|

    Sie können die Domäne bei Bedarf später ändern.

4. Wählen Sie eine Ressourcengruppe aus. Im Ressourcengruppen-Dropdownfeld werden alle Ihre Azure-Ressourcengruppen angezeigt, die eine Custom Vision Service-Ressource enthalten. Sie können sich darüber hinaus entscheiden, eine __eingeschränkte Testversion__ zu erstellen. Der Eintrag für die eingeschränkte Testversion ist die einzige Ressourcengruppe, die für einen Benutzer ohne Azure-Abonnement zur Wahl steht.

    Um das Projekt zu erstellen, wählen Sie __Projekt erstellen__ aus.

## <a name="upload-and-tag-images"></a>Hochladen und Kennzeichnen von Bildern

1. Verwenden Sie zum Hinzufügen von Bildern zur Klassifizierung die Schaltfläche __Bilder hinzufügen__, und wählen Sie dann __Lokale Dateien durchsuchen__ aus. Wählen Sie __Öffnen__ aus, um zur Kategorisierung zu gelangen.

    > [!TIP]
    > Nach dem Auswählen von Bildern müssen Sie diese kategorisieren. Die Kategorie wird auf die Gruppe von Bildern angewendet, die Sie zum Hochladen ausgewählt haben, daher kann es einfacher sein, Bilder anhand der Kategorien auszuwählen, die Sie verwenden möchten. Sie können die Kategorie für ausgewählte Bilder auch nach dem Kategorisieren und Hochladen ändern.

    > [!TIP]
    > Laden Sie Bilder hoch, die sich in Kamerablickwinkeln, Beleuchtung, Hintergrund, Typen, Stilen, Gruppen, der Größe usw. unterscheiden. Verwenden Sie eine Vielzahl unterschiedlicher Typen von Fotos, um sicherzustellen, dass Ihre Klassifizierung nicht einseitig beeinflusst wird und gut generalisieren kann.

    Der Custom Vision Service akzeptiert Trainingsbilder in den Formaten JPG, PNG und BMP mit bis zu 6 MB pro Bild. (Vorhersagebilder dürfen bis zu 4 MB pro Bild groß sein.) Wir empfehlen, dass Bilder an der kürzesten Seite mindestens 256 Pixel messen. Bilder, die an der kürzesten Seite kleiner als 256 Pixel sind, werden vom Custom Vision Service hochskaliert.

    ![Das Steuerelement zum Hinzufügen von Bildern wird oben links und als Schaltfläche unten in der Mitte angezeigt.](./media/getting-started-build-a-classifier/add-images01.png)

    >[!NOTE] 
    > Die REST-API kann zum Laden von Trainingsbildern über URLs verwendet werden.

2. Zum Festlegen der Kategorie geben Sie Text im Feld __My Tags__ (Meine Kategorien) ein und verwenden dann die __+__-Schaltfläche. Um die Bilder hochzuladen und sie zu kategorisieren, verwenden Sie die Schaltfläche __Upload [number] files__ ([Anzahl] Dateien hochladen). Sie können den Bildern mehrere Kategorien hinzufügen. 

    > [!NOTE]
    > Die für das Hochladen erforderliche Zeit unterscheidet sich je nach Anzahl und Größe der ausgewählten Bilder.

    ![Bild der Seite zum Hochladen und Kategorisieren](./media/getting-started-build-a-classifier/add-images03.png)

3. Wählen Sie __Fertig__ aus, nachdem die Bilder hochgeladen wurden.

    ![Die Statusleiste zeigt alle Aufgaben als abgeschlossen an.](./media/getting-started-build-a-classifier/add-images04.png)

4. Um einen weiteren Satz Bilder hochzuladen, kehren Sie zu Schritt 1 zurück. Wenn Sie beispielsweise zwischen Hunden und Ponys unterscheiden möchten, laden Sie Bilder von Ponys hoch, und kategorisieren Sie sie.

## <a name="train-and-evaluate-the-classifier"></a>Trainieren und Bewerten der Klassifizierung

Um die Klassifizierung zu trainieren, wählen Sie die Schaltfläche **Trainieren** aus.

![Die Trainieren-Schaltfläche wird im rechten oberen Bereich des Browserfensters angezeigt.](./media/getting-started-build-a-classifier/train01.png)

Das Trainieren der Klassifizierung nimmt nur einige Minuten in Anspruch. Während dieser Zeit werden Informationen über den Trainingsvorgang angezeigt.

![Die Trainieren-Schaltfläche wird im rechten oberen Bereich des Browserfensters angezeigt.](./media/getting-started-build-a-classifier/train02.png)

Nach dem Training wird die __Leistung__ angezeigt. Die Präzisions- und Erinnerungsanzeigen informieren Sie auf der Grundlage automatischer Tests darüber, wie gut Ihre Klassifizierung ist. Custom Vision Service verwendet die Bilder, die Sie zum Training gesendet haben, um diese Zahlen mithilfe eines Verfahrens namens [k-fache Kreuzvalidierung](https://en.wikipedia.org/wiki/Cross-validation_(statistics)) zu berechnen.

![Die Trainingsergebnisse zeigen die Gesamtgenauigkeit und die Erinnerung für jede Kategorie der Klassifizierung an.](./media/getting-started-build-a-classifier/train03.png)

> [!NOTE] 
> Jedes Mal, wenn Sie die Schaltfläche **Train** (Trainieren) auswählen, erstellen Sie eine neue Iteration Ihrer Klassifizierung. Auf der Registerkarte **Performance** (Leistung) können Sie alle Ihre alten Iterationen anzeigen und eventuell veraltete Iterationen löschen. Beim Löschen einer Iteration werden auch alle Bilder gelöscht, die dieser eindeutig zugeordnet sind.

Die Klassifizierung verwendet alle Bilder, um ein Modell zu erstellen, das jede Kategorie identifiziert. Um die Qualität des Modells zu testen, probiert die Klassifizierung jedes Bild mit dem Modell aus, um zu ermitteln, was das Modell findet.

Die Qualität der Klassifizierungsergebnisse wird angezeigt.

|Begriff|Definition|
|---|---|
|__Genauigkeit__|Wenn Sie ein Bild klassifizieren, wie hoch ist die Wahrscheinlichkeit, dass Ihre Klassifizierung das Bild richtig klassifiziert? Von allen Bildern, die zum Trainieren der Klassifizierung verwendet wurden (Hunde und Ponys), welchen Prozentsatz hat das Modell richtig erkannt? 99 richtige Kategorien aus 100 Bildern ergeben eine Genauigkeit von 99 Prozent.|
|__Recall (Trefferquote)__|Wie viele der Bilder, die korrekt klassifiziert werden sollten, wurden von Ihrer Klassifizierung korrekt identifiziert? Eine Trefferquote von 100 % bedeutet, dass in 38 Hundebildern, die zum Trainieren der Klassifizierung verwendet wurden, 38 Hunde gefunden wurden.|

## <a name="next-steps"></a>Nächste Schritte

[Testen und erneutes Trainieren des Modells](test-your-model.md)

