---
title: Problembehandlung und Protokollierung bei der Vorschau von Azure AD-Kennwortschutz
description: Informationen zu Protokollierung und allgemeiner Problembehandlung bei der Vorschau von Azure AD-Kennwortschutz
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 1eea6380d4276644db0c7681f23a4b0c5e79ff09
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187348"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Vorschau: Überwachung, Berichterstellung und Problembehandlung beim Azure AD-Kennwortschutz

|     |
| --- |
| Azure AD-Kennwortschutz und die benutzerdefinierte Liste der gesperrten Kennwörter sind in der öffentlichen Vorschau befindliche Features von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Nach der Bereitstellung von Azure AD-Kennwortschutz sind Überwachung und Berichterstattung wichtige Aufgaben. Dieser Artikel geht ins Detail, damit Sie verstehen, wo jeder Dienst Informationen protokolliert, und wie über die Verwendung von Azure AD-Kennwortschutz berichtet wird.

## <a name="on-premises-logs-and-events"></a>Lokale Protokolle und Ereignisse

### <a name="dc-agent-service"></a>DC-Agent-Dienst

Auf jedem Domänencontroller schreibt die DC-Agent-Dienstsoftware die Ergebnisse der Kennwortüberprüfungen (und andere Status) in ein lokales Ereignisprotokoll: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin.

Ereignisse werden durch die verschiedenen DC-Agent-Komponenten mit den folgenden Bereichen protokolliert:

|Komponente |Ereignis-ID-Bereich|
| --- | --- |
|DC-Agent-Kennwortfilter-DLL| 10000-19999|
|DC-Agent-Diensthostprozess| 20000-29999|
|Validierungslogik für DC-Agent-Dienstrichtlinie| 30000-39999|

Für eine erfolgreiche Kennwortvalidierung wird im Allgemeinen ein Ereignis aus der DC-Agent-Kennwortfilter-DLL protokolliert. Für eine nicht erfolgreiche Kennwortvalidierung werden im Allgemeinen zwei Ereignisse protokolliert, eines aus dem DC-Agent-Dienst und eines aus der DC-Agent-Kennwortfilter-DLL.

Diskrete Ereignisse zum Erfassen dieser Situationen werden basierend auf folgenden Faktoren protokolliert:

* Ob ein bestimmtes Kennwort festgelegt oder geändert wird.
* Ob die Validierung eines angegebenen Kennworts erfolgreich oder nicht erfolgreich ist.
* Ob die Validierung aufgrund eines Gegensatzes zwischen der globalen Richtlinie von Microsoft und der Organisationsrichtlinie nicht erfolgreich ist.
* Ob der ausschließliche Überwachungsmodus für die aktuelle Kennwortrichtlinie derzeit aktiviert oder deaktiviert ist.

Die wichtigsten auf die Kennwortüberprüfung bezogenen Ereignisse sind wie folgt:

|   |Kennwortänderung |Kennwortfestlegung|
| --- | :---: | :---: |
|Pass |10014 |10015|
|Fehler (Kundenkennwortrichtlinie nicht bestanden)| 10016, 30002| 10017, 30003|
|Fehler (Kennwortrichtlinie von Microsoft nicht bestanden)| 10016, 30004| 10017, 30005|
|Im ausschließlichen Überwachungsmodus bestanden (würde Kundenkennwortrichtlinie nicht bestehen)| 10024, 30008| 10025, 30007|
|Im ausschließlichen Überwachungsmodus bestanden (würde Kennwortrichtlinie von Microsoft nicht bestehen)| 10024, 30010| 10025, 30009|

> [!TIP]
> Eingehende Kennwörter werden zuerst anhand der Liste global gesperrter Kennwörter von Microsoft überprüft; wenn dies nicht erfolgreich ist, wird keine weitere Verarbeitung ausgeführt. Das gleiche Verhalten wird für Kennwortänderungen in Azure ausgeführt.

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Beispiel einer Ereignisprotokollmeldung für Ereignis-ID 10014 – Kennwort erfolgreich festgelegt

Das geänderte Kennwort für den angegebenen Benutzer wurde als konform mit der aktuellen Azure-Kennwortrichtlinie validiert.

 UserName: BPL_02885102771 FullName:

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Beispiel einer Ereignisprotokollmeldung für Ereignis-ID 10017 und 30003 – Kennwort nicht erfolgreich festgelegt

10017:

Das zurückgesetzte Kennwort für den angegebenen Benutzer wurde zurückgewiesen, weil es nicht mit der aktuellen Azure-Kennwortrichtlinie übereinstimmt. Weitere Details finden Sie in der korrelierten Ereignisprotokollmeldung.

 UserName: BPL_03283841185 FullName:

