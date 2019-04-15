---
title: Bereitstellen des Moduls zur Azure IoT OPC UA-Geräteverwaltung in einem vorhandenen Projekt| Microsoft-Dokumentation
description: Bereitstellen von OPC Twin in einem vorhandenen Projekt
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: dcf6acca344fe2a34fdc48fe89c5a1ee62b10b23
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59255885"
---
# <a name="deploy-opc-twin-to-an-existing-project"></a>Bereitstellen von OPC Twin in einem vorhandenen Projekt

Das OPC Device Twin-Modul wird auf IoT Edge ausgeführt und stellt mehrere Edge-Dienste für die OPC Device Twin- und Registry-Dienste zur Verfügung. 

Der OPC Device Twin-Microservice ermöglicht die Kommunikation zwischen Bedienern und OPC UA-Servergeräten im Fertigungsbereich über ein OPC Twin IoT Edge-Modul. Der Microservice stellt OPC UA-Dienste („Durchsuchen“, „Lesen“, „Schreiben“ und „Ausführen“) über seine REST-API zur Verfügung. 

Der Mikroservice der OPC UA-Geräteregistrierung ermöglicht den Zugriff auf registrierte OPC UA-Anwendungen und deren Endpunkte. Bediener und Administratoren können neue OPC UA-Anwendungen registrieren und deren Registrierung aufheben sowie die vorhandenen OPC UA-Anwendungen einschließlich ihrer Endpunkte durchsuchen. Zusätzlich zur Verwaltung von Anwendungen und Endpunkten katalogisiert der Registrierungsdienst auch registrierte OPC Device Twin IoT Edge-Module. Die Dienst-API ermöglicht die Steuerung der Funktionalität des Edge-Moduls, z. B. das Starten oder Beenden der Servererkennung (Überprüfungsdienste) oder das Aktivieren neuer Endpunktzwillinge, auf die über den OPC Twin-Microservice zugegriffen werden kann.

Der Modulkern ist die Supervisoridentität. Der Supervisor verwaltet den Endpunktzwilling, der den OPC UA-Serverendpunkten entspricht, die über die entsprechende API der OPC UA-Registrierung aktiviert werden. Diese Endpunktzwillinge übersetzen OPC UA JSON, das vom OPC Twin-Microservice in der Cloud empfangen wurde, in binäre OPC UA-Nachrichten, die über einen zustandsbehafteten sicheren Kanal an den verwalteten Endpunkt gesendet werden. Der Supervisor stellt auch Erkennungsdienste bereit, die Geräteerkennungsereignisse zur Verarbeitung an den OPC UA Device Onboarding-Dienst senden, wenn diese Ereignisse zu Aktualisierungen der OPC UA-Registrierung führen.  In diesem Artikel erfahren Sie, wie Sie das OPC Twin-Modul in einem bestehenden Projekt bereitstellen können. 

