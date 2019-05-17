---
title: Grundlegendes zur Azure IoT Hub AMQP-Unterstützung | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Unterstützung für Geräte, die unter Verwendung des AMQP-Protokolls eine Verbindung mit einem geräteseitigen und dienstseitigen IoT Hub-Endpunkt herstellen. Enthält Informationen zur integrierten AMQP-Unterstützung der Azure IoT-Geräte-SDKs.'
author: rezasherafat
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: rezas
ms.openlocfilehash: 703e2c842fb42bad8aa112d84c516a29c2327378
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2019
ms.locfileid: "65473434"
---
# <a name="communicate-with-your-iot-hub-using-the-amqp-protocol"></a>Kommunikation mit Ihrem IoT Hub mithilfe des Protokolls AMQP

IoT Hub unterstützt [AMQP Version 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf), um eine Vielzahl von Funktionen durch geräteseitige und dienstseitige Endpunkte bereitzustellen. Dieses Dokument beschreibt die Verwendung des AMQP-Clients, um eine Verbindung mit IoT Hub zur Verwendung der IoT Hub-Funktionalität herzustellen.

## <a name="service-client"></a>Dienstclient

### <a name="connection-and-authenticating-to-iot-hub-service-client"></a>Herstellen der Verbindung mit und Authentifizierung bei IoT Hub (Dienstclient)
Ein Client kann zum Herstellen der Verbindung mit IoT Hub mithilfe von AMQP die Authentifizierung mit [Claims Based Security (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) – auf Ansprüchen basierende Sicherheit – oder [Simple Authentication and Security Layer (SASL)](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) verwenden.

Für den Dienstclient sind die folgenden Informationen erforderlich:

| Information | Wert | 
|-------------|--------------|
| IoT Hub-Hostname | `<iot-hub-name>.azure-devices.net` |
| Schlüsselname | `service` |
| Zugriffsschüssel | Primärer oder sekundärer dem Dienst zugeordneter Schlüssel |
| Shared Access Signature (SAS) | Kurzlebige SAS in folgendem Format: `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}` (den Code zum Generieren dieser Signatur finden Sie [hier](./iot-hub-devguide-security.md#security-token-structure)).


Der Codeausschnitt unten verwendet die [uAMQP-Bibliothek in Python](https://github.com/Azure/azure-uamqp-python) zum Herstellen der Verbindung mit IoT Hub über einen Senderlink.

```python
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '<operation-link-name>' # e.g., '/messages/devicebound'

username = '{policy_name}@sas.root.{iot_hub_name}'.format(iot_hub_name=iot_hub_name, policy_name=policy_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Create a send or receive client
send_client = uamqp.SendClient(uri, debug=True)
receive_client = uamqp.ReceiveClient(uri, debug=True)
```

### <a name="invoking-cloud-to-device-messages-service-client"></a>Aufrufen von C2D-Nachrichten (Dienstclient)
Der C2D-Nachrichtenaustausch zwischen Dienst und IoT Hub sowie zwischen Gerät und IoT Hub wird [hier](iot-hub-devguide-messages-c2d.md) beschrieben. Der Dienstclient verwendet die beiden unten beschriebenen Links, um Nachrichten zu senden und Feedback für zuvor von Geräten gesendete Nachrichten zu empfangen.

| Erstellt von | Verknüpfungstyp | Verknüpfungspfad | BESCHREIBUNG |
|------------|-----------|-----------|-------------|
| Dienst | Senderlink | `/messages/devicebound` | C2D-Nachrichten an Geräte werden vom Dienst an diesen Link gesendet. Die `To`-Eigenschaft von Nachrichten, die über diesen Link gesendet werden, wird auf den Linkpfad des Empfänger des Zielgeräts gesetzt, d.h. `/devices/<deviceID>/messages/devicebound`. |
| Dienst | Empfängerlink | `/messages/serviceBound/feedback` | Abschluss-, Ablehnungs- und Abbruch-Feedbacknachrichten von Geräten, die über diesen Link vom Dienst empfangen werden. [Hier](./iot-hub-devguide-messages-c2d.md#message-feedback) finden Sie weitere Informationen zu Feedbacknachrichten. |

Der folgende Codeausschnitt veranschaulicht, wie eine C2D-Nachricht erstellt und mit der [uAMQP-Bibliothek in Python](https://github.com/Azure/azure-uamqp-python) an ein Gerät gesendet wird.

```python
import uuid
# Create a message and set message property 'To' to the devicebound link on device
msg_id = str(uuid.uuid4())
msg_content = b"Message content goes here!"
device_id = '<device-id>'
to = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
ack = 'full' # Alternative values are 'positive', 'negative', and 'none'
app_props = { 'iothub-ack': ack }
msg_props = uamqp.message.MessageProperties(message_id=msg_id, to=to)
msg = uamqp.Message(msg_content, properties=msg_props, application_properties=app_props)

# Send the message using the send client created and connected IoT Hub earlier
send_client.queue_message(msg)
results = send_client.send_all_messages()

# Close the client if not needed
send_client.close()
```

Um Feedback zu erhalten, erstellt der Dienstclient einen Empfängerlink. Der folgende Codeausschnitt veranschaulicht, wie hierzu die [uAMQP-Bibliothek in Python](https://github.com/Azure/azure-uamqp-python) verwendet wird.

```python
import json

operation = '/messages/serviceBound/feedback'

# ...
# Recreate the URI using the feedback path above and authenticate
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
batch = receive_client.receive_message_batch(max_batch_size=10)
for msg in batch:
  print('received a message')
  # Check content_type in message property to identify feedback messages coming from device
  if msg.properties.content_type == 'application/vnd.microsoft.iothub.feedback.json':
    msg_body_raw = msg.get_data()
    msg_body_str = ''.join(msg_body_raw)
    msg_body = json.loads(msg_body_str)
    print(json.dumps(msg_body, indent=2))
    print('******************')
    for feedback in msg_body:
      print('feedback received')
      print('\tstatusCode: ' + str(feedback['statusCode']))
      print('\toriginalMessageId: ' + str(feedback['originalMessageId']))
      print('\tdeviceId: ' + str(feedback['deviceId']))
      print
  else:
    print('unknown message:', msg.properties.content_type)
```

Wie oben gezeigt, hat eine C2D-Feedbacknachricht den Inhaltstyp `application/vnd.microsoft.iothub.feedback.json`, und Eigenschaften in ihrem JSON-Text können verwendet werden, um den Zustellungsstatus der ursprünglichen Nachricht abzuleiten:
* Schlüssel `statusCode` im Feedbacktext hat einen der folgenden Werte: `['Success', 'Expired', 'DeliveryCountExceeded', 'Rejected', 'Purged']`.
* Schlüssel `deviceId` im Feedbacktext enthält die ID des Zielgeräts.
* Schlüssel `originalMessageId` im Feedbacktext enthält die ID der ursprünglichen vom Dienst gesendeten C2D-Nachricht. Diese kann zum Korrelieren des Feedbacks für C2D-Nachrichten verwendet werden.

### <a name="receive-telemetry-messages-service-client"></a>Empfangen von Telemetrienachrichten (Dienstclient)


### <a name="additional-notes"></a>Zusätzliche Hinweise
* Die AMQP-Verbindungen können aufgrund von Netzwerkstörungen oder Ablauf des (im Code generierten) Authentifizierungstokens unterbrochen werden. Der Dienstclient muss diese Situationen bewältigen und die Verbindung und Links bei Bedarf wiederherstellen. Im Fall des Ablaufs des Authentifizierungstokens kann der Client das Token auch proaktiv vor dessen Ablauf verlängern, um einen Verbindungsausfall zu vermeiden.
* In einigen Fällen muss Ihr Client Linkumleitungen ordnungsgemäß verarbeiten können. Informationen hierzu finden Sie in der Dokumentation Ihres AMQP-Clients.

### <a name="receive-cloud-to-device-messages-device-and-module-client"></a>Empfangen von C2D-Nachrichten (Gerät- und Modulclient)
Folgende AMQP-Links werden geräteseitig verwendet:

| Erstellt von | Verknüpfungstyp | Verknüpfungspfad | BESCHREIBUNG |
|------------|-----------|-----------|-------------|
| Geräte | Empfängerlink | `/devices/<deviceID>/messages/devicebound` | An Geräte gerichtete C2D-Nachrichten werden von jedem Zielgerät über diesen Link empfangen. |
| Geräte | Senderlink | `/messages/serviceBound/feedback` | Von Geräten über diesen Link an den Dienst gesendetes C2D-Nachrichtenfeedback. |
| Module | Empfängerlink | `/devices/<deviceID>/modules/<moduleID>/messages/devicebound` | An Module gerichtete C2D-Nachrichten werden von jedem Zielmodul über diesen Link empfangen. |
| Module | Senderlink | `/messages/serviceBound/feedback` | Von Modulen über diesen Link an den Dienst gesendetes C2D-Nachrichtenfeedback. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum AMQP-Protokoll finden Sie in der [AMQP 1.0-Spezifikation](http://www.amqp.org/sites/amqp.org/files/amqp.pdf).

Weitere Informationen zum IoT Hub-Messaging finden Sie unter:

* [Cloud-zu-Gerät-Nachrichten](./iot-hub-devguide-messages-c2d.md)
* [Unterstützen zusätzlicher Protokolle](iot-hub-protocol-gateway.md)
* [Unterstützung für das MQTT-Protokoll](./iot-hub-mqtt-support.md)