30003:

Das Zurücksetzen des Kennworts für den angegebenen Benutzer wurde zurückgewiesen, da es mit mindestens einem der Tokens in der Liste pro Mandant gesperrter Kennwörter der aktuellen Azure-Kennwortrichtlinie übereinstimmte.

 UserName: BPL_03283841185 FullName:

Einige andere wichtige, zu berücksichtigende Ereignisprotokollmeldungen sind:

#### <a name="sample-event-log-message-for-event-id-30001"></a>Beispiel einer Ereignisprotokollmeldung für Ereignis-ID 30001

Das Kennwort für den angegebenen Benutzer wurde angenommen, da eine Azure-Kennwortrichtlinie noch nicht verfügbar ist.

UserName: <user> FullName: <user>

Für diese Bedingung kommen eine oder mehrere der folgenden Ursachen infrage:%n.

1. Die Gesamtstruktur wurde noch nicht in Azure registriert.

   Lösungsschritte: Ein Administrator muss die Gesamtstruktur mit dem Cmdlet „Register-AzureADPasswordProtectionForest“ registrieren.

2. Ein Azure AD-Kennwortschutzproxy ist auf mindestens einem Computer in der aktuellen Gesamtstruktur noch nicht verfügbar.

   Lösungsschritte: Ein Administrator muss einen Proxy mit dem Cmdlet „Register-AzureADPasswordProtectionProxy“ installieren und registrieren.

3. Dieser Domänencontroller verfügt nicht über Netzwerkkonnektivität mit Azure AD-Kennwortschutzproxy-Instanzen.

   Lösungsschritte: Stellen Sie sicher, dass Netzwerkkonnektivität mit mindestens einer Azure AD-Kennwortschutzproxy-Instanz besteht.

4. Dieser Domänencontroller verfügt nicht über Konnektivität mit anderen Domänencontrollern in der Domäne.

   Lösungsschritte: Stellen Sie sicher, dass Netzwerkkonnektivität mit der Domäne besteht.

#### <a name="sample-event-log-message-for-event-id-30006"></a>Beispiel einer Ereignisprotokollmeldung für Ereignis-ID 30006

Der Dienst erzwingt jetzt die Durchsetzung der folgenden Azure-Kennwortrichtlinie.

 AuditOnly: 1

 Datum der globalen Richtlinie: 2018-05-15T00:00:00.000000000Z

 Datum der Mandantenrichtlinie: 2018-06-10T20:15:24.432457600Z

 Mandantenrichtlinie erzwingen: 1

#### <a name="dc-agent-log-locations"></a>DC-Agent-Protokollspeicherorte

Der DC-Agent-Dienst protokolliert auch vorgangsbezogene Ereignisse in folgendem Protokoll: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational.

Der DC-Agent-Dienst kann auch ausführlich auf Debugebene Ereignisse in folgendem Protokoll protokollieren: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace.

> [!WARNING]
> Das Ablaufverfolgungsprotokoll ist standardmäßig deaktiviert. Wenn aktiviert, empfängt dieses Protokoll eine große Anzahl von Ereignissen und kann sich auf die Leistung des Domänencontrollers auswirken. Aus diesem Grund sollte dieses verbesserte Protokoll nur aktiviert werden, wenn ein Problem genauere Untersuchung erfordert, und dann nur für einen minimalen Zeitraum.

### <a name="azure-ad-password-protection-proxy-service"></a>Azure AD-Kennwortschutz-Proxydienst

Der Kennwortschutz-Proxydienst gibt einen minimalen Satz von Ereignissen in folgendem Ereignisprotokoll aus: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational

Der Kennwortschutz-Proxydienst kann auch ausführlich auf Debugebene Ereignisse in folgendem Protokoll protokollieren: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace.

> [!WARNING]
> Das Ablaufverfolgungsprotokoll ist standardmäßig deaktiviert. Wenn aktiviert, empfängt dieses Protokoll eine große Anzahl von Ereignissen, und dies kann sich auf die Leistung des Proxyhosts auswirken. Aus diesem Grund sollte dieses Protokoll nur aktiviert werden, wenn ein Problem genauere Untersuchung erfordert, und dann nur für einen minimalen Zeitraum.

### <a name="dc-agent-discovery"></a>DC-Agent-Ermittlung

