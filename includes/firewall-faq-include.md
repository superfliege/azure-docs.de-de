---
title: Includedatei
description: Includedatei
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 7/30/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: e23579479c61810d651bebae7b486b53aaaf0d42
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39361384"
---
### <a name="what-is-azure-firewall"></a>Was ist Azure Firewall?

Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt. Es ist eine vollständig zustandsbehaftete Firewall als ein Dienst mit integrierter Hochverfügbarkeit und uneingeschränkter Cloudskalierbarkeit. Sie können Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und virtuelle Netzwerke zentral erstellen, erzwingen und protokollieren. Azure Firewall ist zurzeit als öffentliche Vorschauversion verfügbar.

### <a name="what-capabilities-are-supported-in-the-azure-firewall-public-preview-release"></a>Welche Funktionen werden in der öffentlichen Vorschauversion von Azure Firewall unterstützt?  

* Zustandsbehaftete Firewall als Dienst
* Integrierte Hochverfügbarkeit mit uneingeschränkter Cloudskalierbarkeit
* FQDN-Filterung 
* Filterregeln für den Netzwerkdatenverkehr
* SNAT-Unterstützung für ausgehenden Datenverkehr
* Zentrale Erstellung, Erzwingung und Protokollierung der Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und VNETs
* Vollständige Integration in Azure Monitor für Protokollierung und Analysen 

### <a name="how-can-i-join-the-azure-firewall-public-preview"></a>Wie kann ich der öffentlichen Vorschauversion von Azure Firewall beitreten?

Azure Firewall ist derzeit eine verwaltete öffentliche Vorschauversion, der Sie mit dem Befehl „Register-AzureRmProviderFeature PowerShell“ beitreten können, wie in der Dokumentation zur öffentlichen Vorschauversion von Azure Firewall beschrieben.

### <a name="what-is-the-pricing-for-azure-firewall"></a>Wie sieht das Preismodell für Azure Firewall aus?

Azure Firewall setzt sich aus festen und variablen Gebühren zusammen. Die Preise sind unten aufgeführt, und im Rahmen der öffentlichen Vorschauversion erhalten Sie Nachlass von 50 %.

* Feste Gebühr: 1,25 USD/Firewall/Stunde
* Variable Gebühr: 0,03 USD/von der Firewall verarbeiteten GB (ein- oder ausgehend).

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Was ist das typische Bereitstellungsmodell für Azure Firewall?

Es ist zwar möglich, Azure Firewall in jedem VNET einzusetzen, aber Kunden stellen Azure Firewall typischerweise in einem zentralen VNET bereit und verbinden andere VNETs anschließend mithilfe eines Hub-Spoke-Modells. Sie können die Standardroute von den verbundenen VNETs dann so einstellen, dass sie auf dieses zentrale Firewall-VNET zeigt.

### <a name="how-can-i-install-the-azure-firewall"></a>Wie kann ich Azure Firewall installieren?

Azure Firewall kann über das Azure-Portal, PowerShell, die REST-API oder Vorlagen eingerichtet werden. Schrittanleitungen finden Sie im [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal](../articles/firewall/tutorial-firewall-deploy-portal.md).

### <a name="what-are-some-azure-firewall-concepts"></a>Wie lauten einige der Azure Firewall-Konzepte?

Azure Firewall unterstützt-Regeln und Regelsammlungen. Eine Regelsammlung ist eine Liste von Regeln mit gleicher Reihenfolge und Priorität. Regelsammlungen werden in der Reihenfolge ihrer Priorität ausgeführt, Netzwerkregelsammlungen haben höhere Priorität als Anwendungsregelsammlungen, alle Regeln werden beendet.
Es gibt zwei Arten von Regelsammlungen:

* Anwendungsregeln: Ermöglichen das Konfigurieren vollqualifizierter Domänennamen (Fully Qualified Domain Names, FQDNs), auf die von einem Subnetz aus zugegriffen werden kann. 
* Netzwerkregeln: Ermöglichen das Konfigurieren von Regeln mit Quelladresse, Protokoll, Zielport und Zieladresse. 

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Unterstützt Azure Firewall das Filtern des eingehenden Datenverkehrs?

