---
title: 'Tutorial: Moderieren von E-Commerce-Produktbildern – Content Moderator'
titlesuffix: Azure Cognitive Services
description: Richten Sie eine Anwendung ein, die Produktbilder analysiert und mit bestimmten Bezeichnungen klassifiziert (unter Verwendung von maschinellem Sehen und Custom Vision von Azure) sowie anstößige Bilder zur weiteren Prüfung markiert (mithilfe von Azure Content Moderator).
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 41437933f715af092dea563b0b1be4794c71925c
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863158"
---
# <a name="tutorial-moderate-e-commerce-product-images-with-azure-content-moderator"></a>Tutorial: Moderieren von E-Commerce-Produktbildern mit Azure Content Moderator

In diesem Tutorial erfahren Sie, wie Sie Azure Cognitive Services einschließlich Content Moderator verwenden, um Produktbilder für ein E-Commerce-Szenario effektiv zu klassifizieren und zu moderieren. Sie verwenden maschinelles Sehen und Custom Vision, um Bilder mit verschiedenen Tags (Bezeichnungen) zu versehen. Anschließend erstellen Sie eine Teamüberprüfung, um die Machine-Learning-basierten Technologien von Content Moderator mit menschlichen Prüfungsteams zu kombinieren und so ein intelligentes Moderationssystem zu erhalten.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Registrieren für Content Moderator und Erstellen eines Prüfungsteams
> * Verwenden Sie die Bildersuche-API von Content Moderator, um Bilder auf potenziell nicht jugendfreie Inhalte zu prüfen.
> * Verwenden des Diensts für maschinelles Sehen, um nach Prominenten zu suchen (oder nach anderen Tags, die durch maschinelles Sehen erkannt werden können)
> * Verwenden des Custom Vision-Diensts, um Bilder auf Flaggen, Spielsachen und Stifte (oder andere benutzerdefinierte Tags) zu überprüfen
> * Weiterleiten der kombinierten Überprüfungsergebnisse an menschliche Prüfer zur abschließenden Entscheidungsfindung

Der vollständige Beispielcode steht im GitHub-Repository [Samples eCommerce Catalog Moderation](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) zur Verfügung.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Content Moderator-Abonnementschlüssel. Gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um den Content Moderator-Dienst zu abonnieren und Ihren Schlüssel zu erhalten.
- Ein Abonnementschlüssel für maschinelles Sehen (siehe obige Anweisungen).
- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/).
- Bilder für jede Bezeichnung zur Verwendung durch die Custom Vision-Klassifizierung (in diesem Fall Spielsachen, Stifte und US-Flaggen).

## <a name="create-a-review-team"></a>Erstellen eines Prüfungsteams

