---
title: "Office 365-Lösung in der Operations Management Suite (OMS) | Microsoft Docs"
description: "Dieser Artikel bietet Einzelheiten zu Konfiguration und Verwendung der Office 365-Projektmappe in der OMS an.  Es enthält eine ausführliche Beschreibung der Office 365-Datensätze, die in die Log Analytics erstellt."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: dcc44986acbb76eafc3cfacb79acf237802de021
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2017
---
# <a name="office-365-solution-in-operations-management-suite-oms"></a>Office 365-Lösung in der Operations Management Suite (OMS)

![Office 365-logo](media/oms-solution-office-365/icon.png)

Die Office 365-Lösung für die Operations Management Suite (OMS) können Sie Ihre Office 365-Umgebung in Protokollanalyse zu überwachen.  

- Überwachen von Benutzeraktivitäten auf Ihre Office 365-Konten Analysieren der Verwendungsmuster als auch verhaltensbasierten Trends zu identifizieren. Beispielsweise können Sie spezielle Nutzungsszenarien wie z. B. Dateien extrahieren, die sich außerhalb Ihrer Organisation oder die am häufigsten verwendeten SharePoint-Websites gemeinsam verwendet werden.
- Überwachen Sie die Administrator-Aktivitäten zum Nachverfolgen von Änderungen an der Konfiguration oder höhere Privilegstufe Vorgänge.
- Erkennen Sie, und untersuchen Sie unerwünschte Benutzerverhalten, die für Ihren organisatorischen Anforderungen angepasst werden können.
- Führen Sie die überwachungs- und Kompatibilitätsstatus vor. Beispielsweise können Sie Dateivorgänge für den Zugriff auf vertrauliche Dateien überwachen, die Sie mit der überwachungs- und Kompatibilitätsstatus können.
- Führen Sie die operative Problembehandlung mithilfe der OMS-Suche auf Office 365-Aktivitätsdaten Ihrer Organisation.

## <a name="prerequisites"></a>Voraussetzungen
Folgendes muss vor dieser Lösung wird installiert und konfiguriert.

