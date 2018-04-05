---
title: Azure Standard Load Balancer – Diagnose | Microsoft-Dokumente
description: Verwenden der verfügbaren Metriken und Integritätsinformationen für die Diagnose für Azure Standard Load Balancer
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: Kumud
ms.openlocfilehash: 7d60925381abe617f6e2fac51176b8e30517c3ba
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="metrics-and-health-diagnostics-for-standard-load-balancer"></a>Metriken und Integritätsdiagnosen für Standard Load Balancer

Azure Standard Load Balancer stellt die folgenden Diagnosefunktionen für Ihre Ressourcen bereit:
* Mehrdimensionale Metriken: Standard Load Balancer stellt neue mehrdimensionale Diagnosefunktionen sowohl für öffentliche als auch für interne Load Balancer-Konfigurationen bereit. Damit können Sie Ihre Load Balancer-Ressourcen überwachen, verwalten und hinsichtlich Fehlern behandeln.

* Resource Health: Die Seite „Load Balancer“ im Azure-Portal und die Seite „Resource Health“ (unter „Monitor“) machen die Ressourcenintegrität für die öffentliche Load Balancer-Konfiguration von Standard Load Balancer verfügbar.

Dieser Artikel enthält einen kurzen Überblick über diese Funktionen und zeigt Möglichkeiten auf, wie diese für Standard Load Balancer verwendet werden können.

## <a name = "MultiDimensionalMetrics"></a>Mehrdimensionale Metriken

Azure Load Balancer stellt die neuen mehrdimensionalen Metriken über die neuen Azure-Metriken (Vorschau) im Portal zur Verfügung und ermöglicht es Ihnen, in Echtzeit diagnostische Einblicke in Ihre Load Balancer-Ressourcen zu erhalten. 

Folgende Metriken sind derzeit für verschiedene Standard Load Balancer-Konfigurationen verfügbar:

| Metrik | Ressourcentyp | BESCHREIBUNG | Empfohlene Aggregation |
| --- | --- | --- | --- |
| VIP-Verfügbarkeit (Datenpfadverfügbarkeit) | Öffentlicher Load Balancer | Standard Load Balancer wendet kontinuierlich den Datenpfad aus einer Region auf das Load Balancer-Front-End bis hin zum SDN-Stapel an, der Ihren virtuellen Computer unterstützt. Solange integre Instanzen verbleiben, folgt die Messung demselben Pfad wie der Datenverkehr mit Lastenausgleich Ihrer Anwendungen. Der Datenpfad, der von Ihren Kunden verwendet wird, wird ebenfalls überprüft. Die Messung ist für Ihre Anwendung nicht sichtbar und bewirkt keine Beeinträchtigung bei anderen Vorgängen.| Durchschnitt |
| DIP-Verfügbarkeit (Integritätsteststatus) |  Öffentlicher und interner Load Blancer | Standard Load Balancer verwendet einen verteilten Integritätsprüfungsdienst, der die Integrität Ihres Anwendungsendpunkts gemäß Ihren Konfigurationseinstellungen überwacht. Diese Metrik stellt eine Aggregat- oder nach Endpunkt gefilterte Ansicht jedes einzelnen Instanzendpunkts im Load Balancer-Pool bereit.  Sie können sehen, wie Load Balancer die Integrität Ihrer Anwendung gemäß Ihrer Integritätsprüfungskonfiguration beurteilt. |  Durchschnitt |
| SYN-Pakete |  Öffentlicher Load Balancer | Standard Load Balancer beendet keine TCP-Verbindungen und interagiert nicht mit TCP- oder UDP-Paketdatenflüssen. Datenflüsse und deren Handshakes erfolgen immer zwischen der Quelle und der VM-Instanz. Für eine bessere Problembehandlung Ihrer TCP-Protokollszenarien können Sie SYN-Paketzähler verwenden, um zu verstehen, wie viele TCP-Verbindungsversuche vorgenommen werden. Die Metrik gibt die Anzahl der TCP-SYN-Pakete an, die empfangen wurden.| Durchschnitt |
| SNAT-Verbindungen |  Öffentlicher Load Balancer |Standard Load Balancer meldet die Anzahl von maskierten ausgehenden Flows an das Front-End mit der öffentlichen IP-Adresse. SNAT Ports stellen eine erschöpfbare Ressource dar. Diese Metrik kann einen Hinweis darauf geben, in welchem Umfang Ihre Anwendung SNAT für ausgehende Flows nutzt.  Die Zählerstände für erfolgreiche und fehlgeschlagene ausgehende SNAT-Flows werden gemeldet und können zur Problembehebung und Analyse Ihrer ausgehenden Flows verwendet werden.| Durchschnitt |
| Byteleistungsindikatoren |  Öffentlicher und interner Load Blancer | Standard Load Balancer meldet die pro Front-End verarbeiteten Daten.| Durchschnitt |
| Paketleistungsindikatoren |  Öffentlicher und interner Load Blancer | Standard Load Balancer meldet die pro Front-End verarbeiteten Pakete.| Durchschnitt |

