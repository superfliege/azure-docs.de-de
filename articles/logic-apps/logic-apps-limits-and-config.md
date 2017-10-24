---
title: "Grenzwerte und Konfiguration – Azure Logic Apps | Microsoft-Dokumentation"
description: "Dienstlimits und Konfigurationswerte für Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 4babb3033e75edc5c85ce89dac569b9f2beae9f7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="logic-apps-limits-and-configuration"></a>Logic Apps-Grenzwerte und -Konfiguration

In diesem Thema werden die aktuellen Grenzwerte und Konfigurationsdetails für Azure Logic Apps beschrieben.

## <a name="limits"></a>Einschränkungen

### <a name="http-request-limits"></a>Grenzwerte für HTTP-Anforderungen

Diese Grenzwerte gelten für eine einzelne HTTP-Anforderung oder einen Connectoraufruf.

#### <a name="timeout"></a>Timeout

| Name | Begrenzung | Hinweise | 
| ---- | ----- | ----- | 
| Anforderungszeitlimit | 120 Sekunden | Bei Bedarf können ein [asynchrones Muster](../logic-apps/logic-apps-create-api-app.md) oder eine [Until-Schleife](logic-apps-loops-and-scopes.md) zur Kompensation verwendet werden. |
|||| 

#### <a name="message-size"></a>Nachrichtengröße

| Name | Begrenzung | Hinweise | 
| ---- | ----- | ----- | 
| Nachrichtengröße | 100 MB | Einige Connectors und APIs unterstützen 100 MB möglicherweise nicht. | 
| Grenzwert für die Auswertung von Ausdrücken | 131.072 Zeichen | `@concat()`, `@base64()`, `string` dürfen diesen Grenzwert nicht überschreiten. | 
|||| 

#### <a name="retry-policy"></a>Wiederholungsrichtlinie

| Name | Begrenzung | Hinweise | 
| ---- | ----- | ----- | 
| Wiederholungsversuche | 90 | Der Standardwert ist 4. Sie können die Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](../logic-apps/logic-apps-workflow-actions-triggers.md) durchführen. | 
| Maximale Wiederholungsverzögerung | 1 Tag | Sie können die Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](../logic-apps/logic-apps-workflow-actions-triggers.md) durchführen. | 
| Minimale Wiederholungsverzögerung | 5 Sekunden | Sie können die Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](../logic-apps/logic-apps-workflow-actions-triggers.md) durchführen. |
|||| 

### <a name="run-duration-and-retention"></a>Ausführungsdauer und Aufbewahrung

Diese Grenzwerte gelten für die Ausführung einer einzelnen Logik-App.

| Name | Begrenzung | 
| ---- | ----- | 
| Ausführungsdauer | 90 Tage | 
| Aufbewahrungsdauer im Speicher | 90 Tage ab der Startzeit der Ausführung | 
| Min. Wiederholungsintervall | 1 Sekunde </br>Für Logik-Apps mit App Service-Plan: 15 Sekunden | 
| Max. Wiederholungsintervall | 500 Tage | 
||| 

