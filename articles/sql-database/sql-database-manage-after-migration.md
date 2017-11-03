---
title: 'Azure SQL-Datenbank: Verwalten nach der Migration | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Ihre Datenbank nach der Migration zu Azure SQL-Datenbank verwalten.
services: sql-database
documentationcenter: 
author: joesackmsft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 10/14/2016
ms.author: Joe.Sack
ms.suite: SQL
ms.prod_service: sql-database
ms.component: management
ms.openlocfilehash: 663ab4aaf229f8a88d1116b34ccb74450aa66c9d
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="how-should-i-manage-my-azure-sql-database-after-migration"></a>Wie wird meine Azure SQL-Datenbank nach der Migration verwaltet?

*Häufig gestellte Fragen zum Verwalten Ihrer Investitionen von Azure SQL-Datenbank*

Sie haben kürzlich SQL Server-Datenbanken in Azure SQL-Datenbank verschoben, oder Sie planen in näherer Zukunft eine Verschiebung? Folgendes ist nach der Verschiebung zu beachten: Da es sich bei SQL-Datenbank um eine *Platform as a Service* handelt, übernimmt Microsoft in Ihrem Namen die Handhabung verschiedene Bereiche. Dadurch verändern sich die Verfahren Ihres Unternehmens u.a. in wichtigen Bereichen wie Sicherheit, Geschäftskontinuität, Datenbankwartung, Leistungsoptimierung und Überwachung. 

In diesem Artikel sollen Ressourcen klar organisiert werden, und Sie sollen über die nötigen Schritte für den Wechsel zum Verwalten Ihrer Investitionen in SQL-Datenbank informiert werden. Es werden vor allem Themenbereiche wie Geschäftskontinuität, Sicherheit, Datenbankwartung und -überwachung sowie Leistung und Datenverschiebung abgedeckt. Sie werden über wichtige Bereiche, in denen sich SQL Server und SQL-Datenbank unterscheiden, und über bewährte Methoden für den Betrieb informiert, mit denen Sie Ihre Vorteile maximieren und Risiken minimieren können. 

## <a name="manage-business-continuity-after-migration"></a>Verwalten der Geschäftskontinuität nach der Migration

### <a name="how-do-i-create-and-manage-backups-on-sql-database"></a>Wie werden Sicherungen auf SQL-Datenbank erstellt und verwaltet? 
SQL-Datenbank sichert Ihre Datenbanken automatisch, und Sie können einen bestimmten Zeitpunkt, der im Aufbewahrungszeitraum liegt, wiederherstellen. Der Aufbewahrungszeitraum beträgt für Standard- und Premium-Datenbanken 35 Tage und sieben Tage für Basic-Datenbanken. Außerdem können Sie über die langfristige Aufbewahrungsfunktion Sicherheitsdateien über einen längeren Zeitraum hinweg (bis zu zehn Jahre) speichern und jederzeit eine Wiederherstellung aus diesen Sicherungen vornehmen. Zudem werden Datenbankensicherungen georepliziert, damit Sie im Falle eines Unglücks oder einer regionalen Katastrophe eine Geowiederherstellung vornehmen können. Weitere Informationen finden Sie unter [Business continuity overview (Übersicht über die Geschäftskontinuität)](sql-database-business-continuity.md).

### <a name="how-do-i-ensure-business-continuity-in-the-event-of-a-datacenter-level-disaster-or-a-regional-catastrophe"></a>Wie wird im Falle einer Katastrophe auf der Ebene des Rechenzentrums oder einer regionalen Katastrophe Geschäftskontinuität gewährleistet? 

Datenbankensicherungen werden georepliziert, damit Sie im Falle eines Unglücks oder einer regionalen Katastrophe überall eine Geowiederherstellung vornehmen können. Weitere Informationen finden Sie unter [Business continuity overview (Übersicht über die Geschäftskontinuität)](sql-database-business-continuity.md). Außerdem können Sie mit SQL-Datenbank aktiv georeplizierte sekundäre Datenbanken in einer anderen Region warten. Wenn Sie die Datenbanken in eine Gruppe für automatische Failover konfigurieren, wird im Falle einer Katastrophe ein automatisches Failover auf eine sekundäre Datenbank ausgeführt. Wenn keine Gruppe für automatische Failover konfiguriert wird, muss Ihre Anwendung aktiv auf eine Katastrophe überwachen und ein Failover auf eine sekundäre Datenbank einleiten, wenn es zu einer Katastrophe kommen sollte. 
### <a name="sql-server-provided-me-readable-secondary-replicas-can-i-access-the-secondaries-on-sql-database"></a>SQL Server hat lesbare sekundäre Replikate erstellt. Kann ich darauf über SQL-Datenbank zugreifen? 

