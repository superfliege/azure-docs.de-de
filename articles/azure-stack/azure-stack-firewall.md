---
title: Azure Stack-Firewallplanung für integrierte Azure Stack-Systeme | Microsoft-Dokumentation
description: Informationen zu Überlegungen der Azure Stack-Firewall für mit Azure verbundene Bereitstellungen von Azure Stack-Systemen mit mehreren Knoten.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 3759a9845d4ad1514fc5f0183c78b5eca2e31464
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960650"
---
# <a name="azure-stack-firewall-integration"></a>Azure Stack-Firewallintegration
Es empfiehlt sich, Azure Stack mit einem Firewallgerät zu schützen. Firewalls können zwar bei verteilten Denial-of-Service-Angriffen (DDoS), bei der Angriffserkennung und bei der Inhaltsuntersuchung hilfreich sein, andererseits können sie sich aber auch als Engpass für den Durchsatz von Azure-Speicherdiensten wie Blobs, Tabellen und Warteschlangen erweisen.

Je nach Identitätsmodell von Azure Active Directory (Azure AD) oder Windows Server Active Directory Federation Services (AD FS) müssen Sie möglicherweise den AD FS-Endpunkt veröffentlichen. Bei Verwendung eines getrennten Bereitstellungsmodus muss der AD FS-Endpunkt veröffentlicht werden. Weitere Informationen finden Sie im Artikel zur [Rechenzentrumsintegration mithilfe von Identitäten](azure-stack-integrate-identity.md).

Für die Endpunkte „Azure Resource Manager (Administrator)“, „Portal (Administrator)“ und „Schlüsseltresor (Administrator)“ ist nicht zwingend eine externe Veröffentlichung erforderlich. Als Dienstanbieter können Sie beispielsweise die Angriffsfläche verkleinern und Azure Stack nur innerhalb Ihres Netzwerks (und nicht über das Internet) verwalten.

Bei einem Unternehmen kann das Unternehmensnetzwerk als externes Netzwerk fungieren. In einem solchen Szenario müssen Sie die Endpunkte veröffentlichen, um Azure Stack über das Unternehmensnetzwerk bedienen zu können.

### <a name="network-address-translation"></a>Netzwerkadressenübersetzung
Die Netzwerkadressenübersetzung (Network Address Translation, NAT) stellt die empfohlene Methode dar, um virtuellen Bereitstellungscomputern (Deployment Virtual Machine, DVM) bei der Bereitstellung sowie ERCS-VMs (Emergency Recovery Console) oder privilegierten Endpunkten (PEP) bei der Registrierung und Problembehandlung Zugriff auf externe Ressourcen und das Internet zu ermöglichen.

Die NAT kann auch alternativ zu öffentlichen IP-Adressen im externen Netzwerk oder öffentlichen VIPs verwendet werden. Hiervon wird jedoch abgeraten, da diese die Benutzerfreundlichkeit des Mandanten verringert und die Komplexität erhöht. Die beiden Optionen wären eine 1:1-NAT, die noch eine öffentliche IP pro Benutzer-IP für den Pool oder viele andere erfordert: 1 NAT, die eine NAT-Regel pro Benutzer-VIP mit Zuordnungen für alle Ports erfordert, die ein Benutzer verwenden könnte.

Die Verwendung der NAT für öffentliche VIPs hat folgende Nachteile:
- Durch die NAT erhöht sich der Verwaltungsaufwand für Firewallregeln, da Benutzer ihre eigenen Endpunkte und ihre eigenen Veröffentlichungsregeln im SDN-Stapel (Software Defined Networking) steuern. Benutzer müssen sich zur Veröffentlichung ihrer VIPs sowie zur Aktualisierung der Portliste an den Azure Stack-Betreiber wenden.
- Die Verwendung der NAT beeinträchtigt zwar die Benutzerfreundlichkeit, gibt dem Betreiber jedoch die volle Kontrolle über Veröffentlichungsanforderungen.
- In Hybrid Cloud-Szenarien mit Azure wird die Einrichtung eines VPN-Tunnels zu einem Endpunkt mit NAT nicht unterstützt.

