---
title: Azure IoT-Geräteverwaltung mit IoT-Erweiterung für Azure CLI 2.0 | Microsoft-Dokumentation
description: Verwenden Sie das Tool IoT-Erweiterung für Azure CLI 2.0 mit den Verwaltungsmethoden für direkte Methoden und gewünschte Eigenschaften von Gerätezwillingen.
services: iot-hub
documentationcenter: ''
author: chrissie926
manager: timlt
tags: ''
keywords: Azure Iot-Geräteverwaltung, Azure IoT Hub-Geräteverwaltung, IoT-Geräteverwaltung, IoT Hub-Geräteverwaltung
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2018
ms.author: menchi
ms.openlocfilehash: 1bf2c10c5267157f6d74962c00d587a6956fc574
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="use-the-iot-extension-for-azure-cli-20-for-azure-iot-hub-device-management"></a>Verwenden der IoT-Erweiterung für Azure CLI 2.0 für die Verwaltung von Azure IoT Hub-Geräten

![Lückenloses Diagramm](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[Die IoT-Erweiterung für Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) ist eine neue Open Source-IoT-Erweiterung, die die Funktionen von [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest) ergänzt. Azure CLI 2.0 enthält Befehle zum Interagieren mit Azure Resource Manager- und Verwaltungsendpunkten. So können Sie beispielsweise Azure CLI 2.0 verwenden, um einen virtuellen Azure-Computer oder einen IoT Hub zu erstellen. Eine CLI-Erweiterung ermöglicht es einem Azure-Dienst, die Azure-Befehlszeilenschnittstelle zu ergänzen, wodurch Sie Zugriff auf zusätzliche dienstspezifische Funktionen erhalten. Die IoT-Erweiterung ermöglicht IoT-Entwicklern Befehlszeilenzugriff auf alle IoT Hub-, IoT Edge- und IoT Hub Device Provisioning-Dienst-Funktionen.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Verwaltungsoption          | Aufgabe                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Direkte Methoden             | Lassen Sie ein Gerät beispielsweise mit dem Senden von Nachrichten beginnen oder dies beenden, oder starten Sie es neu.                                        |
| Gewünschte Eigenschaften von Gerätezwillingen    | Setzen Sie ein Gerät in bestimmte Status, stellen Sie z.B. das Leuchten einer grünen LED ein, oder legen Sie das Telemetriesendeintervall auf 30 Minuten fest.         |
| Berichtete Eigenschaften von Gerätezwillingen   | Rufen Sie den berichteten Status eines Geräts ab. Das Gerät meldet z.B., das die LED jetzt blinkt.                                    |
| Gerätezwillingstags                  | Speichern gerätespezifischer Metadaten in der Cloud – beispielsweise den Aufstellungsort eines Automaten.                         |
| Gerätezwillingabfragen        | Fragen Sie alle Gerätezwillinge ab, um diejenigen abzurufen, die beliebige Bedingungen erfüllen; identifizieren Sie z.B. die zur Verwendung verfügbaren Geräte. |

Eine ausführlichere Erläuterung zu den Unterschieden und Anweisungen zur Verwendung dieser Optionen finden Sie im [Leitfaden zur D2C-Kommunikation](iot-hub-devguide-d2c-guidance.md) und [Leitfaden zur C2D-Kommunikation](iot-hub-devguide-c2d-guidance.md).

Gerätezwillinge sind JSON-Dokumente, in denen Gerätestatusinformationen (Metadaten, Konfigurationen und Bedingungen) gespeichert werden. Von IoT Hub wird für jedes Gerät, das eine Verbindung herstellt, dauerhaft ein Gerätezwilling gespeichert. Weitere Informationen zu Gerätezwillingen finden Sie unter [Erste Schritte mit Gerätezwillingen (Node)](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Lerninhalt

Sie lernen, wie Sie die IoT-Erweiterung für Azure CLI 2.0 mit verschiedenen Verwaltungsoptionen auf Ihrem Entwicklungscomputer verwenden können.

## <a name="what-you-do"></a>Aufgaben

Sie führen Azure CLI 2.0 und die IoT-Erweiterung für Azure CLI 2.0 mit verschiedenen Verwaltungsoptionen aus.

## <a name="what-you-need"></a>Voraussetzungen

- Abschluss des Tutorials [Einrichten Ihres Geräts](iot-hub-raspberry-pi-kit-node-get-started.md), in dem die folgenden Anforderungen behandelt werden:
  - Ein aktives Azure-Abonnement.
  - Ein Azure IoT Hub in Ihrem Abonnement.
  - Eine Clientanwendung, die Nachrichten an Ihren Azure IoT Hub sendet.

- Stellen Sie sicher, dass Ihr Gerät im Verlauf dieses Tutorials mit der Clientanwendung ausgeführt wird.

- [Python 2.7x oder Python 3.x](https://www.python.org/downloads/)

- Installieren Sie die Azure CLI 2.0. Eine einfache Möglichkeit zur Installation unter Windows besteht darin, den [Microsoft Windows Installer (MSI)](https://aka.ms/InstallAzureCliWindows) herunterzuladen und zu installieren. Sie können auch entsprechend den Installationsanleitungen in [Microsoft-Dokumentation](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Vorgehen, um Azure CLI 2.0 in Ihrer Umgebung einzurichten. Ihre Azure CLI 2.0-Version muss mindestens 2.0.24 lauten. Verwenden Sie `az –version`, um dies zu überprüfen. 

- Installieren Sie die IoT-Erweiterung. Die einfachste Möglichkeit ist die Ausführung von `az extension add --name azure-cli-iot-ext`. In der [Infodatei zur IoT-Erweiterung](https://github.com/Azure/azure-iot-cli-extension/blob/master/README.md) werden mehrere Wege zum Installieren der Erweiterung beschrieben.


## <a name="log-in-to-your-azure-account"></a>Melden Sie sich bei Ihrem Azure-Konto an.

Melden Sie sich mithilfe des folgenden Befehls bei Ihrem Azure-Konto an:

```bash
az login
```

## <a name="direct-methods"></a>Direkte Methoden

```bash
az iot hub invoke-device-method --device-id <your device id> --hub-name <your hub name> --method-name <the method name> --method-payload <the method payload>
```

## <a name="device-twin-desired-properties"></a>Gewünschte Eigenschaften von Gerätezwillingen

Legen Sie mit folgendem Befehl für die gewünschte Eigenschaft ein Intervall von 3.000 fest:

```bash
az iot hub device-twin update -n <your hub name> -d <your device id> --set properties.desired.interval = 3000
```

Diese Eigenschaft kann von Ihrem Gerät gelesen werden.

## <a name="device-twin-reported-properties"></a>Gemeldete Eigenschaften von Gerätezwillingen

Zeigen Sie mithilfe des folgenden Befehls die berichteten Eigenschaften des Geräts an:

```bash
az iot hub device-twin update -n <your hub name> -d <your device id> --set properties.reported.interval = 3000
```

Eine der Eigenschaften ist „$metadata.$lastUpdated“. Sie gibt an, wann dieses Geräts zuletzt eine Nachricht gesendet oder empfangen hat.

## <a name="device-twin-tags"></a>Tags von Gerätezwillingen

Zeigen Sie mithilfe des folgenden Befehls die Tags und Eigenschaften des Geräts an:

```bash
az iot hub device-twin show --hub-name <your hub name> --device-id <your device id>
```

Fügen Sie mit folgendem Befehl dem Gerät eine Feldrolle „temperature&humidity“ hinzu:

```bash
az iot hub device-twin update --hub-name <your hub name> --device-id <your device id> --set tags = '{"role":"temperature&humidity"}}'
```

## <a name="device-twin-queries"></a>Gerätezwillingabfragen

Fragen Sie mit folgendem Befehl Geräte mit einem Rollentag „temperature&humidity“ ab:

```bash
az iot hub query --hub-name <your hub name> --query-command "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Fragen Sie mit folgendem Befehl alle Geräte außer denen mit einem Rollentag „temperature&humidity“ ab:

```bash
az iot hub query --hub-name <your hub name> --query-command "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, Gerät-zu-Cloud-Nachrichten zu überwachen und Cloud-zu-Gerät-Nachrichten zwischen dem IoT-Gerät und Azure IoT Hub zu senden.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