Ja. Die Funktion „Aktive Georeplikation“ wird verwendet, um lesbare sekundäre Replikate zu erstellen. 

### <a name="how-does-my-disaster-recovery-plan-change-from-on-premise-to-sql-database"></a>Inwiefern ändert sich der lokale Notfallwiederherstellungsplan mit SQL-Datenbank? 
Im Zusammenhang mit SQL Server-Implementierungen mussten Sicherungen mithilfe von Funktionen wie Failoverclustering, Datenbankspiegelung, Replikation, Protokollversand oder einfachen BACPAC-Sicherungen aktiv verwaltet werden. Unter SQL-Datenbank werden Sicherungen hingegen vollständig von Microsoft verwaltet und Sicherungs- und Notfallwiederherstellungspläne können mit nur wenigen Klicks im Azure-Portal (bzw. über wenige Befehle unter PowerShell) konfiguriert und funktionsfähig gemacht werden. ‌
### <a name="in-the-event-of-disaster-how-do-i-recover-my-databases"></a>Wie werden Datenbanken im Falle einer Katastrophe wiederhergestellt? 
SQL-Datenbank lässt eine Wiederherstellung Ihrer Datenbank zu einem bestimmten Zeitpunkt innerhalb der letzten 35 Tage zu. Diese Option sollten Sie wählen, wenn Daten verloren gehen, oder wenn es zu einer Katastrophe im Zusammenhang mit der Anwendung kommt. 

Im Falle einer regionalen Katastrophe können Sie eine Wiederherstellung aus Ihren geosekundären Datenbanken in einer anderen Region durchführen, wenn die georeplizierten sekundären Datenbanken konfiguriert wurden. Damit Sie in Echtzeit auf Ihre Anwendungen zugreifen können, besteht die Möglichkeit, ein Failover auf eine geosekundäre Datenbank in einer anderen Region manuell auszuführen. Wenn allerdings eine Gruppe für automatische Failover konfiguriert wurde, wird dieses Failover auf eine geosekundäre Datenbank im Falle einer Katastrophe automatisch ausgeführt. Wenn aber keine georeplizierte sekundäre Datenbank konfiguriert wurde, können Sie Ihre Datenbanken trotzdem aus einer automatisch replizierten Sicherungsdatei (integrierte Funktion, keine Konfiguration notwendig) wiederherstellen. Allerdings verlängert sich dabei die Wiederherstellungszeit (12 Stunden RTO), und es kann zu einem Datenverlust von bis zu einer Stunde kommen. 

### <a name="are-the-failovers-to-secondary-transparent-how-does-my-application-handle-database-failovers"></a>Sind die Failover auf eine sekundäre Datenbank transparent? Wie bewältigt meine Anwendung Failover von Datenbanken? 
Wenn Gruppen für automatische Failover konfiguriert wurden, ist das Failover auf die sekundäre Datenbank transparent. Wenn dies nicht der Fall ist, muss Ihre Anwendung allerdings die Logik beinhalten, mit der die Verfügbarkeit der primären Datenbank überwacht werden kann, und einen manuellen Failover auf die zweite Datenbank ausführen. 
 
## <a name="manage-security-after-migration"></a>Verwalten der Sicherheit nach der Migration

### <a name="how-can-i-restrict-access-to-my-sql-database"></a>Wie kann der Zugriff auf meine SQL-Datenbank eingeschränkt werden? 
 
Es gibt einige Möglichkeiten, den Verbindungszugang zu Ihrer SQL-Datenbank einzuschränken. 
1. Wenn Sie den Datenverkehr über Internet Express Route einschränken, wird eine dedizierte Verbindung mit dem Azure-Netzwerk hergestellt, damit Ihre Daten nicht über das Internet übertragen werden. Außerdem ist es möglich, die Verbindung regionsübergreifend mithilfe von Express Route zu konfigurieren. Unter den folgenden Links finden Sie eine detailliertere Beschreibung von Express Route: 
 - [Einführung in Express Route](../expressroute/expressroute-introduction.md)
 - [Voraussetzungen](../expressroute/expressroute-prerequisites.md) 
 - [Workflows](../expressroute/expressroute-workflows.md) 
 
