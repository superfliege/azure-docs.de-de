---
title: Erstellen einer selbstgehosteten Integrationslaufzeit in Azure Data Factory | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie in Azure Data Factory eine selbstgehostete Integrationslaufzeit erstellen, mit der Data Factorys das Zugreifen auf Datenspeicher in einem privaten Netzwerk ermöglicht wird."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: abnarain
ms.openlocfilehash: 0fcc245369d90042066cbfc516a8c32db7272bd3
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="how-to-create-and-configure-self-hosted-integration-runtime"></a>Erstellen und Konfigurieren einer selbstgehosteten Integrationslaufzeit
Bei der Integrationslaufzeit (Integration Runtime, IR) handelt es sich um die Computeinfrastruktur, mit der Azure Data Factory Datenintegrationsfunktionen übergreifend für verschiedene Netzwerkumgebungen bereitstellt. Weitere Informationen zur Integrationslaufzeit finden Sie unter [Integration Runtime Overview](concepts-integration-runtime.md) (Übersicht über Integrationslaufzeit).

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 der Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 (GA) des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md) weiter.

Mit einer selbstgehosteten Integrationslaufzeit können Kopieraktivitäten zwischen einem Clouddatenspeicher und einem Datenspeicher im privaten Netzwerk ausgeführt und Transformationsaktivitäten für Computeressourcen in einem lokalen Netzwerk oder einem Azure Virtual Network verteilt werden. Für die Installation einer selbstgehosteten Integrationslaufzeit ist ein lokaler Computer oder ein virtueller Computer in einem privaten Netzwerk erforderlich.  

In diesem Dokument wird beschrieben, wie Sie die selbstgehostete Integrationslaufzeit erstellen und konfigurieren können.

## <a name="high-level-steps-to-install-self-hosted-ir"></a>Allgemeine Schritte zum Installieren der selbstgehosteten Integrationslaufzeit
1.  Erstellen Sie eine selbstgehostete Integrationslaufzeit. Hier ist ein PowerShell-Beispiel angegeben:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
    ```
2.  Laden Sie die selbstgehostete Integrationslaufzeit herunter, und installieren Sie sie (auf dem lokalen Computer).
3.  Rufen Sie den Authentifizierungsschlüssel ab, und registrieren Sie die selbstgehostete Integrationslaufzeit mit dem Schlüssel. Hier ist ein PowerShell-Beispiel angegeben:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime.  
    ```

## <a name="command-flow-and-data-flow"></a>Befehls- und Datenfluss
Wenn Sie die Daten zwischen der lokalen Umgebung und der Cloud verschieben, wird für die Aktivität eine selbstgehostete Integrationslaufzeit verwendet, um die Daten aus der lokalen Datenquelle in die Cloud zu übertragen (und umgekehrt).

Hier ist ein allgemeiner Datenfluss als Zusammenfassung der Schritte zum Kopieren per selbstgehosteter Integrationslaufzeit angegeben:

![Allgemeine Übersicht](media\create-self-hosted-integration-runtime\high-level-overview.png)

