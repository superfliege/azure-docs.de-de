---
title: Cognitive Search für die Datenextrahierung und die Verarbeitung natürlicher Sprache in Azure Search | Microsoft-Dokumentation
description: Datenextrahierung, die Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) und die Bildverarbeitung zum Erstellen durchsuchbarer Inhalte in der Azure Search-Indizierung mithilfe von kognitiven Fähigkeiten.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: heidist
ms.openlocfilehash: 64b4c0a315e206cd260f2f1108362e92f55d1843
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304281"
---
# <a name="what-is-cognitive-search"></a>Was ist Cognitive Search?

Cognitive Search ist ein Vorschaufeature von [Azure Search](search-what-is-azure-search.md), das in allen Tarifen in den USA (Mitte/Süden) und Europa (Westen) verfügbar ist und KI zu Indizierungsworkloads hinzufügt. Bei der Datenextrahierung, der Verarbeitung natürlicher Sprache und der Bildverarbeitung werden die latenten Informationen während der Indizierung in unstrukturierten oder nicht durchsuchbaren Inhalten gesucht und in Azure Search für eine Durchsuchung zur Verfügung gestellt.

Die KI-Integration erfolgt durch *kognitive Fähigkeiten*, die Quelldokumente auf dem Weg zu einem Suchindex durch sequentielle Prozesse anreichern. 

![Diagramm der Cognitive Search-Pipeline](./media/cognitive-search-intro/cogsearch-architecture.png "Übersicht über die Cognitive Search-Pipeline")

Während der Indizierung verwendete Fähigkeiten können vordefiniert oder benutzerdefiniert sein:

+ [Vordefinierte Skills](cognitive-search-predefined-skills.md) basieren auf den gleichen KI-Algorithmen, die auch in Cognitive Services-APIs verwendet werden: [Erkennung benannter Entitäten](cognitive-search-skill-named-entity-recognition.md), [Schlüsselbegriffserkennung](cognitive-search-skill-keyphrases.md) und [OCR](cognitive-search-skill-ocr.md) sind nur wenige Beispiele dafür. 

+ [Benutzerdefinierte Fähigkeiten](cognitive-search-create-custom-skill-example.md) können von Ihnen für eine spezielle, für Sie erforderliche Verarbeitung entwickelt werden. Zu Beispielen für benutzerdefinierte Fähigkeiten zählen ein benutzerdefiniertes Entitätsmodul oder ein benutzerdefinierter Dokumentenklassifizierer, das bzw. der auf eine bestimmte Domäne ausgerichtet ist, wie z.B. Finanzen, wissenschaftliche Veröffentlichungen oder Medizin.

> [!NOTE]
> Die kognitive Suche ist als öffentliche Vorschauversion verfügbar, und die Ausführung von Skillsets ist derzeit kostenlos. Die Preise für diese Funktion werden zu einem späteren Zeitpunkt bekannt gegeben.

## <a name="components-of-cognitive-search"></a>Komponenten von Cognitive Search

Die Cognitive Search-Pipeline basiert auf [Azure Search-*Indexern*](search-indexer-overview.md), die Datenquellen durchforsten und End-to-End-Indexverarbeitung bieten. Die Fähigkeitengruppen werden jetzt an Indexer angefügt. Dabei werden Dokumente gemäß der von Ihnen definierten Fähigkeiten abgefangen und angereichert. Nach der Indizierung können Sie über Suchanforderungen über sämtliche [von Azure Search unterstützte Abfragetypen](search-query-overview.md) auf Inhalte zugreifen.  Wenn Sie mit Indexern noch nicht vertraut sind, werden Ihnen in diesem Abschnitt die erforderlichen Schritte erläutert.

### <a name="source-data-and-document-cracking-phase"></a>Phase der Entschlüsselung von Quelldaten und Dokumenten

Am Anfang der Pipeline befindet sich unstrukturierter Text oder Nicht-Text-Inhalt (z.B. Bilder und gescannte Dokumente als JPEG-Dateien). Die Daten müssen in einem Azure-Datenspeicherdienst enthalten sein, auf den ein Indexer Zugriff hat. Indexer können Quelldokumente entschlüsseln, um Text aus Quelldaten zu extrahieren.

