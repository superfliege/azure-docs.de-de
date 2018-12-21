---
title: Tutorial zum Indizieren, Abfragen und Filtern im Azure-Portal – Azure Search
description: In diesem Tutorial generieren Sie über das Azure-Portal und mit vordefinierten Beispieldaten einen Index in Azure Search. Machen Sie sich unter anderem mit Volltextsuche, Filtern, Facets, Fuzzysuche und Geosuche vertraut.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/10/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: d8b95aaab99fc4f0aa5df21374d6ec023f869b7d
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314022"
---
# <a name="tutorial-use-built-in-portal-tools-for-azure-search-indexing-and-queries"></a>Tutorial: Verwenden von integrierten Portaltools für Azure Search-Indizierungen und -Abfragen

Sie können die integrierten Tools auf der Seite des Azure Search-Diensts im Azure-Portal nutzen, um sich mit Azure Search-Konzepten vertraut zu machen. Diese Tools bieten unter Umständen nicht den vollen Funktionsumfang der .NET- und REST-APIs. Die Assistenten und Editoren bieten jedoch eine codefreie Einführung in Azure Search und ermöglichen umgehend das Schreiben interessanter Abfragen für ein Beispieldataset.

> [!div class="checklist"]
> * Beginnen Sie mit den öffentlichen Beispieldaten, und generieren Sie automatisch einen Azure Search-Index, indem Sie den Assistenten **Daten importieren** verwenden.
> * Sie können das Indexschema und die Attribute für alle Indizes anzeigen, die unter Azure Search veröffentlicht werden.
> * Verwenden Sie den **Suchexplorer**, um sich mit Volltextsuche, Filtern, Facets, Fuzzysuche und Geosuche vertraut zu machen.  