1. Der Datenentwickler erstellt eine selbstgehostete Integrationslaufzeit in einer Azure Data Factory per PowerShell-Cmdlet. Derzeit wird dieses Feature vom Azure-Portal nicht unterstützt.
2. Der Datenentwickler erstellt einen verknüpften Dienst für einen lokalen Datenspeicher, indem er die Instanz der selbstgehosteten Integrationslaufzeit angibt, die zum Verbinden der Datenspeicher verwendet werden soll. Beim Einrichten des verknüpften Diensts verwendet der Datenentwickler die Anwendung „Anmeldeinformationsverwaltung“ (derzeit nicht unterstützt), um die Authentifizierungstypen und Anmeldeinformationen festzulegen. Das Dialogfeld der Anwendung „Anmeldeinformationsverwaltung“ kommuniziert mit dem Datenspeicher, um die Verbindung zu testen, und mit der selbstgehosteten Integrationslaufzeit, um Anmeldeinformationen zu speichern.
4.  Über den Knoten der selbstgehosteten Integrationslaufzeit werden die Anmeldeinformationen per DPAPI (Windows Data Protection Application Programming Interface) verschlüsselt und lokal gespeichert. Falls mehrere Knoten festgelegt sind, um Hochverfügbarkeit zu erzielen, werden die Anmeldeinformationen für andere Knoten weiter synchronisiert. Jeder Knoten verwendet DPAPI für die Verschlüsselung und speichert die Daten lokal. Die Synchronisierung der Anmeldeinformationen ist für den Datenentwickler transparent und wird von der selbstgehosteten Integrationslaufzeit verarbeitet.    
5.  Der Data Factory-Dienst kommuniziert für die Planung und Verwaltung von Aufträgen mit der selbstgehosteten Integrationslaufzeit. Hierfür wird ein **Steuerungskanal** genutzt, der eine freigegebene Azure Service Bus-Warteschlange verwendet. Wenn ein Aktivitätsauftrag ausgeführt werden muss, reiht Data Factory die Anforderung zusammen mit den Anmeldeinformationen in die Warteschlange ein (falls Anmeldeinformationen nicht bereits unter der selbstgehosteten Integrationslaufzeit gespeichert sind). Die selbstgehostete Integrationslaufzeit startet den Auftrag, nachdem die Warteschlange abgefragt wurde.
6.  Die selbstgehostete Integrationslaufzeit kopiert Daten aus einem lokalen Speicher in einen Cloudspeicher oder in umgekehrter Richtung. Dies hängt davon ab, wie die Kopieraktivität in der Datenpipeline konfiguriert ist. Für diesen Schritt kommuniziert die selbstgehostete Integrationslaufzeit über einen sicheren Kanal (HTTPS) direkt mit einem cloudbasierten Speicherdienst, z.B. Azure Blob Storage.

## <a name="considerations-for-using-self-hosted-ir"></a>Aspekte der Nutzung von selbstgehosteter Integrationslaufzeit

- Eine einzelne selbstgehostete Integrationslaufzeit kann für mehrere lokale Datenquellen verwendet werden. Allerdings ist eine **einzelne selbstgehostete Integrationslaufzeit nur an eine Azure Data Factory gebunden** und kann nicht mit einer anderen Data Factory gemeinsam genutzt werden.
- Sie können auf einem Computer **nur eine Instanz der selbstgehosteten Integrationslaufzeit** installieren. Wenn Sie beispielsweise über zwei Data Factorys verfügen, die auf lokale Datenquellen zugreifen müssen, ist es erforderlich, dass Sie auf zwei lokalen Computern jeweils eine selbstgehostete Integrationslaufzeit installieren. Anders ausgedrückt: Eine selbstgehostete Integrationslaufzeit wird an eine bestimmte Data Factory gebunden.
- Die **selbstgehostete Integrationslaufzeit muss sich nicht auf demselben Computer wie die Datenquelle befinden**. Wenn sich die selbstgehostete Integrationslaufzeit näher an der Datenquelle befindet, dauert es weniger lange, bis die selbstgehostete Integrationslaufzeit eine Verbindung mit der Datenquelle hergestellt hat. Es wird empfohlen, die selbstgehostete Integrationslaufzeit auf einem anderen Computer als dem Computer zu installieren, auf dem die lokale Datenquelle gehostet wird. Wenn sich die selbstgehostete Integrationslaufzeit und die Datenquelle auf unterschiedlichen Computern befinden, steht die selbstgehostete Integrationslaufzeit mit der Datenquelle nicht im Wettbewerb um Ressourcen.
- Sie können über **mehrere selbstgehostete Integrationslaufzeiten auf verschiedenen Computern verfügen, die eine Verbindung mit der gleichen lokalen Datenquelle herstellen**. Beispielsweise können Sie über zwei selbstgehostete Integrationslaufzeiten verfügen, die zwei Data Factorys mit Daten versorgen, wobei aber dieselbe lokale Datenquelle für beide Data Factorys registriert ist.
- Falls Sie auf Ihrem Computer bereits ein Gateway installiert haben, das für ein **Power BI**-Szenario verwendet wird, installieren Sie auf einem anderen Computer eine **separate selbstgehostete Integrationslaufzeit für die Azure Data Factory**.
- Die selbstgehostete Integrationslaufzeit muss verwendet werden, um die Datenintegration im virtuellen Azure-Netzwerk zu unterstützen.
- Behandeln Sie Ihre Datenquelle wie eine lokale Datenquelle (die sich hinter einer Firewall befindet), selbst wenn Sie **ExpressRoute**verwenden. Verwenden Sie die selbstgehostete Integrationslaufzeit, um die Konnektivität zwischen dem Dienst und der Datenquelle herzustellen.
- Sie müssen die selbstgehostete Integrationslaufzeit auch verwenden, wenn sich der Datenspeicher in der Cloud auf einem **virtuellen Azure IaaS-Computer** befindet.

