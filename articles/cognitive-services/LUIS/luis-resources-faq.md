---
title: Häufig gestellte Fragen zu Language Understanding (LUIS) in Azure | Microsoft-Dokumentation
description: Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Language Understanding (LUIS).
author: v-geberr
manager: kaiqb
services: cognitive-services
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: fd63ffd312e3ac17a6376eb3c9bef8f1978e3935
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333614"
---
# <a name="language-understanding-faq"></a>Häufig gestellte Fragen zu Language Understanding

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Language Understanding (LUIS).

## <a name="luis-authoring"></a>Erstellen von LUIS-Apps

### <a name="what-are-the-luis-best-practices"></a>Welche Best Practices gibt es für LUIS? 
Lesen Sie zunächst [Authoring Cycle](luis-concept-app-iteration.md) (Erstellungszyklus) und anschließend [Best Practices](luis-concept-best-practices.md). 

### <a name="what-is-the-best-way-to-start-building-my-app-in-luis"></a>Wie sollte ich bei der Erstellung einer LUIS-App vorgehen?

Bei der App-Erstellung sollte ein [inkrementeller Prozess](luis-concept-app-iteration.md) verwendet werden. 

### <a name="what-is-a-good-practice-to-model-the-intents-of-my-app-should-i-create-more-specific-or-more-generic-intents"></a>Wie sollte ich Absichten in meiner App modellieren? Sind spezifische oder allgemeine Absichten zu bevorzugen?

Die Absichten sollten nicht so allgemein sein, dass Überlappungen auftreten. Sie sollten aber auch nicht so spezifisch sein, dass LUIS nicht mehr zwischen ähnlichen Absichten unterscheiden kann. Als Best Practice für die LUIS-Modellierung empfiehlt sich die Erstellung spezifischer, aber unterscheidbarer Absichten.

### <a name="is-it-important-to-train-the-none-intent"></a>Sollte die Absicht „None“ (Keine) trainiert werden?

Ja, beim Hinzufügen von weiteren Bezeichnungen für andere Absichten sollten Sie auch die Absicht **None** mit zusätzlichen Äußerungen trainieren. Für jede zehnte Absichtsbezeichnung sollten ein oder zwei Bezeichnungen zu **None** hinzugefügt werden. Durch dieses Verhältnis wird die Unterscheidungsfähigkeit von LUIS optimiert.

### <a name="how-can-i-correct-spelling-mistakes-in-utterances"></a>Wie lassen sich Rechtschreibfehler in Äußerungen korrigieren?

Informationen hierzu finden Sie im Tutorial zur [Bing-Rechtschreibprüfungs-API v7](luis-tutorial-bing-spellcheck.md). LUIS setzt die Einschränkungen um, die durch die Bing-Rechtschreibprüfungs-API v7 vorgegeben werden. 

