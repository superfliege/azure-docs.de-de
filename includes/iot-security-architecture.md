# <a name="internet-of-things-security-architecture"></a>Internet der Dinge-Sicherheitsarchitektur
Beim Entwerfen eines Systems ist es wichtig zu verstehen die potenziellen Gefahren für das System und entsprechende Schutzmaßnahmen entsprechend hinzugefügt, wie das System konzipiert und ausgelegt ist. Es ist besonders wichtig, das Produkt vom Beginn unter Berücksichtigung von Sicherheitsaspekten entworfen werden, da verstehen, wie ein Angreifer ein System beeinträchtigen möglicherweise hilft dabei, stellen Sie sicher, dass entsprechende Gegenmaßnahmen sind vom Anfang. 

## <a name="security-starts-with-a-threat-model"></a>Sicherheit beginnt mit eines Bedrohungsmodells
Microsoft hat gefahrenmodelle zu lange für seine Produkte verwendet und des Unternehmens Bedrohungsanalyse öffentlich verfügbar gemacht hat. Die Unternehmens-Erfahrung veranschaulicht, dass die Modellierung hat unerwartete Vorteile hinter der sofortige Überblick darüber, was die meisten Bedrohungen sind bereit. Beispielsweise erstellt, die auch Bestimmtheit für eine offene Diskussion andere außerhalb der vom Entwicklungsteam vorgenommen, was zu neue Ideen und Verbesserungen in das Produkt führen kann.

Die Erstellung von Bedrohungsmodellen anstreben, zu verstehen, wie ein Angreifer möglicherweise auf ein System, und stellen Sie sicher, dass entsprechende Gegenmaßnahmen erfüllt sind. Threat Modeling erzwingt, dass das Entwurfsteam Maßnahmen zu berücksichtigen, wie das System vorgesehen ist und nicht nach einem System bereitgestellt wird. Diese Tatsache ist ausgesprochen wichtig, da den für angepasste Schutzmaßnahmen für eine Vielzahl von Geräten in das Feld unmöglich ist, ist fehleranfällig und Kunden gefährdet.

Viele Entwicklungsteams sind eine hervorragende Arbeit erfassen die funktionalen Anforderungen für das System, die Kunden zu profitieren. Identifizieren von nicht offensichtliche Verfahren, die ein Benutzer das System bei falscher Verwendung möglicherweise ist jedoch schwieriger. Erstellung von Bedrohungsmodellen helfen zu verstehen, was ein Angreifer Synchronisierungsgrenze Entwicklungsteams und deren Ursachen. Erstellung von Bedrohungsmodellen ist ein strukturierter Prozess, der erstellt einer Diskussion über die Sicherheit entwurfsentscheidungen im System als auch Änderungen am Entwurf, die Erweiterungsklassen, der Auswirkungen auf die Sicherheit vorgenommen werden. Während ein Bedrohungsmodells einfach ein Dokument ist, stellt dieser Dokumentation auch sicherstellen, dass die Kontinuität des Wissens, Beibehaltung von Lektionen erfahren und Hilfe neuen team integrieren schnell eine ideale Möglichkeit dar. Schließlich wird ein Ergebnis der Erstellung von Bedrohungsmodellen, sodass Sie andere Aspekte der Sicherheit, z. B. welche Sicherheit Verpflichtungen berücksichtigen, die Sie für Ihre Kunden bereitstellen möchten. Werden diese Verpflichtungen in Verbindung mit der Erstellung von Bedrohungsmodellen informieren, und Testen der Projektmappe Internet of Things (IoT) Laufwerk.

### <a name="when-to-threat-model"></a>Wenn die Bedrohung Modell
[Erstellung von Bedrohungsmodellen](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) bietet den größten Wert aus, wenn es in der Entwurfsphase eingebunden werden. Beim Entwerfen, müssen Sie die größte Flexibilität, zu ändern, um Gefahren zu beseitigen. Eliminieren von Bedrohungen beabsichtigt ist das gewünschte Ergebnis. Es ist sehr viel einfacher als Gegenmaßnahmen hinzufügen, sie testen und sie aktuelle bleiben und außerdem solche für Eliminierung von Duplikaten ist nicht immer möglich, sicherzustellen. Es wird immer schwieriger, um Gefahren zu beseitigen, ein Produkt wird ausgereiftere und wiederum letztlich benötigen weitere Arbeit "und" sehr viel schwieriger vor-und Nachteile als Gefahrenmodellen am Anfang der Entwicklung.

### <a name="what-to-threat-model"></a>Auszuführende Aktion Gefahrenmodells
Sie sollten thread Modell der Projektmappe als Ganzes und konzentrieren Sie sich auch in den folgenden Bereichen:

* Die Funktionen für Sicherheit und Datenschutz
* Die Funktionen, deren Fehler Sicherheit relevant sind
* Die Funktionen, die eine Vertrauensgrenze berühren. 

### <a name="who-threat-models"></a>Die Bedrohung Modelle
Erstellung von Bedrohungsmodellen ist ein Prozess wie jeder andere.  Es ist ratsam, die Bedrohung Modelldokument wie andere Komponenten der Lösung zu behandeln und überprüft werden. Viele Entwicklungsteams sind eine hervorragende Arbeit erfassen die funktionalen Anforderungen für das System, die Kunden zu profitieren. Identifizieren von nicht offensichtliche Verfahren, die ein Benutzer das System bei falscher Verwendung möglicherweise ist jedoch schwieriger. Erstellung von Bedrohungsmodellen helfen zu verstehen, was ein Angreifer Synchronisierungsgrenze Entwicklungsteams und deren Ursachen.