Informationen zum Registrieren für das [Content Moderator-Prüfungstool](https://contentmoderator.cognitive.microsoft.com/) sowie zum Erstellen eines Prüfungsteams finden Sie unter [Schnellstart: Erste Schritte mit Content Moderator](quick-start.md). Notieren Sie sich auf der Seite **Anmeldeinformationen** die **Team-ID**.

## <a name="create-custom-moderation-tags"></a>Erstellen benutzerdefinierter Moderationstags

Erstellen Sie als Nächstes im Prüfungstool benutzerdefinierte Tags. (Informationen hierzu finden Sie bei Bedarf im Artikel [Infos zu Tags](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags).) Wir fügen hier folgende Tags hinzu: **celebrity** (prominente Person), **USA**, **flag** (Flagge), **toy** (Spielzeug) und **pen** (Stift). Beachten Sie, dass es sich nicht bei allen der Tags um Kategorien handeln muss, die durch maschinelles Sehen erkannt werden können (beispielsweise **celebrity**). Sie können gerne eigene benutzerdefinierte Tags hinzufügen, solange Sie die Custom Vision-Klassifizierung später für deren Erkennung trainieren.

![Konfigurieren von benutzerdefinierten Tags](images/tutorial-ecommerce-tags2.PNG)

## <a name="create-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

1. Öffnen Sie in Visual Studio das Dialogfeld „Neues Projekt“. Erweitern Sie **Installiert** und dann **Visual C#**, und wählen Sie **Konsolen-App (.NET Framework)** aus.
1. Nennen Sie die Anwendung **EcommerceModeration**, und klicken Sie anschließend auf **OK**.
1. Falls Sie dieses Projekt einer vorhandenen Projektmappe hinzufügen, wählen Sie es als einzelnes Startprojekt aus.

In diesem Tutorial wird der zentrale Code für das Projekt erläutert. Dabei wird jedoch nicht auf jede einzelne erforderliche Codezeile eingegangen. Kopieren Sie den gesamten Inhalt der Datei _Program.cs_ aus dem Beispielprojekt ([Samples eCommerce Catalog Moderation](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration)) in die Datei _Program.cs_ Ihres neuen Projekts. Gehen Sie dann Schritt für Schritt die folgenden Abschnitte durch, um sich mit der Funktionsweise und der praktischen Verwendung des Projekts vertraut zu machen.

## <a name="define-api-keys-and-endpoints"></a>Definieren von API-Schlüsseln und -Endpunkten

Da in diesem Tutorial wie bereits erwähnt drei Cognitive Services verwendet werden, sind auch drei entsprechende Schlüssel und API-Endpunkte erforderlich. Sehen Sie sich die folgenden Felder in der Klasse **Program** an: 

[!code-csharp[define API keys and endpoint URIs](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=21-29)]

Sie müssen die Felder vom Typ `___Key` mit den Werten Ihrer Abonnementschlüssel aktualisieren. (`CustomVisionKey` erhalten Sie später.) Unter Umständen müssen Sie auch die Felder vom Typ `___Uri` ändern, sodass sie jeweils die korrekte Regions-ID enthalten. Geben Sie für den Teil `YOURTEAMID` des Felds `ReviewUri` die ID des weiter oben erstellten Prüfungsteams an. Der letzte Teil des Felds `CustomVisionUri` wird später ausgefüllt.

## <a name="primary-method-calls"></a>Primäre Methodenaufrufe

Sehen Sie sich den folgenden Code in der Methode **Main** an. Er durchläuft eine Liste von Bild-URLs. Er analysiert jedes Bild mit den drei verschiedenen Diensten, erfasst die angewendeten Tags im Array **ReviewTags** und erstellt eine Prüfung für menschliche Moderatoren. Dazu werden die Bilder an das Content Moderator-Prüfungstool gesendet. Diese Methoden werden in den folgenden Abschnitten behandelt. Hier können Sie auf Wunsch steuern, welche Bilder zur Überprüfung gesendet werden. Verwenden Sie dazu das Array **ReviewTags** in einer Bedingungsanweisung, um zu überprüfen, welche Tags angewendet wurden.

[!code-csharp[Main: evaluate each image and create review](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=53-70)]

## <a name="evaluateadultracy-method"></a>Methode „EvaluateAdultRacy“

Sehen Sie sich die Methode **EvaluateAdultRacy** in der Klasse **Program** an. Diese Methode akzeptiert eine Bild-URL und ein Array von Schlüssel-Wert-Paaren als Parameter. Sie ruft (unter Verwendung von REST) die Bild-API von Content Moderator auf, um die Ergebnisse für nicht jugendfreie und freizügige Inhalte des Bilds zu erhalten. Ist eines der Ergebnisse größer als 0,4 (der mögliche Bereich liegt zwischen 0 und 1), wird der entsprechende Wert im Array **ReviewTags** auf **true** festgelegt.

[!code-csharp[define EvaluateAdultRacy method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=73-113)]

## <a name="evaluatecustomvisiontags-method"></a>Methode „EvaluateCustomVisionTags“

Die nächste Methode akzeptiert eine Bild-URL und die Informationen Ihres Abonnements für maschinelles Sehen und analysiert, ob das Bild Prominente enthält. Wurde mindestens eine prominente Person gefunden, wird der entsprechende Wert im Array **ReviewTags** auf **true** festgelegt. 

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=115-146)]

