---
title: Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service
description: Erfahren Sie, wie Sie die Diagnoseprotokollierung aktivieren und Instrumentierung zu Ihrer Anwendung hinzufügen und wie Sie auf die von Azure protokollierten Informationen zugreifen.
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.openlocfilehash: 8a58f8722b41944a7be02254e0f00682575c1bbb
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636958"
---
# <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a>Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service
## <a name="overview"></a>Übersicht
Azure bietet integrierte Diagnosefunktionen zur Unterstützung beim Debuggen einer [App Service-Web-App](https://go.microsoft.com/fwlink/?LinkId=529714). In diesem Artikel erfahren Sie, wie Sie die Diagnoseprotokollierung aktivieren und Ihrer Anwendung Instrumentierung hinzufügen und wie Sie die von Azure protokollierten Informationen abrufen.

In diesem Artikel werden das [Azure-Portal](https://portal.azure.com) und die Azure-Befehlszeilenschnittstelle (Azure CLI) verwendet, um mit Diagnoseprotokollen zu arbeiten. Informationen zum Arbeiten mit Diagnoseprotokollen in Visual Studio finden Sie unter [Problembehandlung von Azure in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Webserver- und Anwendungsdiagnose
App Service-Web-Apps bieten Diagnosefunktionen zum Protokollieren von Informationen sowohl über den Webserver als auch über die Webanwendung. Diese sind logisch in **Webserverdiagnose** und **Anwendungsdiagnose** unterteilt.

### <a name="web-server-diagnostics"></a>Webserverdiagnose
Sie können die folgenden Protokollarten aktivieren oder deaktivieren:

* **Detaillierte Fehlerprotokollierung** - Detaillierte Fehlerinformationen für HTTP-Statuscodes, die auf einen Fehler hinweisen (Statuscode 400 oder höher). Diese können Informationen enthalten, mit deren Hilfe sich bestimmen lässt, warum der Server den Fehlercode zurückgegeben hat.
* **Ablaufverfolgung von Anforderungsfehlern** - Detaillierte Informationen zu fehlgeschlagenen Anforderungen, einschließlich der Verfolgung von IIS-Komponenten, die zur Verarbeitung der Anforderung verwendet wurden, sowie die in jeder Komponente benötigte Zeit. Dies ist nützlich, wenn Sie versuchen, die Leistung von Websites zu verbessern oder herauszufinden, warum ein bestimmter HTTP-Fehler zurückgegeben wird.
* **Webserverprotokollierung** - Informationen über HTTP-Transaktionen im erweiterten [W3C-Protokolldateiformat](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Dies ist hilfreich, wenn Sie allgemeine Websitemetriken ermitteln möchten, wie die Anzahl der verarbeiteten Anforderungen oder die Anzahl der Anforderungen von einer bestimmten IP-Adresse.

### <a name="application-diagnostics"></a>Anwendungsdiagnose
Mit der Option "Application Diagnostics" können Sie die von einer Webanwendung erzeugten Informationen erfassen. ASP.NET-Anwendungen können die Klasse [System.Diagnostics.Trace](https://msdn.microsoft.com/library/36hhw2t6.aspx) verwenden, um Informationen im Anwendungs-Diagnoseprotokoll aufzuzeichnen. Beispiel: 

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

Sie können diese Protokolle zur Laufzeit abrufen und bei der Problembehandlung nutzen. Weitere Informationen finden Sie unter [Problembehandlung von Azure-Web-Apps in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

App Service-Web-Apps protokollieren auch Bereitstellungsinformationen, wenn Sie Inhalte in einer Web-App veröffentlichen. Dies erfolgt automatisch, und es gibt keine Konfigurationseinstellung für die Bereitstellungsprotokollierung. Anhand der Bereitstellungsprotokollierung können Sie bestimmen, warum eine Bereitstellung fehlgeschlagen ist. Wenn Sie beispielsweise ein benutzerdefiniertes Bereitstellungsskript verwenden, können Sie die Bereitstellungsprotokollierung verwenden, um festzustellen, warum das Skript fehlgeschlagen ist.

## <a name="enablediag"></a>Aktivieren der Diagnose
Zum Aktivieren der Diagnose im [Azure-Portal](https://portal.azure.com) navigieren Sie zur Seite für Ihre Web-App und klicken auf **Einstellungen > Diagnoseprotokolle**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Bereich „Protokolle“](./media/web-sites-enable-diagnostic-log/logspart.png)

Wenn Sie die **Anwendungsdiagnose** aktivieren, können Sie auch die **Ebene** auswählen. Diese Einstellung ermöglicht das Filtern der aufgezeichneten Informationen nach **Informationen**, **Warnungen** oder **Fehlern**. Sie können auch **Ausführlich** auswählen, um alle von der Anwendung erzeugten Informationen zu protokollieren.

> [!NOTE]
> Im Gegensatz zum Ändern der Datei "web.config" wird durch das Aktivieren der Anwendungsdiagnose oder durch das Ändern des Diagnoseprotokollumfangs nicht die Anwendungsdomäne neu gestartet, in der die Anwendung ausgeführt wird.
>
>

Unter **Anwendungsprotokollierung** können Sie zum Debuggen vorübergehend die Dateisystemoption aktivieren. Diese Option wird in 12 Stunden automatisch deaktiviert. Sie können auch die Blob Storage-Option aktivieren, um einen Blobcontainer auszuwählen, in den Protokolle geschrieben werden sollen.

Unter **Webserverprotokollierung** können Sie **Speicher** oder **Dateisystem** auswählen. Durch Auswahl von **storage** können Sie ein Speicherkonto festlegen und dann einen Blob-Container hinzufügen, in den die Protokolle geschrieben werden. 

Wenn Sie Protokolle im Dateisystem speichern, können Sie über FTP auf diese Dateien zugreifen oder die Dateien mithilfe der Azure CLI als ZIP-Archiv herunterladen.

Protokolle werden standardmäßig nicht automatisch gelöscht (ausgenommen bei der Option **Anwendungsprotokollierung (Dateisystem)**). Damit Protokolle automatisch gelöscht werden, legen Sie das Feld **Beibehaltungsdauer (Tage)** fest.

> [!NOTE]
> Wenn Sie [den Zugriffsschlüssel für Ihr Speicherkonto neu generieren](../storage/common/storage-create-storage-account.md), müssen Sie die jeweilige Protokollierungskonfiguration zur Verwendung der aktualisierten Schlüssel zurücksetzen. Gehen Sie dazu folgendermaßen vor:
>
> 1. Stellen Sie auf der Registerkarte **Konfigurieren** das jeweilige Protokollierungsfeature auf **Aus**. Speichern Sie die Einstellungen.
> 2. Aktivieren Sie die Protokollierung im Speicherkontoblob erneut. Speichern Sie die Einstellungen.
>
>

Sie können gleichzeitig eine beliebige Kombination aus Speicherung im Dateisystem und in Blob Storage aktivieren und individuelle Protokollierungsebenen konfigurieren. Sie können beispielsweise Fehler und Warnmeldungen als langfristige Protokollierungslösung im Blob-Speicher protokollieren, während die Dateisystemprotokollierung ausführlich erfolgt.

Beide Speicherorte bieten dieselben grundlegenden Informationen zu protokollierten Ereignissen. **Blob Storage**-Protokolle zeichnen jedoch im Vergleich zur Protokollierung im **Dateisystem** mehr Informationen auf, beispielsweise die Instanz-ID, die Thread-ID und einen ausführlicheren Zeitstempel (Taktformat).

> [!NOTE]
> In **Blob Storage** gespeicherte Informationen können nur über Storage-Clients oder Anwendungen aufgerufen werden, die direkt mit diesen Speichersystemen arbeiten können. Beispielsweise enthält Visual Studio 2013 einen Speicher-Explorer, mit dem Blob Storage erkundet werden kann, und HDInsight kann auf die in Blob Storage gespeicherten Daten zugreifen. Sie können auch mithilfe der [Azure SDKs](https://azure.microsoft.com/downloads/)eine Anwendung schreiben, die auf den Azure-Speicher zugreift.
>

## <a name="download"></a> Vorgehensweise: Herunterladen von Blobs
Im Dateisystem der Web-App gespeicherte Diagnoseinformationen können direkt über FTP aufgerufen werden. Sie können auch mithilfe der Azure CLI als ZIP-Archiv heruntergeladen werden.

Protokolle werden in der folgenden Verzeichnisstruktur gespeichert:

* **Anwendungsprotokolle** - /LogFiles/Application/. Dieser Ordner enthält eine oder mehrere Textdateien mit Informationen, die bei der Anwendungsprotokollierung erzeugt wurden.
* **Protokolle für fehlgeschlagene Anforderungen** - /LogFiles/W3SVC#########/. Dieser Ordner enthält eine XSL-Datei und eine oder mehrere XML-Dateien. Vergewissern Sie sich, dass Sie die XSL-Datei in dasselbe Verzeichnis wie die XML-Datei(en) herunterladen, da die XSL-Datei die Funktionalität zum Formatieren und Filtern des Inhalts der XML-Datei(en) zur Anzeige in Internet Explorer zur Verfügung stellt.
* **Detaillierte Fehlerprotokolle** - /LogFiles/DetailedErrors/. Dieser Ordner enthält eine oder mehrere HTM-Dateien, die umfangreiche Informationen zu aufgetretenen HTTP-Fehlern bereitstellen.
* **Webserverprotokolle** - /LogFiles/http/RawLogs. Dieser Ordner enthält eine oder mehrere Textdateien im [erweiterten W3C-Protokolldateiformat](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Bereitstellungsprotokolle** - /LogFiles/Git. Dieser Ordner enthält Protokolle, die von den internen Bereitstellungsprozessen der Azure-Web-Apps erzeugt werden, sowie Protokolle für Git-Bereitstellungen. Bereitstellungsprotokolle können sich auch unter „D:\home\site\deployments“ befinden.

### <a name="ftp"></a>FTP

Informationen zum Öffnen einer FTP-Verbindung mit dem FTP-Server Ihrer App finden Sie unter [Bereitstellen der App in Azure App Service mithilfe von FTP/S](app-service-deploy-ftp.md).

Wenn die Verbindung mit dem FTP/S-Server Ihrer Web-App hergestellt wurde, öffnen Sie den Ordner **LogFiles**, in dem die Protokolldateien gespeichert sind.

### <a name="download-with-azure-cli"></a>Herunterladen mit der Azure-CLI
Zum Herunterladen der Protokolldateien mit der Azure-Befehlszeilenschnittstelle öffnen Sie eine neue Eingabeaufforderung, PowerShell-, Bash- oder Terminalsitzung, und geben Sie den folgenden Befehl ein:

    az webapp log download --resource-group resourcegroupname --name webappname

Mit diesem Befehl werden die Protokolle für die Web-App mit dem Namen „webappname“ in der Datei **diagnostics.zip** im aktuellen Verzeichnis gespeichert.

> [!NOTE]
> Wenn Sie die Azure CLI nicht installiert oder nicht zur Verwendung Ihres Azure-Abonnements konfiguriert haben, gehen Sie wie unter [Verwenden der Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) beschrieben vor.
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Vorgehensweise: Anzeigen von Protokollen in Application Insights
Visual Studio Application Insights bietet Tools zum Filtern und Suchen von Protokollen sowie zu Korrelation der Protokolle mit Anforderungen und anderen Ereignissen.

1. Fügen Sie Ihrem Projekt in Visual Studio Application Insights SDK hinzu.
   * Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie „Application Insights hinzufügen“ aus. Auf der Benutzeroberfläche werden Sie durch Schritte geführt, zu denen auch die Erstellung einer Application Insights-Ressource gehört. [Weitere Informationen](../application-insights/app-insights-asp-net.md)
2. Fügen Sie Ihrem Projekt das Ablaufverfolgungs-Listener-Paket hinzu.
   * Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie „NuGet-Pakete verwalten“. Falls die Protokolle automatisch gelöscht werden sollen, wählen Sie `Microsoft.ApplicationInsights.TraceListener` [Weitere Informationen](../application-insights/app-insights-asp-net-trace-logs.md)
3. Laden Sie das Projekt hoch, und führen sie es zum Generieren von Protokolldaten aus.
4. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu der neuen Application Insights-Ressource, und öffnen Sie die Option **Suche**. Ihre Protokolldaten sollten zusammen mit der Anforderung, Nutzungsdaten und sonstigen Telemetriedaten angezeigt werden. Einige Telemetrie kann einige Minuten in Anspruch nehmen, bis sie angezeigt wird: Klicken Sie auf "Aktualisieren". [Weitere Informationen](../application-insights/app-insights-diagnostic-search.md)

[Erfahren Sie mehr über die Leistungsüberwachung mit Application Insights](../application-insights/app-insights-azure-web-apps.md)

## <a name="streamlogs"></a> Vorgehensweise: Streaming von Protokollen
Beim Entwickeln einer Anwendung ist es häufig nützlich, Protokollinformationen nahezu in Echtzeit zu sehen. Sie können Protokollierungsinformationen mithilfe der Azure CLI in Ihre Entwicklungsumgebung streamen.

> [!NOTE]
> Einige Protokolltypen puffern die Schreibvorgänge in die Protokolldatei, was zu Störereignissen im Stream führen kann. Beispielsweise kann ein Anwendungsprotokolleintrag für den Besuch einer Seite im Stream vor dem zugehörigen HTTP-Protokolleintrag für die Seitenanforderung angezeigt werden.
>
> [!NOTE]
> Beim Protokollstreaming werden auch Informationen gestreamt, die in eine Textdatei im Ordner **D:\\home\\LogFiles\\** geschrieben werden.
>
>

### <a name="streaming-with-azure-cli"></a>Streamen mit der Azure CLI
Zum Streaming der Protokollinformationen öffnen Sie eine neue Eingabeaufforderung, PowerShell, Bash- oder Terminalsitzung und geben den folgenden Befehl ein:

    az webapp log tail --name webappname --resource-group myResourceGroup

Mit diesem Befehl wird eine Verbindung zu der Web-App „webappname“ hergestellt, und das Streaming von Informationen in das Fenster wird gestartet, sobald Protokollereignisse in der Web-App auftreten. Alle Informationen, die in Dateien mit der Erweiterung TXT, LOG oder HTM im Verzeichnis „/LogFiles“ (d:/home/logfiles) geschrieben werden, werden an die lokale Konsole gestreamt.

Um bestimmte Ereignisse wie beispielsweise Fehler zu filtern, verwenden Sie den Parameter **--Filter** . Beispiel: 

    az webapp log tail --name webappname --resource-group myResourceGroup --filter Error

Um bestimmte Protokolltypen wie HTTP zu filtern, verwenden Sie den Parameter **--Path** . Beispiel: 

    az webapp log tail --name webappname --resource-group myResourceGroup --path http

> [!NOTE]
> Wenn Sie die Azure CLI nicht installiert oder nicht zur Verwendung Ihres Azure-Abonnements konfiguriert haben, gehen Sie wie unter [Verwenden der Azure CLI](../cli-install-nodejs.md) beschrieben vor.
>
>

## <a name="understandlogs"></a> Vorgehensweise: Verstehen von Diagnoseprotokollen
### <a name="application-diagnostics-logs"></a>Anwendungs-Diagnoseprotokolle
Die Anwendungsdiagnose speichert Informationen in einem spezifischen Format für .NET-Anwendungen, je nachdem, ob Sie Protokolle im Dateisystem oder in Blob Storage speichern. 

Die grundlegenden Daten, die gespeichert werden, sind für beide Speichertypen gleich: Datum und Uhrzeit des Ereignisses, Prozess-ID, die das Ereignis erzeugt hat, sowie Ereignistyp (Info, Warnung, Fehler) und Ereignismeldung. Die Verwendung des Dateisystems zum Speichern von Protokollen ist nützlich, wenn Sie zum Beheben eines Problems sofortigen Zugriff auf die Protokolle benötigen, da die Protokolldateien nahezu sofort aktualisiert werden. Blob Storage eignet sich zu Archivierungszwecken, da die Dateien zwischengespeichert und dann nach einem Zeitplan in den Speichercontainer geleert werden.

**Dateisystem**

Jede im Dateisystem protokollierte oder per Streaming empfangene Zeile hat das folgende Format:

    {Date}  PID[{process ID}] {event type/level} {message}

Beispielsweise würde ein Fehlerereignis in etwa wie im folgenden Beispiel aussehen:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

Bei der Protokollierung im Dateisystem erhalten Sie nur die grundlegendsten Informationen der drei verfügbaren Methoden, nämlich Zeit, Prozess-ID, Ereignisebene und Meldung.

**Blob Storage**

Bei der Protokollierung in einem Blob-Speicher werden die Daten im CSV-Format (durch Kommas getrennte Werte) gespeichert. Zusätzliche Felder werden protokolliert, um ausführlichere Informationen zum Ereignis bereitzustellen. Die folgenden Eigenschaften werden für jede Zeile in der CSV-Datei angegeben:

| Eigenschaftenname | Wert/Format |
| --- | --- |
| Datum |Datum und Zeit des Auftretens des Ereignisses |
| Ebene |Ereignisebene (z.B. Fehler, Warnung, Information) |
| ApplicationName |Der Name der Web-App |
| InstanceId |Instanz der Web-App, in der das Ereignis auftrat |
| EventTickCount |Datum und Zeit des Auftretens des Ereignisses im Tick-Format (höhere Präzision) |
| EventId |Die Ereignis-ID<p><p>Bei fehlender Angabe standardmäßig 0 |
| Pid |Prozess-ID |
| Tid |Thread-ID des Threads, der das Ereignis erzeugt hat |
| Message |Meldung zu den Ereignisdetails |

In einem Blob gespeicherte Daten sehen in etwa wie im folgenden Beispiel aus:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> Für ASP.NET Core erfolgt die Protokollierung mit dem Anbieter [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices), der zusätzliche Protokolldateien im Blobcontainer ablegt. Weitere Informationen finden Sie unter [Protokollierung in ASP.NET Core](/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#logging-in-azure).
>
>

### <a name="failed-request-traces"></a>Protokolle für fehlgeschlagene Anforderungen
Die Verfolgungsprotokolle für fehlerhafte Anforderungen werden in XML-Dateien mit dem Namen **fr######.xml** gespeichert. Um das Anzeigen der protokollierten Informationen zu erleichtern, wird ein XSL-Stylesheet namens **freb.xsl** im selben Verzeichnis wie die XML-Dateien bereitgestellt. Wenn Sie eine der XML-Dateien in Internet Explorer öffnen, wird das XSL-Stylesheet in Internet Explorer verwendet, um eine formatierte Ansicht mit den Ablaufverfolgungsinformationen ähnlich wie im folgenden Beispiel anzuzeigen:

![Anzeige fehlgeschlagener Anforderungen im Browser](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Detaillierte Fehlerprotokolle
Detaillierte Fehlerprotokolle sind HTML-Dokumente, die ausführlichere Informationen zu aufgetretenen HTTP-Fehlern bereitstellen. Da es sich um einfache HTML-Dokumente handelt, können sie in einem Webbrowser angezeigt werden.

### <a name="web-server-logs"></a>Webserverprotokolle
Webserverprotokolle werden im [erweiterten W3C-Protokolldateiformat](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx)gespeichert. Die Informationen können in einem Texteditor gelesen oder mit einem Dienstprogramm wie [Log Parser](https://go.microsoft.com/fwlink/?LinkId=246619)analysiert werden.

> [!NOTE]
> Die von den Azure-Web-Apps erzeugten Protokolle unterstützen nicht die Felder **s-computername**, **s-ip** oder **cs-version**.
>
>

## <a name="nextsteps"></a> Nächste Schritte
* [Überwachen von Web-Apps](web-sites-monitor.md)
* [Problembehandlung von Azure-Web-Apps in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md)
* [Analyse von Web-App-Protokollen in HDInsight](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
>
>