> [!NOTE]
> Weitere Informationen zu den Details und Anweisungen zur Bereitstellung finden Sie im GitHub-[Repository](https://github.com/Azure/azure-iiot-opc-twin-module).

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die PowerShell- und [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)-Erweiterungen installiert sind.   Wenn dieser Schritt noch nicht erfolgt ist, klonen Sie dieses GitHub-Repository.  Öffnen Sie eine Eingabeaufforderung oder ein Terminal, und führen Sie Folgendes aus:

```bash
git clone --recursive https://github.com/Azure/azure-iiot-components 
cd azure-iiot-components
```

## <a name="deploy-industrial-iot-services-to-azure"></a>Bereitstellen von industriellen IoT-Diensten für Azure

1. Führen Sie in der geöffneten Eingabeaufforderung oder im Terminal Folgendes aus:

   ```bash
   deploy
   ```

2. Befolgen Sie die Anweisungen, um der Ressourcengruppe für die Bereitstellung und der Website einen Namen zuzuweisen.   Das Skript stellt die Microservices und ihre Abhängigkeiten von der Azure-Plattform in der Ressourcengruppe in Ihrem Azure-Abonnement bereit.  Das Skript registriert auch eine Anwendung in Ihrem AAD-Mandanten (Azure Active Directory), um die OAUTH-basierte Authentifizierung zu unterstützen.  Die Bereitstellung dauert mehrere Minuten.  Ein Beispiel für die Ergebnisse, die Sie sehen werden, wenn die Lösung erfolgreich bereitgestellt wurde:

   ![Industrielle IoT OPC Twin-Bereitstellung in vorhandenen Projekten](media/howto-opc-twin-deploy-existing/opc-twin-deploy-existing1.png)

   Die Ausgabe enthält die URL des öffentlichen Endpunkts. 

3. Nachdem das Skript erfolgreich abgeschlossen wurde, wählen Sie aus, ob die ENV-Datei gespeichert werden soll.  Sie benötigen die ENV-Umgebungsdatei, wenn Sie mit Tools wie der Konsole eine Verbindung mit dem Cloudendpunkt herstellen oder Module für Entwicklung und Debugging bereitstellen möchten.

## <a name="troubleshooting-deployment-failures"></a>Beheben von Problemen bei der Bereitstellung

### <a name="resource-group-name"></a>Ressourcengruppenname

Verwenden Sie einen kurzen und einfachen Namen für die Ressourcengruppe.  Der Name wird auch zum Benennen von Ressourcen verwendet, daher muss er den Namenskonventionen für Ressourcen entsprechen.  

### <a name="website-name-already-in-use"></a>Websitename wird bereits verwendet.

Es ist möglich, dass der Name der Website bereits verwendet wird.  Wenn dieser Fehler auftritt, müssen Sie einen anderen Anwendungsnamen verwenden.

### <a name="azure-active-directory-aad-registration"></a>Azure Active Directory-Registrierung (AAD)

Über das Bereitstellungsskript wird versucht, zwei AAD-Anwendungen in Azure Active Directory zu registrieren.  Je nach Ihren Rechten für den ausgewählten AAD-Mandanten kann bei der Bereitstellung ein Fehler auftreten. Es gibt zwei Optionen:

1. Wenn Sie in einer Liste mit AAD-Mandanten einen Mandanten ausgewählt haben, starten Sie das Skript neu, und wählen Sie einen anderen Mandanten aus.
2. Alternativ können Sie einen privaten AAD-Mandanten in einem anderen Abonnement bereitstellen, das Skript neu starten und diesen Mandanten verwenden.

> [!WARNING]
> Fahren Sie NIE ohne Authentifizierung fort.  Wenn Sie dies zulassen, kann jeder ohne Authentifizierung über das Internet auf Ihre OPC Device Management-Endpunkte zugreifen.   Sie können jederzeit die [„lokale“ Bereitstellungsoption ](howto-opc-twin-deploy-dependencies.md) auswählen, um dies sorgfältig zu prüfen.

## <a name="deploy-an-all-in-one-industrial-iot-services-demo"></a>Bereitstellen einer All-in-One-Demo für industrielle IoT-Dienste

Anstatt nur die Dienste und Abhängigkeiten zu implementieren, können Sie auch eine All-in-One-Demo bereitstellen.  Die All-in-One-Demo enthält drei OPC UA-Server, das OPC Twin-Modul, alle Microservices und eine Beispielwebanwendung.  Sie dient zu Demonstrationszwecken.

1. Stellen Sie sicher, dass Sie über einen Klon des Repositorys verfügen (siehe oben). Öffnen Sie eine Eingabeaufforderung oder ein Terminal im Stammverzeichnis des Repositorys, und führen Sie Folgendes aus:

   ```bash
   deploy -type demo
   ```

2. Befolgen Sie die Anweisungen, um der Ressourcengruppe und der Website einen neuen Namen zuzuweisen.  Nach erfolgreicher Bereitstellung zeigt das Skript die URL des Endpunkts der Webanwendung an.

## <a name="deployment-script-options"></a>Optionen des Bereitstellungsskripts

Dem Skript werden die folgenden Parameter übergeben:

```bash
-type
```

Die Art der Bereitstellung (vm, local, demo)

```bash
-resourceGroupName
```

Dies kann der Name einer bestehenden oder einer neuen Ressourcengruppe sein.

```bash
-subscriptionId
```

Optional kann die Abonnement-ID verwendet werden, unter der die Ressourcen bereitgestellt werden.

```bash
-subscriptionName
```

Oder die Bezeichnung für das Abonnement.

```bash
-resourceGroupLocation
```

Optional kann dies ein Standort für die Ressourcengruppe sein. Wenn diese Option angegeben ist, wird versucht, eine neue Ressourcengruppe an dieser Position zu erstellen.

```bash
-aadApplicationName
```

Ein Name für die AAD-Anwendung, unter der die Registrierung erfolgen soll. 

```bash
-tenantId
```

Der zu verwendende AAD-Mandant.

```bash
-credentials
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie OPC Twin für ein vorhandenes Projekt bereitstellen, lautet unser Vorschlag für den nächsten Schritt:

> [!div class="nextstepaction"]
> [Schützen der Kommunikation zwischen OPC-Client und OPC PLC](howto-opc-vault-deploy-existing-client-plc-communication.md)
