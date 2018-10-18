---
title: Grundlegendes zu Azure IoT Edge-Zertifikaten | Microsoft-Dokumentation
description: Informationen zu Azure IoT Edge-Zertifikaten und deren Verwendung.
author: stevebus
manager: timlt
ms.author: stevebus
ms.date: 09/13/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a782a7341e53420dbc31fefc86007951df967a9e
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46307979"
---
# <a name="azure-iot-edge-certificate-usage-detail"></a>Details zur Verwendung von Azure IoT Edge-Zertifikaten

IoT Edge-Zertifikate werden für die Module und Downstream-IoT-Geräte verwendet, um Identität und Rechtmäßigkeit des [Edge Hub](iot-edge-runtime.md#iot-edge-hub)-Runtimemoduls zu überprüfen, mit dem sie eine Verbindung herstellen. Diese Überprüfungen ermöglichen eine sichere TLS-Verbindung (Transport Layer Security) zwischen Runtime, Modulen und IoT-Geräten. Wie IoT Hub selbst erfordert IoT Edge eine sichere und verschlüsselte Verbindung zwischen IoT-Downstreamgeräten (oder Blattgeräten) und IoT Edge-Modulen. Um eine sichere TLS-Verbindung herzustellen, bietet das Edge Hub-Modul eine Serverzertifikatkette zur Verbindung von Clients, damit sie seine Identität bestätigen.

Um Verwirrung über die Verwendung von Zertifikaten durch IoT Edge aufzuklären, wird in diesem Artikel erläutert, wie IoT Edge-Zertifikate in bedarfsorientierten Szenarien sowie Entwicklungs- und Testszenarien funktionieren können. Die Skripts sind zwar unterschiedlich (PowerShell oder Bash), aber die Konzepte sind bei Linux und Windows identisch.

## <a name="iot-edge-certificates"></a>IoT Edge-Zertifikate

In den meisten Fällen sind Hersteller Entitäten, die von Endbenutzern eines Edge-Geräts unabhängig sind. Manchmal ist die einzige Beziehung zwischen Hersteller und Operator der Erwerb eines handelsüblichen Edge-Geräts. In anderen Fällen ist der Hersteller vertraglich verpflichtet, ein Edge-Gerät für den Operator zu erstellen. Der Entwurf des IoT Edge-Zertifikats versucht, beide Szenarien in Betracht zu ziehen.

Die folgende Abbildung veranschaulicht die IoT Edge-Verwendung von Zertifikaten. 0 (null), ein oder mehrere Zwischensignaturzertifikate sind zwischen dem Zertifikat der Stammzertifizierungsstelle und dem Zertifikat der Gerätezertifizierungsstelle möglich. Dies ist von der Anzahl der beteiligten Entitäten abhängig – in unserem Beispiel ist nur eine Entität beteiligt.

![Diagramm typischer Zertifikatsbeziehungen](./media/iot-edge-certs/edgeCerts-general.png)

### <a name="certificate-authority"></a>Zertifizierungsstelle

Die Zertifizierungsstelle, kurz „ZS“ genannt (Certificate Authority, CA), ist eine Entität, die digitale Zertifikate ausstellt. Eine Zertifizierungsstelle fungiert als vertrauenswürdige dritte Partei zwischen Besitzer und Empfänger des Zertifikats. Ein digitales Zertifikat zertifiziert den Empfänger des Zertifikats als Besitzer eines öffentlichen Schlüssels. Die Zertifikatsvertrauenskette stellt zunächst ein Stammzertifikat aus, das in allen von der Zertifizierungsstelle ausgestellten Zertifikaten als Grundlage der Vertrauensstellung gilt. Danach kann der Besitzer mit dem Stammzertifikat zusätzliche Zwischenzertifikate („untergeordnete Zertifikate“) ausgeben.

### <a name="root-ca-certificate"></a>Zertifikat der Stammzertifizierungsstelle

Ein Zertifikat der Stammzertifizierungsstelle ist der Vertrauensanker des gesamten Prozesses. In Produktionsszenarien ist dies in der Regel ein ZS-Zertifikat, das bei einer vertrauenswürdigen kommerziellen Zertifizierungsstelle wie Baltimore, Verisign, DigiCert usw. erworben wurde. Wenn Sie vollständige Kontrolle über die Geräte haben, die mit Ihren Edge-Geräten verbunden sind, kann eine Zertifizierungsstelle auf Unternehmensebene verwendet werden. In jedem Fall ist die gesamte Zertifikatkette ab dem Edge Hub ununterbrochen, sodass die Blatt-IoT-Geräte dem Stammzertifikat vertrauen müssen. Sie können das Zertifikat der Stammzertifizierungsstelle entweder im Speicher der vertrauenswürdigen Stammzertifizierungsstelle speichern oder die Details des Zertifikats der Stammzertifizierungsstelle in Ihrem Code bereitstellen, wenn der IoT-Client eine Verbindung mit IoT Edge herstellt.

### <a name="intermediate-certificates"></a>Zwischenzertifikat(e)

In einem typischen Fertigungsprozess zum Erstellen sicherer Geräte werden die Zertifikate der Stammzertifizierungsstelle selten direkt verwendet – in erster Linie wg. des Risikos von Datenlecks oder Offenlegung. In der Regel werden ein oder mehrere Zwischenzertifikate der Zertifizierungsstellen erstellt und digital vom Zertifikat der Stammzertifizierungsstelle signiert. Es kann nur ein Zwischenzertifikat oder eine Kette dieser Zwischenzertifikate vorhanden sein. Folgende Szenarien erfordern z.B. eine Kette von Zwischenzertifikaten:

* Eine Hierarchie von Abteilungen innerhalb eines Herstellers.

* Mehrere Unternehmen sind nacheinander an der Produktion eines Geräts beteiligt.

* Ein Kunde erwirbt ein Zertifikat einer Stammzertifizierungsstelle und leitet ein Signaturzertifikat für den Hersteller ab, womit der die Geräte signiert, die er im Auftrag des Kunden herstellt.

In jedem Fall verwendet der Hersteller am Ende dieser Kette ein Zertifikat der Zwischenzertifizierungsstelle, um das am Endgerät platzierte Zertifikat der Gerätezertifizierungsstelle zu signieren. Im Allgemeinen werden diese Zwischenzertifikate in der Fertigungsanlage streng geheim gehalten. Zur Verwendung durchlaufen sie strenge, sowohl physische als auch elektronische Prozesse.

### <a name="device-ca-certificate"></a>Zertifikat der Gerätezertifizierungsstelle

Das Zertifikat der Gerätezertifizierungsstelle wird aus dem letzten Zertifikat der Zwischenzertifizierungsstelle im Prozess generiert und davon signiert. Dieses Zertifikat wird auf dem Edge-Gerät selbst installiert, vorzugsweise in einem sicheren Speicherort wie z.B. einem Hardwaresicherheitsmodul (Hardware Security Module, HSM). Darüber hinaus identifiziert ein Zertifikat der Gerätezertifizierungsstelle eindeutig ein IoT Edge-Gerät. Für IoT Edge kann das Zertifikat der Gerätezertifizierungsstelle andere Zertifikate ausstellen. Das Zertifikat der Gerätezertifizierungsstelle stellt beispielsweise Blattgerätezertifikate aus, mit denen Geräte beim [Azure IoT Hub Device Provisioning-Dienst](..\iot-dps\about-iot-dps.md) authentifiziert werden.

### <a name="iot-edge-workload-ca"></a>IoT Edge-Workloadzertifizierungsstelle

Dieses Zertifikat, das erste auf der „Operator“-Seite des Prozesses, wird beim ersten Start von IoT Edge vom [Azure IoT Edge-Sicherheits-Manager](iot-edge-security-manager.md) generiert. Dieses Zertifikat wird aus dem „Zertifikat der Gerätezertifizierungsstelle“ generiert und davon signiert. Mit diesem Zertifikat, das nur ein anderes Zwischensignaturzertifikat ist, werden alle anderen von der IoT Edge-Runtime verwendeten Zertifikate generiert und signiert. Zurzeit ist dies in erster Linie das im folgenden Abschnitt erläuterte Edge Hub-Serverzertifikat, aber in Zukunft könnten auch andere Zertifikate für die Authentifizierung von IoT Edge-Komponenten infrage kommen.

### <a name="edge-hub-server-certificate"></a>Edge Hub-Serverzertifikat

Das Edge Hub-Serverzertifikat ist das eigentliche Zertifikat, das Blattgeräten und Modulen während des Herstellens der von IoT Edge geforderten TLS-Verbindung zur Identitätsüberprüfung präsentiert wird. Dieses Zertifikat stellt die vollständige Kette der zu seinem Generieren verwendeten Signaturzertifikate dar – bis zum Zertifikat der Stammzertifizierungsstelle, dem das IoT-Blattgerät vertrauen muss. Beim Generieren durch den IoT Edge-Sicherheits-Manager wird als allgemeiner Name (Common Name, CN) dieses Edge Hub-Zertifikats nach der Konvertierung in Kleinbuchstaben der Name festgelegt, der von der Eigenschaft „hostname“ in der Datei „config.yaml“ bereitgestellt wird. Dies ist eine häufige Ursache für eine Verwechslung mit IoT Edge.

## <a name="production-implications"></a>Auswirkungen auf die Produktion

Eine angemessene Frage wäre: „Warum benötigt IoT Edge das zusätzliche Zertifikat der „Workloadzertifizierungsstelle“? Könnte IoT Edge nicht das Zertifikat der Gerätezertifizierungsstelle verwenden, um direkt das Edge Hub-Serverzertifikat zu generieren?“. Technisch gesehen, ja. Allerdings soll dieses „Workload“-Zwischenzertifikat“ zur Abgrenzung zwischen den Interessen des Herstellers des Geräts und des Operators des Geräts dienen. Stellen Sie sich ein Szenario vor, in dem ein IoT Edge-Gerät verkauft oder von einem Kunden an einen anderen übertragen wird. Sie wünschen wahrscheinlich, dass das vom Hersteller bereitgestellte Zertifikat der Gerätezertifizierungsstelle unveränderlich ist. Die für den Betrieb des Geräts spezifischen „Workload“-Zertifikate sollten jedoch gelöscht und für die neue Bereitstellung neu erstellt werden.

Da Hersteller- und Operatorprozesse getrennt sind, müssen beim Veröffentlichen von IoT Edge für Produktionsgeräte ein paar Auswirkungen berücksichtigt werden.

* Mit einem beliebigen zertifikatbasierten Prozess sollten das Zertifikat der Stammzertifizierungsstelle und alle Zertifikate der Zwischenzertifizierungsstelle während des gesamten Prozesses des Veröffentlichens auf einem IoT Edge-Gerät geschützt und überwacht werden. Der Hersteller des IoT Edge-Geräts sollten über sichere Prozesse für richtige Speicherung und Nutzung ihrer Zwischenzertifikate verfügen. Darüber hinaus sollte das Zertifikat der Gerätezertifizierungsstelle in so sicherem Speicher wie auf dem Gerät selbst möglich gespeichert werden, vorzugsweise in einem Hardwaresicherheitsmodul.

* Da das Edge Hub-Serverzertifikat den die Verbindung herstellenden Clientgeräten und Modulen von Edge Hub präsentiert wird, darf der CN des Zertifikats der Gerätezertifizierungsstelle mit dem „hostname“, der in „config.yaml“ auf dem IoT Edge-Gerät verwendet wird, **nicht identisch** sein. Der Name, der von Clients zum Herstellen der Verbindung mit IoT Edge (z.B. über den GatewayHostName-Parameter der Verbindungszeichenfolge oder den Befehl CONNECT in MQTT) verwendet wird, **darf nicht** mit dem im Zertifikat der Gerätezertifizierungsstelle verwendeten allgemeinen Namen identisch sein. Der Grund ist, dass Edge Hub die gesamte Zertifikatskette für die Überprüfung durch Clients präsentiert. Wenn das Edge Hub-Serverzertifikat und das Zertifikat der Gerätezertifizierungsstelle beide über denselben CN verfügen, geraten Sie in eine Überprüfungsschleife, und das Zertifikat wird ungültig.

* Da das Zertifikat der Gerätezertifizierungsstelle vom IoT Edge-Sicherheitsdaemon zum Generieren der endgültigen IoT Edge-Zertifikate verwendet wird, muss es selbst ein Signaturzertifikat sein, also Funktionen für die Zertifikatsignatur besitzen. Um dies zu einem Zertifikat der Signaturzertifizierungsstelle zu machen, richtet das Anwenden von „V3 Basic constraints CA:True“ auf das Zertifikat der Gerätezertifizierungsstelle im Wesentlichen automatisch die erforderlichen Schlüsselverwendungseigenschaften ein.

>[!Tip]
> Wenn Sie bereits die Einrichtung von IoT Edge als transparentes Gateway in einem Entwicklungs-/Testszenario unter Verwendung unserer „Komfortskripts“ (siehe nächster Abschnitt) durchgeführt und den gleichen Hostnamen beim Erstellen des Zertifikats der Gerätezertifizierungsstelle, wie für den Hostnamen in „config.yaml“, verwendet haben, haben Sie sich vielleicht gefragt, warum es funktioniert. Um die Entwicklererfahrung zu verbessern, tragen Komfortskripts die Erweiterung „.ca“ am Ende des Namens, den Sie an das Skript übergeben. Wenn Sie also z.B. für beide Gerätenamen in den Skripts und im Hostnamen in „config.yaml“ „mygateway“ verwendet haben, erfolgt vor der Verwendung als CN für das Zertifikat der Gerätezertifizierungsstelle die Umwandlung in „mygateway.ca“.

## <a name="devtest-implications"></a>Auswirkungen auf Entwicklung/Test

Um Entwicklungs- und Testszenarien zu vereinfachen, bietet Microsoft eine Reihe von [Komfortskripts](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) zum Generieren von nicht zur Produktion bestimmten, für IoT Edge im transparenten Gatewayszenario geeignete Zertifikate an.

Diese Skripts generieren Zertifikate, die der in diesem Artikel erläuterten Zertifikatskettenstruktur folgen. Für [Linux](how-to-create-transparent-gateway-linux.md#certificate-creation) oder [Windows](how-to-create-transparent-gateway-windows.md#certificate-creation). Die folgenden Befehle generieren das „Zertifikat der Stammzertifizierungsstelle“ und ein einzelnes „Zertifikat der Zwischenzertifizierungsstelle“.

```bash
./certGen.sh create_root_and_intermediate 
```

```Powershell
New-CACertsCertChain rsa 
```

Diese Befehle generieren ebenso das „Zertifikat der Gerätezertifizierungsstelle“.

```bash
./certGen.sh create_edge_device_certificate "<gateway device name>"
```

```Powershell
New-CACertsEdgeDevice "<gateway device name>"
```

* Der diesen Skripts übergebene **\<Gatewaygerätename\>** **darf nicht** mit dem Parameter „hostname“ in „config.yaml“ identisch sein. Wie bereits erwähnt, vermeiden Sie mit den Skripts durch Anhängen der Zeichenfolge „.ca“ an den **\<Gatewaygerätenamen\>** Probleme, indem Namenskonflikte verhindert werden, falls ein Benutzer Edge an beiden Orten mit dem gleichen Namen einrichtet – allerdings sollte die Verwendung des gleichen Namens vermieden werden.

>[!Tip]
> Um Verbindungen Ihrer Geräte-IoT-„Blattgeräte“ und Anwendungen herzustellen, die unser IoT-Geräte-SDK über IoT Edge verwenden, müssen Sie den optionalen GatewayHostName-Parameter dem Ende der Verbindungszeichenfolge des Geräts hinzufügen. Wenn das Edge Hub-Serverzertifikat generiert wird, basiert es auf einer Kleinbuchstabenversion des Hostnamens aus „config.yaml“ – damit die Namen übereinstimmen und die Überprüfung der TLS-Zertifikats erfolgreich ausgeführt werden kann, sollten Sie darum den GatewayHostName-Parameter in Kleinbuchstaben eingeben.

## <a name="example-of-iot-edge-certificate-hierarchy"></a>Beispiel für IoT Edge-Zertifikathierarchie

Ein Beispiel dieses Zertifikatspfads veranschaulicht der folgende Screenshot eines ausgeführten IoT Edge-Geräts, das als transparentes Gateway eingerichtet ist. OpenSSL wird verwendet, um eine Verbindung mit Edge Hub herzustellen, zu überprüfen und die Zertifikate zu sichern.

![Screenshot der Zertifikatshierarchie auf jeder Ebene](./media/iot-edge-certs/iotedge-cert-chain.png)

Sie sehen die Hierarchie der Zertifikatstiefe im Screenshot dargestellt:

| Zertifikat der Stammzertifizierungsstelle         | Zertifikat der Azure IoT Hub-Zertifizierungsstelle – nur Test                                                                           |
|-----------------------------|-----------------------------------------------------------------------------------------------------------|
| Zertifikat der Zwischenzertifizierungsstelle | Zertifikat der Azure IoT Hub-Zwischenzertifizierungsstelle – nur Test                                                                 |
| Zertifikat der Gerätezertifizierungsstelle       | iotgateway.ca („iotgateway“ wurde als <Gatewayhostname> den Komfortskripts übergeben)      |
| Zertifikat der Workloadzertifizierungsstelle     | IoT Edge-Workloadzertifizierungsstelle                                                                                       |
| Edge Hub-Serverzertifikat | iotedgegw.Local (entspricht dem „Hostname“ aus „config.yaml“)                                                |

## <a name="next-steps"></a>Nächste Schritte

[Grundlegendes zu Azure IoT Edge-Modulen](iot-edge-modules.md)

[Verwenden eines IoT Edge-Geräts als transparentes Gateway (Linux)](how-to-create-transparent-gateway-linux.md)

[Verwenden eines IoT Edge-Geräts als transparentes Gateway (Windows)](how-to-create-transparent-gateway-windows.md)
