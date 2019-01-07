---
title: Verwalten von Sicherheitsempfehlungen in Azure Security Center | Microsoft Docs
description: In diesem Dokument erfahren Sie, wie Sicherheitsempfehlungen in Azure Security Center Ihnen helfen, Ihre Azure-Ressourcen zu schützen und Ihre Sicherheitsrichtlinien einzuhalten.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2018
ms.author: rkarlin
ms.openlocfilehash: d0c61f6e905ca109f3f178996a08f353c36e7880
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53337205"
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Verwalten von Sicherheitsempfehlungen in Azure Security Center
In diesem Dokument erfahren Sie, wie Sie Sicherheitsempfehlungen in Azure Security Center verwenden, um Ihre Azure-Ressourcen zu schützen.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Dieses Dokument ist keine Schritt-für-Schritt-Anleitung.
>
>

## <a name="what-are-security-recommendations"></a>Was sind Sicherheitsempfehlungen?
Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen. Wenn Security Center potenzielle Sicherheitsrisiken identifiziert, werden Empfehlungen erstellt. Entsprechend den Empfehlungen werden Sie durch den Prozess der Konfiguration des erforderlichen Sicherheitsmechanismus geführt.

## <a name="implementing-security-recommendations"></a>Implementieren von Sicherheitsempfehlungen
### <a name="set-recommendations"></a>Festlegen von Empfehlungen
Unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](tutorial-security-policy.md)wird Folgendes beschrieben:

* Konfigurieren von Sicherheitsrichtlinien
* Aktivieren der Datensammlung
* Auswählen der Empfehlungen, die Sie als Teil Ihrer Sicherheitsrichtlinie verwenden möchten

Aktuelle Richtlinienempfehlungen beziehen sich auf Systemupdates, Grundregeln, Antischadsoftware, [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md) für Subnetze und Netzwerkschnittstellen, Überwachung der SQL-Datenbank, Transparent Data Encryption für die SQL-Datenbank und Web Application Firewalls.  [Einrichten von Sicherheitsrichtlinien](tutorial-security-policy.md) enthält eine Beschreibung der einzelnen Empfehlungsoptionen.

### <a name="monitor-recommendations"></a>Überwachen von Empfehlungen
Nach Einstellung einer Sicherheitsrichtlinie analysiert Security Center den Sicherheitsstatus Ihrer Ressourcen, um potenzielle Sicherheitsrisiken zu erkennen. Auf der Kachel **Empfehlungen** unter **Übersicht** können Sie die Gesamtzahl der von Security Center identifizierten Empfehlungen sehen.

![Kachel „Empfehlungen“][1]

Um die Details zu jeder Empfehlung anzuzeigen, wählen Sie unter **Übersicht** die Kachel **Empfehlungen**. Das Blatt **Empfehlungen** wird geöffnet.

![Empfehlungen filtern][2]

Sie können Empfehlungen filtern. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Filter**, um die Empfehlungen zu filtern. Das Blatt **Filter** wird geöffnet. Sie können Werte für Schweregrad und Status auswählen, die Sie anzeigen möchten.

Die Empfehlungen werden in einem Tabellenformat angezeigt, wobei jede Zeile eine bestimmte Empfehlung darstellt. Die Spalten dieser Tabelle sind:

* **BESCHREIBUNG**: Eine Erläuterung der Empfehlung und der erforderlichen Schritte.
* **RESSOURCE**: Eine Liste mit den Ressourcen, für die diese Empfehlung gilt.
* **STATUS**: Beschreibt den aktuellen Status der Empfehlung:
  * **Offen**: Die Empfehlung wurde noch nicht bearbeitet.
  * **In Bearbeitung**: Die Empfehlung wird derzeit auf die Ressourcen angewendet, und es ist keine Aktion Ihrerseits erforderlich.
  * **Behoben**: Die Empfehlung wurde bereits abgeschlossen (in diesem Fall ist die Zeile grau hinterlegt).
