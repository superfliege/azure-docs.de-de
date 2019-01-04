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
ms.openlocfilehash: 6572a9b72554691441cb258a87a5db4ba7845087
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53077119"
---
# <a name="migrate-hub-workspace-and-projects-to-custom-translator"></a>Migrieren des Hub-Arbeitsbereichs und der Projekte zu Custom Translator

Sie können Ihren [Microsoft Translator-Hub](https://hub.microsofttranslator.com/)-Arbeitsbereich und Ihre Projekte ganz einfach zu Benutzerdefinierter Translator migrieren. Die Migration wird vom Microsoft-Hub aus initiiert, indem Sie einen Arbeitsbereich oder ein Projekt und anschließend einen Arbeitsbereich in Benutzerdefinierter Translator auswählen. Anschließend wählen Sie noch die Trainings aus, die Sie übertragen möchten.  Nach dem Start der Migration werden die Einstellungen für die ausgewählten Trainings mit allen relevanten Dokumenten übertragen.  Bereitgestellte Modelle werden trainiert und können nach Fertigstellung automatisch bereitgestellt werden.

Während der Migration werden die folgenden Aktionen ausgeführt:
* Die Namen aller Dokumente und Projektdefinitionen werden durch Voranstellen des Präfix „hub_“ vor dem Namen übertragen.  Automatisch generierte Test- und Optimierungsdaten werden in „hub_systemtune_<modelid>“ bzw. „hub_systemtest_<modelid>“ umbenannt.  
* Alle Trainings, die zum Zeitpunkt der Migration bereitgestellt waren, werden automatisch mit den Dokumenten des Hubtrainings trainiert.  Dieses Training wird Ihrem Abonnement nicht in Rechnung gestellt.  Wenn für die Migration die automatische Bereitstellung ausgewählt wurde, wird das trainierte Modell nach Abschluss bereitgestellt. Es gelten die regulären Hostinggebühren.  
* Auf alle migrierten Trainings, die nicht den Status „bereitgestellt“ hatten, wird der migrierte Entwurfsstatus angewandt.   In diesem Zustand haben Sie die Möglichkeit, ein Modell mit der migrierten Definition zu trainieren – dafür fallen jedoch die regulären Trainingsgebühren an.
* Sie können die BLEU-Bewertung aus dem Hubtraining jederzeit auf der Seite „TrainingDetails“ des Modells unter der Überschrift „Bleu score in MT Hub“ (BLEU-Punktzahl in MT-Hub) einsehen.

>[!Note]
>Damit ein Training erfolgreich ausgeführt werden kann, benötigt Custom Translator mindestens 10.000 extrahierte Sätze. Wenn weniger extrahierte Sätze als die [vorgeschlagene Mindestanzahl](sentence-alignment.md#suggested-minimum-number-of-extracted-and-aligned-sentences) vorhanden sind, kann Custom Translator kein Training ausführen.

## <a name="enable-account-migration"></a>Aktivieren der Migration von Konten 

Um das Migrationstool verwenden zu können, müssen Sie die Hubkontomigration aktivieren.  Senden Sie dazu eine E-Mail mit einer Liste aller LiveID-Konten, die Sie aktivieren möchten, an [custommt@microsoft.com](mailto:custommt@microsoft.com). Diese Konten sollten die E-Mail-Adressen sein, mit denen Sie sich anmelden.

## <a name="find-custom-translator-workspace-id"></a>Suchen der ID für den Benutzerdefinierter Translator-Arbeitsbereich

Für die Migration des [Microsoft Translator-Hub](https://hub.microsofttranslator.com/)-Arbeitsbereichs benötigen Sie die ID des Zielarbeitsbereichs in Benutzerdefinierter Translator. Der Zielarbeitsbereich in Custom Translator ist der Ort, an den alle Ihre Hub-Arbeitsbereiche und Projekte migriert werden sollen.

Die ID des Zielarbeitsbereichs finden Sie auf der Seite „Einstellungen“ von Benutzerdefinierter Translator: 

1.  Rufen Sie im Custom Translator-Portal die Seite „Einstellungen“ auf.

2.  Die ID des Arbeitsbereichs finden Sie im Abschnitt „Grundlegende Informationen“.

    ![So finden Sie die ID des Zielarbeitsbereichs](media/how-to/how-to-find-destination-ws-id.png)

3. Sie benötigen die ID des Zielarbeitsbereichs während des Migrationsprozesses.

## <a name="migrate-a-project"></a>Migrieren eines Projekts

Wenn Sie Ihre Projekte selektiv migrieren möchten, bietet Ihnen Microsoft Translator-Hub diese Möglichkeit.

So migrieren Sie ein Projekt:

1.  Melden Sie sich beim Microsoft Translator-Hub an.

2.  Rufen Sie die Seite „Projekte“ auf.

3.  Klicken sie auf den Link „Migrieren“ für das entsprechende Projekt.

    ![So führen Sie die Migration aus dem Hub durch](media/how-to/how-to-migrate-from-hub.png)

4.  Nach dem Aktivieren des Links für die Migration wird ein Formular angezeigt, in dem Sie Folgendes ausführen können:
   * Angeben des Arbeitsbereichs, den Sie zu Benutzerdefinierter Translator übertragen möchten
   * Angeben, ob alle erfolgreichen Trainings oder nur bereitgestellte Trainings übertragen werden sollen. Standardmäßig werden alle erfolgreichen Trainings übertragen.
   * Angeben, ob Ihr Training nach dem Abschluss automatisch bereitgestellt werden soll. Standardmäßig wird Ihr Training nach dem Abschluss nicht automatisch bereitgestellt.


5.  Klicken Sie auf „Anforderung übermitteln“.

## <a name="migrate-a-workspace"></a>Migrieren eines Arbeitsbereichs

Zusätzlich zum Migrieren eines einzelnen Projekts können Sie auch alle Projekte mit erfolgreichen Trainings in einem Arbeitsbereich migrieren.  Dabei wird für jedes Projekt im Arbeitsbereich überprüft, ob der Link für die Migration aktiviert wurde.  Diese Funktion eignet sich für Benutzer mit vielen Projekten, die alle mit den gleichen Einstellungen zu Benutzerdefinierter Translator migriert werden sollen.  Die Migration eines Arbeitsbereichs kann auf der Einstellungsseite des Translator-Hubs initiiert werden.

So migrieren Sie einen Arbeitsbereich:

1.  Melden Sie sich beim Microsoft Translator-Hub an.

2.  Rufen Sie die Seite „Einstellungen“ auf.

3.  Klicken Sie auf der Seite „Einstellungen“ auf „Arbeitsbereichsdaten zu Custom Translator migrieren“.

    ![So führen Sie die Migration aus dem Hub durch](media/how-to/how-to-migrate-workspace-from-hub.png)

4.  Wählen Sie auf der nächsten Seite eine der beiden Optionen aus:

    a.  Nur bereitgestellte Trainings: Nach Auswahl dieser Option werden nur Ihre bereitgestellten Systeme und zugehörige Dokumente migriert.

    b.  Alle erfolgreichen Trainings: Nach Auswahl dieser Option werden alle erfolgreichen Trainings und zugehörige Dokumente migriert.

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

3.  Projekte: Anzahl der für die Migration angeforderten Projekte im Vergleich zur Anzahl der erfolgreich migrierten Projekte.

4.  Trainings: Anzahl der für die Migration angeforderten Trainings im Vergleich zur Anzahl der erfolgreich migrierten Trainings.

5.  Dokumente: Anzahl der für die Migration angeforderten Dokumente im Vergleich zur Anzahl der erfolgreich migrierten Dokumente.

    ![Details des Migrationsverlaufs](media/how-to/how-to-migration-history-details.png)

Wenn Sie einen detaillierteren Migrationsbericht über Ihre Projekte, Trainings und Dokumente wünschen, haben Sie die Möglichkeit, Details als CSV zu exportieren.

## <a name="implementation-notes"></a>Hinweise zur Implementierung
* Das Migrieren eines Projekts von Hub zu Benutzerdefinierter Translator hat keinen Einfluss auf Ihre Hubtrainings oder -projekte. Wir löschen während einer Migration keine Projekte oder Dokumente in Hub, und wir heben keine Bereitstellungen von Modellen auf.
* Es ist nur eine Migration pro Projekt zulässig.  Wenn Sie eine Migration für ein Projekt wiederholen müssen, kontaktieren Sie uns.
* Zurzeit unterstützt Benutzerdefinierter Translator 36 Sprachen für die Übersetzung aus der englischen Sprache und in diese – und wir arbeiten daran, weitere Sprachen hinzufügen.  Hub erfordert keine Baselinemodelle und unterstützt daher mehrere Tausend Sprachen.  Sie können zwar ein nicht unterstütztes Sprachpaar migrieren, dabei werden aber nur die Dokumente und Projektdefinitionen migriert.  Sie können das neue Modell nicht trainieren.  Darüber hinaus werden diese Dokumente und Projekte als inaktiv angezeigt, um anzugeben, dass sie zu diesem Zeitpunkt nicht verwendbar sind. Wenn eine Unterstützung für diese Projekte und/oder Dokumente hinzugefügt wird, werden sie aktiviert und sind damit trainierbar.
* Benutzerdefinierter Translator unterstützt derzeit keine einsprachigen Trainingsdaten.  Wie bei nicht unterstützten Sprachpaaren können Sie einsprachige Dokumente zwar migrieren, aber sie werden als inaktiv angezeigt, bis einsprachige Daten unterstützt werden.  
* Benutzerdefinierter Translator benötigt für das Trainieren 10.000 parallele Sätze.  Microsoft-Hub konnte auch mit einem kleineren Datensatz trainiert werden.  Wenn ein Training, das diese Anforderungen nicht erfüllt, migriert wird, wird es trainiert.


## <a name="custom-translator-versus-hub"></a>Benutzerdefinierter Translator im Vergleich zum Hub

In der folgenden Tabelle werden die Features von Microsoft Translator Hub und Custom Translator gegenübergestellt:

|   | Hub | Benutzerdefinierter Translator |
|:-----|:----:|:----:|
|Status des Anpassungsfeatures   | Allgemeine Verfügbarkeit  | Allgemeine Verfügbarkeit |
| Version der Text-API  | V2    | V3  |
| SMT-Anpassung | JA   | Nein  |
| NMT-Anpassung | Nein     | JA |
| Neue einheitliche Anpassung der Sprachdienste | Nein     | JA |
| Keine Ablaufverfolgung | JA | JA |

## <a name="next-steps"></a>Nächste Schritte

- [Trainieren eines Modells](how-to-train-model.md).
- Beginnen Sie mit der Verwendung Ihres bereitstellten benutzerdefinierten Übersetzungsmodells über die [Microsoft-Textübersetzungs-API (V3)](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
