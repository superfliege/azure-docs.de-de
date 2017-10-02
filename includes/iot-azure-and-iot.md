
# <a name="azure-and-internet-of-things"></a>Azure und Internet der Dinge

Willkommen bei Microsoft Azure und dem Internet der Dinge (IoT). In diesem Artikel werden die allgemeinen Merkmale einer IoT-Lösung vorgestellt, die Sie mithilfe von Azure-Diensten bereitstellen können. IoT-Lösungen sind auf eine sichere, bidirektionale Kommunikation zwischen zahlreichen Geräten und einem Lösungs-Back-End angewiesen. Das Lösungs-Back-End kann mithilfe automatisierter, prognosebasierter Analysen Informationen zu Ihrem D2C-Ereignisdatenstrom ermitteln.

[Azure IoT Hub][lnk-iot-hub] ist ein zentraler Baustein jeder IoT-Lösung mit Azure-Diensten. IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Lösungs-Back-End ermöglicht. 

Die [Azure IoT Suite][lnk-iot-suite] bietet umfassende End-to-End-Implementierungen dieser Architektur für spezifische IoT-Szenarien. Beispiel:

* Mit der *Remoteüberwachung* können Sie den Status von Geräten (etwa Verkaufsautomaten) überwachen.
* *Predictive Maintenance* hilft Ihnen dabei, den Wartungsbedarf von Geräten (etwa von Pumpen in abgelegenen Pumpwerken) zu antizipieren und ungeplante Downtime zu vermeiden.
* Mit der *Connected Factory*-Lösung können Sie eine Verbindung mit Ihren Industrieanlagen herstellen und diese Anlagen überwachen.

## <a name="iot-solution-architecture"></a>Architektur einer IoT-Lösung

Das folgende Diagramm zeigt die Architektur einer typischen IoT-Lösung. Es enthält keine Namen von bestimmten Azure-Diensten, sondern beschreibt die wichtigsten Elemente in der Architektur einer generischen IoT-Lösung. In dieser Architektur werden von IoT-Geräten Daten erfasst und an ein Cloudgateway gesendet. Das Cloudgateway macht die Daten für die Verarbeitung durch andere Back-End-Dienste verfügbar. Das Lösungs-Back-End stellt Daten für Branchenanwendungen oder über ein Dashboard oder einen Bericht für Bedienpersonal zur Verfügung.

![Architektur einer IoT-Lösung][img-solution-architecture]

> [!NOTE]
> Ausführliche Informationen zur IoT-Architektur finden Sie unter [Microsoft Azure IoT Reference Architecture (Architektur der Microsoft Azure IoT-Referenz)][lnk-refarch].

### <a name="device-connectivity"></a>Gerätekonnektivität

In dieser IoT-Lösung senden Geräte Telemetriedaten wie etwa Sensormesswerte von einem Pumpwerk an einen Cloudendpunkt, wo sie gespeichert und verarbeitet werden. In einem Szenario mit vorbeugender Wartung verwendet das Lösungs-Back-End möglicherweise den Sensordatenstrom, um zu ermitteln, wann eine bestimmte Pumpe gewartet werden muss. Geräte können auch Cloud-zu-Gerät-Nachrichten (Cloud-to-Device, C2D) empfangen und darauf reagieren, indem Nachrichten von einem Cloudendpunkt gelesen werden. Im Szenario mit vorbeugender Wartung kann das Lösungs-Back-End beispielsweise Nachrichten an andere Pumpen im Pumpwerk senden, sodass kurz vor der fälligen Wartung mit der Umleitung des Wassers begonnen wird. So kann der Wartungstechniker umgehend mit der Behebung des Problems beginnen.

Eines der größten Probleme im Zusammenhang mit IoT-Projekten ist die sichere und zuverlässige Verbindung von Geräten mit dem Lösungs-Back-End. IoT-Geräte weisen andere Merkmale als andere Clients wie Browser und mobile Apps auf. IoT-Geräte:

* Es handelt sich häufig um eingebettete Systeme ohne menschlichen Bediener.
* Sie können an Remotestandorten bereitgestellt werden, an denen der physische Zugriff mit hohen Kosten verbunden ist.
* Sie sind unter Umständen nur über das Back-End der Lösung erreichbar. Es gibt keine andere Möglichkeit zur Interaktion mit dem Gerät.
* Sie verfügen ggf. über eingeschränkte Stromversorgungs- und Verarbeitungsressourcen.
* Sie verfügen ggf. über intermittierende, langsame oder teure Netzwerkverbindungen.
* Sie müssen unter Umständen proprietäre, benutzerdefinierte oder branchenspezifische Anwendungsprotokolle verwenden.
* Sie können mit vielen gängigen Hardware- und Softwareplattformen erstellt werden.

Zusätzlich zu den obigen Anforderungen muss jede IoT-Lösung auch Skalierbarkeit, Sicherheit und Zuverlässigkeit bieten. Die sich ergebenden Verbindungsanforderungen sind schwierig und zeitaufwändig zu implementieren, wenn herkömmliche Technologie verwendet wird, z. B. Webcontainer und Nachrichtenbroker. Azure IoT Hub und die Azure IoT-Geräte-SDKs vereinfachen die Implementierung von Lösungen, die diese Anforderungen erfüllen.

Ein Gerät kann direkt mit dem Endpunkt eines Cloudgateways kommunizieren. Sollte das Gerät mit keinem der vom Cloudgateway unterstützten Kommunikationsprotokolle kompatibel sein, kann die Verbindung auch über ein Zwischengateway hergestellt werden. So kann beispielsweise das [Azure IoT-Protokollgateway][lnk-protocol-gateway] das Protokoll übersetzen, falls Geräte keines der von IoT Hub unterstützten Protokolle verwenden können.

### <a name="data-processing-and-analytics"></a>Datenverarbeitung und Analysen

In der Cloud findet ein Großteil der Datenverarbeitung in einem IoT-Lösungs-Back-End statt. Das IoT-Lösungs-Back-End:

* Es empfängt enorme Mengen an Telemetriedaten von Ihren Geräten und bestimmt, wie diese Daten verarbeitet und gespeichert werden sollen. 
* Es ermöglicht unter Umständen das Senden von Befehlen aus der Cloud an bestimmte Geräte.
* Es bietet Registrierungsfunktionen, mit denen Sie Geräte bereitstellen sowie steuern können, welche Geräte eine Verbindung mit Ihrer Infrastruktur herstellen können.
* Mit dem Back-End können Sie den Status Ihrer Geräte nachverfolgen und deren Aktivitäten überwachen.

Im Szenario mit der vorhersagbaren Wartung speichert das Lösungs-Back-End historische Telemetriedaten. Auf der Grundlage dieser Daten kann das Lösungs-Back-End Muster erkennen, die darauf hindeuten, dass bei einer bestimmten Pumpe eine Wartung fällig ist.

IoT-Lösungen können automatische Feedback-Schleifen enthalten. Beispielsweise kann ein Analysemodul im Lösungs-Back-End anhand der Telemetriedaten ermitteln, dass die Temperatur eines bestimmten Geräts über der normalen Betriebstemperatur liegt. Daraufhin kann die Lösung dann einen Befehl an das Gerät senden und Korrekturmaßnahmen einleiten.

### <a name="presentation-and-business-connectivity"></a>Präsentations- und Geschäftskonnektivität

Die Präsentations- und Geschäftskonnektivitätsebene ermöglicht Endbenutzern die Interaktion mit der IoT-Lösung und den Geräten. Damit können die Benutzer die auf den Geräten erfassten Daten anzeigen und analysieren. Bei diesen Sichten kann es sich um Dashboards oder Berichte handeln, die sowohl historische Daten als auch aktuelle Daten enthalten können. Ein Bediener kann etwa den Status eines bestimmten Pumpwerks überprüfen und alle Warnungen anzeigen, die vom System ausgelöst wurden. Diese Ebene ermöglicht auch die Integration des IoT-Lösungs-Back-Ends in vorhandene Line-of-Business-Anwendungen, um es in Geschäftsprozesse oder Workflows im Unternehmen einzubinden. So lässt sich beispielsweise die Predictive Maintenance-Lösung in ein Terminplansystem integriert werden. Dadurch kann ein Techniker in ein Pumpwerk bestellt werden, wenn die Lösung feststellt, dass eine Pumpe gewartet werden muss.

![IoT-Lösungsdashboard][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-iot-hub]: ../articles/iot-hub/iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: ../articles/iot-suite/iot-suite-overview.md
[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