Die öffentliche Vorschauversion von Azure Firewall unterstützt nur das Filtern des ausgehenden Datenverkehrs. Der Schutz des eingehenden Datenverkehrs für HTTP/S-fremde Protokolle (z.B. RDP, SSH, FTP) ist vorläufig für Azure Firewall GA geplant.  
 
### <a name="what-logginganalytics-is-supported-by-the-azure-firewall"></a>Welche Protokolle/Analysen werden von Azure Firewall unterstützt?

Azure Firewall ist zum Anzeigen und Analysieren von Firewallprotokollen in Azure Monitor integriert. Protokolle können an Log Analytics, Azure Storage oder Event Hub gesendet werden. Sie können in Log Analytics oder von anderen Tools, wie Excel und Power BI, analysiert werden. Weitere Details finden Sie in [Tutorial: Überwachen von Azure Firewall-Protokollen](../articles/firewall/tutorial-diagnostics.md).

### <a name="how-does-azure-firewall-work-relative-to-existing-like-nvas-in-the-marketplace"></a>Wie funktioniert Azure Firewall im Vergleich zu vorhandenen ähnlichen NVAs am Markt?

Azure Firewall ist ein grundlegender Firewalldienst für verschiedene Kundenszenarios. Es wird erwartet, dass Sie über eine Mischung aus Drittanbieter-NVAs und Azure Firewall verfügen. Eine bessere Zusammenarbeit hat höchste Priorität.
 
### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Was ist der Unterschied zwischen Application Gateway-WAF und Azure Firewall?

Die Web Application Firewall (WAF) ist ein Feature von Application Gateway, das zentralisierten Schutz des eingehenden Datenverkehrs für Ihre Webanwendungen vor allgemeinen Exploits und Sicherheitsrisiken bietet. Azure Firewall bietet Schutz auf ausgehender Netzwerkebene für alle Ports und Protokolle und Schutz auf Anwendungsebene für ausgehende HTTP/S-Protokolle. Der Schutz des eingehenden Datenverkehrs für HTTP/S-fremde Protokolle (z.B. RDP, SSH, FTP) ist vorläufig für Azure Firewall GA geplant.

### <a name="what-is-the-difference-between-network-security-groups-nsg-and-azure-firewall"></a>Was ist der Unterschied zwischen Netzwerksicherheitsgruppen (NSG) und Azure Firewall?

Der Azure Firewall-Dienst ergänzt die Funktionalität von Netzwerksicherheitsgruppen und bietet zusammen mit diesen eine bessere und tief greifende Netzwerksicherheit. NSGs bieten verteilte Datenverkehrsfilterung auf Netzwerkebene, um den Datenverkehr auf Ressourcen in virtuellen Netzwerken in jedem Abonnement zu beschränken.  Azure Firewall ist eine vollständig statusbehaftete, zentrale Netzwerkfirewall als Dienst, die Schutz auf Netzwerk- und Anwendungsebene für verschiedene Abonnements und virtuelle Netzwerke (VNets) bietet. 

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Wie richte ich Azure Firewall für meine Dienstendpunkte ein?

Für den sicheren Zugriff auf PaaS-Dienste empfehlen wir die Dienstendpunkte. Kunden von Azure Firewall können die Dienstendpunkte im Azure Firewall-Subnetz aktivieren und auf den verbundenen Spoke-VNETs deaktivieren, um von beiden Features zu profitieren – der Sicherheit der Dienstendpunkte und der zentralen Protokollierung des gesamten Datenverkehrs.

### <a name="what-are-the-known-service-limits"></a>Welche Dienstgrenzwerte sind bekannt?

* Azure Firewall verfügt über einen weichen Grenzwert von 1.000 TB/Firewall/Monat. 
* Azure Firewall, die in einem zentralen VNET läuft, unterliegt VNET-Peeringbeschränkungen: max. 50 Spoke-VNETs.  
* Azure Firewall arbeitet nicht mit globalem Peering. Daher sollten Sie mindestens eine Firewall pro Region bereitstellen.
* Azure Firewall unterstützt 10-k-Anwendungsregeln und 10-k-Netzwerkregeln.