### <a name="ssl-decryption"></a>SSL-Entschlüsselung
Derzeit wird empfohlen, die SSL-Entschlüsselung bei allem Azure Stack-Datenverkehr zu deaktivieren. Wenn sie in zukünftigen Updates unterstützt wird, werden Anleitungen zum Aktivieren der SSL-Entschlüsselung für Azure Stack bereitgestellt.

## <a name="edge-firewall-scenario"></a>Szenario mit Edgefirewall
Azure Stack wird in einer Edgebereitstellung direkt hinter dem Edgerouter oder der Firewall bereitgestellt. In diesen Szenarien kann die Firewall dem Border-Gerät übergeordnet sein (Szenario 1) und sowohl Aktiv/Aktiv- als auch Aktiv/Passiv-Firewallkonfigurationen unterstützen oder als Border-Gerät fungieren (Szenario 2) und nur eine Aktiv/Aktiv-Firewallkonfiguration unterstützen, wobei ECMP (Equal Cost Multi Path) mit BGP oder statischem Routing für Failover erforderlich ist.

In der Regel werden in einer Edgebereitstellung zum Zeitpunkt der Bereitstellung über das externe Netzwerk routingfähige IP-Adressen für den öffentlichen VIP-Pool angegeben. In einem Edgeszenario wird aus Sicherheitsgründen davon abgeraten, öffentliche routingfähige IP-Adressen in anderen Netzwerken zu verwenden. Dadurch erhalten Benutzer eine vollständig selbstgesteuerte Cloudumgebung – genau wie bei einer öffentlichen Cloud (z.B. Azure).  

![Beispiel für eine Azure Stack-Edgefirewall](./media/azure-stack-firewall/firewallScenarios.png)

## <a name="enterprise-intranet-or-perimeter-network-firewall-scenario"></a>Firewallszenario mit Unternehmensintranet oder -umkreisnetzwerk
Bei einer Bereitstellung im Unternehmensintranet oder -umkreisnetzwerk wird Azure Stack in einer Firewall mit mehreren Zonen oder zwischen der Edgefirewall und der internen Unternehmensnetzwerkfirewall bereitgestellt. Dann wird wie im Folgenden beschrieben der Datenverkehr zwischen dem sicheren Umkreisnetzwerk (oder der DMZ) und unsicheren Zonen verteilt:

- **Sichere Zone**: Dies ist das interne Netzwerk, das interne oder routingfähige Unternehmens-IP-Adressen verwendet. Das sichere Netzwerk kann unterteilt werden, über ausgehenden Internetzugriff über die NAT in der Firewall verfügen und in der Regel von einem beliebige Ort aus in Ihrem Rechenzentrum über das interne Netzwerk aufgerufen werden. Alle Azure Stack-Netzwerke sollten sich in der sicheren Zone befinden, ausgenommen dem öffentlichen VIP-Pool im externen Netzwerk.
- **Umkreiszone**: Das Umkreisnetzwerk wird dort eingesetzt, wo in der Regel externe oder Internetanwendungen wie Webserver bereitgestellt werden. Es wird normalerweise durch eine Firewall überwacht, um Angriffe wie DDoS-Angriffe und Eindringversuche (Hacken) zu verhindern, lässt jedoch gleichzeitig angegebenen eingehenden Datenverkehr aus dem Internet zu. In der DMZ-Zone sollte sich nur der öffentliche VIP-Pool des externen Netzwerks von Azure Stack befinden.
- **Unsichere Zone**: Dies ist das externe Netzwerk, das Internet. Es wird davon **abgeraten**, Azure Stack in der unsicheren Zone bereitzustellen.

![Beispiel für eine Azure Stack-Bereitstellung in einem Umkreisnetzwerk](./media/azure-stack-firewall/perimeter-network-scenario.png)

## <a name="learn-more"></a>Weitere Informationen
Erfahren Sie mehr über [die von Azure Stack-Endpunkten verwendeten Ports und Protokolle](azure-stack-integrate-endpoints.md).

## <a name="next-steps"></a>Nächste Schritte
[PKI-Anforderungen für Azure Stack](azure-stack-pki-certs.md)

