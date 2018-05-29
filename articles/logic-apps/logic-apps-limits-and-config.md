---
title: Grenzwerte und Konfiguration – Azure Logic Apps | Microsoft-Dokumentation
description: Dienstlimits und Konfigurationswerte für Azure Logic Apps
services: logic-apps
documentationcenter: ''
author: ecfan
manager: cfowler
editor: ''
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 05/14/2018
ms.author: estfan
ms.openlocfilehash: 8c2ac4b8f55d25d5d3fcfdd6a9bcb6f6c8cfc201
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34166300"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Grenzwert- und Konfigurationsinformationen für Azure Logic Apps

In diesem Artikel sind die Einschränkungen und Konfigurationsdetails beschrieben, die beim Erstellen und Ausführen von automatisierten Workflows mit Azure Logic Apps zu beachten sind. Informationen zu Microsoft Flow finden Sie unter [Grenzwerte und Konfiguration in Microsoft Flow](https://docs.microsoft.com/flow/limits-and-config).

<a name="definition-limits"></a>

## <a name="definition-limits"></a>Definitionslimits

Dies sind die Grenzwerte für eine einzelne Logik-App-Definition:

| NAME | Begrenzung | Notizen | 
| ---- | ----- | ----- | 
| Aktionen pro Workflow | 500 | Zur Erhöhung dieses Grenzwerts können Sie bei Bedarf geschachtelte Workflows hinzufügen. |
| Zulässige Schachtelungstiefe für Aktionen | 8 | Zur Erhöhung dieses Grenzwerts können Sie bei Bedarf geschachtelte Workflows hinzufügen. | 
| Workflows pro Region und Abonnement | 1.000 | | 
| Trigger pro Workflow | 10 | Wenn in der Codeansicht gearbeitet wird, nicht im Designer | 
| Bereichsumschaltbegrenzung | 25 | | 
| Variablen pro Workflow | 250 | | 
| Zeichen pro Ausdruck | 8.192 | | 
| Maximale Größe für `trackedProperties` | 16.000 Zeichen | 
| Name für `action` oder `trigger` | 80 Zeichen | | 
| Länge von `description` | 256 Zeichen | | 
| Größter Wert für `parameters` | 50 | | 
| Größter Wert für `outputs` | 10 | | 
||||  

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>Grenzwerte für Ausführungsdauer und Aufbewahrung

Dies sind die Grenzwerte für eine einzelne Ausführung der Logik-App:

| NAME | Begrenzung | Notizen | 
|------|-------|-------| 
| Ausführungsdauer | 90 Tage | Informationen dazu, wie dieser Grenzwert geändert wird, finden Sie unter [Ändern von Ausführungsdauer](#change-duration). | 
| Aufbewahrungsdauer im Speicher | 90 Tage ab der Startzeit der Ausführung | Informationen dazu, wie dieser Grenzwert geändert wird, finden Sie unter [Ändern von Aufbewahrungsdauer im Speicher](#change-retention). | 
| Kürzestes Wiederholungsintervall | 1 Sekunde | | 
| Längstes Wiederholungsintervall | 500 Tage | | 
|||| 

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-storage-retention"></a>Ändern von Ausführungsdauer und Aufbewahrungsdauer im Speicher

Sie können diesen Grenzwert auf einen Wert zwischen sieben Tage und 90 Tage ändern. Wenn Sie aber den maximalen Grenzwert überschreiten müssen, [wenden Sie sich an das Logic Apps-Team ](mailto://logicappsemail@microsoft.com), um Hilfestellung zu Ihren Anforderungen zu erhalten.

1. Wählen Sie im Azure-Portal im Menü Ihrer Logik-App den Befehl **Workfloweinstellungen** aus. 

2. Wählen Sie unter **Laufzeitoptionen** in der Liste **Aufbewahrung des Ausführungsverlaufs in Tagen** die Option **Benutzerdefiniert** aus. 

3. Geben Sie die gewünschte Anzahl von Tagen ein, oder ziehen Sie den Schieberegler bis zur gewünschten Anzahl.

<a name="looping-debatching-limits"></a>

## <a name="looping-and-debatching-limits"></a>Grenzwerte für Schleifen und Auflösen von Batches

Dies sind die Grenzwerte für eine einzelne Ausführung der Logik-App:

| NAME | Begrenzung | Notizen | 
| ---- | ----- | ----- | 
| Until-Iterationen | 5.000 | | 
| ForEach-Elemente | 100.000 | Bei Bedarf können Sie die [Abfrageaktion](../connectors/connectors-native-query.md) verwenden, um größere Arrays zu filtern. | 
| ForEach-Parallelität | 50 | Der Standardwert ist 20. <p>Legen Sie die Eigenschaft `runtimeConfiguration` in der Aktion `foreach` fest, um ein bestimmtes Maß an Parallelität in einer ForEach-Schleife zu definieren. <p>Legen Sie die Eigenschaft `operationOptions` in der Aktion `foreach` auf „Sequenziell“ fest, um eine ForEach-Schleife sequenziell auszuführen. | 
| SplitOn-Elemente | 100.000 | | 
|||| 

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>Durchsatzlimits

Dies sind die Grenzwerte für eine einzelne Ausführung der Logik-App:

| NAME | Begrenzung | Notizen | 
| ----- | ----- | ----- | 
| Aktionsausführungen pro 5 Minuten | 100.000 | Um das Limit auf 300.000 zu erhöhen, führen Sie eine Logik-App im `High Throughput`-Modus aus. Um den Modus mit hohem Durchsatz zu konfigurieren, legen Sie unter der `runtimeConfiguration` der Workflowressource die `operationOptions`-Eigenschaft auf `OptimizedForHighThroughput` fest. <p>**Hinweis**: Beachten Sie, dass sich dieser Modus für den hohen Durchsatz in der Vorschauphase befindet. Bei Bedarf können Sie eine Workload auch auf mehrere Apps verteilen. | 
| Gleichzeitige ausgehende Aufrufe für Aktionen | ca. 2.500 | Verringern Sie nach Bedarf die Anzahl gleichzeitiger Anforderungen oder die Dauer. | 
| Endpunkt zur Laufzeit: Gleichzeitige eingehende Aufrufe | ca. 1.000 | Verringern Sie nach Bedarf die Anzahl gleichzeitiger Anforderungen oder die Dauer. | 
| Endpunkt zur Laufzeit: Read-Aufrufe pro 5 Minuten  | 60.000 | Bei Bedarf können Workloads auf mehrere Apps verteilt werden. | 
| Endpunkt zur Laufzeit: Invoke-Aufrufe pro 5 Minuten| 45.000 |Bei Bedarf können Workloads auf mehrere Apps verteilt werden. | 
|||| 

Wenn diese Grenzwerte bei der normalen Verarbeitung überschritten oder Auslastungstests ausgeführt werden sollen, bei denen diese Grenzwerte möglicherweise überschritten werden, [bitten Sie das Logic Apps-Team](mailto://logicappsemail@microsoft.com) um Unterstützung im Hinblick auf Ihre Anforderungen.

<a name="request-limits"></a>

## <a name="http-request-limits"></a>Grenzwerte für HTTP-Anforderungen

Diese Grenzwerte gelten für eine einzelne HTTP-Anforderung oder einen einzelnen synchronen Connectoraufruf:

#### <a name="timeout"></a>Timeout

Einige Connectorvorgänge führen asynchrone Aufrufe aus oder lauschen auf Webhookanforderungen, sodass das Timeout für diese Vorgänge länger sein kann, als diese Grenzwerte angeben. Weitere Informationen hierzu finden Sie in den technischen Details für den bestimmten Connector sowie unter [Workflowtrigger und -aktionen](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action).

| NAME | Begrenzung | Notizen | 
| ---- | ----- | ----- | 
| Ausgehende Anforderung | 120 Sekunden | Verwenden Sie für Vorgänge, die länger ausgeführt werden, ein [asynchrones Abrufmuster](../logic-apps/logic-apps-create-api-app.md#async-pattern) oder eine [Until-Schleife](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action). | 
| Synchrone Antwort | 120 Sekunden | Damit die ursprüngliche Anforderung die Antwort erhält, müssen alle Schritte in der Antwort innerhalb des Grenzwerts abgeschlossen werden, es sei denn, Sie rufen eine andere Logik-App als geschachtelten Workflow auf. Weitere Informationen hierzu finden Sie unter [Aufrufen, Auslösen oder Schachteln von Logik-Apps](../logic-apps/logic-apps-http-endpoint.md). | 
|||| 

#### <a name="message-size"></a>Nachrichtengröße

| NAME | Begrenzung | Notizen | 
| ---- | ----- | ----- | 
| Nachrichtengröße | 100 MB | Informationen, wie Sie diese Beschränkung umgehen können, finden Sie unter [Verarbeiten von großen Nachrichten durch Blockerstellung in Logic Apps](../logic-apps/logic-apps-handle-large-messages.md). Es kann aber sein, dass einige Connectors und APIs Blockerstellung (Segmentierung) oder sogar den Standardgrenzwert nicht unterstützen. | 
| Nachrichtengröße mit Blockerstellung (Segmentierung) | 1 GB | Dieser Grenzwert gilt für Aktionen, die Blockerstellung automatisch unterstützen oder für die Blockerstellungsunterstützung in der Laufzeitkonfiguration aktiviert werden kann. Weitere Informationen hierzu finden Sie unter [Verarbeiten von großen Nachrichten durch Blockerstellung in Logic Apps](../logic-apps/logic-apps-handle-large-messages.md). | 
| Grenzwert für die Auswertung von Ausdrücken | 131.072 Zeichen | Keiner der Ausdrücke `@concat()`, `@base64()` und `@string()` darf länger sein, als dieser Grenzwert angibt. | 
|||| 

#### <a name="retry-policy"></a>Wiederholungsrichtlinie

| NAME | Begrenzung | Notizen | 
| ---- | ----- | ----- | 
| Wiederholungsversuche | 90 | Der Standardwert ist 4. Um den Standardwert zu ändern, verwenden Sie den [Parameter für Richtlinienwiederholung](../logic-apps/logic-apps-workflow-actions-triggers.md) (retryPolicy). | 
| Maximale Wiederholungsverzögerung | 1 Tag | Um den Standardwert zu ändern, verwenden Sie den [Parameter für Richtlinienwiederholung](../logic-apps/logic-apps-workflow-actions-triggers.md) (retryPolicy). | 
| Minimale Wiederholungsverzögerung | 5 Sekunden | Um den Standardwert zu ändern, verwenden Sie den [Parameter für Richtlinienwiederholung](../logic-apps/logic-apps-workflow-actions-triggers.md) (retryPolicy). |
|||| 

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Grenzwerte für einen benutzerdefinierten Connector

Die folgenden Grenzwerte gelten für benutzerdefinierte Connectors, die Sie über Web-APIs erstellen können.

| NAME | Begrenzung | 
| ---- | ----- | 
| Anzahl von benutzerdefinierten Connectors | 1.000 pro Azure-Abonnement | 
| Anzahl der Anforderungen pro Minute für jede von einem benutzerdefinierten Connector erstellte Verbindung | 500 Anforderungen pro Verbindung |
|||| 

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Grenzwerte für Integrationskonten

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Artefaktgrenzwerte pro Integrationskonto

Die folgenden Grenzwerte gelten für die Anzahl von Artefakten für jedes Integrationskonto. Weitere Informationen hierzu finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps/).

*Free-Tarif*

| Artefakt | Begrenzung | Notizen | 
|----------|-------|-------| 
| EDI-Handelspartner | 25 | | 
| EDI-Handelsverträge | 10 | | 
| Karten | 25 | | 
| Schemas | 25 | 
| Assemblys | 10 | | 
| Batchkonfigurationen | 5 | 
| Zertifikate | 25 | | 
|||| 

*Basic-Tarif*

| Artefakt | Begrenzung | Notizen | 
|----------|-------|-------| 
| EDI-Handelspartner | 2 | | 
| EDI-Handelsverträge | 1 | | 
| Karten | 500 | | 
| Schemas | 500 | 
| Assemblys | 25 | | 
| Batchkonfigurationen | 1 | | 
| Zertifikate | 2 | | 
|||| 

*Standard-Tarif*

| Artefakt | Begrenzung | Notizen | 
|----------|-------|-------| 
| EDI-Handelspartner | 500 | | 
| EDI-Handelsverträge | 500 | | 
| Karten | 500 | | 
| Schemas | 500 | 
| Assemblys | 50 | | 
| Batchkonfigurationen | 5 |  
| Zertifikate | 50 | | 
|||| 

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Artefaktkapazitätsgrenzen

| NAME | Begrenzung | Notizen | 
| ---- | ----- | ----- | 
| Schema | 8 MB | Wenn Sie Dateien hochladen möchten, die größer sind als 2 MB, verwenden Sie den [Blob-URI](../logic-apps/logic-apps-enterprise-integration-schemas.md). | 
| Zuordnung (XSLT-Datei) | 2 MB | | 
| Endpunkt zur Laufzeit: Read-Aufrufe pro 5 Minuten | 60.000 | Sie können den Workload nach Bedarf auf mehrere Konten verteilen. | 
| Endpunkt zur Laufzeit: Invoke-Aufrufe pro 5 Minuten | 45.000 | Sie können den Workload nach Bedarf auf mehrere Konten verteilen. | 
| Endpunkt zur Laufzeit: Aufrufüberwachungen pro 5 Minuten | 45.000 | Sie können den Workload nach Bedarf auf mehrere Konten verteilen. | 
| Endpunkt zur Laufzeit: Gleichzeitige Aufrufe zum Blockieren | ca. 1.000 | Sie können die Anzahl gleichzeitiger Anforderungen oder die Dauer nach Bedarf verringern. | 
||||  

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>Nachrichtengröße für B2B-Protokoll (AS2, X12, EDIFACT)

Hier sind die Grenzwerte, die für B2B-Protokolle gelten:

| NAME | Begrenzung | Notizen | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Gilt für das Decodieren und das Codieren | 
| X12 | 50 MB | Gilt für das Decodieren und das Codieren | 
| EDIFACT | 50 MB | Gilt für das Decodieren und das Codieren | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Konfiguration: IP-Adressen

### <a name="azure-logic-apps-service"></a>Azure Logic Apps-Dienst

Für alle Logik-Apps in einer Region wird derselbe Bereich von IP-Adressen verwendet.
Die Aufrufe, die Logik-Apps direkt über [HTTP](../connectors/connectors-native-http.md), [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) oder andere HTTP-Anforderungen vornehmen, stammen von IP-Adressen in dieser Liste. 

| Logic Apps-Bereich | Ausgehende IP-Adresse |
|-------------------|-------------|
| Australien | 13.73.114.207, 13.77.3.139, 13.70.159.205 |
| Australien (Osten) | 13.75.149.4, 104.210.91.55, 104.210.90.241 |
| Brasilien Süd | 191.235.82.221, 191.235.91.7, 191.234.182.26 |
| Kanada, Mitte | 52.233.29.92, 52.228.39.241, 52.228.39.244 |
| Kanada, Osten | 52.232.128.155, 52.229.120.45, 52.229.126.25 |
| Indien, Mitte | 52.172.154.168, 52.172.186.159, 52.172.185.79 |
| USA (Mitte) | 13.67.236.125, 104.208.25.27, 40.122.170.198 |
| Asien, Osten | 13.75.94.173, 40.83.127.19, 52.175.33.254 |
| USA (Ost) | 13.92.98.111, 40.121.91.41, 40.114.82.191 |
| USA (Ost) 2 | 40.84.30.147, 104.208.155.200, 104.208.158.174 |
| Japan, Osten | 13.71.158.3, 13.73.4.207, 13.71.158.120 |
| Japan, Westen | 40.74.140.4, 104.214.137.243, 138.91.26.45 |
| USA Nord Mitte | 168.62.248.37, 157.55.210.61, 157.55.212.238 |
| Nordeuropa | 40.113.12.95, 52.178.165.215, 52.178.166.21 |
| USA Süd Mitte | 104.210.144.48, 13.65.82.17, 13.66.52.232 |
| Indien (Süden) | 52.172.50.24, 52.172.55.231, 52.172.52.0 |
| Asien, Südosten | 13.76.133.155, 52.163.228.93, 52.163.230.166 |
| USA, Westen-Mitte | 52.161.27.190, 52.161.18.218, 52.161.9.108 |
| Europa, Westen | 40.68.222.65, 40.68.209.23, 13.95.147.65 |
| Indien, Westen | 104.211.164.80, 104.211.162.205, 104.211.164.136 |
| USA (Westen) | 52.160.92.112, 40.118.244.241, 40.118.241.243 |
| USA, Westen 2 | 13.66.210.167, 52.183.30.169, 52.183.29.132 |
| UK, Süden | 51.140.74.14, 51.140.73.85, 51.140.78.44 |
| UK, Westen | 51.141.54.185, 51.141.45.238, 51.141.47.136 |
| | |

| Logic Apps-Bereich | Eingehende IP-Adresse |
|-------------------|-------------|
| Australien (Osten) | 3.75.153.66, 104.210.89.222, 104.210.89.244 |
| Australien, Südosten | 13.73.115.153, 40.115.78.70, 40.115.78.237 |
| Brasilien Süd | 191.235.86.199, 191.235.95.229, 191.235.94.220 |
| Kanada, Mitte | 13.88.249.209, 52.233.30.218, 52.233.29.79 |
| Kanada, Osten | 52.232.129.143, 52.229.125.57, 52.232.133.109 |
| Indien, Mitte | 52.172.157.194, 52.172.184.192, 52.172.191.194 |
| USA (Mitte) | 13.67.236.76, 40.77.111.254, 40.77.31.87 |
| Asien, Osten | 168.63.200.173, 13.75.89.159, 23.97.68.172 |
| USA (Ost) | 137.135.106.54, 40.117.99.79, 40.117.100.228 |
| USA (Ost) 2 | 40.84.25.234, 40.79.44.7, 40.84.59.136 |
| Japan, Osten | 13.71.146.140, 13.78.84.187, 13.78.62.130 |
| Japan, Westen | 40.74.140.173, 40.74.81.13, 40.74.85.215 |
| USA Nord Mitte | 168.62.249.81, 157.56.12.202, 65.52.211.164 |
| Nordeuropa | 13.79.173.49, 52.169.218.253, 52.169.220.174 |
| USA Süd Mitte | 52.172.9.47, 52.172.49.43, 52.172.51.140 |
| Indien (Süden) | 52.172.9.47, 52.172.49.43, 52.172.51.140 |
| Asien, Südosten | 52.163.93.214, 52.187.65.81, 52.187.65.155 |
| USA, Westen-Mitte | 52.161.26.172, 52.161.8.128, 52.161.19.82 |
| Europa, Westen | 13.95.155.53, 52.174.54.218, 52.174.49.6 |
| Indien, Westen | 104.211.164.112, 104.211.165.81, 104.211.164.25 |
| USA (Westen) | 52.160.90.237, 138.91.188.137, 13.91.252.184 |
| USA, Westen 2 | 13.66.224.169, 52.183.30.10, 52.183.39.67 |
| UK, Süden | 51.140.79.109, 51.140.78.71, 51.140.84.39 |
| UK, Westen | 51.141.48.98, 51.141.51.145, 51.141.53.164 |
| | |

### <a name="connectors"></a>Connectors

Die Aufrufe der [Connectors](../connectors/apis-list.md) stammen von den IP-Adressen in dieser Liste.

| Logic Apps-Bereich | Ausgehende IP-Adresse |
|-------------------|-------------|
| Australien (Osten) | 40.126.251.213 |
| Australien, Südosten | 40.127.80.34 |
| Brasilien Süd | 191.232.38.129 |
| Kanada, Mitte | 52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13 |
| Kanada, Osten | 52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52 |
| Indien, Mitte | 104.211.98.164 |
| USA (Mitte) | 40.122.49.51 |
| Asien, Osten | 23.99.116.181 |
| USA (Ost) | 191.237.41.52 |
| USA (Ost) 2 | 104.208.233.100 |
| Japan, Osten | 40.115.186.96 |
| Japan, Westen | 40.74.130.77 |
| USA Nord Mitte | 65.52.218.230 |
| Nordeuropa | 104.45.93.9 |
| USA Süd Mitte | 104.214.70.191 |
| Indien (Süden) | 104.211.227.225 |
| Asien, Südosten | 13.76.231.68 |
| Europa, Westen | 40.115.50.13 |
| Indien, Westen | 104.211.161.203 |
| USA (Westen) | 104.40.51.248 |
| UK, Süden | 51.140.80.51 |
| UK, Westen | 51.141.47.105 |
| | | 

## <a name="next-steps"></a>Nächste Schritte  

* [Erstellen Ihrer ersten Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* [Allgemeine Beispiele und Szenarios](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Video: Automatisieren von Geschäftsprozessen mit Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Video: Integrieren Ihrer Systeme in Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