Falls die Tools über zu viele Einschränkungen verfügen, können Sie die Verwendung einer [codebasierten Einführung in die Azure Search-Programmierung in .NET](search-howto-dotnet-sdk.md) oder von [Webtesttools für REST-API-Aufrufe](search-fiddler.md) erwägen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. Außerdem können Sie sich ein sechsminütiges Demovideo zu den Schritten in diesem Tutorial ansehen (etwa ab der dritten Minute [dieses Übersichtsvideos für Azure Search](https://channel9.msdn.com/Events/Connect/2016/138)).

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie einen Azure Search-Dienst](search-create-service-portal.md), oder suchen Sie unter Ihrem aktuellen Abonnement nach einem vorhandenen Dienst.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Öffnen Sie das Service-Dashboard des Azure-Suchdiensts. Falls Sie die Dienstkachel nicht an Ihr Dashboard angeheftet haben, finden Sie Ihren Dienst wie folgt:

   * Klicken Sie auf der Navigationsleiste im linken Navigationsbereich auf **Alle Dienste**.
   * Geben Sie in das Suchfeld den Suchbegriff *Suche* ein, um eine Liste mit suchbezogenen Diensten für Ihr Abonnement zu erhalten. Klicken Sie auf **Suchdienste**. Ihr Dienst sollte in der Liste angezeigt werden.

### <a name="check-for-space"></a>Überprüfen des Speicherplatzes

Viele Kunden beginnen mit dem kostenlosen Dienst (Free). Diese Version ist auf drei Indizes, drei Datenquellen und drei Indexer beschränkt. Stellen Sie sicher, dass Sie über ausreichend Platz für zusätzliche Elemente verfügen, bevor Sie beginnen. In diesem Tutorial wird jeweils eine Instanz von jedem Objekt erstellt.

> [!TIP]
> Auf den Kacheln des Service-Dashboards sehen Sie, über wie viele Indizes, Indexer und Datenquellen Sie bereits verfügen. Die Indexerkachel zeigt Erfolgs- und Fehlerindikatoren. Klicken Sie auf die Kachel, um die Indexeranzahl anzuzeigen.
>
> ![Kacheln für Indexer und Datenquellen][1]
>

## <a name="create-index"></a> Erstellen eines Index und Laden von Daten

Suchabfragen durchlaufen einen [*Index*](search-what-is-an-index.md) mit durchsuchbaren Daten, Metadaten und zusätzlichen Konstrukten, die bestimmte Suchverhaltensweisen optimieren.

Für dieses Tutorial verwenden Sie ein integriertes Beispieldataset, das über den Assistenten **Daten importieren** mit einem [*Indexer*](search-indexer-overview.md) durchforstet werden kann. Ein Indexer ist ein quellspezifischer Crawler, mit dem Metadaten und Inhalte von unterstützten Azure-Datenquellen gelesen werden können. Diese Indexer werden im Portal über den Assistenten **Daten importieren** angezeigt. Später können Sie Indexer als unabhängige Ressourcen programmgesteuert erstellen und verwalten.

### <a name="step-1-start-the-import-data-wizard"></a>Schritt 1: Starten des Datenimport-Assistenten

1. Klicken Sie auf dem Dashboard des Azure Search-Diensts in der Befehlsleiste auf **Daten importieren**, um den Assistenten zu starten. Dieser Assistent unterstützt Sie beim Erstellen und Auffüllen eines Suchindex.

    ![Befehl zum Importieren von Daten][2]

2. Klicken Sie im Assistenten auf **Daten verbinden** > **Beispiele** > **realestate-us-sample**. Diese Datenquelle ist mit einem Namen, einem Typ und Verbindungsinformationen vorkonfiguriert. Nach der Erstellung wird sie zu einer vorhandenen Datenquelle, die in anderen Importvorgängen wiederverwendet werden kann.

    ![Auswählen des Beispieldatasets][9]

3. Klicken Sie auf **OK**, um es zu verwenden.

### <a name="skip-cognitive-skills"></a>Überspringen von kognitiven Qualifikationen

**Daten importieren** bietet einen optionalen Schritt für kognitive Qualifikationen, in dem der Indizierung KI-Algorithmen hinzugefügt werden können. Überspringen Sie diesen Schritt vorerst, und fahren Sie mit **Zielindex anpassen** fort.

> [!TIP]
> Sie können das neue Vorschaufeature für die kognitive Suche in Azure Search in der [Schnellstartanleitung zur kognitiven Suche](cognitive-search-quickstart-blob.md) oder im [Tutorial](cognitive-search-tutorial-blob.md) ausprobieren.

   ![Überspringen des Schritts zu kognitiven Qualifikationen][11]

### <a name="step-2-define-the-index"></a>Schritt 2: Definieren des Index

Ein Index wird in der Regel manuell und mithilfe von Code erstellt. Für dieses Tutorial kann der Assistent jedoch einen Index für jede Datenquelle erstellen, die er durchforsten kann. Für einen Index sind mindestens ein Name und eine Felderauflistung erforderlich, wobei ein Feld als Dokumentschlüssel gekennzeichnet sein muss, um die einzelnen Dokumente eindeutig zu identifizieren.

Felder besitzen Datentypen und Attribute. Die Kontrollkästchen im oberen Bereich sind *Indexattribute*, mit denen die Verwendung des Felds gesteuert wird.

* **Abrufbar** bedeutet, dass es in der Liste mit den Suchergebnissen angezeigt wird. Sie können einzelne Felder kennzeichnen, indem Sie dieses Kontrollkästchen deaktivieren, um sie aus den Suchergebnissen auszuschließen, z. B. wenn Felder nur in Filterausdrücken verwendet werden.
* Mit **Filterbar**, **Sortierbar** und **Facettenreich** wird angegeben, ob ein Feld in einer Filter-, Sortier- oder Facettennavigationsstruktur verwendet werden kann.
* **Durchsuchbar** bedeutet, dass ein Feld in die Volltextsuche einbezogen wird. Zeichenfolgen sind durchsuchbar. Numerische Felder und boolesche Felder werden häufig als „Nicht durchsuchbar“ markiert.

Standardmäßig durchsucht der Assistent die Datenquelle nach eindeutigen Bezeichnern als Grundlage für das Schlüsselfeld. Zeichenfolgen werden durch Attribute als abrufbar und durchsuchbar gekennzeichnet. Ganze Zahlen werden durch Attribute als abrufbar, filterbar, sortierbar und facettierbar gekennzeichnet.

  ![Generierter Immobilienindex][3]

Klicken Sie auf **OK**, um den Index zu erstellen.

### <a name="step-3-define-the-indexer"></a>Schritt 3: Definieren des Indexers

Klicken Sie im Assistenten **Daten importieren** auf **Indexer** > **Name**, und geben Sie einen Namen für den Indexer ein.

Mit diesem Objekt wird ein ausführbarer Prozess definiert. Sie könnten zwar auch einen wiederkehrenden Zeitplan einrichten, in diesem Tutorial verwenden Sie allerdings die Standardoption, um den Indexer nach dem Klicken auf **OK** umgehend einmalig auszuführen.  

  ![Immobilienindexer][8]

### <a name="check-progress"></a>Überprüfen des Status

Wechseln Sie zum Überwachen des Datenimports zurück zum Dashboard des Diensts, scrollen Sie nach unten, und doppelklicken Sie auf die Kachel **Indexer**, um die Liste mit den Indexern zu öffnen. Der neu erstellte Indexer sollte in der Liste angezeigt werden. Der Status sollte „In Bearbeitung“ oder „Erfolgreich“ lauten, und die Anzahl indizierter Dokumente sollte angegeben sein.

   ![Statusmeldung des Indexers][4]

### <a name="step-4-view-the-index"></a>Schritt 4: Anzeigen des Index

Die Kacheln im Dienstdashboard liefern zusammenfassende Informationen zu den verschiedenen Objekten in einer Ressource und ermöglichen den Zugriff auf ausführlichere Informationen. Auf der Kachel **Indizes** wird eine Liste der vorhandenen Indizes angezeigt, z.B. der Index *realestate-us-sample*, den Sie gerade im vorherigen Schritt erstellt haben.

Klicken Sie nun auf den Index *realestate-us-sample*, um die Portaloptionen für die zugehörige Definition anzuzeigen. Mit der Option **Felder hinzufügen/bearbeiten** können Sie neue Felder erstellen und vollständig mit Attributen versehen. Vorhandene Felder verfügen über eine physische Darstellung in Azure Search und können daher nicht geändert werden (auch nicht im Code). Erstellen Sie ein neues Feld, und verwerfen Sie das Original, um ein vorhandenes Feld grundlegend zu ändern.

   ![Beispiel für Indexdefinition][10]

Andere Konstrukte, z.B. Bewertungsprofile und CORS-Optionen, können jederzeit hinzugefügt werden.

Nehmen Sie sich kurz Zeit, um die Optionen für die Indexdefinition anzuzeigen und sich damit vertraut zu machen, was Sie beim Entwerfen des Index bearbeiten können. Wenn Optionen ausgegraut sind, ist dies ein Hinweis darauf, dass ein Wert nicht geändert oder gelöscht werden kann. Überspringen Sie ebenfalls vorerst die Kontrollkästchen für das Analysetool und das Vorschlagsmodul.

## <a name="query-index"></a> Abfragen des Index

Sie sollten nun über einen Suchindex verfügen, der über die integrierte [**Suchexplorer**](search-explorer.md)-Abfrageseite abgefragt werden kann. Sie enthält ein Suchfeld, über das Sie beliebige Abfragezeichenfolgen testen können.

> [!TIP]
> Die folgenden Schritte werden im [Übersichtsvideo für Azure Search](https://channel9.msdn.com/Events/Connect/2016/138) ab 6:08 vorgeführt.
>

1. Klicken Sie auf der Befehlsleiste auf **Suchexplorer** .

   ![Suchexplorerbefehl][5]

2. Klicken Sie auf der Befehlsleiste auf **Index ändern**, um zu *realestate-us-sample* zu wechseln. Klicken Sie auf der Befehlsleiste auf **API-Version festlegen**, um die verfügbaren REST-APIs anzuzeigen. Verwenden Sie für die Abfragen weiter unten die allgemein verfügbare Version (2017-11-11).

   ![Index- und API-Befehle][6]

3. Geben Sie über die Suchleiste die folgenden Abfragezeichenfolgen ein, und klicken Sie auf **Suchen**.

    > [!NOTE]
    > Der **Suchexplorer** ist nur für die Verarbeitung von [REST-API-Anforderungen](https://docs.microsoft.com/rest/api/searchservice/search-documents) ausgelegt. Er akzeptiert sowohl Syntax für [einfache Abfragen](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) als auch für [vollständige Lucene-Abfrageparser](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) sowie alle Suchparameter, die bei Vorgängen vom Typ [Dokument durchsuchen](https://docs.microsoft.com/rest/api/searchservice/search-documents) zur Verfügung stehen.
    >

### <a name="simple-query-with-top-n-results"></a>Einfache Abfrage mit Top N-Ergebnissen

#### <a name="example-string-searchseattle"></a>Beispiel (Zeichenfolge): `search=seattle`

* Der Parameter **search** dient zum Eingeben eines Schlüsselworts für die Volltextsuche. In diesem Fall werden Angebote in King County (Washington) zurückgegeben, die den Begriff *Seattle* in einem beliebigen durchsuchbaren Feld des Dokuments enthalten.

* Der **Suchexplorer** gibt Ergebnisse im JSON-Format zurück. Dieses Format ist sehr ausführlich und in Dokumenten mit einer dichten Struktur nur schwer lesbar. Dies ist so gewollt. Die Sichtbarkeit des gesamten Dokuments ist wichtig für Entwicklungszwecke, vor allem beim Testen. Zur Verbesserung der Benutzerfreundlichkeit müssen Sie Code schreiben, mit dem [Suchergebnisse verarbeitet werden](search-pagination-page-layout.md), um wichtige Elemente hervorzuheben.

* Dokumente bestehen aus allen Feldern, die im Index als „abrufbar“ gekennzeichnet sind. Klicken Sie zum Anzeigen von Indexattributen im Portal auf der Kachel **Indizes** auf *realestate-us-sample*.

#### <a name="example-parameterized-searchseattlecounttruetop100"></a>Beispiel (parametrisiert): `search=seattle&$count=true&$top=100`

* Das Symbol **&** dient zum Anfügen von Suchparametern. Diese können in beliebiger Reihenfolge angegeben werden.

* Der Parameter **$count=true** gibt die Gesamtanzahl aller zurückgegebenen Dokumente zurück. Dieser Wert wird oben im Bereich mit den Suchergebnissen angezeigt. Zur Überprüfung von Filterabfragen können Sie von **$count=true** gemeldete Änderungen überwachen. Niedrigere Zahlen weisen darauf hin, dass Ihr Filter funktioniert.

* **$top=100** gibt von allen Dokumenten die 100 Dokumente mit dem höchsten Rang zurück. Standardmäßig gibt Azure Search die ersten 50 der besten Treffer zurück. Die Menge kann mithilfe von **$top** erhöht oder verringert werden.

### <a name="filter-query"></a> Filtern der Abfrage

Filter werden in Suchanfragen eingefügt, wenn Sie den Parameter **$filter** anfügen. 

#### <a name="example-filtered-searchseattlefilterbeds-gt-3"></a>Beispiel (gefiltert): `search=seattle&$filter=beds gt 3`

* Der Parameter **$filter** gibt Ergebnisse zurück, die den angegebenen Kriterien entsprechen. In diesem Fall: mehr als drei Schlafzimmer.

* Bei der Filtersyntax handelt es sich um eine OData-Konstruktion. Weitere Informationen finden Sie unter [OData Expression Syntax for Azure Search](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) (OData-Ausdruckssyntax für Azure Search).

### <a name="facet-query"></a> Facettieren der Abfrage

Facettenfilter werden in Suchanfragen eingebunden. Sie können den Parameter „facet“ verwenden, um eine aggregierte Anzahl von Dokumenten zurückzugeben, die mit einem von Ihnen angegebenen Facetwert übereinstimmen.

#### <a name="example-faceted-with-scope-reduction-searchfacetcitytop2"></a>Beispiel (facettiert mit Verringerung des Umfangs): `search=*&facet=city&$top=2`

* **search=*** ist eine leere Suche. Bei einer leeren Suche wird alles durchsucht. Eine leere Abfrage kann beispielsweise übermittelt werden, um den vollständigen Satz von Dokumenten zu filtern oder zu facettieren – etwa, wenn eine Faceting-Navigationsstruktur alle Städte im Index enthalten soll.

* **facet** gibt eine Navigationsstruktur zurück, die Sie an ein Benutzeroberflächenelement übergeben können. Er gibt Kategorien und eine Anzahl zurück. In diesem Fall basieren die Kategorien auf der Anzahl von Städten. In Azure Search steht zwar keine Aggregation zur Verfügung, mit `facet` lässt sich jedoch eine Art Aggregation erreichen, die eine Anzahl von Dokumenten in den einzelnen Kategorien liefert.

* **$top=2** gibt zwei Dokumente zurück und zeigt, dass Sie mithilfe von `top` die Ergebnisse sowohl verringern als auch erhöhen können.

#### <a name="example-facet-on-numeric-values-searchseattlefacetbeds"></a>Beispiel (Facet für numerische Werte): `search=seattle&facet=beds`**

* Bei dieser Abfrage handelt es sich um ein Facet für Betten bei einer Textsuche nach *Seattle*. Der Ausdruck *beds* kann als Facet angegeben werden, da das Feld im Index als filterbar und facettierbar markiert ist und die enthaltenen Werte (numerische Werte von 1 bis 5) für die Kategorisierung von Angeboten in Gruppen (Angebote mit drei Schlafzimmern, vier Schlafzimmern...) geeignet ist.

* Nur filterbare Felder können facettiert werden. In den Ergebnissen können nur abrufbare Felder zurückgegeben werden.

### <a name="highlight-query"></a> Hervorheben von Suchergebnissen

Bei Treffermarkierungen wird Text, der dem Schlüsselwort entspricht, mit einer Formatierung versehen (sofern Treffer in einem bestimmten Feld gefunden werden). Falls Ihr Suchbegriff Teil einer umfangreicheren Beschreibung ist, können Sie ihn mithilfe einer Treffermarkierung hervorheben.

#### <a name="example-highlighter-searchgranite-countertopshighlightdescription"></a>Beispiel (Textmarker): `search=granite countertops&highlight=description`

* In diesem Beispiel ist der formatierte Ausdruck *granite countertops* im Beschreibungsfeld leichter zu erkennen.

#### <a name="example-linguistic-analysis-searchmicehighlightdescription"></a>Beispiel (linguistische Analyse): `search=mice&highlight=description`

* Die Volltextsuche sucht nach Wortformen mit ähnlicher Semantik. In diesem Fall wird bei Häusern mit einer Mäuseplage in den Ergebnissen einer Suche nach dem Schlüsselwort „mice“ der Text „mouse“ hervorgehoben. Dank linguistischer Analyse können in den Ergebnissen unterschiedliche Formen des gleichen Worts erscheinen.

* Azure Search unterstützt 56 Analysen von Lucene und Microsoft. Standardmäßig verwendet Azure Search die Standardanalyse von Lucene.

### <a name="fuzzy-search"></a> Ausprobieren der Fuzzysuche

Für falsch geschriebene Wörter wie *samamish* für das Samammish-Plateau in der Region Seattle werden bei einer typischen Suche standardmäßig keine Treffer zurückgegeben. Im folgenden Beispiel werden keine Ergebnisse zurückgegeben.

#### <a name="example-misspelled-term-unhandled-searchsamamish"></a>Beispiel (falsch geschriebener Begriff, keine Verarbeitung): `search=samamish`

Sie können die Fuzzysuche verwenden, damit auch Begriffe mit Rechtschreibfehlern einbezogen werden. Die Fuzzysuche wird aktiviert, wenn Sie die vollständige Lucene-Abfragesyntax verwenden. Dies ist der Fall, wenn Sie zwei Schritte ausführen: Festlegen von **queryType=full** für die Abfrage und Anfügen von **~** an die Suchzeichenfolge.

#### <a name="example-misspelled-term-handled-searchsamamishquerytypefull"></a>Beispiel (falsch geschriebener Begriff, Verarbeitung): `search=samamish~&queryType=full`

In diesem Beispiel werden jetzt Dokumente zurückgegeben, die Treffer zu „Sammamish“ enthalten.

Ohne Angabe von **queryType** wird der standardmäßige Parser für einfache Abfragen verwendet. Der Parser für einfache Abfragen ist zwar schneller, Fuzzysuche, reguläre Ausdrücke, NEAR-Suche und andere erweiterte Abfragetypen stehen jedoch nur bei Verwendung der vollständigen Syntax zur Verfügung.

Die Fuzzysuche und die Platzhaltersuche haben Auswirkungen auf die Suchausgabe. Die linguistische Analyse wird für diese Abfrageformate nicht durchgeführt. Bevor Sie die Fuzzy- und Platzhaltersuche verwenden, sollten Sie sich unter [Funktionsweise der Volltextsuche in Azure Search – Phase 2: Lexikalische Analyse](search-lucene-query-architecture.md#stage-2-lexical-analysis) den Abschnitt zu den Ausnahmen bei der lexikalischen Analyse durchlesen.

Weitere Informationen zu möglichen Abfrageszenarien mit dem Parser für vollständige Abfragen finden Sie unter [Lucene query syntax in Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) (Lucene-Abfragesyntax in Azure Search).

### <a name="geo-search"></a> Ausprobieren der Geosuche

Die Geosuche wird über den [Datentyp „edm.GeographyPoint“](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) für ein Feld mit Koordinaten unterstützt. Die Geosuche ist ein Filtertyp und wird in der [OData-Ausdruckssyntax für Azure Search](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) angegeben.

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-122121513-47673988-le-5"></a>Beispiel (Geokoordinatenfilter): `search=*&$count=true&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5`

Die Beispielabfrage filtert alle Ergebnisse nach Positionsdaten und gibt Ergebnisse zurück, die weniger als fünf Kilometer von einem bestimmten Punkt (angegeben als Koordinaten mit Breiten-und Längengrad) entfernt sind. Durch Hinzufügen von **$count** sehen Sie, wie viele Ergebnisse zurückgegeben werden, wenn Sie die Entfernung oder die Koordinaten ändern.

Die Geosuche ist hilfreich, wenn Ihre Suchanwendung über eine Umgebungssuche oder Kartennavigation verfügt. Es ist allerdings keine Volltextsuche. Falls eine Suche nach Städte- oder Ländernamen möglich sein muss, fügen Sie zusätzlich zu den Koordinaten Felder mit Städte- oder Ländernamen hinzu.

## <a name="takeaways"></a>Wesentliche Punkte

Dieses Tutorial enthielt eine kurze Einführung in die Verwendung von Azure Search über das Azure-Portal.

Sie haben erfahren, wie Sie mithilfe des Assistenten **Daten importieren** einen Suchindex erstellen. Sie haben sich über [Indexer](search-indexer-overview.md) und über den grundlegenden Workflow für das Entwerfen von Indizes informiert, z.B. [unterstützte Änderungen an einem veröffentlichten Index](https://docs.microsoft.com/rest/api/searchservice/update-index).

Unter Verwendung des **Suchexplorers** im Azure-Portal haben Sie eine grundlegende Abfragesyntax anhand von praktischen Beispielen erlernt, und es wurden wichtige Funktionen wie Filter, Treffermarkierung, Fuzzysuche und die geografische Suche veranschaulicht.

Darüber hinaus haben Sie erfahren, wie Sie die Kacheln auf dem Portaldashboard für den Suchindex, den Indexer und Datenquellen verwenden. Wenn Sie zukünftig Datenquellen verwenden, können Sie schnell und mit minimalem Aufwand über das Portal ihre Definitionen oder Felderauflistungen überprüfen.

## <a name="clean-up"></a>Bereinigen

Wenn Sie Azure Search in diesem Tutorial zum ersten Mal verwendet haben, löschen Sie die Ressourcengruppe mit dem Azure Search-Dienst. Ist dies nicht das erste Mal, suchen Sie den entsprechenden Ressourcengruppennamen in der Liste der Dienste, und löschen Sie ihn.

## <a name="next-steps"></a>Nächste Schritte

Anhand der programmatischen Tools können Sie Azure Search genauer erkunden:

* [Erstellen eines Azure Search-Index mit dem .NET SDK](https://docs.microsoft.com/azure/search/search-create-index-dotnet)
* [Erstellen eines Azure Search-Indexes mit der REST-API](https://docs.microsoft.com/azure/search/search-create-index-rest-api)
* Verwenden eines [Webtesttools wie Postman oder Fiddler zum Aufrufen der Azure Search-REST-APIs](search-fiddler.md)

<!--Image references-->
[1]: ./media/search-get-started-portal/tiles-indexers-datasources2.png
[2]: ./media/search-get-started-portal/import-data-cmd2.png
[3]: ./media/search-get-started-portal/realestateindex2.png
[4]: ./media/search-get-started-portal/indexers-inprogress2.png
[5]: ./media/search-get-started-portal/search-explorer-cmd2.png
[6]: ./media/search-get-started-portal/search-explorer-changeindex-se2.png
[7]: ./media/search-get-started-portal/search-explorer-query2.png
[8]: ./media/search-get-started-portal/realestate-indexer2.png
[9]: ./media/search-get-started-portal/import-datasource-sample2.png
[10]: ./media/search-get-started-portal/sample-index-def.png
[11]: ./media/search-get-started-portal/skip-cog-skill-step.png