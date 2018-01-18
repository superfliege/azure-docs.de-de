---
title: "Bereitstellen des Connected Factory-Gateways – Azure | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie ein Gateway unter Windows oder Linux bereitgestellt wird, um Verbindungen mit der vorkonfigurierten Connected Factory-Lösung zu ermöglichen."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/11/2017
ms.author: dobett
ms.openlocfilehash: c9854c68a95c2c1cc584503eb2f0b0dba6091016
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="deploy-an-edge-gateway-for-the-connected-factory-preconfigured-solution-on-windows-or-linux"></a>Bereitstellen eines Edge-Gateways für die vorkonfigurierte Connected Factory-Lösung unter Windows oder Linux

Sie benötigen zwei Softwarekomponenten für die Bereitstellung eines Edge-Gateways für die vorkonfigurierte *Connected Factory*-Lösung:

- Der *OPC-Proxy* stellt eine Verbindung mit der Connected Factory her. Der OPC-Proxy wartet dann auf Befehls- und Kontrollmeldungen vom integrierten OPC-Browser, der im Portal der Connected Factory-Lösung ausgeführt wird.

- Der *OPC Publisher* stellt eine Verbindung mit vorhandenen lokalen OPC UA-Servern her und leitet deren Telemetriemeldungen an die Connected Factory weiter. Sie können eine Verbindung mit einem klassischen OPC-Gerät mithilfe des [OPC Classic-Adapters für OPC UA](https://github.com/OPCFoundation/UA-.NETStandard/blob/master/ComIOP/README.md) herstellen.

Beide Komponenten sind Open-Source-Lösungen, die als Quelle auf GitHub und als Docker-Container auf DockerHub verfügbar sind:

| GitHub | DockerHub |
| ------ | --------- |
| [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) | [OPC Publisher](https://hub.docker.com/r/microsoft/iot-edge-opc-publisher/)   |
| [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy)         | [OPC Proxy](https://hub.docker.com/r/microsoft/iot-edge-opc-proxy/) |

Für keine der beiden Komponenten sind öffentlich erreichbare IP-Adressen oder offene eingehende Ports in der Gateway-Firewall erforderlich. Die Komponenten OPC Proxy und OPC Publisher nutzen nur den ausgehenden Port 443.

Die Schritte in diesem Artikel veranschaulichen die Bereitstellung eines Edge-Gateways mit Docker unter Windows oder Linux. Das Gateway ermöglicht Verbindungen mit der vorkonfigurierten Connected Factory-Lösung. Die Komponenten können auch ohne Connected Factory verwendet werden.

> [!NOTE]
> Beide Komponenten können als Module in [Azure IoT Edge](https://github.com/Azure/iot-edge) verwendet werden.

## <a name="choose-a-gateway-device"></a>Auswählen eines Gatewaygeräts

Falls Sie noch kein Gatewaygerät haben, empfiehlt Microsoft, ein kommerzielles Gateway von einem unserer Partner zu kaufen. Eine Liste der Gatewaygeräte, die mit der Connected Factory-Lösung kompatibel sind, finden Sie im [Katalog der Azure IoT-Geräte](https://catalog.azureiotsuite.com/?q=opc). Befolgen Sie die für das Gerät geltenden Anweisungen, um das Gateway einzurichten.

Alternativ können Sie ein vorhandenes Gatewaygerät manuell konfigurieren, indem Sie die folgenden Anweisungen befolgen.

## <a name="install-and-configure-docker"></a>Installieren und Konfigurieren von Docker

Installieren Sie [Docker für Windows](https://www.docker.com/docker-windows) auf Ihrem Windows-basierten Gatewaygerät, oder verwenden Sie einen Paket-Manager zum Installieren von Docker auf Ihrem Linux-basierten Gatewaygerät.

Wählen Sie während der Installation von Docker für Windows ein Laufwerk auf dem Hostcomputer aus, das für Docker freigegeben werden soll. Der folgende Screenshot zeigt das Freigeben von Laufwerk **D** auf Ihrem Windows-System, um den Zugriff auf das Hostlaufwerk aus einem Docker-Container zu ermöglichen:

![Installieren von Docker für Windows](./media/iot-suite-connected-factory-gateway-deployment/image1.png)

> [!NOTE]
> Sie können diesen Schritt auch nach der Installation von Docker über das Dialogfeld **Einstellungen** ausführen. Klicken Sie mit der rechten Maustaste auf das **Docker**-Symbol in der Windows-Taskleiste, und wählen Sie **Einstellungen** aus.

Bei Verwendung von Linux ist keine zusätzliche Konfiguration erforderlich, um den Zugriff auf das Dateisystem zu ermöglichen.

Erstellen Sie unter Windows einen Ordner auf dem Laufwerk, das Sie für Docker freigegeben haben, und erstellen Sie unter Linux einen Ordner unter dem Stammdateisystem. In dieser exemplarischen Vorgehensweise wird dieser Ordner als `<SharedFolder>` bezeichnet.

Wenn Sie in einem Docker-Befehl auf den `<SharedFolder>` verweisen, achten Sie darauf, die richtige Syntax für Ihr Betriebssystem zu verwenden. Hier finden Sie zwei Beispiele, eins für Windows und eins für Linux:

- Wenn Ihr `<SharedFolder>` unter Windows `D:\shared` ist, lautet die Befehlssyntax von Docker `//d/shared`.

- Wenn Ihr `<SharedFolder>` unter Linux `/shared` ist, lautet die Befehlssyntax von Docker `/shared`.

Weitere Informationen finden Sie unter [Use volumes](https://docs.docker.com/engine/admin/volumes/volumes/) (Verwenden von Volumes) in der Docker-Engine-Referenz.

## <a name="configure-the-opc-components"></a>Konfigurieren der OPC-Komponenten

Bevor Sie die OPC-Komponenten installieren, führen Sie die folgenden Schritte aus, um die Umgebung vorzubereiten:

1. Um die Gatewaybereitstellung abzuschließen, benötigen Sie die **iothubowner**-Verbindungszeichenfolge des IoT-Hubs in Ihrer Connected Factory-Bereitstellung. Navigieren Sie im [Azure-Portal](http://portal.azure.com/) zu Ihrem IoT Hub in der Ressourcengruppe, die bei der Bereitstellung der Connected Factory-Lösung erstellt wurde. Klicken Sie auf **SAS-Richtlinien**, um auf die **iothubowner**-Verbindungszeichenfolge zuzugreifen:

    ![Suchen der IoT Hub-Verbindungszeichenfolge](./media/iot-suite-connected-factory-gateway-deployment/image2.png)

    Kopieren Sie den Wert von **Verbindungszeichenfolge – Primärschlüssel**.

1. Um Kommunikation zwischen Docker-Containern zu ermöglichen, benötigen Sie ein benutzerdefiniertes Bridgenetzwerk. Um ein Bridgenetzwerk für Ihre Container zu erstellen, führen Sie an der Eingabeaufforderung die folgenden Befehle aus:

    ```cmd/sh
    docker network create -d bridge iot_edge
    ```

    Führen Sie den folgenden Befehl aus, um zu überprüfen, ob das **iot_edge**-Bridgenetzwerk erstellt wurde:

    ```cmd/sh
    docker network ls
    ```

    Ihr **iot_edge**-Bridgenetzwerk ist in der Liste der Netzwerke enthalten.

Um OPC Publisher auszuführen, führen Sie an der Eingabeaufforderung den folgenden Befehl aus:

```cmd/sh
docker run --rm -it -v <SharedFolder>:/docker -v x509certstores:/root/.dotnet/corefx/cryptography/x509stores --network iot_edge --name publisher -h publisher -p 62222:62222 --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-publisher:2.1.3 publisher "<IoTHubOwnerConnectionString>" --lf /docker/publisher.log.txt --as true --si 1 --ms 0 --tm true --vc true --di 30
```

- Das [OPC Publisher-GitHub](https://github.com/Azure/iot-edge-opc-publisher) und die [Docker run reference](https://docs.docker.com/engine/reference/run/) (Docker-Ausführungsreferenz) enthalten weitere Informationen zu den folgenden Punkten:

  - Den vor dem Containernamen angegebenen Docker-Befehlszeilenoptionen (`microsoft/iot-edge-opc-publisher:2.1.3`).
  - Der Bedeutung der nach dem Containernamen angegebenen Befehlszeilenparameter von OPC Publisher (`microsoft/iot-edge-opc-publisher:2.1.3`).

- Der `<IoTHubOwnerConnectionString>` ist die **iothubowner**-Verbindungszeichenfolge für die SAS-Richtlinie aus dem Azure-Portal. Sie haben diese Verbindungszeichenfolge bereits in einem vorhergehenden Schritt kopiert. Diese Verbindungszeichenfolge wird nur für die erstmalige Ausführung von OPC Publisher benötigt. Bei nachfolgenden Ausführungen sollte sie entfallen, da ihre Verwendung ein Sicherheitsrisiko darstellt.

- Der von Ihnen verwendete `<SharedFolder>` und seine Syntax sind im Abschnitt [Installieren und Konfigurieren von Docker](#install-and-configure-docker) beschrieben. OPC Publisher verwendet den `<SharedFolder>`, um die Konfigurationsdatei von OPC Publisher zu lesen, die Protokolldatei zu schreiben und beide Dateien außerhalb des Containers verfügbar zu machen.

- OPC Publisher liest seine Konfiguration aus der Datei **publishednodes.json**, die Sie im Ordner `<SharedFolder>/docker` platzieren sollten. Diese Konfigurationsdatei definiert, welche OPC UA-Knotendaten auf einem bestehenden OPC UA-Server vom OPC Publisher abonniert werden sollten.

- Sobald der OPC UA-Server OPC Publisher über eine Datenänderung benachrichtigt, wird der Wert an IoT Hub gesendet. Abhängig von den Batcheinstellungen sammelt OPC Publisher die Daten möglicherweise zunächst, um sie dann als Batch an IoT Hub zu senden.

- Die vollständige Syntax der Datei **publishednodes.json** ist auf der Seite [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) auf GitHub beschrieben.

    Der folgende Codeausschnitt zeigt ein einfaches Beispiel für eine **publishednodes.json**-Datei. Dieses Beispiel zeigt, wie der **CurrentTime**-Wert von einem OPC UA-Server mit dem Hostnamen **win10pc** veröffentlicht wird:

    ```json
    [
      {
        "EndpointUrl": "opc.tcp://win10pc:48010",
        "OpcNodes": [
          {
            "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258"
          }
        ]
      }
    ]
    ```

    In der **publishednodes.json**-Datei ist der OPC UA durch die Endpunkt-URL angegeben. Wenn Sie den Hostnamen mithilfe einer Hostnamenbezeichnung (wie etwa **win10pc**) anstelle einer IP-Adresse angeben – wie im vorhergehenden Beispiel – muss die Netzwerkadressauflösung im Container in der Lage sein, diese Hostnamenbezeichnung in eine IP-Adresse aufzulösen.

- Docker unterstützt keine NetBIOS-Namensauflösung, nur DNS-Namensauflösung. Wenn Sie nicht über einen DNS-Server im Netzwerk verfügen, können Sie die im vorhergehenden Befehlszeilenbeispiel verwendete Problemumgehung verwenden. Im vorhergehenden Befehlszeilenbeispiel wird der Parameter `--add-host` verwendet, um der hosts-Datei des Containers einen Eintrag hinzuzufügen. Dieser Eintrag ermöglicht das Nachschlagen des Hostnamens für den angegebenen `<OpcServerHostname>` und seine Auflösung in die angegebene IP-Adresse `<IpAddressOfOpcServerHostname>`.

- OPC UA verwendet X.509-Zertifikate zur Authentifizierung und Verschlüsselung. Sie müssen das OPC Publisher-Zertifikat auf dem OPC UA-Server platzieren, mit dem Sie die Verbindung herstellen, um sicherzustellen, dass er OPC Publisher vertraut. Der OPC Publisher-Zertifikatspeicher befindet sich im Ordner `<SharedFolder>/CertificateStores`. Sie finden das OPC Publisher-Zertifikat im Ordner `trusted/certs` im Ordner `CertificateStores`.

  Die für die Konfiguration des OPC UA-Servers auszuführenden Schritte hängen vom verwendeten Gerät ab. Diese Schritte sind normalerweise im Benutzerhandbuch des OPC UA-Servers dokumentiert.

Um den OPC Proxy zu installieren, führen Sie an der Eingabeaufforderung den folgenden Befehl aus:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.2 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db
```

Sie brauchen die Installation auf einem System nur einmalig auszuführen.

Führen Sie den OPC Proxy mithilfe des folgenden Befehls aus:

```cmd/sh
docker run -it --rm -v <SharedFolder>:/mapped --network iot_edge --name proxy --add-host <OpcServerHostname>:<IpAddressOfOpcServerHostname> microsoft/iot-edge-opc-proxy:1.0.2 -D /mapped/cs.db
```

OPC Proxy speichert die Verbindungszeichenfolge während der Installation. Bei nachfolgenden Ausführungen sollte die Verbindungszeichenfolge entfallen, da ihre Verwendung ein Sicherheitsrisiko darstellt.

## <a name="enable-your-gateway"></a>Aktivieren des Gateways

Führen Sie die folgenden Schritte aus, um Ihr Gateway in der vorkonfigurierten Lösung für die Connected Factory zu aktivieren:

1. Wenn beide Komponenten ausgeführt werden, navigieren Sie zur Seite **Verbinden mit dem eigenen OPC UA-Server** im Portal der Connected Factory-Lösung. Diese Seite steht in der Lösung nur Administratoren zur Verfügung. Geben Sie die Herausgeberendpunkt-URL (opc.tcp://publisher:62222) ein, und klicken Sie auf **Verbinden**.

1. Richten Sie eine Vertrauensstellung zwischen dem Portal der Connected Factory und OPC Publisher ein. Wenn eine Zertifikatwarnung angezeigt wird, klicken Sie auf **Vorgang fortsetzen**. Als Nächstes sehen Sie eine Fehlermeldung, die besagt, dass OPC Publisher dem UA-Webclient nicht vertraut. Kopieren Sie zum Beheben dieses Fehlers das **UA-Webclient**-Zertifikat aus dem Ordner `<SharedFolder>/CertificateStores/rejected/certs` in den Ordner `<SharedFolder>/CertificateStores/trusted/certs` auf dem Gateway. Sie brauchen das Gateway nicht neu zu starten.

Sie können jetzt über die Cloud eine Verbindung mit dem Gateway herstellen, und Sie sind bereit, OPC UA-Server zur Lösung hinzuzufügen.

## <a name="add-your-own-opc-ua-servers"></a>Hinzufügen von eigenen OPC UA-Servern

So fügen Sie der vorkonfigurierten Connected Factory-Lösung eigene OPC UA-Server hinzu:

1. Navigieren Sie zur Seite **Verbinden mit dem eigenen OPC UA-Server** im Portal der Connected Factory-Lösung. Führen Sie die gleichen Schritte wie im vorherigen Abschnitt aus, um eine Vertrauensstellung zwischen dem Connected Factory-Portal und dem OPC UA-Server herzustellen.

    ![Lösungsportal](./media/iot-suite-connected-factory-gateway-deployment/image4.png)

1. Wechseln Sie zur OPC UA-Knotenstruktur Ihres OPC UA-Servers, klicken Sie mit der rechten Maustaste auf die OPC-Knoten, die Sie an die Connected Factory senden möchten, und wählen **Veröffentlichen** aus.

1. Telemetriedaten werden jetzt vom Gatewaygerät übertragen. Sie können die Telemetriedaten in der Ansicht **Factoryspeicherorte** des Connected Factory-Portals unter **Neue Factory** anzeigen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über die Architektur der vorkonfigurierten Connected Factory-Lösung finden Sie unter [Vorkonfigurierte Connected Factory-Lösung – Exemplarische Vorgehensweise](https://docs.microsoft.com/azure/iot-suite/iot-suite-connected-factory-sample-walkthrough).

Informieren Sie sich über die [OPC Publisher-Referenzimplementierung](https://docs.microsoft.com/azure/iot-suite/iot-suite-connected-factory-publisher).