### <a name="view-load-balancer-metrics-in-the-portal"></a>Anzeigen von Load Balancer-Metriken im Portal

Das Azure-Portal macht die Load Balancer Metriken über die Seite „Metriken (Vorschau)“ verfügbar, die auf der Seite „Load Balancer-Ressource“ für eine bestimmte Load Balancer-Ressource und auf der Seite „Azure Monitor“ verfügbar ist. 

Um die Metriken für Ihre Standard Load Balancer-Ressourcen anzuzeigen, navigieren Sie an einer dieser Stellen in „Metriken (Vorschau)“. Wählen Sie (die Load Balancer-Ressource aus, wenn Sie auf der Seite „Monitor“ sind) in der Dropdownliste den Metriktyp aus, legen Sie den entsprechenden Aggregationstyp fest, und konfigurieren Sie optional die erforderliche Filterung und Gruppierung. Danach können Sie die historische Ansicht für die Metriken unter den angegebenen Bedingungen sehen.

![Metrikenvorschau für Standard Load Balancer](./media/load-balancer-standard-diagnostics/LBMetrics1.png)

*Abbildung: Metrik für DIP-Verfügbarkeit/Integritätsteststatus für Load Balancer*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>Programmgesteuertes Abrufen von mehrdimensionalen Metriken über APIs

Eine API-Anleitung zum Abrufen der Definitionen und Werte für multidimensionale Metriken Definitionen und Werte finden Sie unter [Exemplarische Vorgehensweise für die Azure Monitoring-REST-API > Abrufen von Metrikdefinitionen (mehrdimensionale API)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api).

### <a name = "DiagnosticScenarios"></a>Allgemeine Diagnoseszenarien und empfohlene Ansichten

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>Ist der Datenpfad aktiv und für meine Load Balancer-VIPs verfügbar?

VIP-Verfügbarkeitsmetrik beschreibt die Integrität des Datenpfads innerhalb des Bereichs zu dem Computehost, auf dem sich Ihre virtuellen Computer befinden. Dies ist eine Reflektionsintegrität der Azure-Infrastruktur. Mit dieser Metrik können Sie:
- Die externe Verfügbarkeit Ihres Diensts überwachen
- Tiefer einsteigen und ermitteln, ob die Plattform, auf der Ihr Dienst bereitgestellt wird, fehlerfrei ist, oder ob Ihr Gastbetriebssystem oder die Anwendungsinstanz fehlerfrei ist
- Bestimmen, ob sich ein Ereignis auf Ihren Dienst oder die zugrunde liegende Datenebene bezieht. Verwechseln Sie dies nicht mit dem Integritätsteststatus („DIP-Verfügbarkeit“).

So rufen Sie die VIP-Verfügbarkeit für Ihre Standard Load Balancer-Ressourcen ab
- Vergewissern Sie sich, dass die richtige Load Balancer-Ressource ausgewählt ist. 
- Wählen Sie **VIP-Verfügbarkeit** in der Dropdownliste **Metrik** aus. 
- Wählen Sie **Mittelwert** für **Aggregation** aus. 
- Fügen Sie außerdem einen Filter für die VIP-Adresse oder den VIP-Port als die Dimension mit der erforderlichen Front-End-IP-Adresse oder dem erforderlichen Front-End-Port hinzu, und gruppieren Sie nach der ausgewählten Dimension.