* **SCHWEREGRAD**: Beschreibt den Schweregrad der jeweiligen Empfehlung:
  * **Hoch:** Ein Sicherheitsrisiko betrifft eine wichtige Ressource (z. B. eine Anwendung, eine VM oder eine Netzwerksicherheitsgruppe) und erfordert einen Eingriff.
  * **Mittel:** Es besteht ein Sicherheitsrisiko, und es sind nicht kritische oder zusätzliche Schritte erforderlich, um es zu beseitigen oder einen Prozess abzuschließen.
  * **Niedrig**: Es besteht ein Sicherheitsrisiko, das behandelt werden sollte, aber keinen unmittelbaren Eingriff erfordert. (Standardmäßig werden Empfehlungen mit dem Status „Niedrig“ nicht angezeigt, aber Sie können bei Bedarf nach diesen Empfehlungen filtern.)

Der folgenden Tabelle können Sie entnehmen, welche Empfehlungen verfügbar sind und welche Aktionen bei ihrer Anwendung jeweils ausgeführt werden.

> [!NOTE]
> Es empfiehlt sich, sich mit dem [klassischen Bereitstellungsmodell und dem Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md) für Azure-Ressourcen vertraut zu machen.
>
>

| Empfehlung | BESCHREIBUNG |
| --- | --- |
| [Sammlung von Daten für Abonnements aktivieren](security-center-enable-data-collection.md) |Empfiehlt Ihnen, in der Sicherheitsrichtlinie die Datensammlung für alle Abonnements sowie alle virtuellen Azure-Computer (VMs) und Computer, die keine Azure-Computer sind, zu aktivieren. |
| [Optimieren von Sicherheitskonfigurationen](security-center-remediate-os-vulnerabilities.md) |Empfiehlt Ihnen, Ihre Betriebssystemkonfigurationen mit den empfohlenen Sicherheitskonfigurationsregeln in Einklang zu bringen und beispielsweise nicht zuzulassen, dass Kennwörter gespeichert werden. |
| [Systemupdates anwenden](security-center-apply-system-updates.md) |Empfiehlt Ihnen, fehlende Systemsicherheitsupdates und kritische Updates für Windows- und Linux-VMs und -Computer bereitzustellen. |
| [JIT-Netzwerkzugriffssteuerung anwenden](security-center-just-in-time.md) | Empfiehlt Ihnen, Just-In-Time-VM-Zugriff anzuwenden. Das Just-In-Time-Feature ist im Standard-Tarif von Security Center verfügbar. Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md). |
| [Neustart nach Systemupdates](security-center-apply-system-updates.md#reboot-after-system-updates) |Empfiehlt Ihnen, einen virtuellen Computer neu zu starten, um die Anwendung von Systemupdates abzuschließen. |
| [Add a web application firewall](security-center-add-web-application-firewall.md) |Empfiehlt Ihnen, eine Web Application Firewall (WAF) für Webendpunkte bereitzustellen. Eine WAF-Empfehlung wird für jede öffentlich zugängliche IP-Adresse (sowohl auf Instanzebene als auch mit Lastenausgleich) angezeigt, die über eine zugeordnete Netzwerksicherheitsgruppe mit offenen eingehenden Webports (80, 443) verfügt. </br>Security Center empfiehlt die Bereitstellung einer WAF zum Schutz vor Angriffen auf Ihre Webanwendungen auf virtuellen Computern und in der App Service-Umgebung. Eine App Service-Umgebung ist eine Option des [Premium](https://azure.microsoft.com/pricing/details/app-service/)-Tarifs von Azure App Service, die eine vollständig isolierte und dedizierte Umgebung zur sicheren Ausführung von Azure App Service-Apps bereitstellt. Weitere Informationen zu ASE finden Sie unter [Dokumentation zur App Service-Umgebung](../app-service/environment/intro.md).</br>Sie können mehrere Webanwendungen in Security Center schützen, indem Sie diese Anwendungen Ihren vorhandenen WAF-Bereitstellungen hinzufügen. |
| [Finalize application protection (Anwendungsschutz abschließen) (Anwendungsschutz abschließen)](security-center-add-web-application-firewall.md#finalize-application-protection) |Um die Konfiguration einer WAF abzuschließen, muss Datenverkehr an das WAF-Gerät umgeleitet werden. Nach dieser Empfehlung werden die erforderlichen Setupänderungen vorgenommen. |
| [Firewall der nächsten Generation hinzufügen](security-center-add-next-generation-firewall.md) |Empfiehlt, dass Sie zum Verbessern Ihrer Sicherheitsmaßnahmen eine Firewall der nächsten Generation (NGFW) eines Microsoft-Partners hinzufügen |
| [Datenverkehr nur durch Firewall der nächsten Generation leiten](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Empfiehlt, dass Sie die Regeln der Netzwerksicherheitsgruppe so konfigurieren, dass zu Ihrer VM eingehender Datenverkehr durch Ihre Firewall der nächsten Generation geleitet werden muss. |
| [Endpoint Protection installieren](security-center-install-endpoint-protection.md) |Empfiehlt Ihnen, Antischadsoftware für virtuelle Maschinen bereitzustellen (nur Windows-VMs). |
| [Netzwerksicherheitsgruppen in Subnetzen oder auf virtuellen Computern aktivieren](security-center-enable-network-security-groups.md) |Empfiehlt, dass Sie NSGs in Subnetzen oder auf virtuellen Computern aktivieren. |
| [Zugriff über Endpunkt mit Internetzugriff einschränken](security-center-restrict-access-through-internet-facing-endpoints.md) |Empfiehlt Ihnen, Regeln für eingehenden Datenverkehr für NSGs zu konfigurieren. |
| [Aktivieren der Überwachung und Bedrohungserkennung auf SQL-Servern](security-center-enable-auditing-on-sql-servers.md) |Empfiehlt, die Überwachung und Bedrohungserkennung für Azure SQL-Server zu aktivieren. (Nur Azure SQL-Dienst. Schließt nicht die Ausführung von SQL auf Ihren virtuellen Computern mit ein.) |
| [Aktivieren der Überwachung und Bedrohungserkennung in SQL-Datenbanken](security-center-enable-auditing-on-sql-databases.md) |Empfiehlt, die Überwachung und Bedrohungserkennung für Azure SQL-Datenbanken zu aktivieren. (Nur Azure SQL-Dienst. Schließt nicht die Ausführung von SQL auf Ihren virtuellen Computern mit ein.) |
| [Transparent Data Encryption für SQL-Datenbanken aktivieren](security-center-enable-transparent-data-encryption.md) |Empfiehlt die Verschlüsselung für SQL-Datenbanken zu aktivieren. (Nur Azure SQL-Dienst.) |
| [VM-Agent aktivieren](security-center-enable-vm-agent.md) |Ermöglicht Ihnen, anzuzeigen, welche VMs den VM-Agent benötigen. Der VM-Agent muss auf virtuellen Computern installiert werden, um die Überprüfung von Patches, die Überprüfung von Grundregeln und Antischadsoftware bereitzustellen. Der VM-Agent wird standardmäßig für virtuelle Maschinen installiert, die vom Azure Marketplace bereitgestellt werden. Der Artikel [VM-Agent und Erweiterungen – Teil 2](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) enthält Informationen zum Installieren des VM-Agents. |
| [Datenträgerverschlüsselung anwenden](security-center-apply-disk-encryption.md) |Empfiehlt Ihnen, dass Sie Ihre VM-Datenträger per Azure Disk Encryption (Windows- und Linux-VMs) verschlüsseln. Die Verschlüsselung wird sowohl für die Betriebssystem- als auch für die Datenvolumes auf Ihrer VM empfohlen. |
| [Sicherheitskontaktinformationen bereitstellen](security-center-provide-security-contact-details.md) |Empfiehlt, dass Sie Sicherheitskontaktinformationen für all Ihre Abonnements bereitstellen. Die Kontaktinformationen bestehen aus einer E-Mail-Adresse und einer Telefonnummer. Die Informationen werden verwendet, um mit Ihnen Kontakt aufzunehmen, sobald unser Sicherheitsteam feststellt, dass Ihre Ressourcen kompromittiert wurden. |
| [Betriebssystemversion aktualisieren](security-center-update-os-version.md) |Die Empfehlung lautet, dass Sie die Betriebssystemversion für den Clouddienst auf die aktuelle Version aktualisieren sollten, die für Ihre Betriebssystemfamilie verfügbar ist.  Weitere Informationen zu Cloud Services finden Sie unter [Übersicht über Cloud Services](../cloud-services/cloud-services-choose-me.md). |
| [Sicherheitsrisikobewertung nicht installiert](security-center-vulnerability-assessment-recommendations.md) |Empfiehlt die Installation einer Lösung zur Sicherheitsrisikobewertung auf dem virtuellen Computer. |
| [Sicherheitsrisiken beheben](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Ermöglicht das Anzeigen von System- und Anwendungssicherheitslücken, die von der auf dem virtuellen Computer installierten Lösung zur Sicherheitsrisikobewertung erkannt werden. |
| [Aktivieren der Verschlüsselung für das Azure Storage-Konten](security-center-enable-encryption-for-storage-account.md) | Empfiehlt die Aktivierung von Azure Storage Service Encryption für ruhende Daten. Storage Service Encryption (SSE) verschlüsselt die Daten, wenn diese in Azure Storage geschrieben werden, und entschlüsselt sie vor dem Abrufen. SSE steht zurzeit nur für den Azure Blob-Dienst zur Verfügung und kann für Blockblobs, Seitenblobs und Anfügeblobs verwendet werden. Weitere Informationen finden Sie unter [Storage Service Encryption für ruhende Daten](../storage/common/storage-service-encryption.md).</br>SSE wird nur für Resource Manager-Speicherkonten unterstützt. |
| [Adaptive Anwendungssteuerungen aktivieren](security-center-adaptive-application.md) | Empfiehlt, adaptive Anwendungssteuerungen auf Ihre Windows-VMs anzuwenden. Dieses Feature ist im Standard-Tarif von Security Center verfügbar. Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md). |
| Zugriff auf App Service nur über HTTPS gestatten | Empfiehlt, den Zugriff auf App Service nur auf HTTPS zu beschränken. |
| WebSockets für Webanwendung deaktivieren| Empfiehlt, die Verwendung von WebSockets innerhalb von Webanwendungen sorgfältig zu prüfen.  Das WebSockets-Protokoll ist für verschiedene Arten von Sicherheitsrisiken anfällig. |
| Benutzerdefinierte Domänen für Webanwendung verwenden | Empfiehlt, benutzerdefinierte Domänen zu verwenden, damit eine Webanwendung vor gängigen Angriffen wie Phishing und anderen DNS-bezogenen Angriffen geschützt wird. |
| IP-Einschränkungen für Webanwendung konfigurieren | Empfiehlt, eine Liste mit IP-Adressen zu definieren, die auf Ihre Anwendung zugreifen dürfen.  Mithilfe von IP-Einschränkungen kann eine Webanwendung vor gängigen Angriffen geschützt werden. |
| Nicht allen („*“) Ressourcen Zugriff auf Ihre Anwendung erteilen | Rät davon ab, den WEBSITE_LOAD_CERTIFICATES-Parameter auf „*“ festzulegen. Durch die Einstellung des Parameters auf „*“ werden alle Zertifikate in den persönlichen Zertifikatspeicher Ihrer Webanwendungen geladen.  Dies kann zum Missbrauch des Prinzips der „geringsten Rechte“ führen, da es unwahrscheinlich ist, dass die Website Zugriff auf sämtliche Zertifikate zur Laufzeit benötigt. |
| Nicht jeder Ressource den Zugriff auf Ihre Anwendung über CORS gestatten | Empfiehlt, nur erforderlichen Domänen die Interaktion mit Ihrer Webanwendung zu ermöglichen. Bei der Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) sollte nicht allen Domänen gestattet werden, auf Ihre Webanwendung zuzugreifen. |
| Die neueste unterstützte .NET Framework-Version für Webanwendungen verwenden | Empfiehlt, die neueste .NET Framework-Version für die neuesten Sicherheitsklassen zu verwenden. Die Verwendung von älteren Klassen und Typen kann die Anfälligkeit Ihrer Anwendung für Sicherheitsrisiken erhöhen. |
| Die neueste unterstützte Java-Version für Webanwendungen verwenden | Empfiehlt, die neueste Java-Version für die neuesten Sicherheitsklassen zu verwenden. Die Verwendung von älteren Klassen und Typen kann die Anfälligkeit Ihrer Anwendung für Sicherheitsrisiken erhöhen. |
| Die neueste unterstützte PHP-Version für Webanwendungen verwenden | Empfiehlt, die neueste PHP-Version für die neuesten Sicherheitsklassen zu verwenden. Die Verwendung von älteren Klassen und Typen kann die Anfälligkeit Ihrer Anwendung für Sicherheitsrisiken erhöhen. |
| [Add a web application firewall](security-center-add-web-application-firewall.md) |Empfiehlt Ihnen, eine Web Application Firewall (WAF) für Webendpunkte bereitzustellen. Eine WAF-Empfehlung wird für jede öffentlich zugängliche IP-Adresse (sowohl auf Instanzebene als auch mit Lastenausgleich) angezeigt, die über eine zugeordnete Netzwerksicherheitsgruppe mit offenen eingehenden Webports (80, 443) verfügt.</br></br>Security Center empfiehlt die Bereitstellung einer WAF zum Schutz vor Angriffen auf Ihre Webanwendungen auf virtuellen Computern und in der App Service-Umgebung. Eine App Service-Umgebung ist eine Option des [Premium](https://azure.microsoft.com/pricing/details/app-service/)-Tarifs von Azure App Service, die eine vollständig isolierte und dedizierte Umgebung zur sicheren Ausführung von Azure App Service-Apps bereitstellt. Weitere Informationen zu ASE finden Sie unter [Dokumentation zur App Service-Umgebung](../app-service/environment/intro.md).</br></br>Sie können mehrere Webanwendungen in Security Center schützen, indem Sie diese Anwendungen Ihren vorhandenen WAF-Bereitstellungen hinzufügen. |
| [Finalize application protection (Anwendungsschutz abschließen) (Anwendungsschutz abschließen)](security-center-add-web-application-firewall.md#finalize-application-protection) |Um die Konfiguration einer WAF abzuschließen, muss Datenverkehr an das WAF-Gerät umgeleitet werden. Nach dieser Empfehlung werden die erforderlichen Setupänderungen vorgenommen. |
| Die neueste unterstützte Node.js-Version für Webanwendungen verwenden | Empfiehlt, die neueste Node.js-Version für die neuesten Sicherheitsklassen zu verwenden. Die Verwendung von älteren Klassen und Typen kann die Anfälligkeit Ihrer Anwendung für Sicherheitsrisiken erhöhen. |
| Nicht jeder Ressource den Zugriff auf Ihre Funktions-App über CORS gestatten | Empfiehlt, nur erforderlichen Domänen die Interaktion mit Ihrer Webanwendung zu ermöglichen. Bei der Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) sollte nicht allen Domänen gestattet werden, auf Ihre Funktionsanwendung zuzugreifen. |
| Benutzerdefinierte Domänen für Funktions-App verwenden | Empfiehlt, benutzerdefinierte Domänen zu verwenden, damit eine Funktions-App vor gängigen Angriffen wie Phishing und anderen DNS-bezogenen Angriffen geschützt wird. |
| IP-Einschränkungen für Funktions-App konfigurieren | Empfiehlt, eine Liste mit IP-Adressen zu definieren, die auf Ihre Anwendung zugreifen dürfen. Mithilfe von IP-Einschränkungen kann eine Funktions-App vor gängigen Angriffen geschützt werden. |
| Zugriff auf Funktions-App nur über HTTPS gestatten | Empfiehlt, den Zugriff auf Funktions-Apps nur über HTTPS zu beschränken. |
| Remotedebuggen sollte für Funktions-Apps deaktiviert werden | Empfiehlt, das Debuggen für Funktions-Apps zu deaktivieren, wenn Sie sie nicht mehr benötigen. Für das Remotedebuggen müssen die eingehenden Ports für eine Funktions-App geöffnet sein. |
| WebSockets für Funktions-App deaktivieren | Empfiehlt, die Verwendung von Websockets innerhalb von Funktions-Apps sorgfältig zu prüfen. Das WebSockets-Protokoll ist für verschiedene Arten von Sicherheitsrisiken anfällig. |
| Mehrere Besitzer für Ihr Abonnement festlegen | Empfiehlt, mehrere Abonnementbesitzer festzulegen, um Redundanz beim Administratorzugriff zu gewährleisten. |
| Bis zu drei Besitzer für Ihr Abonnement festlegen | Empfiehlt, höchstens drei Abonnementbesitzer festzulegen, um die Möglichkeit einer Sicherheitsverletzung durch einen kompromittierten Besitzer zu verringern. |
| MFA für Konten mit Besitzerberechtigungen in Ihrem Abonnement aktivieren | Empfiehlt, für alle Abonnementkonten mit Administratorrechten die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) zu aktivieren, um eine Sicherheitsverletzung von Konten oder Ressourcen zu verhindern. |
| MFA für Konten mit Schreibberechtigungen in Ihrem Abonnement aktivieren | Empfiehlt, für alle Abonnementkonten mit Schreibberechtigungen die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) zu aktivieren, um eine Sicherheitsverletzung von Konten oder Ressourcen zu verhindern. |
| MFA für Konten mit Leseberechtigungen in Ihrem Abonnement aktivieren | Empfiehlt, für alle Abonnementkonten mit Leseberechtigungen die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) zu aktivieren, um eine Sicherheitsverletzung von Konten oder Ressourcen zu verhindern. |
| Externe Konten mit Leseberechtigungen aus Ihrem Abonnement entfernen | Empfiehlt, externe Konten mit Leseberechtigungen aus Ihrem Abonnement zu entfernen, um nicht überwachten Zugriff zu verhindern. |
| Externe Konten mit Schreibberechtigungen aus Ihrem Abonnement entfernen | Empfiehlt, externe Konten mit Schreibberechtigungen aus Ihrem Abonnement zu entfernen, um nicht überwachten Zugriff zu verhindern. |
| Externe Konten mit Besitzerberechtigungen aus Ihrem Abonnement entfernen | Empfiehlt, externe Konten mit Besitzerberechtigungen aus Ihrem Abonnement zu entfernen, um nicht überwachten Zugriff zu verhindern. |
| Veraltete Konten aus dem Abonnement entfernen | Empfiehlt, veraltete Konten aus Ihren Abonnements zu entfernen. |
| Veraltete Konten mit Besitzerberechtigungen aus dem Abonnement entfernen | Empfiehlt, veraltete Konten mit Besitzerberechtigungen aus Ihren Abonnements entfernen. |

### <a name="apply-recommendations"></a>Anwenden von Empfehlungen
Nach Auswertung aller Empfehlungen entscheiden Sie, welche zuerst angewendet werden soll. Es ist ratsam, den Schweregrad als wichtigsten Parameter bei der Entscheidung heranzuziehen, welche Empfehlungen zuerst angewendet werden sollen.

Wählen Sie in der obigen Tabelle der Empfehlungen eine Empfehlung aus, und durchlaufen Sie das entsprechende Beispiel, um sie umzusetzen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurden Ihnen die Sicherheitsempfehlungen in Security Center vorgestellt. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