### <a name="how-to-threat-model"></a>Wie Gefahrenmodells
Die Bedrohungsanalyse besteht aus vier Schritten; die Schritte sind:

* Modellieren der Anwendung
* Auflisten von Bedrohungen
* Mindern von Bedrohungen
* Überprüfen Sie die Maßnahmen

#### <a name="the-process-steps"></a>Die Prozessschritte
Drei Regeln gilt zu bedenken, wenn ein Bedrohungsmodell zu erstellen:

1. Erstellen Sie ein Diagramm aus Referenzarchitektur. 
2. Starten Sie breitensuchindex. Bietet eine Übersicht über, und verstehen Sie das System als Ganzes vor dem Deep Einstieg.  Dadurch wird sichergestellt, Sie tiefer gehende in den richtigen Stellen stattfinden.
3. Laufwerk des Prozesses, lassen Sie den Prozess, der Sie Laufwerk nicht. Wenn Sie ein Problem in der Phase der Modellierung finden und es untersuchen möchten, los geht's!  Können Sie nicht, die folgenden Schritte Servicekraft ausführen müssen.  

#### <a name="threats"></a>Bedrohungen
Die vier Kernelemente eines Bedrohungsmodells sind:

* Prozesse (Web Services, Win32-Diensten * nix-Daemons usw.. Beachten Sie, dass einige komplexe Entitäten (z. B. Feld Gateways und Sensoren) abstrahiert werden können, wie ein Prozess, bei dem eine technische Drilldown in den folgenden Bereichen nicht möglich ist.
* Datenspeicher (überall, wo Daten, z. B. einer Konfigurationsdatei oder Datenbank gespeichert werden)
* Datenfluss (, in dem Daten zwischen den anderen Elementen in der Anwendung wechselt)
* Externe Entitäten (Elemente, die Interaktion mit dem System aber nicht unter der Kontrolle der Anwendung, Beispiele für die Benutzer auswählen und Satelliten-Feeds)

Alle Elemente im Architekturdiagramm unterliegen verschiedenen Bedrohungen; Wir verwenden das STRIDE mnemonische Zeichen. Lesen [Threat Modeling erneut, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) um weitere Informationen zu den STRIDE-Elementen.

Andere Elemente von dem Anwendungsdiagramm unterliegen bestimmter STRIDE-Risiken:

* Prozesse unterliegen STRIDE
* Datenflüsse unterliegen TID
* Datenspeicher unterliegen TID und manchmal R, ein, wenn der Datenspeicher Protokolldateien sind.
* Externe Entitäten unterliegen SRD

## <a name="security-in-iot"></a>Sicherheit in IoT
Verbundene zweckgebundene Geräte wurden besonders viele mögliche Interaktion Oberflächen und interaktionsmustern, die berücksichtigt werden müssen, um ein Framework für die digitale Zugriffsschutz für diese Geräte bereitzustellen. Der Begriff "digitale Zugriff" ist hier verwendet, um alle Vorgänge unterscheiden, die durch direkte geräteinteraktion durchgeführt werden, auf dem Access-Sicherheit über physischen Zugriff-Steuerelement bereitgestellt. Platzieren das Gerät z. B. in einem Raum mit einer Sperre auf die Tür. Während der Zugang verweigert werden kann, Software und Hardware verwenden, können Maßnahmen ergriffen werden, um physischen Zugang von führenden auf System Störungen zu verhindern. 

Wie wir die interaktionsmustern untersuchen, werden wir "Gerät Control" und "Gerätedaten" mit der gleichen Ebene der Aufmerksamkeit betrachten. "Gerät Control" kann alle Informationen klassifiziert werden, die ein Gerät von einer Partei mit dem Ziel ändern oder beeinflussen das Verhalten für den Zustand oder den Zustand der eigenen Umgebung bereitgestellt wird. "Gerätedaten" können alle Informationen klassifiziert werden, die ein Gerät an eine andere Partei über seinen Status und den erfassten Zustand der eigenen Umgebung ausgibt.

Um bewährte Sicherheitsmethoden zu optimieren, wird empfohlen, dass eine typische IoT-Architektur in mehreren Komponente/Zonen als Teil der Bedrohungsmodell aufgeteilt werden. Diese Zonen werden vollständig in diesem Abschnitt beschrieben und umfassen Folgendes:

* Gerät
* Feld-Gateway
* Cloud-Gateways, und
* Dienste.

Zonen sind allgemeine Möglichkeit für eine Projektmappe zu segmentieren. Jede Zone verfügt häufig eigene Daten, Authentifizierung und Autorisierung aufweisen. Zonen können auch verwendet werden, um Isolation Schaden und beschränken die Auswirkungen der geringen Vertrauenswürdigkeit Zonen für höhere Vertrauensstellung Zonen.

Jeder Zone wird durch eine Grenze vertrauen getrennt, die als die rote gepunktete Linie im Diagramm unten hingewiesen. Es stellt einen Übergang von Daten/Informationen von einer Quelle zu einem anderen dar. Während dieses Übergangs konnte die Dateninformationen Spoofing, Manipulation, Nichtanerkennung, Offenlegung von Informationen, Denial-of-Service und Erhöhung der Rechte (STRIDE) unterliegen.

![IoT-Sicherheitszonen](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Die Komponenten, die in jede Grenze dargestellt werden auch vorhanden ist, die STRIDE, aktivieren eine vollständige 360 Gefahrenmodellen Überblick über die Lösung. Die folgenden Abschnitte Details für jede der Komponenten und spezielle Sicherheitsrisiken und Lösungen, die installiert werden soll.

In den Abschnitten, die folgt besprechen Standardkomponenten, die in der Regel in diese Zonen gefunden.

### <a name="the-device-zone"></a>Die Geräte-Zone
Die geräteumgebung ist die sofortige physischen Platz um das Gerät bei physischen Zugriff und/oder "LAN" digitale Peer-zu-Peer-Zugriff auf das Gerät ist möglich. Ein "Lokales Netzwerk" wird angenommen, ein Netzwerk zu sein, die ist eindeutig und umgebungsabfragen – jedoch potenziell an – das öffentliche Internet überbrückt und umfasst alle eine drahtlosen Sender-Technologie, die Peer-zu-Peer-Kommunikation der Geräte ermöglicht. Dies geschieht *nicht* enthalten Netzwerk-Virtualisierungstechnologie, die den Eindruck eines lokalen Netzwerks zu erstellen und es umfasst außerdem nicht öffentlicher Operator-Netzwerke, die verlangen, zwei Geräte über öffentliche netzwerkraums zu kommunizieren dass, wären sie eine Peer-zu-Peer-Kommunikation Beziehung geben.

### <a name="the-field-gateway-zone"></a>Der Feld-Gateway-Zone
Feld-Gateway ist ein Gerät/Appliance oder einige allgemeine Server-Computersoftware, die als Voraussetzung für Kommunikation und ggf. als Geräte Quellcodeverwaltungssystems und die Datenverarbeitung Hub Gerät fungiert. Die Feld-Gateway-Zone enthält das Feld-Gateway selbst und alle Geräte, die damit verbunden sind. Wie der Name schon sagt, Feld Gateways externen Daten Verarbeitungsfunktionen dedizierten fungieren, befinden sich normalerweise Speicherort gebunden physischen Erkennen von Eindringlingen potenziell unterliegen und haben eingeschränkte betriebliche Redundanz. Alle um sagen, dass ein Feld-Gateway häufig eine Sache ist kann eine berühren und sabotage, während Sie wissen, was die Funktion ist. 

Ein Feld-Gateway unterscheidet sich von einem Router bloße Datenverkehr, es wurde eine aktive Rolle Verwalten des Zugriffs und Informationsfluss, was bedeutet, dass es sich um eine Anwendung ist, Entität und die Netzwerkverbindung oder Terminaldienste-Sitzung behoben. Ein NAT-Gerät oder eine Firewall, im Gegensatz dazu eignen sich nicht als Feld Gateways da es keine explizite Verbindung oder Sitzung Terminals Verbindungen können jedoch stattdessen eine Route (oder Block) sind oder Sitzungen, die Sie vorgenommen. Das Feld-Gateway hat zwei unterschiedliche Oberflächen. Einen portalclient Geräte, die damit verbunden sind und innerhalb der Zone darstellt, und die andere portalclient alle externe Parteien und am Rand der Zone ist.   

### <a name="the-cloud-gateway-zone"></a>Die Cloud-Gateway-zone
Cloud-Gateway ist ein System, das remote-Kommunikationsfehlers alte und neue Geräte oder Feld-Gateways aus mehreren unterschiedlichen Standorten über öffentliche netzwerkraums, in der Regel für ein Cloud-basierten-Steuerelement und Analyse Datensystem für einen Verbund solche Systeme kann. In einigen Fällen kann ein Cloud-Gateway sofort Zugriff auf spezielle Geräte von Terminals wie Tablets und Telefone erleichtern. Im Kontext hier erläuterten "Cloud" bedeutet, dass auf einem dedizierten Datenverarbeitungssystem verweisen, die nicht am selben Standort wie der angeschlossene Geräte oder Feld-Gateways gebunden ist. In einer Zone Cloud operational Measures targeted physischen Zugriff verhindern außerdem nicht unbedingt für eine "public" Cloudinfrastruktur verfügbar gemacht.  

Ein Cloud-Gateway kann potenziell ein netzwerkoverlay für die Virtualisierung, die Cloud-Gateway und alle angeschlossenen Geräte oder Feld-Gateways auf alle sonstigen Datenverkehr im Netzwerk zu isolieren zugeordnet werden. Das Cloud-Gateway selbst ist weder ein Gerät-System noch eine Verarbeitung oder für die Aufbewahrung für Gerätedaten; Diese Funktionen eine Schnittstelle mit dem Cloud-Gateway. Die Cloud-Gateway-Zone enthält die Cloud-Gateway selbst sowie alle Feld Gateways und direkt oder indirekt angeschlossene Geräte. Am Rand der Zone ist eine unterschiedliche Oberfläche, in dem alle externe Parteien über kommunizieren.

### <a name="the-services-zone"></a>Die Dienste-zone
Für diesen Kontext als beliebiges Softwarekomponente oder Modul, das mit Geräten über ein Feld oder Cloud-Gateway für die Datenerfassung und-Analyse sowie zum Befehl und eine Schnittstelle ist, wird ein "Dienst" definiert.  Dienste sind Vermittler. Sie ihre Identität anhand von Gateways und andere Subsysteme fungieren, speichern und Analysieren von Daten, autonom Problem Befehle aus, um Geräte basierend auf dateneinblicke oder Zeitpläne und bereitstellen und zum Steuern von Funktionen, um autorisierte Endbenutzer.

### <a name="information-devices-vs-special-purpose-devices"></a>Informationen – Geräte im Vergleich zu zweckgebundene-Geräte
PCs, Smartphones und Tablets sind in erster Linie Informationen interaktive Geräte. Telefone und Tablets sind explizit auf die Maximierung der Lebensdauer des Akkus optimiert. Sie deaktivieren vorzugsweise teilweise nicht sofort die Interaktion mit einer Person oder bei der Bereitstellung von Diensten wie Musik wiedergeben oder abgleichsarbeit ihres Besitzers an einem bestimmten Speicherort nicht. Im Hinblick auf Systeme sind diese Informationen Technologie Geräte hauptsächlich als Proxys für Personen fungiert. Sie sind "Personen Aktuatoren" vorschlägt, Aktionen und "Personen Sensoren" Sammeln von Eingaben. 

Spezielle-Geräte sind vom einfachen Temperatursensoren auf komplexe Factory-Production-Fertigungslinien mit Tausenden von Komponenten, die darin enthaltenen, unterschiedlich. Diese Geräte beziehen sich wesentlich mehr ihres Verwendungszwecks und auch wenn sie eine Benutzeroberfläche möchten bereitstellen, beziehen sich größtenteils um Schnittstellenfunktion zu oder in den Bestand in der realen Welt integriert werden. Sie messen und environmental Umstände zu melden, Ventile aktivieren, steuern Servos, sound Alarme, Leuchten wechseln und zahlreiche andere Aufgaben ausführen. Sie sind hilfreich zum funktionieren für die ein Gerät Informationen zu generischen, zu teuer, Groß oder zu jedoch fehleranfällig ist. Der konkrete Zweck bestimmt sofort ihre technischen Entwurf als auch das verfügbaren Budget für ihre produktionsumgebung und geplanten Lebensdauer-Vorgang. Die Kombination aus diesen zwei Schlüsselfaktoren schränkt die verfügbaren operational Energie Budget physischer Speicherplatzbedarf und somit verfügbarer Speicher COMPUTE- und Sicherheitsfunktionen.  

Wenn etwas "sinkt falsche" automatisierte oder remote steuerbar Geräte, Fehlern beispielsweise physische Mängel oder Steuerelementlogik Einhaltung unberechtigten Zugriff zu bearbeiten. Produktion lose können zerstört, Gebäude looted oder nach unten gebrannt werden und Personen möglicherweise verletzten oder sogar Die. Dies ist natürlich eine völlig andere Klasse Schäden als Personen aus einem gestohlenen Kreditkarte Grenzwert maximal ausnutzen. Das Sicherheitsniveau für Geräte, die Dinge verschieben sowie für Sensordaten, die letztendlich Befehle, die dazu führen, Punkte ausgibt dass zu verschieben, muss in allen e-Commerce und bankszenarios größer sein. 

### <a name="device-control-and-device-data-interactions"></a>Geräte zu kontrollieren und Gerät von klasseninteraktionen
Verbundene zweckgebundene Geräte wurden besonders viele mögliche Interaktion Oberflächen und interaktionsmustern, die berücksichtigt werden müssen, um ein Framework für die digitale Zugriffsschutz für diese Geräte bereitzustellen. Der Begriff "digitale Zugriff" ist hier verwendet, um alle Vorgänge unterscheiden, die durch direkte geräteinteraktion durchgeführt werden, auf dem Access-Sicherheit über physischen Zugriff-Steuerelement bereitgestellt. Platzieren das Gerät z. B. in einem Raum mit einer Sperre auf die Tür. Während der Zugang verweigert werden kann, Software und Hardware verwenden, können Maßnahmen ergriffen werden, um physischen Zugang von führenden auf System Störungen zu verhindern. 

Wie wir die interaktionsmustern untersuchen, werden wir "Gerät Control" und "Gerätedaten" mit der gleichen Ebene der Aufmerksamkeit während der Erstellung von Bedrohungsmodellen betrachten. "Gerät Control" kann alle Informationen klassifiziert werden, die ein Gerät von einer Partei mit dem Ziel ändern oder beeinflussen das Verhalten für den Zustand oder den Zustand der eigenen Umgebung bereitgestellt wird. "Gerätedaten" können alle Informationen klassifiziert werden, die ein Gerät an eine andere Partei über seinen Status und den erfassten Zustand der eigenen Umgebung ausgibt. 

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Erstellen von Gefahrenmodellen Azure IoT-Referenzarchitektur
Microsoft verwendet das Framework erläuterten für Azure IoT Modellierung Bedrohung. Im Abschnitt unten verwenden wir daher Azure IoT-Referenzarchitektur konkrete Beispiel zum veranschaulichen, wie im Wesentlichen Bedrohung für IoT Modellierung und die identifizierten Gefahren zu beheben. In unserem Fall wurden vier Hauptbereiche der Fokus:

* Geräte und Datenquellen
* Datentransport
* Geräte- und Verarbeitung von Ereignissen und
* Abschnitt „Presentation“

![Erstellen von Gefahrenmodellen für Azure IoT](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Das folgende Diagramm bietet eine vereinfachte Ansicht der Microsofts IoT-Architektur mit einer Data Flow Diagram-Modell, das von dem Microsoft Threat Modeling-Tool verwendet wird:

![Erstellen von Gefahrenmodellen für Azure IoT mit MS Threat Modeling-Tool](media/iot-security-architecture/iot-security-architecture-fig3.png)

Es ist wichtig zu beachten, dass die Architektur der Geräte- und Gateway-Verwaltungsfunktionen trennt. Dies ermöglicht dem Benutzer Gateway-Geräte nutzen, die sicherer sind: sie sind mit dem Cloud-Gateway über sichere Protokolle, die in der Regel erfordert mehr Verarbeitungsaufwand, dass ein natives Gerät – z. B. ein Thermostat - selbst bereitstellen konnte kommunizieren kann. In der Azure-Dienste-Zone wird angenommen, dass die Cloud-Gateway durch die Azure IoT Hub-Dienst dargestellt wird.

### <a name="device-and-data-sourcesdata-transport"></a>Geräte- und Quellen/Daten-transport
In diesem Abschnitt wird erklärt, die Erstellung von Bedrohungsmodellen Sicht oben beschriebene Architektur und vermittelt einen Überblick darüber, wie wir einige der inhärenten Bedenken adressiert werden. Wir konzentriert sich auf Kernelemente des eines Bedrohungsmodells:

* Prozesse (Entitäten unter unserer Kontrolle und externe Elemente)
* Kommunikation (so genannte Datenflüsse)
* Speicher (auch als Datenspeicher bezeichnet)

#### <a name="processes"></a>Prozesse
In den einzelnen Kategorien beschrieben, die in der Azure IoT-Architektur, versuchen wir eine Anzahl von verschiedenen Bedrohungen in den verschiedenen Stufen mindern Daten/Informationen vorhanden, in sind: Prozess, Kommunikation und Speicher. Im folgenden geben wir eine Übersicht über die gängigsten für die Kategorie "Prozess" gefolgt von einen Überblick darüber, wie diese am besten entschärft werden konnte: 

**Spoofing (S)**: ein Angreifer möglicherweise kryptografischen schlüsselmaterialien von einem Medium auf der Software oder Hardware-Ebene extrahieren und anschließend Zugriff dem System mit einem anderen physischen oder virtuellen Gerät unter der Identität des Geräts das Schlüsselmaterial aus vorgenommen wurde. Ein gutes Beispiel ist die Remote-Steuerelemente können, die alle TV aktivieren und beliebten Hackers Tools sind.

**Denial of Service (D)**: ein Gerät nicht ausgeführt werden oder von Funkfrequenzen oder Ausschneiden Drähten stören Kommunikation unterstützen gerendert werden kann. Z. B. meldet eine Überwachung Kamera, die die Stromversorgung oder Verbindung, die absichtlich ausgefallen mussten keine Daten, überhaupt.

**Manipulationen (T)**: ein Angreifer kann ganz oder teilweise ersetzen die Software auf dem Gerät ausgeführte potenziell ermöglicht die ersetzte Software, die Echtheit des Geräts zu nutzen, wäre das Schlüsselmaterial oder die Funktionen für die kryptografischen Schlüssel Materialien enthalten die unerlaubtem Anwendung zur Verfügung. Beispielsweise kann ein Angreifer nutzen, extrahierten Schlüsselmaterial zum Abfangen und Daten vom Gerät für den Kommunikationspfad zu unterdrücken, und Ersetzen Sie sie mit "false" Daten, die mit dem gestohlenen Schlüsselmaterial authentifiziert werden.

**Offenlegung von Informationen (I)**: Wenn das Gerät bearbeitete Software ausgeführt wird, bearbeitete Software kann potenziell zur Offenlegung von Daten keine unbefugten Parteien. Beispielsweise kann ein Angreifer extrahierten Schlüsselmaterial, um sich selbst in der Kommunikationspfad zwischen dem Gerät und Controller oder das Feld-Gateway oder Cloud-Gateway auf off Informationen einbeziehen einfügen nutzen.

**Erhöhung der Rechte (E)**: ein Gerät, das bestimmte Funktion ist kann erzwungen werden, um etwas anderes zu tun. Beispielsweise kann ein Ventil, die so programmiert ist, öffnen Sie halbem Weg dazu veranlasst werden, um ganz zu öffnen.

| **Komponente** | **Mindern von Bedrohungen** | **Risikominderung** | **Risiko** | **Implementierung** |
| --- | --- | --- | --- | --- |
| Gerät |S |Das Gerät Identität zuweisen und die Authentifizierung des Geräts |Ersetzen Gerät oder einen Teil des Geräts mit einem anderen Gerät an. Woher wissen wir, dass es mit dem richtigen Gerät kommuniziert werden? |Authentifizieren das Gerät, das Transport Layer Security (TLS) oder IPSec verwenden. Infrastruktur sollten unterstützen Verwendung vorinstallierten Schlüssel (PSK) auf diesen Geräten, die vollständige asymmetrische Kryptografie nicht behandeln kann. Azure AD nutzen, [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt) |
| TRID |Übernehmen Sie manipulationsgeschützter Mechanismen für das Gerät z. B. stammschlüsselpaars es sehr schwierig unmöglich, Schlüssel und andere kryptografische Material vom Gerät zu extrahieren. |Das Risiko ist, wenn ein Benutzer das Gerät (physische Störungen) Manipulation ist. Wie wir stellen Sie sicher, dass, hat das Gerät nicht manipuliert sind. |Das Minderung am effektivsten ist eine vertrauenswürdige Platform Module (TPM)-Funktion, die ermöglicht das Speichern von Schlüsseln in speziellen on a Chip Schaltungstechnik, von dem der Schlüssel können nicht gelesen werden, sondern können nur für kryptografische Vorgänge, die Schlüssel verwenden, aber nie Offenlegen des Schlüssels verwendet werden. Arbeitsspeicher-Verschlüsselung des Geräts. Die schlüsselverwaltung für das Gerät. Codesignatur. | |
| E |HAVING Zugriffssteuerung des Geräts an. Autorisierungsschemas. |Wenn das Gerät für einzelne Aktionen, die ausgeführt werden, basierend auf Befehle aus einer externen Quelle oder sogar gefährdeten Sensoren zulässt, lässt er das Risiko eines Angriffs, um Vorgänge auszuführen, ohne dass ansonsten zugegriffen werden kann. |HAVING Autorisierungsschemas für das Gerät | |
| Feld-Gateway |S |Authentifizieren von Feld-Gateway mit Cloud-Gateway (Cert basieren, PSK-Anspruchs basierend,...) |Wenn jemand Feld-Gateway zu spoofen kann, können sie selbst als beliebiges Gerät vorhanden. |TLS RSA/PSK, IPSec, [RFC 4279](https://tools.ietf.org/html/rfc4279). Die gleichen Schlüssel Bedenken hinsichtlich Speicher- und eine Beglaubigung von Geräten im Allgemeinen – Idealfall wird verwendet, TPM. 6LowPAN-Erweiterung für IPSec zur Unterstützung von Wireless Sensor Netzwerke (WSN). |
| TRID |Schützen der Feld-Gateway vor Manipulation (TPM)? |Spoofingangriffe, die der Cloud-Gateway-Gedanken zu bringen, die es Feld-Gateway kommuniziert kann Offenlegung von Informationen und Datenmanipulation führen. |Arbeitsspeicher-Verschlüsselung, TPM, Authentifizierung. | |
| E |Zugriffssteuerungsmechanismus für Feld-Gateway | | | |

Hier sind einige Beispiele für die Vermeidung von Risiken in diese Kategorie:

Spoofing: Ein Angreifer möglicherweise extrahieren kryptografischen schlüsselmaterialien von einem Gerät, das entweder zur Software oder Hardware-Ebene und anschließend den Zugriff, die aus das System mit einem anderen physischen oder virtuellen Gerät unter der Identität des Geräts das Schlüsselmaterial angenommen wurde.

**Dienstverweigerung**: ein Gerät nicht ausgeführt werden oder von Funkfrequenzen oder Ausschneiden Drähten stören Kommunikation unterstützen gerendert werden kann. Z. B. meldet eine Überwachung Kamera, die die Stromversorgung oder Verbindung, die absichtlich ausgefallen mussten keine Daten, überhaupt.

**Manipulationen**: ein Angreifer kann ganz oder teilweise ersetzen die Software auf dem Gerät ausgeführte potenziell ermöglicht die ersetzte Software, die Echtheit des Geräts zu nutzen, wäre das Schlüsselmaterial oder die Funktionen für die kryptografischen Schlüssel Materialien enthalten die unerlaubtem Anwendung zur Verfügung.

**Manipulationen**: konnte eine Überwachung Kamera, der einen sichtbare Spektrum Überblick über eine leere Gang angezeigt wird ein Foto des solche einem Gang Vorbeugung. Ein Sensor Rauchmelder oder Feuer konnte eine Person mit einer helleren darunter reporting. In beiden Fällen wird das Gerät evtl. nicht technisch voll vertrauenswürdig in Richtung des Systems, aber bearbeitete Informationen gemeldet.

**Manipulationen**: ein Angreifer möglicherweise nutzen extrahierten Schlüsselmaterial zum Abfangen und Daten vom Gerät für den Kommunikationspfad zu unterdrücken, und Ersetzen Sie sie mit "false" Daten, die mit dem gestohlenen Schlüsselmaterial authentifiziert werden.

**Manipulationen**: ein Angreifer möglicherweise teilweise oder vollständig ersetzen die Software auf dem Gerät ausgeführte potenziell ermöglicht die ersetzte Software, die Echtheit des Geräts zu nutzen, wäre das Schlüsselmaterial oder die Funktionen für die kryptografischen Schlüssel Materialien enthalten die unerlaubtem Anwendung zur Verfügung.

**Offenlegung von Informationen**: Wenn das Gerät bearbeitete Software ausgeführt wird, bearbeitete Software kann potenziell zur Offenlegung von Daten keine unbefugten Parteien.

**Offenlegung von Informationen**: ein Angreifer möglicherweise extrahierten Schlüsselmaterial, um sich selbst in der Kommunikationspfad zwischen dem Gerät und einen Controller oder ein Feld-Gateway oder Cloud-Gateway auf off Informationen einbeziehen einfügen nutzen.

**Dienstverweigerung**: das Gerät kann deaktiviert oder aktiviert in einen Modus, in dem kein Kommunikation möglich ist (Dies ist in vielen industriellen Maschinen beabsichtigt) werden.

**Manipulationen**: das Gerät kann neu konfiguriert werden, um in einem Status unbekannt ist, an das Steuerelement-System (außerhalb von bekannten Kalibrierungsparameter) verwenden und daher bieten Daten, die nicht interpretiert werden können

**Ausweitung von Berechtigungen**: ein Gerät, das bestimmte Funktion ist kann erzwungen werden, um etwas anderes zu tun. Beispielsweise kann ein Ventil, die so programmiert ist, öffnen Sie halbem Weg dazu veranlasst werden, um ganz zu öffnen.

**Dienstverweigerung**: das Gerät aktiviert werden kann, in einem Zustand, in dem der Kommunikation nicht möglich ist.

**Manipulationen**: das Gerät kann neu konfiguriert werden, um in einem Status unbekannt ist, an das Steuerelement-System (außerhalb von bekannten Kalibrierungsparameter) verwenden und daher bieten Daten, die nicht interpretiert werden können.

**Spoofing/Manipulation/Repudiation**: Wenn Sie nicht sicher (Dies ist nur selten der Fall Steuerungselemente Remote Consumer) kann ein Angreifer die Status eines Geräts anonym verändern. Ein gutes Beispiel ist die Remote-Steuerelemente können, die alle TV aktivieren und beliebten Hackers Tools sind.

#### <a name="communication"></a>Kommunikation
Bedrohungen um Kommunikationspfad zwischen Geräten, Geräte und Feld-Gateways und Gerät und Cloud-Gateway. In der folgenden Tabelle hat hilfreiche Informationen, um offene Sockets auf dem Gerät/VPN:

| **Komponente** | **Mindern von Bedrohungen** | **Risikominderung** | **Risiko** | **Implementierung** |
| --- | --- | --- | --- | --- |
| Gerät IoT Hub |TID |(D) TLS (PSK/RSA) zur Verschlüsselung des Datenverkehrs |Lauschangriffe oder beeinträchtigen die Kommunikation zwischen dem Gerät und das gateway |Sicherheit auf der Protokollebene. Mit benutzerdefinierten Protokolle möchten wir herausfinden, wie Sie sie schützen. In den meisten Fällen erfolgt die Kommunikation vom Gerät an den IoT Hub (Gerät initiiert die Verbindung). |
| Gerät |TID |(D) TLS (PSK/RSA), um den Datenverkehr zu verschlüsseln. |Lesen von Daten während der Übertragung zwischen Geräten. Manipulation von Daten. Das Gerät bei neuen Verbindungen überladen |Sicherheit auf der Protokollebene (MQTT/AMQP/HTTP/CoAP. Mit benutzerdefinierten Protokolle möchten wir herausfinden, wie Sie sie schützen. Zur Risikominderung für die DoS-Bedrohung werden von peer-Geräte über ein Gateway Cloud oder ein Feld, und lassen Sie sie nur Act als Clients auf das Netzwerk. Das peering kann in eine direkte Verbindung zwischen den Peers nach müssen vom Gateway brokermessaging wurde führen. |
| Externe Entität Gerät |TID |Die externe Entität, auf dem Gerät starke Kopplung |Die Verbindung zum Gerät wird Lauschangriffen. Die Kommunikation beeinträchtigen mit dem Gerät |Koppeln die externe Entität, die das LE NFC/Bluetooth-Gerät sicher. Steuern der Bereich zum Betrieb des Geräts (physikalisch) |
| Feld-Gateway-Cloud-Gateway |TID |TLS (PSK/RSA), um den Datenverkehr zu verschlüsseln. |Lauschangriffe oder beeinträchtigen die Kommunikation zwischen dem Gerät und das gateway |Sicherheit auf der Protokollebene (MQTT/AMQP/HTTP/CoAP). Mit benutzerdefinierten Protokolle möchten wir herausfinden, wie Sie sie schützen. |
| Gerät Cloud-Gateway |TID |TLS (PSK/RSA), um den Datenverkehr zu verschlüsseln. |Lauschangriffe oder beeinträchtigen die Kommunikation zwischen dem Gerät und das gateway |Sicherheit auf der Protokollebene (MQTT/AMQP/HTTP/CoAP). Mit benutzerdefinierten Protokolle möchten wir herausfinden, wie Sie sie schützen. |

Hier sind einige Beispiele für die Vermeidung von Risiken in diese Kategorie:

**Dienstverweigerung**: eingeschränkte Geräte in der Regel unter DoS Bedrohung wenn damit aktiv für eingehende Verbindungen oder unerwünschte Datagramme in einem Netzwerk überwacht, da ein Angreifer öffnen kann viele Verbindungen parallel und nicht bedient werden oder bedient werden sehr langsam oder das Gerät kann mit unaufgeforderten Datenverkehr überflutet werden. In beiden Fällen kann das Gerät effektiv nicht mehr funktionsfähig, auf das Netzwerk gerendert werden.

**Spoofing, Offenlegung von Informationen**: eingeschränkte Geräte und zweckgebundene verfügen häufig über eine für alle Sicherheitsfunktionen wie Kennwort oder PIN-Schutz oder uneingeschränkt vertrauen im Netzwerk, d. h., sie werden Zugriff auf Informationen gewährt, wenn ein Gerät sich im selben Netzwerk befindet und das Netzwerk ist häufig nur durch einen gemeinsam verwendeten Schlüssel geschützt abhängig. Das bedeutet, dass der gemeinsame geheime Schlüssel auf dem Gerät oder offengelegt werden, es möglich ist, zu das Gerät steuern oder beobachten von Daten vom Gerät ausgegeben.  

**Spoofing**: ein Angreifer abfangen oder teilweise überschreiben die Übertragung und vom Absender (Man in der Mitte) vortäuschen möglicherweise

**Manipulationen**: ein Angreifer abfangen oder teilweise überschreiben die Übertragung und "false" Daten senden kann 

**Offenlegung von Informationen:** ein Angreifer möglicherweise auf einen Broadcast belauschen und Abrufen von Informationen ohne Autorisierung **Denial-of-Service:** ein Angreifer möglicherweise Papierstau broadcast Signal und deny-Verteilung von Informationen

#### <a name="storage"></a>Speicher
Jedes Gerät und Feld-Gateway ist eine Form des Speichers (temporären für Warteschlangen, die Daten, die Bildspeichers Betriebssystem (BS)).

| **Komponente** | **Mindern von Bedrohungen** | **Risikominderung** | **Risiko** | **Implementierung** |
| --- | --- | --- | --- | --- |
| Speicher des Geräts |TRID |Speicherverschlüsselung, Signieren die Protokolle |Lesen von Daten aus dem Speicher (PII-Daten), die Manipulation von Telemetriedaten. Manipulation von in die Warteschlange eingereiht oder Befehl gesteuert werden zwischengespeichert. Manipulation von Konfigurations- oder Firmware Updatepakete kann zwischengespeichert oder lokal in der Warteschlange zu OS und/oder dem System Komponenten Gefährdung führen |Verschlüsselung, Nachrichtenauthentifizierungscode (MAC) oder digitale Signatur. Wo möglich, sichere Zugriffssteuerung über Zugriff auf Ressourcen steuern, Zugriffssteuerungslisten (ACLs) oder Berechtigungen. |
| Betriebssystem des Geräts-Bild |TRID | |Manipulation von OS, / und Ersetzen Sie dabei die Betriebssystemkomponenten |Nur-Lese OS-Partition, signiert, Betriebssystem-Image, Verschlüsselung |
| Feld-Gateway-Speicher (queuing Daten) |TRID |Speicherverschlüsselung, Signieren die Protokolle |Lesen von Daten aus dem Speicher (PII-Daten), die Manipulation von Telemetriedaten, Manipulation von in die Warteschlange eingereiht oder Befehl gesteuert werden zwischengespeichert. Manipulation von Konfigurations- oder Firmware Updatepakete (bestimmt für Geräte oder Feld-Gateway) kann während zwischengespeichert oder lokal in der Warteschlange zu OS und/oder dem System Komponenten Gefährdung führen |BitLocker |
| Feld-Gateway-Betriebssystemabbilds |TRID | |Manipulation von OS, / und Ersetzen Sie dabei die Betriebssystemkomponenten |Nur-Lese OS-Partition, signiert, Betriebssystem-Image, Verschlüsselung |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Geräte- und Ereignis Verarbeitung/Cloud-Gateway-zone
Ein Cloud-Gateway ist System, das remote-Kommunikationsfehlers alte und neue Geräte oder Feld-Gateways aus mehreren unterschiedlichen Standorten über öffentliche netzwerkraums, in der Regel für ein Cloud-basierten-Steuerelement und Analyse Datensystem für einen Verbund solche Systeme kann. In einigen Fällen kann ein Cloud-Gateway sofort Zugriff auf spezielle Geräte von Terminals wie Tablets und Telefone erleichtern. Im Kontext hier erläuterten "Cloud" sollte auf einem dedizierten Datenverarbeitungssystem verweisen, die nicht am selben Standort wie der angeschlossene Geräte oder Feld-Gateways gebunden ist, und, in denen operational Measures zu verhindern, dass physischen Zugriff vorgesehen ist jedoch nicht unbedingt für eine "public" Cloudinfrastruktur.  Ein Cloud-Gateway kann potenziell ein netzwerkoverlay für die Virtualisierung, die Cloud-Gateway und alle angeschlossenen Geräte oder Feld-Gateways auf alle sonstigen Datenverkehr im Netzwerk zu isolieren zugeordnet werden. Das Cloud-Gateway selbst ist weder ein Gerät-System noch eine Verarbeitung oder für die Aufbewahrung für Gerätedaten; Diese Funktionen eine Schnittstelle mit dem Cloud-Gateway. Die Cloud-Gateway-Zone enthält die Cloud-Gateway selbst sowie alle Feld Gateways und direkt oder indirekt angeschlossene Geräte.

Cloud-Gateway ist größtenteils benutzerdefinierte erstellten Softwarekomponente, die mit dem verfügbar gemachten Endpunkte, die mit denen Feld-Gateway und Geräte eine Verbindung als Dienst ausgeführt. Daher müssen unter Berücksichtigung von Sicherheitsaspekten entworfen werden. Befolgen Sie [SDL](http://www.microsoft.com/sdl) Prozess zum Entwerfen und erstellen diesen Dienst. 

#### <a name="services-zone"></a>Dienste-zone
Eine Steuerelement-System (oder Domänencontroller) ist eine softwarelösung, die Schnittstelle mit einem Gerät oder ein Feld-Gateway oder Cloud-Gateway für ein oder mehrere Geräte zu steuern und/oder zu sammeln und/oder speichern und/oder Gerätedaten zur Präsentation oder nachfolgende Kontrolle zu analysieren. Verwaltungssysteme sind die einzigen Entitäten im Rahmen dieser Diskussion, die Interaktion mit Personen sofort erleichtern. Die Ausnahme bilden intermediate physischen Steuerelement Flächen auf Geräten, z. B. einem Switch, die eine Person, die das Gerät deaktivieren, oder anderen Eigenschaften ändern kann, und für das es kein funktionelles äquivalent, das Digital zugegriffen werden kann. 

Intermediate physischen Steuerelement Flächen sind diejenigen, in dem jegliche Art von Logik bei schränkt die Funktion des physischen Steuerelementoberfläche, dass eine entsprechende Funktion Remote initiiert werden kann oder Eingabe im Widerspruch zu remote Eingabe können vermieden werden – solche intermediated Steuerelement Flächen sind konzeptionell angefügt werden, um ein lokales System, die die gleiche zugrunde liegende Funktionalität wie jedes andere Remotesteuerung System nutzt, die das Gerät gleichzeitig angefügt werden kann. Sicherheitsrisiken im Cloud-computing kann zu lesen [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) Seite.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Finden Sie in den folgenden Artikeln für zusätzliche Informationen:

* [SDL Bedrohung Modellierungstool](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
* [Microsoft Azure IoT-Referenzarchitektur](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)