## <a name="prerequisites"></a>Voraussetzungen

- Die unterstützten **Betriebssystemversionen** sind Windows 7 Service Pack 1, Windows 8.1, Windows 10, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2 und Windows Server 2016. Die Installation der selbstgehosteten Integrationslaufzeit auf einem **Domänencontroller wird nicht unterstützt**.
- Sie benötigen **mindestens .NET Framework 4.6.1**. Wenn Sie die selbstgehostete Integrationslaufzeit auf einem Windows 7-Computer installieren, sollten Sie .NET Framework 4.6.1 oder höher installieren. Ausführlichere Informationen finden Sie unter [Systemanforderungen für .NET Framework](/dotnet/framework/get-started/system-requirements) .
- Die empfohlene **Konfiguration** für den Computer mit der selbstgehosteten Integrationslaufzeit lautet wie folgt: mindestens 2 GHz, 4 Kerne, 8 GB RAM und Datenträger mit 80 GB.
- Wenn sich der Hostcomputer im Ruhezustand befindet, reagiert die selbstgehostete Integrationslaufzeit nicht auf Datenanforderungen. Aus diesem Grund sollten Sie vor der Installation der selbstgehosteten Integrationslaufzeit einen entsprechenden Energiesparplan auf dem Computer konfigurieren. Wenn für den Computer der Ruhezustand konfiguriert ist, wird bei der Installation der selbstgehosteten Integrationslaufzeit eine Meldung angezeigt.
- Sie müssen der Administrator des Computers sein, um die selbstgehostete Integrationslaufzeit erfolgreich installieren und konfigurieren zu können.
- Da die Kopieraktivität mit einer bestimmten Häufigkeit ausgeführt wird, folgt die Ressourcenverwendung (CPU, Arbeitsspeicher) auf dem Computer dem gleichen Muster mit Spitzen- und Leerlaufzeiten. Die Ressourcenverwendung hängt auch stark von der Datenmenge ab, die verschoben wird. Wenn mehrere Kopieraufträge in Bearbeitung sind, steigt die Ressourcenverwendung zu Spitzenzeiten an.

## <a name="installation-best-practices"></a>Bewährte Methoden für die Installation
Sie können die selbstgehostete Integrationslaufzeit installieren, indem Sie aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717) ein MSI-Setuppaket herunterladen. Im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](tutorial-hybrid-copy-powershell.md) finden Sie eine Schritt-für-Schritt-Anleitung.

- Konfigurieren Sie den Energiesparplan auf dem Hostcomputer für die selbstgehostete Integrationslaufzeit, damit der Computer nicht in den Ruhezustand versetzt wird. Wenn der Hostcomputer in den Ruhezustand versetzt wird, wechselt die selbstgehostete Integrationslaufzeit in den Offlinemodus.
- Sichern Sie regelmäßig die Anmeldeinformationen, die der selbstgehosteten Integrationslaufzeit zugeordnet sind.

## <a name="install-and-register-self-hosted-ir-from-download-center"></a>Installieren und Registrieren der selbstgehosteten Integrationslaufzeit über das Download Center

