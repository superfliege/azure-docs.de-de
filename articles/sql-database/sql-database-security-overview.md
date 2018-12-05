---
title: Übersicht über die Sicherheit der Azure SQL-Datenbank | Microsoft-Dokumentation
description: Informationen zu Azure SQL-Datenbank und zur SQL Server-Sicherheit, z.B. Unterschiede zwischen der Cloud und lokalem SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto, carlrab, emlisa
manager: craigg
ms.date: 10/22/2018
ms.openlocfilehash: 5bb3dc0245371248b005d642debb5b60026b9f4c
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635474"
---
# <a name="an-overview-of-azure-sql-database-security-capabilities"></a>Eine Übersicht über die Sicherheitsfunktionen von Azure SQL-Datenbank

Dieser Artikel beschreibt die Grundlagen zum Sichern der Datenebene einer Anwendung mit der Azure SQL-Datenbank. In diesem Artikel erhalten Sie vor allem die ersten Informationen zu den Ressourcen zum Schützen von Daten, zum Steuern des Zugriffs und für die proaktive Überwachung.

Eine vollständige Übersicht der für alle Varianten von SQL verfügbaren Sicherheitsfunktionen finden Sie unter [Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589). Weitere Informationen finden Sie auch im [technischen Whitepaper zu Sicherheit und Azure SQL-Datenbank](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="protect-data"></a>Schützen von Daten

### <a name="encryption"></a>Verschlüsselung

SQL-Datenbank schützt Ihre Daten, indem die Verschlüsselung für Daten in Bewegung mit [Transport Layer Security](https://support.microsoft.com/kb/3135244), für ruhende Daten mit [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) und für verwendete Daten mit [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) ermöglicht wird.

> [!IMPORTANT]
> Azure SQL-Datenbank erzwingt eine kontinuierliche Verschlüsselung (SSL/TLS) für alle Verbindungen, um sicherzustellen, dass alle Daten während der Übertragung zwischen Datenbank und Client verschlüsselt sind. Diese Verschlüsselung erfolgt unabhängig von der Einstellung für **Encrypt** oder **TrustServerCertificate** in der Verbindungszeichenfolge.
>
> Achten Sie in der Verbindungszeichenfolge Ihrer Anwendung darauf, eine verschlüsselte Verbindung anzugeben und dem Serverzertifikat *nicht* zu vertrauen. (Für den ADO.NET-Treiber müssen dazu **Encrypt=True** und **TrustServerCertificate=False** verwendet werden.) So schützen Sie Ihre Anwendung vor Man-in-the-Middle-Angriffen, da die Anwendung den Server überprüfen muss und eine Verschlüsselung erzwungen wird. Wenn Sie Ihre Verbindungszeichenfolge über das Azure-Portal abrufen, verfügt sie bereits über die richtigen Einstellungen.
>
> Informationen zu TLS und zur Konnektivität finden Sie unter [Überlegungen zu TLS](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity).

Es gibt noch weitere Möglichkeiten zum Verschlüsseln Ihrer Daten:

- [Verschlüsselung auf Zellenebene](https://msdn.microsoft.com/library/ms179331.aspx) werden bestimmte Spalten oder sogar Zellen, die Daten enthalten, mit unterschiedlichen Verschlüsselungsschlüsseln verschlüsselt.
-  Wenn Sie ein Hardwaresicherheitsmodul oder BYOK-Technologie (Bring Your Own Key) für Transparent Data Encryption benötigen, können Sie ggf. [Azure SQL Transparent Data Encryption: Bring Your Own Key support](transparent-data-encryption-byok-azure-sql.md) (Azure SQL – Transparent Data Encryption: Unterstützung von Bring Your Own Key (BYOK)) verwenden.

### <a name="data-discovery--classification"></a>Datenermittlung und -klassifizierung

Datenermittlung und -klassifizierung (zurzeit in der Vorschau) bietet erweiterte Funktionen für Azure SQL-Datenbank zum Ermitteln, Klassifizieren, Bezeichnen und Schützen sensibler Daten in Ihren Datenbanken. Das Ermitteln und Klassifizieren Ihrer sensiblen Daten (Geschäfts-/Finanz-/Gesundheitsdaten, PII usw.) kann eine entscheidende Rolle in der Strategie Ihrer Organisation zum Datenschutz spielen. Das Feature kann als Infrastruktur für Folgendes dienen:

- Verschiedene Sicherheitsszenarien, z.B. Überwachung und Warnungen bei abweichendem Zugriff auf sensible Daten
- Steuern des Zugriffs auf und Härten der Sicherheit von Datenbanken, die sensible Daten enthalten
- Unterstützen der Einhaltung von Datenschutzstandards und gesetzlicher Bestimmungen

Weitere Informationen finden Sie unter [Erste Schritte bei der Datenermittlung und -klassifizierung für SQL-Datenbank](sql-database-data-discovery-and-classification.md).

## <a name="control-access"></a>Steuern des Zugriffs

SQL-Datenbank schützt Ihre Daten, indem der Zugriff auf Ihre Datenbank beschränkt wird. Hierfür wird Folgendes verwendet: Firewallregeln, Authentifizierungsmechanismen, bei denen Benutzer ihre Identität nachweisen müssen, und Datenautorisierung über rollenbasierte Mitgliedschaften und Berechtigungen sowie mithilfe von Sicherheit auf Zeilenebene und dynamischer Datenmaskierung. Eine Beschreibung der Nutzung von Zugriffssteuerungsfunktionen in SQL-Datenbank finden Sie unter [Steuern des Zugriffs](sql-database-control-access.md).

> [!IMPORTANT]
> Die Verwaltung von Datenbanken und logischen Servern in Azure wird über die Rollenzuweisungen in Ihrem Portalbenutzerkonto gesteuert. Weitere Informationen zu diesem Artikel finden Sie unter [Rollenbasierte Zugriffssteuerung im Azure-Portal](../role-based-access-control/overview.md). Die Zugriffskontrolle mit Firewallregeln gilt *nicht* für die **verwaltete Azure SQL-Datenbank-Instanz**. Weitere Informationen über die erforderliche Netzwerkkonfiguration finden Sie im Artikel zum [Herstellen einer Verbindung mit einer verwalteten Instanz](sql-database-managed-instance-connect-app.md).

### <a name="firewall-and-firewall-rules"></a>Firewall und Firewallregeln

Als Schutz für Ihre Daten verhindern Firewalls jeglichen Zugriff auf Ihren Datenbankserver, bis Sie mit [Firewallregeln](sql-database-firewall-configure.md) angeben, welche Computer zugriffsberechtigt sind. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.

### <a name="authentication"></a>Authentifizierung

Die SQL-Datenbank-Authentifizierung bezieht sich darauf, auf welche Weise Sie Ihre Identität beim Herstellen der Verbindung mit der Datenbank nachweisen. SQL-Datenbank unterstützt zwei Arten der Authentifizierung:

- **SQL-Authentifizierung**

  Diese Authentifizierungsmethode verwendet einen Benutzernamen und ein Kennwort. Bei der Erstellung des logischen Servers für die Datenbank haben Sie eine "Server Admin"-Anmeldung mit Benutzername und Kennwort angegeben. Mit diesen Anmeldeinformationen können Sie sich bei jeder Datenbank auf diesem Server als Datenbankbesitzer bzw. "dbo" (database owner) authentifizieren.

- **Azure Active Directory-Authentifizierung**

  Diese Authentifizierungsmethode verwendet von Azure Active Directory verwaltete Identitäten und wird für verwaltete und integrierte Domänen unterstützt. Verwenden Sie immer die Active Directory-Authentifizierung (integrierte Sicherheit), [sofern dies möglich ist](https://msdn.microsoft.com/library/ms144284.aspx). Wenn Sie die Azure Active Directory-Authentifizierung verwenden möchten, müssen Sie einen weiteren Serveradministrator mit der Bezeichnung "Azure AD Admin" erstellen, der zum Verwalten von Azure Active Directory-Benutzern und -Gruppen berechtigt ist. Dieser Administrator kann außerdem die gleichen Aufgaben wie ein normaler Serveradministrator ausführen. Unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) finden Sie eine exemplarische Vorgehensweise zum Erstellen eines Azure AD-Administrators, um die Azure Active Directory-Authentifizierung zu aktivieren.

### <a name="authorization"></a>Autorisierung

Autorisierung bezieht sich darauf, welche Aufgaben ein Benutzer in einer Azure SQL-Datenbank ausführen kann. Dies wird durch die Datenbank-Rollenmitgliedschaften und Objektebenenberechtigungen Ihres Benutzerkontos gesteuert. Als bewährte Methode sollten Sie Benutzern nur die minimal erforderlichen Berechtigungen erteilen. Das Server-Admin-Konto, mit dem Sie eine Verbindung herstellen, ist Mitglied von db_owner und verfügt daher über alle Berechtigungen in der Datenbank. Speichern Sie dieses Konto für die Bereitstellung von Schemaänderungen und andere Verwaltungsvorgänge. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkteren Berechtigungen, um eine Verbindung von Ihrer Anwendung zur Datenbank mit den geringsten Berechtigungen herzustellen, die von Ihrer Anwendung benötigt werden.

### <a name="row-level-security"></a>Sicherheit auf Zeilenebene

Bei der Sicherheit auf Zeilenebene können Kunden den Zugriff auf Zeilen in einer Datenbanktabelle basierend auf den Merkmalen des Benutzers steuern, der eine Abfrage ausführt (etwa Gruppenmitgliedschaft oder Ausführungskontext). Weitere Informationen finden Sie unter [Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security).

### <a name="dynamic-data-masking"></a>Dynamische Datenmaskierung

Die dynamische Datenmaskierung für SQL-Datenbank schränkt die Offenlegung vertraulicher Daten ein, indem sie für nicht berechtigte Benutzer maskiert werden. Mit der dynamischen Datenmaskierung werden in Azure SQL-Datenbank automatisch potenziell sensible Daten ermittelt und direkt umsetzbare Empfehlungen bereitgestellt, um diese Felder mit minimalen Auswirkungen auf die Anwendungsschicht zu maskieren. Hierzu werden die sensiblen Daten im Resultset einer Abfrage in festgelegten Datenbankfeldern verborgen, ohne dass die Daten in der Datenbank geändert werden. Weitere Informationen finden Sie unter [Dynamische Datenmaskierung für SQL-Datenbank](sql-database-dynamic-data-masking-get-started.md).

## <a name="proactive-monitoring"></a>Proaktive Überwachung

SQL-Datenbank schützt Ihre Daten, indem Funktionen für die Überwachung und Bedrohungserkennung bereitgestellt werden.

### <a name="auditing"></a>Überwachung

Bei der Überwachung von SQL-Datenbank werden Datenbankaktivitäten nachverfolgt, und Sie erhalten Unterstützung bei der Einhaltung von gesetzlichen Bestimmungen, indem Datenbankereignisse in Ihrem Azure Storage-Konto in einem Überwachungsprotokoll aufgezeichnet werden. Dank der Überwachung können Sie die derzeitigen Datenbankaktivitäten verstehen und die Verlaufsaktivität analysieren und untersuchen, um potenzielle Bedrohungen oder vermutete Missbrauchsfälle und Sicherheitsverletzungen zu identifizieren. Weitere Informationen finden Sie unter [Erste Schritte mit der SQL-Datenbanküberwachung](sql-database-auditing.md).  

### <a name="threat-detection"></a>Bedrohungserkennung

Mit der Bedrohungserkennung wird die Überwachung vervollständigt, indem der Azure SQL-Datenbank-Dienst um eine zusätzliche Security Intelligence-Ebene erweitert wird, die ungewöhnliche und eventuell schädliche Versuche erkennt, auf Datenbanken zuzugreifen oder diese zu missbrauchen. Sie werden vor verdächtigen Aktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen mit Einschleusung von SQL-Befehlen und anomalen Datenbankzugriffsmustern gewarnt. Warnungen der Bedrohungserkennung können in [Azure Security Center](https://azure.microsoft.com/services/security-center/) angezeigt werden und bieten Detailinformationen zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann. Die Bedrohungserkennung kostet 15 $/Server/Monat. Die ersten 60 Tage sind kostenlos. Weitere Informationen finden Sie unter [Erste Schritte mit der Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection.md).

## <a name="compliance"></a>Compliance

Zusätzlich zu den oben aufgeführten Features und Funktionalitäten, mit denen Ihre Anwendung eine Reihe von Sicherheitsanforderungen erfüllen kann, wird Azure SQL-Datenbank außerdem regelmäßigen Überprüfungen unterzogen und ist für eine Reihe von Kompatibilitätsstandards zertifiziert. Weitere Informationen finden Sie im [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/), wo die aktuellste Liste von [Compliance-Zertifizierungen für SQL-Datenbank](https://www.microsoft.com/trustcenter/compliance/complianceofferings) angezeigt wird.

## <a name="security-management"></a>Sicherheitsverwaltung

SQL-Datenbank unterstützt Sie bei der Verwaltung Ihrer Datensicherheit durch Datenbanküberprüfungen und ein zentrales Sicherheitsdashboard mit der [SQL-Sicherheitsrisikobewertung](sql-vulnerability-assessment.md).

Die **[SQL-Sicherheitsrisikobewertung](sql-vulnerability-assessment.md)** ist ein einfach konfigurierbares, in Azure SQL-Datenbank integriertes Tool, mit dem potenzielle Sicherheitsrisiken der Datenbank ermittelt, nachverfolgt und behoben werden können. Die Bewertung führt einen Schwachstellenprüfung in Ihrer Datenbank durch und generiert einen Bericht zum Sicherheitszustand, der verwertbare Schritte zur Lösung von Sicherheitsproblemen und zur Verbesserung Ihrer Datenbanksicherheit enthält. Ein Bewertungsbericht kann für Ihre Umgebung angepasst werden, indem eine akzeptable Baseline für Berechtigungskonfigurationen, Funktionskonfigurationen und Datenbankeinstellungen festgelegt wird. Dies kann Ihnen bei Folgendem helfen:

- Erfüllen der Konformitätsanforderungen, die die Berichte für die Datenbanküberprüfung erfordern
- Erfüllen der Datenschutzstandards
- Überwachen einer dynamischen Datenbankumgebung, bei der Änderungen schwer nachzuverfolgen sind.

Weitere Informationen finden Sie unter [SQL-Sicherheitsrisikobewertung](sql-vulnerability-assessment.md).

## <a name="next-steps"></a>Nächste Schritte

- Eine Beschreibung der Nutzung von Zugriffssteuerungsfunktionen in SQL-Datenbank finden Sie unter [Steuern des Zugriffs](sql-database-control-access.md).
- Eine Erörterung zur Datenbanküberwachung finden Sie unter [Überwachung von SQL-Datenbank](sql-database-auditing.md).
- Eine Erörterung zur Bedrohungserkennung finden Sie unter [Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection.md).
