---
title: Was ist der Azure Search-Dienst? – Azure Search
description: Azure Search ist ein vollständig verwalteter, gehosteter Cloudsuchdienst von Microsoft. Lesen Sie Beschreibungen der Features, oder untersuchen Sie einen Entwicklungsworkflow, um zu erfahren, was Azure Search im Vergleich zu anderen Microsoft-Suchprodukten auszeichnet, und wie die ersten Schritte aussehen.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: overview
ms.date: 02/01/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: ab41826661db3f46161aca4e650e5e974c6b3b45
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "55994975"
---
# <a name="what-is-azure-search"></a>Was ist Azure Search?
Azure Search ist eine Search-as-a-Service-Cloudlösung, die Entwicklern APIs und Tools zum Hinzufügen von umfangreichen Suchfunktionen für private, heterogene Inhalte in Web- und Unternehmensanwendungen sowie in mobilen Anwendungen bietet. Die Abfrageausführung erfolgt über einen benutzerdefinierten Index.

+ Erstellen Sie einen Suchbestand, der nur Ihre Daten aus verschiedenen Inhaltstypen und Plattformen enthält. 
+ Nutzen Sie auf KI basierende Indizierung, um Text und Merkmale aus Bilddateien bzw. Entitäten und Schlüsselwörter aus unformatiertem Text zu extrahieren.
+ Erstellen Sie Suchoberflächen mit Facettennavigation und Filtern, Synonymen, AutoVervollständigen und Textanalyse für automatisch korrigierte Suchbegriffe („Meinten Sie...?“).
+ Fügen Sie eine geografische Umgebungssuche, Sprachanalysen für die Volltextsuche in nicht englischen Texten und Bewertungslogik für den Suchrang hinzu.

Die Funktionalität wird über eine einfache [REST-API](/rest/api/searchservice/) oder ein [.NET SDK](search-howto-dotnet-sdk.md) bereitgestellt, sodass die inhärente Komplexität des Informationsabrufs verborgen bleibt. Zusätzlich zu den APIs bietet das Azure-Portal mit Tools für die Prototyperstellung und Indexabfrage Unterstützung für Administration und Inhaltsverwaltung. Da der Dienst in der Cloud ausgeführt wird, werden die Infrastruktur und Verfügbarkeit von Microsoft verwaltet.

<a name="feature-drilldown"></a>

## <a name="feature-summary"></a>Zusammenfassung der Funktionen

