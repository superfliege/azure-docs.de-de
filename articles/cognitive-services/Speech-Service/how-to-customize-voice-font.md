---
title: Was ist Custom Voice? Azure Cognitive Services | Microsoft-Dokumentation
description: Dieser Artikel stellt einen Überblick über die Microsoft-Sprachsynthese dar, mit der Sie für Ihre Marke eine unverwechselbare Stimme erstellen.
services: cognitive-services
author: noellelacharite
manager: nolach
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2018
ms.author: nolach
ms.openlocfilehash: 7c4abb6832a030c2cb3cc2088dc5d0f1350a6ab8
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258846"
---
# <a name="creating-custom-voice-fonts"></a>Erstellen benutzerdefinierter Voicefonts

Mit der Microsoft-Text-to-Speech-API erstellen Sie für Ihre Marke eine unverwechselbare Stimme, die als *Voicefont* bezeichnet wird. 

Zur Erstellung Ihres Voicefonts laden Sie eine Tonstudioaufnahme und die zugehörigen Skripts als Trainingsdaten hoch. Der Dienst erstellt daraufhin ein individuelles Stimmenmodell, das auf Ihre Aufnahme abgestimmt ist. Dieses Voicefont können Sie anschließend zum Synthetisieren von Sprache verwenden. 

Zu Beginn sind für einen Proof of Concept nur wenige Daten erforderlich. Je mehr Daten Sie anschließend bereitstellen, desto natürlicher und professioneller klingt die Stimme.

Die Stimmanpassung ist für US-amerikanisches Englisch (en-us) und vereinfachtes Chinesisch (zh-CN) verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Das Text-to-Speech-API-Feature zur Anpassung einer Stimme befindet sich aktuell in der privaten Vorschau. Wenn Sie das Feature nutzen möchten, ist es erforderlich, dass Sie [das Antragsformular ausfüllen](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0N8Vcdi8MZBllkZb70o6KdURjRaUzhBVkhUNklCUEMxU0tQMEFPMjVHVi4u).

Außerdem benötigen Sie ein Azure-Konto und ein Abonnement für den Speech-Dienst. Falls dies noch nicht geschehen ist, müssen Sie ein [Abonnement erstellen](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started). Stellen Sie zwischen Ihrem Abonnement und dem Custom Voice-Portal wie folgt eine Verbindung her.

