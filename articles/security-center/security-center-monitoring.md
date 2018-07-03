---
title: Sicherheitsüberwachung in Azure Security Center | Microsoft-Dokumentation
description: Diese Artikel bietet eine Einführung in die Überwachungsfunktionen von Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: terrylan
ms.openlocfilehash: dc362306e20b4d39aa73a552e47cbcbd3037edbd
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063390"
---
# <a name="security-health-monitoring-in-azure-security-center"></a>Überwachen der Sicherheitsintegrität in Azure Security Center
Dieser Artikel unterstützt Sie bei der Überwachung der Richtliniencompliance mithilfe der Überwachungsfunktionen von Azure Security Center.

## <a name="what-is-security-health-monitoring"></a>Was ist Überwachen der Sicherheitsintegrität?
Überwachung wird häufig als Beobachten und Warten auf ein Eintreten eines Ereignisses aufgefasst, damit auf diese Situation reagiert werden kann. Sicherheitsüberwachung bezieht sich auf eine proaktive Strategie, bei der Ihre Ressourcen überwacht werden, um Systeme zu erkennen, die nicht den Unternehmensstandards oder bewährten Methoden entsprechen.

## <a name="monitoring-security-health"></a>Überwachung der Sicherheitsintegrität
Nach Aktivierung der [Sicherheitsrichtlinien](security-center-policies.md) für die Ressourcen eines Abonnements analysiert Security Center die Sicherheit Ihrer Ressourcen, um mögliche Sicherheitsrisiken zu erkennen. Informationen zur Netzwerkkonfiguration sind sofort verfügbar. Abhängig davon, auf wie vielen Ihrer (virtuellen und physischen) Computer der Agent installiert ist, kann es eine Stunde oder länger dauern, bis die Konfigurationsinformationen für die Computer (beispielsweise Sicherheitsupdatestatus und Betriebssystemkonfiguration) gesammelt wurden und verfügbar sind. Der Sicherheitsstatus Ihrer Ressourcen sowie ggf. vorliegende Probleme werden im Abschnitt **Prävention** angezeigt. Eine Liste mit diesen Problemen steht auch auf der Kachel **Empfehlungen** zur Verfügung.

Weitere Informationen zur Anwendung der Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).

Unter **Überwachung der Ressourcenintegrität** können Sie den Sicherheitszustand Ihrer Ressourcen überwachen. Im folgenden Beispiel sehen Sie, dass auf der Kachel der jeweiligen Ressource (Compute und Apps, Netzwerk, Datensicherheit und Identität und Zugriff) die Gesamtzahl der identifizierten Probleme angegeben ist.

![Kachel „Sicherheitsintegrität der Ressource“](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)


### <a name="monitor-compute--apps"></a>Überwachen von Compute und Apps
Weitere Informationen finden Sie unter [Schützen Ihrer virtuellen Computer in Azure Security Center](security-center-virtual-machine-recommendations.md).

### <a name="monitor-virtual-networks"></a>Überwachen virtueller Netzwerke
Wenn Sie auf die Kachel **Netzwerk** klicken, wird das Blatt **Netzwerk** mit weiteren Details geöffnet, wie im folgenden Screenshot zu sehen:

![Blatt „Netzwerk“](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Netzwerkempfehlungen
Auch hier sehen Sie (ähnlich wie bei den Ressourcenintegritätsinformationen des virtuellen Computers) im oberen Bereich eine zusammengefasste Liste mit Problemen und im unteren Bereich eine Liste mit den überwachten Netzwerken.

Im Abschnitt für die Statusanalyse der Netzwerke sind potenzielle Sicherheitsprobleme und entsprechende [Empfehlungen](security-center-network-recommendations.md)aufgeführt. Beispiele für Probleme:

* Keine Firewall der nächsten Generation (Next Generation Firewall, NGFW) installiert
* Keine Netzwerksicherheitsgruppen für Subnetze aktiviert
* Keine Netzwerksicherheitsgruppen für virtuelle Computer aktiviert
* Eingeschränkter Zugriff über öffentliche externe Endpunkte
* Fehlerfreie Endpunkte mit Internetzugriff

Wenn Sie auf eine Empfehlung klicken, erscheinen weitere Details, wie im folgenden Beispiel gezeigt:

![Details für eine Empfehlung unter „Netzwerk“](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

In diesem Beispiel wird unter **Fehlende Netzwerksicherheitsgruppen für Subnetze konfigurieren** eine Liste mit Subnetzen und virtuellen Computern angezeigt, die nicht durch eine Netzwerksicherheitsgruppe geschützt sind. Wenn Sie auf das Subnetz klicken, auf das Sie die Netzwerksicherheitsgruppe anwenden möchten, erscheint **Netzwerksicherheitsgruppe auswählen**. Hier können Sie eine geeignete Netzwerksicherheitsgruppe für das Subnetz auswählen oder eine neue Netzwerksicherheitsgruppe erstellen.

#### <a name="internet-facing-endpoints-section"></a>Abschnitt „Endpunkte mit Internetanbindung“
Im Abschnitt **Endpunkte mit Internetzugriff** werden die virtuellen Computer, die derzeit mit einem Endpunkt mit Internetzugriff konfiguriert sind, und der jeweilige Status angezeigt.

![Mit einem Endpunkt mit Internetzugriff konfigurierte virtuelle Computer und Status](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Diese Tabelle enthält den Endpunktnamen, der den virtuellen Computer darstellt, die Internet-IP-Adresse und den aktuellen Status der Netzwerksicherheitsgruppe und der NGFW. Die Tabelle ist nach Schweregrad sortiert:

* Rot (oben): Hohe Priorität; sollte umgehend behoben werden.
* Orange: Mittlere Priorität; sollte zeitnah behoben werden.
* Grün (ganz unten): Fehlerfrei.

#### <a name="networking-topology-section"></a>Abschnitt „Netzwerktopologie“
Der Abschnitt **Netzwerktopologie** enthält eine hierarchische Ansicht der Ressourcen, wie im folgenden Screenshot zu sehen:

![Hierarchische Ansicht der Ressourcen im Abschnitt „Netzwerktopologie“](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Diese Tabelle ist nach Schweregrad sortiert (virtuelle Computer und Subnetze):

* Rot (oben): Hohe Priorität; sollte umgehend behoben werden.
* Orange: Mittlere Priorität; sollte zeitnah behoben werden.
* Grün (ganz unten): Fehlerfrei.

In dieser Hierarchie umfasst die erste Ebene [virtuelle Netzwerke](../virtual-network/virtual-networks-overview.md), [virtuelle Netzwerkgateways](/vpn-gateway/vpn-gateway-site-to-site-create.md) und [virtuelle Netzwerke (klassisch)](/virtual-network/virtual-networks-create-vnet-classic-pportal.md). Die zweite Ebene umfasst Subnetze und die dritte Ebene die virtuellen Computer dieser Subnetze. Die rechte Spalte enthält den aktuellen Status der Netzwerksicherheitsgruppe für diese Ressourcen, wie im folgenden Beispiel zu sehen:

![Status der Netzwerksicherheitsgruppe im Abschnitt „Netzwerktopologie“](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

Im unteren Bereich des Blatts befinden sich die Empfehlungen für diesen virtuellen Computer (ähnlich wie weiter oben beschrieben). Sie können auf eine Empfehlung klicken, um weitere Informationen zu erhalten oder die erforderliche Sicherheitskontrolle/-konfiguration anzuwenden.

### <a name="monitor-data-security"></a>Überwachen der Datensicherheit

Wenn Sie im Abschnitt **Prävention** auf **Datensicherheit** klicken, wird **Datenressourcen** mit Empfehlungen für SQL und Speicher geöffnet. Außerdem werden [Empfehlungen](security-center-sql-service-recommendations.md) zum allgemeinen Integritätsstatus der Datenbank abgegeben. Weitere Informationen zur Speicherverschlüsselung finden Sie unter [Enable encryption for Azure storage account in Azure Security Center](security-center-enable-encryption-for-storage-account.md) (Aktivieren der Verschlüsselung für Azure-Speicherkonten in Azure Security Center).

![Datenressourcen](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

Sie können unter **SQL Recommendations** (SQL-Empfehlungen) auf eine Empfehlung klicken und ausführlichere Informationen zu weiteren Aktionen erhalten, um ein Problem zu beheben. Das folgende Beispiel zeigt die Erweiterung der Empfehlung **Database Auditing & Threat detection on SQL databases** (Datenbanküberwachung und Bedrohungserkennung bei SQL-Datenbanken).

![Details zu einer SQL-Empfehlung](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

Unter **Überwachung und Bedrohungserkennung für SQL-Datenbanken aktivieren** werden folgende Informationen angezeigt:

* Liste der SQL-Datenbanken
* Server, auf dem sich eine Datenbank befindet
* Informationen dazu, ob die Einstellung vom Server geerbt wurde oder in der Datenbank eindeutig ist
* Aktueller Status
* Schweregrad des Problems

Wenn Sie auf die Datenbank klicken, um die Empfehlung zu implementieren, erscheint **Überwachung und Bedrohungserkennung**, wie im folgenden Screenshot zu sehen:

![Überwachung und Bedrohungserkennung](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Um die Überwachung zu aktivieren, wählen Sie für **Überwachung** die Option **EIN** aus.

### <a name="monitor-identity--access"></a>Überwachen von Identität und Zugriff

Weitere Informationen finden Sie unter [Überwachen von Identität und Zugriff in Azure Security Center](security-center-identity-access.md).

## <a name="see-also"></a>Weitere Informationen
In diesem Artikel haben Sie erfahren, wie Sie die Überwachungsfunktionen in Azure Security Center verwenden können. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Hier erfahren Sie, wie Sie Sicherheitseinstellungen in Azure Security Center konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.