### <a name="how-do-i-edit-my-luis-app-programmatically"></a>Wie kann ich eine LUIS-App programmgesteuert bearbeiten?
Sie können Ihre LUIS-App mit der [Erstellungs-API](https://aka.ms/luis-authoring-apis) programmgesteuert bearbeiten. Unter [Aufrufen der Erstellungs-API für LUIS-Apps](./luis-quickstart-node-add-utterance.md) und [Build a LUIS app programmatically using Node.js (Programmgesteuertes Erstellen einer LUIS-App mit Node.js)](./luis-tutorial-node-import-utterances-csv.md) finden Sie Beispiele zum Aufrufen der Erstellungs-API. Die Erstellungs-API setzt voraus, dass Sie einen [Erstellungsschlüssel](luis-concept-keys.md#authoring-key) anstelle eines Endpunktschlüssels verwenden. Bei der programmgesteuerte Erstellung sind bis 1.000.000 Aufrufe pro Monat und fünf Transaktionen pro Sekunde möglich. Weitere Informationen zu den mit LUIS verwendeten Schlüsseln finden Sie unter [Verwalten von Schlüsseln](./luis-concept-keys.md).

### <a name="where-is-the-pattern-feature-that-provided-regular-expression-matching"></a>Wo finde ich das Feature für Muster, mit den bisher nach regulären Ausdrücken gesucht werden konnte?
Das bisherige **Feature für Muster** ist als veraltet gekennzeichnet und wurde durch **[Muster](luis-concept-patterns.md)** ersetzt. 

### <a name="how-do-i-use-an-entity-to-pull-out-the-correct-data"></a>Wie extrahiere ich mithilfe von Entitäten bestimmte Daten? 
Informationen hierzu finden Sie unter [Entitäten](luis-concept-entity-types.md) und [Datenextraktion](luis-concept-data-extraction.md).

### <a name="should-variations-of-an-example-utterance-include-punctuation"></a>Sollten Variationen einer Beispieläußerung Satzzeichen enthalten? 
Sie sollten die Variationen entweder als Beispieläußerung der Absicht hinzufügen oder das Muster der Beispieläußerung zusammen mit der [Syntax](luis-concept-patterns.md#pattern-syntax) hinzufügen, durch die Satzzeichen ignoriert werden. 

## <a name="luis-endpoint"></a>LUIS-Endpunkt

### <a name="why-does-luis-add-spaces-to-the-query-around-or-in-the-middle-of-words"></a>Warum fügt LUIS vor und nach Wörtern oder in der Mitte von Wörtern Leerzeichen in die Abfrage ein?
LUIS nutzt für die [Tokenisierung](luis-supported-languages.md#tokenization) der Äußerung deren [Kultur](luis-glossary.md#token). Sowohl der ursprüngliche Wert als auch der nach der Tokenisierung vorhandene Wert können [extrahiert](luis-concept-data-extraction.md#tokenized-entity-returned) werden.

### <a name="how-do-i-create-and-assign-a-luis-endpoint-key"></a>Wie lässt sich ein LUIS-Endpunkt erstellen und zuweisen?
Erstellen Sie zunächst für Ihren [Servicelevel](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) den [Endpunktschlüssel](luis-how-to-azure-subscription.md#create-luis-endpoint-key) in Azure. Anschließend müssen Sie den Schlüssel auf auf der Seite **[Veröffentlichen](publishapp.md)** [zuweisen](Manage-keys.md#assign-endpoint-key). Für diese Aktion steht keine API zur Verfügung. Danach müssen Sie die HTTP-Anforderung an den Endpunkt so ändern, dass der [neue Endpunktschlüssel](luis-concept-keys.md#use-endpoint-key-in-query) verwendet wird.

### <a name="how-do-i-interpret-luis-scores"></a>Wie lassen sich LUIS-Bewertungen interpretieren? 
Ihr System sollte die am höchsten bewertete Absicht unabhängig vom Wert verwenden. Ein Wert unter 0,5 (weniger als 50 %) ist nicht zwangsläufig mit einer geringen Zuverlässigkeit von LUIS gleichzusetzen. Durch das Bereitstellen weiterer Trainingsdaten kann die Bewertung für die wahrscheinlichste Absicht verbessert werden.

### <a name="why-dont-i-see-my-endpoint-hits-in-my-apps-dashboard"></a>Warum werden Endpunktabrufe nicht im App-Dashboard angezeigt?
Die Gesamtzahl der Endpunktabrufe in Ihrem App-Dashboard wird regelmäßig aktualisiert. Die Metriken, die dem LUIS-Abonnementschlüssel im Azure-Portal zugeordnet sind, werden jedoch häufiger aktualisiert. 

Wenn die Endpunktabrufe im Dashboard nicht aktualisiert werden, müssen Sie sich beim Azure-Portal anmelden, nach der Ressource suchen, die Ihrem LUIS-Abonnementschlüssel zugeordnet ist, und **Metriken** öffnen. Dort wählen Sie dann die Metrik **Aufrufe gesamt** aus. Wenn der Abonnementschlüssel für mehr als eine LUIS-App verwendet wird, zeigt die Metrik im Azure-Portal die aggregieren Aufrufe durch alle LUIS-Apps an, die den Schlüssel verwenden.

### <a name="my-luis-app-was-working-yesterday-but-today-im-getting-403-errors-i-didnt-change-the-app-how-do-i-fix-it"></a>Bisher hat meine LUIS-App problemlos funktioniert. Nun werden jedoch 403-Fehlermeldungen angezeigt. Ich habe die App nicht geändert. Wie behebe ich das Problem? 
Führen Sie die Schritte in der [nächsten FAQ-Anleitung](#how-do-i-create-and-assign-a-luis-endpoint-key) durch, um einen LUIS-Endpunktschlüssel zu erstellen und ihn der App zuzuweisen. Danach müssen Sie die HTTP-Anforderung an den Endpunkt so ändern, dass der [neue Endpunktschlüssel](luis-concept-keys.md#use-endpoint-key-in-query) verwendet wird.

### <a name="how-do-i-secure-my-luis-endpoint"></a>Wie sichere ich meinen LUIS-Endpunkt? 
Informationen hierzu finden Sie unter [Securing the endpoint (Schützen von Endpunkten)](luis-concept-security.md#securing-the-endpoint).

## <a name="working-within-luis-limits"></a>LUIS-Grenzwerte

### <a name="what-is-the-maximum-number-of-intents-and-entities-that-a-luis-app-can-support"></a>Wie viele Absichten und Entitäten kann eine LUIS-App maximal unterstützen?
Informationen hierzu finden Sie in der Referenzdokumentation zu [Begrenzungen](luis-boundaries.md).

### <a name="i-want-to-build-a-luis-app-with-more-than-the-maximum-number-of-intents-what-should-i-do"></a>Ich möchte eine LUIS-App erstellen, die mehr als die maximale Absichtsanzahl unterstützt. Wie sollte ich vorgehen?

Informationen hierzu finden Sie unter [Best Practices für Absichten](luis-concept-intent.md#if-you-need-more-than-the-maximum-number-of-intents).

### <a name="i-want-to-build-an-app-in-luis-with-more-than-the-maximum-number-of-entities-what-should-i-do"></a>Ich möchte eine LUIS-App erstellen, die mehr als die maximale Entitätszahl unterstützt. Wie sollte ich vorgehen?

Informationen hierzu finden Sie unter [Best Practices für Entitäten](luis-concept-entity-types.md#if-you-need-more-than-the-maximum-number-of-entities).

### <a name="what-are-the-limits-on-the-number-and-size-of-phrase-lists"></a>Welche Grenzwerte gelten für die Anzahl und den Umfang von Begriffslisten?
Informationen zur maximalen Länge einer [Begriffsliste](./luis-concept-feature.md) finden Sie in der Referenzdokumentation zu [Begrenzungen](luis-boundaries.md).

### <a name="what-are-the-limits-on-example-utterances"></a>Welche Grenzwerte gelten für Beispieläußerungen?
Informationen hierzu finden Sie in der Referenzdokumentation zu [Begrenzungen](luis-boundaries.md).

## <a name="testing-and-training"></a>Testen und Trainieren von LUIS

### <a name="i-see-some-errors-in-the-batch-testing-pane-for-some-of-the-models-in-my-app-how-can-i-address-this-problem"></a>In meinem Batchtestbereich werden mehrere Fehler für Modelle in meiner App angezeigt. Wie kann ich dieses Problem beheben?

Diese Fehler deuten darauf hin, dass eine Diskrepanz zwischen den Bezeichnungen und den Modellvorhersagen besteht. Führen Sie einen oder beide Schritte aus, um das Problem zu beheben:
* Fügen Sie weitere Bezeichnungen hinzu, damit LUIS Absichten besser unterscheiden kann.
* Fügen Sie Begriffslistenfeatures mit Fachwortschatz hinzu, damit LUIS schneller lernt.

Informationen hierzu finden Sie im Tutorial [Batchtests](luis-tutorial-batch-testing.md).

### <a name="when-an-app-is-exported-then-reimported-into-a-new-app-with-a-new-app-id-the-luis-prediction-scores-are-different-why-does-this-happen"></a>Wenn eine App exportiert und anschließend in eine neue App (mit einer neuen App-ID) reimportiert wird, unterscheiden sich die LUIS-Vorhersagebewertungen. Was ist dafür die Ursache? 

Informationen hierzu finden Sie unter [Vorhersageunterschiede zwischen Kopien derselben App](luis-concept-prediction-score.md#differences-with-predictions).

## <a name="app-publishing"></a>Veröffentlichen von Apps

### <a name="what-is-the-tenant-id-in-the-add-a-key-to-your-app-window"></a>Was ist die Mandanten-ID im Fenster „Add a key to your app“ (Schlüssel zur App hinzufügen)?
In Azure stellen Mandanten Clients oder Organisationen dar, die einem Dienst zugeordnet sind. Ihre Mandanten-ID finden Sie im Azure-Portal im Feld **Verzeichnis-ID**. Dieses rufen Sie über **Azure Active Directory** > **Verwalten** > **Eigenschaften** auf.

![Mandanten-ID im Azure-Portal](./media/luis-manage-keys/luis-assign-key-tenant-id.png)

### <a name="why-are-there-more-subscription-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>Warum befinden sich auf der Veröffentlichungsseite meiner App mehr Abonnementschlüssel, als ich der App zugewiesen habe? 
Jede LUIS-App verfügt über einen Erstellungsschlüssel (auch als Startschlüssel bezeichnet). LUIS-Abonnementschlüssel, die innerhalb des GA-Zeitrahmens (General Availability, allgemeine Verfügbarkeit) erstellt wurden, werden immer auf Ihrer Veröffentlichungsseite angezeigt, und zwar unabhängig davon, ob Sie diese der App hinzugefügt haben. Dadurch soll die GA-Migration erleichtert werden. Neue LUIS-Abonnementschlüssel werden nicht auf der Veröffentlichungsseite angezeigt. 

## <a name="app-management"></a>App-Verwaltung

### <a name="how-do-i-transfer-ownership-of-a-luis-app"></a>Wie übertrage ich eine LUIS-App an einen anderen Besitzer?
Wenn Sie eine LUIS-App an ein anderes Azure-Abonnement übertragen möchten, müssen Sie die App exportieren und mit einem neuen Konto importieren. Aktualisieren Sie die LUIS-App-ID in der Clientanwendung, die die ID aufruft. Die neue App gibt möglicherweise LUIS-Bewertungen zurück, die sich geringfügig von den Werten der ursprünglichen App unterscheiden. 

### <a name="how-do-i-download-a-log-of-user-utterances"></a>Wie lade ich ein Protokoll mit Benutzeräußerungen herunter?
Ihrer LUIS-App protokolliert standardmäßig Äußerungen von Benutzern. Wechseln Sie zu **Meine Apps**, und klicken Sie auf die Auslassungspunkte (***...*** ) in der Liste für Ihre App, um ein Protokoll mit Äußerungen herunterzuladen, die Benutzer an Ihre LUIS-App senden. Klicken Sie anschließend auf **Export Endpoint Logs** (Exportieren von Endpunktprotokollen). Das Protokoll wird als durch Trennzeichen getrennte Datei (CSV) gespeichert.

### <a name="how-can-i-disable-the-logging-of-utterances"></a>Wie kann ich die Protokollierung von Äußerungen deaktivieren?
Sie können die Protokollierung von Benutzeräußerungen deaktivieren, indem Sie in der Endpunkt-URL, die Ihre Clientanwendung zur Abfrage von LUIS verwendet, `log=false` festlegen. Durch das Deaktivieren der Protokollierung kann die LUIS-App jedoch nicht mehr Äußerungen vorschlagen oder die auf dem [aktiven Lernen](luis-concept-review-endpoint-utterances.md#what-is-active-learning) basierende Leistung verbessern. Wenn Sie `log=false` aus Datenschutzgründen festlegen, können Sie keinen Datensatz mit diesen Benutzeräußerungen von LUIS herunterladen oder diese Äußerungen zur Verbesserung Ihrer App verwenden.

Äußerungen werden ausschließlich über die Protokollierungsfunktion gespeichert. 

### <a name="why-dont-i-want-all-my-endpoint-utterances-logged"></a>In welchem Fall sollten nicht alle Endpunktäußerungen protokolliert werden?
Sie sollten keine Testäußerungen protokollieren, wenn Sie das Protokoll für die Vorhersageanalyse verwenden.

## <a name="data-management"></a>Datenverwaltung

### <a name="can-i-delete-data-from-luis"></a>Kann ich Daten aus LUIS löschen? 

* Sie können Beispieläußerungen für das Trainieren von LUIS jederzeit löschen. Wenn Sie eine Beispieläußerung aus Ihrer LUIS-App löschen, wird sie vom LUIS-Webdienst entfernt und ist nicht für den Export verfügbar.
* Sie können Äußerungen aus der Liste der Benutzeräußerungen löschen, die LUIS auf der Seite **Review endpoint utterance** (Endpunktäußerungen überprüfen) vorschlägt. Das Löschen von Äußerungen aus dieser Liste verhindert, dass diese empfohlen werden. Sie werden aber nicht aus den Protokollen gelöscht.
* Wenn Sie ein Konto löschen, werden alle Apps zusammen mit ihren Beispieläußerungen und Protokollen gelöscht. Die Daten werden 60 Tage lang auf den Servern gespeichert und erst danach dauerhaft gelöscht.

## <a name="language-and-translation-support"></a>Sprach- und Übersetzungsunterstützung 

### <a name="i-have-an-app-in-one-language-and-want-to-create-a-parallel-app-in-another-language-what-is-the-easiest-way-to-do-so"></a>Ich habe eine App in einer bestimmten Sprache erstellt und möchte nun dieselbe App in einer anderen Sprache erstellen. Wie geht das am einfachsten?
1. Exportieren Sie zunächst Ihre App,
2. und übersetzen Sie anschließend die in der JSON-Datei der exportieren App vorhandenen Äußerungen mit Bezeichnungen in die Zielsprache.
3. Möglicherweise müssen Sie die Namen der Absichten und Entitäten ändern.
4. Wenn Sie die App abschließend importieren, steht Ihnen eine LUIS-App in der Zielsprache zur Verfügung.

## <a name="app-notification"></a>App-Benachrichtigungen

### <a name="why-did-i-get-an-email-saying-im-almost-out-of-quota"></a>Wieso habe ich eine E-Mail erhalten, in der darauf hingewiesen wird, dass mein Kontingent bald aufgebraucht ist?
Ihr Erstellungsschlüssel (auch als Startschlüssel bezeichnet) kann für maximal 1000 Endpunktabfragen pro Monat verwendet werden. Erstellen Sie einen kostenlosen oder kostenpflichtigen LUIS-Abonnementschlüssel, und verwenden Sie diesen für Endpunktabfragen. Wenn Sie Endpunktabfragen über einen Bot oder eine andere Clientanwendung ausführen, müssen Sie den LUIS-Endpunktschlüssel dort ändern. 

## <a name="integrating-luis"></a>Integrieren von LUIS

### <a name="where-is-my-luis-app-created-during-the-azure-web-app-bot-subscription-process"></a>Wo wird meine LUIS-App bei der Abonnementerstellung durch den Azure-Web-App-Bot erstellt?
Wenn Sie eine LUIS-Vorlage auswählen und im Vorlagenbereich auf **Select** (Auswählen) klicken, wird im linken Bereich der Vorlagentyp angezeigt, und Sie werden dazu aufgefordert, eine Region für die Erstellung der LUIS-Vorlage anzugeben. Der Web-App-Bot erstellt jedoch kein LUIS-Abonnement.

![Web-App-Bot-Region für LUIS-Vorlage](./media/luis-faq/web-app-bot-location.png)

### <a name="what-luis-regions-support-bot-framework-speech-priming"></a>Welche LUIS-Regionen unterstützen die Bot Framework-Sprachoptimierung?
Die [Sprachoptimierung ](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) wird nur von LUIS-Apps in der Instanz für die mittlere USA unterstützt. 

## <a name="luis-service"></a>LUIS-Dienst 

### <a name="is-luis-available-on-premise-or-in-private-cloud"></a>Kann LUIS lokal oder in einer privaten Cloud genutzt werden?
Nein. 

## <a name="changes-to-the-docs"></a>Dokumentationsänderungen

### <a name="where-did-the-tutorials-go"></a>Wohin wurden die Tutorials verschoben? 
Die Artikel, die sich zuvor im Tutorialabschnitt befanden, sind nun im Abschnitt „Vorgehensweise“ der Dokumentation. 

|Tutorial|
|--|
|Integrieren von LUIS in einen Bot über [C#](luis-csharp-tutorial-build-bot-framework-sample.md) und [Node.js](luis-nodejs-tutorial-build-bot-framework-sample.md)|
|Hinzufügen von Application Insights zu einem Bot über [C#](luis-tutorial-bot-csharp-appinsights.md) und [Node.js](luis-tutorial-function-appinsights.md)|
|Programmgesteuertes Erstellen einer LUIS-App über [Node.js](luis-tutorial-node-import-utterances-csv.md)|
|Verwenden einer [zusammengesetzten Entität](luis-tutorial-composite-entity.md) zum Extrahieren von gruppierten Daten|
|Hinzufügen einer [Listenentität](luis-tutorial-list-entity.md) zur verbesserten Entitätserkennung mithilfe von Node.js|
|Verbessern der Vorhersagegenauigkeit mit [Begriffslisten](luis-tutorial-interchangeable-phrase-list.md), [Mustern](luis-tutorial-pattern.md) und [Batchtests](luis-tutorial-batch-testing.md)|
|[Verbessern von Rechtschreibfehlern](luis-tutorial-batch-testing.md) mit der Bing-Rechtschreibprüfungs-API v7

### <a name="at-the-build-2018-conference-i-heard-about-a-language-understanding-feature-or-demo-but-i-dont-remember-what-it-was-called"></a>Bei der Build 2018-Konferenz wurde ein Language Understanding-Feature oder eine Language Understanding-Demo erwähnt, aber ich erinnere mich nicht mehr an den Namen. Um welche Features oder Demos handelte es sich? 

Die folgenden Features wurden bei der Build 2018-Konferenz veröffentlicht:

|NAME|Inhalt|
|--|--|
|Verbesserungen|Entitäten für [reguläre Ausdrücke](luis-concept-data-extraction.md##regular-expression-entity-data) und [Schlüsselbegriffe](luis-concept-data-extraction.md#key-phrase-extraction-entity-data)
|Muster|[Musterkonzept](luis-concept-patterns.md), [Tutorial](luis-tutorial-pattern.md), [Vorgehensweise](luis-how-to-model-intent-pattern.md)<br>[Patterns.Any](luis-concept-entity-types.md)-Entitätskonzept einschließlich [expliziter Listen](luis-concept-patterns.md#explicit-lists) für Ausnahmen<br>[Rollenkonzept](luis-concept-roles.md)|
|Integrationen|Integration der [Standpunktanalyse](publishapp.md#enable-sentiment-analysis) in die [Textanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/)<br>Integration der [Sprachoptimierung](publishapp.md#enable-speech-priming) in Verbindung mit dem [Speech SDK](https://aka.ms/SpeechSDK) in [Speech](https://docs.microsoft.com/azure/cognitive-services/speech)|
|Dispatch-Tool|Das [Dispatch-Befehlszeilentool](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps) ist Teil der [Bot Builder-Tools](https://github.com/Microsoft/botbuilder-tools) und dient dazu, mehrere LUIS- und QnA Maker-Apps in einer einzelnen LUIS-App zu vereinen. Dadurch kann die Absichtserkennung eines Bots verbessert werden.

Zusätzliche [API-Erstellungsrouten](https://github.com/Microsoft/LUIS-Samples/blob/master/authoring-routes.md) wurden hinzugefügt. 

Videos: 
* [Azure Friday At Build 2018: Cognitive Services - Language (LUIS) (Azure Friday bei der Build 2018-Konferenz: Cognitive Services – Language Understanding (LUIS))](https://channel9.msdn.com/Shows/Azure-Friday/At-Build-2018-Cognitive-Services-Language-LUIS/player)
* [Build 2018 AI Show - What’s New with Language Understanding Service (Build 2018 AI Show: Neuerungen bei Language Understanding Service (LUIS))](https://channel9.msdn.com/Shows/AI-Show/Whats-New-with-Language-Understanding-Service-LUIS/player)
* [Build 2018 Session – Bot Intelligence, Speech Capabilities, and NLU Best Practices (Build 2018 Session – Bot-Intelligence, Sprachfunktionen und bewährte Methoden für NLU)](https://channel9.msdn.com/events/Build/2018/BRK3208)
* [Build 2018 - LUIS Updates (Build 2018: Neue LUIS-Features)](https://channel9.msdn.com/events/Build/2018/THR3118/player)

Projekte: 
* Demo zum [Contoso Cafe Bot](https://github.com/botbuilderbuild2018/build2018demo) (Quellcode auf Github)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu LUIS finden Sie in den folgenden Ressourcen:
* [Mit dem Tag „LUIS“ versehene Fragen auf Stack Overflow](https://stackoverflow.com/questions/tagged/luis)
* [MSDN-Forum für Language Understanding Intelligent Services (LUIS)](https://social.msdn.microsoft.com/forums/azure/home?forum=LUIS) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
