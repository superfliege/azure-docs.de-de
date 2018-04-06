---
title: 'IoT Hub Device Provisioning-Dienst: Konzepte für automatische Bereitstellung'
description: Dieser Artikel bietet einen konzeptionellen Überblick über die Phasen der automatischen Gerätebereitstellung mit dem IoT Device Provisioning-Dienst, IoT Hub und Client-SDKs.
services: iot-dps
keywords: ''
author: BryanLa
ms.author: bryanla
ms.date: 03/27/2018
ms.topic: conceptual
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: ''
ms.openlocfilehash: cd458b1f6d26fbd5f5821a04cd01be5c3a4e4514
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="auto-provisioning-concepts"></a>Konzepte für die automatische Bereitstellung

Wie in der [Übersicht](about-iot-dps.md) beschrieben, ist der Device Provisioning-Dienst ein Hilfsdienst, der die Just-in-Time-Bereitstellung von Geräten für einen IoT Hub ermöglicht, ohne dass menschliches Eingreifen erforderlich ist. Nach erfolgreicher Bereitstellung verbinden sich die Geräte direkt mit dem dafür vorgesehenen IoT Hub. Dieser Vorgang wird als automatische Bereitstellung bezeichnet und bietet eine sofort einsatzbereite Registrierung und erste Konfiguration für Geräte.

## <a name="overview"></a>Übersicht

Die automatische Azure IoT-Bereitstellung kann in drei Phasen unterteilt werden:

1. **Dienstkonfiguration**: Eine einmalige Konfiguration des Azure IoT Hub und der Instanzen des IoT Hub Device Provisioning-Diensts, die deren Einrichtung und das Erstellen einer Verbindung zwischen ihnen bewirkt.

   > [!NOTE]
   > Unabhängig von der Größe Ihrer IoT-Lösung (selbst wenn Sie vorhaben, Millionen von Geräten zu unterstützen) ist dies eine **einmalige Konfiguration**.

