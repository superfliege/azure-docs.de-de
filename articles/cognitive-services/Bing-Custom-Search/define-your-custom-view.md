---
title: Definieren einer benutzerdefinierten Ansicht – Benutzerdefinierte Bing-Suche
titlesuffix: Azure Cognitive Services
description: Enthält Informationen zum Erstellen einer Website und vertikaler Suchdienste
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: a74e6a6a90a242d4d1b2fd71a5fc6cf949ea55cb
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815255"
---
# <a name="configure-your-custom-search-experience"></a>Konfigurieren der Benutzeroberfläche für die benutzerdefinierte Suche

Mit einer benutzerdefinierten Suchinstanz können Sie die Benutzeroberfläche für die Suche so anpassen, dass nur Inhalte von Websites einbezogen werden, die Ihre Benutzer interessieren. Anstatt eine webweite Suche auszuführen, durchsucht Bing nur das Segment des Webs, das Sie interessiert. Um Ihre benutzerdefinierte Ansicht des Webs zu erstellen, verwenden Sie das [Portal](https://customsearch.ai) für die benutzerdefinierte Bing-Suche. Informationen zum Anmelden am Portal finden Sie unter [Erstellen Ihrer ersten benutzerdefinierten Bing-Suchinstanz](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/quick-start). 

Im Portal können Sie eine Suchinstanz erstellen, die die Domänen, Unterseiten und Webseiten angibt, die Bing durchsuchen soll, sowie diejenigen, die nicht durchsucht werden sollen. Zusätzlich zur Angabe der URLs der Inhalte, die Ihnen bekannt sind, können Sie vom Portal auch Vorschläge zu Inhalten erhalten, die Sie Ihrer Ansicht ggf. hinzufügen möchten. 

Im Folgenden werden die Möglichkeiten beschrieben, wie Sie ein Segment des Webs definieren können: 

1.  Domäne: Ein Domänensegment umfasst alle Inhalte, die innerhalb einer Internetdomäne gefunden werden. Beispielsweise www.microsoft.com. Das Auslassen von „www“ bewirkt, dass Bing auch die Unterdomänen der Domäne durchsucht. Wenn Sie beispielsweise microsoft.com angeben, gibt Bing auch Ergebnisse aus support.microsoft.com oder technet.microsoft.com zurück.  
  
2.  Unterseite: Ein Unterseitensegment umfasst sämtliche Inhalte, die auf der Unterseite und in den Pfaden darunter gefunden werden. Sie können maximal zwei Unterseiten im Pfad angeben. Beispielsweise www.microsoft.com/en-us/windows/  
  
3.  Webseite: Ein Webseitensegment kann nur diese Webseite in eine benutzerdefinierten Suche einschließen. Sie können optional angeben, ob Unterseiten enthalten sein sollen.

Alle Domänen, Unterseiten und Webseiten, die Sie angeben, müssen öffentlich und von Bing indiziert sein. Wenn Sie der Besitzer einer öffentlichen Website sind, die in der Suche enthalten sein soll, und Bing diese noch nicht indiziert hat, finden Sie in der Bing-[Webmasterdokumentation](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) Details dazu, wie Bing die Website indizieren kann. Außerdem finden Sie in der Webmasterdokumentation Details dazu, wie Bing die durchforstete Site aktualisieren kann, wenn der Index nicht mehr aktuell ist.

## <a name="adding-slices-to-your-custom-search"></a>Hinzufügen von Segmenten zu Ihrer benutzerdefinierten Suche

Wenn Sie Ihre benutzerdefinierte Suchinstanz definieren, geben Sie aktive und blockierte Domänen, Unterseiten und Webseiten an, die Sie durchsuchen oder nicht durchsuchen möchten.  

- Aktiv: Eine Liste der Domänen, Unterseiten oder Webseiten, die in die Suche eingeschlossen werden sollen.  
  
- Blockiert: Eine Liste der Domänen, Unterseiten oder Webseiten, die aus der Suche ausgeschlossen werden sollen. Die Elemente, die Sie blockieren, sollten Inhalte sein, die unter den Domänen und Unterseiten gefunden werden, die in Ihrer Liste „Aktiv“ aufgeführt werden.

Um auf jede der Listen zuzugreifen, klicken Sie in Ihrer benutzerdefinierten Suchinstanz auf die Registerkarten „Aktiv“ und „Blockiert“. 

<a name="active-and-blocked-lists"></a>
## <a name="active-and-blocked-lists"></a>Listen „Aktiv“ und „Blockiert“ 

Um ein Segment des Webs anzugeben, das Bing durchsuchen soll, klicken Sie auf die Registerkarte **Aktiv** und listen die Domänen, Unterseiten und Webseiten auf, die durchsucht werden sollen. Sie können der Liste ein Segment direkt hinzufügen oder mehrere Segmente hinzufügen, indem Sie eine Textdatei mit dem Uploadsymbol hochladen.

Dateiuploaddetails: 

- Der Dateiupload ist nur für das Hinzufügen von Segmenten zur Liste „Aktiv“ verfügbar. Sie können ihn nicht verwenden, um der Liste „Blockiert“ Segmente hinzuzufügen.  
  
- Erstellen Sie eine Textdatei, und geben Sie eine einzelne Domäne, Unterseite oder Webseite pro Zeile an. Der gesamte Upload wird abgelehnt, wenn ein Fehler auftritt.  
  
- Wenn die Liste „Blockiert“ die Domäne, Unterseite oder Webseite enthält, die Sie in der Uploaddatei angegeben haben, entfernt der Dienst sie aus der Liste „Blockiert“ und fügt sie der Liste „Aktiv“ hinzu.  
  
- Der Dienst ignoriert alle Duplikate in der Uploaddatei.

Verwenden Sie zum Bearbeiten oder Löschen von Segmenten die Optionen unter der Spalte **Steuerelemente**. 

Auf ähnliche Weise können Sie der Liste „Blockiert“ Segmente hinzufügen (Sie können nur keine Uploaddatei zum Angeben der Segmente verwenden).

## <a name="pinned-list"></a>Angeheftete Liste

Im Portal können auch eine bestimmte Webseite am Anfang des Suchergebnisses anheften, wenn der Benutzer einen bestimmten Suchbegriff eingibt. Die Registerkarte **Angeheftet** enthält eine Liste mit Abfragebegriff- und Webseitenpaaren, die die Webseite angeben, die als oberstes Ergebnis für eine bestimmte Abfrage angezeigt wird. Weitere Informationen zum Anheften von Ergebnissen finden Sie unter [Anpassen der Rangfolge](#adjustrank).

Das Anheften von Ergebnissen ist nicht für die Bildersuche und die Videosuche verfügbar.

## <a name="website-suggestions"></a>Websitevorschläge

Nachdem Sie der Liste „Aktiv“ Segmente hinzugefügt haben, generiert der Dienst Website- und Unterseitenvorschläge, die Sie der Suche möglicherweise hinzufügen möchten. Der Abschnitt **Vorschläge zum Hinzufügen** enthält diese Vorschläge. Die Instanzeinstellungsseite enthält diesen Abschnitt nur, wenn Vorschläge zur Verfügung stehen. 

Um Vorschläge zur Liste „Aktiv“ hinzuzufügen, klicken Sie auf das Symbol „+“.  Da der Dienst Vorschläge basierend auf Ihren Einstellungen generiert, müssen Sie nach dem Hinzufügen der Vorschläge auf **Aktualisieren** klicken. 

## <a name="preview-pane"></a>Vorschaubereich

Sie können Ihre Suchinstanz im Vorschaubereich auf der rechten Seite testen, um Suchanfragen zu übermitteln und Ergebnisse anzuzeigen. 

1. Auswählen von **Eigene Instanz**
2. Wählen Sie einen sicheren Suchfilter und den zu durchsuchenden Markt aus (siehe [Abfrageparameter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters)).
3. Geben Sie eine Abfrage ein, und drücken Sie die EINGABETASTE, oder klicken Sie auf das Suchsymbol, um die Ergebnisse aus der aktuellen Konfiguration anzuzeigen. 

Um den Typ der auszuführenden Suche auszuwählen, klicken Sie auf **Web**, wenn Sie Webergebnisse abrufen möchten, auf **Bild**, wenn Sie die Ergebnisse der Bildersuche abrufen möchten, oder auf **Video**, wenn Sie Videoergebnisse abrufen möchten. 

Mit dem Vorschaubereich können Sie auch Bing-Ergebnisse überprüfen, indem Sie **Bing** anstelle von **Eigene Instanz** auswählen. Dies kann nützlich sein, um die Ergebnisse aus Ihrer Benutzeroberfläche für die Suche mit den von Bing zurückgegebenen Ergebnissen zu vergleichen.

<a name="adjustrank"></a>
## <a name="adjust-rank"></a>Anpassen der Rangfolge

Im Portal können Sie die Rangfolge anpassen, um die Ergebnisse zu bearbeiten, die Bing zurückgibt. Geben Sie im Vorschaubereich einen Suchbegriff ein, und führen Sie die Abfrage aus. Im Vorschaubereich wird eine Liste mit dem Suchergebnis für die Abfrage angezeigt. Rechts von jedem Ergebnis befindet sich die Liste der Anpassungen, die Sie vornehmen können. 

- Blockierung. Verschiebt die Domäne, Unterseite oder Webseite in die Liste „Blockiert“. Sie wählen die zu blockierende Ebene aus. Bing schließt Inhalte aus der ausgewählten Website aus den Suchergebnissen aus.  
  
- Verstärken. Zeigt Inhalte aus der Domäne oder Unterseite weiter oben in den Suchergebnissen an. Sie wählen aus, ob Inhalt aus der Domäne oder Unterseite, zu der die Webseite gehört, verstärkt werden soll. [Weitere Informationen finden Sie hier.](#boosting-and-demoting)  
  
- Tiefer stufen. Stuft Inhalte aus der Domäne oder Unterseite tiefer und zeigt sie weiter unten in den Suchergebnissen an. Sie wählen aus, ob Inhalt aus der Domäne oder Unterseite, zu der die Webseite gehört, tiefer gestuft werden soll. [Weitere Informationen](#boosting-and-demoting).  
  
- Oben anheften. Definieren Sie die Webseite, die in den Ergebnissen ganz oben angezeigt wird, wenn die Abfragezeichenfolge des Benutzers der angehefteten Abfragezeichenfolge den zugehörigen Übereinstimmungsbedingung entspricht. Die Liste „Aktiv“ muss die Webseite nicht enthalten, damit Sie sie anheften können. [Weitere Informationen](#pin-to-top).

Das Anpassen der Rangfolge ist nicht für die Bildersuche und die Videosuche verfügbar.

## <a name="boosting-and-demoting"></a>Verstärken und tiefer stufen

Sie können jede Domäne oder Unterseite in Ihrer Liste „Aktiv“ extrem verstärken, verstärken oder tiefer stufen. Standardmäßig werden alle Segmente mit der gleichen Gewichtung hinzugefügt. Elemente, die extrem verstärkt oder verstärkt werden, erhalten in den Suchergebnissen einen höheren Rang (dabei besitzt „extrem verstärkt“ eine höhere Rangfolge als „verstärkt“). Elemente, die tiefer gestuft werden, erhalten in den Suchergebnissen eine niedrigere Rangfolge.

Es ist wichtig zu beachten, dass die Domänen oder Unterseiten durch extreme Verstärkung, Verstärkung und Tieferstufung entsprechende Gewichtungsvarianten erhalten. Dies ist nur eines von den vielen Signalen, die das Rangfolgemodul verwendet, um die Reihenfolge der Ergebnisse zu bestimmen. Dies bedeutet, dass ihre Auswirkungen für eine bestimmte Abfrage nicht garantiert sind, weil zahlreiche andere Faktoren die Gesamtrangfolge der Webergebnisse beeinflussen könnten.  Um die möglichen Auswirkungen zu bestimmen, die eine Verstärkung oder Tieferstufung auf das Rangfolgemodul besitzt, testen Sie die Benutzeroberfläche für die Suche im Vorschaubereich.

Sie können Elemente extrem verstärken, verstärken oder tiefer stufen, indem Sie die Rangfolgeanpassungs-Steuerelemente in der Liste „Aktiv“ oder die Verstärkungs- und Tieferstufungssteuerelemente im Vorschaubereich verwenden. Der Dienst fügt das Segment der Liste „Aktiv“ hinzu und passt die Rangfolge entsprechend an.

Extrem Verstärken, Verstärken und Tieferstufen sind nicht für die Bildersuche und die Videosuche verfügbar.

## <a name="pin-to-top"></a>Oben anheften

Um eine Webseite am Anfang der Suchergebnisse für eine bestimmte Abfrage anzuheften, wählen Sie eine der folgenden Optionen aus:

1.  Geben Sie auf der Registerkarte  **Angeheftet**  die URL der Webseite an, die am Anfang der Ergebnisse angeheftet werden soll, sowie die Abfrage, die das Anheften auslöst.  
  
2.  Geben Sie im Bereich **Vorschau** einen Abfragebegriff ein, und klicken Sie auf „Suchen“. Identifizieren Sie anschließend die Webseite in den Ergebnissen, die Sie am Anfang der Ergebnisse anheften möchten, wenn der Benutzer die gleiche Abfrage eingibt. Klicken Sie dann auf  **Oben anheften**. Der Dienst fügt die Webseite und die Abfrage der Liste **Angeheftet** hinzu. 

Sie können Ihre Anheftungen auf der Registerkarte  **Angeheftet**  nachverfolgen. Die Anheftungen werden als Paare aus „\<Abfrage\>“ und „\<Webseite\>“ angezeigt. 

Für eine bestimmte Abfrage können Sie maximal eine Webseite am Anfang der Ergebnisse anheften.

Das Anheften ist nicht für die Bildersuche und die Videosuche verfügbar.

### <a name="specifying-the-pins-match-condition"></a>Angeben der Übereinstimmungsbedingung für das Anheften

Die Webseite wird nur dann am Anfang der Ergebnisse angeheftet, wenn die Abfragezeichenfolge des Benutzers basierend auf der Übereinstimmungsbedingung für das Anheften mit der Abfragezeichenfolge zum Anheften übereinstimmt. Standardmäßig muss die Abfragezeichenfolge für das Anheften genau mit der Abfragezeichenfolge des Benutzers übereinstimmen. Sie können das Standardverhalten ändern, indem Sie eine der folgenden Übereinstimmungsbedingungen angeben.

- Beginnt mit &mdash; zum Anheften muss die Abfragezeichenfolge des Benutzers mit der Abfragezeichenfolge zum Anheften beginnen.
- Endet mit &mdash; zum Anheften muss die Abfragezeichenfolge des Benutzers auf die Abfragezeichenfolge zum Anheften enden.
- Enthält &mdash; zum Anheften muss die Abfragezeichenfolge des Benutzers die Abfragezeichenfolge zum Anheften enthalten.

Um die Übereinstimmungsbedingung zum Anheften zu ändern, klicken Sie auf das Symbol zum Bearbeiten (Bleistift). Klicken Sie in der Spalte **Query match condition** (Übereinstimmungsbedingung für Abfrage) auf die Dropdownliste, und wählen Sie die neue Bedingung aus. Klicken Sie dann auf das Symbol zum Speichern, um die Änderung zu speichern.

Beim allen Vergleichen wird die Groß-/Kleinschreibung nicht beachtet.

### <a name="changing-the-order-of-the-pins"></a>Ändern der Reihenfolge beim Anheften

Um die Reihenfolge beim Anheften zu ändern, können Sie die Einträge ziehen und ablegen oder einen Eintrag bearbeiten und dabei die Reihenfolgenummer ändern. Zum Ändern der Reihenfolge per Drag & Drop klicken Sie auf einen angehefteten Eintrag, halten die Maustaste gedrückt und ziehen den Eintrag an die gewünschte Position. Lassen Sie anschließend die Maustaste los. Beachten Sie, dass auch die Reihenfolgenummer entsprechend geändert wird.

Sie können auch die Reihenfolgenummer bearbeiten. Klicken Sie in der Spalte **Steuerelemente** auf das Symbol zum Bearbeiten (Bleistift). Geben Sie die neue Reihenfolgenummer in der Liste ein, und drücken Sie die EINGABETASTE, oder klicken Sie auf das Symbol „Speichern“. Wenn ein Eintrag derzeit an der sechsten Position in der Liste angezeigt wird und Sie ihn an die zweite Position verschieben möchten, ändern Sie die Reihenfolgenummer in „2“.

Wenn mehrere Einträge die Übereinstimmungsbedingung erfüllen, bestimmt die Reihenfolge, welcher von Bing verwendet wird. Wenn beispielsweise die Einträge 2 und 3 der Übereinstimmungsbedingung entsprechen, verwendet Bing den Eintrag mit der Nummer 2.

## <a name="use-bing-to-specify-slices"></a>Verwenden von Bing zum Angeben von Segmenten

Es gibt mehrere Möglichkeiten, die Segmente des Webs anzugeben, aus denen Ihre benutzerdefinierte Suche besteht. Wenn Sie die Segmente kennen, die in Ihrer Instanz enthalten sein sollen, fügen Sie diese der Liste **Aktiv** Ihrer Instanz hinzu. Informationen dazu, wie Sie selbst der Liste **Aktiv** Elemente hinzufügen können, finden Sie unter [Listen „Aktiv“ und „Blockiert“](#active-and-blocked-lists).

Wenn Sie jedoch nicht sicher sind, welche Segmente enthalten sein sollten, können Sie im Bereich **Vorschau** Bing-Abfragen ausführen und untersuchen, welche Ergebnisse Bing zurückgibt. Sie können dann die Segmente auswählen, die Sie in die benutzerdefinierte Suche einbeziehen möchten. Sie müssen wahrscheinlich mehrere Abfrageausdrücke ausführen, um sicherzustellen, dass Sie alle Segmente identifizieren, die Sie für Ihre Instanz verwenden möchten. 

Führen Sie die folgenden Schritte aus, um Bing zum Hinzufügen von Segmenten zu Ihrer benutzerdefinierten Suchinstanz zu verwenden. 

1.  Melden Sie sich am [Portal](https://customsearch.ai) für die benutzerdefinierte Bing-Suche an.
2.  Erstellen Sie eine Instanz, oder wählen Sie eine zu aktualisierende Instanz aus.
3.  Wählen Sie Bing im Vorschaubereich auf der rechten Seite aus der Dropdownliste aus.
4.  Geben Sie in das Suchfeld einen Abfrageausdruck ein, der für Ihre Instanz relevant ist.
5.  Klicken Sie neben dem Ergebnis, das enthalten sein soll, auf **Website hinzufügen**.
6.  Klicken Sie auf die Schaltfläche **OK** .

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="view-statistics"></a>Anzeigen von Statistiken

Wenn Sie die benutzerdefinierte Suche auf der entsprechenden Ebene abonniert haben (siehe [Preisseiten](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), wird Ihren Produktionsinstanzen eine Registerkarte **Statistiken** hinzugefügt. Die Registerkarte „Statistiken“ zeigt Details zur Verwendung Ihrer benutzerdefinierten Suchendpunkte einschließlich Aufrufaufkommen, häufigste Abfragen, geografische Verteilung, Antwortcodes und sichere Suche an. Sie können die Details mit den bereitgestellten Steuerelementen filtern.

## <a name="understanding-quota"></a>Grundlegendes zum Kontingent

- Für jede benutzerdefinierte Suchinstanz ist die maximale Anzahl von Rangfolgeanpassungen, die Sie an den Segmenten **Aktiv** und **Blockiert** vornehmen können, auf 400 beschränkt.
- Das Hinzufügen eines Segments zu den Registerkarten „Aktiv“ oder „Blockiert“ wird als eine Rangfolgeanpassung gezählt.
- Die Verstärkung und Tieferstufung wird als zwei Rangfolgeanpassungen gezählt.
- Für jede benutzerdefinierte Suchinstanz ist die maximale Anzahl von Anheftungen, die Sie vornehmen können, auf 200 beschränkt.

## <a name="next-steps"></a>Nächste Schritte

- [Aufrufen der benutzerdefinierten Suche](./search-your-custom-view.md)
- [Konfigurieren der gehosteten Benutzeroberfläche](./hosted-ui.md)
- [Verwenden von Decorator-Markierungen zum Hervorheben von Text](./hit-highlighting.md)
- [Einteilen von Webseiten](./page-webpages.md)