---
title: Migrieren des Microsoft Translator-Hub-Arbeitsbereichs und der Projekte? – Custom Translator
titleSuffix: Azure Cognitive Services
description: Migrieren Sie den Hub-Arbeitsbereich und die Projekte zu Custom Translator.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 611cd0878e88d2e1c0a988f73b57e391c5a8551d
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2018
ms.locfileid: "51975906"
---
# <a name="migrate-hub-workspace-and-projects-to-custom-translator"></a>Migrieren des Hub-Arbeitsbereichs und der Projekte zu Custom Translator

Sie können Ihren [Microsoft Translator-Hub](https://hub.microsofttranslator.com/)-Arbeitsbereich und Ihre Projekte zu Custom Translator migrieren. Die Migration wird über den Hub gestartet.


Diese Elemente werden während des Prozesses migriert:

1.  Die Definitionen für Ihr(e) Projekt(e).

2.  Die Trainingsdefinition wird zum Erstellen einer neuen Modelldefinition in Custom Translator verwendet.

3.  Die parallelen und monolingualen Dateien, die in den Trainings verwendet werden, werden alle als neue Dokumente in Custom Translator migriert.

4.  Die automatisch generierten Daten für den Systemtest und die Optimierung werden exportiert und als neue Dokumente im Custom Translator erstellt.

Für alle bereitgestellten Trainings trainiert Custom Translator das Modell ohne zusätzliche Kosten. Sie können sie manuell bereitstellen.

>[!Note]
>Damit ein Training erfolgreich ausgeführt werden kann, benötigt Custom Translator mindestens 10.000 extrahierte Sätze. Wenn weniger extrahierte Sätze als die [vorgeschlagene Mindestanzahl](sentence-alignment.md#suggested-minimum-number-of-extracted-and-aligned-sentences) vorhanden sind, kann Custom Translator kein Training ausführen.

Alle erfolgreichen Training, die nicht bereitgestellt werden, werden als Entwurf in Custom Translator migriert. benutzerdefinierten Konverter migriert werden.

## <a name="find-custom-translator-workspace-id"></a>Suchen der ID für den Custom Translator-Arbeitsbereich

Für die Migration des [Microsoft Translator-Hub](https://hub.microsofttranslator.com/)-Arbeitsbereichs benötigen Sie die ID des Definitionsarbeitsbereichs in Custom Translator. Der Zielarbeitsbereich in Custom Translator ist der Ort, an den alle Ihre Hub-Arbeitsbereiche und Projekte migriert werden sollen.

Die ID des Zielarbeitsbereichs finden Sie auf der Seite „Einstellungen“ von Custom Translator: 

1.  Rufen Sie im Custom Translator-Portal die Seite „Einstellungen“ auf.

2.  Die ID des Arbeitsbereichs finden Sie im Abschnitt „Grundlegende Informationen“.

    ![So finden Sie die ID des Zielarbeitsbereichs](media/how-to/how-to-find-destination-ws-id.png)

3. Sie benötigen die ID des Zielarbeitsbereichs während des Migrationsprozesses.

## <a name="migrate-workspace"></a>Migrieren des Arbeitsbereichs

Wenn Sie Ihren gesamten Hub-Arbeitsbereich zu Custom Translator migrieren, werden Ihre Projekte, Dokumente und Trainings auf Custom Translator übertragen. Vor der Migration müssen Sie wählen, ob Sie nur bereitgestellt Trainings oder alle Ihre erfolgreichen Trainings migrieren möchten.

So migrieren Sie einen Arbeitsbereich:

1.  Melden Sie sich beim Microsoft Translator-Hub an.

2.  Rufen Sie die Seite „Einstellungen“ auf.

3.  Klicken Sie auf der Seite „Einstellungen“ auf „Arbeitsbereichsdaten zu Custom Translator migrieren“.

    ![So führen Sie die Migration aus dem Hub durch](media/how-to/how-to-migrate-workspace-from-hub.png)

4.  Wählen Sie auf der nächsten Seite eine der beiden Optionen aus:

    a.  Nur bereitgestellte Trainings: Nach Auswahl dieser Option werden nur Ihre bereitgestellten Systeme und dazugehörige Dokumente migriert.

    b.  Alle erfolgreichen Trainings: Nach Auswahl dieser Option werden alle erfolgreichen Trainings und dazugehörige Dokumente migriert.

    c.  Geben Sie die ID des Zielarbeitsbereichs in Custom Translator ein.

    ![So führen Sie die Migration aus dem Hub durch](media/how-to/how-to-migrate-from-hub-screen.png)

5.  Klicken Sie auf „Anforderung übermitteln“.

## <a name="migrate-project"></a>Migrieren eines Projekts

Wenn Sie Ihre Projekte selektiv migrieren möchten, bietet Ihnen Microsoft Translator-Hub diese Möglichkeit.

So migrieren Sie ein Projekt:

1.  Melden Sie sich beim Microsoft Translator-Hub an.

2.  Rufen Sie die Seite „Projekte“ auf.

3.  Klicken sie auf den Link „Migrieren“ für das entsprechende Projekt.

    ![So führen Sie die Migration aus dem Hub durch](media/how-to/how-to-migrate-from-hub.png)

4.  Wählen Sie auf der nächsten Seite eine der beiden Optionen aus:

    a.  Nur bereitgestellte Trainings: Nach Auswahl dieser Option werden nur Ihre bereitgestellten Systeme und dazugehörige Dokumente migriert. 

    b.  Alle erfolgreichen Trainings: Nach Auswahl dieser Option werden alle erfolgreichen Trainings und dazugehörige Dokumente migriert.

    c.  Geben Sie die ID des Zielarbeitsbereichs in Custom Translator ein.

    ![So führen Sie die Migration aus dem Hub durch](media/how-to/how-to-migrate-from-hub-screen.png)

5.  Klicken Sie auf „Anforderung übermitteln“.

## <a name="migration-history"></a>Migrationsverlauf

Wenn Sie die Migration von Arbeitsbereichen/Projekten vom Hub aus angefordert haben, finden Sie Ihren Migrationsverlauf auf der Seite „Einstellungen“ von Custom Translator. 

Um den Migrationsverlauf anzuzeigen, gehen Sie folgendermaßen vor:

1.  Rufen Sie im Custom Translator-Portal die Seite „Einstellungen“ auf.

2.  Klicken Sie im Abschnitt „Migrationsverlauf“ auf der Seite „Einstellungen“ auf „Migrationsverlauf“.

    ![Migrationsverlauf](media/how-to/how-to-migration-history.png)

Auf der Seite „Migrationsverlauf“ werden die folgenden Informationen als Zusammenfassung für jede angeforderte Migration angezeigt:

1.  Migriert von: Name und E-Mail-Adresse des Benutzers, der diese Migrationsanforderung gesendet hat

2.  Migriert am: Datum und Zeitstempel der Migration

3.  Projekte: Die Anzahl der für die Migration angeforderten Projekte im Vergleich zur Anzahl der erfolgreich migrierten Projekte.

4.  Trainings: Die Anzahl der für die Migration angeforderten Trainings im Vergleich zur Anzahl der erfolgreich migrierten Trainings.

5.  Dokumente: Die Anzahl der für die Migration angeforderten Dokumente im Vergleich zur Anzahl der erfolgreich migrierten Dokumente.

    ![Details des Migrationsverlaufs](media/how-to/how-to-migration-history-details.png)

Wenn Sie einen detaillierteren Migrationsbericht über Ihre Projekte, Trainings und Dokumente wünschen, haben Sie die Möglichkeit, Details als CSV zu exportieren.

>[!Note]
>Die Migration wird nur für die Sprachpaaren unterstützt, in denen es NMT-Sprachen gibt. Überprüfen Sie die Liste der derzeit [unterstützten NMT-Sprachen](https://www.microsoft.com/translator/business/languages/). Für die Sprachpaare, in denen es keine NMT-Sprachen gibt, werden die Daten vom Hub zum Custom Translator verschoben, aber es können keine Trainings für diese Sprachpaare durchgeführt werden.

## <a name="custom-translator-versus-hub"></a>Benutzerdefinierter Translator im Vergleich zum Hub

In der folgenden Tabelle werden die Features von Microsoft Translator Hub und Custom Translator gegenübergestellt:

|   | Hub | Custom Translator |
|:-----|:----:|:----:|
|Status des Anpassungsfeatures   | Allgemeine Verfügbarkeit  | Vorschau |
| Version der Text-API  | V2    | V3  |
| SMT-Anpassung | JA   | Nein  |
| NMT-Anpassung | Nein     | JA |
| Neue einheitliche Anpassung der Sprachdienste | Nein     | JA |
| Keine Ablaufverfolgung | JA | JA |

## <a name="next-steps"></a>Nächste Schritte

- [Trainieren eines Modells](how-to-train-model.md).
- Beginnen Sie mit der Verwendung Ihres bereitstellten benutzerdefinierten Übersetzungsmodells über die [Microsoft-Textübersetzungs-API (V3)](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
