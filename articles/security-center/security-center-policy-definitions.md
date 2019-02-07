---
title: In Azure Security Center überwachte Azure Policy-Definitionen | Microsoft-Dokumentation
description: In Azure Security Center überwachte Azure Policy-Definitionen
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: c89cb1aa-74e8-4ed1-980a-02a7a25c1a2f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/15/2019
ms.author: rkarlin
ms.openlocfilehash: 925bf325d128f1757d24d29013f4e27eb788d7e2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180669"
---
# <a name="azure-security-policies-monitored-by-azure-security-center"></a>In Azure Security Center überwachte Azure-Sicherheitsrichtlinien
Dieser Artikel enthält eine Liste der Azure Policy-Definitionen, die in Security Center überwacht werden können. Weitere Informationen zu Sicherheitsrichtlinien finden Sie unter [Arbeiten mit Sicherheitsrichtlinien](tutorial-security-policy.md).

## <a name="available-security-policies"></a>Verfügbare Sicherheitsrichtlinien

Informationen zu den integrierten Richtlinien, die von Security Center überwacht werden, finden Sie in der folgenden Tabelle:

| Richtlinie | Zweck der Richtlinie |
| --- | --- |
|Aktivierung von Diagnoseprotokollen in Service Fabric und Virtual Machine Scale Sets überwachen|Es wird empfohlen, die Protokollierung zu aktivieren. So können Sie Aktivitäten nachvollziehen, wenn bei einem Incident oder einer Beeinträchtigung Untersuchungen durchgeführt werden müssen.|
|Autorisierungsregeln für Event Hub-Namespaces überwachen|Event Hub-Clients dürfen keine Zugriffsrichtlinie auf Namespace-Ebene verwenden, die Zugriff auf alle Warteschlangen und Themen in einem Namespace bereitstellt. Um dem Sicherheitsmodell der geringsten Rechte zu entsprechen, müssen Sie Zugriffsrichtlinien auf Entitätsebene erstellen, damit nur der jeweiligen Entität Zugriff auf Warteschlangen und Themen gewährt wird.|
|Vorhandensein von Autorisierungsregeln für Event Hub-Entitäten überwachen|Hiermit wird das Vorhandensein von Autorisierungsregeln für Event Hub-Entitäten überwacht, um Zugriff mit den geringsten Rechten zu gewähren.|
|Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen|Überwachen Sie uneingeschränkten Netzwerkzugriff in den Firewalleinstellungen Ihres Speicherkontos. Konfigurieren Sie stattdessen Netzwerkregeln, sodass nur Anwendungen aus zulässigen Netzwerken auf ein Speicherkonto zugreifen können. Um Verbindungen von bestimmten Internetclients oder lokalen Clients zuzulassen, kann Zugriff für Datenverkehr aus bestimmten virtuellen Azure-Netzwerken oder an IP-Adressbereiche im öffentlichen Internet gewährt werden.|
|Verwendung benutzerdefinierter RBAC-Regeln überwachen|Hiermit werden integrierte Rollen (z.B. „Benutzer“, „Mitwirkender“ und „Leser“) anstelle benutzerdefinierter RBAC-Rollen überwacht, die fehleranfällig sind. Die Verwendung benutzerdefinierter Rollen wird als Ausnahme betrachtet und erfordert eine strenge Überprüfung und Bedrohungsmodellierung.|
|Aktivierung von Diagnoseprotokollen in Azure Stream Analytics überwachen|Hiermit wird die Aktivierung von Protokollen überwacht, und die Protokolle werden bis zu einem Jahr aufbewahrt. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist.|
|Sichere Übertragung in Speicherkonten überwachen|Hiermit wird die Anforderung sicherer Übertragungen in Ihren Speicherkonto überwacht. Sichere Übertragung ist eine Option, die erzwingt, dass Ihr Storage-Konto nur Anforderungen von sicheren Verbindungen (HTTPS) akzeptiert. Durch die Verwendung von HTTPS wird eine Authentifizierung zwischen dem Server und dem Dienst sichergestellt, und die übertragenen Daten werden vor Angriffen auf Netzwerkebene geschützt, wie z.B. Man-in-the-Middle-Angriffe, Abhörangriffe und Session Hijacking.|
|Bereitstellung eines Azure Active Directory-Administrators für SQL Server überwachen|Hiermit wird die Bereitstellung eines Azure Active Directory-Administrators für Ihre SQL Server-Instanz überwacht, um die Azure AD-Authentifizierung zu aktivieren. Die Azure AD-Authentifizierung ermöglicht eine vereinfachte Verwaltung von Berechtigungen und eine zentralisierte Identitätsverwaltung von Datenbankbenutzern und anderen Microsoft-Diensten.|
|Autorisierungsregeln für Service Bus-Namespaces überwachen|Service Bus-Clients dürfen keine Zugriffsrichtlinie auf Namespace-Ebene verwenden, die Zugriff auf alle Warteschlangen und Themen in einem Namespace bereitstellt. Um dem Sicherheitsmodell der geringsten Rechte zu entsprechen, müssen Sie Zugriffsrichtlinien auf Entitätsebene erstellen, damit nur der jeweiligen Entität Zugriff auf Warteschlangen und Themen gewährt wird.|
|Aktivierung von Diagnoseprotokollen in Service Bus überwachen|Hiermit wird die Aktivierung von Protokollen überwacht, und die Protokolle werden bis zu einem Jahr aufbewahrt. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist.|
|Festlegung der ClusterProtectionLevel-Eigenschaft auf EncryptAndSign in Service Fabric überwachen|Service Fabric bietet drei Schutzebenen („None“, „Sign“ und „EncryptAndSign“) für die Kommunikation zwischen zwei Knoten unter Verwendung eines primären Clusterzertifikats. Legen Sie die Schutzebene fest, um sicherzustellen, dass alle zwischen zwei Knoten übertragenen Nachrichten verschlüsselt und digital signiert werden.| 
|Verwendung von Azure Active Directory für Clientauthentifizierung in Service Fabric überwachen|Hiermit wird überwacht, ob die Clientauthentifizierung in Service Fabric ausschließlich über Azure Active Directory erfolgt.| 
|Aktivierung von Diagnoseprotokollen für Search-Dienst überwachen|Hiermit wird die Aktivierung von Protokollen überwacht, und die Protokolle werden bis zu einem Jahr aufbewahrt. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist.| 
|Aktivieren der Überprüfung nur für sichere Verbindungen mit Azure Cache for Redis|Aktivieren der Überprüfung nur für Verbindungen über SSL mit Azure Cache for Redis Durch die Verwendung sicherer Verbindungen wird eine Authentifizierung zwischen dem Server und dem Dienst sichergestellt, und die übertragenen Daten werden vor Angriffen auf Netzwerkebene geschützt, wie z.B. Man-in-the-Middle-Angriffe, Abhörangriffe und Session Hijacking.| 
|Aktivierung von Diagnoseprotokollen in Logic Apps überwachen|Hiermit wird die Aktivierung von Protokollen überwacht, und die Protokolle werden bis zu einem Jahr aufbewahrt. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist.| 
|Aktivierung von Diagnoseprotokollen in Key Vault überwachen|Hiermit wird die Aktivierung von Protokollen überwacht, und die Protokolle werden bis zu einem Jahr aufbewahrt. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist.|
|Aktivierung von Diagnoseprotokollen in Event Hub überwachen|Hiermit wird die Aktivierung von Protokollen überwacht, und die Protokolle werden bis zu einem Jahr aufbewahrt. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist.| 
|Aktivierung von Diagnoseprotokollen in Azure Data Lake Storage überwachen|Hiermit wird die Aktivierung von Protokollen überwacht, und die Protokolle werden bis zu einem Jahr aufbewahrt. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist.| 
|Aktivierung von Diagnoseprotokollen in Data Lake Analytics überwachen|Hiermit wird die Aktivierung von Protokollen überwacht, und die Protokolle werden bis zu einem Jahr aufbewahrt. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist.| 
|Verwendung von klassischen Speicherkonten überwachen|Verwenden Sie Azure Resource Manager für Ihre Speicherkonten, um von den folgenden Sicherheitsverbesserungen zu profitieren: strengere Zugriffssteuerung (RBAC), bessere Überwachung, Azure Resource Manager-basierte Bereitstellung und Governance, Zugriff auf verwaltete Identitäten, Zugriff auf Schlüsseltresore für Geheimnisse, Azure AD-basierte Authentifizierung und Unterstützung für Markierungen und Ressourcengruppen für eine einfachere Sicherheitsverwaltung.| 
|Verwendung klassischer VMs überwachen|Verwenden Sie Azure Resource Manager für Ihre virtuellen Computer, um von den folgenden Sicherheitsverbesserungen zu profitieren: strengere Zugriffssteuerung (RBAC), bessere Überwachung, Azure Resource Manager-basierte Bereitstellung und Governance, Zugriff auf verwaltete Identitäten, Zugriff auf Schlüsseltresore für Geheimnisse, Azure AD-basierte Authentifizierung und Unterstützung für Markierungen und Ressourcengruppen für eine einfachere Sicherheitsverwaltung.| 
|Konfiguration von Metrikwarnungsregeln für Batch-Konten überwachen|Hiermit wird die Konfiguration von Metrikwarnungsregeln für ein Batch-Konto überwacht, um die erforderliche Metrik zu aktivieren.| 
|Konfiguration von Metrikwarnungsregeln für Batch-Konten überwachen|Hiermit wird die Konfiguration von Metrikwarnungsregeln für ein Batch-Konto überwacht, um die erforderliche Metrik zu aktivieren.| 
|Aktivierung von Diagnoseprotokollen in Batch-Konten überwachen|Hiermit wird die Aktivierung von Protokollen überwacht, und die Protokolle werden bis zu einem Jahr aufbewahrt. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist.| 
|Aktivierung der Verschlüsselung für Variablen von Automation-Konten überwachen|Es ist wichtig, die Verschlüsselung für Variablenassets von Automation-Konten zu aktivieren, wenn vertrauliche Daten gespeichert werden.| 
|Aktivierung von Diagnoseprotokolle in App Services überwachen|Hiermit wird die Aktivierung von Diagnoseprotokollen für die App überwacht. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist.| 
|Überwachen des Status der transparenten Datenverschlüsselung|Hiermit wird der Transparent Data Encryption-Status für SQL-Datenbanken überwacht.| 
|Überwachungseinstellungen auf SQL Server-Ebene überwachen|Überwacht das Vorhandensein der SQL-Überwachung auf Serverebene| 
|[Vorschau]: Nicht verschlüsselte SQL-Datenbank in Azure Security Center überwachen|Hiermit werden nicht verschlüsselte SQL Server-Instanzen oder -Datenbanken über Azure Security Center in Form von Empfehlungen überwacht.| 
|[Vorschau]: Nicht überwachte SQL-Datenbank in Azure Security Center überwachen|Hiermit werden SQL Server-Instanzen und -Datenbanken ohne aktivierte SQL-Überwachung über Azure Security Center in Form von Empfehlungen überwacht.| 
|[Vorschau]: Fehlende Systemupdates in Azure Security Center überwachen|Hiermit werden fehlende Sicherheitssystemupdates auf Ihren Servern über Azure Security Center in Form von Empfehlungen überwacht.| 
|[Vorschau]: Fehlende Blobverschlüsselung für Speicherkonten überwachen|Diese Richtlinie überwacht Speicherkonten ohne Blobverschlüsselung. Sie gilt nur für Microsoft.Storage-Ressourcentypen und nicht für andere Speicheranbieter. Ein möglicher Just-In-Time-Netzwerkzugriff wird über Azure Security Center in Form von Empfehlungen überwacht.| 
|[Vorschau]: Möglichen Just-In-Time-Netzwerkzugriff in Azure Security Center überwachen|Hiermit wird der mögliche Just-In-Time-Netzwerkzugriff über Azure Security Center in Form von Empfehlungen überwacht.| 
|[Vorschau]: Mögliche App-Whitelist in Azure Security Center überwachen|Hiermit wird eine eventuell vorhandene Anwendungswhitelistkonfiguration über Azure Security Center überwacht.| 
|[Vorschau]: Zu wenig einschränkenden Netzwerkzugriff in Azure Security Center überwachen|Hiermit werden Netzwerksicherheitsgruppen mit zu wenig einschränkenden Regeln über Azure Security Center in Form von Empfehlungen überwacht.| 
|[Vorschau]: Betriebssystem-Sicherheitsrisiken in Azure Security Center überwachen|Hiermit werden Server, die nicht der konfigurierten Baseline entsprechen, über Azure Security Center in Form von Empfehlungen überwacht.| 
|[Vorschau]: Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen|Hiermit werden Server ohne installierten Endpoint Protection-Agent über Azure Security Center in Form von Empfehlungen überwacht.| 
|[Vorschau]: Nicht verschlüsselte VM-Datenträger in Azure Security Center überwachen|Hiermit werden virtuelle Computer ohne aktivierte Datenträgerverschlüsselung über Azure Security Center in Form von Empfehlungen überwacht.| 
|[Vorschau]: VM-Sicherheitsrisiken in Azure Security Center überwachen|Überwacht Sicherheitsrisiken, die von der Lösung zur Sicherheitsrisikobewertung erkannt werden, und virtuelle Computer ohne eine Lösung zur Sicherheitsrisikobewertung im Azure Security Center in Form von Empfehlungen.| 
|[Vorschau]: Nicht geschützte Webanwendung in Azure Security Center überwachen|Hiermit werden Webanwendungen ohne Web Application Firewall-Schutz über Azure Security Center in Form von Empfehlungen überwacht.| 
|[Vorschau]: Nicht geschützte Netzwerkendpunkte in Azure Security Center überwachen|Hiermit werden Netzwerkendpunkte ohne Next Generation Firewall-Schutz über Azure Security Center in Form von Empfehlungen überwacht.| 
|[Vorschau]: Ergebnisse der SQL-Sicherheitsrisikobewertung in Azure Security Center überwachen|Überprüfungsergebnisse der Sicherheitsrisikobewertung und Empfehlungen zum Beheben von Sicherheitsrisiken in der Datenbank überwachen.| 
|[Vorschau]: Höchstanzahl der Besitzer für ein Abonnement überwachen|Es wird empfohlen, bis zu drei Abonnementbesitzer festzulegen, um die Möglichkeit einer Sicherheitsverletzung durch einen kompromittierten Besitzer zu verringern.| 
|[Vorschau]: Mindestanzahl der Besitzer für ein Abonnement überwachen|Es wird empfohlen, mehrere Abonnementbesitzer festzulegen, um Redundanz beim Administratorzugriff zu gewährleisten.| 
|[Vorschau]: Konten mit Besitzerberechtigungen überwachen, für die MFA in einem Abonnement nicht aktiviert ist|MFA (Multi-Factor Authentication) muss für alle Abonnementkonten mit Besitzerberechtigungen aktiviert werden, um eine Sicherheitsverletzung für Konten oder Ressourcen zu verhindern.| 
|[Vorschau]: Konten mit Schreibberechtigungen überwachen, für die MFA in einem Abonnement nicht aktiviert ist|MFA (Multi-Factor Authentication) muss für alle Abonnementkonten mit Schreibrechten aktiviert sein, um eine Sicherheitsverletzung von Konten oder Ressourcen zu verhindern.| 
|[Vorschau]: Konten mit Leseberechtigungen überwachen, für die MFA in einem Abonnement nicht aktiviert ist|MFA (Multi-Factor Authentication) muss für alle Abonnementkonten mit Leserechten aktiviert sein, um eine Sicherheitsverletzung von Konten oder Ressourcen zu verhindern.| 
|[Vorschau]: Veraltete Konten mit Besitzerberechtigungen für ein Abonnement überwachen|Veraltete Konten mit Besitzerberechtigungen müssen aus Ihrem Abonnement entfernt werden. Veraltete Konten sind Konten, bei denen die Anmeldung blockiert wurde.| 
|[Vorschau]: Veraltete Konten in einem Abonnement überwachen|Veraltete Konten sollten aus Ihren Abonnements entfernt werden. Veraltete Konten sind Konten, bei denen die Anmeldung blockiert wurde.| 
|[Vorschau]: Externe Konten mit Besitzerberechtigungen für ein Abonnement überwachen|Externe Konten mit Besitzerberechtigungen müssen aus Ihrem Abonnement entfernt werden, um einen nicht überwachten Zugriff zu verhindern.| 
|[Vorschau]: Externe Konten mit Schreibberechtigungen für ein Abonnement überwachen|Externe Konten mit Schreibrechten müssen aus Ihrem Abonnement entfernt werden, um nicht überwachten Zugriff zu verhindern.| 
|[Vorschau]: Externe Konten mit Leseberechtigungen für ein Abonnement überwachen|Externe Konten mit Leserechten müssen aus Ihrem Abonnement entfernt werden, um nicht überwachten Zugriff zu verhindern.| 




## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie Sicherheitsrichtlinien in Security Center konfigurieren können. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md): Hier erfahren Sie, wie Sie die Entwurfsaspekte zu Azure Security Center planen und verstehen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

Weitere Informationen zu Azure Policy finden Sie unter [Was ist Azure Policy?](../azure-policy/azure-policy-introduction.md).
