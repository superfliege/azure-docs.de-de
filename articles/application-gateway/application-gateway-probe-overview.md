---
title: Übersicht über die Systemüberwachung für Azure Application Gateway
description: Weitere Informationen zu den Überwachungsfunktionen in Azure Application Gateway
services: application-gateway
documentationcenter: na
author: vhorne
manager: jpconnock
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/30/2018
ms.author: victorh
ms.openlocfilehash: 2f62f01c1178f9529eb46051f088affccc5279a7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="application-gateway-health-monitoring-overview"></a>Systemüberwachung des Application Gateways – Übersicht

Azure Application Gateway überwacht standardmäßig die Integrität aller Ressourcen in seinem Back-End-Pool und entfernt automatisch alle als fehlerhaft geltenden Ressourcen aus dem Pool. Application Gateway überwacht die fehlerhaften Instanzen weiterhin und fügt sie dem fehlerfreien Back-End-Pool hinzu, sobald sie verfügbar sind und auf Zustandsüberprüfungen reagieren. Application Gateway sendet die Integritätstests über denselben Port, der in den HTTP-Einstellungen des Back-Ends festgelegt wurde. Durch diese Konfiguration wird sichergestellt, dass derselbe Port getestet wird, den der Kunde für die Verbindung mit dem Back-End verwenden würde.

![Beispiel für einen Application Gateway-Test][1]

Zusätzlich zur Nutzung der standardmäßigen Überwachung der Integritätsüberprüfung können Sie die Integritätsüberprüfung auch an die Anforderungen Ihrer Anwendung anpassen. In diesem Artikel werden sowohl standardmäßige als auch benutzerdefinierte Integritätstests behandelt.

> [!NOTE]
> Wenn ein Application Gateway-Subnetz eine NSG enthält, muss der Portbereich 65503 bis 65534 im Application Gateway-Subnetz für eingehenden Datenverkehr geöffnet werden. Diese Ports sind erforderlich, damit die Back-End-Integritäts-API verwendet werden kann.

## <a name="default-health-probe"></a>Standardmäßige Integritätsüberprüfung

Ein Anwendungsgateway konfiguriert automatisch eine standardmäßige Integritätsüberprüfung, wenn Sie keine benutzerdefinierte Überprüfungskonfiguration einrichten. Das Verhalten der Überwachung funktioniert durch das Erstellen einer HTTP-Anforderung an die für den Back-End-Pool konfigurierten IP-Adressen. Bei Standardüberprüfungen wird auch HTTPS für den Integritätstest der Back-Ends verwendet, wenn die HTTP-Einstellungen des Back-Ends für HTTPS konfiguriert sind.