Wenn die Grenzwerte für die Ausführungsdauer oder Speicheraufbewahrung im normalen Verarbeitungsfluss überschritten werden sollen, [wenden Sie sich an uns](mailto://logicappsemail@microsoft.com), damit wir Sie im Hinblick auf Ihre Anforderungen unterstützen können.

### <a name="looping-and-debatching-limits"></a>Grenzwerte für Schleifen und Auflösen von Batches

Diese Grenzwerte gelten für die Ausführung einer einzelnen Logik-App.

| Name | Begrenzung | Hinweise | 
| ---- | ----- | ----- | 
| ForEach-Elemente | 100.000 | Bei Bedarf können Sie die [Abfrageaktion](../connectors/connectors-native-query.md) verwenden, um größere Arrays zu filtern. | 
| Until-Iterationen | 5.000 | | 
| SplitOn-Elemente | 100.000 | | 
| ForEach-Parallelität | 50 | Der Standardwert ist 20. <p>Legen Sie die Eigenschaft `runtimeConfiguration` in der Aktion `foreach` fest, um ein bestimmtes Maß an Parallelität in einer ForEach-Schleife zu definieren. <p>Legen Sie die Eigenschaft `operationOptions` in der Aktion `foreach` auf „Sequenziell“ fest, um eine ForEach-Schleife sequenziell auszuführen. | 
|||| 

### <a name="throughput-limits"></a>Durchsatzlimits

Folgende Grenzwerte gelten für eine einzelne Instanz der Logik-App.

| Name | Begrenzung | Hinweise | 
| ---- | ----- | ----- | 
| Aktionsausführungen pro 5 Minuten | 100.000 | Bei Bedarf können Workloads auf mehrere Apps verteilt werden. | 
| Gleichzeitige ausgehende Aufrufe für Aktionen | ca. 2.500 | Verringern Sie nach Bedarf die Anzahl gleichzeitiger Anforderungen oder die Dauer. | 
| Endpunkt zur Laufzeit: Gleichzeitige eingehende Aufrufe | ca. 1.000 | Verringern Sie nach Bedarf die Anzahl gleichzeitiger Anforderungen oder die Dauer. | 
| Endpunkt zur Laufzeit: Read-Aufrufe pro 5 Minuten | 60.000 | Bei Bedarf können Workloads auf mehrere Apps verteilt werden. | 
| Endpunkt zur Laufzeit: Invoke-Aufrufe pro 5 Minuten | 45.000 | Bei Bedarf können Workloads auf mehrere Apps verteilt werden. | 
|||| 

Wenn diese Grenzwerte bei der normalen Verarbeitung überschritten werden sollen oder Auslastungstests ausgeführt werden sollen, bei denen diese Grenzwerte möglicherweise überschritten werden, [wenden Sie sich an uns](mailto://logicappsemail@microsoft.com), damit wir Sie im Hinblick auf Ihre Anforderungen unterstützen können.

### <a name="logic-app-definition-limits"></a>Definitionslimits für die Logik-App

Folgende Grenzwerte gelten für die Definition einer einzelnen Logik-App.

| Name | Begrenzung | Hinweise | 
| ---- | ----- | ----- | 
| Aktionen pro Workflow | 500 | Zur Erhöhung dieses Grenzwerts können Sie bei Bedarf geschachtelte Workflows hinzufügen. |
| Zulässige Schachtelungstiefe für Aktion | 8 | Zur Erhöhung dieses Grenzwerts können Sie bei Bedarf geschachtelte Workflows hinzufügen. | 
| Workflows pro Region und Abonnement | 1000 | | 
| Trigger pro Workflow | 10 | | 
| Bereichsumschaltbegrenzung | 25 | | 
| Anzahl der Variablen pro Workflow | 250 | | 
| Maximale Anzahl von Zeichen pro Ausdruck | 8.192 | | 
| Max. `trackedProperties`-Größe in Zeichen | 16.000 | 
| `action`/`trigger` -Namensbeschränkung | 80 | | 
| `description` | 256 | | 
| `parameters` limit | 50 | | 
| `outputs` limit | 10 | | 
|||| 

<a name="custom-connector-limits"></a>

### <a name="custom-connector-limits"></a>Grenzwerte für einen benutzerdefinierten Connector

Folgende Grenzwerte gelten für benutzerdefinierte Connectors, die Sie über Web-APIs erstellen können.

| Name | Begrenzung | 
| ---- | ----- | 
| Anzahl der erstellbaren benutzerdefinierten Connectors | 1.000 pro Azure-Abonnement | 
| Anzahl der Anforderungen pro Minute für jede von einem benutzerdefinierten Connector erstellte Verbindung | 500 Anforderungen für jede von dem Connector erstellte Verbindung |
||| 

### <a name="integration-account-limits"></a>Grenzwerte für Integrationskonten

Die folgenden Grenzwerte gelten für Artefakte, die zu einem Integrationskonto hinzugefügt werden können.

| Name | Begrenzung | Hinweise | 
| ---- | ----- | ----- | 
| Schema | 8 MB | Sie können den [Blob-URI](../logic-apps/logic-apps-enterprise-integration-schemas.md) zum Hochladen von Dateien verwenden, die größer als 2 MB sind. | 
| Zuordnung (XSLT-Datei) | 2 MB | | 
| Endpunkt zur Laufzeit: Read-Aufrufe pro 5 Minuten | 60.000 | Bei Bedarf kann der Workload auf mehrere Konten verteilt werden. | 
| Endpunkt zur Laufzeit: Invoke-Aufrufe pro 5 Minuten | 45.000 | Bei Bedarf kann der Workload auf mehrere Konten verteilt werden. | 
| Endpunkt zur Laufzeit: Aufrufüberwachungen pro 5 Minuten | 45.000 | Bei Bedarf kann der Workload auf mehrere Konten verteilt werden. | 
| Endpunkt zur Laufzeit: Gleichzeitige Aufrufe zum Blockieren | ca. 1.000 | Verringern Sie nach Bedarf die Anzahl gleichzeitiger Anforderungen oder die Dauer. | 
|||| 

Die folgenden Grenzwerte gelten für die Anzahl von Artefakten, die zu einem Integrationskonto hinzugefügt werden können.

#### <a name="free-pricing-tier"></a>Tarif „Free“

| Name | Begrenzung | Hinweise | 
| ---- | ----- | ----- | 
| Vereinbarungen | 10 | | 
| Andere Artefakttypen | 25 |Zu den Artefakttypen zählen Partner, Schemas, Zertifikate und Zuordnungen. Die Anzahl von Artefakten, die für die jeweiligen Typen festgelegt werden können, ist auf die maximale Anzahl begrenzt. | 
|||| 

#### <a name="standard-pricing-tier"></a>Tarif „Standard“

| Name | Begrenzung | Hinweise | 
| ---- | ----- | ----- | 
| Ein beliebiger Artefakttyp | 500 | Zu den Artefakttypen zählen Vereinbarungen, Partner, Schemas, Zertifikate und Zuordnungen. Die Anzahl von Artefakten, die für die jeweiligen Typen festgelegt werden können, ist auf die maximale Anzahl begrenzt. | 
|||| 

### <a name="b2b-protocols-as2-x12-edifact-message-size"></a>Nachrichtengröße für B2B-Protokolle (AS2, X12, EDIFACT)

Folgende Grenzwerte gelten für B2B-Protokolle.

| Name | Begrenzung | Hinweise | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Gilt für das Decodieren und das Codieren | 
| X12 | 50 MB | Gilt für das Decodieren und das Codieren | 
| EDIFACT | 50 MB | Gilt für das Decodieren und das Codieren | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Konfiguration: IP-Adressen

### <a name="logic-apps-service"></a>Logic Apps-Dienst

Die direkten Aufrufe einer Logik-App, sprich Aufrufe über [HTTP](../connectors/connectors-native-http.md) oder [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) oder andere HTTP-Anforderungen, stammen von IP-Adressen in dieser Liste.

|Logic Apps-Bereich|Ausgehende IP-Adresse|
|-----------------|-----------|
|Australien (Osten)|13.75.149.4, 104.210.91.55, 104.210.90.241|
|Australien, Südosten|13.73.114.207, 13.77.3.139, 13.70.159.205|
|Brasilien Süd|191.235.82.221, 191.235.91.7, 191.234.182.26|
|Kanada, Mitte|52.233.29.92, 52.228.39.241, 52.228.39.244|
|Kanada, Osten|52.232.128.155, 52.229.120.45, 52.229.126.25|
|Indien (Mitte)|52.172.154.168, 52.172.186.159, 52.172.185.79|
|USA (Mitte)|13.67.236.125, 104.208.25.27, 40.122.170.198|
|Ostasien|13.75.94.173, 40.83.127.19, 52.175.33.254|
|USA (Ost)|13.92.98.111, 40.121.91.41, 40.114.82.191|
|USA (Ost) 2|40.84.30.147, 104.208.155.200, 104.208.158.174|
|Japan Ost|13.71.158.3, 13.73.4.207, 13.71.158.120|
|Japan (Westen)|40.74.140.4, 104.214.137.243, 138.91.26.45|
|USA (Mitte/Norden)|168.62.248.37, 157.55.210.61, 157.55.212.238|
|Nordeuropa|40.113.12.95, 52.178.165.215, 52.178.166.21|
|USA, Süden-Mitte|104.210.144.48, 13.65.82.17, 13.66.52.232|
|Asien, Südosten|13.76.133.155, 52.163.228.93, 52.163.230.166|
|Indien (Süden)|52.172.50.24, 52.172.55.231, 52.172.52.0|
|USA, Westen-Mitte|52.161.27.190, 52.161.18.218, 52.161.9.108|
|Europa, Westen|40.68.222.65, 40.68.209.23, 13.95.147.65|
|Indien, Westen|104.211.164.80, 104.211.162.205, 104.211.164.136|
|USA (Westen)|52.160.92.112, 40.118.244.241, 40.118.241.243|
|USA, Westen 2|13.66.210.167, 52.183.30.169, 52.183.29.132|
|UK, Süden|51.140.74.14, 51.140.73.85, 51.140.78.44|
|UK, Westen|51.141.54.185, 51.141.45.238, 51.141.47.136|
| | |

### <a name="connectors"></a>Connectors

Die Aufrufe der [Connectors](../connectors/apis-list.md) stammen von den IP-Adressen in dieser Liste.

|Logic Apps-Bereich|Ausgehende IP-Adresse|
|-----------------|-----------|
|Australien (Osten)|40.126.251.213|
|Australien (Südosten)|40.127.80.34|
|Brasilien Süd|191.232.38.129|
|Kanada, Mitte|52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13|
|Kanada, Osten|52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52|
|Indien (Mitte)|104.211.98.164|
|USA (Mitte)|40.122.49.51|
|Ostasien|23.99.116.181|
|USA (Ost)|191.237.41.52|
|USA (Ost) 2|104.208.233.100|
|Japan Ost|40.115.186.96|
|Japan (Westen)|40.74.130.77|
|USA (Mitte/Norden)|65.52.218.230|
|Nordeuropa|104.45.93.9|
|USA (Mitte/Süden)|104.214.70.191|
|Südostasien|13.76.231.68|
|Indien, Süden|104.211.227.225|
|Westeuropa|40.115.50.13|
|Indien, Westen|104.211.161.203|
|USA (West)|104.40.51.248|
|UK, Süden|51.140.80.51|
|UK, Westen|51.141.47.105|
| | | 

## <a name="next-steps"></a>Nächste Schritte  

* [Erstellen Ihrer ersten Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)  
* [Allgemeine Beispiele und Szenarios](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Video: Automatisieren von Geschäftsprozessen mit Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Video: Integrieren Ihrer Systeme in Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