- Organisation Office 365-Abonnement.
- Anmeldeinformationen für ein Benutzerkonto, das ein Globaladministrator ist.
- Um die Überwachungsdaten zu erhalten, müssen Sie [konfigurieren Sie die Überwachung](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&rs=en-US&ad=US#PickTab=Before_you_begin) in Ihrem Office 365-Abonnement.  Beachten Sie, dass [postfachüberwachung](https://technet.microsoft.com/library/dn879651.aspx) wird separat konfiguriert.  Sie können weiterhin installieren die Lösung und andere Daten sammeln, wenn die Überwachung nicht konfiguriert ist.
 


## <a name="management-packs"></a>Management Packs
Bei dieser Lösung werden keine Management Packs in verbundenen Verwaltungsgruppen installiert.
  

## <a name="configuration"></a>Konfiguration
Nach dem [die Office 365-Lösung für Ihr Abonnement hinzufügen](../log-analytics/log-analytics-add-solutions.md), für die Verbindung mit Ihrem Office 365-Abonnement.

1. Fügen Sie die Alert Management-Lösung dem OMS-Arbeitsbereich hinzu, indem Sie den unter [Hinzufügen von Lösungen](../log-analytics/log-analytics-add-solutions.md)beschriebenen Prozess verwenden.
2. Wechseln Sie zu **Einstellungen** im OMS-Portal.
3. Klicken Sie unter **verbundene Datenquellen**Option **Office 365**.
4. Klicken Sie auf **Office 365 verbinden**.<br>![Verbinden der Prozessschritte Office 365](media/oms-solution-office-365/configure.png)
5. Melden Sie sich beim Office 365 mit einem Konto an, die ein Globaladministrator für Ihr Abonnement ist an. 
6. Das Abonnement wird mit der Arbeitslasten aufgeführt, die die Lösung überwachen.<br>![Verbinden der Prozessschritte Office 365](media/oms-solution-office-365/connected.png) 


## <a name="data-collection"></a>Datensammlung
### <a name="supported-agents"></a>Unterstützte Agents
Die Office 365-Lösung nicht abrufen von Daten aus jeder der [OMS-Agenten](../log-analytics/log-analytics-data-sources.md).  Es ruft Daten direkt aus Office 365.

### <a name="collection-frequency"></a>Sammlungshäufigkeit
Office 365 sendet eine [Webhook Benachrichtigung](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) mit detaillierten Daten an die Protokollanalyse wird ein Datensatz erstellt.

## <a name="using-the-solution"></a>Verwenden der Lösung
Wenn Sie die Office 365-Lösung zu Ihrem OMS-Arbeitsbereich hinzufügen der **Office 365** Kachel zu Ihrem OMS-Dashboard hinzugefügt. Auf dieser Kachel werden ein Zahlenwert und eine grafische Darstellung der Anzahl von Computern in Ihrer Umgebung und jeweils die Updatekonformität angezeigt.<br><br>
![Kachel "Zusammenfassung" Office 365](media/oms-solution-office-365/tile.png)  

Klicken Sie auf die **Office 365** zu öffnenden Kacheln der **Office 365** Dashboard.

![Office 365-Dashboard](media/oms-solution-office-365/dashboard.png)  

Das Dashboard enthält die Spalten, die in der folgenden Tabelle angegeben sind. In jeder Spalte sind die zehn wichtigsten Warnungen nach ihrer Anzahl aufgeführt. Dies richtet sich jeweils nach den Spaltenkriterien für den angegebenen Bereich und Zeitraum. Sie können eine protokollsuche ausführen, die die gesamte Liste durch Klicken auf Weitere Informationen finden Sie alle am unteren Rand der Spalte oder durch Klicken auf die Kopfzeile der Spalte bereitstellt.

| Column | Beschreibung |
|:--|:--|
| Vorgänge | Enthält Informationen über die aktiven Benutzer aus Ihrer alle überwachten Office 365-Abonnements. Sie werden auch die Anzahl der Aktivitäten anzuzeigen, die mit der Zeit auftreten können.
| Exchange | Zeigt die Aufschlüsselung der Exchange Server-Aktivitäten, z. B. Postfach hinzufügen-Berechtigung oder die Set-Postfach. |
| SharePoint | Zeigt die obersten Aktivitäten an, dass Benutzer auf SharePoint-Dokumenten ausführen. Wenn Sie diese Kachel Drilldown von der Seite "Suche" zeigt die Details dieser Aktivitäten, z. B. Zieldokument und den Speicherort dieser Aktivität. Z. B. für ein Ereignis auf die Datei zugegriffen werden kann das Dokument anzeigen, auf die zugegriffen wird, ihre zugeordneten Kontonamen, und die IP-Adresse. |
| Azure Active Directory | Enthält die obersten Benutzeraktivitäten, z. B. das Benutzerkennwort zurücksetzen und Anmeldeversuche. Wenn Sie einen Drilldown ausführen, werden Sie können die Details dieser Aktivitäten wie Ergebnisstatus anzuzeigen. Dies ist hilfreich, wenn Sie verdächtige Aktivitäten auf Ihrem Azure Active Directory überwachen möchten. |




## <a name="log-analytics-records"></a>Log Analytics-Datensätze

Alle Datensätze in den Arbeitsbereich für Protokollanalyse durch die Office 365-Lösung erstellt eine **Typ** von **OfficeActivity**.  Die **OfficeWorkload** Eigenschaft bestimmt, welche Office 365-Dienst, der den Datensatz bezieht sich auf – Exchange, AzureActiveDirectory, SharePoint oder OneDrive.  Die **RecordType** Eigenschaft gibt den Typ des Vorgangs an.  Die Eigenschaften für jeden Vorgangstyp variieren und sind in den folgenden Tabellen dargestellt.

### <a name="common-properties"></a>Allgemeine Eigenschaften
Die folgenden Eigenschaften gelten für alle Office 365-Datensätze zur Verfügung.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ | *OfficeActivity* |
| ClientIP | Die IP-Adresse des Geräts, das verwendet wurde, wenn die Aktivität protokolliert wurde. Die IP-Adresse wird im Format für eine IPv4- oder IPv6-Adresse angezeigt. |
| OfficeWorkload | Office 365-Dienst, der der Datensatz bezieht.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| Vorgang | Der Name der Aktivität Benutzer oder Administrator.  |
| Organisations-ID | Die GUID für Ihre Organisation Office 365-Mandanten. Dieser Wert wird immer dem für Ihre Organisation, unabhängig vom Office 365-Dienst übereinstimmen, es erfolgt. |
| RecordType | Der Typ des Vorgangs ausgeführt. |
| ResultStatus | Gibt an, ob die Aktion (angegeben in die Vorgangseigenschaft) erfolgreich oder nicht war. Mögliche Werte sind erfolgreich, PartiallySucceded oder fehlgeschlagen. Für Exchange-administratoraktivität, ist der Wert entweder "true" oder "false". |
| UserId | Der UPN (User Principal Name) des Benutzers, der die Aktion ausgeführt, mit der der Datensatz wird protokolliert; beispielsweise my_name@my_domain_name. Beachten Sie, dass auch Einträge für die Aktivitäten von Systemkonten (z. B. SHAREPOINT\system "oder" NTAUTHORITY\SYSTEM ") enthalten sind. | 
| UserKey | Eine alternative ID für den Benutzer in der UserId-Eigenschaft identifiziert.  Diese Eigenschaft wird z. B. mit der eindeutige Passport-ID (PUID) für Ereignisse, die von Benutzern in SharePoint, OneDrive for Business und Exchange ausgeführt aufgefüllt. Diese Eigenschaft kann auch den gleichen Wert als der UserID-Eigenschaft für Ereignisse, die in anderen Diensten und Ereignissen von Systemkonten ausgeführt angeben.|
| UserType | Der Typ des Benutzers, der der Vorgang ausgeführt werden soll.<br><br>Administrator<br>Anwendung<br>DcAdmin<br>Regulär <br>Reserviert<br>ServicePrincipal<br>System |


### <a name="azure-active-directory-base"></a>Azure Active Directory-Basis
Die folgenden Eigenschaften gelten für alle Datensätze von Azure Active Directory zur Verfügung.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | Der Typ des Azure AD-Ereignis. |
| Die "ExtendedProperties" | Die erweiterten Eigenschaften des Azure AD-Ereignisses. |


### <a name="azure-active-directory-account-logon"></a>Azure Active Directory-Konto anmelden
Diese Datensätze werden erstellt, wenn ein Active Directory-Benutzer versucht, sich anzumelden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectoryAccountLogon |
| Anwendung | Die Anwendung, die dem Anmeldeereignis Konto wie z. B. Office 15 auslöst. |
| Client- | Details des Clients Geräte, Betriebssystem des Geräts und Gerätebrowser, die verwendet wurde, für die der Konto-Login-Ereignis. |
| loginStatus | Diese Eigenschaft ist direkt vom OrgIdLogon.LoginStatus. Die Zuordnung der verschiedenen interessante Anmeldefehler konnte durch Warnungen Algorithmen erfolgen. |
| UserDomain | Die Mandanten-Identitätsinformationen (TII). | 


### <a name="azure-active-directory"></a>Azure Active Directory
Diese Datensätze werden erstellt, wenn Änderungen oder Erweiterungen an Azure Active Directory-Objekten vorgenommen werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AADTarget | Der Benutzer, dem für die Aktion (identifiziert durch die Vorgangseigenschaft) ausgeführt wurde. |
| Akteur | Der Benutzer oder der Dienstprinzipal, der die Aktion ausgeführt werden soll. |
| ActorContextId | Die GUID der Organisation, der der Akteur gehört. |
| ActorIpAddress | Der Akteur IP-Adresse in IPV4- oder IPV6-Format vorliegen. |
| InterSystemsId | Die GUID, die die Aktionen für Komponenten in Office 365-Dienst zu überwachen. |
| IntraSystemId |   Die GUID, die von Azure Active Directory zum Nachverfolgen von der Aktions generiert wird. |
| SupportTicketId | Das Ticket-ID für die Aktion in Situationen von "Act-on-Behalf-of" unterstützt. |
| TargetContextId | Die GUID der Organisation, der der Zielbenutzer gehört. |


### <a name="data-center-security"></a>Datencenter-Sicherheit
Diese Datensätze werden von Data Center Security Audit-Daten erstellt.  

| Eigenschaft | Beschreibung |
|:--- |:--- |
| EffectiveOrganization | Der Name des Mandanten, dem die Erhöhung der Rechte-Cmdlet als Ziel verwendet wurde. |
| ElevationApprovedTime | Der Zeitstempel für, wenn die Erhöhung der Rechte genehmigt wurde. |
| ElevationApprover | Der Name eines Microsoft-Managers. |
| ElevationDuration | Die Dauer, für die die Erhöhung der Rechte aktiv war. |
| ElevationRequestId |  Ein eindeutiger Bezeichner für die Erhöhung der Rechte-Anforderung. |
| ElevationRole | Die Rolle der Erhöhung der Rechte, die für angefordert wurde. |
| ElevationTime | Die Startzeit für die Erhöhung der Rechte. |
| Start_Time | Die Startzeit für die Cmdlet-Ausführung. |


### <a name="exchange-admin"></a>Exchange-Administrator
Diese Datensätze werden erstellt, wenn Änderungen an der Exchange-Konfiguration vorgenommen werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeAdmin |
| "ExternalAccess" |  Gibt an, ob das Cmdlet von einem Benutzer in Ihrer Organisation mit Microsoft-Datencenter Mitarbeitern oder ein Datencenter-Dienstkonto oder ein delegierter Administrator ausgeführt wurde. Der Wert "false" gibt an, dass das Cmdlet von einem Benutzer in Ihrer Organisation ausgeführt wurde. Der Wert "true" gibt an, dass das Cmdlet von Datacenter-Mitarbeiter, ein Datencenter-Dienstkonto oder ein delegierter Administrator ausgeführt wurde. |
| ModifiedObjectResolvedName |  Dies ist der benutzerfreundliche Name des Objekts, das vom Cmdlet geändert wurde. Dies wird nur protokolliert, wenn das Cmdlet das Objekt ändert. |
| OrganizationName | Der Name des Mandanten. |
| OriginatingServer | Der Name des Servers, von dem das Cmdlet ausgeführt wurde. |
| Parameter | Der Name und der Wert für alle Parameter, die mit dem-Cmdlet verwendet wurden, die in der Operations-Eigenschaft identifiziert wird. |


### <a name="exchange-mailbox"></a>Exchange-Postfach
Diese Datensätze werden erstellt, wenn Änderungen oder Erweiterungen an Exchange-Postfächern vorgenommen werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| ClientInfoString | Informationen zu den e-Mail-Client, der zum Ausführen des Vorgangs, z. B. eine Browserversion, Outlook-Version und Informationen zu mobilen Geräten verwendet wurde. |
| Client_IPAddress | Die IP-Adresse des Geräts, das verwendet wurde, wenn der Vorgang protokolliert wurde. Die IP-Adresse wird im Format für eine IPv4- oder IPv6-Adresse angezeigt. |
| ClientMachineName | Der Computername, der den Outlook-Client hostet. |
| ClientProcessName | Die e-Mail-Client, der verwendet wurde, auf das Postfach zugreifen. |
| ClientVersion | Die Version des e-Mail-Clients. |
| InternalLogonType | Für die interne Verwendung reserviert. |
| Logon_Type | Gibt den Typ des Benutzers, der das Postfach zugegriffen und der ausgeführt wurde, das angemeldet war. |
| LogonUserDisplayName |    Der benutzerfreundliche Name des Benutzers, der der Vorgang ausgeführt. |
| LogonUserSid | Die SID des Benutzers, der den Vorgang ausgeführt hat. |
| MailboxGuid | Die Exchange-GUID des Postfachs, auf die zugegriffen wurde. |
| MailboxOwnerMasterAccountSid | Postfach Datenbankbesitzer-Konto Hauptkonto-SID. |
| MailboxOwnerSid | Die SID des Postfachbesitzers. |
| MailboxOwnerUPN | Die e-Mail-Adresse der Person, die das Postfach besitzt, auf die zugegriffen wurde. |


### <a name="exchange-mailbox-audit"></a>Überwachung der Exchange-Postfach
Diese Datensätze werden erstellt, wenn ein Überwachungseintrag Postfach erstellt wird.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| Element | Stellt das Element, auf dem der Vorgang ausgeführt wurde | 
| SendAsUserMailboxGuid | Die Exchange-GUID des Postfachs, auf die zugegriffen wurde, um e-Mail zu senden. |
| SendAsUserSmtp | SMTP-Adresse des Benutzers, dessen Identität angenommen wird. |
| SendonBehalfOfUserMailboxGuid | Die Exchange-GUID des Postfachs, das zum Senden von Nachrichten im Auftrag von zugegriffen wurde. |
| SendOnBehalfOfUserSmtp | SMTP-Adresse des Benutzers, der die e-Mail-Adresse gesendet wird. |


### <a name="exchange-mailbox-audit-group"></a>Die Gruppe "Exchange-Postfach-Überwachung"
Diese Datensätze werden erstellt, wenn Änderungen oder Erweiterungen an Exchange-Gruppen vorgenommen werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | Exchange |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | Informationen über jedes Element in der Gruppe. |
| CrossMailboxOperations | Gibt an, ob der Vorgang mehr als ein Postfach beteiligt. |
| DestMailboxId | Nur festgelegt, wenn der CrossMailboxOperations-Parameter auf "true" ist. Gibt die Zielpostfach-GUID an. |
| DestMailboxOwnerMasterAccountSid | Nur festgelegt, wenn der CrossMailboxOperations-Parameter auf "true" ist. Gibt die SID für die Hauptkonto-SID des Ziel-Postfachbenutzers an. |
| DestMailboxOwnerSid | Nur festgelegt, wenn der CrossMailboxOperations-Parameter auf "true" ist. Gibt die SID des Postfachs Ziel. |
| DestMailboxOwnerUPN | Nur festgelegt, wenn der CrossMailboxOperations-Parameter auf "true" ist. Gibt den UPN des Besitzers des Postfachs Ziel. |
| DestFolder | Der Zielordner für Vorgänge wie das verschieben. |
| Ordner | Der Ordner, in denen eine Gruppe von Elementen gefunden wird. |
| Ordner |     Informationen zu den Quellordnern in einem Vorgang beteiligten; Wenn z. B. Ordner ausgewählt sind, und klicken Sie dann gelöscht. |


### <a name="sharepoint-base"></a>SharePoint-Basis
Diese Eigenschaften gelten für alle SharePoint-Datensätze zur Verfügung.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| EventSource | Gibt an, dass in SharePoint ein Ereignis aufgetreten ist. Mögliche Werte sind SharePoint- oder ObjectModel. |
| itemType | Der Typ des Objekts, das Zugriff auf oder geändert wurde. Finden Sie in der Tabelle ItemType Details zu den Arten von Objekten. |
| MachineDomainInfo | Informationen zum Gerät Synchronisierungsvorgänge. Diese Informationen wird nur gemeldet, wenn sie in der Anforderung vorhanden ist. |
| MachineId |   Informationen zum Gerät Synchronisierungsvorgänge. Diese Informationen wird nur gemeldet, wenn sie in der Anforderung vorhanden ist. |
| Site_ | Die GUID des Standorts, auf dem sich die Datei oder einen Ordner zugegriffen, die vom Benutzer befindet. |
| Source_Name | Die Entität, die den überwachten Vorgang ausgelöst hat. Mögliche Werte sind SharePoint- oder ObjectModel. |
| UserAgent | Informationen zur Client- oder im Browser des Benutzers. Diese Informationen werden vom Client oder vom Browser bereitgestellt. |


### <a name="sharepoint-schema"></a>SharePoint-Schema
Diese Datensätze werden erstellt, wenn Änderungen an der Konfiguration an SharePoint vorgenommen werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | Optionale Zeichenfolge für benutzerdefinierte Ereignisse. |
| Event_Data |  Optionale Nutzlast für benutzerdefinierte Ereignisse. |
| Geänderte Eigenschaften | Die Eigenschaft bezieht sich auf Admin-Ereignisse, z. B. einen Benutzer als Mitglied einer Website oder eine Site Collection-Administratorgruppe hinzufügen. Die Eigenschaft enthält den Namen der Eigenschaft, die den neuen Wert der geänderten Eigenschaft (solche Benutzers, der als ein Standort-Administrator hinzugefügt wurde) und den vorherigen Wert des geänderten Objekts (z. B. den Website-Administratorgruppe) geändert wurde. |


### <a name="sharepoint-file-operations"></a>SharePoint-Dateivorgänge
Diese Datensätze werden als Antwort auf Dateivorgänge in SharePoint erstellt.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | Die Dateierweiterung einer Datei, die verschoben oder kopiert wird. Diese Eigenschaft wird nur für FileCopied und FileMoved Ereignisse angezeigt. |
| DestinationFileName | Der Name der Datei, die kopiert oder verschoben wird. Diese Eigenschaft wird nur für FileCopied und FileMoved Ereignisse angezeigt. |
| DestinationRelativeUrl | Die URL des Zielordners, in dem eine Datei kopiert oder verschoben wird. Die Kombination der Werte für Parameter SiteURL, DestinationRelativeURL und DestinationFileName ist identisch mit dem Wert für die ObjectID-Eigenschaft, die den vollständigen Pfadnamen für die Datei ist, die kopiert wurde. Diese Eigenschaft wird nur für FileCopied und FileMoved Ereignisse angezeigt. |
| SharingType | Der Typ der Freigabe von Berechtigungen, die dem Benutzer zugewiesen wurden, denen für die Ressource freigegeben wurde. Dieser Benutzer wird durch den Parameter UserSharedWith identifiziert. |
| Site_Url | Die URL der Website auf dem sich die Datei oder einen Ordner zugegriffen, die vom Benutzer befindet. |
| SourceFileExtension | Die Dateierweiterung der Datei, die der Benutzer zugegriffen hat. Diese Eigenschaft ist leer, wenn das Objekt, auf die zugegriffen wurde ein Ordner ist. |
| Quelldateiname |  Der Name der Datei oder des Ordners, der vom Benutzer zugegriffen werden soll. |
| SourceRelativeUrl | Die URL des Ordners, der die Datei zugegriffen wird, durch den Benutzer enthält. Die Kombination der Werte für die Parameter SiteURL SourceRelativeURL und Quelldateiname ist identisch mit dem Wert für die ObjectID-Eigenschaft, die den vollständigen Pfadnamen für die Datei, die vom Benutzer zugegriffen wird. |
| UserSharedWith |  Der Benutzer, dem für eine Ressource freigegeben wurde. |




## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen
Die folgende Tabelle enthält Beispiele für Protokollsuchen für Updatedatensätze, die mit dieser Lösung erfasst wurden.

| Abfrage | Beschreibung |
| --- | --- |
|Anzahl aller Vorgänge in Ihrem Office 365-Abonnement |"Type = OfficeActivity | "Measure Count()" "Vorgang" |
|Verwendung von SharePoint-Websites|"Type = OfficeActivity OfficeWorkload = Sharepoint | als Anzahl von SiteUrl "Measure Count()" " | Anzahl Asc sortieren "|
|Datei Zugriffsvorgänge von Benutzertyp|"Type = OfficeActivity OfficeWorkload = Sharepoint Vorgang FileAccessed = | "Measure Count()" "von UserType"|
|Suchen Sie mit einem bestimmten-Schlüsselwort|`Type=OfficeActivity OfficeWorkload=azureactivedirectory "MyTest"`|
|Monitor externe Aktionen für Exchange|`Type=OfficeActivity OfficeWorkload=exchange ExternalAccess = true`|



## <a name="troubleshooting"></a>Problembehandlung

Wenn Ihre Office 365-Lösung keine Daten erfassen ist, wie erwartet, überprüfen Sie den Status im OMS-Portal unter **Einstellungen** -> **verbundene Datenquellen** -> **Office 365**. Die folgende Tabelle beschreibt die einzelnen Status.

| Status | Beschreibung |
|:--|:--|
| Aktiv | Die Office 365-Abonnement aktiv ist, und die arbeitsauslastung wurde erfolgreich mit Ihrem OMS-Arbeitsbereich verbunden ist. |
| Ausstehend | Das Office 365-Abonnement ist aktiv, aber die arbeitsauslastung ist noch nicht verbunden mit Ihrem OMS-Arbeitsbereich erfolgreich. Verbinden von Office 365-Abonnement erstmalig werden alle Arbeitslasten auf diesen Status, bis diese erfolgreich verbunden sind. Warten Sie 24 Stunden für alle Arbeitslasten auf aktiv zu wechseln. |
| Inaktiv | Die Office 365-Abonnement wird in einen inaktiven Status auf. Überprüfen Sie Ihre Office 365-Administrator-Seite für Details. Nachdem Sie Ihr Office 365-Abonnement zu aktivieren, Ihrem OMS-Arbeitsbereich heben Sie auf, und verknüpfen Sie sie erneut, um den Empfang von Daten starten. |



## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die Protokollsuche in [Log Analytics](../log-analytics/log-analytics-log-searches.md), um ausführliche Daten zu Updates anzuzeigen.
* [Erstellen eigener Dashboards](../log-analytics/log-analytics-dashboards.md) auf die bevorzugten Office 365-Suchabfragen anzuzeigen.
* [Erstellen von Warnungen](../log-analytics/log-analytics-alerts.md) proaktiv wichtige Office 365-Aktivitäten benachrichtigt zu werden.  