2. **Geräteregistrierung**: Der Vorgang, durch den die Instanz des Device Provisioning-Diensts die Geräte erkennt, die in Zukunft versuchen werden, sich zu registrieren. Die Registrierung erfolgt durch die Konfiguration von Geräteidentitätsinformationen im Bereitstellungsdienst, entweder als „individuelle Registrierung“ für ein einzelnes Gerät oder als „Gruppenregistrierung“ für mehrere Geräte. Die Identität basiert auf dem [Bestätigungsmechanismus](concepts-security.md#attestation-mechanism), den das Gerät verwenden soll, der es dem Bereitstellungsdienst ermöglicht, die Echtheit des Geräts während der Registrierung zu bestätigen:

   - **TPM**: Als „individuelle Registrierung“ konfiguriert, basiert die Geräteidentität auf der TPM-Registrierungs-ID und dem öffentlichen Endorsement Key. Da es sich bei TPM um eine [Spezifikation]((https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)) handelt, erwartet der Dienst nur eine Bestätigung gemäß der Spezifikation, und zwar unabhängig von der TPM-Implementierung (Hardware oder Software). Ausführliche Informationen zur TPM-basierten Bestätigung finden Sie unter [Gerätebereitstellung: Identitätsbestätigung mit TPM](https://azure.microsoft.com/blog/device-provisioning-identity-attestation-with-tpm/). 

   - **X509**: Konfiguriert als „individuelle Registrierung“ oder „Gruppenregistrierung“, basiert die Geräteidentität auf einem digitalen X.509-Zertifikat, das als PEM- oder CER-Datei in die Registrierung hochgeladen wird.

   > [!IMPORTANT]  
   > Obwohl dies keine Voraussetzung für die Verwendung der Device Provisioning-Dienste ist, wird dringend empfohlen, dass Ihr Gerät ein Hardwaresicherheitsmodul (HSM) verwendet, um sensible Geräteidentitätsinformationen wie Schlüssel und X.509-Zertifikate zu speichern.

3. **Geräteregistrierung und -konfiguration**: Wird beim Starten durch Registrierungssoftware initiiert, die mit einem Client-SDK des Device Provisioning-Diensts erstellt wird, das für das Gerät und den Bestätigungsmechanismus geeignet ist. Die Software stellt eine Verbindung mit dem Bereitstellungsdienst für die Authentifizierung des Geräts und die anschließende Registrierung im IoT Hub her. Nach erfolgreicher Registrierung erhält das Gerät seine eindeutige Geräte-ID für den IoT Hub und Verbindungsinformationen, sodass es seine ursprüngliche Konfiguration aufrufen und den Telemetrieprozess starten kann. In Produktionsumgebungen kann diese Phase Wochen oder Monate nach den beiden vorangegangenen Phasen erfolgen.

## <a name="roles-and-operations"></a>Rollen und Vorgänge

Die im vorhergehenden Abschnitt beschriebenen Phasen können sich über Wochen oder Monate erstrecken, bedingt durch Produktionsrealitäten wie Fertigungszeit, Versand, Zollabwicklung usw. Darüber hinaus können sie Aktivitäten auf mehrere Rollen verteilen, wenn die verschiedenen beteiligten Entitäten berücksichtigt werden. In diesem Abschnitt werden die verschiedenen Rollen und Vorgänge der einzelnen Phasen genauer untersucht, und der Ablauf wird in einem Sequenzdiagramm dargestellt. 

Die automatische Bereitstellung stellt auch Anforderungen an den Gerätehersteller, um den Bestätigungsmechanismus zu aktivieren. Die Fertigung kann auch unabhängig vom Zeitpunkt der Phasen der automatischen Bereitstellung erfolgen, insbesondere dann, wenn neue Geräte beschafft werden, nachdem die automatische Bereitstellung bereits eingerichtet wurde.

Im Inhaltsverzeichnis auf der linken Seite finden Sie eine Reihe von Schnellstarts, um die automatische Bereitstellung durch praktische Erfahrungen zu verdeutlichen. Um den Lernprozess zu ermöglichen/vereinfachen, wird Software verwendet, die ein physisches Gerät für die Anmeldung und Registrierung simuliert. Einige Schnellstarts erfordern aufgrund ihrer Simulationsnatur, dass Sie Vorgänge für mehrere Rollen ausführen, z.B. auch Vorgänge für nicht vorhandene Rollen.

| Rolle | Vorgang | Beschreibung | 
|------| --------- | ------------| 
| Hersteller | Codieren der Identität und Registrierungs-URL | Basierend auf dem verwendeten Bestätigungsmechanismus ist der Hersteller für die Verschlüsselung der Geräteidentitätsinformationen und der Registrierungs-URL des Device Provisioning-Diensts verantwortlich.<br><br>**Schnellstarts**: Da das Gerät simuliert wird, ist keine Rolle „Hersteller“ vorhanden. Weitere Informationen darüber, wie Sie diese Informationen abrufen können, finden Sie in der Rolle „Entwickler“, die beim Codieren einer Beispielregistrierungsanwendung verwendet wird. |  
| | Bereitstellen der Geräteidentität | Als Urheber der Geräteidentitätsinformationen ist der Hersteller dafür verantwortlich, diese an den Operator (oder einen bestimmten Agent) zu übermitteln oder sie über APIs direkt beim Device Provisioning-Dienst zu registrieren.<br><br>**Schnellstarts**: Da das Gerät simuliert wird, ist keine Rolle „Hersteller“ vorhanden. Weitere Informationen darüber, wie Sie die Geräteidentität abrufen, mit der ein simuliertes Gerät in Ihrer Instanz des Device Provisioning-Diensts registriert wird, finden Sie in der Rolle „Operator“. | 
| Operator | Konfigurieren der automatischen Bereitstellung | Dieser Vorgang entspricht der ersten Phase der automatischen Bereitstellung.<br><br>**Schnellstarts**: Sie führen die Rolle „Operator“ aus und konfigurieren den Device Provisioning-Dienst und die IoT Hub-Instanzen in Ihrem Azure-Abonnement. | Einrichten automatischer Bereitstellung |
|  | Registrieren der Geräteidentität | Dieser Vorgang entspricht der zweiten Phase der automatischen Bereitstellung.<br><br>**Schnellstarts**: Sie führen die Rolle „Operator“ aus und registrieren Ihr simuliertes Gerät in Ihrer Instanz des Device Provisioning-Diensts. Die Geräteidentität richtet sich nach der Bestätigungsmethode, die im Schnellstart simuliert wird (TPM oder X.509). Details zur Bestätigung finden Sie in der Rolle „Entwickler“. | 
| Device Provisioning-Dienst,<br>IoT Hub | \<alle Vorgänge\> | Sowohl für eine Produktionsimplementierung mit physischen Geräten als auch für Schnellstarts mit simulierten Geräten werden diese Rollen über die IoT-Dienste ausgeführt, die Sie in Ihrem Azure-Abonnement konfigurieren. Die Rollen/Vorgänge funktionieren genau gleich, da die Bereitstellung von physischen und simulierten Geräten durch die IoT-Dienste auf die gleiche Weise erfolgt. | 
| Entwickler | Erstellen/Bereitstellen von Registrierungssoftware | Dieser Vorgang entspricht der dritten Phase der automatischen Bereitstellung. Der Entwickler ist für die Erstellung und Bereitstellung der Registrierungssoftware auf dem Gerät unter Verwendung des entsprechenden SDKs verantwortlich.<br><br>**Schnellstarts**: Die von Ihnen erstellte Beispielregistrierungsanwendung simuliert ein echtes Gerät für Ihre bevorzugte Plattform/Sprache, das auf Ihrer Arbeitsstation ausgeführt wird (anstatt es auf einem physischen Gerät zu installieren). Die Registrierungsanwendung führt die gleichen Vorgänge aus wie eine Registrierungsanwendung auf einem physischen Gerät. Sie geben die Bestätigungsmethode (TPM oder X.509-Zertifikat) sowie die Registrierungs-URL und den „ID-Bereich“ Ihrer Instanz des Device Provisioning-Diensts an. Die Geräteidentität wird durch die SDK-Bestätigungslogik zur Laufzeit bestimmt, basierend auf der von Ihnen angegebenen Methode: <ul><li>**TPM-Bestätigung**: Ihre Entwicklungsarbeitsstation führt eine [TPM-Simulatoranwendung](how-to-use-sdk-tools.md#trusted-platform-module-tpm-simulator) aus. Sobald die Ausführung erfolgt, wird eine separate Anwendung verwendet, um den „Endorsement Key“ und die „Registrierungs-ID“ des TPM für die Registrierung der Geräteidentität zu extrahieren. Die SDK-Bestätigungslogik verwendet den Simulator auch während der Registrierung, um ein signiertes SAS-Token zur Authentifizierung und Überprüfung der Registrierung vorzulegen.</li><li>**X.509-Bestätigung**: Sie verwenden ein Tool zum [Generieren eines Zertifikats](how-to-use-sdk-tools.md#x509-certificate-generator). Nachdem dieses generiert wurde, erstellen Sie die Zertifikatdatei, die für die Verwendung bei der Registrierung erforderlich ist. Die SDK-Bestätigungslogik verwendet das Zertifikat auch während der Registrierung, um es zur Authentifizierung und Überprüfung der Registrierung vorzulegen.</li></ul> | 
| Gerät | Starten und registrieren | Dieser Vorgang entspricht der dritten Phase der automatischen Bereitstellung, die von der vom Entwickler entwickelten Geräteregistrierungssoftware übernommen wird. Details finden Sie in der Rolle „Entwickler“. Beim ersten Start: <ol><li>Die Anwendung stellt eine Verbindung mit der Instanz des Device Provisioning-Diensts her, und zwar über die bei der Entwicklung angegebene globale URL und den „ID-Bereich“ des Diensts.</li><li>Nach dem Herstellen der Verbindung wird das Gerät gemäß der während der Registrierung angegebenen Bestätigungsmethode und Identität authentifiziert.</li><li>Nachdem die Authentifizierung erfolgt ist, wird das Gerät bei der durch die Bereitstellungsdienstinstanz angegebenen IoT Hub-Instanz registriert.</li><li>Nach erfolgreicher Registrierung werden eine eindeutige Geräte-ID und ein IoT Hub-Endpunkt zur Kommunikation mit dem IoT Hub an die Registrierungsanwendung zurückgegeben.</li><li> Von dort kann das Gerät seinen ursprünglichen [Gerätezwillings](~/articles/iot-hub/iot-hub-devguide-device-twins.md)zustand für die Konfiguration abrufen und mit dem Melden von Telemetriedaten beginnen.</li></ol>**Schnellstarts**: Da das Gerät simuliert wird, wird die Registrierungssoftware auf Ihrer Entwicklungsarbeitsstation ausgeführt.| 

Die folgende Abbildung fasst die Rollen und die Reihenfolge der Vorgänge bei der automatischen Gerätebereitstellung zusammen:
<br><br>
![Automatische Bereitstellungssequenz für ein Gerät](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png) 

> [!NOTE]
> Optional kann der Hersteller den Vorgang „Geräteidentität registrieren“ auch über APIs des Device Provisioning-Diensts (anstatt über die Rolle „Operator“) ausführen. Eine detaillierte Beschreibung dieser Abläufe und mehr finden Sie im Video [Zero touch device registration with Azure IoT](https://myignite.microsoft.com/sessions/55087) (Geräteregistrierung mit Azure IoT ohne jegliche Intervention, in englischer Sprache) ab Markierung 41:00.

## <a name="next-steps"></a>Nächste Schritte

Möglicherweise ist es hilfreich, diesen Artikel zu Referenzzwecken mit einem Lesezeichen zu versehen, während Sie die entsprechenden Schnellstarts für die automatische Bereitstellung durcharbeiten. 

Beginnen Sie mit einem Schnellstart zum Einrichten der automatischen Bereitstellung, der am besten zu Ihrem Verwaltungstool passt und Sie durch die Phase „Dienstkonfiguration“ führt:

- [Einrichten der automatischen Bereitstellung mit der Azure CLI](quick-setup-auto-provision-cli.md)
- [Einrichten der automatischen Bereitstellung mit dem Azure-Portal](quick-setup-auto-provision.md)
- [Einrichten der automatischen Bereitstellung mit einer Resource Manager-Vorlage](quick-setup-auto-provision-rm.md)

Fahren Sie dann mit einem Schnellstart zur automatischen Bereitstellung eines simulierten Geräts fort, der zu Ihrem Gerätebestätigungsmechanismus und dem SDK Ihres Device Provisioning-Diensts sowie Ihrer Spracheinstellung passt. In diesem Schnellstart durchlaufen Sie die Phasen „Geräteregistrierung“ und „Geräteregistrierung und -konfiguration“: 

|  | Bestätigungsmechanismus für simulierte Geräte | Schnellstart SDK/Sprache |  |
|--|--|--|--|
|  | Trusted Platform Module (TPM) | [C](quick-create-simulated-device.md)<br>[Java](quick-create-simulated-device-tpm-java.md)<br>[C#](quick-create-simulated-device-tpm-csharp.md)<br>[Python](quick-create-simulated-device-tpm-python.md) |  |
|  | X.509-Zertifikat | [C](quick-create-simulated-device-x509.md)<br>[Java](quick-create-simulated-device-x509-java.md)<br>[C#](quick-create-simulated-device-x509-csharp.md)<br>[Node.js](quick-create-simulated-device-x509-node.md)<br>[Python](quick-create-simulated-device-x509-python.md) |  |




