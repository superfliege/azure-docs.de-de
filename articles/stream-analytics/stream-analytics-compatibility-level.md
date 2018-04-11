---
title: Verstehen des Kompatibilitätsgrads für Azure Stream Analytics-Aufträge | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Kompatibilitätsgrad für einen Azure Stream Analytics-Auftrag festlegen und welche größeren Änderungen am neuesten Kompatibilitätsgrad vorgenommen wurden.
keywords: Kompatibilitätsgrad, Streamen von Daten
documentationcenter: ''
services: stream-analytics
author: SnehaGunda
manager: kfile
ms.assetid: ''
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/03/2018
ms.author: sngun
ms.openlocfilehash: 6f7f2314d225aa02be28fa817151f8f245478cf0
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Kompatibilitätsgrad für Azure Stream Analytics-Aufträge
 
Der Kompatibilitätsgrad bezieht sich auf das releasespezifischen Verhalten des Azure Stream Analytics-Diensts. Azure Stream Analytics ist ein verwalteter Dienst mit regelmäßigen Featureupdates und Leistungsverbesserungen. Normalerweise sind Updates automatisch für Endbenutzer verfügbar. Allerdings können einige neue Features zu grundlegenden Änderungen führen, z.B. zu Veränderungen am Verhalten eines bestehenden Auftrags, Änderungen bei den Prozessen, die Daten aus diesen Aufträgen nutzen usw. Mithilfe eines Kompatibilitätsgrads wird eine wesentliche Änderung bei Stream Analytics dargestellt. Bei grundlegenden Änderungen wird immer ein neuer Kompatibilitätsgrad eingeführt. 

Der Kompatibilitätsgrad stellt sicher, dass vorhandene Aufträge ohne Fehler ausgeführt werden. Wenn Sie einen neuen Stream Analytics-Auftrag erstellen, gilt es als bewährte Methode, diesen mit dem neuesten Kompatibilitätsgrad, der für Sie verfügbar ist, zu erstellen. 
 
## <a name="set-a-compatibility-level"></a>Festlegen eines Kompatibilitätsgrads 

Der Kompatibilitätsgrad steuert das Laufzeitverhalten eines Stream Analytics-Auftrags. Sie können den Kompatibilitätsgrad für einen Stream Analytics-Auftrag über das Portal oder mithilfe des [REST-API-Aufrufs „create job“](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job) festlegen. Azure Stream Analytics unterstützt zurzeit zwei Kompatibilitätsgrade: 1.0 und 1.1. Standardmäßig ist der Kompatibilitätsgrad auf „1.0“ festgelegt, der mit der allgemeinen Verfügbarkeit von Azure Stream Analytics eingeführt wurde. Navigieren Sie zum Aktualisieren des Standardwerts zum vorhandenen Stream Analytics-Auftrag. Wählen Sie die Option **Kompatibilitätsgrad** im Abschnitt **Konfigurieren** aus, und ändern Sie den Wert. 

Sie müssen den Auftrag beenden, bevor Sie den Kompatibilitätsgrad aktualisieren. Der Kompatibilitätsgrad kann nicht aktualisiert werden, wenn der Auftrag ausgeführt wird. 

![Kompatibilitätsgrad im Portal](media\stream-analytics-compatibility-level/image1.png)

 
Wenn Sie den Kompatibilitätsgrad aktualisieren, überprüft der T-SQL-Compiler den Auftrag mit der Syntax, die dem ausgewählten Kompatibilitätsgrad entspricht. 

## <a name="major-changes-in-the-latest-compatibility-level-11"></a>Wichtige Änderungen beim aktuellen Kompatibilitätsgrad (1.1)

Beim Kompatibilitätsgrad 1.1 wurden die folgenden grundlegenden Änderungen eingeführt:

* **Service Bus-XML-Format**  

  * **Vorgängerversionen:** Azure Stream Analytics verwendete DataContractSerializer, damit der Inhalt der Meldung XML-Tags enthielt. Beispiel: 
    
   @\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{ “SensorId”:”1”, “Temperature”:64\}\u0001 

  * **Aktuelle Version:** Der Nachrichteninhalt enthält den Datenstrom direkt und ohne weitere Tags. Beispiel: 
  
   { "SensorId":"1", "Temperature":64} 
 
* **Beibehalten von Groß-/Kleinbuchstaben für Feldnamen**  

  * **Vorgängerversionen:** Feldnamen wurden in Kleinbuchstaben umgewandelt, wenn sie vom Azure Stream Analytics-Modul verarbeitet wurden. 

  * **Aktuelle Version:** Die Groß- oder Kleinschreibung für Feldnamen wird beibehalten, während sie vom Azure Stream Analytics-Modul verarbeitet werden. 

  > [!NOTE] 
  > Das Beibehalten der Groß- und Kleinschreibung ist für Stream Analytics-Aufträge, die mithilfe der Edge-Umgebung gehostet werden, noch nicht verfügbar. Daher werden alle Feldnamen in Kleinbuchstaben konvertiert, wenn der Auftrag auf Edge gehostet wird. 

* **FloatNaNDeserializationDisabled**  

  * **Vorgängerversionen:** Der Befehl CREATE TABLE filterte nicht nach Ereignissen mit NaN (Not-a-Number, z.B. Infinity, -Infinity) in einer Spalte vom Typ FLOAT, da diese sich außerhalb des dokumentierten Bereichs für diese Zahlen befinden.

  * **Aktuelle Version:** CREATE TABLE erlaubt die Angabe eines festen Schemas. Das Stream Analytics-Modul überprüft, ob die Daten diesem Schema entsprechen. Mit diesem Modell kann der Befehl Ereignisse mit NaN-Werten filtern. 

* **Deaktivieren der automatischen Typumwandlung nach oben für datetime-Zeichenfolgen im JSON-Format**  

  * **Vorgängerversionen:** Der JSON-Parser führte automatisch eine Typumwandlung nach oben für Zeichenfolgen mit Informationen zu Datum/Uhrzeit/Zeitzone in den Typ DateTime durch und konvertierte diese dann in UTC. Dies führte zum Verlust von Zeitzoneninformationen.

  * **Aktuelle Version:** Es erfolgt keine automatische Typumwandlung nach oben mehr für Zeichenfolgenwerte mit Informationen zu Datum/Uhrzeit/Zeitzone in den Typ DateTime. Daher werden die Zeitzoneninformationen beibehalten. 

## <a name="next-steps"></a>Nächste Schritte
* [Leitfaden zur Behandlung von Problemen bei Azure Stream Analytics](stream-analytics-troubleshooting-guide.md)
* [Stream Analytics-Blatt „Ressourcenintegrität“](stream-analytics-resource-health.md)