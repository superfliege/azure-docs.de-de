---
title: 'Schnellstart: Erstellen einer Pipeline für die kognitive Suche in Azure Search mithilfe des Portals | Microsoft Docs'
description: Beispiel mit Qualifikationen für die Extrahierung von Daten, natürliche Sprache und Bildverarbeitung im Azure-Portal anhand von Beispieldaten.
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.topic: quickstart
ms.date: 05/01/2018
ms.author: heidist
ms.openlocfilehash: c5b6036efa4fcd6b7b8d756fa20d63952482518e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640260"
---
# <a name="quickstart-create-a-cognitive-search-pipeline-using-skills-and-sample-data"></a>Schnellstart: Erstellen einer Pipeline für die kognitive Suche mithilfe von Qualifikationen und Beispieldaten

Die kognitive Suche (Vorschau) fügt einer Indizierungspipeline von Azure Search Qualifikationen zur Extrahierung von Daten, Verarbeitung von natürlicher Sprache (Natural Language Processing, NLP) und Bildverarbeitung hinzu und macht nicht durchsuchbare oder unstrukturierte Inhalte besser durchsuchbar. Von einer Qualifikation erstellte Informationen, wie etwa Entitätserkennung oder Bildanalyse, werden einem Index in Azure Search hinzugefügt.