![VIP-Sondierung](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Abbildung: VIP-Sondierungsdetails für Load Balancer*

Die Metrik wird durch eine aktive bandinterne Messung generiert. Ein Sondierungsdienst innerhalb der Region löst Datenverkehr für diese Messung aus und wird aktiviert, sobald Sie eine Bereitstellung mit einem öffentliche Front-End erstellen, und wir weiter ausgeführt, bis Sie das Front-End entfernen. 

>[!NOTE]
>Interne Front-Ends werden zu diesem Zeitpunkt nicht unterstützt. 

Ein Paket, das dem Front-End und der Regel Ihrer Bereitstellung entspricht, wird regelmäßig generiert. Es durchläuft die Region von der Quelle bis zu dem Host, auf dem sich ein virtueller Computer im Back-End-Pool befindet. Die Load Balancer-Infrastruktur führt dieselben Lastenausgleichs- und Übersetzungsvorgänge aus, wie sie dies für jeden anderen Datenverkehr tut. Diese Sondierung erfolgt bandintern auf Ihrem Endpunkt mit Lastenausgleich. Sobald der Test (Sondierung) auf dem Computehost eingeht, auf dem sich ein fehlerfreier virtueller Computer im Back-End-Pool befindet, generiert der Computehost eine Antwort an den Sondierungsdienst. Ihr virtueller Computer „sieht“ diesen Datenverkehr nicht.
VIP-Verfügbarkeit kann aus folgenden Gründen fehlschlagen:
- Ihre Bereitstellung hat keine fehlerfreien virtuellen Computern, die im Back-End-Pool verblieben sind. 
- Es ist ein Infrastrukturausfall aufgetreten, der ein Fehlschlagen von VIP-Verfügbarkeit verursacht.

Sie können die [VIP-Verfügbarkeitsmetrik zusammen mit dem Integritätsteststatus für Diagnosezwecke](#vipavailabilityandhealthprobes) verwenden.

Verwenden Sie **Mittelwert** als Aggregation für die meisten Szenarien.

#### <a name="are-the-backend-instances-for-my-vip-responding-to-probes"></a>Antworten die Back-End-Instanzen für meine VIP auf Tests?

Integritätstest-Statusmetrik beschreibt die Integrität Ihrer Anwendungsbereitstellung, wie Sie diese konfiguriert haben, als Sie den Integritätstest von Load Balancer konfiguriert haben. Load Balancer verwendet den Status des Integritätstests, um zu bestimmen, wohin neue Datenflüsse gesendet werden sollen. Integritätstests stammen aus einer Azure-Infrastrukturadresse und können im Gastbetriebssystem des virtuellen Computers angezeigt werden.

So rufen Sie die DIP-Verfügbarkeit für Ihre Standard Load Balancer-Ressourcen ab
- Wählen Sie die Metrik **DIP-Verfügbarkeit** mit dem Aggregationstyp **Mittelwert** aus. 
- Wenden Sie einen Filter auf die erforderliche VIP-IP-Adresse oder den erforderlichen VIP-Port (oder auf beides) an.

![DIP-Verfügbarkeit](./media/load-balancer-standard-diagnostics/LBMetrics-DIPAvailability.png)

*Abbildung: VIP-Verfügbarkeit für Load Balancer*

Integritätstests schlagen aus den folgenden Gründen fehl:
- Sie konfigurieren einen Integritätstest für einen Port, der nicht lauscht oder nicht reagiert, oder Sie konfigurieren den Test mit dem falschen Protokoll. Wenn für Ihren Dienst DSR-Regeln (Floating IP-Regeln) verwendet werden, müssen Sie sich vergewissern, dass der Dienst auf die IP-Adresse der IP-Konfiguration der Netzwerkkarte und nicht nur auf den Loopback lauscht, der mit der Front-End-IP-Adresse konfiguriert ist.
- Ihr Test wird von der Netzwerksicherheitsgruppe, der Firewall des Gastbetriebssystems des virtuellen Computers oder den Anwendungsschichtfilter nicht zugelassen.

Verwenden Sie **Mittelwert** als Aggregation für die meisten Szenarien.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>Wie überprüfe ich meine Statistiken für ausgehende Verbindungen? 

Eine SNAT-Verbindungen-Metrik beschreibt das Volumen erfolgreicher und fehlgeschlagene Datenflüsse (für [ausgehende Datenflüsse](https://aka.ms/lboutbound)).

Ein Volumen für fehlgeschlagene Verbindungen, das größer als 0 (null) ist, kennzeichnet eine SNAT-Portüberlastung. Sie müssen weitere Untersuchungen vornehmen und feststellen, wodurch diese Fehler verursacht werden können. Eine SNAT-Portüberlastung zeigt sich dadurch, dass keine [ausgehenden Datenflüsse](https://aka.ms/lboutbound) eingerichtet werden können. Lesen Sie den Artikel über ausgehende Verbindungen, um die Szenarien, die greifenden Mechanismen und die Maßnahmen für Abmildern oder Entwurf zu verstehen, um SNAT-Portüberlastung zu verhindern. 

So rufen Sie SNAT-Verbindungsstatistiken ab
- Wählen Sie den Metriktyp **SNAT-Verbindungen** und **Summe** als Aggregation aus. 
- Gruppieren Sie nach dem **Verbindungsstatus** für die Anzahl erfolgreicher und die Anzahl fehlgeschlagener SNAT-Verbindungen, die durch unterschiedliche Linien dargestellt werden. 

![SNAT-Verbindung](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Abbildung: Anzahl von SNAT-Verbindungen für Load Balancer*


#### <a name="how-do-i-check-inbound--outbound-connection-attempts-for-my-service"></a>Wie überprüfe ich Versuche für eingehende/ausgehende Verbindungen für meinen Dienst?

Eine SYN-Pakete-Metrik beschreibt das Volumen der TCP-SYN-Pakete, die angekommen sind oder gesendet wurden (für [ausgehende Datenflüsse](https://aka.ms/lboutbound)) und einem bestimmten Front-End zugeordnet sind. Mit dieser Metrik können TCP-Verbindungsversuche für Ihren Dienst ausgewertet werden.
Verwenden Sie „Gesamt“ als Aggregation für die meisten Szenarien.

![SYN-Verbindung](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Abbildung: SYN-Anzahl für Load Balancer*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>Wie überprüfe ich meine Netzwerkbandbreitennutzung? 

Eine Byteanzahl/Paketzahl-Metrik beschreibt das Volumen der Bytes und Pakete, die jeweils pro Front-End von Ihrem Dienst gesendet oder empfangen wurden.
Verwenden Sie **Gesamt** als Aggregation für die meisten Szenarien.

So rufen Sie eine Byteanzahl- oder Paketzahlstatistik ab
- Wählen Sie die Metriktypen **Byteanzahl** und/oder **Paketzahl** mit **Mittelwert** als Aggregation aus. 
- Wenden Sie einen Filter auf die gewünschte Komponente an: Front-End-IP, Front-End-Port oder Back-End-IP oder gewünschter Port. 
- Rufen Sie die Gesamtstatistik für die Load Balancer-Ressource ohne jegliche Filterung ab.


Einige Beispielansichten für Metriken mit unterschiedlichen Konfigurationen

![Byteanzahl](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Abbildung: Byteanzahl für Load Balancer*

#### <a name = "vipavailabilityandhealthprobes"></a>Wie führe ich eine Diagnose meiner Load Balancer-Bereitstellung durch?

Über die Kombination der Metriken „VIP-Verfügbarkeit“ und „Integritätstests“ in einem einzigen Diagramm können Sie erkennen, wo das Problem zu suchen ist und wie es sich beheben lässt. Sie können sich davon überzeugen, dass Azure ordnungsgemäß funktioniert, und damit eindeutig feststellen, dass die Konfiguration oder die Anwendung die Hauptursache ist.

Sie können Integritätstestmetriken verwenden, um zu erkennen, wie Azure die Integrität Ihrer Bereitstellung entsprechend der Konfiguration beurteilt, die Sie bereitgestellt haben. Ein Anzeigen der Integritätstests ist immer ein hervorragender erster Schritt beim Überwachen oder Ermitteln einer Ursache.

Sie können einen Schritt weiter gehen und VIP-Verfügbarkeit-Metriken verwenden, um zu erfahren, wie Azure die Integrität der zugrunde liegenden Datenebene beurteilt, die für Ihre spezielle Bereitstellung zuständig ist. Wenn Sie beide Metriken kombinieren, können Sie, wie in diesem Beispiel veranschaulicht, gezielt feststellen, wo sich der Fehler befindet:

![VIP-Diagnose](./media/load-balancer-standard-diagnostics/LBMetrics-DIPnVIPAvailability.png)

*Abbildung: Kombinieren von DIP- und VIP-Verfügbarkeit-Metriken*

Im Diagramm werden die folgenden Informationen angezeigt:
- Die Infrastruktur selbst war fehlerfrei, die Infrastruktur, in der Ihr virtueller Computer gehostet wird, war erreichbar, und im Back-End waren mehrere virtuelle Computer platziert. Dies ist durch die blaue Ablaufverfolgung für VIP-Verfügbarkeit gekennzeichnet, die als 100 % dargestellt wird. 
- Der Integritätsteststatus (DIP-Verfügbarkeit) ist jedoch am Anfang des Diagramms bei 0 %, wie dies die orangefarbene Ablaufverfolgung kennzeichnet. Der eingekreiste Bereich in Rot markiert, ab wann Integritätstests (DIP-Verfügbarkeit) fehlerfrei wurden und an welchem Punkt die Bereitstellung des Kunden neue Datenflüsse akzeptieren konnte.

Anhand es Diagramms konnte der Kunde eigenständig eine Fehlerbehebung für die Bereitstellung vornehmen, ohne zu vermuten oder beim Support zu erfragen, ob andere Probleme aufgetreten sind. Der Dienst des Kunden war nicht verfügbar, weil Integritätstests wegen einer Fehlkonfiguration oder einer fehlerhaften Anwendung fehlgeschlagen sind.

### <a name = "Limitations"></a>Einschränkungen

- VIP-Verfügbarkeit ist derzeit nur für öffentliche Front-Ends verfügbar.

## <a name = "ResourceHealth"></a>Ressourcenintegritätsstatus

Der Integritätsstatus für die Standard Load Balancer-Ressourcen wird über die vorhandene **Ressourcenintegrität** unter **Monitor > Service Health** verfügbar gemacht.

>[!NOTE]
>Die Resource Health (Ressourcenintegrität) für Load Balancer ist derzeit nur für die öffentliche Konfiguration von Standard Load Balancer verfügbar. Interne Load Balancer-Ressourcen oder Basic-SKU von Load Balancer-Ressourcen machen keine Ressourcenintegrität verfügbar.

So zeigen Sie die Integrität Ihrer öffentlichen Standard Load Balancer-Ressourcen an
 - Navigieren Sie zu **Monitor > Service Health**.

  ![Seite „Monitor“](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Abbildung: „Service Health“ (Dienstintegrität) in Azure Monitor*

 - Wählen Sie **Resource Health** aus, und stellen Sie sicher, dass die richtige **Abonnement-ID** und **Ressourcentyp = Load Balancer** ausgewählt ist.

  ![Ressourcenintegritätsstatus](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Abbildung: Ressource für Integritätsanzeige auswählen*

 - Klicken Sie in der Liste auf die Load Balancer-Ressource, um deren Verlauf des Integritätsstatus anzuzeigen.

    ![Load Balancer-Integritätsstatus](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Abbildung: Ansicht des Load Balancer-Integritätsstatus*
 
In der folgenden Tabelle sind die verschiedenen Ressourcenintegritätsstatus und deren Beschreibungen aufgeführt. 

| Ressourcenintegritätsstatus | BESCHREIBUNG |
| --- | --- |
| Verfügbar | Ihre öffentliche Standard Load Balancer-Ressource ist fehlerfrei und verfügbar. |
| Nicht verfügbar | Ihre öffentliche Standard Load Balancer-Ressource ist nicht fehlerfrei. Führen Sie über „Azure Monitor > Metriken“ eine Diagnose der Integrität aus. (*Dies kann auch bedeuten, dass die Ressource keine öffentliche Standard Load Balancer-Ressource ist.*) |
| Unknown | Die Resource Health (Ressourcenintegrität) für Ihre öffentliche Standard Load Balancer-Ressource wurde noch nicht aktualisiert. (*Dies kann auch bedeuten, dass die Ressource keine öffentliche Standard Load Balancer-Ressource ist.*)  |

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Load Balancer Standard](load-balancer-standard-overview.md).
- Weitere Informationen zu [ausgehenden Verbindungen für Load Balancer](https://aka.ms/lboutbound)


