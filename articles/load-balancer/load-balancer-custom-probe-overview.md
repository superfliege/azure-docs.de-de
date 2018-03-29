---
title: Verwenden von benutzerdefinierten Load Balancer-Tests zum Überwachen des Integritätsstatus | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit benutzerdefinierten Tests für Azure Load Balancer Instanzen hinter einem Load Balancer überwachen.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/8/2018
ms.author: kumud
ms.openlocfilehash: 0aab72fdf48589a72707ae87f90af11f65f35088
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="understand-load-balancer-probes"></a>Grundlegendes zu Load Balancer-Tests

Azure Load Balancer nutzt Integritätstests, um zu bestimmen, welche Back-End-Poolinstanz neue Datenflüsse erhalten soll. Wenn ein Integritätstest fehlschlägt, stoppt Load Balancer das Senden von neuen Datenflüssen an die jeweilige fehlerhafte Instanz. Vorhandene Datenflüsse auf dieser Instanz sind hiervon nicht betroffen.  Wenn alle Tests für Back-End-Poolinstanzen ausgefallen sind, tritt für alle vorhandenen Datenflüsse auf allen Instanzen im Back-End-Pool eine Zeitüberschreitung auf.

Clouddienstrollen (Workerrollen und Webrollen) verwenden einen Gast-Agent für die Testüberwachung. Wenn Sie virtuelle Computer hinter einem Load Balancer verwenden, muss ein benutzerdefinierter TCP- oder HTTP-Integritätstest konfiguriert werden.

## <a name="understand-probe-count-and-timeout"></a>Grundlegendes zu Anzahl und Timeout von Tests

Das Verhalten von Tests hängt von folgenden Faktoren ab:

* Anzahl der erfolgreichen Tests, bei der eine Instanz als „In Betrieb“ bezeichnet werden kann.
* Anzahl der fehlerhaften Tests, bei der eine Instanz als „Ausgefallen“ bezeichnet wird.

Die in „SuccessFailCount“ festgelegten Werte für Timeout und Häufigkeit legen fest, ob eine Instanz ausgeführt oder nicht ausgeführt wird. Im Azure-Portal wird das Timeout auf das Doppelte des Werts für die Häufigkeit festgelegt.

Die Konfiguration von Tests muss für alle Instanzen mit Lastenausgleich für einen Endpunkt (mit anderen Worten: eine Gruppe mit Lastenausgleich) identisch sein. Sie können nicht für jede Rolleninstanz oder VM im selben gehosteten Dienst für eine bestimmte Endpunktkombination eine unterschiedliche Testkonfiguration wählen. Beispielsweise muss jede Instanz identische lokale Ports und Timeouts aufweisen.

> [!IMPORTANT]
> Ein Load Balancer-Test verwendet die IP-Adresse 168.63.129.16. Diese öffentliche IP-Adresse ermöglicht die Kommunikation mit internen Plattformressourcen für das Azure Virtual Network-Szenario mit eigener IP-Adresse. Die virtuelle öffentliche IP-Adresse 168.63.129.16 wird in allen Regionen verwendet und nicht geändert. Es wird empfohlen, dass Sie diese IP-Adresse in lokalen Firewallrichtlinien zulassen. Dies dürfte kein Sicherheitsrisiko darstellen, da nur die interne Azure-Plattform eine Nachricht von dieser Adresse senden kann. Wenn Sie diese IP-Adresse in Ihren Firewallrichtlinien nicht zulassen, tritt in einer Vielzahl von Szenarien ein unerwartetes Verhalten auf. Das Verhalten umfasst die Konfiguration desselben IP-Adressbereichs von 168.63.129.16 und das Duplizieren von IP-Adressen.

## <a name="learn-about-the-types-of-probes"></a>Weitere Informationen über die Testtypen

### <a name="guest-agent-probe"></a>Gast-Agent-Test

Ein Gast-Agent-Test ist nur für Azure Cloud Services verfügbar. Der Load Balancer nutzt den Gast-Agent innerhalb der VM. Er lauscht dann und antwortet nur mit einer HTTP-OK-200-Antwort, wenn sich die Instanz im Zustand „Bereit“ befindet. (Andere Zustände sind „Beschäftigt“, „Wird wiederverwendet“ oder „Wird beendet“.)

