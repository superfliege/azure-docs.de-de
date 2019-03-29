---
title: Konfigurieren von Apps – Azure App Service
description: Konfigurieren einer App in Azure App Service
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: fb8dedac8b795ec127d7b4a14728d73c9397a1dd
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2019
ms.locfileid: "56807404"
---
# <a name="configure-apps-in-azure-app-service"></a>Konfigurieren von Apps in Azure App Service

In diesem Thema wird erläutert, wie Sie eine Web-App, ein mobiles Back-End oder eine API-App über das [Azure-Portal] konfigurieren.

## <a name="application-settings"></a>Anwendungseinstellungen
1. Öffnen Sie im [Azure-Portal] das Blatt für die App.
2. Klicken Sie auf **Anwendungseinstellungen**.

![Anwendungseinstellungen][configure01]

Das Blatt **Anwendungseinstellungen** enthält Einstellungen, die unter verschiedenen Kategorien gruppiert sind.

### <a name="general-settings"></a>Allgemeine Einstellungen
**Frameworkversionen**. Legen Sie diese Optionen fest, falls Ihre App von einem der folgenden Frameworks Gebrauch macht: 

* **.NET Framework**: Legen Sie die .NET Framework-Version fest. 
* **PHP**: Legen Sie die PHP-Version fest, oder wählen Sie **AUS**, um PHP zu deaktivieren. 
* **Java**: Wählen Sie die Java-Version oder **AUS**, um Java zu deaktivieren. Über die Option **Webcontainer** können Sie zwischen Tomcat und Jetty wählen.
* **Python**: Legen Sie die Python-Version fest, oder wählen Sie **AUS**, um Python zu deaktivieren.

Aus technischen Gründen werden durch Aktivierung von Java für Ihre App die Optionen für .NET, PHP und Python deaktiviert.

<a name="platform"></a>
**Plattform**. Wählt aus, ob Ihre App in einer 32-Bit- oder 64-Bit-Umgebung ausgeführt wird. Für die 64-Bit-Umgebung ist die Ebene „Basic“ oder „Standard“ erforderlich. Die Ebenen „Kostenlos“ und „Freigegeben“ werden immer in einer 32-Bit-Umgebung ausgeführt.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