1. Melden Sie sich im [Custom Voice-Portal](https://customvoice.ai) mit demselben Microsoft-Konto an, mit dem Sie dem Nutzungsantrag gestellt haben.

2. Klicken Sie oben rechts unter Ihrem Kontonamen auf „Subscriptions“ (Abonnements).

    ![Abonnements](media/custom-voice/subscriptions.png)

3. Klicken Sie auf der Seite „Subscriptions“ (Abonnements) auf „Connect existing subscription“ (Verbindung mit vorhandenem Abonnement herstellen).

     ![Verbindung mit vorhandenem Abonnement herstellen](media/custom-voice/connect-existing-sub.png)

4. Fügen Sie Ihren Abonnementschlüssel wie unten gezeigt in die Tabelle ein. Jedem Abonnement sind zwei Schlüssel zugewiesen. Sie können beide verwenden.

     ![Abonnement hinzufügen](media/custom-voice/add-subscription.png)

Es kann losgehen!

## <a name="prepare-recordings-and-transcripts"></a>Vorbereiten von Aufnahmen und Transkripten

Ein Stimmentrainingsdataset umfasst mehrere Audiodateien und eine Textdatei, die die Transkripte aller Audiodateien enthält.

Sie können die Dateien auf zwei unterschiedliche Arten vorbereiten. Entweder erstellen Sie zunächst das Skript, das dann von einem Sprecher eingesprochen wird, oder Sie nutzen eine öffentlich zugängliche Audiodatei und transkribieren diese. Wenn Sie sich für Letzteres entscheiden, müssen Füllwörter wie „ähm“ sowie gestotterte, undeutlich gesprochene und falsch ausgesprochene Wörter entfernt werden.

Die Erstellung eines guten Voicefonts setzt voraus, dass die Aufnahmen in einem ruhigen Raum mit einem qualitativ hochwertigen Mikrofon erfolgen. Eine gleichmäßige Lautstärke, Geschwindigkeit und Tonhöhe sowie eine ausdrucksstarke Prosodie sind bei der Erstellung einer angenehmen digitalen Stimme entscheidend. Wenn Sie eine Stimme für eine Produktionsumgebung erstellen möchten, werden ein professioneller Sprecher und ein professionelles Tonstudio empfohlen. Weitere Informationen finden Sie im Artikel [Aufzeichnen von Sprachbeispielen für eine benutzerdefinierte Stimme](record-custom-voice-samples.md).

### <a name="audio-files"></a>Audiodateien

Jede Audiodatei sollte genau eine Äußerung enthalten. Dies kann beispielsweise ein Satz oder aber ein Gesprächsbeitrag eines Sprachdialogsystems sein. Alle Dateien müssen in derselben Sprache vorliegen. Die Anpassung von Stimmen für mehrere Sprachen wird nicht unterstützt. Zusätzlich müssen die Audiodateien über einen eindeutigen numerischen Dateinamen mit der Erweiterung `.wav` verfügen.

Die Audiodateien sollten so vorbereitet werden, dass sie die unten aufgeführten Bedingungen erfüllen. Andere Formate werden nicht unterstützt und abgelehnt.

| **Eigenschaft** | **Wert** |
| ------------ | --------- |
| Dateiformat  | RIFF (WAV)|
| Samplingrate| Mindestens 16.000 Hz |
| Beispielformat| PCM, 16 Bit |
| Dateiname    | Numerisch mit `.wav`-Erweiterung |
| Archivformat| ZIP      |
| Maximale Archivgröße|200 MB|

Stellen Sie alle Dateien in genau einem Ordner ohne Unterverzeichnisse zur Verfügung, und erstellen Sie daraus ein Paket mit einem ZIP-Datei-Archiv.

> [!NOTE]
> WAVE-Dateien mit einer Samplingrate von unter 16.000 Hz werden zurückgewiesen. Wenn eine ZIP-Datei WAVE-Dateien mit unterschiedlichen Samplingraten enthält, werden nur solche importiert, deren Rate gleich oder höher als 16.000 Hz ist.
> Aktuell werden im Portal ZIP-Archiv-Importe von bis zu 200 MB unterstützt. Sie haben allerdings die Möglichkeit, auch mehrere Archive hochzuladen. Benutzer des kostenlosen Abonnements können maximal zehn ZIP-Dateien als Datasets hochladen. Bei Benutzern des Standardabonnements liegt die Höchstzahl bei 50.

### <a name="transcripts"></a>Transkripte

Die Transkriptionsdatei ist eine reine Textdatei (ANSI/UTF-8/UTF-8-BOM/UTF-16-LE/UTF-16-BE). In jeder Zeile der Transkriptionsdatei muss der Name einer Audiodatei vermerkt sein. Darauf folgen das Tabstoppzeichen (Codepunkt 9) und das Transkript. Leeren Zeilen sind nicht erlaubt.

Beispiel: 

```
0000000001  This is the waistline, and it's falling.
0000000002  We have trouble scoring.
0000000003  It was Janet Maslin.
```

Das Custom Voice-System normalisiert die Transkripte, indem der Text in Kleinbuchstaben umgewandelt wird und unwichtige Interpunktionszeichen entfernt werden. Entscheidend ist, dass die Transkripte genau mit den zugehörigen Audioaufnahmen übereinstimmen.

> [!TIP]
> Beim Erstellen von Stimmen mit der Text-to-Speech-API für eine Produktionsumgebung sollten Sie bei der Auswahl der Äußerungen (oder beim Schreiben der Skripte) sowohl auf eine ausreichende Anzahl phonetischer Eigenschaften als auch auf die phonetische Effizienz achten.

## <a name="upload-your-datasets"></a>Hochladen von Datasets

Nach der Vorbereitung des Audiodateiarchivs und der Transkripte können Sie diese über das [Custom Voice-Dienstportal](https://customvoice.ai) hochladen.

> [!NOTE]
> Nach dem Upload können Datasets nicht mehr bearbeitet werden. Wenn Sie beispielsweise vergessen haben, für Audiodateien Transkripte bereitzustellen, oder das falsche Geschlecht für die Stimme ausgewählt haben, müssen Sie das gesamte Dataset noch einmal hochladen. Überprüfen Sie das Dataset und die Einstellungen sorgfältig vor dem Upload.

1. Melden Sie sich beim Portal an.

2. Klicken Sie unter „Custom Voice“ auf der Hauptseite auf **Data** (Daten). 

    ![Meine Projekte](media/custom-voice/my-projects.png)

    Die Datentabelle „My Voice Data“ (Meine Stimmdaten) wird angezeigt. Wenn Sie noch keine Datasets für Ihre Stimme hochgeladen haben, ist die Tabelle leer.

3. Klicken Sie auf **Import data** (Daten importieren), um die Seite zum Hochladen eines neuen Datasets zu öffnen.

    ![Stimmdaten importieren](media/custom-voice/import-voice-data.png)

4. Geben Sie einen Namen und eine Beschreibung in den entsprechenden Feldern ein. 

5. Wählen Sie das Gebietsschema für Ihre Voicefonts aus. Stellen Sie sicher, dass die Informationen für das Gebietsschema mit der Sprache übereinstimmen, die für die Aufnahmedaten und die Skripte verwendet wurde. 

6. Wählen Sie das Geschlecht des Sprechers aus, dessen Stimme Sie verwenden.

7. Wählen Sie das Skript und die Audiodateien aus, die Sie hochladen möchten. 

8. Klicken Sie auf **Import** (Importieren), um die Daten hochzuladen. Bei größeren Datasets kann der Importvorgang mehrere Minuten dauern.

> [!NOTE]
> Benutzer des kostenlosen Abonnements können zwei Datasets gleichzeitig hochladen. Bei Benutzern des Standardabonnements sind es fünf. Wenn Sie die maximale Uploadanzahl erreicht haben, müssen Sie warten, bis der Importvorgang eines oder mehrerer Datasets beendet wurde. Versuchen Sie es anschließend erneut.

Nach dem Upload wird die Datentabelle „My Voice Data“ (Meine Stimmdaten) erneut angezeigt. In dieser sollte sich ein Eintrag für das hochgeladene Dataset befinden. 

Datasets werden nach dem Hochladen automatisch überprüft. Bei der Datenüberprüfung wird darauf geachtet, dass die Audiodateien im richtigen Format und in der richtigen Größe vorliegen und die korrekte Samplingfrequenz verwendet wird. Bei der Überprüfung der Transkriptionsdateien wird sichergestellt, dass diese im richtigen Dateiformat vorliegen. Außerdem wird eine Textnormalisierung durchgeführt. Die Äußerungen werden mithilfe der Spracherkennung transkribiert. Der daraus entstehende Text wird anschließend mit dem bereitgestellten Transkript verglichen.

![Meine Stimmdaten](media/custom-voice/my-voice-data.png)

In der folgenden Tabelle werden die Verarbeitungsstatus der importierten Datasets angezeigt. 

| State (Zustand) | Bedeutung
| ----- | -------
| `NotStarted` | Das Dataset wurde hochgeladen und befindet sich in der Verarbeitungswarteschlange.
| `Running` | Das Dataset wird überprüft.
| `Succeeded` | Das Dataset wurde überprüft und kann nun zur Erstellung eines Voicefonts verwendet werden.

Nachdem die Überprüfung abgeschlossen ist, wird die Gesamtzahl der übereinstimmenden Äußerungen für alle Datasets in der Spalte „Utterance“ (Äußerung) angezeigt.

Sie können einen Bericht herunterladen, um die Aussprachebewertung und den Rauschpegel aller Aufnahmen zu überprüfen. Die Aussprachebewertung liegt zwischen 0 und 100. Werte unter 70 deuten in der Regel auf einen Aussprachefehler oder auf einen Fehler bei der Skriptzuordnung hin. Ein starker Akzent kann die Aussprachebewertung verringern und die Qualität der erzeugten digitalen Stimme negativ beeinflussen.

Ein höheres Signal-Rausch-Verhältnis (SNR) deutet auf ein geringeres Rauschen in den Audioaufnahmen hin. Wenn Ihre Aufnahmen in einem professionellen Tonstudio durchgeführt wurden, können Sie SNR-Werte größer gleich 50 erzielen. Ein SNR-Wert unter 20 kann dazu führen, dass in der generierten Stimme das Rauschen deutlich zu hören ist.

Bei einer niedrigen Aussprachebewertung oder einem geringen SNR-Wert sollten Sie die betroffenen Äußerungen erneut aufnehmen. Wenn dies nicht möglich ist, können Sie diese Äußerungen aus dem Dataset ausschließen.

## <a name="build-your-voice-font"></a>Erstellen des Voicefonts

Nachdem das Dataset überprüft wurde, können Sie mit diesem Ihr eigenes Voicefont erstellen. 

1. Klicken Sie im Dropdownmenü „Custom Voice“ auf **Models** (Modelle). 
 
    Die Tabelle „My Voice Fonts“ (Meine Voicefonts) wird angezeigt. In dieser werden alle Voicefonts aufgeführt, die Sie bereits erstellt haben.

1. Klicken Sie unter der Tabellenüberschrift auf **Create voices** (Stimmen erstellen). 

    Die Seite zum Erstellen eines Voicefonts wird angezeigt. Das aktuelle Gebietsschema wird in der ersten Zeile der Tabelle angezeigt. Wenn Sie eine Stimme in einer anderen Sprache erstellen möchten, können Sie das Gebietsschema ändern. Das Gebietsschema muss mit dem Gebietsschema der Datasets übereinstimmen, die zum Erstellen der Stimme verwendet wurden.

1. Geben Sie wie bereits beim Upload des Datasets einen Namen und eine Beschreibung ein, damit Sie dieses Modell von anderen Modellen unterscheiden können. 

    Der hier eingegebene Name ist der Name, der zur Festlegung der Stimme in der Sprachsyntheseanforderung als Teil der SSML-Eingabe verwendet wird. Sie sollten den Namen daher mit Bedacht wählen. Zulässig sind nur Buchstaben, Zahlen und einige wenige Satzzeichen wie „-“, „_“, „(“ und „)“.

    Im Beschreibungsfeld werden in der Regel die Datasets erfasst, die zur Erstellung des Modells verwendet wurden.

1. Wählen Sie das Geschlecht für Ihren Voicefont aus. Das Geschlecht muss mit demjenigen übereinstimmen, das für die Datasets verwendet wurde.

1. Wählen Sie die Datasets aus, mit denen der Voicefont trainiert werden soll. Die verwendeten Datasets müssen von genau einem Sprecher stammen.

1. Klicken Sie auf **Create** (Erstellen), um mit der Erstellung des Voicefonts zu beginnen.

    ![Modell erstellen](media/custom-voice/create-model.png)

Das neue Modell wird in der Tabelle „My Voice Fonts “ (Meine Voicefonts) angezeigt. 

![Meine Voicefonts](media/custom-voice/my-voice-fonts.png)

Der angezeigte Status gibt Aufschluss über den Prozess, durch den das Dataset in einen Voicefont konvertiert wird. Folgende Status werden verwendet:

| State (Zustand) | Bedeutung
| ----- | -------
| `NotStarted` | Die Anforderung zur Erstellung eines Voicefonts befindet sich in der Verarbeitungswarteschlange.
| `Running` | Der Voicefont wird erstellt.
| `Succeeded` | Der Voicefont wurde erstellt und kann bereitgestellt werden.

Die Trainingszeit variiert je nach Umfang der verarbeiteten Audiodaten. In der Regel liegt diese zwischen 30 Minuten bei mehreren Hundert Äußerungen und maximal 40 Stunden im Fall von 20.000 Äußerungen.

> [!NOTE]
> Benutzer mit einem kostenlosen Abonnement können jeweils nur einen Voicefont trainieren. Bei Benutzern des Standardabonnements sind es drei. Wenn die maximale Uploadanzahl erreicht wurde, müssen Sie warten, bis der Trainingsvorgang eines oder mehrerer Voicefonts beendet wurde. Versuchen Sie es anschließend erneut.

## <a name="test-your-voice-font"></a>Testen des Voicefonts

Nach der erfolgreichen Erstellung des Voicefonts können Sie ihn vor der Bereitstellung testen. Klicken Sie in der Spalte „Operations“ (Vorgänge) auf **Test** (Testen). Die Testseite wird für den ausgewählten Voicefont angezeigt. Wenn Sie noch keine Testanforderungen für die Stimme gesendet haben, ist die Tabelle leer.

![Meine Voicefonts, Teil 2](media/custom-voice/my-voice-fonts2.png)

Klicken Sie unter der Tabellenüberschrift auf **Test with text** (Mit Text testen). Daraufhin wird ein Kontextmenü angezeigt, indem Sie die Textanforderung senden können. Sie können für Ihre Testanforderung entweder Nur-Text oder SSML verwenden. Die maximale Eingabegröße liegt bei 1024 Zeichen einschließlich aller Tags für die SSML-Anforderung. Die Textsprache muss mit der Sprache des Voicefonts übereinstimmen.

![Testen des Voicefonts](media/custom-voice/voice-font-testing.png)

Füllen Sie zuerst das Textfeld aus, und wählen Sie den Eingabemodus aus. Klicken Sie anschließend auf **Yes** (Ja), um Ihre Testanforderung zu senden und zur Testseite zurückzukehren. In der Tabelle befindet sich nun ein Eintrag für die neue Anforderung und die Statusspalte. Die Sprachsynthese kann einige Minuten in Anspruch nehmen. Wenn in der Statusspalte „Succeeded“ (Erfolgreich) angezeigt wird, können Sie die Texteingabe (eine `.txt`-Datei) und die Audioausgabe (eine `.wav`-Datei) herunterladen und die Qualität letzterer überprüfen.

![Testen des Voicefonts, Teil 2](media/custom-voice/voice-font-testing2.png)

## <a name="create-and-use-a-custom-endpoint"></a>Erstellen und Verwenden eines benutzerdefinierten Endpunkts

Nach dem erfolgreichen Erstellen und Testen des Sprachmodells können Sie es auf einem benutzerdefinierten Text-to-Speech-API-Endpunkt bereitstellen. Diesen verwenden Sie anschließend anstelle des üblichen Endpunkts beim Senden von Text-to-Speech-API-Anforderungen über die REST-API. Der benutzerdefinierte Endpunkt kann nur von dem Abonnement aufgerufen werden, mit dem Sie den Voicefont bereitgestellt haben.

Wählen Sie zum Erstellen eines Endpunkts aus dem Menü „Custom Voice“ am oberen Rand der Seite die Option **Endpoints** (Endpunkte) aus. Die Seite „My Deployed Voices“ (Meine bereitgestellten Stimmen) mit der Tabelle der aktuellen Stimmenendpunkte wird angezeigt. Das aktuelle Gebietsschema wird in der ersten Zeile der Tabelle angezeigt. Wenn Sie eine Bereitstellung für eine andere Sprache durchführen möchten, müssen Sie das Gebietsschema ändern. Dieses muss mit dem Gebietsschema der Stimme übereinstimmen, die Sie bereitstellen.

Klicken Sie auf **Deploy voices** (Stimmen bereitstellen), um einen Endpunkt zu erstellen. Geben Sie den Namen und die Beschreibung für den benutzerdefinierten Endpunkt an.

Wählen Sie im Menü „Subscription“ (Abonnement) das Abonnement aus, das Sie verwenden möchten. Benutzer des kostenlosen Abonnements können nur ein Modell gleichzeitig bereitstellen. Benutzer des Standardabonnements können bis zu 20 Endpunkte erstellen, wobei für jeden eine eigene Stimme verwendet werden kann.

![Endpunkt erstellen](media/custom-voice/create-endpoint.png)

Klicken Sie nach der Auswahl des bereitzustellenden Modells auf **Create** (Erstellen). Die Seite „My Deployed Voices“ (Meine bereitgestellten Stimmen) wird erneut angezeigt und enthält nun einen Eintrag für den neuen Endpunkt. Das Instanziieren des neuen Endpunkts kann einige Minuten in Anspruch nehmen. Wenn als Bereitstellungsstatus „Succeeded“ (Erfolgreich) angezeigt wird, kann der Endpunkt verwendet werden.

![Meine bereitgestellten Stimmen](media/custom-voice/my-deployed-voices.png)

Wenn als Bereitstellungsstatus „Succeeded“ (Erfolgreich) angezeigt wird, wird der Endpunkt des bereitstellen Voicefonts in der Tabelle „My Deployed Voices“ (Meine bereitgestellten Stimmen) angezeigt. Sie können diesen URI direkt in einer HTTP-Anforderung verwenden.

Die Onlineüberprüfung des Endpunkts kann auch über das Custom Voice-Portal erfolgen. Wählen Sie zum Testen des Endpunkts **Endpoints testing** (Endpunkte testen) aus dem Custom Voice-Dropdownmenü aus. Die Seite zum Testen des Endpunkts wird angezeigt. Wählen Sie eine bereitgestellte benutzerdefinierte Stimme aus, und geben Sie im Textfeld den Text ein, der vorgelesen werden soll. Als Eingabe kann entweder das Nur-Text- oder SSML-Format verwendet werden.

> [!NOTE] 
> Wenn Sie das SSML-Format verwenden, muss mit dem `<voice>`-Tag der Namen angegeben werden, den Sie bei der Erstellung Ihrer individuellen Stimme verwendet haben.

Klicken Sie auf **Play** (Wiedergabe), damit der Text von Ihrem benutzerdefinierten Voicefont vorgelesen wird.

![Testen von Endpunkten](media/custom-voice/endpoint-testing.png)

Der benutzerdefinierte Endpunkt verfügt über dieselben Funktionen wie der Standardendpunkt für Text-to-Speech-API-Anforderungen. Weitere Informationen finden Sie unter [REST-API](rest-apis.md).

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