Probieren Sie in diesem Schnellstart die Anreicherungspipeline im [Azure-Portal](https://portal.azure.com) aus, bevor Sie auch nur eine Zeile Code schreiben:

* Beginnen Sie mit Beispieldaten in Azure Blob Storage
* Konfigurieren Sie den [Datenimport-Assistenten](search-import-data-portal.md) für Indizierung und Anreicherung 
* Führen Sie den Assistenten aus (eine Entitäts-Qualifikation erkennt Personen, Orte und Organisationen)
* Verwenden Sie den [Suchexplorer](search-explorer.md), um die angereicherten Daten abzufragen.

Sie können die kognitive Suche in einem Azure Search-Dienst ausprobieren, der in den folgenden Regionen erstellt wurde:

* USA Süd Mitte
* Europa, Westen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

[„What is cognitive search?“](cognitive-search-concept-intro.md) (Was ist kognitive Suche?) stellt die Architektur und die Komponenten für die Anreicherung vor. 

In diesem Szenario werden ausschließlich Azure-Dienste verwendet. Das Erstellen der benötigten Dienste ist Teil der Vorbereitung.

+ Azure Blob Storage stellt die Quelldaten bereit.
+ Azure Search verarbeitet die Datenerfassung und -indizierung, die Anreicherung der kognitiven Suche und die Abfragen für die Volltextsuche.

### <a name="set-up-azure-search"></a>Einrichten von Azure Search

Registrieren Sie sich zunächst beim Azure Search-Dienst. 

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

1. Klicken Sie auf **Ressource erstellen**, suchen Sie nach Azure Search, und klicken Sie auf **Erstellen**. Wenn Sie zum ersten Mal einen Suchdienst einrichten und dazu weitere Hilfe benötigen, lesen Sie [Erstellen eines Azure Search-Diensts im Portal](search-create-service-portal.md).

  ![Dashboard-Portal](./media/cognitive-search-tutorial-blob/create-service-full-portal.png "Erstellen eines Azure Search-Diensts im Portal")

1. Erstellen Sie unter „Ressourcengruppe“ eine Ressourcengruppe zur Aufnahme aller Ressourcen, die Sie im Rahmen dieses Schnellstarts erstellen. Das macht Ihnen das Bereinigen der Ressourcen nach dem Abschluss des Schnellstarts leichter.

1. Wählen Sie als Standortort entweder **USA, Süden-Mitte** oder **Europa, Westen** aus. Aktuell ist die Vorschau nur in diesen Regionen verfügbar.

1. Für die Preisstufe können Sie einen **kostenlosen** Dienst erstellen, um Tutorials und Schnellstarts zu absolvieren. Wenn Sie eine gründlichere Untersuchung unter Verwendung Ihrer eigenen Daten wünschen, erstellen Sie einen [kostenpflichtigen Dienst](https://azure.microsoft.com/pricing/details/search/), wie etwa **Basic** oder **Standard**. 

  Ein kostenloser Dienst ist auf 3 Indizes, eine maximale Blobgröße von 16 MB und Indizierungsvorgänge von 2 Minuten Dauer beschränkt, was zum vollständigen Nutzen der Möglichkeiten der kognitiven Suche unzureichend ist. Informationen zum Überprüfen der Grenzwerte für verschiedene Stufen finden Sie unter [Diensteinschränkungen](search-limits-quotas-capacity.md).

  > [!NOTE]
  > Die kognitive Suche befindet sich derzeit in der öffentlichen Vorschauphase. Die Ausführung von Qualifikationsgruppen ist zurzeit in allen Stufen verfügbar, einschließlich der kostenlosen. Die Preisgestaltung für diese Funktion wird zu einem späteren Zeitpunkt bekannt gegeben.

1. Heften Sie den Dienst zwecks schnellem Zugriff auf Dienstinformationen an das Dashboard an.

  ![Dienstdefinitionsseite im Portal](./media/cognitive-search-tutorial-blob/create-search-service.png "Dienstdefinitionsseite im Portal")

### <a name="set-up-azure-blob-service-and-load-sample-data"></a>Einrichten des Azure Blob-Diensts und Laden von Beispieldaten

Die Anreicherungspipeline ruft mit Unterstützung von [Azure Search-Indexern](search-indexer-overview.md) aus Azure-Datenquellen ab. Für diese Übung verwenden wir Blobspeicher, um verschiedene Inhaltstypen anschaulich vorzustellen.

1. [Laden Sie die Beispieldaten herunter](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4), die aus einem kleinen Satz Dateien verschiedener Typen bestehen. 

1. Registrieren Sie sich für Azure Blob-Speicher, erstellen Sie ein Speicherkonto, melden Sie sich beim Storage-Explorer an, und erstellen Sie einen Container. Anweisungen zu all diesen Schritten finden Sie im [Azure Storage-Explorer-Schnellstart](../storage/blobs/storage-quickstart-blobs-storage-explorer.md).

1. Klicken Sie mithilfe des Azure Storage-Explorers in dem von Ihnen erstellten Container auf **Hochladen**, um die Beispieldateien hochzuladen.

  ![Quelldateien in Azure Blob Storage](./media/cognitive-search-quickstart-blob/sample-data.png)

## <a name="create-the-enrichment-pipeline"></a>Erstellen der Anreicherungspipeline

Wechseln Sie zur Dashboardseite des Azure Search-Diensts zurück, und klicken Sie auf der Eingabeleiste auf **Daten importieren**, um die Anreicherung in vier Schritten einzurichten.

### <a name="step-1-create-a-data-source"></a>Schritt 1: Erstellen einer Datenquelle

Wählen Sie unter **Verbindung mit Ihren Daten herstellen** > **Azure-Blobspeicher** das erstellte Konto und den erstellten Container aus. Geben Sie der Datenquelle einen Namen, und verwenden Sie für alles andere die Standardwerte. 

   ![Azure-Blobkonfiguration](./media/cognitive-search-quickstart-blob/blob-datasource.png)


Klicken Sie auf **OK**, um die Datenquelle zu erstellen.

Ein Vorteil des **Datenimport**-Assistenten besteht darin, dass er ebenfalls Ihren Index erstellen kann. Während die Datenquelle erstellt wird, konstruiert der Assistent zugleich ein Indexschema. Die Erstellung des Index kann einige Sekunden dauern.

### <a name="step-2-add-cognitive-skills"></a>Schritt 2: Hinzufügen von kognitiven Qualifikationen

Fügen Sie als nächstes der Indizierungspipeline Anreicherungsschritte hinzu. Das Portal stellt Ihnen vordefinierte kognitive Qualifikationen für die Bildanalyse und die Textanalyse zur Verfügung. Im Portal arbeitet eine Qualifikationsgruppe auf einem einzelnen Quellfeld. Das sieht vielleicht nach einer sehr bescheidenen Zielsetzung aus, aber im Fall von Azure-Blobs enthält das Feld `content` den größten Teil des Blob-Dokuments (beispielsweise ein Word-Dokument oder einen PowerPoint-Foliensatz). Daher stellt dieses Feld die ideale Eingabe dar, da sich hier alle Inhalte eines Blobs finden.

Manchmal kann es hilfreich sein, eine Textdarstellung aus Dateien zu extrahieren, die größtenteils aus gescannten Bildern bestehen – beispielsweise aus einer PDF-Datei, die durch einen Scanner erzeugt wurde. Azure Search kann automatisch Inhalte aus Bildern extrahieren, die in das Dokument eingebettet sind. Verwenden Sie hierzu die Option **OCR aktivieren und den gesamten Text im Feld "merged_content" zusammenführen**. Dadurch wird automatisch ein Feld namens `merged_content` erstellt, das sowohl den aus dem Dokument extrahierten Text als auch die Textdarstellung von Bildern enthält, die in das Dokument eingebettet sind. Bei Verwendung dieser Option wird `Source data field` auf `merged_content` festgelegt.

Wählen Sie unter **Add cognitive skills**, (Kognitive Qualifikationen hinzufügen) Qualifikationen aus, die Verarbeitung natürlicher Sprache ausführen. Wählen Sie für diesen Schnellstart Entitätserkennung für Personen, Organisationen und Orte aus.

Klicken Sie auf **OK**, um die Definition zu akzeptieren.
   
  ![Definition von Qualifikationsgruppen](./media/cognitive-search-quickstart-blob/skillset.png)

Qualifikationen für die Verarbeitung natürlicher Sprache arbeiten auf Textinhalten im Beispieldatenset. Da wir keine Optionen für die Bildverarbeitung ausgewählt haben, werden die JPEG-Dateien im Beispieldatenset im Rahmen dieses Schnellstarts nicht verarbeitet. 

### <a name="step-3-configure-the-index"></a>Schritt 3: Konfigurieren des Index

Erinnern Sie sich an den Index, der zusammen mit der Datenquelle erstellt wurde? In diesem Schritt können Sie sein Schema anzeigen und ggf. Einstellungen überarbeiten. 

Für diesen Schnellstart legt der Assistent sinnvolle Standardwerte fest: 

+ Jeder Index muss einen Namen haben. Für diesen Datenquellentyp ist der Standardname *azureblob-index*.

+ Jedes Dokument muss einen Schlüssel aufweisen. Der Assistent wählt ein Feld mit eindeutigen Werten aus. In diesem Schnellstart ist der Schlüssel *metadata_storage_path*.

+ Jede Feldsammlung muss Felder mit einem Datentyp aufweisen, der die Werte der Felder beschreibt, und jedes Feld sollte über Indexattribute verfügen, die ihre Verwendung in einem Suchszenario beschreiben. 

Da Sie eine Qualifikationsgruppe definiert haben, geht der Assistent davon aus, dass es Ihnen um das Quelldatenfeld zuzüglich der von den Qualifikationen erstellten Ausgabefelder geht. Aus diesem Grund fügt das Portal Indexfelder für `content`, `people`, `organizations` und `locations` hinzu. Beachten Sie, dass der Assistent diese Felder automatisch als „Abrufbar“ und „Durchsuchbar“ festlegt.

Überprüfen Sie in **Index anpassen**, die Attribute der Felder, um zu sehen, wie sie in einem Index verwendet werden. „Durchsuchbar“ gibt an, dass ein Feld durchsucht werden kann. „Abrufbar“ bedeutet, dass es in Ergebnissen zurückgegeben werden kann. 

Erwägen Sie, „Abrufbar“ aus dem Feld `content` zu löschen. In Blobs kann dieses Feld leicht mehrere Tausend Zeilen beinhalten, was sich in einem Tool wie dem **Suchexplorer** nur schwer lesen lässt.

Klicken Sie auf **OK**, um die Indexdefinition zu akzeptieren.

  ![Indexfelder](./media/cognitive-search-quickstart-blob/index-fields.png)

> [!NOTE]
> Nicht verwendete Felder wurden aus dem Screenshot zwecks knapperer Darstellung entfernt. Beim Nachvollziehen im Portal werden Sie in Ihrer Liste zusätzliche Felder bemerken.

### <a name="step-4-configure-the-indexer"></a>Schritt 4: Konfigurieren des Indexers

Der Indexer ist eine allgemeine Ressource, die den Indizierungsvorgang antreibt. Er gibt den Datenquellennamen, den Index und die Häufigkeit der Ausführung an. Das Endergebnis des **Datenimport**-Assistenten ist immer ein Indexer, der separat ausgeführt werden kann.

Geben Sie auf der Seite **Indexer** dem Indexer einen Namen, und verwenden Sie den Standardwert "Einmal ausführen", um ihn sofort auszuführen. 

  ![Indexerdefinition](./media/cognitive-search-quickstart-blob/indexer-def.png)

Klicken Sie auf **OK**, um die Daten zu importieren, anzureichern und zu indizieren.

  ![Azure Search-Benachrichtigung](./media/cognitive-search-quickstart-blob/indexer-notification.png)

Indizierung und Anreicherung können zeitaufwändig sein, was der Grund dafür ist, dass wir fürs erste Kennenlernen kleinere Datensets empfehlen. Sie können die Indizierung auf der Seite „Benachrichtigungen“ im Azure-Portal überwachen. 

## <a name="query-in-search-explorer"></a>Abfragen im Suchexplorer

Nach der Erstellung eines Index können Sie Abfragen senden, um Dokumente aus dem Index zurückzugeben. Verwenden Sie im Portal den **Suchexplorer**, um Abfragen auszuführen und Ergebnisse anzuzeigen. 

1. Klicken Sie auf der Dashboardseite des Suchdiensts auf der Befehlsleiste auf **Suchexplorer**.

1. Wählen Sie oben **Index ändern** aus, um den von Ihnen erstellten Index auszuwählen.

1. Geben Sie eine Suchzeichenfolge ein, um den Index abzufragen, wie etwa „John F. Kennedy“.

Die Ergebnisse werden in JSON zurückgegeben, was sehr ausführlich und schwierig zu lesen sein kann, insbesondere bei langen Dokumenten, die aus Azure-Blobs stammen. 

Wenn Sie die Ergebnisse nicht komfortabel durchsuchen können, verwenden Sie STRG-F, um in Dokumenten zu suchen. Bei dieser Abfrage können Sie im JSON nach "John F. Kennedy" suchen, um Instanzen dieses Suchbegriffs anzuzeigen. 

STRG-F kann Ihnen darüber hinaus helfen, zu ermitteln, wie viele Dokumente sich in einem bestimmten Resultset befinden. Für Azure-Blobs wählt das Portal „metadata_storage_path“ als Schlüssel aus, das jeder Wert für das Dokument eindeutig ist. Suchen Sie mithilfe von STRG-F nach „metadata_storage_path“, um die Anzahl der Dokumente zu erhalten. Bei dieser Abfrage enthalten zwei Dokumente im Resultset den Ausdruck „John F. Kennedy“.

  ![Suchexplorer-Beispiel](./media/cognitive-search-quickstart-blob/search-explorer.png)

## <a name="takeaways"></a>Wesentliche Punkte

Sie haben jetzt Ihre erste Übung in erweiterter Indizierung abgeschlossen. Der Zweck dieses Schnellstarts bestand darin, Ihnen wichtige Konzepte vorzustellen und Sie praktisch durch den Assistenten zu führen, damit Sie schnell einen Prototypen einer kognitiven Suchlösung für Ihre eigenen Daten erstellen können.

Einige wichtige Konzepte, von denen wir hoffen, dass Sie sie verinnerlicht haben, schließen die Abhängigkeit von Azure-Datenquellen ein. Die Anreicherung der kognitiven Suche ist an Indexer gebunden, und Indexer sind Azure- und quellenspezifisch. Dieser Schnellstart verwendet Azure Blob Storage, jedoch sind auch andere Azure-Datenquellen möglich. Weitere Informationen finden Sie unter [Indexer in Azure Search](search-indexer-overview.md).

Ein anderes wichtiges Konzept ist, dass Qualifikationen auf Eingabefeldern arbeiten. Im Portal müssen Sie ein einzelnes Quellfeld für alle Qualifikationen auswählen. Im Code können andere Felder oder die Ausgabe einer Upstreamqualifikation als Eingaben dienen.

 Eingaben für eine Qualifikation werden einem Ausgabefeld in einem Index zugeordnet. Intern richtet das Portal [Anmerkungen](cognitive-search-concept-annotations-syntax.md) ein und definiert eine [Qualifikationsgruppe](cognitive-search-defining-skillset.md), um die Reihenfolge der Vorgänge und den allgemeinen Ablauf festzulegen. Diese Schritte sind im Portal ausgeblendet, werden aber wichtig, wenn Sie selbst mit der Erstellung von Code beginnen.

Schließlich haben Sie gelernt, dass das Anzeigen von Ergebnissen durch Abfragen des Index erreicht wird. Was Azure Search bereitstellt, ist ein durchsuchbarer Index, den Sie entweder mit der [einfachen](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) oder mit der [vollständig erweiterten Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) abfragen können. Ein Index, der angereicherte Felder enthält, ist wie jeder andere. Wenn Sie Standard- oder [benutzerdefinierte Analysetools](search-analyzers.md), [Bewertungsprofile](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [Synonyme](search-synonyms.md), [facettierte Filter](search-filters-facets.md), geografische Suche oder andere Funktionen von Azure Search einbeziehen möchten, stehen Ihnen alle Wege offen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie Ihre Erkundung abgeschlossen haben, besteht die schnellste Möglichkeit, das System aufzuräumen, im Löschen der Ressourcengruppe, die den Azure Search-Dienst und den Azure Blob-Dienst enthält.  

Unter der Annahme, dass Sie beide Dienste in der gleichen Gruppe platziert haben, löschen Sie nun einfach die Ressourcengruppe, um endgültig ihren gesamten Inhalt zu löschen, einschließlich der Dienste und aller gespeicherten Inhalte, die Sie für diese Übung erstellt haben. Im Portal finden Sie den Namen der Ressourcengruppe auf der Seite „Übersicht“ der einzelnen Dienste.

## <a name="next-steps"></a>Nächste Schritte

Machen Sie Experimente mit Indizierung und Anreicherung, indem Sie den Assistenten mit verschiedenen Qualifikationen und Quelldatenfeldern erneut ausführen. Um diese Schritte zu wiederholen, löschen Sie den Index und den Indexer, und erstellen Sie den Indexer mit einer neuen Mischung von Optionen neu.

+ Wählen Sie unter **Übersicht** > **Indizes** den von Ihnen erstellten Index aus, und klicken Sie dann auf **Löschen**.

+ Doppelklicken Sie unter **Übersicht** auf die Kachel **Indexer**. Suchen Sie den von Ihnen erstellten Indexer, und löschen Sie ihn.

Alternativ können Sie die erstellten Beispieldaten und Dienste wiederverwenden und im nächsten Tutorial lernen, wie Sie diese Aufgaben programmgesteuert erledigen. 

> [!div class="nextstepaction"]
> [Tutorial: Erlernen der REST-APIs für kognitive Suche](cognitive-search-tutorial-blob.md)