**Websockets**. Legen Sie **EIN** fest, um das WebSocket-Protokoll zu aktivieren, beispielsweise wenn Ihre App [ASP.NET SignalR] oder [socket.io](https://socket.io/) verwendet.

<a name="alwayson"></a>
**Always On**. Standardmäßig werden Apps entladen, wenn sie sich einige Zeit im Leerlauf befunden haben. Dadurch spart das System Ressourcen. Im Modus „Basic“ oder „Standard“ können Sie **Always On** aktivieren, sodass die Web-App permanent geladen bleibt. Wenn Ihre App fortlaufende WebJobs oder unter Verwendung eines CRON-Ausdrucks ausgelöste WebJobs ausführt, sollten Sie **Always On** aktivieren. Ansonsten werden die Webaufträge möglicherweise nicht zuverlässig ausgeführt.

**Verwalteter Pipelinemodus**. Legt den IIS- [Pipelinemodus]fest. Lassen Sie diese Einstellung bei "Integriert" (der Standard), sofern Sie nicht eine ältere App haben, die eine ältere IIS-Version erfordert.

**HTTP-Version**. Legen Sie die Einstellung auf **2.0** fest, um die Unterstützung für das [HTTPS/2](https://wikipedia.org/wiki/HTTP/2)-Protokoll zu aktivieren. 

> [!NOTE]
> Die aktuellen Browser unterstützen das HTTP/2-Protokoll in der Regel nur über TLS, während unverschlüsselter Datenverkehr weiterhin HTTP/1.1 verwendet. Um sicherzustellen, dass sich Clientbrowser mit Ihrer App über HTTP/2 verbinden, erwerben Sie entweder [ein App Service Certificate](web-sites-purchase-ssl-web-site.md) für die benutzerdefinierte Domäne Ihrer App oder [binden Sie ein Zertifikat eines Drittanbieters](app-service-web-tutorial-custom-ssl.md).

**ARR-Affinität**. In einer App, die auf mehrere VM-Instanzen skaliert ist, garantieren ARR Affinität-Cookies, dass der Client für die Dauer der Sitzung an dieselbe Instanz weitergeleitet wird. Um die Leistung zustandsloser Anwendungen zu verbessern, setzen Sie diese Option auf **Aus**.   

**Automatisch tauschen**. Wenn Sie Auto Swap für einen Bereitstellungsslot aktivieren, ändert App Service den Status der App automatisch in „Produktion“, wenn Sie ein Update an diesen Slot übermitteln. Weitere Informationen finden Sie unter [Bereitstellen von Apps in Azure App Service in Stagingslots](deploy-staging-slots.md).

### <a name="debugging"></a>Debuggen
**Remotedebuggen**. Aktiviert das Remotedebugging. Wenn diese Option aktiviert ist, können Sie mit dem Remotedebugger in Visual Studio direkt eine Verbindung mit Ihrer App herstellen. Das Remotedebuggen bleibt 48 Stunden aktiviert. 

### <a name="app-settings"></a>App-Einstellungen
Dieser Abschnitt enthält Name-Wert-Paare, die Ihre App beim Start lädt. 

* Bei .NET-Apps werden diese Einstellungen zur Laufzeit in die `AppSettings` der .NET-Konfiguration eingefügt, wobei vorhandene Einstellungen überschrieben werden. 
* Wenn Sie bei App Service unter Linux oder Web-App für Container eine verschachtelte JSON-Schlüsselstruktur im Namen verwenden, wie etwa `ApplicationInsights:InstrumentationKey`, müssen Sie `ApplicationInsights__InstrumentationKey` als Schlüsselnamen verwenden. Beachten Sie also, dass jeder `:` durch `__` (d.h. doppelten Unterstrich) ersetzt werden muss.
* PHP-, Python-, Java- und Node-Anwendungen können auf diese Einstellungen als Umgebungsvariablen zur Laufzeit zugreifen. Für jede App-Einstellung werden zwei Umgebungsvariablen erstellt: eine mit dem Namen, der für die App-Einstellung eingegeben wurde, und eine weitere mit dem Präfix APPSETTING_. Beide enthalten denselben Wert.

App-Einstellungen werden bei der Speicherung stets verschlüsselt (encrypted-at-rest).

App-Einstellungen können über Key Vault mithilfe von [Key Vault-Referenzen](app-service-key-vault-references.md) aufgelöst werden.

### <a name="connection-strings"></a>Verbindungszeichenfolgen
Verbindungszeichenfolgen für verknüpfte Ressourcen. 

Bei .NET-Apps werden diese Verbindungszeichenfolgen zur Laufzeit in die `connectionStrings` -Einstellungen der .NET-Konfiguration eingefügt, wobei vorhandene Einträge überschrieben werden, wenn der Schlüssel dem verknüpften Datenbanknamen entspricht. 

Bei PHP-, Python-, Java- und Node-Anwendungen sind diese Einstellungen als Umgebungsvariablen zur Laufzeit mit dem Verbindungstyp als Präfix verfügbar. Die Präfixe der Umgebungsvariable lauten wie folgt: 

* SQL Server: `SQLCONNSTR_`
* MySQL: `MYSQLCONNSTR_`
* SQL-Datenbank: `SQLAZURECONNSTR_`
* Benutzerdefiniert: `CUSTOMCONNSTR_`

Wenn beispielsweise eine MySQL-Verbindungszeichenfolge "`connectionstring1`" heißt, wird sie über die Umgebungsvariable `MYSQLCONNSTR_connectionString1` aufgerufen.

Verbindungszeichenfolgen werden bei der Speicherung stets verschlüsselt (encrypted-at-rest).

Verbindungszeichenfolgen können über Key Vault mithilfe von [Key Vault-Referenzen](app-service-key-vault-references.md) aufgelöst werden.

### <a name="default-documents"></a>Standarddokumente
Das Standarddokument ist die Webseite, die an der Stamm-URL für eine Website angezeigt wird.  Die erste übereinstimmende Datei in der Liste wird verwendet. 

Apps verwenden möglicherweise Module, die eine Weiterleitung anhand der URL ausführen und keinen statischen Inhalt bereitstellen. In diesem Fall gibt es kein Standarddokument.    

### <a name="handler-mappings"></a>Handlerzuordnungen
Fügen Sie in diesem Bereich benutzerdefinierte Skriptprozessoren hinzu, die Anforderungen für bestimmte Dateierweiterungen verarbeiten. 

* **Erweiterung**. Die zu verarbeitende Dateierweiterung wie *.php oder handler.fcgi. 
* **Skriptprozessorpfad**. Der absolute Pfad des Skriptprozessors. Anforderungen für Dateien, die dieser Dateierweiterung entsprechen, werden vom Skriptprozessor verarbeitet. Verwenden Sie den Pfad `D:\home\site\wwwroot` , um auf das Stammverzeichnis Ihrer App zu verweisen.
* **Zusätzliche Argumente**. Optionale Befehlszeilenargumente für den Skriptprozessor. 

### <a name="virtual-applications-and-directories"></a>Virtuelle Anwendungen und Verzeichnisse
Um die virtuellen Anwendungen und Verzeichnisse zu konfigurieren, geben Sie jedes virtuelle Verzeichnis und den zugehörigen physischen Pfad relativ zum Stammverzeichnis der Website an. Sie haben außerdem die Möglichkeit, mit dem Kontrollkästchen **Anwendung** ein virtuelles Verzeichnis als Anwendung zu markieren.

## <a name="enabling-diagnostic-logs"></a>Aktivieren von Diagnoseprotokollen
So aktivieren Sie Diagnoseprotokolle

1. Klicken Sie auf dem Blatt für die App auf **Alle Einstellungen**.
2. Klicken Sie auf **Diagnoseprotokolle**. 

Optionen zum Erstellen von Diagnoseprotokollen über eine Webanwendung, die die Protokollierung unterstützt: 

* **Anwendungsprotokollierung**. Schreibt Anwendungsprotokolle in das Dateisystem. Die Protokollierung erfolgt über zwölf Stunden. 

**Ebene**. Wenn die Anwendungsprotokollierung aktiviert ist, bestimmt diese Option, ob Fehler, Warnmeldungen, Infos oder ausführliche Informationen protokolliert werden.

**Webserverprotokollierung**. Protokolle werden im erweiterten W3C-Protokolldateiformat gespeichert. 

**Detaillierte Fehlermeldungen**. Speichert detaillierte Fehlermeldungen in HTM-Dateien. Die Dateien werden unter "/LogFiles/DetailedErrors" gespeichert. 

**Verfolgung fehlgeschlagener Anforderungen**. Protokolliert fehlgeschlagene Anforderungen in XML-Dateien. Die Dateien werden unter "/LogFiles/W3SVC*xxx*" gespeichert, wobei xxx ein eindeutiger Bezeichner ist. Dieser Ordner enthält eine XSL-Datei und eine oder mehrere XML-Dateien. Stellen Sie sicher, dass Sie die XSL-Datei herunterladen, da sie die Funktionalität zum Formatieren und Filtern des Inhalts der XML-Dateien zur Verfügung stellt.

Zum Anzeigen der Protokolldateien müssen Sie die FTP-Anmeldeinformationen wie folgt bereitstellen:

1. Klicken Sie auf dem Blatt für die App auf **Alle Einstellungen**.
2. Klicken Sie auf **Anmeldeinformationen für die Bereitstellung**.
3. Geben Sie einen Benutzernamen und ein Kennwort ein.
4. Klicken Sie auf **Speichern**.

![Anmeldeinformationen für die Bereitstellung zurücksetzen][configure03]

Der vollständige FTP-Benutzername lautet „App\Benutzername“. Dabei ist *App* der Name Ihrer App. Den Benutzernamen finden Sie auf dem Blatt der App unter **Zusammenfassung**.

![FTP-Anmeldeinformationen für die Bereitstellung][configure02]

## <a name="other-configuration-tasks"></a>Weitere Konfigurationsaufgaben
### <a name="ssl"></a>SSL
Im Modus "Basic" oder "Standard" können Sie SSL-Zertifikate für benutzerdefinierte Domänen hochladen. Weitere Informationen finden Sie unter [Aktivieren von HTTPS für eine App](app-service-web-tutorial-custom-ssl.md). 

Klicken Sie zum Anzeigen der hochgeladenen Zertifikate auf **Alle Einstellungen** > **Benutzerdefinierte Domänen und SSL**konfigurieren.

### <a name="domain-names"></a>Domänennamen
Fügen Sie Ihrer App benutzerdefinierte Domänennamen hinzu. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für eine App in Azure App Service](app-service-web-tutorial-custom-domain.md).

Klicken Sie zum Anzeigen des Domänennamens auf **Alle Einstellungen** > **Benutzerdefinierte Domänen und SSL**konfigurieren.

### <a name="deployments"></a>Bereitstellungen
* Legen Sie die kontinuierliche Bereitstellung fest. Weitere Informationen finden Sie unter [Bereitstellen von Apps in Azure App Service mit Git](deploy-local-git.md).
* Bereitstellungsslots. Weitere Informationen finden Sie unter [Bereitstellen in Stagingumgebungen für Azure App Service].

Klicken Sie zum Anzeigen der Bereitstellungsslots auf **Alle Einstellungen** > **Bereitstellungsslots**.

### <a name="monitoring"></a>Überwachung
Im Modus „Basic“ oder „Standard“ können Sie die Verfügbarkeit von HTTP- oder HTTPS-Endpunkten von bis zu drei geografisch verteilten Standorten aus testen. Ein Überwachungstest verläuft nicht erfolgreich, wenn der HTTP-Antwortcode ein Fehler ist (4xx oder 5xx) oder die Antwort länger als 30 Sekunden benötigt. Der Endpunkt wird als verfügbar betrachtet, wenn die Überwachungstests von allen angegebenen Standorten aus erfolgreich waren. 

Weitere Informationen finden Sie unter [Gewusst wie: Überwachen des Web-Endpunktstatus].

## <a name="next-steps"></a>Nächste Schritte
* [Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service]
* [Aktivieren von HTTPS für eine App in Azure App Service]
* [Skalieren einer App in Azure App Service]
* [Grundlagen der Überwachung in Azure App Service]
* [Ändern der „applicationHost.config“-Einstellungen mit „applicationHost.xdt“](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)

<!-- URL List -->

[ASP.NET SignalR]: https://www.asp.net/signalr
[Azure-Portal]: https://portal.azure.com/
[Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service]: ./app-service-web-tutorial-custom-domain.md
[Bereitstellen in Stagingumgebungen für Azure App Service]: ./deploy-staging-slots.md
[Aktivieren von HTTPS für eine App in Azure App Service]: ./app-service-web-tutorial-custom-ssl.md
[Gewusst wie: Überwachen des Web-Endpunktstatus]: https://go.microsoft.com/fwLink/?LinkID=279906
[Grundlagen der Überwachung in Azure App Service]: ./web-sites-monitor.md
[Pipelinemodus]: https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Skalieren einer App in Azure App Service]: ./web-sites-scale.md

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