![Phase der Entschlüsselung von Dokumenten](./media/cognitive-search-intro/document-cracking-phase-blowup.png "Dokumententschlüsselung")

 Zu den unterstützten Quellen zählen Azure Blob Storage, Azure Table Storage, Azure SQL-Datenbank und Azure Cosmos DB. Textbasierte Inhalte können aus den folgenden Dateitypen extrahiert werden: PDF-, Word-, PowerPoint und CSV-Dateien. Eine vollständige Liste finden Sie unter [Unterstützte Formate](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="cognitive-skills-and-enrichment-phase"></a>Phase der kognitiven Fähigkeiten und Anreicherung

Die Anreicherung erfolgt durch *kognitive Fähigkeiten*, die atomische Vorgänge durchführen. Sobald Sie über Textinhalte aus einer PDF-Datei verfügen, können Sie beispielsweise die Entitätserkennung, die Sprachenerkennung oder die Schlüsselbegriffserkennung anwenden, um neue Felder in Ihrem Index zu erstellen, die nativ in der Quelle nicht verfügbar sind. Die Auflistung der in Ihrer Pipeline verwendeten Fähigkeiten wird als *Fähigkeitengruppe* bezeichnet.  

![Anreicherungsphase](./media/cognitive-search-intro/enrichment-phase-blowup.png "enrichment phase")

Eine Fähigkeitengruppe basiert auf [vordefinierten kognitiven Fähigkeiten](cognitive-search-predefined-skills.md) oder [benutzerdefinierte Fähigkeiten](cognitive-search-create-custom-skill-example.md), die von Ihnen bereitgestellt und mit der Fähigkeitengruppe verbunden werden. Eine Fähigkeitengruppe kann minimal oder höchst komplex sein. Sie bestimmt nicht nur den Verarbeitungstyp, sondern auch die Reihenfolge der Vorgänge. Eine Fähigkeitengruppe bietet zusammen mit den als Bestandteil eines Indexers definierten Feldzuordnungen eine umfassende Beschreibung der Anreicherungspipeline. Weitere Informationen zum Zusammensetzen all dieser Teile finden Sie unter [Definieren einer Fähigkeitengruppe](cognitive-search-defining-skillset.md).

Die Pipeline generiert intern eine Sammlung angereicherter Dokumente. Sie können entscheiden, welche Teile der angereicherten Dokumente indizierbaren Feldern in Ihrem Suchindex zugeordnet werden sollen. Wenn Sie beispielsweise die Fähigkeiten der Schlüsselbegriffserkennung und der Entitätserkennung angewendet haben, werden diese neuen Felder Bestandteil des angereicherten Dokuments und können Feldern in Ihrem Index zugeordnet werden. Weitere Informationen zu Eingabe-/Ausgabeformationen finden Sie unter [Anmerkungen](cognitive-search-concept-annotations-syntax.md).

### <a name="search-index-and-query-based-access"></a>Suchindex und abfragebasierter Zugriff

Nach Abschluss der Verarbeitung verfügen Sie über einen Suchkorpus mit angereicherten Dokumenten, die mit Azure Search im Volltext durchsucht werden können. Entwickler und Benutzer greifen über das [Abfragen des Indexes](search-query-overview.md) auf den von der Pipeline generierten angereicherten Inhalt zu. 

![Index mit Suchsymbol](./media/cognitive-search-intro/search-phase-blowup.png "Index with search icon")

Der Index ist vergleichbar mit beliebigen anderen Indizes, die für Azure Search erstellt werden können: Sie können ihn durch benutzerdefinierte Analysen ergänzen, Fuzzysuchabfragen aufrufen, gefilterte Suchvorgänge hinzufügen oder zur Umgestaltung der Suchergebnisse mit Bewertungsprofilen experimentieren.

Indizes werden über ein Indexschema generiert, das die Felder, Attribute und weitere Konstrukte definiert, die an einen bestimmten Index angefügt wurden, wie z.B. Bewertungsprofile und Synonymzuordnungen. Nachdem ein Index definiert und aufgefüllt wurde, können Sie die Indizierung inkrementell durchführen, um neue und aktualisierte Quelldokumente zu übernehmen. Für bestimmte Änderungen ist eine vollständige Neuerstellung erforderlich. Sie sollten ein kleines Dataset verwenden, bis der Schemaentwurf stabil ist. Weitere Informationen finden Sie unter [Neuerstellen eines Indexes](search-howto-reindex.md).

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Wichtige Features und Konzepte

| Konzept | BESCHREIBUNG| Links |
|---------|------------|-------|
| Fähigkeitengruppe | Eine benannte allgemeine Ressource mit einer Sammlung von Fähigkeiten. Eine Fähigkeitengruppe in der Anreicherungspipeline. Sie wird während der Indizierung durch einen Indexer aufgerufen. | [Definieren einer Fähigkeitengruppe](cognitive-search-defining-skillset.md) |
| Kognitive Fähigkeit | Eine atomische Transformation in einer Anreicherungspipeline. Häufig handelt es sich dabei um eine Komponente, die sich mit der Extrahierung oder der Erschließung einer Struktur befasst und folglich Ihr Verständnis der Eingabedaten ergänzt. Die Ausgabe ist fast immer textbasiert. Bei der Verarbeitung wird auf die Verarbeitung natürlicher Sprache oder die Bildbearbeitung zurückgegriffen, bei der Text aus Bildeingaben extrahiert oder generiert wird. Die Ausgabe aus einer Fähigkeit kann einem Feld in einem Index zugeordnet werden oder als Eingabe für eine Downstream-Anreicherung verwendet werden. Eine Fähigkeit ist entweder vordefiniert und wird von Microsoft bereitgestellt, oder sie ist benutzerdefiniert und wird von Ihnen erstellt und bereitgestellt. | [Vordefinierte Skills](cognitive-search-predefined-skills.md) |
| Extrahieren von Daten | Deckt ein breites Verarbeitungsspektrum ab, in Bezug auf Cognitive Search wird die Fähigkeit zur Erkennung benannter Entitäten jedoch zumeist zum Extrahieren von Daten (einer Entität) aus einer Quelle verwendet, die diese Informationen nicht nativ bereitstellt. | [Fähigkeit: Erkennung benannter Entitäten](cognitive-search-skill-named-entity-recognition.md)| 
| Bildverarbeitung | Erschließt sich Text aus einem Bild, z.B. die Fähigkeit der Erkennung eines Wahrzeichens oder des Extrahierens von Text aus einem Bild. Typische Beispiele dafür sind OCR für das Löschen von Zeichen aus einer Datei mit einem gescannten Dokument (JPEG) oder das Erkennen eines Straßennamens auf einem Foto mit einem Straßenschild. | [Fähigkeit: Bildanalyse](cognitive-search-skill-image-analysis.md) oder [Fähigkeit: OCR](cognitive-search-skill-ocr.md)
| Verarbeitung natürlicher Sprache | Textverarbeitung für Einblicke und Informationen zu Texteingaben. Sprachenerkennung, Standpunktanalyse und Schlüsselbegriffserkennung sind Fähigkeiten, die in den Bereich der Verarbeitung natürlicher Sprache fallen.  | [Fähigkeit: Schlüsselbegriffserkennung](cognitive-search-skill-keyphrases.md), [Fähigkeit: Sprachenerkennung](cognitive-search-skill-language-detection.md), [Fähigkeit: Standpunktanalyse](cognitive-search-skill-sentiment.md) |
| Dokumententschlüsselung | Der Prozess des Extrahierens oder Erstellens von Textinhalt aus Nicht-Text-Quellen während der Indizierung. OCR (Optical Character Recognition) ist ein Beispiel hierfür. In der Regel bezieht sich dies jedoch auf die Kernfunktionalität des Indexers, da dieser Inhalte aus Anwendungsdateien extrahiert. Sowohl die Datenquelle, die den Speicherort der Quelldatei bereitstellt, als auch die Indexerdefinition, die Feldzuordnungen zur Verfügung stellt, stellen Schlüsselfaktoren bei der Dokumententschlüsselung dar. | Weitere Informationen finden Sie unter [Indexer](search-indexer-overview.md). |
| Strukturierung | Textfragmente werden zu einer größeren Struktur konsolidiert, oder größere Textblöcke werden zur weiteren Downstream-Verarbeitung in eine überschaubare Größe aufgeteilt. | [Fähigkeit: Strukturierung](cognitive-search-skill-shaper.md), [Fähigkeit: Textzusammenführung](cognitive-search-skill-textmerger.md), [Fähigkeit: Textunterteilung](cognitive-search-skill-textsplit.md) |
| Angereicherte Dokumente | Eine vorübergehende interne Struktur, auf die im Code nicht direkt zugegriffen werden kann. Angereicherte Dokumente werden während der Verarbeitung generiert. In einem Suchindex werden jedoch nur endgültige Ausgaben permanent gespeichert. Feldzuordnungen bestimmen, welche Datenelemente zum Index hinzugefügt werden. | Weitere Informationen finden Sie unter [Zugriff auf angereicherte Dokumente](cognitive-search-tutorial-blob.md#access-enriched-document). |
| Indexer |  Ein Crawler, mit dem durchsuchbare Daten und Metadaten aus einer externen Datenquelle extrahiert werden und mit dem ein Index basierend auf Feld-zu-Feld-Zuordnungen zwischen dem Index und Ihrer Datenquelle zur Dokumententschlüsselung aufgefüllt wird. Bei Cognitive Search-Anreicherungen ruft der Indexer eine Fähigkeitengruppe auf und enthält die Feldzuordnungen, durch welche die Ausgabe der Anreicherung Zielfeldern im Index zugeordnet wird. Die Indexerdefinition enthält sämtliche Anweisungen und Verweise für Pipelinevorgänge. Die Pipeline wird bei Ausführung des Indexers aufgerufen. | [Indexer](search-indexer-overview.md) |
| Data source  | Ein von einem Indexer verwendetes Objekt zum Verbinden mit einer externen Datenquelle von unterstützten Typen in Azure. | Weitere Informationen finden Sie unter [Indexer](search-indexer-overview.md). |
| Index | Ein persistenter Suchkorpus in Azure Search, der aus einem Indexschema erstellt wurde, das die Feldstruktur und -verwendung definiert. | [Indizes in Azure Search](search-what-is-an-index.md) | 


## <a name="where-do-i-start"></a>Wo beginne ich?

**Schritt 1: Erstellen Sie in einer Region, in der APIs bereitgestellt werden, einen Suchdienst** 

+ USA Süd Mitte
+ Europa, Westen

**Schritt 2: Praktische Erfahrung zur Bewältigung des Workflows**

+ [Schnellstart (Portal)](cognitive-search-quickstart-blob.md)
+ [Tutorial (HTTP-Anforderungen)](cognitive-search-tutorial-blob.md)
+ [Beispiel für benutzerdefinierte Fähigkeiten (C#)](cognitive-search-create-custom-skill-example.md)

**Schritt 3: Überprüfen der API (nur REST)**

Derzeit werden nur REST-APIs bereitgestellt. Verwenden Sie `api-version=2017-11-11-Preview` für alle Anforderungen. Verwenden Sie die folgenden APIs zum Erstellen einer Cognitive Search-Lösung. Es werden nur zwei APIs für Cognitive Search hinzugefügt oder erweitert. Andere APIs haben die gleiche Syntax wie die allgemein verfügbaren Versionen.

| REST-API | BESCHREIBUNG |
|-----|-------------|
| [Erstellen der Datenquelle](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Eine Ressource, die eine externe Datenquelle identifiziert, welche Quelldaten zum Erstellen angereicherter Dokumente bereitstellt.  |
| [Erstellen einer Fähigkeitengruppe (api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Eine Ressource, welche die Verwendung von [vordefinierten Fähigkeiten](cognitive-search-predefined-skills.md) und [benutzerdefinierten kognitiven Fähigkeiten](cognitive-search-custom-skill-interface.md) in einer Anreicherungspipeline während der Indizierung koordiniert. |
| [Index erstellen](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Ein Schema zur Beschreibung eines Azure Search-Indexes. Felder im Index werden Feldern in den Quelldaten zugeordnet oder Feldern, die während der Anreicherungsphase erstellt wurden (z.B. ein von der Entitätserkennung erstelltes Feld für Organisationsnamen). |
| [Erstellen eines Indexers (api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Eine Ressource, die während der Indizierung verwendete Komponenten definiert: Dazu zählen eine Datenquelle, eine Fähigkeitengruppe, Feldzuordnungen der Quelle, intermediäre Datenstrukturen für den Zielindex und der Index selbst. Die Ausführung des Indexers ist Auslöser für die Datenerfassung und -anreicherung. Die Ausgabe ist ein Suchkorpus basierend auf dem Indexschema und wird mit Quelldaten aufgefüllt, die durch Fähigkeitengruppen angereichert werden.  |

**Prüfliste: Ein typischer Workflow**

1. Fügen Sie Teilmengen Ihrer Azure-Quelldaten in eine repräsentative Stichprobe ein. Die Indizierung braucht Zeit. Daher sollten Sie mit einem kleinen, repräsentativen Dataset beginnen, das Sie anschließend schrittweise aufbauen, während sich Ihre Lösung entwickelt.

1. Erstellen Sie ein [Datenquellenobjekt](https://docs.microsoft.com/rest/api/searchservice/create-data-source) in Azure Search, um eine Verbindungszeichenfolge für den Datenabruf bereitzustellen.

1. Erstellen Sie mithilfe von Anreicherungsschritten [Fähigkeitengruppen](https://docs.microsoft.com/rest/api/searchservice/create-skillset).

1. Definieren Sie das [Indexschema](https://docs.microsoft.com/rest/api/searchservice/create-index). Die *Fields*-Auflistung enthält Felder aus Quelldaten. Sie sollten zudem zusätzliche Felder durch Stubs ersetzen, um generierte Werte für Inhalte zu speichern, die während der Anreicherung erstellt werden.

1. Definieren Sie den [Indexer](https://docs.microsoft.com/rest/api/searchservice/create-skillset), der auf die Datenquelle, die Fähigkeitengruppen und den Index verweist.

1. Fügen Sie *outputFieldMappings* innerhalb des Indexers hinzu. In diesem Abschnitt wird die Ausgabe aus der Fähigkeitengruppe (in Schritt 3) den Eingabefeldern im Indexschema (in Schritt 4) zugeordnet.

1. Senden Sie die gerade erstellte Anforderung *Indexer erstellen* (eine POST-Anforderung mit einer Indexerdefinition im Anforderungstext), um den Indexer in Azure Search darzustellen. Dieser Schritt befasst sich mit der Ausführung des Indexers durch Aufrufen der Pipeline.

1. Führen Sie zum Auswerten von Ergebnissen und zum Ändern von Code Abfragen aus, um Fähigkeitengruppen, Schemas oder die Indexerkonfiguration zu aktualisieren.

1. [Setzen Sie den Indexer zurück](search-howto-reindex.md), bevor Sie die Pipeline neu erstellen.

Weitere Informationen zu bestimmten Fragen oder Problemen finden Sie unter [Problembehandlungstipps](cognitive-search-concept-troubleshooting.md).

## <a name="next-steps"></a>Nächste Schritte

+ [Dokumentation zu Cognitive Search](cognitive-search-resources-documentation.md)
+ [Schnellstart: Testen von Cognitive Search in einer Präsentation der Portalfunktion](cognitive-search-quickstart-blob.md)
+ [Tutorial: Informationen zu Such-APIs für Cognitive Search](cognitive-search-tutorial-blob.md)