2. Auswählen der Ressourcen, die mit SQL-Datenbank verbunden sein sollen: 

   Standardmäßig ist Ihre SQL-Datenbank so konfiguriert, dass Sie „Alle Azure-Dienste zulässt“. Das bedeutet, das möglicherweise alle virtuellen Computer in Azure versuchen, eine Verbindung zu der Datenbank herzustellen.  Trotzdem müssen weiterhin alle Anmeldungen authentifiziert werden. Wenn Sie nicht möchten, dass Ihre Datenbanken für alle Azure-IP-Adressen zugänglich ist, können Sie die Auswahl „Alle Azure-Dienste zulassen“ deaktivieren und [VNET Service Endpoints](sql-database-vnet-service-endpoint-rule-overview.md) (VNET-Dienstendpunkte) verwenden, um den eingehenden Zugriff auf die Datenbank auf Azure-Ressourcen innerhalb eines vorgegebenen Azure-VNET-Subnetzes zu beschränken. 

   ![VNET-Dienstendpunkte](./media/sql-database-manage-after-migration/vnet-service-endpoints.png) 

   Stattdessen können Sie auch [reservierte IP-Adressen](../virtual-network/virtual-networks-reserved-public-ip.md) für Ihre virtuellen Computer bereitstellen, und diese in den Firewalleinstellungen für den Server auf die Positivliste setzen. (Im unten abgebildeten Screenshot sehen Sie ein Beispiel aus dem Azure-Portal.) Das Zuweisen von reservierten IP-Adressen erspart Ihnen das Aktualisieren von Firewallregeln, wenn sich die IP-Adressen ändern. 