Mit dem `Get-AzureADPasswordProtectionDCAgent`-Cmdlet können grundlegende Informationen über die verschiedenen DC-Agents angezeigt werden, die in einer Domäne oder Gesamtstruktur ausgeführt werden. Diese Informationen werden aus den serviceConnectionPoint-Objekten abgerufen, die von den ausgeführten DC-Agent-Diensten registriert werden. Eine Beispielausgabe dieses Cmdlets lautet folgendermaßen:

```
PS C:\> Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
Heartbeat             : 2/16/2018 8:35:01 AM
```

Die verschiedenen Eigenschaften werden von jedem DC-Agent-Dienst ungefähr stündlich aktualisiert. Die Daten unterliegen nach wie vor der Replikationswartezeit von Active Directory.

Der Bereich der Cmdlet-Abfrage wird möglicherweise durch die Verwendung entweder des Parameters „–Forest“ oder „–Domain“ beeinflusst.

### <a name="emergency-remediation"></a>Notfallbereinigung

Wenn eine ungünstige Situation eintritt, in der der DC-Agent-Dienst Probleme verursacht, kann der DC-Agent-Dienst sofort heruntergefahren werden. Die DC-Agent-Kennwortfilter-DLL versucht, den nicht ausgeführten Dienst aufzurufen, und protokolliert die Warnungsereignisse (10012, 10013), aber alle eingehenden Kennwörter werden während dieser Zeit akzeptiert. Der DC-Agent-Dienst kann dann auch bei Bedarf über den Dienststeuerungs-Manager von Windows mit dem Starttyp „Disabled“ konfiguriert werden.

### <a name="performance-monitoring"></a>Leistungsüberwachung

Die DC-Agent-Dienstsoftware installiert ein Leistungsindikatorobjekt mit dem Namen **Azure AD password protection**. Die folgenden Leistungsindikatoren sind derzeit verfügbar:

|Name des Leistungsindikators | BESCHREIBUNG|
| --- | --- |
|Verarbeitete Kennwörter |Dieser Leistungsindikator gibt die Gesamtanzahl der verarbeiteten Kennwörter (akzeptiert oder abgelehnt) seit dem letzten Neustart an.|
|Akzeptierte Kennwörter |Dieser Leistungsindikator gibt die Gesamtanzahl der Kennwörter an, die seit dem letzten Neustart akzeptiert wurden.|
|Abgelehnte Kennwörter |Dieser Leistungsindikator gibt die Gesamtanzahl der Kennwörter an, die seit dem letzten Neustart abgelehnt wurden.|
|Laufende Kennwortfilteranforderungen |Dieser Leistungsindikator zeigt die Anzahl der laufenden Kennwortfilteranforderungen an.|
|Spitzenwert der Kennwortfilteranforderungen |Dieser Leistungsindikator zeigt den Spitzenwert gleichzeitiger Kennwortfilteranforderungen seit dem letzten Neustart an.|
|Kennwortfilteranforderungen-Fehler |Dieser Leistungsindikator gibt die Gesamtzahl der Kennwortfilteranforderungen an, die seit dem letzten Neustart aufgrund eines Fehlers nicht erfolgreich waren. Fehler können auftreten, wenn der DC-Agent-Dienst des Azure AD-Kennwortschutzes nicht ausgeführt wird.|
|Kennwortfilteranforderungen/s |Dieser Leistungsindikator zeigt die Rate an, mit der Kennwörter verarbeitet werden.|
|Verarbeitungszeit der Kennwortfilteranforderung |Dieser Leistungsindikator zeigt die zum Verarbeiten einer Kennwortfilteranforderung durchschnittlich erforderliche Zeit an.|
|Spitzenwert der Verarbeitungszeit der Kennwortfilteranforderung |Dieser Leistungsindikator zeigt den Spitzenwert der Verarbeitungszeit der Kennwortfilteranforderung seit dem letzten Neustart an.|
|Aufgrund des Überwachungsmodus akzeptierte Kennwörter |Dieser Leistungsindikator zeigt die Gesamtzahl der Kennwörter (seit dem letzten Neustart) an, die normalerweise abgelehnt worden wären, jedoch akzeptiert wurden, da die Kennwortrichtlinie für den Überwachungsmodus konfiguriert wurde.|

## <a name="directory-services-repair-mode"></a>Reparaturmodus für Verzeichnisdienste

Wenn der Domänencontroller im Reparaturmodus für Verzeichnisdienste gestartet wird, erkennt der DC-Agent-Dienst, dass dies unabhängig von der derzeit aktiven Richtlinienkonfiguration die Deaktivierung aller Kennwortüberprüfungs- oder Erzwingungsaktivitäten zur Folge hat.

