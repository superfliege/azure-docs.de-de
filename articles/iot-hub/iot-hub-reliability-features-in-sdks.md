---
title: Verwalten von Konnektivität und zuverlässigem Messaging mit Azure IoT Hub-Geräte-SDKs
description: Erfahren Sie, wie Sie Gerätekonnektivität und Messaging mithilfe der Azure IoT Hub-Geräte-SDKs verbessern
services: iot-hub
keywords: ''
author: yzhong94
ms.author: yizhon
ms.date: 07/07/2018
ms.topic: article
ms.service: iot-hub
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 9a07fa2010eef22c4d1477641d07dee70ab5a9cb
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227441"
---
# <a name="how-to-manage-connectivity-and-reliable-messaging-using-azure-iot-hub-device-sdks"></a>Verwalten von Konnektivität und zuverlässigem Messaging mithilfe von Azure IoT Hub-Geräte-SDKs

Dieser Leitfaden bietet eine allgemeine Anleitung für die Entwicklung stabiler Geräteanwendungen, indem die Vorteile der Features für Konnektivität und des zuverlässigen Messagings in den Azure IoT-Geräte-SDKs genutzt werden. Ziel dieses Artikels ist es, Antworten auf Fragen zu liefern und Hilfe für die folgenden Szenarien bereitzustellen:

- Verwalten einer getrennten Netzwerkverbindung
- Umschalten zwischen verschiedenen Netzwerkverbindungen
- Erneutes Verbinden aufgrund vorübergehender Verbindungsfehler im Dienst

Die Implementierungsdetails können je nach Sprache variieren. Weitere Details erhalten Sie nach Klicken auf die Links zur API-Dokumentation oder zum jeweiligen SDK.