1. Navigieren Sie zur [Downloadseite für die Microsoft-Integrationslaufzeit](https://www.microsoft.com/download/details.aspx?id=39717).
2. Klicken Sie auf **Herunterladen**, wählen Sie die entsprechende Version (**32-Bit** oder **64-Bit**) aus, und klicken Sie auf **Weiter**.
3. Führen Sie **MSI** direkt aus, oder speichern Sie es zur späteren Ausführung auf Ihrer Festplatte.
4. Wählen Sie auf der **Willkommensseite** eine **Sprache** aus, und klicken Sie auf **Weiter**.
5. **Akzeptieren** Sie den Endbenutzer-Lizenzvertrag, und klicken Sie auf **Weiter**.
6. Wählen Sie den **Ordner** für die Installation der selbstgehosteten Integrationslaufzeit aus, und klicken Sie **Weiter**.
7. Klicken Sie auf der Seite **Bereit zur Installation** auf **Installieren**.
8. Klicken Sie auf **Fertig stellen** , um die Installation abzuschließen.
9. Rufen Sie den Authentifizierungsschlüssel über die Azure PowerShell ab. PowerShell-Beispiel zum Abrufen des Authentifizierungsschlüssels:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime
    ```
11. Führen Sie im Konfigurations-Manager für die Azure Data Factory-Integrationslaufzeit, der auf Ihrem Computer ausgeführt wird, auf der Seite **Integrationslaufzeit (selbstgehostet) registrieren** die folgenden Schritte aus:
    1. Fügen Sie im Textbereich den **Authentifizierungsschlüssel** ein.
    2. Optional: Klicken Sie auf **Show authentication key** (Authentifizierungsschlüssel anzeigen), um den Schlüsseltext anzuzeigen.
    3. Klicken Sie auf **Registrieren**.


## <a name="high-availability-and-scalability"></a>Hochverfügbarkeit und Skalierbarkeit
Eine selbstgehostete Integration Runtime kann mehreren lokalen Computern zugeordnet werden. Diese Computer werden als Knoten bezeichnet. Einer selbstgehosteten Integrationslaufzeit können bis zu vier Knoten zugeordnet sein. Die Vorteile der Nutzung mehrerer Knoten (lokale Computer mit installiertem Gateway) für ein logisches Gateway sind:
1. Höhere Verfügbarkeit der selbstgehosteten Integrationslaufzeit, damit es sich nicht mehr um die einzige Fehlerquelle (Single Point of Failure) in Ihrer Big Data-Lösung oder Clouddatenintegration mit Azure Data Factory handelt. Auf diese Weise wird die Kontinuität mit bis zu vier Knoten sichergestellt.
2. Verbesserung in Bezug auf die Leistung und den Durchsatz während der Datenverschiebung zwischen lokalen und Clouddatenspeichern. Informieren Sie sich über [Leistungsvergleiche](copy-activity-performance.md).

Sie können mehrere Knoten zuordnen, indem Sie einfach die Software für die selbstgehostete Integration Runtime aus dem [Download Center](https://www.microsoft.com/download/details.aspx?id=39717) installieren und registrieren und einen der Authentifizierungsschlüssel verwenden, der mit dem New-AzureRmDataFactoryV2IntegrationRuntimeKey-Cmdlet ermittelt wurde. Dies ist im entsprechenden [Tutorial](tutorial-hybrid-copy-powershell.md) beschrieben.

> [!NOTE]
> Für die Zuordnung zu den einzelnen Knoten müssen Sie nicht jeweils eine neue selbstgehostete Integrationslaufzeit erstellen.

## <a name="system-tray-icons-notifications"></a>Symbole und Benachrichtigungen in der Taskleiste
Wenn Sie den Cursor auf das Taskleistensymbol bzw. die Benachrichtigungsmeldung bewegen, können Sie auf Details zum Status der selbstgehosteten Integrationslaufzeit zugreifen.

![Benachrichtigungen in der Taskleiste](media\create-self-hosted-integration-runtime\system-tray-notifications.png)

## <a name="ports-and-firewall"></a>Ports und Firewall
Zwei Firewalls müssen berücksichtigt werden: Die **Unternehmensfirewall**, die auf dem zentralen Router des Unternehmens ausgeführt wird, und die **Windows-Firewall**, die als Daemon auf dem lokalen Computer mit der selbstgehosteten Integrationslaufzeit konfiguriert ist.

![Firewall](media\create-self-hosted-integration-runtime\firewall.png)

Auf Ebene der **Unternehmensfirewall** müssen Sie die folgenden Domänen und ausgehenden Ports konfigurieren:

Domänennamen | Ports | Beschreibung
------------ | ----- | ------------
*.servicebus.windows.net | 443, 80 | Wird für die Kommunikation mit dem Back-End für den Datenverschiebungsdienst verwendet
*.core.windows.net | 443 | Wird für das gestaffelte Kopieren mit einem Azure-Blob (sofern konfiguriert) verwendet
*.frontend.clouddatahub.net | 443 | Wird für die Kommunikation mit dem Back-End für den Datenverschiebungsdienst verwendet

Auf Ebene der **Windows-Firewall** (Computerebene) sind diese ausgehenden Ports normalerweise aktiviert. Falls nicht, können Sie die Domänen und Ports auf dem Computer mit der selbstgehosteten Integrationslaufzeit entsprechend konfigurieren.

> [!NOTE]
> Basierend auf Ihrer Datenquelle und den Senken müssen Sie unter Umständen der Whitelist Ihrer Unternehmens-/Windows-Firewall zusätzliche Domänen und ausgehende Ports hinzufügen.
>
> Für einige Clouddatenbanken (z.B. Azure SQL-Datenbank, Azure Data Lake usw.) müssen Sie die IP-Adresse des Computers mit der selbstgehosteten Integrationslaufzeit für die Firewallkonfiguration ggf. auf eine Whitelist setzen.

### <a name="copy-data-from-a-source-to-a-sink"></a>Kopieren von Daten von einer Quelle in eine Senke
Stellen Sie sicher, dass die Firewallregeln für die Unternehmensfirewall, die Windows-Firewall auf dem Computer mit der selbstgehosteten Integrationslaufzeit und den Datenspeicher selbst richtig aktiviert sind. Bei Aktivierung dieser Regeln kann die selbstgehostete Integrationslaufzeit erfolgreich eine Verbindung mit der Quelle und der Senke herstellen. Aktivieren Sie die Regeln für jeden Datenspeicher, der am Kopiervorgang beteiligt ist.

Führen Sie beispielsweise die folgenden Schritte aus, um Daten aus **einem lokalen Datenspeicher in eine Azure SQL-Datenbanksenke oder eine Azure SQL Data Warehouse-Senke** zu kopieren:

- Lassen Sie ausgehende **TCP**-Kommunikation an Port **1433** sowohl für die Windows-Firewall als auch die Unternehmensfirewall zu.
- Konfigurieren Sie die Firewall der Azure SQL Server-Instanz, um die IP-Adresse des Computers mit der selbstgehosteten Integrationslaufzeit der Liste mit den zulässigen IP-Adressen hinzuzufügen.

> [!NOTE]
> Falls Ihre Firewall den ausgehenden Port 1433 nicht zulässt, kann die selbstgehostete Integrationslaufzeit nicht direkt auf Azure SQL zugreifen. In diesem Fall können Sie das [gestaffelte Kopieren](copy-activity-performance.md) in Azure SQL-Datenbank oder Azure SQL Data Warehouse verwenden. Sie benötigen in einem solchen Szenario nur HTTPS (Port 443) für das Verschieben der Daten.


## <a name="proxy-server-considerations"></a>Proxyserver-Aspekte
Konfigurieren Sie die selbstgehostete Integrationslaufzeit mit den geeigneten Proxyeinstellungen, wenn die Netzwerkumgebung Ihres Unternehmens einen Proxyserver für den Internetzugriff verwendet. Sie können den Proxy während der anfänglichen Registrierungsphase festlegen.

![Angeben des Proxys](media\create-self-hosted-integration-runtime\specify-proxy.png)

Die selbstgehostete Integrationslaufzeit verwendet den Proxyserver zum Herstellen der Verbindung mit dem Clouddienst. Klicken Sie während des Anfangssetups auf „Ändern“. Das Dialogfeld mit den Proxyeinstellungen wird angezeigt.

![Festlegen des Proxys](media\create-self-hosted-integration-runtime\set-http-proxy.png)

Es gibt drei Konfigurationsoptionen:

- **Proxy nicht verwenden**: Für die selbstgehostete Integrationslaufzeit wird nicht explizit ein Proxy verwendet, um eine Verbindung mit Clouddiensten herzustellen.
- **Systemproxy verwenden**: Die selbstgehostete Integrationslaufzeit verwendet die in „diahost.exe.config“ und „diawp.exe.config“ konfigurierten Proxyeinstellungen. Wenn in „diahost.exe.config“ und „diawp.exe.config“ kein Proxy konfiguriert ist, stellt die selbstgehostete Integrationslaufzeit die Verbindung mit den Clouddiensten nicht über einen Proxy, sondern direkt her.
- **Benutzerdefinierten Proxy verwenden**: Konfigurieren Sie hier die HTTP-Proxyeinstellungen, die für die selbstgehostete Integrationslaufzeit verwendet werden sollen, anstatt die Konfigurationen in den Dateien „diahost.exe.config“ und „diawp.exe.config“ zu nutzen. Adresse und Port sind erforderlich. Benutzername und Kennwort sind optional, je nach den Authentifizierungseinstellungen Ihres Proxys. Alle Einstellungen werden für die selbstgehostete Integrationslaufzeit per Windows DPAPI verschlüsselt und lokal auf dem Computer gespeichert.

Der Hostdienst der Integrationslaufzeit wird automatisch neu gestartet, nachdem Sie die aktualisierten Proxyeinstellungen gespeichert haben.

Wenn Sie nach der erfolgreichen Registrierung der selbstgehosteten Integrationslaufzeit die Proxyeinstellungen anzeigen oder aktualisieren möchten, können Sie den Konfigurations-Manager für die Integrationslaufzeit verwenden.

1.  Starten Sie den **Konfigurations-Manager für die Azure Data Factory-Integrationslaufzeit**.
2.  Wechseln Sie zur Registerkarte **Einstellungen** .
3.  Klicken Sie im Abschnitt **HTTP-Proxy** auf den Link **Ändern**, um das Dialogfeld **HTTP-Proxy festlegen** zu öffnen.
4.  Nachdem Sie auf die Schaltfläche **Weiter** geklickt haben, wird ein Dialogfeld mit einer Warnung angezeigt, in dem Sie bestätigen müssen, dass die Proxyeinstellung gespeichert und der Hostdienst der Integrationslaufzeit neu gestartet werden soll.

Sie können den HTTP-Proxy im Konfigurations-Manager anzeigen und aktualisieren.

![Anzeigen des Proxys](media\create-self-hosted-integration-runtime\view-proxy.png)

> [!NOTE]
> Wenn Sie einen Proxyserver mit NTLM-Authentifizierung einrichten, wird der Hostdienst der Integrationslaufzeit unter dem Domänenkonto ausgeführt. Wenn Sie das Kennwort für das Domänenkonto später ändern, denken Sie daran, die Konfigurationseinstellungen für den Dienst entsprechend zu aktualisieren und neu zu starten. Aufgrund dieser Anforderung empfiehlt es sich, ein dediziertes Domänenkonto für den Zugriff auf den Proxyserver zu verwenden, in dem das Kennwort nicht regelmäßig geändert werden muss.

### <a name="configure-proxy-server-settings"></a>Konfigurieren von Proxyservereinstellungen

Wenn Sie die Einstellung **Systemproxy verwenden** für den HTTP-Proxy auswählen, verwendet die selbstgehostete Integrationslaufzeit die Proxyeinstellungen in „diahost.exe.config“ und „diawp.exe.config“. Falls in „diahost.exe.config“ und „diawp.exe.config“ kein Proxy angegeben ist, stellt die selbstgehostete Integrationslaufzeit die Verbindung mit den Clouddiensten nicht über einen Proxy, sondern direkt her. Das folgende Verfahren enthält Anweisungen für die Aktualisierung der Datei „diahost.exe.config“.

1. Erstellen Sie im Datei-Explorer eine sichere Kopie von „C:\Programme\Microsoft Integration Runtime\3.0\Shared\diahost.exe.config“, um die Originaldatei zu sichern.
2. Starten Sie „Notepad.exe“ als Administrator, und öffnen Sie die Textdatei „C:\Programme\Microsoft Integration Runtime\3.0\Shared\diahost.exe.config“. Sie finden das Standardtag für „system.net“, wie im folgenden Code gezeigt:

    ```xml
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```
    Anschließend können Sie die Informationen zum Proxyserver wie im folgenden Beispiel gezeigt hinzufügen:

    ```xml
    <system.net>
        <defaultProxy enabled="true">
              <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Zusätzliche Eigenschaften sind im Proxytag zulässig, um die erforderlichen Einstellungen anzugeben, z. B. scriptLocation. Die Syntax finden Sie unter [<proxy>-Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx).

    ```xml
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Speichern Sie die Konfigurationsdatei am ursprünglichen Speicherort, und starten Sie den Hostdienst für die selbstgehostete Integrationslaufzeit, der die Änderungen übernimmt. Um den Dienst neu zu starten, verwenden Sie das Applet „Dienste“ in der Systemsteuerung, oder klicken Sie im **Konfigurations-Manager für die Integrationslaufzeit** auf die Schaltfläche **Dienst beenden** und anschließend auf **Dienst starten**. Wenn der Dienst nicht gestartet wird, wurde der bearbeiteten Anwendungskonfigurationsdatei wahrscheinlich eine falsche XML-Tagsyntax hinzugefügt.

> [!IMPORTANT]
> Vergessen Sie nicht, beide Dateien („diahost.exe.config“ und „diawp.exe.config“) zu aktualisieren.

Zusätzlich zu den obigen Punkten müssen Sie auch sicherstellen, dass Microsoft Azure in der Whitelist Ihres Unternehmens aufgeführt ist. Sie können die Liste mit den gültigen Microsoft Azure-IP-Adressen im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653)herunterladen.

### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Mögliche Symptome für Probleme im Zusammenhang mit der Firewall und dem Proxyserver
Wenn Sie ähnliche Fehler wie die unten aufgeführten feststellen, liegt dies meist an einer unsachgemäßen Konfiguration der Firewall oder des Proxyservers. Hierdurch wird verhindert, dass die selbstgehostete Integrationslaufzeit eine Verbindung mit der Data Factory herstellt, um sich zu authentifizieren. Überprüfen Sie den vorherigen Abschnitt, um sicherzustellen, dass die Firewall und der Proxyserver richtig konfiguriert sind.

1.  Wenn Sie versuchen, die selbstgehostete Integrationslaufzeit zu registrieren, erhalten Sie den folgenden Fehler: „Fehler beim Registrieren dieses Knotens der Integrationslaufzeit. Stellen Sie sicher, dass der Authentifizierungsschlüssel gültig ist und dass der Hostdienst der Integrationslaufzeit auf diesem Computer ausgeführt wird. "
2.  Wenn Sie den Konfigurations-Manager für die Integrationslaufzeit öffnen, wird der Status als **Getrennt** oder **Verbindung wird hergestellt** angezeigt. Beim Anzeigen der Windows-Ereignisprotokolle sehen Sie unter „Ereignisanzeige“ > „Anwendungs- und Dienstprotokolle“ > „Microsoft-Integrationslaufzeit“ beispielsweise folgende Fehlermeldung:

    ```
    Unable to connect to the remote server
    A component of Integration Runtime has become unresponsive and restarts automatically. Component name: Integration Runtime (Self-hosted).
    ```

### <a name="open-port-8060-for-credential-encryption"></a>Öffnen von Port 8060 für die Verschlüsselung der Anmeldeinformationen
Für die Anwendung **Anmeldeinformationen festlegen** (derzeit nicht unterstützt) wird der eingehende Port 8060 verwendet, um Anmeldeinformationen an die selbstgehostete Integrationslaufzeit weiterzuleiten, wenn Sie im Azure-Portal einen lokalen verknüpften Dienst einrichten. Während der Einrichtung der selbstgehosteten Integrationslaufzeit wird sie vom entsprechenden Installationsvorgang auf dem Computer für die selbstgehostete Integrationslaufzeit geöffnet.

Bei Verwendung einer Drittanbieterfirewall können Sie den Port 8050 manuell öffnen. Falls beim Einrichten der selbstgehosteten Integrationslaufzeit Firewallprobleme auftreten, können Sie mit dem unten angegebenen Befehl versuchen, die selbstgehostete Integrationslaufzeit zu installieren, ohne die Firewall zu konfigurieren.

```
msiexec /q /i IntegrationRuntime.msi NOFIREWALL=1
```

Falls Sie den Port 8060 auf dem Computer für die selbstgehostete Integrationslaufzeit nicht öffnen, sollten Sie andere Verfahren als die Anwendung „Anmeldeinformationen festlegen“ nutzen, um Anmeldeinformationen für den Datenspeicher zu konfigurieren. Beispielsweise können Sie das New-AzureRmDataFactoryV2LinkedServiceEncryptCredential-PowerShell-Cmdlet verwenden. Informationen zum Festlegen von Datenspeicher-Anmeldeinformationen finden Sie unter „Festlegen von Anmeldeinformationen und Sicherheit“.


## <a name="next-steps"></a>Nächste Schritte
Das folgende Tutorial enthält eine Schritt-für-Schritt-Anleitung: [Tutorial: Kopieren von lokalen Daten in die Cloud](tutorial-hybrid-copy-powershell.md).
