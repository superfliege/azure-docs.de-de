---
title: In Azure Security Center überwachte Azure Policy-Definitionen | Microsoft-Dokumentation
description: In Azure Security Center überwachte Azure Policy-Definitionen
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: c89cb1aa-74e8-4ed1-980a-02a7a25c1a2f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/15/2019
ms.author: rkarlin
ms.openlocfilehash: 9d9369afd36f64c27cd2222cab0de5912aa913de
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57877495"
---
# <a name="azure-security-policies-monitored-by-security-center"></a>In Security Center überwachte Azure-Sicherheitsrichtlinien
Dieser Artikel enthält eine Liste mit Azure Policy-Definitionen, die Sie in Azure Security Center überwachen können. Weitere Informationen zu Sicherheitsrichtlinien finden Sie unter [Arbeiten mit Sicherheitsrichtlinien](tutorial-security-policy.md).

## <a name="available-security-policies"></a>Verfügbare Sicherheitsrichtlinien

Informationen zu den integrierten Richtlinien, die von Security Center überwacht werden, finden Sie in der folgenden Tabelle:

| Richtlinie | Zweck der Richtlinie |
| --- | --- |
|Aktivierung von Diagnoseprotokollen in Azure Service Fabric und VM-Skalierungsgruppen überwachen|Wir empfehlen Ihnen, Protokolle zu aktivieren, damit nach einem Incident oder einer Kompromittierung die Aktivitäten im Rahmen einer Untersuchung nachvollzogen werden können.|
|Autorisierungsregeln für Event Hubs-Namespaces überwachen|Azure Event Hubs-Clients dürfen keine Zugriffsrichtlinie auf Namespace-Ebene verwenden, die Zugriff auf alle Warteschlangen und Themen in einem Namespace gewährt. Um die Anforderungen des Sicherheitsmodells der geringsten Rechte zu erfüllen, müssen Sie Zugriffsrichtlinien auf Entitätsebene erstellen, damit nur der jeweiligen Entität Zugriff auf Warteschlangen und Themen gewährt wird.|
|Vorhandensein von Autorisierungsregeln für Event Hubs-Entitäten überwachen|Hiermit wird das Vorhandensein von Autorisierungsregeln für Event Hubs-Entitäten überwacht, um Zugriff mit den geringsten Rechten zu gewähren.|
|Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen|Überwachen Sie uneingeschränkten Netzwerkzugriff in den Firewalleinstellungen Ihres Speicherkontos. Konfigurieren Sie die Netzwerkregeln so, dass nur Anwendungen aus zulässigen Netzwerken auf ein Speicherkonto zugreifen können. Um Verbindungen von bestimmten Internetclients oder lokalen Clients zuzulassen, können Sie Zugriff für Datenverkehr aus bestimmten virtuellen Azure-Netzwerken oder an IP-Adressbereiche im öffentlichen Internet gewähren.|
|Verwendung benutzerdefinierter RBAC-Regeln überwachen|Hiermit werden integrierte Rollen (z. B. „Benutzer“, „Mitwirkender“ und „Leser“) anstelle von benutzerdefinierten RBAC-Rollen (rollenbasierte Zugriffssteuerung) überwacht, da diese fehleranfällig sind. Die Verwendung benutzerdefinierter Rollen wird als Ausnahme betrachtet und erfordert eine strenge Überprüfung und Bedrohungsmodellierung.|
|Aktivierung von Diagnoseprotokollen in Azure Stream Analytics überwachen|Hierbei wird die Aktivierung von Protokollen überwacht, die dann bis zu einem Jahr lang aufbewahrt werden. Auf diese Weise können im Rahmen von Untersuchungen die Aktivitäten nachvollzogen werden, wenn ein Sicherheitsincident eintritt oder Ihr Netzwerk kompromittiert wird.|
|Sichere Übertragung in Speicherkonten überwachen|Hiermit werden die Anforderungen sicherer Übertragungen in Ihr Speicherkonto überwacht. Sichere Übertragung ist eine Option, die erzwingt, dass Ihr Storage-Konto nur Anforderungen von sicheren Verbindungen (HTTPS) akzeptiert. Durch die Verwendung von HTTPS wird die Authentifizierung zwischen dem Server und dem Dienst sichergestellt. Darüber hinaus werden Daten während der Übertragung vor Angriffen auf Netzwerkebene geschützt, z. B. Man-in-the-Middle-Angriffe, Abhörangriffe und Session Hijacking.|
|Bereitstellung eines Azure Active Directory-Administrators für SQL Server überwachen|Hiermit wird die Bereitstellung eines Azure AD-Administrators (Azure Active Directory) für die SQL Server-Instanz überwacht, um die Azure AD-Authentifizierung zu aktivieren. Die Azure AD-Authentifizierung unterstützt die vereinfachte Verwaltung von Berechtigungen und eine zentralisierte Identitätsverwaltung von Datenbankbenutzern und anderen Microsoft-Diensten.|
|Autorisierungsregeln für Service Bus-Namespaces überwachen|Azure Service Bus-Clients dürfen keine Zugriffsrichtlinie auf Namespace-Ebene verwenden, die Zugriff auf alle Warteschlangen und Themen in einem Namespace bereitstellt. Um die Anforderungen des Sicherheitsmodells der geringsten Rechte zu erfüllen, erstellen Sie Zugriffsrichtlinien auf Entitätsebene, damit nur der jeweiligen Entität Zugriff auf Warteschlangen und Themen gewährt wird.|
|Aktivierung von Diagnoseprotokollen in Service Bus überwachen|Überwachen Sie die Aktivierung von Protokollen, und bewahren Sie sie bis zu einem Jahr lang auf. Auf diese Weise können im Rahmen von Untersuchungen die Aktivitäten nachvollzogen werden, wenn ein Sicherheitsincident eintritt oder Ihr Netzwerk kompromittiert wird.|
|Festlegung der ClusterProtectionLevel-Eigenschaft auf EncryptAndSign in Service Fabric überwachen|Service Fabric bietet drei Schutzebenen für die Kommunikation zwischen zwei Knoten, bei der ein primäres Clusterzertifikat verwendet wird: „None“, „Sign“ und „EncryptAndSign“. Legen Sie die Schutzebene fest, um sicherzustellen, dass alle zwischen zwei Knoten übertragenen Nachrichten verschlüsselt und digital signiert werden.|
|Verwendung von Azure Active Directory für Clientauthentifizierung in Service Fabric überwachen|Hierbei wird die Verwendung der Clientauthentifizierung nur über Azure AD in Service Fabric überwacht.|
|Aktivierung von Diagnoseprotokollen für Search-Dienst überwachen|Hierbei wird die Aktivierung von Protokollen überwacht, die dann bis zu einem Jahr lang aufbewahrt werden. Auf diese Weise können im Rahmen von Untersuchungen die Aktivitäten nachvollzogen werden, wenn ein Sicherheitsincident eintritt oder Ihr Netzwerk kompromittiert wird.|
|Aktivierung von ausschließlich sicheren Verbindungen mit Azure Cache for Redis überwachen|Hierbei wird die ausschließliche Aktivierung von Verbindungen über SSL mit Azure Cache for Redis überwacht. Durch die Verwendung von sicheren Verbindungen wird die Authentifizierung zwischen dem Server und dem Dienst sichergestellt. Darüber hinaus werden Daten während der Übertragung vor Angriffen auf Netzwerkebene geschützt, z. B. Man-in-the-Middle-Angriffe, Abhörangriffe und Session Hijacking.|
|Aktivierung von Diagnoseprotokollen in Logic Apps überwachen|Hierbei wird die Aktivierung von Protokollen überwacht, die dann bis zu einem Jahr lang aufbewahrt werden. Auf diese Weise können im Rahmen von Untersuchungen die Aktivitäten nachvollzogen werden, wenn ein Sicherheitsincident eintritt oder Ihr Netzwerk kompromittiert wird.|
|Aktivierung von Diagnoseprotokollen in Key Vault überwachen|Hierbei wird die Aktivierung von Protokollen überwacht, die dann bis zu einem Jahr lang aufbewahrt werden. Auf diese Weise können im Rahmen von Untersuchungen die Aktivitäten nachvollzogen werden, wenn ein Sicherheitsincident eintritt oder Ihr Netzwerk kompromittiert wird.|
|Aktivierung von Diagnoseprotokollen in Event Hubs überwachen|Hierbei wird die Aktivierung von Protokollen überwacht, die dann bis zu einem Jahr lang aufbewahrt werden. Auf diese Weise können im Rahmen von Untersuchungen die Aktivitäten nachvollzogen werden, wenn ein Sicherheitsincident eintritt oder Ihr Netzwerk kompromittiert wird.|
|Aktivierung von Diagnoseprotokollen in Azure Data Lake Storage überwachen|Hierbei wird die Aktivierung von Protokollen überwacht, die dann bis zu einem Jahr lang aufbewahrt werden. Auf diese Weise können im Rahmen von Untersuchungen die Aktivitäten nachvollzogen werden, wenn ein Sicherheitsincident eintritt oder Ihr Netzwerk kompromittiert wird.|
|Aktivierung von Diagnoseprotokollen in Data Lake Analytics überwachen|Hierbei wird die Aktivierung von Protokollen überwacht, die dann bis zu einem Jahr lang aufbewahrt werden. Auf diese Weise können im Rahmen von Untersuchungen die Aktivitäten nachvollzogen werden, wenn ein Sicherheitsincident eintritt oder Ihr Netzwerk kompromittiert wird.|
|Verwendung von klassischen Speicherkonten überwachen|Verwenden Sie Azure Resource Manager für Ihre Speicherkonten, um Sicherheitsverbesserungen zu erzielen. Das umfasst: <br>- Höhere Sicherheit bei der Zugriffssteuerung (RBAC)<br>- Bessere Überwachung<br>- Auf Azure Resource Manager basierende Bereitstellung und Governance<br>- Zugriff auf verwaltete Identitäten<br>- Zugriff auf Azure Key Vault für Geheimnisse<br>- Azure AD-basierte Authentifizierung<br>- Unterstützung von Tags und Ressourcengruppen zur Vereinfachung der Sicherheitsverwaltung|
|Verwendung klassischer VMs überwachen|Verwenden Sie Azure Resource Manager für Ihre virtuellen Computer, um Sicherheitsverbesserungen zu erzielen.  Das umfasst: <br>- Höhere Sicherheit bei der Zugriffssteuerung (RBAC)<br>- Bessere Überwachung<br>- Auf Azure Resource Manager basierende Bereitstellung und Governance<br>- Zugriff auf verwaltete Identitäten<br>- Zugriff auf Azure Key Vault für Geheimnisse<br>- Azure AD-basierte Authentifizierung<br>- Unterstützung von Tags und Ressourcengruppen zur Vereinfachung der Sicherheitsverwaltung|
|Konfiguration von Metrikwarnungsregeln für Batch-Konten überwachen|Hiermit wird die Konfiguration von Metrikwarnungsregeln für Azure Batch-Konten überwacht, um die erforderliche Metrik zu aktivieren.|
|Aktivierung von Diagnoseprotokollen in Batch-Konten überwachen|Hierbei wird die Aktivierung von Protokollen überwacht, die dann bis zu einem Jahr lang aufbewahrt werden. Auf diese Weise können im Rahmen von Untersuchungen die Aktivitäten nachvollzogen werden, wenn ein Sicherheitsincident eintritt oder Ihr Netzwerk kompromittiert wird.|
|Aktivierung der Verschlüsselung für Variablen von Automation-Konten überwachen|Es ist wichtig, die Verschlüsselung für Variablenassets von Azure Automation-Konten zu aktivieren, wenn Sie vertrauliche Daten speichern.|
|Aktivierung von Diagnoseprotokollen in App Service überwachen|Hiermit wird die Aktivierung von Diagnoseprotokollen für die App überwacht. Auf diese Weise können im Rahmen von Untersuchungen die Aktivitäten nachvollzogen werden, wenn ein Sicherheitsincident eintritt oder Ihr Netzwerk kompromittiert wird.|
|Überwachen des Status der transparenten Datenverschlüsselung|Hiermit wird der Transparent Data Encryption-Status für SQL-Datenbanken überwacht.|
|Überwachungseinstellungen auf SQL Server-Ebene überwachen|Hierbei wird das Vorhandensein der SQL-Überwachung auf Serverebene überwacht.|
|\[Vorschauversion]: Nicht verschlüsselte SQL-Datenbank in Azure Security Center überwachen|Azure Security Center überwacht unverschlüsselte SQL Server-Instanzen oder -Datenbanken (wie empfohlen).|
|\[Vorschauversion]: Nicht überwachte SQL-Datenbank in Azure Security Center überwachen|Azure Security Center überwacht SQL Server-Instanzen und -Datenbanken, für die keine SQL-Überwachung aktiviert ist (wie empfohlen).|
|\[Vorschauversion]: Fehlende Systemupdates in Azure Security Center überwachen|Azure Security Center überwacht fehlende Updates des Sicherheitssystems auf Ihren Servern (wie empfohlen).|
|\[Vorschauversion]: Fehlende Blobverschlüsselung für Speicherkonten überwachen|Hierbei werden Speicherkonten überwacht, für die keine Blobverschlüsselung verwendet wird. Dies gilt nur für Microsoft Storage-Ressourcentypen und nicht für Speicher von anderen Anbietern. Azure Security Center überwacht den möglichen Just-In-Time-Netzwerkzugriff (wie empfohlen).|
|\[Vorschauversion]: Möglichen Just-In-Time-Netzwerkzugriff in Azure Security Center überwachen|Azure Security Center überwacht den möglichen Just-In-Time-Netzwerkzugriff (wie empfohlen).|
|\[Vorschauversion]: Mögliche App-Whitelist in Azure Security Center überwachen|Azure Security Center überwacht eine mögliche Konfiguration der Anwendungswhitelist.|
|\[Vorschauversion]: Zu wenig einschränkenden Netzwerkzugriff in Azure Security Center überwachen|Azure Security Center überwacht Netzwerksicherheitsgruppen, die über zu tolerante Regeln verfügen (wie empfohlen).|
|\[Vorschauversion]: Betriebssystem-Sicherheitsrisiken in Azure Security Center überwachen|Azure Security Center überwacht Server, die nicht die Anforderungen der konfigurierten Baseline erfüllen (wie empfohlen).| 
|\[Vorschauversion]: Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen|Azure Security Center überwacht Server, die nicht über einen installierten Microsoft System Center Endpoint Protection-Agent verfügen (wie empfohlen).|
|\[Vorschauversion]: Nicht verschlüsselte VM-Datenträger in Azure Security Center überwachen|Azure Security Center überwacht virtuelle Computer, für die keine Datenträgerverschlüsselung aktiviert ist (wie empfohlen).|
|\[Vorschauversion]: VM-Sicherheitsrisiken in Azure Security Center überwachen|Hiermit werden Sicherheitsrisiken überwacht, die von der Lösung zur Sicherheitsrisikobewertung erkannt werden, sowie virtuelle Computer, die nicht über eine Lösung zur Sicherheitsrisikobewertung in Azure Security Center verfügen (wie empfohlen).|
|\[Vorschauversion]: Nicht geschützte Webanwendung in Azure Security Center überwachen|Azure Security Center überwacht Webanwendungen, die nicht über einen Web Application Firewall-Schutz verfügen (wie empfohlen).|
|\[Vorschauversion]: Nicht geschützte Netzwerkendpunkte in Azure Security Center überwachen|Azure Security Center überwacht Netzwerkendpunkte, die nicht über einen Firewallschutz der nächsten Generation verfügen (wie empfohlen).|
|\[Vorschauversion]: Ergebnisse der SQL-Sicherheitsrisikobewertung in Azure Security Center überwachen|Hierbei werden Überprüfungsergebnisse der Sicherheitsrisikobewertung überwacht und Empfehlungen zum Beheben von Sicherheitsrisiken in der Datenbank bereitgestellt.|
|\[Vorschauversion]: Höchstanzahl der Besitzer für ein Abonnement überwachen|Wir empfehlen Ihnen, maximal drei Abonnementbesitzer festzulegen, um die Möglichkeit einer Sicherheitsverletzung durch einen kompromittierten Besitzer zu verringern.|
|\[Vorschauversion]: Mindestanzahl der Besitzer für ein Abonnement überwachen|Wir empfehlen Ihnen, mehr als einen Abonnementbesitzer festzulegen, um die Redundanz beim Administratorzugriff sicherzustellen.|
|\[Vorschauversion]: Konten mit Besitzerberechtigungen überwachen, für die MFA in einem Abonnement nicht aktiviert ist|MFA (Multi-Factor Authentication) muss für alle Abonnementkonten mit Besitzerberechtigungen aktiviert werden, um eine Sicherheitsverletzung für Konten oder Ressourcen zu verhindern.|
|\[Vorschauversion]: Konten mit Schreibberechtigungen überwachen, für die MFA in einem Abonnement nicht aktiviert ist|Multi-Factor Authentication muss für alle Abonnementkonten mit Schreibberechtigungen aktiviert werden, um eine Sicherheitsverletzung für Konten oder Ressourcen zu verhindern.|
|\[Vorschauversion]: Konten mit Leseberechtigungen überwachen, für die MFA in einem Abonnement nicht aktiviert ist|Multi-Factor Authentication muss für alle Abonnementkonten mit Leseberechtigungen aktiviert werden, um eine Sicherheitsverletzung für Konten oder Ressourcen zu verhindern.|
|\[Vorschauversion]: Veraltete Konten mit Besitzerberechtigungen für ein Abonnement überwachen|Veraltete Konten mit Besitzerberechtigungen müssen aus Ihrem Abonnement entfernt werden. Für veraltete Konten wurde die Anmeldung blockiert.|
|\[Vorschauversion]: Veraltete Konten in einem Abonnement überwachen|Veraltete Konten sollten aus Ihren Abonnements entfernt werden. Für veraltete Konten wurde die Anmeldung blockiert.|
|\[Vorschauversion]: Externe Konten mit Besitzerberechtigungen für ein Abonnement überwachen|Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden, um den Zugriff über diese Berechtigungen zu verhindern.|
|\[Vorschauversion]: Externe Konten mit Schreibberechtigungen für ein Abonnement überwachen|Externe Konten mit Schreibberechtigungen sollten aus Ihrem Abonnement entfernt werden, um einen nicht überwachten Zugriff zu verhindern.|
|\[Vorschauversion]: Externe Konten mit Leseberechtigungen für ein Abonnement überwachen|Externe Konten mit Leseberechtigungen sollten aus Ihrem Abonnement entfernt werden, um einen nicht überwachten Zugriff zu verhindern.|




## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie Sicherheitsrichtlinien in Security Center konfigurieren können. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln.

* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md): Hier erfahren Sie, wie Sie Entwurfsaspekte in Azure Security Center planen und verstehen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](https://blogs.msdn.com/b/azuresecurity/): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

Weitere Informationen zu Azure Policy finden Sie unter [Was ist Azure Policy?](../governance/policy/overview.md).