- [C/Python/iOS SDK](https://github.com/azure/azure-iot-sdk-c)
- [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [Node SDK](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)


## <a name="designing-for-resiliency"></a>Entwerfen mit Blick auf Resilienz

IoT-Geräte nutzen häufig durch Unterbrechungen gekennzeichnete bzw. instabile Netzwerkverbindungen, z.B. über GSM oder Satellit. Darüber hinaus können bei der Interaktion mit cloudbasierten Diensten Fehler aufgrund vorübergehender Bedingungen wie unregelmäßige Dienstverfügbarkeit und Störungen auf Infrastrukturebene (allgemein als kurzzeitige Störungen bezeichnet) auftreten. Eine auf einem Gerät ausgeführte Anwendung muss die Verbindungs- und Wiederverbindungsmechanismen sowie die Wiederholungslogik für das Senden und Empfangen von Nachrichten handhaben. Darüber hinaus hängen die Anforderungen an die Wiederholungsstrategie besonders vom IoT-Szenario, an dem das Gerät beteiligt ist, sowie vom Kontext und den Fähigkeiten des Geräts ab.

Die Azure IoT Hub-Geräte-SDKs zielen darauf ab, die Verbindung und Kommunikation von Cloud-zu-Gerät und Gerät-zu-Cloud zu vereinfachen, indem sie eine zuverlässige und umfassende Möglichkeit zum Verbinden mit und Senden bzw. Empfangen von Nachrichten an bzw. von Azure IoT Hub bieten. Entwickler können auch bestehende Implementierungen modifizieren, um die optimale Wiederholungsstrategie für ein bestimmtes Szenario zu entwickeln.

Die relevanten SDK-Funktionen, die Konnektivität und zuverlässiges Messaging unterstützen, werden in den folgenden Abschnitten behandelt.

## <a name="connection-and-retry"></a>Verbindung und Wiederholung

Dieser Abschnitt bietet einen Überblick über die verfügbaren Wiederverbindungs- und Wiederholungsmuster bei der Verwaltung von Verbindungen, Implementierungsanleitungen für unterschiedliche Wiederholungsrichtlinien in Ihrer Geräteanwendung und relevante APIs für die Geräte-SDKs.

### <a name="error-patterns"></a>Fehlermuster
Verbindungsfehler können auf vielen Ebenen auftreten:

-  Netzwerkfehler wie getrennter Socket und Namensauflösungsfehler
- Fehler auf Protokollebene beim HTTP-, AMQP- und MQTT-Transport, wie z.B. getrennte Verbindungen oder abgelaufene Sitzungen
- Fehler auf Anwendungsebene, die entweder aus lokalen Fehlern wie ungültigen Anmeldeinformationen oder einem Dienstverhalten wie Kontingentüberschreitung oder Drosselung resultieren

Die Geräte-SDKs erkennen Fehler auf allen drei Ebenen.  Fehler im Zusammenhang mit dem Betriebssystem und Hardwarefehler werden von den Geräte-SDKs weder erkannt noch behandelt.  Der Entwurf basiert auf der Anleitung [Behandeln vorübergehender Fehler](/azure/architecture/best-practices/transient-faults#general-guidelines) im Azure Architecture Center.

### <a name="retry-patterns"></a>Wiederholungsmuster

Der allgemeine Ablauf für eine Wiederholung bei Erkennen von Verbindungsfehlern ist wie folgt: 
1. Das SDK erkennt den Fehler und den damit verbundenen Fehler im Netzwerk, Protokoll oder in der Anwendung.
2. Basierend auf dem Fehlertyp nutzt das SDK den Fehlerfilter, um zu entscheiden, ob eine Wiederholung erforderlich ist.  Wenn vom SDK ein **nicht behebbarer Fehler** erkannt wird, werden Vorgänge (Verbindung und Senden/Empfangen) abgebrochen, und der Benutzer wird vom SDK benachrichtigt. Ein nicht behebbarer Fehler ist ein Fehler, den das SDK ermitteln und bei dem es feststellen kann, dass er nicht korrigiert werden kann, z.B. ein Authentifizierungsfehler oder Fehler aufgrund eines falschen Endpunkts.
3. Wenn ein **behebbarer Fehler** erkannt wird, beginnt das SDK mit der Wiederholung gemäß der angegebenen Wiederholungsrichtlinie, bis ein definiertes Zeitlimit abläuft.
4. Wenn das definierte Zeitlimit abläuft, versucht das SDK nicht mehr, eine Verbindung herzustellen oder eine Nachricht zu senden, und benachrichtigt den Benutzer.
5.  Das SDK ermöglicht dem Benutzer, einen Rückruf anzufügen, um Änderungen am Verbindungsstatus zu empfangen. 

Drei Wiederholungsrichtlinien stehen zur Verfügung:
- **ExponentialBackOffWithJitter**: Dies ist die standardmäßig angewendete Wiederholungsrichtlinie.  Sie ist am Anfang eher aggressiv, verlangsamt sich und erreicht dann eine maximale Verzögerung, die nicht überschritten wird.  Der Entwurf basiert auf der [Wiederholungsanleitung](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific) im Azure Architecture Center.
- **Benutzerdefinierte Wiederholung**: Sie können eine benutzerdefinierte Wiederholungsrichtlinie implementieren und diese je nach gewählter Sprache in die Wiederholungsrichtlinie (RetryPolicy) einfügen. Sie können eine Wiederholungsrichtlinie entwerfen, die für Ihr Szenario geeignet ist.  Diese ist nicht für das C SDK verfügbar.
- **Keine Wiederholung**: Es gibt die Option, die Wiederholungsrichtlinie auf „no retry“ festzulegen, wodurch die Wiederholungslogik deaktiviert wird.  Das SDK versucht, einmal eine Verbindung herzustellen und einmal eine Nachricht zu senden, vorausgesetzt, die Verbindung ist hergestellt. Diese Richtlinie wird typischerweise in Fällen verwendet, in denen Bandbreiten- oder Kostenaspekte eine Rolle spielen.   Bei Wahl dieser Option gehen Nachrichten, die nicht gesendet werden können, verloren und können nicht wiederhergestellt werden. 

### <a name="retry-policy-apis"></a>APIs für Wiederholungsrichtlinien

   | SDK | SetRetryPolicy-Methode | Richtlinienimplementierungen | Implementierungsleitfaden |
   |-----|----------------------|--|--|
   |  C/Python/iOS  | [IOTHUB_CLIENT_RESULT IoTHubClient_SetRetryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/2018-05-04/iothub_client/inc/iothub_client.h#L188)        | **Standard**: [IOTHUB_CLIENT_RETRY_EXPONENTIAL_BACKOFF](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Benutzerdefiniert:** Verfügbare [retryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies) verwenden<BR>**Keine Wiederholung:** [IOTHUB_CLIENT_RETRY_NONE](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)  | [C-/Python-/iOS-Implementierung](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#)  |
   | Java| [SetRetryPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._device_client_config.setretrypolicy?view=azure-java-stable)        | **Standard**: [ExponentialBackoffWithJitter-Klasse](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)<BR>**Benutzerdefiniert:** [RetryPolicy-Schnittstelle](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/RetryPolicy.java) implementieren<BR>**Keine Wiederholung:** [NoRetry-Klasse](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)  | [Java-Implementierung](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md) |[.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
   | .NET| [DeviceClient.SetRetryPolicy](/dotnet/api/microsoft.azure.devices.client.deviceclient.setretrypolicy?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_SetRetryPolicy_Microsoft_Azure_Devices_Client_IRetryPolicy) | **Standard**: [ExponentialBackoff-Klasse](/dotnet/api/microsoft.azure.devices.client.exponentialbackoff?view=azure-dotnet)<BR>**Benutzerdefiniert:** [IRetryPolicy-Schnittstelle](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.iretrypolicy?view=azure-dotnet) implementieren<BR>**Keine Wiederholung:** [NoRetry-Klasse](/dotnet/api/microsoft.azure.devices.client.noretry?view=azure-dotnet) | [C#-Implementierung](https://github.com/Azure/azure-iot-sdk-csharp) |
   | Knoten| [setRetryPolicy](/javascript/api/azure-iot-device/client?view=azure-iot-typescript-latest#azure_iot_device_Client_setRetryPolicy) | **Standard**: [ExponentialBackoffWithJitter-Klasse](/javascript/api/azure-iot-common/exponentialbackoffwithjitter?view=azure-iot-typescript-latest)<BR>**Benutzerdefiniert:** [RetryPolicy-Schnittstelle](/javascript/api/azure-iot-common/retrypolicy?view=azure-iot-typescript-latest) implementieren<BR>**Keine Wiederholung:** [NoRetry-Klasse](/javascript/api/azure-iot-common/noretry?view=azure-iot-typescript-latest) | [Node-Implementierung](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them) |
   

Es folgen Codebeispiele, die diesen Ablauf veranschaulichen: 

#### <a name="net-implementation-guidance"></a>Leitfaden zur .NET-Implementierung

Das folgende Codebeispiel veranschaulicht die Definition und Festlegung der Standardwiederholungsrichtlinie:

   ```csharp
   # define/set default retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(int.MaxValue, TimeSpan.FromMilliseconds(100), TimeSpan.FromSeconds(10), TimeSpan.FromMilliseconds(100));
   SetRetryPolicy(retryPolicy);
   ```

Um eine hohe CPU-Auslastung zu vermeiden, werden die Wiederholungsversuche gedrosselt, wenn der Code sofort fehlschlägt (z.B. wenn es kein Netzwerk oder keine Route zum Ziel gibt), sodass die Mindestzeit für den nächsten Wiederholungsversuch 1 Sekunde ist. 

Wenn der Dienst mit einem Drosselungsfehler reagiert, ist die Wiederholungsrichtlinie anders und kann nicht über die öffentliche API geändert werden:

   ```csharp
   # throttled retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(RetryCount, TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(60), TimeSpan.FromSeconds(5));
   SetRetryPolicy(retryPolicy);
   ```

Der Wiederholungsmechanismus wird nach der Einstellung `DefaultOperationTimeoutInMilliseconds`, die derzeit auf 4 Minuten festgelegt ist, abgebrochen.

#### <a name="other-languages-implementation-guidance"></a>Leitfaden zur Implementierung anderer Sprachen
Informationen zu anderen Sprachen finden Sie in der folgenden Implementierungsdokumentation.  Beispiele, die die Verwendung von APIs für Wiederholungsrichtlinien veranschaulichen, werden im Repository bereitgestellt.
- [C/Python/iOS SDK](https://github.com/azure/azure-iot-sdk-c)
- [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [Node SDK](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)

## <a name="next-steps"></a>Nächste Schritte
- [Verwenden von Geräte- und Dienst-SDKs](.\iot-hub-devguide-sdks.md)
- [Verwenden des Azure IoT-Geräte-SDKs für C](.\iot-hub-device-sdk-c-intro.md)
- [Entwickeln für eingeschränkte Geräte](.\iot-hub-devguide-develop-for-constrained-devices.md)
- [Entwickeln für mobile Geräte](.\iot-hub-how-to-develop-for-mobile-devices.md)
- [Troubleshoot device disconnects](iot-hub-troubleshoot-connectivity.md) (Problembehandlung bei getrennten Geräteverbindungen)