## <a name="evaluatecustomvisiontags-method"></a>Methode „EvaluateCustomVisionTags“

Sehen Sie sich als Nächstes die Methode **EvaluateCustomVisionTags** an. Diese klassifiziert die eigentlichen Produkte (in diesem Fall Flaggen, Spielsachen und Stifte). Gehen Sie gemäß der Anleitung unter [Erstellen einer Klassifizierung mit Custom Vision](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) vor, um Ihre eigene benutzerdefinierte Bildklassifizierung für die Erkennung von Flaggen, Spielsachen und Stiften (oder anderen benutzerdefinierten Tags) auf Bildern zu erstellen.

![Custom Vision-Webseite mit Trainingsbildern für Stifte, Spielsachen und Flaggen](images/tutorial-ecommerce-custom-vision.PNG)

Rufen Sie nach dem Trainieren Ihrer Klassifizierung den Vorhersageschlüssel und die Vorhersageendpunkt-URL ab (siehe [Abrufen der Vorhersage-URL und des Vorhersage-Schlüssels](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api#get-the-url-and-prediction-key)), und weisen Sie diese Werte dem Feld `CustomVisionKey` bzw. `CustomVisionUri` zu. Die Methode verwendet diese Werte, um die Klassifizierung abzufragen. Findet die Klassifizierung mindestens eines der benutzerdefinierten Tags auf dem Bild, legt diese Methode die entsprechenden Werte im Array **ReviewTags** auf **true** fest. 

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=148-171)]

## <a name="create-reviews-for-review-tool"></a>Erstellen von Prüfungen für das Prüfungstool

In den vorherigen Abschnitten haben Sie sich mit den Methoden beschäftigt, die eingehende Bilder auf nicht jugendfreie und freizügige Inhalte (Content Moderator), Prominente (maschinelles Sehen) und verschiedene Objekte (Custom Vision) überprüfen. Sehen Sie sich als Nächstes die Methode **CreateReview** an. Diese Methode lädt die Bilder mit allen angewendeten (als _Metadata_ übergebenen) Tags in das Content Moderator-Prüfungstool hoch, damit sie von menschlichen Prüfern geprüft werden können. 

[!code-csharp[define CreateReview method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=173-196)]

Die Bilder werden auf der Prüfungsregisterkarte des [Content Moderator-Prüfungstools](https://contentmoderator.cognitive.microsoft.com/) angezeigt.

![Screenshot des Content Moderator-Prüfungstools mit mehreren Bildern und hervorgehobenen Tags](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="submit-a-list-of-test-images"></a>Übermitteln einer Liste mit Testbildern

In der Methode **Main** sehen Sie, dass dieses Programm im Verzeichnis „C:\test\“ nach einer Datei namens _Urls.txt_ sucht, die eine Liste mit Bild-URLs enthält. Erstellen Sie eine entsprechende Datei und das entsprechende Verzeichnis, oder ändern Sie den Pfad, sodass er zu Ihrer Textdatei führt, und füllen Sie diese Datei mit URLs von Bildern auf, die Sie testen möchten.

[!code-csharp[Main: set up test directory, read lines](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=38-51)]

## <a name="run-the-program"></a>Ausführen des Programms

Wenn Sie alle oben genannten Schritte ausgeführt haben, sollte das Programm jedes Bild verarbeiten (also relevante Tags von allen drei Diensten abfragen) und die Bilder dann zusammen mit den Taginformationen in das Content Moderator-Prüfungstool hochladen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Programm für die Produktbildanalyse eingerichtet, um sie mit produkttypspezifischen Tags zu versehen und ein Prüfungsteam beim Treffen fundierter Entscheidungen für die Inhaltsmoderation zu unterstützen. Machen Sie sich als Nächstes mit den Details der Bildmoderation vertraut.

> [!div class="nextstepaction"]
> [Überprüfen moderierter Bilder](./review-tool-user-guide/review-moderated-images.md)