Beispiel: Sie konfigurieren Ihr Application Gateway für die Verwendung der Back-End-Server A, B und C zum Empfang von HTTP-Netzwerkdatenverkehr an Port 80. Die standardmäßige Integritätsüberwachung testet die drei Server alle 30 Sekunden auf eine fehlerfreie HTTP-Antwort. Eine fehlerfreie HTTP-Antwort weist einen [Statuscode](https://msdn.microsoft.com/library/aa287675.aspx) zwischen 200 und 399 auf.

Wenn die Standardüberprüfung für Server A fehlschlägt, entfernt das Application Gateway sie aus dem Back-End-Pool, und der Netzwerkdatenverkehr an diesen Server wird angehalten. Die Standardüberprüfung führt weiterhin alle 30 Sekunden eine Überprüfung für Server A aus. Wenn Server A erfolgreich auf eine Anforderung einer standardmäßigen Integritätsüberprüfung antwortet, wird er dem Back-End-Pool wieder als fehlerfrei hinzugefügt, und der Datenverkehr an den Server startet erneut.

### <a name="probe-matching"></a>Testabgleich

Standardmäßig gilt eine HTTP(S)-Antwort mit dem Statuscode 200 als fehlerfrei. Benutzerdefinierte Integritätstests unterstützen außerdem zwei Abgleichskriterien. Abgleichskriterien können verwendet werden, um optional die Standardinterpretation einer fehlerfreien Antwort zu ändern.

Abgleichskriterien: 

- **Abgleich des Statuscodes der HTTP-Antwort**: Testabgleichskriterium zum Akzeptieren der vom Benutzer angegebenen HTTP-Antwortcodes oder -Antwortcodebereiche. Einzelne kommagetrennte Antwortstatuscodes und ein Bereich von Statuscodes werden unterstützt.
- **Abgleich des HTTP-Antworttexts**: Testabgleichskriterium, das den HTTP-Antworttext heranzieht und ihn mit einer vom Benutzer angegebenen Zeichenfolge abgleicht. Beim Abgleich wird lediglich auf das Vorhandensein der vom Benutzer angegebenen Zeichenfolge im Antworttext geachtet, es ist kein Abgleich mit einem vollständigen regulären Ausdruck.

Abgleichskriterien können mithilfe des Cmdlets `New-AzureRmApplicationGatewayProbeHealthResponseMatch` angegeben werden.

Beispiel: 

```
$match = New-AzureRmApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399
$match = New-AzureRmApplicationGatewayProbeHealthResponseMatch -Body "Healthy"
```
Nachdem Sie die Abgleichskriterien angegeben haben, können sie der Testkonfiguration mit einem `-Match`-Parameter in PowerShell angefügt werden.

### <a name="default-health-probe-settings"></a>Einstellungen für die standardmäßige Integritätsüberprüfung

| Überprüfungseigenschaft | Wert | BESCHREIBUNG |
| --- | --- | --- |
| Überprüfungs-URL |http://127.0.0.1:\<Port\>/ |URL-Pfad |
| Intervall |30 |Überprüfungsintervall in Sekunden |
| Zeitüberschreitung |30 |Zeitüberschreitung der Überprüfung in Sekunden |
| Fehlerhafter Schwellenwert |3 |Anzahl der Wiederholungsversuche der Überprüfung Der Back-End-Server wird als außer Betrieb markiert, nachdem die Anzahl der aufeinanderfolgenden fehlgeschlagenen Überprüfungen den fehlerhaften Schwellenwert erreicht. |

> [!NOTE]
> Dieser Port stimmt mit dem Port in den HTTP-Einstellungen des Back-Ends überein.

Der Standardtest untersucht nur http://127.0.0.1:\<Port\>, um den Integritätsstatus zu bestimmen. Wenn Sie die Integritätsüberprüfung für eine benutzerdefinierte URL konfigurieren oder andere Einstellungen ändern möchten, müssen Sie benutzerdefinierte Überprüfungen wie in den folgenden Schritten beschrieben verwenden:

## <a name="custom-health-probe"></a>Benutzerdefinierte Integritätsüberprüfung

Benutzerdefinierte Überprüfungen ermöglichen Ihnen eine präzisere Kontrolle über die Überwachung des Systemzustands. Bei Verwendung von benutzerdefinierten Überprüfungen können Sie das Überprüfungsintervall, die URL und den zu überprüfenden Pfad konfigurieren und festlegen, wie viele fehlerhafte Antworten akzeptiert werden, bevor die Back-End-Pool-Instanz als fehlerhaft gekennzeichnet wird.

### <a name="custom-health-probe-settings"></a>Einstellungen für die benutzerdefinierte Integritätsüberprüfung

Die folgende Tabelle enthält Definitionen der Eigenschaften eines benutzerdefinierten Integritätstests.

| Überprüfungseigenschaft | BESCHREIBUNG |
| --- | --- |
| NAME |Name der Überprüfung. Dieser Name wird verwendet, um in den Back-End-HTTP-Einstellungen auf die Überprüfung zu verweisen. |
| Protokoll |Das zum Senden der Überprüfung verwendete Protokoll. Für die Überprüfung wird das in den HTTP-Einstellungen des Back-Ends festgelegte Protokoll verwendet. |
| Host |Hostname zum Senden der Überprüfung Nur relevant, wenn in Application Gateway mehrere Standorte konfiguriert sind. Andernfalls verwenden Sie 127.0.0.1. Dieser Wert entspricht nicht dem Hostnamen des virtuellen Computers. |
| path |Relativer Pfad der Überprüfung. Der gültige Pfad beginnt mit „/“. |
| Intervall |Überprüfungsintervall in Sekunden Dieser Wert ist das Zeitintervall zwischen zwei aufeinanderfolgenden Überprüfungen. |
| Zeitüberschreitung |Zeitüberschreitung der Überprüfung in Sekunden. Die Überprüfung wird als fehlerhaft markiert, wenn innerhalb des Zeitraums für die Zeitüberschreitung keine gültige Antwort empfangen wird.  |
| Fehlerhafter Schwellenwert |Anzahl der Wiederholungsversuche der Überprüfung Der Back-End-Server wird als außer Betrieb markiert, nachdem die Anzahl der aufeinanderfolgenden fehlgeschlagenen Überprüfungen den fehlerhaften Schwellenwert erreicht. |

> [!IMPORTANT]
> Falls Application Gateway für einen einzelnen Standort konfiguriert ist, muss der Hostname standardmäßig als „127.0.0.1“ angegeben werden, sofern in der benutzerdefinierten Überprüfung nichts anderes konfiguriert ist.
> Als Verweis wird ein Test an \<Protokoll\>://\<Host\>:\<Port\>\<Pfad\> gesendet. Dieser Port ist immer der gleiche Port, der in den HTTP-Einstellungen des Back-Ends definiert ist.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit der Systemüberwachung von Application Gateway vertraut gemacht haben, können Sie einen [benutzerdefinierten Integritätstest](application-gateway-create-probe-portal.md) im Azure-Portal oder einen [benutzerdefinierten Integritätstest](application-gateway-create-probe-ps.md) mit PowerShell und dem Azure Resource Manager-Bereitstellungsmodell konfigurieren.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