## <a name="domain-controller-demotion"></a>Herabstufung des Domänencontrollers

Dies wird unterstützt, um einen Domänencontroller herabzustufen, der noch die DC-Agent-Software ausführt. Administratoren sollten jedoch bedenken, dass die DC-Agent-Software weiterhin ausgeführt wird und weiterhin während der Herabstufung die Durchsetzung der aktuellen Kennwortrichtlinie erzwingt. Das neue lokale Kennwort des Administratorkontos (angegeben als Teil des Herabstufungsvorgangs) wird wie jedes andere Kennwort überprüft. Microsoft empfiehlt, als Teil eines DC-Herabstufungsvorgangs sichere Kennwörter für lokale Administratorkonten auszuwählen. Jedoch kann die Überprüfung des neuen lokalen Kennworts des Administratorkontos durch die DC-Agent-Software bereits vorhandene Herabstufungsvorgänge stören.
Wenn die Herabstufung erfolgreich abgeschlossen, der Domänencontroller neu gestartet wurde und wieder als normaler Mitgliedsserver ausgeführt wird, kehrt die DC-Agent-Software zur Ausführung im passiven Modus zurück. Sie kann dann zu einem beliebigen Zeitpunkt deinstalliert werden.

## <a name="removal"></a>Entfernen

Wenn die Software der öffentlichen Vorschau deinstalliert und jeder zugehörige Status in Domänen und Gesamtstruktur bereinigt werden soll, kann diese Aufgabe mit den folgenden Schritten erzielt werden:

> [!IMPORTANT]
> Es ist wichtig, diese Schritte der Reihe nach auszuführen. Wenn die Ausführung einer Instanz des Kennwortschutz-Proxydiensts beibehalten wird, erstellt sie in regelmäßigen Abständen sowohl ihr serviceConnectionPoint-Objekt als auch den SYSVOL-Status neu.

1. Deinstallieren Sie die Kennwortschutz-Proxysoftware von allen Computern. Dieser Schritt erfordert **keinen** Neustart.
2. Deinstallieren Sie die DC-Agent-Software von allen Domänencontrollern. Dieser Schritt **erfordert** einen Neustart.
3. Entfernen Sie manuell alle Proxydienst-Verbindungspunkte in jedem Domänennamenkontext. Der Speicherort dieser Objekte kann möglicherweise mit dem folgenden Active Directory-Powershell-Befehl erkannt werden:
   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{EBEFB703-6113-413D-9167-9F8DD4D24468}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Lassen Sie nicht das Sternchen („*“) am Ende des Variablenwerts „$keywords“ aus.

   Das über den `Get-ADObject`-Befehl gefundene resultierende Objekt kann dann an `Remove-ADObject` weitergereicht oder manuell gelöscht werden. 

4. Entfernen Sie manuell alle DC-Agent-Verbindungspunkte in jedem Domänennamenkontext. Es gibt möglicherweise eines dieser Objekte pro Domänencontroller in der Gesamtstruktur, je nachdem, wie umfassend die Software der öffentlichen Vorschau bereitgestellt wurde. Der Speicherort dieser Objekte kann möglicherweise mit dem folgenden Active Directory-Powershell-Befehl erkannt werden:

   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{B11BB10A-3E7D-4D37-A4C3-51DE9D0F77C9}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Das über den `Get-ADObject`-Befehl gefundene resultierende Objekt kann dann an `Remove-ADObject` weitergereicht oder manuell gelöscht werden.

5. Entfernen Sie manuell den Konfigurationsstatus auf Gesamtstrukturebene. Der Konfigurationsstatus der Gesamtstruktur wird in einem Container im Active Directory-Konfigurationsnamenskontext beibehalten. Er kann wie folgt ermittelt und gelöscht werden:

   ```
   $passwordProtectonConfigContainer = "CN=Azure AD password protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject $passwordProtectonConfigContainer
   ```

6. Entfernen Sie manuell den gesamten auf SYSVOL bezogenen Status durch manuelles Löschen des folgenden Ordners und seines gesamten Inhalts:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Falls erforderlich, kann auch lokal auf einem bestimmten Domänencontroller auf diesen Pfad zugegriffen werden; der Standardspeicherort entspräche etwa dem folgenden Pfad:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Dieser Pfad ist anders, wenn die SYSVOL-Freigabe an einem nicht standardmäßigen Speicherort konfiguriert wurde.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu der Liste global gesperrter und benutzerdefinierter gesperrter Kennwörter finden Sie im Artikel [Beseitigen falscher Kennwörter in Ihrer Organisation](concept-password-ban-bad.md)
