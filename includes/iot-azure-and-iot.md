
# <a name="azure-and-the-internet-of-things"></a>Azure und das Internet der Dinge

Willkommen bei Microsoft Azure und dem Internet der Dinge (IoT). In diesem Artikel werden die häufigsten Merkmale einer IoT-Lösung in der Cloud beschrieben. IoT-Lösungen sind auf eine sichere, bidirektionale Kommunikation zwischen unzähligen Geräten und einem Lösungs-Back-End angewiesen. Eine Lösung kann beispielsweise mithilfe einer automatisierten, prognoseorientierten Analyse Informationen zum Ereignisdatenstrom zwischen Ihrem Gerät und der Cloud ermitteln.

## <a name="iot-solution-architecture"></a>Architektur einer IoT-Lösung

Im folgenden Diagramm sind die wichtigsten Elemente der typischen Architektur einer IoT-Lösung dargestellt. Das Diagramm gilt unabhängig von den spezifischen Implementierungsdetails, z.B. den verwendeten Azure-Diensten, und den Gerätebetriebssystemen. In dieser Architektur werden von IoT-Geräten Daten erfasst und an ein Cloud-Gateway gesendet. Das Cloudgateway macht die Daten für die Verarbeitung durch andere Back-End-Dienste verfügbar. Mit diesen Back-End-Diensten können Daten für folgende Ziele bereitgestellt werden:

* Andere Branchenanwendungen
* Per Dashboard für menschliche Bediener oder andere Darstellungsgeräte

![Architektur einer IoT-Lösung][img-solution-architecture]

> [!NOTE]
> Ausführliche Informationen zur IoT-Architektur finden Sie unter [Microsoft Azure IoT Reference Architecture (Architektur der Microsoft Azure IoT-Referenz)][lnk-refarch].

### <a name="device-connectivity"></a>Gerätekonnektivität

Bei einer Architektur für IoT-Lösungen senden Geräte normalerweise Telemetriedaten zur Speicherung und Verarbeitung in die Cloud. In einem Szenario mit vorbeugender Wartung verwendet das Lösungs-Back-End beispielsweise ggf. den Sensordatenstrom, um zu ermitteln, wann eine bestimmte Pumpe gewartet werden muss. Geräte können auch Cloud-zu-Gerät-Nachrichten (Cloud-to-Device, C2D) empfangen und darauf reagieren, indem Nachrichten von einem Cloudendpunkt gelesen werden. In demselben Beispiel kann das Lösungs-Back-End beispielsweise Nachrichten an andere Pumpen im Pumpwerk senden, sodass kurz vor der fälligen Wartung mit der Umleitung des Wassers begonnen wird. So kann der Wartungstechniker gleich nach seiner Ankunft mit der Arbeit beginnen.

Das Herstellen von sicheren und zuverlässigen Verbindungen zwischen Geräten ist bei IoT-Lösungen häufig die größte Herausforderung. Der Grund hierfür ist, dass IoT-Geräte im Vergleich zu anderen Clients, z.B. Browsern und mobilen Apps, andere Merkmale aufweisen. Für IoT-Geräte gilt beispielsweise Folgendes:

* Es handelt sich häufig um eingebettete Systeme ohne menschlichen Bediener (im Gegensatz zu einem Smartphone).
* Sie können an Remotestandorten bereitgestellt werden, an denen der physische Zugriff mit hohen Kosten verbunden ist.
* Sie sind unter Umständen nur über das Back-End der Lösung erreichbar. Es gibt keine andere Möglichkeit zur Interaktion mit dem Gerät.
* Sie verfügen ggf. über eingeschränkte Stromversorgungs- und Verarbeitungsressourcen.
* Sie verfügen ggf. über intermittierende, langsame oder teure Netzwerkverbindungen.
* Sie müssen unter Umständen proprietäre, benutzerdefinierte oder branchenspezifische Anwendungsprotokolle verwenden.
* Sie können mit vielen gängigen Hardware- und Softwareplattformen erstellt werden.

Zusätzlich zu den oben genannten Einschränkungen müssen alle IoT-Lösungen außerdem skalierbar, sicher und zuverlässig sein.