3. Vermeiden des Verfügbarmachens von Port 1433 außerhalb von Azure

   Führen Sie SSMS in Azure mithilfe der [Azure RemoteApp](https://www.microsoft.com/cloud-platform/azure-remoteapp-client-apps) aus. Dafür müssen Sie keine ausgehenden Verbindungen mit Port 1433 öffnen. Die IP-Adresse ist statisch, sodass die Datenbank nur für die RemoteApp geöffnet sein kann, die eine mehrstufige Authentifizierung unterstützt und mehrere Benutzer zulässt. 

### <a name="what-authentication-methods-are-offered-in-sql-database"></a>Welche Authentifizierungsmethoden werden in SQL-Datenbank angeboten?

Die wichtigsten Authentifizierungsmethoden, die in SQL-Datenbank und SQL Data Warehouse angeboten werden, sind die Authentifizierung über Azure Active Directory und die SQL-Authentifizierung. Azure Active Directory (AD) ist ein zentralisierter Identitäts- und Zugriffsverwaltungsdienst, und SQL ist nur einer von vielen Azure-Diensten, die in Azure AD integriert sind. Ein zentralisiert verwalteter Dienst hat den Vorteil, dass Ihre Anmeldeinformationen über alle von Ihnen verwendeten Azure-Dienste hinweg freigegeben sind, damit die Authentifizierung vereinfacht wird. Dadurch können SQL-Datenbank und SQL Data Warehouse außerdem eine mehrstufige Authentifizierung und Benutzerkonten für Gäste innerhalb einer Azure AD-Domäne anbieten. 

Wenn Sie bereits über eine lokale Active Directory verfügen, können Sie dieses Verzeichnis mit Azure Active Directory verbinden und es auf Azure erweitern. 


|||
|---|---|
| Wenn Sie|Azure SQL-Datenbank/Azure SQL Data Warehouse|
| Azure Active Directory (AD) lieber nicht in Azure verwenden möchten,|verwenden Sie [SQL-Authentifizierung](sql-database-security-overview.md).|
| AD auf einem lokalen SQL-Server verwendet haben,|[Verbinden Sie AD mit Azure AD](../active-directory/connect/active-directory-aadconnect.md), und verwenden Sie die Azure AD-Authentifizierung. So können Sie auf die Funktion „Einmaliges Anmelden“ zurückgreifen.|
| eine mehrstufige Authentifizierung (MFA) erzwingen müssen,|muss über [Microsoft Conditional Access (bedingter Zugang zu Microsoft)](sql-database-conditional-access.md) MFA als Strategie festgelegt sein, und Sie müssen [Azure AD Universal authentication with MFA support (Universelle Authentifizierung von Azure AD mit MFA-Unterstützung)](sql-database-ssms-mfa-authentication.md) verwenden.|
| über Gastkonten aus Microsoft-Konten (live.com, outlook.com) oder anderen Domänen (gmail.com) verfügen,|verwenden Sie [die universelle Authentifizierung von Azure AD](sql-database-ssms-mfa-authentication.md) in SQL-Datenbank/Data Warehouse, die die [Zusammenarbeit zwischen Azure AD und B2B unterstützt](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).|
| mit Ihren Azure AD-Anmeldeinformationen aus einer Verbunddomäne bei Windows angemeldet sind,|verwenden Sie [Azure AD integrated authentication (die integrierte Azure AD-Authentifizierung)](sql-database-aad-authentication-configure.md).|
| mit Ihren Anmeldeinformationen aus einer Domäne, die nicht mit Azure verbunden ist, bei Windows angemeldet sind,|verwenden Sie [Azure AD password authentication (Kennwortauthentifizierung mit Azure AD)](sql-database-aad-authentication-configure.md).|
| über Dienste auf mittlerer Ebene verfügen, die sich mit Azure SQL-Datenbank oder Data Warehouse verbinden müssen,|verwenden Sie [Azure AD token authentication (Tokenauthentifizierung mit Azure AD)](sql-database-aad-authentication-configure.md).
|||

### <a name="how-can-i-limit-access-to-sensitive-data-in-my-databases-from-the-application-side"></a>Wie kann der Zugriff auf sensible Daten in meiner Datenbank auf Anwendungsseite eingeschränkt werden? 

Es gibt einige Optionen in SQL-Datenbank, um nicht autorisierten Benutzern den Zugriff auf sensible Daten zu verweigern: 

- [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) ist eine Form der clientseitigen Verschlüsselung, die sensible Spalten in Ihrer Datenbank verschlüsselt (sodass Sie für Datenbankadministratoren und nicht autorisierte Benutzern in Chriffretext dargestellt werden). Der Schlüssel für die Always Encrypted-Verschlüsselung wird auf Clientseite gespeichert, damit nur autorisierte Clients die sensiblen Spalten entschlüsseln können. Always Encrypted unterstützt derzeit Übereinstimmungsvergleiche, sodass DBAs weiterhin als Teil ihrer SQL-Befehle verschlüsselte Spalten anfragen können. Diese Verschlüsselungsmethode kann mit einer Vielzahl von Schlüsselspeicheroptionen wie [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md), dem Windows-Zertifikatspeicher sowie lokalen Hardware-Sicherheitsmodulen verwendet werden.
- Die [dynamische Datenmaskierung](sql-database-dynamic-data-masking-get-started.md) ist eine Funktion zur Datenmaskierung, die die Freigabe von sensiblen Daten beschränkt, indem sie sie für nicht berechtigte Benutzer auf Anwendungsebene maskiert. Sie legen eine Regel für die Maskierung fest, die ein Maskierungsmuster erstellen (z.B. sollen nur die letzten vier Ziffern einer nationalen ID angezeigt werden, und der Rest wird durch ein X ersetzt) und bestimmen kann, welche Benutzer von ihr ausgeschlossen sein sollen.
- Bei der [Sicherheit auf Zeilenebene](/sql/relational-databases/security/row-level-security) können Sie den Zugriff auf Zeilen in einer Datenbanktabelle basierend auf dem Benutzer steuern, der die Abfrage ausführt (Gruppenmitgliedschaft oder Ausführungskontext). Die Zugangsbeschränkung wird nicht auf Anwendungsebene, sondern auf Datenbankebene festgelegt, damit die App-Logik vereinfacht wird. 

### <a name="what-encryption-options-do-i-have-in-sql-database-and-what-actors-does-the-encryption-protect-from"></a>Welche Verschlüsselungsoptionen gibt es in SQL-Datenbank, und vor welchen Akteuren schützt die Verschlüsselung?
Für SQL-Datenbank sind vor allem drei Verschlüsselungstechnologien verfügbar: 
- [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) (wird in der vorherigen Antwort erwähnt): End-to-End-Verschlüsselung von sensiblen Tabellenspalten für nicht autorisierte Clients des physischen Datenträgers. Die Administratoren des Servers und der Daten haben keinen Zugriff auf sensible Daten, da die Verschlüsselungsschlüssel auf dem Client gespeichert sind. 
- [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) (TDE): Verschlüsselung ruhender Daten auf Datenbankebene. Datendateien, Protokolldateien und zugeordnete Sicherungen werden vor dem Diebstahl von physischen Medien geschützt. TDE ist standardmäßig für alle neu erstellten Datenbanken aktiviert.
 
  Das folgende Diagramm stellt eine Übersicht über die verschiedenen Verschlüsselungstechnologien dar.

   ![Übersicht über die Verschlüsselung](./media/sql-database-manage-after-migration/overview-encryption.png)

### <a name="how-should-i-manage-encryption-keys-in-the-cloud"></a>Wie werden Verschlüsselungsschlüssel in der Cloud verwaltet? 
Es gibt sowohl für die Methode Always Encrypted (clienstseitige Verschlüsselung) als auch für Transparent Data Encryption (Verschlüsselung ruhender Daten) Optionen zur Schlüsselverwaltung. Wir empfehlen, Verschlüsselungsschlüssel regelmäßig und so häufig zu rotieren, wie es sowohl im Hinblick auf interne Regelungen als auch auf Kompatibilitätsanforderungen nötig ist.

- **Always Encrypted**: Es gibt eine [Zwei-Schlüssel-Hierarchie](/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted). Das bedeutet, dass eine Spalte mit sensiblen Daten mit einem AES 256-Spaltenverschlüsselungsschlüssel verschlüsselt wird, der wiederum von einem Spaltenhauptschlüssel verschlüsselt wird. Die Clienttreiber, die für die Always Encrypted-Methode zur Verfügung gestellt werden, haben keine Beschränkungen im Hinblick auf die Länge des Spaltenhauptschlüssels.

  Der verschlüsselte Wert des Spaltenverschlüsselungsschlüssel wird in der Datenbank gespeichert, und der Spaltenhauptschlüssel wird in einem vertrauenswürdigen Schlüsselspeicher wie dem Windows-Zertifikatspeicher, Azure Key Vault oder einem Hardware-Sicherheitsmodul gespeichert. 
  
  Sowohl der [Spaltenverschlüsselungsschlüssel als auch der Spaltenhauptschlüssel](/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell) können gedreht werden. Die Rotation des Spaltenverschlüsselungsschlüssels kann sehr zeitaufwendig sein. Dies hängt von der Größe der Tabellen ab, die die verschlüsselten Spalten enthalten. Planen Sie diese Art von Rotation daher eingehend. Die Rotation des Spaltenhauptschlüssels beeinträchtigt die Datenbankleistung dagegen nicht. Sie kann mit getrennten Rollen durchgeführt werden.

  Das folgende Diagramm stellt eine Übersicht über die wichtigsten Schlüsselspeicheroptionen für die Spaltenhauptschlüssel bei der Always Encrypted-Methode dar. 

   ![Always Encrypted-Speicheranbieter für den Spaltenhauptschlüssel](./media/sql-database-manage-after-migration/always-encrypted.png)

- **Transparent Data Encryption** (TDE): Es gibt eine Zwei-Schlüssel-Hierarchie. Die Daten in allen Benutzerdatenbanken werden mit einem symmetrischen, für die Datenbank eindeutigen, AES 256-Datenbankverschlüsselungsschlüssel verschlüsselt, der wiederum von einem für den Server eindeutigen asymmetrischen RSA 2048-Masterschlüssel verschlüsselt wird. 

  Der Masterschlüssel für die Transparent Data Encryption-Methode wird der Einfachheit halber vom SQL-Datenbank-Dienst verwaltet. Wenn Ihre Organisation den Masterschlüssel überwachen möchte, können Sie [Azure Key Vault](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql) als Schlüsselspeicher verwenden. 

  Dadurch hat Ihre Organisation die Kontrolle über die Schlüsselbereitstellung, die Rotation und die Berechtigungen. [Die Rotation oder der Typwechsel des TDE-Masterschlüssels](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql-key-rotation) geht schnell vonstatten, da nur Verschlüsselungsschlüssel wieder verschlüsselt werden. 

  Für Organisationen, die die Rollen zwischen der Sicherheits- und der Datenverwaltung trennen, kann ein Sicherheitsadministrator das Schlüsselmaterial für den TDE-Masterschlüssel in Azure Key Vault bereitstellen und dem Datenbankadministrator eine Azure Key Vault-Schlüsselkennung zur Verfügung stellen, den er zur Verschlüsselung ruhender Daten auf einem Server verwenden soll. 

## <a name="monitoring-and-compliance-after-migration"></a>Überwachung und Kompatibilität nach der Migration

### <a name="how-do-i-monitor-database-activities-in-sql-database"></a>Wie werden Datenbankaktivitäten in SQL-Datenbank überwacht?
Es gibt einige in SQL-Datenbank integrierte Möglichkeiten zur Überwachung der Sicherheit und anderer Ereignisse auf der Datenbank:
- Mithilfe der [SQL-Überwachung](sql-database-auditing.md) können Sie Überwachungsprotokolle von Datenbankereignissen in Ihrem eigenen Azure-Speicherkonto sammeln.
- Mithilfe der [SQL-Bedrohungserkennung](sql-database-threat-detection.md) können Sie verdächtige Aktivitäten erkennen, die auf einen möglichen böswilligen Versuch hindeuten, auf Daten in der Datenbank zuzugreifen, gegen die Datensicherheit zu verstoßen oder die Daten zu missbrauchen. Die SQL-Bedrohungserkennung für Datenbanken führt mehrere verschiedene Algorithmen aus, die potentielle Sicherheitsrisiken, Angriffe mit Einschleusung von SQL-Befehlen und ungewöhnliche Muster für den Zugriff auf die Datenbank (wie der Zugriff von einem ungewöhnlichen Ort oder einem unbekannten Prinzipal aus) ermitteln. Sicherheitsbeauftragte oder andere vorgesehene Administratoren erhalten eine E-Mail-Benachrichtigung, wenn eine Bedrohung auf der Datenbank erkannt wird. Jede Benachrichtigung enthält Details zur verdächtigen Aktivität und Empfehlungen zur weiteren Untersuchung und Abwendung der Bedrohung. 
- Die [SQL-Sicherheitsrisikobewertung](sql-vulnerability-assessment.md) ist ein Datenbankdienst zum Scannen und zur Berichterstattung, mit dessen Hilfe Sie den Sicherheitsstatus Ihrer Datenbank effektiv überwachen sowie Sicherheitsrisiken und Abweichungen von der von Ihnen definierten Sicherheitsbaseline ermitteln können. Nach jedem Scan wird neben einer benutzerdefinierte Liste mit zur Umsetzung empfohlenen Schritten und Wiederherstellungsskripts auch ein Bewertungsbericht zur Verfügung gestellt, der zur Einhaltung von Kompatibilitätsvorgaben verwendet werden kann. 
- Die [SQL-OMS-Anwendung zur Sicherheitssynchronisierung](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) verwendet öffentliche APIs der Operations Management Suite (OMS), um SQL-Überwachungsprotokolle für Log Analytics in OMS zu pushen. Außerdem sollen dadurch u.a. folgende benutzerdefinierte Warnungen definiert werden: 
 - Kachel und Dashboard zur Überwachung von SQL-Datenbank, die klare und kohärente Berichte über Ihre Datenbankaktivitäten bereitstellen. 
 - SQL-Log Analytics zur Analyse Ihrer Datenbankaktivität und Untersuchung von Diskrepanzen und Anomalien, die auf verdächtige Sicherheitsbedrohungen hinweisen können.
 - Erweiterte Regelungen für bestimmte Warnungen für beobachtete Ereignisse, die Warnungen via E-Mail, Webhook und Azure Automation-Runbook auslösen (z.B. Kennwortänderungen, Zeiten nach Geschäftsschluss oder bestimmte SQL-Befehle).
- [Azure Security Center](../security-center/security-center-intro.md) bietet eine zentralisierte Sicherheitsverwaltung über verschiedene Workloads hinweg an, die in Azure, lokal und in anderen Clouds ausgeführt werden können. Sie können nachvollziehen, ob essentielle Methoden zum Schutz von SQL-Datenbank wie die Überwachung und Transparent Data Encryption auf allen Ressourcen konfiguriert wurde, und anhand Ihrer eigenen Anforderungen Richtlinien erstellen. 

### <a name="is-sql-database-compliant-with-any-regulatory-requirements-and-how-does-that-help-with-my-own-organizations-compliance"></a>Ist SQL-Datenbank mit allen gesetzlichen Anforderungen kompatibel, und inwiefern hilft das dabei, die Kompatibilitätsvorgaben meiner eigenen Organisation einzuhalten? 
Azure SQL-Datenbank ist mit einer Vielzahl von Kompatibilitätsvorgaben konform. Auf dem [Microsoft Trust Center](https://www.microsoft.com/trustcenter/compliance/complianceofferings) finden Sie die neusten Maßnahmen zur Einhaltung von Kompatibilitätsvorgaben. Dort können Sie sich Detailinformationen zu den Maßnahmen, die für Ihre Organisation von Bedeutung sind, anzeigen lassen, um zu prüfen, ob Azure SQL-Datenbank zu den kompatiblen Azure-Diensten gehört. Bitte beachten Sie, dass Azure SQL-Datenbank zwar möglicherweise als kompatibler Dienst zertifiziert ist, die Kompatibilität Ihres Unternehmens dadurch aber lediglich unterstützt und nicht automatisch garantiert wird. 

## <a name="database-maintenance-and-monitoring-after-migration"></a>Datenbankwartung und Überwachung nach der Migration

### <a name="how-do-i-monitor-growth-in-data-size-and-resource-utilization"></a>Wie überwache ich ein Wachstum der Daten und die Ressourcenverwendung?

- Sie können Monitor-Metriken über die Größe Ihrer Datenbank und die Ressourcenverwendung über das Diagramm „Überwachung“ im Azure-Portal ansehen. 

  ![Überwachungsdiagramm](./media/sql-database-manage-after-migration/monitoring-chart.png)

- Detailliertere Informationen über Abfragen finden Sie in der „Statistik zur Abfrageleistung“ im Azure-Portal. Dafür muss der „Abfragedatenspeicher“ für Ihre Datenbank aktiviert sein.

  ![Query Performance Insight](./media/sql-database-manage-after-migration/query-performance-insight.png)

- Stattdessen können Sie die Metriken auch über Dynamic Management Views (DMVs) über [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) und [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) ansehen. 

### <a name="how-often-do-i-need-to-run-consistency-checks-like-dbcccheckdb"></a>Wie oft muss ich Konsistenzprüfungen wie DBCC_CHECKDB durchführen?
Mit DBCC_CHECKDB überprüfen Sie die logische und physische Integrität aller Objekte in der Datenbank. Sie müssen diese Überprüfungen nicht mehr selbst ausführen, da sie für Azure von Microsoft verwaltet werden. Weitere Informationen finden Sie unter [Data Integrity in Azure SQL Database (Datenintegrität in Azure SQL-Datenbank)](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/).

## <a name="monitor-performance-and-resource-utilization-after-migration"></a>Überwachen der Leistung und Ressourcenverwendung nach der Migration

### <a name="how-do-i-monitor-performance-and-resource-utilization-in-azure-sql-database"></a>Wie wird die Leistung und Ressourcenverwendung in Azure SQL-Datenbank überwacht?
Sie können die Leistung und Ressourcenverwendung in Azure SQL-Datenbank mithilfe der folgenden Methoden überwachen:

- **Azure-Portal**:Im Azure Portal wird Ihnen die Nutzung einer Einzeldatenbank angezeigt, wenn Sie die Datenbank auswählen und auf das Diagramm im Bereich „Übersicht“ klicken. Sie können das Diagramm ändern, sodass mehrere Metriken z.B. zum CPU-, DTU- und Data IO-Prozentsatz sowie zu den Sitzungen und Datenbankgrößen in Prozent angezeigt werden. 
  ![Ressourcenverwendung](./media/sql-database-manage-after-migration/resource-utilization.png)

  Von diesem Diagramm aus können Sie außerdem Sicherheitswarnungen nach Ressourcen konfigurieren. Diese Warnungen ermöglichen es Ihnen, dass Sie auf Ressourcenbedingungen mit einer E-Mail antworten, an einen HTTPS-/HTTP-Endpunkt schreiben oder eine Aktion durchführen können. Genauere Anweisungen finden Sie unter [Monitoring database performance in Azure SQL Database (Überwachen der Datenbankleistung in Azure SQL-Datenbank)](sql-database-single-database-monitor.md).

- **Ansichten**: Sie können die dynamische Verwaltungssicht [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) abfragen, um den statistischen Verlauf des Ressourcenverbrauchs der letzten Stunde zurückzugeben. Demgegenüber können Sie die Systemkatalogsicht [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) abfragen, um den Verlauf der letzten 14 Tage zurückzugeben. 

- **Statistik zur Abfrageleistung**: Über die [Statistik zur Abfrageleistung](sql-database-query-performance.md) können Sie den Verlauf der Abfragen, die den höchsten Ressourcenverbrauch aufweisen, und die Abfragen mit langer Laufzeit für eine bestimmte Datenbank sehen. Für diese Funktion muss der [Abfragespeicher](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) aktiviert und für die Datenbank aktiv sein.

- **Azure SQL-Analyse (Vorschau) in Log Analytics**: Mit [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md) können Sie wichtige Leistungsmetriken für Azure SQL Analytics sammeln und visualisieren, die bis zu 150.000 Azure SQL-Datenbanken und 5.000 elastische SQL-Pools pro Arbeitsbereich unterstützen. Sie können darüber Benachrichtigungen überwachen und erhalten. Sie können Azure SQL-Datenbank und elastische Pool-Metriken über mehrere Azure-Abonnements und elastische Pools hinweg überwachen und sie dafür nutzen, Probleme auf allen Ebene eines Anwendungsstapels zu ermitteln. 

### <a name="how-do-i-ensure-i-am-using-the-appropriate-service-tier-and-performance-level"></a>Wie kann man sichergehen, dass die passende Dienst- und Leistungsebene verwendet wird?
Überwachen Sie die dynamischen Verwaltungssichten [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) und [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database), um den CPU-, I/O- und Speicherverbrauch nachvollziehen zu können. Zudem können Sie die [Statistik zur Abfrageleistung](sql-database-query-performance.md) von SQL-Datenbank verwenden, um den Ressourcenverbrauch zu sehen. Wenn Ihr Verbrauch der verfügbaren Ressourcen dauerhaft hoch ist, sollten Sie in Erwägung ziehen, innerhalb der bestehenden Dienstebene auf eine höhere Leistungsebene umzusteigen oder eine höhere Leistungsebene zu verwenden. Wenn Ihr Verbrauch der verfügbaren Ressourcen umgekehrt dauerhaft sehr niedrig ist, sollten Sie in Erwägung ziehen, auf eine niedrigere Leistungs- oder Dienstebene umzusteigen.

### <a name="i-am-seeing-performance-issues-how-does-my-azure-sql-database-troubleshooting-methodology-differ-from-sql-server"></a>Es gibt Probleme mit der Leistung. Wie unterscheiden sich die Methoden zur Problembehebung mit Azure SQL-Datenbank von den Methoden mit SQL Server?
Größtenteils bleiben die Methoden zur Behandlung von Leistungsproblemen mit Azure SQL-Datenbank erhalten. Trotzdem gibt es einige Unterschiede: Wenn Sie beispielsweise eine Verringerung der Gesamtleistung beobachten, überwachen Sie die dynamischen Verwaltungssichten [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) und [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database), um den CPU-, I/O- und Speicherverbrauch nachvollziehen zu können. Unter Umständen müssen Sie basierend auf den Anforderungen des Workloads die Leistungsebene und bzw. oder die Dienstebene ändern.
Verständliche Empfehlungen für die Optimierung von Leistungsproblemen, finden Sie unter [Tuning performance in Azure SQL Database (Leistungsoptimierung in Azure SQL-Datenbank)](sql-database-performance-guidance.md). 

### <a name="do-i-need-to-maintain-indexes-and-statistics"></a>Müssen Indizes und Statistiken gewartet werden?
Azure SQL-Datenbank wartet Indizes und Statistiken nicht automatisch als Teil des Dienstes. Sie sind dafür verantwortlich, die Wartung von Indizes und Statistiken zu planen. In dem folgenden Artikel, „Methoden zur Azur Automation“, werden einige Optionen zum Planen von Aufträgen für Azure SQL-Datenbank detailliert erklärt.

## <a name="data-movement-after-migration"></a>Datenverschiebung nach der Migration

### <a name="how-do-i-export-and-import-data-as-bacpac-files-from-azure-sql-database"></a>Wie werden Daten als BACPAC-Dateien aus Azure SQL-Datenbank exportiert bzw. in Azure SQL-Datenbank importiert? 

- **Export**: Sie können Ihre Azure SQL-Datenbank als eine BACPAC-Datei aus dem Azure-Portal exportieren.

  ![Als BACPAC exportieren](./media/sql-database-export/database-export.png)

- **Importieren**: Sie können eine BACPAC-Datei über das Azure-Portal in eine Datenbank importieren.

  ![Als BACPAC importieren](./media/sql-database-import/import.png)

### <a name="how-do-i-synchronize-data-between-azure-sql-database-sql-server-2016--2012"></a>Wie werden Daten zwischen Azure SQL-Datenbank und SQL Server 2016/2012 synchronisiert?
Die [Datensynchronisierungs](sql-database-sync-data.md)-Funktion hilft Ihnen dabei, Daten bidirektional zwischen mehreren lokalen SQL Server-Datenbanken und Azure SQL-Datenbank zu synchronisieren. Da dieser Vorgang triggerbasiert ist, wird zwar eine letztliche Konsistenz garantiert (kein Datenverlust), jedoch kann die Transaktionskonsistenz dabei nicht garantiert werden. 

## <a name="next-steps"></a>Nächste Schritte
Informationen zu [Azure SQL-Datenbank](sql-database-technical-overview.md).