Weitere Informationen finden Sie unter [Konfigurieren der Dienstdefinitionsdatei (CSDEF) für Integritätstests](https://msdn.microsoft.com/library/azure/ee758710.aspx) oder [Erste Schritte durch Erstellen eines öffentlichen Lastenausgleichs für Clouddienste](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Was kann einen Gast-Agent-Test veranlassen, eine Instanz als fehlerhaft zu markieren?

Wenn der Gast-Agent nicht mit dem HTTP-OK-Code 200 antwortet, kennzeichnet der Lastenausgleich die Instanz als nicht reagierend. Er sendet dann keinen Datenverkehr mehr an diese Instanz. Das Lastenausgleichsmodul pingt die Instanz weiterhin. Wenn der Gast-Agent mit dem HTTP-Code 200 antwortet, sendet der Lastenausgleich wieder Datenverkehr an diese Instanz.

Wenn Sie eine Webrolle verwenden, wird der Websitecode in der Regel in „w3wp.exe“ ausgeführt. Dieses Programm wird nicht von der Azure-Fabric oder vom Gast-Agent überwacht. Fehler in „w3wp.exe“ (z. B. HTTP 500-Antworten) werden dem Gast-Agent nicht gemeldet. Folglich nimmt der Lastenausgleich diese Instanz nicht aus der Rotation.

### <a name="http-custom-probe"></a>Benutzerdefinierter HTTP-Test

Der benutzerdefinierte HTTP-Test setzt den Gast-Agent-Standardtest außer Kraft. Sie können Ihre eigene Logik zum Ermitteln der Integrität der Rolleninstanz erstellen. Der Lastenausgleich testet Ihren Endpunkt standardmäßig alle 15 Sekunden. Die Instanz wird in die Lastenausgleichrotation einbezogen, wenn sie innerhalb des Zeitlimits mit HTTP 200 reagiert. Das Zeitlimit beträgt standardmäßig 31 Sekunden.

Ein benutzerdefinierter HTTP-Test kann für die Implementierung Ihrer eigenen Logik zum Entfernen von Instanzen aus der Lastenausgleichrotation nützlich sein. Sie können z. B. eine Instanz entfernen, wenn sie über 90 % CPU beansprucht und einen anderen Status als 200 zurückgibt. Wenn Sie über Webrollen verfügen, die „w3wp.exe“ verwenden, erhalten Sie auch eine automatische Überwachung Ihrer Website. Fehler in Ihrem Websitecode geben einen anderen Status als 200 an den Lastenausgleichstest zurück.

> [!NOTE]
> Der benutzerdefinierte HTTP-Test unterstützt nur relative Pfade und das HTTP-Protokoll. HTTPS wird nicht unterstützt.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Was kann einen benutzerdefinierten HTTP-Test veranlassen, eine Instanz als fehlerhaft zu markieren?

* Die HTTP-Anwendung gibt einen anderen HTTP-Antwortcode als 200 zurück (z. B. 403, 404 oder 500). Diese positive Bestätigung warnt Sie, dass die Anwendungsinstanz sofort außer Betrieb genommen werden muss.
* Der HTTP-Server reagiert nach dem Timeoutzeitraum nicht mehr. Je nach dem festgelegten Timeoutwert können mehrere Testanforderungen unbeantwortet bleiben, bevor der Test als nicht ausgeführt markiert wird (d. h. bevor SuccessFailCount-Tests gesendet werden).
* Der Server schließt die Verbindung durch „TCP Reset“.

### <a name="tcp-custom-probe"></a>Benutzerdefinierter TCP-Test

Benutzerdefinierte TCP-Tests leiten eine Verbindung über einen Drei-Wege-Handshake mit dem definierten Port ein.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Was kann einen benutzerdefinierten TCP-Test veranlassen, eine Instanz als fehlerhaft zu markieren?

* Der TCP-Server reagiert nach dem Timeoutzeitraum nicht mehr. Wann der Test als nicht ausgeführt markiert wird, hängt von der Anzahl fehlerhafter Testanforderungen ab, die unbeantwortet bleiben können, bevor der Test als nicht ausgeführt gilt.
* Der Test empfängt ein TCP Reset von der Rolleninstanz.

Weitere Informationen zum Konfigurieren eines HTTP-Integritätstests oder eines TCP-Tests finden Sie unter [Erste Schritte zum Erstellen eines öffentlichen Load Balancers in Resource Manager unter Verwendung von PowerShell](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-the-load-balancer-rotation"></a>Erneutes Hinzufügen fehlerfreier Instanzen zur Lastenausgleichrotation

TCP- und HTTP-Tests werden als fehlerfrei eingestuft und markieren die Rolleninstanz als fehlerfrei, wenn:

* Beim ersten Start der VM erhält der Lastenausgleich einen positiven Test.
* Der Wert für „SuccessFailCount“ (oben beschrieben) definiert die Anzahl erfolgreicher Tests, die erforderlich sind, um die Rolleninstanz als fehlerfrei zu markieren. Wenn eine Rolleninstanz entfernt wurde, muss die Anzahl der erfolgreichen, aufeinanderfolgenden Tests gleich oder größer sein als der Wert von SuccessFailCount, damit die Rolleninstanz als aktiv markiert wird.

> [!NOTE]
> Wenn die Integrität einer Rolleninstanz schwankt, wartet der Lastenausgleich länger, ehe die Rolleninstanz wieder in den fehlerfreien Zustand versetzt wird. Diese zusätzliche Wartezeit schützt den Benutzer und die Infrastruktur und ist eine bewusste Richtlinie.

## <a name="use-log-analytics-for-a-load-balancer"></a>Verwenden der Protokollanalyse für einen Lastenausgleich

Mit der [Protokollanalyse](load-balancer-monitor-log.md) können Sie den Testintegritätsstatus und die Testanzahl für den Lastenausgleich überprüfen. Die Protokollierung kann mit Power BI oder Azure Operational Insights verwendet werden, um Statistiken zum Integritätsstatus des Lastenausgleichs bereitzustellen.