Je nach Kommunikationsprotokoll und Netzwerkverfügbarkeit kann ein Gerät entweder direkt oder über ein zwischengeschaltetes Gateway mit der Cloud kommunizieren. IoT-Architekturen verfügen häufig über eine Mischung dieser beiden Kommunikationsmuster.

### <a name="data-processing-and-analytics"></a>Datenverarbeitung und Analysen

In modernen IoT-Lösungen kann die Datenverarbeitung in der Cloud oder auf dem Gerät erfolgen. Die geräteseitige Verarbeitung wird als *Edge Computing* bezeichnet. Die Wahl des Orts, an dem Daten verarbeitet werden, richtet sich beispielsweise nach den folgenden Faktoren:

* Netzwerkeinschränkungen: Wenn die Bandbreite zwischen den Geräten und der Cloud begrenzt ist, besteht der Anreiz, mehr Verarbeitungsschritte im Edge-Bereich auszuführen.
* Reaktionszeit: Falls die Anforderung besteht, für ein Gerät nahezu in Echtzeit zu reagieren, kann es besser sein, die Verarbeitung der erforderlichen Schritte auf dem Gerät selbst durchzuführen. Ein Beispiel hierfür ist die Bewegung eines Roboterarms, die im Notfall gestoppt werden muss.
* Rechtliche Umgebung: Einige Daten können nicht in die Cloud gesendet werden.

Im Allgemeinen ist die Datenverarbeitung sowohl im Edge-Bereich als auch in der Cloud eine Kombination aus den folgenden Funktionen:

* Es werden bedarfsgesteuert Telemetriedaten von Ihren Geräten empfangen, und es wird bestimmt, wie diese Daten verarbeitet und gespeichert werden sollen.
* Die Telemetriedaten werden analysiert, um Erkenntnisse zu gewinnen (entweder in Echtzeit oder später).
* Es werden Befehle aus der Cloud oder von einem Gatewaygerät an ein bestimmtes Gerät gesendet.

Außerdem sollte ein IoT-Cloud-Back-End Folgendes bereitstellen:

* Funktionen für die Geräteregistrierung, die Folgendes ermöglichen:
    * Bereitstellen von Geräten
    * Steuern, für welche Geräte das Herstellen einer Verbindung mit Ihrer Infrastruktur zulässig ist
* Geräteverwaltung zur Steuerung des Status Ihrer Geräte und zur Überwachung der damit verbundenen Aktivitäten

In einem Szenario mit der vorhersagbaren Wartung werden mit dem Cloud-Back-End beispielsweise Telemetrieverlaufsdaten gespeichert. Von der Lösung werden diese Daten verwendet, um mögliches anomales Verhalten für bestimmte Pumpen zu identifizieren, bevor ein echtes Problem entsteht. Per Datenanalyse kann identifiziert werden, dass die Vorbeugungslösung darin besteht, einen Befehl zurück an das Gerät zu senden, um eine Korrekturmaßnahme durchzuführen. Bei diesem Prozess wird eine automatisierte Feedbackschleife zwischen dem Gerät und der Cloud generiert, mit der die Effizienz der Lösung deutlich gesteigert wird.

### <a name="presentation-and-business-connectivity"></a>Präsentations- und Geschäftskonnektivität

Die Präsentations- und Geschäftskonnektivitätsebene ermöglicht Endbenutzern die Interaktion mit der IoT-Lösung und den Geräten. Damit können die Benutzer die auf den Geräten erfassten Daten anzeigen und analysieren. Diese Sichten können in Form von Dashboards oder BI-Berichten vorkommen, die jeweils historische Daten oder Daten in Echtzeit anzeigen. Ein Bediener kann z.B. den Status eines bestimmten Pumpwerks überprüfen und alle Warnungen anzeigen, die vom System ausgelöst wurden. Diese Ebene ermöglicht auch die Integration des IoT-Lösungs-Back-Ends in vorhandene Branchenanwendungen, um es in Geschäftsprozesse oder Workflows im Unternehmen einzubinden. Beispielsweise kann eine Lösung zur vorbeugenden Wartung mit einem Terminplanungssystem integriert werden, um einen Techniker in ein Pumpwerk zu bestellen, sobald angezeigt wird, dass für eine Pumpe Wartungsbedarf besteht.

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-iot-hub]: ../articles/iot-hub/iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: ../articles/iot-suite/iot-suite-overview.md
[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
