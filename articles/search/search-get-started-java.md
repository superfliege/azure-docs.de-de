---
title: Erste Schritte mit Azure Search in Java | Microsoft Docs
description: Erstellen eine gehosteten Cloud-Search-Anwendung in Azure mithilfe von Java als Ihre Programmiersprache.
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 8b4df3c9-3ae5-4e3a-b4bb-74b516a91c8e
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 07/14/2016
ms.author: evboyle
ms.openlocfilehash: f6ca06a0349def97b38a1bf6d0d8f36236077e92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-search-in-java"></a>Erste Schritte mit Azure Search in Java
> [!div class="op_single_selector"]
> * [Verwaltungsportal](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Erfahren Sie, wie eine benutzerdefinierte Java-Search-Anwendung zu erstellen, die Azure Search für die Suchabfrage verwendet. Dieses Lernprogramm verwendet den [Azure-Suchdienst-REST-API](https://msdn.microsoft.com/library/dn798935.aspx) So erstellen Sie die Objekte und Vorgänge, die in dieser Übung verwendet.

Um dieses Beispiel ausführen zu können, benötigen Sie einen Azure Search-Dienst, der Sie sich für, in signieren können der [Azure-Portal](https://portal.azure.com). Finden Sie unter [Erstellen eines Azure-Suchdiensts im Portal](search-create-service-portal.md) für-Schritt-Anweisungen.

Erstellen und Testen dieses Beispiel verwendet die folgende Software:

* [Eclipse IDE für Java EE-Entwickler](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Achten Sie darauf, dass die EE-Version herunterladen. Eines der Überprüfungsschritte erfordert eine Funktion, die nur in dieser Edition gefunden wird.
* [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>Zu den Daten
Diese beispielanwendung verwendet Daten aus der [United States geologischen Dienste (Clientberechtigungen)](http://geonames.usgs.gov/domestic/download_data.htm), gefilterter auf den Status der Rhode Island zum Verringern der Größe des Datasets. Wir verwenden diese Daten zu eine Search-Anwendung erstellen, die Kennzeichen Gebäude wie Krankenhäusern und Schulen sowie geologischen Funktionen wie Datenströme Seen und Summits zurückgibt.

In dieser Anwendung die **SearchServlet.java** Programm erstellt, und lädt den Index mit einer [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) Konstrukt, das gefilterte Clientberechtigungen Dataset aus einer öffentlichen Azure SQL-Datenbank abgerufen. Vordefinierte Anmeldeinformationen und Verbindungsinformationen für die online-Datenquelle werden in den Programmcode bereitgestellt. In Bezug auf Datenzugriff sind keine weiteren Konfigurationsschritte erforderlich.

> [!NOTE]
> Wir haben einen Filter auf dieses Dataset bleiben unter die kostenlose Preisstufe maximal 10.000 Dokument angewendet. Wenn Sie die standard-Ebene verwenden, dieser Grenzwert gilt nicht, und Sie können diesen Code aus, um ein größeres Dataset verwenden. Weitere Informationen über die Kapazität für jedes Tarif, finden Sie unter [Grenzen und Einschränkungen](search-limits-quotas-capacity.md).
> 
> 

## <a name="about-the-program-files"></a>Über die Programmdateien
Die folgende Liste beschreibt die Dateien, die für dieses Beispiel relevant sind.

* Search.jsp: Die Benutzeroberfläche bereit.
* SearchServlet.java: Bietet Methoden (vergleichbar mit einem Domänencontroller in MVC)
* SearchServiceClient.java: Handles HTTP-Anforderungen
* SearchServiceHelper.java: Eine Hilfsklasse, die statische Methoden bereitstellt.
* Document.Java: Stellt das Datenmodell bereit.
* config.Properties: Legt die Suchdienst-URL und die api-Schlüssel
* Pom.XML: Eine Maven-Abhängigkeit

<a id="sub-2"></a>

## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Suchen Sie den Dienstnamen und den api-Schlüssel von Azure-Suchdienst
Alle REST-API-Aufrufe in Azure Search erfordern, dass Sie die Dienst-URL und ein api-Schlüssel angeben. 

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).
2. Klicken Sie in der Menüleiste Sprung auf **Search-Dienst** zum Auflisten aller Azure Search-Dienste für Ihr Abonnement bereitgestellt.
3. Wählen Sie den Dienst, die, den Sie verwenden möchten.
4. Im dienstdashboard sehen Sie Kacheln, die wichtige Informationen sowie das Schlüsselsymbol für den Zugriff auf die Administratorschlüssel.
   
      ![][3]
5. Kopieren Sie die Dienst-URL und ein Administratorschlüssel. Sie benötigen diese Angaben später, wenn Sie sie zum Hinzufügen der **config.properties** Datei.

## <a name="download-the-sample-files"></a>Herunterladen der Beispieldateien
1. Wechseln Sie zu [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) auf GitHub.
2. Klicken Sie auf **Download ZIP**, speichern Sie die ZIP-Datei auf dem Datenträger und alle darin enthaltenen Dateien extrahieren. Erwägen Sie, extrahieren die Dateien in Ihrem Java-Arbeitsbereich erleichtern das Projekt später zu finden.
3. Die Beispieldateien sind schreibgeschützt. Mit der rechten Maustaste Ordnereigenschaften, und deaktivieren Sie den nur-Lese Attribut.

Alle nachfolgenden Änderungen an und führen Anweisungen werden für Dateien in diesem Ordner vorgenommen werden.  

## <a name="import-project"></a>Import-Projekt
1. Wählen Sie in Eclipse **Datei** > **Import** > **allgemeine** > **vorhandene Projekte in Arbeitsbereich**.
   
    ![][4]
2. In **Select Stammverzeichnis**, navigieren Sie zu dem Ordner mit der Beispieldateien. Wählen Sie den Ordner, der die .Projekt-Ordner enthält. Das Projekt sollte nun in der **Projekte** Liste als ein ausgewähltes Element.
   
    ![][12]
3. Klicken Sie auf **Fertig stellen**.
4. Verwendung **Projektexplorer** anzeigen und bearbeiten Sie die Dateien. Wenn es nicht bereits geöffnet ist, klicken Sie auf **Fenster** > **Ansicht anzeigen** > **Projektexplorer** oder verwenden Sie die Verknüpfung, um ihn zu öffnen.

## <a name="configure-the-service-url-and-api-key"></a>Konfigurieren Sie die Dienst-URL und die api-Schlüssel
1. In **Projektexplorer**, doppelklicken Sie auf **config.properties** so bearbeiten Sie die Konfigurationseinstellungen, die den Servernamen und den api-Schlüssel enthält.
2. Verweisen Sie mit den Schritten weiter oben in diesem Artikel, in denen Sie, die Dienst-URL und die api-Schlüssel in gefunden der [Azure-Portal](https://portal.azure.com), um die Werte zu erhalten, Sie nun in geben **config.properties**.
3. In **config.properties**, ersetzen Sie "API-Schlüssel" mit der api-Schlüssel für Ihren Dienst. Dienstname (die erste Komponente der URL-http://servicename.search.windows.net) wird als Nächstes "Dienstname" in der gleichen Datei ersetzt.
   
    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Konfigurieren Sie das Projekt, Build- und Laufzeitumgebungen
1. In Eclipse, klicken Sie im Projekt-Explorer mit der Maustaste des Projekts > **Eigenschaften** > **Projekt Facets**.
2. Wählen Sie **dynamische Webmodul**, **Java**, und **JavaScript**.
   
    ![][6]
3. Klicken Sie auf **Anwenden**.
4. Select **Window** > **Preferences** > **Server** > **Runtime Environments** > **Add..** .
5. Erweitern Sie Apache, und wählen Sie die Version des Apache Tomcat-Servers ein, die Sie zuvor installiert haben. In unserem System installiert wir Version 8.
   
    ![][7]
6. Geben Sie auf der nächsten Seite der Tomcat-Installationsverzeichnis. Auf einem Windows-Computer wahrscheinlich werden c:\Programme\Microsoft Files\Apache Software Foundation\Tomcat *Version*.
7. Klicken Sie auf **Fertig stellen**.
8. Select **Window** > **Preferences** > **Java** > **Installed JREs** > **Add**.
9. In **JRE hinzufügen**Option **Standard-VM**.
10. Klicken Sie auf **Weiter**.
11. Klicken Sie in der Definition in JRE home, JRE auf **Directory**.
12. Navigieren Sie zu **Programmdateien** > **Java** , und wählen Sie das JDK, die Sie zuvor installiert haben. Es ist wichtig, um für das JDK JRE auszuwählen.
13. Wählen Sie im JREs installiert die **JDK**. Ihre Einstellungen sollte ähnlich wie im folgenden Bildschirmfoto aussehen.
    
    ![][9]
14. Wählen Sie optional **Fenster** > **Webbrowser** > **Internet Explorer** um die Anwendung in einem externen Browser-Fenster zu öffnen. Mit einem externen Browser bietet Ihnen eine Web-Anwendung optimal.
    
    ![][8]

Sie haben jetzt die Konfigurationsaufgaben abgeschlossen. Als Nächstes müssen Sie erstellen und Ausführen des Projekts.

## <a name="build-the-project"></a>Erstellen Sie das Projekt
1. Klicken Sie im Projekt-Explorer mit der rechten Maustaste des Projektnamens, und wählen Sie **ausführende** > **Maven-Builds...**  so konfigurieren Sie das Projekt.
   
    ![][10]
2. Geben Sie im-Konfiguration bearbeiten, in den Zielen "Neuinstallation", und klicken Sie dann auf **ausführen**.

Statusmeldungen werden in das Konsolenfenster ausgegeben. BUILDERFOLG, der angibt, des Projekts ohne Fehler erstellt, sollte angezeigt werden.

## <a name="run-the-app"></a>Führen Sie die app
In diesem letzten Schritt wird die Anwendung in einer Laufzeitumgebung für den lokalen Server ausgeführt werden.

Wenn Sie in Eclipse noch eine Laufzeitumgebung für die Server angegeben haben, müssen Sie als erstes führen.

1. Erweitern Sie im Projektexplorer **WebContent**.
2. Mit der rechten Maustaste **Search.jsp** > **ausführende** > **führen Sie auf Server**. Wählen Sie den Apache Tomcat-Server, und klicken Sie dann auf **ausführen**.

> [!TIP]
> Wenn Sie einen nicht standardmäßigen Arbeitsbereich verwendet, um das Projekt zu speichern, müssen Sie ändern **Testlaufkonfiguration** , zeigen Sie in den Projektspeicherort auf einen Server-Start-Fehler zu vermeiden. Im Projekt-Explorer mit der Maustaste **Search.jsp** > **ausführende** > **Testlaufkonfigurationen**. Wählen Sie den Apache Tomcat-Server. Klicken Sie auf **Argumente**. Klicken Sie auf **Arbeitsbereich** oder **Dateisystem** festzulegende Ordner, der das Projekt enthält.
> 
> 

Wenn Sie die Anwendung ausführen, sehen Sie ein Browserfenster, bietet ein Suchfeld zum Eingeben von Begriffe.

Warten Sie ca. eine Minute, bevor Sie auf **Suche** erteilen die Dienstzeit erstellen und den Index laden. Wenn Sie einen HTTP 404-Fehler erhalten, müssen Sie nur etwas mehr gewartet wird, bevor Sie es erneut zu versuchen.

## <a name="search-on-usgs-data"></a>Für Clientberechtigungen Daten suchen
Das DataSet Clientberechtigungen enthält Datensätze, die auf den Status der Rhode Island relevant sind. Wenn Sie auf **Suche** auf eine leere Suchfeld ein, erhalten Sie die 50 wichtigsten-Einträge, dies ist die Standardeinstellung.

Einen Suchbegriff eingeben erhalten dem Suchmodul etwas auf Gehe zu. Versuchen Sie, einen regionalen Namen einzugeben. "Roger Williams" wurde die erste Ressourcenkontrolle von Rhode Island. Zahlreiche Park, Gebäude und Schulen werden nach ihm benannt.

![][11]

Sie können auch einen dieser Begriffe beheben:

* Pawtucket
* Pembroke
* Goose + Kap

## <a name="next-steps"></a>Nächste Schritte
Dies ist der ersten Azure Search-Lernprogramm, das basierend auf Java und das Dataset Clientberechtigungen. Im Laufe der Zeit verlängern wir dieses Tutorial aus, um zusätzliche Suchfunktionen zu veranschaulicht, die Sie in Ihren benutzerdefinierten Lösungen verwenden möchten.

Wenn Sie bereits einige Hintergrund in Azure Search haben, können Sie dieses Beispiel als Auslöser für weitere Experimente vielleicht erweitern die [Suchseite](search-pagination-page-layout.md), oder implementieren [facettennavigation](search-faceted-navigation.md). Sie können auch auf der Seite der Suchergebnisse verbessern, durch Hinzufügen von Anzahlen und Batchverarbeitung von Dokumenten, sodass Benutzer die Ergebnisse seitenweise können.

Neu bei Azure-Suchdienst? Es wird empfohlen, versucht der anderen Lernprogramme zum Entwickeln Sie ein Verständnis dafür, was Sie erstellen können. Besuchen Sie unsere [Dokumentationsseite](https://azure.microsoft.com/documentation/services/search/) auf Weitere Informationsressourcen gegeben. Sie können auch anzeigen, die Links in unserer [Video und Lernprogramm Liste](search-video-demo-tutorial-list.md) Zugriff auf Weitere Informationen.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