| Category (Kategorie) | Features |
|----------|----------|
|Volltextsuche und Textanalyse | [Volltextsuche](search-lucene-query-architecture.md) ist für die meisten Apps auf Suchbasis ein primärer Anwendungsfall. Abfragen können mit einer unterstützten Syntax formuliert werden. <br/><br/>Die [**einfache Abfragesyntax**](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) bietet die logischen Operatoren, Operatoren für die Suche von Ausdrücken, Suffixoperatoren und Rangfolgenoperatoren.<br/><br/>Die [**Lucene-Abfragesyntax**](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) enthält alle Vorgänge in einfacher Syntax mit Erweiterungen für Fuzzysuche, NEAR-Suche, Term Boosting und reguläre Ausdrücke.|
|Kognitive Suche (Vorschau) | [Auf KI basierende Algorithmen](cognitive-search-concept-intro.md) für Bild- und Textanalyse können auf eine Indizierungspipeline angewendet werden, um Textinformationen aus Rohdateninhalt zu extrahieren. Zu den [integrierten Skills](cognitive-search-predefined-skills.md) zählen beispielsweise die optische Zeichenerkennung (ermöglicht das Durchsuchen gescannter JPEG-Dateien), die Entitätserkennung (Ermitteln einer Organisation, eines Namens oder eines Standorts) und die Schlüsselbegriffserkennung. Sie können auch [benutzerdefinierte Skills codieren](cognitive-search-create-custom-skill-example.md) und sie an die Pipeline anfügen. |
| Datenintegration | Azure Search-Indizes akzeptieren Daten aus beliebigen Quellen, sofern sie als JSON-Datenstruktur gesendet werden. <br/><br/> Optional können Sie für unterstützte Datenquellen in Azure [**Indexer**](search-indexer-overview.md) zum automatischen Durchforsten von [Azure SQL-Datenbank](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB](search-howto-index-cosmosdb.md) oder [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) für den Inhalt des Suchindex mit primären Datenspeicher verwenden. Azure Blob-Indexer können zum [Extrahieren von Text aus gängigen Dateiformaten](search-howto-indexing-azure-blob-storage.md), wie z.B. Microsoft Office-, PDF- und HTML-Dokumente, eine *Funktion zum Knacken von Dokumenten* ausführen. |
| Linguistische Analyse | Analysemodule sind Komponenten, die für die Textverarbeitung während Indizierungs- und Suchvorgängen eingesetzt werden. Es gibt zwei Arten. <br/><br/>[**Benutzerdefinierte lexikalische Analysen**](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) werden für komplexe Suchabfragen mit phonetischem Abgleich und regulären Ausdrücken verwendet. <br/><br/>[**Sprachanalysen**](https://docs.microsoft.com/rest/api/searchservice/language-support) von Lucene oder Microsoft werden verwendet, um sprachspezifische linguistische Probleme intelligent lösen zu können, z.B. Zeiten von Verben, Geschlecht, unregelmäßiger Plural von Substantiven (z.B. in Englisch: mouse/mice), Auflösen von zusammengesetzten Wörtern, Worttrennung (für Sprachen ohne Leerstellen) und vieles mehr. |
| Geografische Suche | Azure Search bietet Verarbeitung, Filterung und Anzeige geografischer Standorte. Mit Azure Search können die Benutzer Daten auf der Grundlage der Nähe eines Suchergebnisses zu einem physischen Ort untersuchen. [In diesem Video](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) oder [in diesem Beispiel](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) erfahren Sie mehr. |
| Funktionen der Benutzeroberfläche | [**AutoVervollständigen (Vorschauversion)**](search-autocomplete-tutorial.md) kann für Abfragen mit Textvervollständigung auf einer Suchleiste aktiviert werden. <br/><br/>[**Suchvorschläge**](https://docs.microsoft.com/rest/api/searchservice/suggesters) funktioniert auch mit Eingaben von Textteilen in eine Suchleiste, jedoch werden die Ergebnisse als tatsächliche Dokumente in Ihrem Index dargestellt und nicht als Abfragebegriffe. <br/><br/>[**Synonyme**](search-synonyms.md) ordnen entsprechende Begriffe zu, die den Bereich einer Abfrage implizit erweitern, ohne dass der Benutzer die alternativen Begriffe bereitstellen muss. <br/><br/>Die [**Facettennavigation**](search-faceted-navigation.md) wird über einen einzigen Abfrageparameter aktiviert. Azure Search gibt eine Struktur für die Facettennavigation zurück, die Sie als Code hinter einer Kategorieliste für eine selbstgesteuerte Filterung nutzen können (z.B., um Katalogartikel nach Preisbereich oder Marke zu filtern). <br/><br/> [**Filter**](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) ermöglichen die Integration einer Facettennavigation in die Benutzeroberfläche Ihrer Anwendung, die Erweiterung von Abfrageformulierungen sowie die Filterung auf der Grundlage von Kriterien von Benutzern oder Entwicklern. Erstellen Sie Filter mithilfe der OData-Syntax.<br/><br/> Mit der [**Treffermarkierung**](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) wird eine Textformatierung auf ein übereinstimmendes Schlüsselwort in den Suchergebnissen angewendet. Sie können auswählen, für welche Felder hervorgehobene Ausschnitte zurückgegeben werden sollen.<br/><br/>Eine [**Sortierung**](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) wird über das Indexschema für mehrere Felder bereitgestellt und dann zur Abfragezeit mit einem einzelnen Suchparameter umgeschaltet.<br/><br/> Dank der fein abgestimmten Steuerung, die Azure Search für Ihre Suchergebnisse bietet, sind [**Paginierung**](search-pagination-page-layout.md) und Einschränkung Ihrer Suchergebnisse kein Problem.  
| Relevance | Ein zentraler Vorteil von Azure Search ist die [**einfache Bewertung**](/rest/api/searchservice/add-scoring-profiles-to-a-search-index). Anhand von Bewertungsprofilen kann die Relevanz als Funktion der Werte in den Dokumenten selbst modelliert werden. Beispielsweise kann es wünschenswert sein, dass neuere Produkte oder herabgesetzte Produkte in den Suchergebnissen weiter oben angezeigt werden. Sie können Bewertungsprofile auch mithilfe von Tags für eine personalisierte Bewertung erstellen, die auf den Sucheinstellungen der Kunden basiert, die Sie nachverfolgt und separat gespeichert haben. |
| Überwachung und Berichterstellung | [**Analysedaten für Suchdatenverkehr**](search-traffic-analytics.md) werden erfasst und analysiert und ermöglichen Rückschlüsse auf Benutzereingaben im Suchfeld. <br/><br/>Metriken zu Abfragen pro Sekunde, Latenz und Drosselung werden erfasst und in Portalseiten protokolliert, ohne dass hierzu eine zusätzliche Konfiguration erforderlich ist. Sie können außerdem auf einfache Weise die Index- und Dokumentanzahl überwachen, um bei Bedarf die Kapazität anzupassen. Weitere Informationen finden Sie unter [Dienstverwaltung](search-manage.md). |
| Tools für Prototyperstellung und Überprüfung | Im Portal können Sie den [**Datenimport-Assistenten**](search-import-data-portal.md) zum Konfigurieren von Indexern, den Index-Designer zum Einrichten eines Index und den [**Suchexplorer**](search-explorer.md) zum Testen von Abfragen und Optimieren von Bewertungsprofilen verwenden. Sie können außerdem einen beliebigen Index öffnen, um sein Schema anzuzeigen. |
| Infrastruktur | Die **hoch verfügbare Plattform** macht den Suchdienst äußerst zuverlässig. [Azure Search bietet eine SLA von 99,9 Prozent](https://azure.microsoft.com/support/legal/sla/search/v1_0/)(bei ordnungsgemäßer Skalierung).<br/><br/> Azure Search ist eine **vollständig verwaltete und skalierbare** End-to-End-Lösung, für die keinerlei Infrastrukturverwaltungsaufwand anfällt. Eine zweidimensionale Skalierung ermöglicht die Anpassung des Diensts an Ihre Anforderungen, um mehr Dokumentspeicher, ein höheres Abfrageaufkommen oder beides zu bewältigen.

## <a name="how-to-use-azure-search"></a>Verwenden von Azure Search
### <a name="step-1-provision-service"></a>Schritt 1: Bereitstellen des Diensts
Einen Azure Search-Dienst können Sie entweder über das [Azure-Portal](https://portal.azure.com/) oder die [Azure-Ressourcenverwaltungs-API](/rest/api/searchmanagement/) bereitstellen. Sie können entweder den gemeinsam mit anderen Abonnenten genutzten kostenlosen Dienst auswählen oder eine [kostenpflichtige Ebene](https://azure.microsoft.com/pricing/details/search/), die Ressourcen zur ausschließlichen Nutzung für Ihren Dienst reserviert. Für kostenpflichtige Ebenen können Sie einen Dienst in zwei Dimensionen skalieren: 

- Fügen Sie Replikate hinzu, um Ihre Kapazität zum Verarbeiten intensiver Abfragelasten zu erweitern.   
- Fügen Sie Partitionen hinzu, um den Speicher für weitere Dokumente zu vergrößern. 

Durch getrenntes Verarbeiten von Dokumentspeicherung und Abfragedurchsatz können Sie Ressourcen auf Produktionsanforderungen basierend kalibrieren.

### <a name="step-2-create-index"></a>Schritt 2: Erstellen des Index
Bevor Sie durchsuchbaren Inhalt hochladen können, müssen Sie einen Azure Search-Index definieren. Einen Index können Sie sich wie eine Datenbanktabelle vorstellen, die Ihre Daten enthält und Suchabfragen entgegennimmt. Sie definieren das zuzuordnende Indexschema, um die Struktur der Dokumente widerzuspiegeln, die Sie durchsuchen möchten (ähnlich wie bei Feldern in einer Datenbank).

Ein Schema kann im Azure-Portal oder programmgesteuert mithilfe des [.NET SDK](search-howto-dotnet-sdk.md) oder der [REST-API](/rest/api/searchservice/) erstellt werden.

### <a name="step-3-load-data"></a>Schritt 3: Laden von Daten
Nachdem Sie einen Index definiert haben, können Sie Inhalte hochladen. Sie können entweder ein Push- oder Pullmodell verwenden.

Das Pullmodell ruft Daten aus externen Datenquellen ab. Es wird durch *Indexer* unterstützt, die Aspekte der Erfassung von Daten, z.B. Herstellen einer Verbindung mit Daten, Lesen und Serialisieren von Daten optimieren und automatisieren. [Indexer](/rest/api/searchservice/Indexer-operations) stehen für Azure Cosmos DB, Azure SQL-Datenbank, Azure Blob Storage und von SQL Server gehosteten Instanzen auf einem Azure-VM zur Verfügung. Sie können einen Indexer für bedarfsgesteuerte oder geplante Datenaktualisierung konfigurieren.

Das Pushmodell wird über das SDK oder die REST-APIs bereitgestellt, die zum Senden von aktualisierten Dokumenten an einen Index verwendet werden. Sie können Daten aus praktisch jedem Dataset mit JSON-Format übertragen. Anleitungen zum Laden von Daten finden Sie unter [Hinzufügen, Aktualisieren oder Löschen von Dokumenten](/rest/api/searchservice/addupdate-or-delete-documents) oder [Verwenden des .NET SDK](search-howto-dotnet-sdk.md).

### <a name="step-4-search"></a>Schritt 4: Suchen,
Nach dem Füllen eines Indexes können Sie über einfache HTTP-Anforderungen mit REST-API oder dem .NET-SDK [Suchabfragen](/rest/api/searchservice/Search-Documents) an Ihren Dienstendpunkt richten.

## <a name="how-it-compares"></a>Im Vergleich

Kunden interessieren sich häufig für einen Vergleich von Azure Search mit anderen Suchlösungen. Die wesentlichen Unterschiede sind in der folgenden Tabelle zusammengefasst.

| Im Vergleich zu | Wesentliche Unterschiede |
|-------------|-----------------|
|Bing | Die [Bing-Websuche-API](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/) durchsucht die Indizes auf „Bing.com“ nach Übereinstimmungen mit Begriffen, die Sie übermitteln. Die Indizes werden auf der Basis von HTML- und XML-Seiten sowie anderer Webinhalte aus öffentlich zugänglichen Websites erstellt. Die [benutzerdefinierte Bing-Suche](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/) basiert auf der gleichen Grundlage und bietet die gleiche Crawlertechnologie für Webinhaltstypen beschränkt auf einzelne Websites.<br/><br/>Azure Search durchsucht einen Index, den Sie definieren, gefüllt mit ihren Daten und Dokumenten, häufig aus verschiedenen Quellen. Azure Search verfügt über Crawlerfunktionen für einige Datenquellen über [Indexer](search-indexer-overview.md), aber Sie können jedes JSON-Dokument, das Ihrem Indexschema entspricht, mittels Pushübertragung in eine einzelne, konsolidierte durchsuchbare Ressource übertragen. |
|Datenbanksuche | Zahlreiche Systemplattformen umfassen eine integrierte Suchfunktion. SQL Server verfügt über eine [Volltextsuche](https://docs.microsoft.com/sql/relational-databases/search/full-text-search). Cosmos DB und ähnliche Technologien verfügen über abfragbare Indizes. Bei der Beurteilung von Produkten, die Suche und Speicher kombinieren, kann die Entscheidung eine Herausforderung darstellen. Viele Lösungen verwenden beides: DBMS als Speicher und Azure Search für spezialisierte Suchfunktionen.<br/><br/>Im Vergleich zur DBMS-Suche speichert Azure Search Inhalt aus heterogenen Quellen und bietet spezialisierte Textverarbeitungsfunktionen wie z.B. linguistikbasierte Textverarbeitung (Wortstammerkennung, Lemmatisierung, Wortformen) in [56 Sprachen](https://docs.microsoft.com/rest/api/searchservice/language-support). Der Dienst unterstützt auch die Autokorrektur falsch geschriebener Wörter, [Synonyme](https://docs.microsoft.com/rest/api/searchservice/synonym-map-operations), [Vorschläge](https://docs.microsoft.com/rest/api/searchservice/suggestions), [Bewertungssteuerelemente](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [Facets](https://docs.microsoft.com/azure/search/search-filters-facets) und [benutzerdefinierte Tokenisierung](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search). Das [Volltext-Suchmodul](search-lucene-query-architecture.md) in Azure Search basiert auf Apache Lucene, einem Branchenstandard für den Informationsabruf. Azure Search speichert Daten zwar in Form eines invertierten Index, ist jedoch nur selten ein Ersatz für echte Datenspeicher. Weitere Informationen finden Sie in diesem [Forumsbeitrag](https://stackoverflow.com/questions/40101159/can-azure-search-be-used-as-a-primary-database-for-some-data). <br/><br/>Die Ressourcenverwendung ist ein weiterer Wendepunkt in dieser Kategorie. Die Indizierung und einige Abfragevorgänge sind häufig rechenintensiv. Die Abladung der Suche vom DBMS auf eine dedizierte Lösung in der Cloud hält Systemressourcen für die Verarbeitung von Transaktionen frei. Wenn Sie die Suche nach außen verlagern, können Sie außerdem problemlos Ihrem Abfragevolumen gemäß skalieren.|
|Dedizierte Suchlösung | Wenn Sie sich für eine dedizierte Suche mit voller Funktionalität entschieden haben, erfolgt der endgültige Vergleich letztendlich zwischen einer lokalen Lösung und einem Cloud-Dienst. Viele Suchtechnologien bieten Kontrolle über Indizierungs- und Abfragepipelines, Zugriff auf umfangreichere Abfrage- und Filtersyntax, Kontrolle über Rangfolge und Relevanz sowie Funktionen für selbstgesteuerte und intelligente Suche. <br/><br/>Ein Clouddienst ist die richtige Wahl, wenn Sie eine sofort einsetzbare Lösung mit minimalem Aufwand, minimaler Wartung und anpassbarer Skalierung wünschen. <br/><br/>Innerhalb des Cloudparadigmas bieten einige Anbieter vergleichbare Basisfunktionen mit Volltextsuche, geografischer Suche und der Möglichkeit zur Verarbeitung einer gewissen Ungenauigkeit bei der Sucheingabe. Das passende Suchmodul ergibt sich in der Regel durch ein [spezialisiertes Feature](#feature-drilldown) oder durch die Benutzerfreundlichkeit und Einfachheit von APIs, Tools und Verwaltungsfunktionen. |

Unter den Cloudanbietern eignet Azure Search sich am besten für die Volltextsuche in Inhaltsspeichern und Datenbanken in Azure, für Apps, bei denen Informationsabruf und Inhaltsnavigation primär auf der Suche basieren. 

Wichtige Vorteile umfassen:

+ Azure-Datenintegration (Crawler) auf Indexebene
+ Azure-Portal für eine zentrale Verwaltung
+ Skalierung, Zuverlässigkeit und erstklassige Verfügbarkeit von Azure
+ Linguistische und benutzerdefinierte Analyse, mit Analysemodulen für eine robuste Volltextsuche in 56 Sprachen
+ [Kernfeatures für auf die Suche ausgerichtete Apps](#feature-drilldown): Bewertung, Facettennavigation, Vorschläge, Synonyme, geografische Suche und mehr.

> [!Note]
> Datenquellen, die nicht aus Azure stammen, werden vollständig unterstützt, unterliegen jedoch statt Indexern einer codeintensiveren Pushmethode. Mit APIs können Sie eine beliebige JSON-Dokumentsammlung an einen Azure Search-Index weiterreichen.

Unter unseren Kunden zählen diejenigen mit Onlinekatalogen, Branchenprogrammen und Anwendungen zur Dokumentermittlung zu denen, die am meisten von den umfassenden Features in Azure Search profitieren.

## <a name="rest-api--net-sdk"></a>REST-API | .NET SDK

Auch wenn im Portal viele Aufgaben ausgeführt werden können, ist Azure Search in erster Linie für Entwickler konzipiert, die Suchfunktionalität in vorhandene Anwendungen integrieren möchten. Folgende Programmierschnittstellen stehen zur Verfügung:

|Plattform |BESCHREIBUNG |
|-----|------------|
|[REST](/rest/api/searchservice/) | Durch eine beliebige Programmierplattform und -sprache (darunter Xamarin, Java und JavaScript) unterstützte HTTP-Befehle.|
|[.NET SDK](search-howto-dotnet-sdk.md) | Der .NET-Wrapper für die REST-API ermöglicht eine effiziente Codierung in C# und andere Sprachen für verwalteten Code, die auf das .NET Framework ausgerichtet sind. |

## <a name="free-trial"></a>Kostenlose Testversion
Azure-Abonnenten können [einen Dienst im Free-Tarif bereitstellen](search-create-service-portal.md).

Wenn Sie kein Abonnent sind, können Sie [ein kostenloses Azure-Konto eröffnen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Sie erhalten Gutschriften zum Testen kostenpflichtiger Azure-Dienste. Wenn das Guthaben aufgebraucht ist, können Sie das Konto behalten und weiterhin [kostenlose Azure-Dienste](https://azure.microsoft.com/free/) nutzen. Ihre Kreditkarte wird nur dann belastet, wenn Sie Ihre Einstellungen explizit ändern und mit der Berechnung von Gebühren einverstanden sind.

Alternativ dazu können Sie Ihre [Vorteile für MSDN-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können. 

## <a name="how-to-get-started"></a>Erste Schritte

1. Erstellen Sie einen [kostenlosen Dienst](search-create-service-portal.md). Alle Schnellstartanleitungen und Tutorials können mit dem kostenlosen Dienst abgeschlossen werden.

2. Gehen Sie das [Tutorial zum Verwenden von integrierten Tools für Indizierung und Abfragen](search-get-started-portal.md) durch. Lernen Sie wichtige Konzepte kennen, und machen Sie sich mit den im Portal bereitgestellten Informationen vertraut.

3. Verwenden Sie den Code entweder mit der .NET- oder der REST-API:

  + [Verwenden von Azure Search aus einer .NET-Anwendung](search-howto-dotnet-sdk.md) veranschaulicht den Hauptworkflow in verwaltetem Code.  
  + [Erste Schritte mit der REST-API](https://github.com/Azure-Samples/search-rest-api-getting-started) zeigt die gleichen Schritte in der REST-API. Sie können auch mit der folgenden Schnellstartanleitung REST-APIs aus Postman oder Fiddler aufrufen: [Untersuchen von Azure Search-REST-APIs mit Fiddler oder Postman](search-fiddler.md).

## <a name="watch-this-video"></a>Video ansehen

Suchmodule sind die wichtigsten Komponenten für den Informationsabruf in mobilen Apps, im Web und in Unternehmensdatenspeichern. Azure Search bietet Tools zum Erstellen einer Suchoberfläche, die denen großer kommerzieller Websites ähnelt.

In diesem 9-Minuten-Video von Programm-Manager Liam Cavanagh erfahren Sie, wie Ihre App von der Integration einer Suchmaschine profitieren kann. Kurze Demos behandeln Schlüsselfeatures in Azure Search und zeigen, wie ein typischer Workflow aussieht. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ Minute 0–3: Vorstellung der wichtigste Features und Anwendungsfälle.
+ Minute 3–4: Erläuterung der Dienstbereitstellung. 
+ Minute 4–6: Vorstellung des Datenimport-Assistenten zum Erstellen eines Index mithilfe des integrierten Immobilien-Beispieldatasets.
+ Minute 6–9: Vorstellung des Search-Explorers und verschiedener Abfragen.
