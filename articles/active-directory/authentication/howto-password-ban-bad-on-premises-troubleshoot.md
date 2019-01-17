---
title: Problembehandlung und Protokollierung bei der Vorschau von Azure AD-Kennwortschutz
description: Informationen zu Protokollierung und allgemeiner Problembehandlung bei der Vorschau von Azure AD-Kennwortschutz
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: d9f3ba642a5d00594aa6bdef597d5db43c2fc121
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188979"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Vorschau: Überwachung, Berichterstellung und Problembehandlung beim Azure AD-Kennwortschutz

|     |
| --- |
| Azure AD-Kennwortschutz ist eine öffentliche Vorschaufunktion für Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Nach der Bereitstellung des Azure AD-Kennwortschutzes sind Überwachung und Berichterstellung wichtige Aufgaben. Dieser Artikel geht ins Detail, damit Sie verstehen, wo jeder Dienst Informationen protokolliert, und wie über die Verwendung von Azure AD-Kennwortschutz berichtet wird.

## <a name="on-premises-logs-and-events"></a>Lokale Protokolle und Ereignisse

### <a name="dc-agent-admin-log"></a>DC-Agent-Administratorprotokoll

Auf jedem Domänencontroller schreibt die DC-Agent-Dienstsoftware die Ergebnisse der Kennwortüberprüfungen (und andere Status) in ein lokales Ereignisprotokoll:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin`

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

```
The changed password for the specified user was validated as compliant with the current Azure password policy.

 UserName: BPL_02885102771
 FullName:
```

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Beispiel einer Ereignisprotokollmeldung für Ereignis-ID 10017 und 30003 – Kennwort nicht erfolgreich festgelegt

10017:

```
The reset password for the specified user was rejected because it did not comply with the current Azure password policy. Please see the correlated event log message for more details.

 UserName: BPL_03283841185
 FullName:
```

30003:

```
The reset password for the specified user was rejected because it matched at least one of the tokens present in the per-tenant banned password list of the current Azure password policy.

 UserName: BPL_03283841185
 FullName:
```

#### <a name="sample-event-log-message-for-event-id-30001-password-accepted-due-to-no-policy-available"></a>Beispiel einer Ereignisprotokollmeldung für Ereignis-ID 30001 – Kennwort akzeptiert, da keine Richtlinie verfügbar

```
The password for the specified user was accepted because an Azure password policy is not available yet

UserName: SomeUser
FullName: Some User

This condition may be caused by one or more of the following reasons:%n

1. The forest has not yet been registered with Azure.

   Resolution steps: an administrator must register the forest using the Register-AzureADPasswordProtectionForest cmdlet.

2. An Azure AD password protection Proxy is not yet available on at least one machine in the current forest.

   Resolution steps: an administrator must install and register a proxy using the Register-AzureADPasswordProtectionProxy cmdlet.

3. This DC does not have network connectivity to any Azure AD password protection Proxy instances.

   Resolution steps: ensure network connectivity exists to at least one Azure AD password protection Proxy instance.

4. This DC does not have connectivity to other domain controllers in the domain.

   Resolution steps: ensure network connectivity exists to the domain.
```

#### <a name="sample-event-log-message-for-event-id-30006-new-policy-being-enforced"></a>Beispiel einer Ereignisprotokollmeldung für Ereignis-ID 30006 – neue Richtlinie wird erzwungen

```
The service is now enforcing the following Azure password policy.

 Enabled: 1
 AuditOnly: 1
 Global policy date: ‎2018‎-‎05‎-‎15T00:00:00.000000000Z
 Tenant policy date: ‎2018‎-‎06‎-‎10T20:15:24.432457600Z
 Enforce tenant policy: 1
```

#### <a name="dc-agent-operational-log"></a>DC-Agent-Betriebsprotokoll

Der DC-Agent-Dienst protokolliert auch betriebsbedingte Ereignisse im folgenden Protokoll:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

#### <a name="dc-agent-trace-log"></a>DC-Agent-Ablaufprotokoll

Der DC-Agent-Dienst kann auch ausführliche Ablaufverfolgungsereignisse auf Debugebene im folgenden Protokoll protokollieren:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

Das Ablaufprotokoll ist standardmäßig deaktiviert.

> [!WARNING]
>  Wenn es aktiviert ist, empfängt es eine große Anzahl von Ereignissen. Das kann sich auf die Leistung des Domänencontrollers auswirken. Aus diesem Grund sollte dieses verbesserte Protokoll nur aktiviert werden, wenn ein Problem genauere Untersuchung erfordert, und dann nur für einen minimalen Zeitraum.

#### <a name="dc-agent-text-logging"></a>DC-Agent-Textprotokoll

Der DC-Agent-Dienst kann zum Schreiben in ein Textprotokoll konfiguriert werden, indem der folgende Registrierungswert festgelegt wird:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionDCAgent\Parameters!EnableTextLogging = 1 (REG_DWORD-Wert)

Das Textprotokoll ist standardmäßig deaktiviert. Damit Änderungen an diesem Wert wirksam werden, ist ein Neustart des DC-Agent-Diensts erforderlich. Wenn der Dienst aktiviert ist, schreibt er in eine Protokolldatei unter folgendem Pfad:

`%ProgramFiles%\Azure AD Password Protection DC Agent\Logs`

> [!TIP]
> Das Textprotokoll empfängt die gleichen Einträge auf Debugebene, die im Ablaufprotokoll protokolliert werden können, hat jedoch i.d.R. ein zur Überprüfung und Analyse besser geeignetes Format.

> [!WARNING]
> Wenn aktiviert, empfängt dieses Protokoll eine große Anzahl von Ereignissen und kann sich auf die Leistung des Domänencontrollers auswirken. Aus diesem Grund sollte dieses verbesserte Protokoll nur aktiviert werden, wenn ein Problem genauere Untersuchung erfordert, und dann nur für einen minimalen Zeitraum.

### <a name="azure-ad-password-protection-proxy-service"></a>Azure AD-Kennwortschutz-Proxydienst

#### <a name="proxy-service-event-logs"></a>Proxydienst-Ereignisprotokolle

Der Proxydienst sendet einen Mindestsatz von Ereignissen an die folgenden Ereignisprotokolle:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational`

Der Proxydienst kann auch ausführliche Ablaufverfolgungsereignisse auf Debugebene im folgenden Protokoll protokollieren:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace`

Das Ablaufprotokoll ist standardmäßig deaktiviert.

> [!WARNING]
> Wenn es aktiviert ist, empfängt es eine große Anzahl von Ereignissen. Das kann sich auf die Leistung des Proxyhosts auswirken. Aus diesem Grund sollte dieses Protokoll nur aktiviert werden, wenn ein Problem genauere Untersuchung erfordert, und dann nur für einen minimalen Zeitraum.

#### <a name="proxy-service-text-logging"></a>Proxydienst-Textprotokoll

Der Proxydienst kann zum Schreiben in ein Textprotokoll konfiguriert werden, indem der folgende Registrierungswert festgelegt wird:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionProxy\Parameters!EnableTextLogging = 1 (REG_DWORD-Wert)

Das Textprotokoll ist standardmäßig deaktiviert. Damit Änderungen an diesem Wert wirksam werden, ist ein Neustart des Proxydiensts erforderlich. Wenn der Proxydienst aktiviert ist, schreibt er in eine Protokolldatei unter folgendem Pfad:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

> [!TIP]
> Das Textprotokoll empfängt die gleichen Einträge auf Debugebene, die im Ablaufprotokoll protokolliert werden können, hat jedoch i.d.R. ein zur Überprüfung und Analyse besser geeignetes Format.

> [!WARNING]
> Wenn aktiviert, empfängt dieses Protokoll eine große Anzahl von Ereignissen und kann sich auf die Leistung des Domänencontrollers auswirken. Aus diesem Grund sollte dieses verbesserte Protokoll nur aktiviert werden, wenn ein Problem genauere Untersuchung erfordert, und dann nur für einen minimalen Zeitraum.

#### <a name="powershell-cmdlet-logging"></a>PowerShell-Cmdlet-Protokoll

Die meisten PowerShell-Cmdlets des Azure AD-Kennwortschutzes schreiben in ein Textprotokoll unter folgendem Pfad:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

Wenn ein Cmdlet-Fehler auftritt und sich die Ursache und/oder Lösung nicht direkt ermitteln lässt, können Sie auch diese Textprotokolle anzeigen.

### <a name="emergency-remediation"></a>Notfallbereinigung

Wenn der DC-Agent-Dienst Probleme verursacht, kann er sofort heruntergefahren werden. Die DC-Agent-Kennwortfilter-DLL versucht weiterhin, den nicht ausgeführten Dienst aufzurufen, und protokolliert die Warnungsereignisse (10012, 10013), aber alle eingehenden Kennwörter werden währenddessen akzeptiert. Der DC-Agent-Dienst kann dann auch bei Bedarf über den Dienststeuerungs-Manager von Windows mit dem Starttyp „Disabled“ konfiguriert werden.

### <a name="performance-monitoring"></a>Leistungsüberwachung

Die DC-Agent-Dienstsoftware installiert ein Leistungsindikatorobjekt mit dem Namen **Azure AD-Kennwortschutz**. Die folgenden Leistungsindikatoren sind derzeit verfügbar:

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
> Es ist wichtig, diese Schritte der Reihe nach auszuführen. Wenn eine Instanz des Proxydiensts weiterhin ausgeführt wird, wird sie ihr „serviceConnectionPoint“-Objekt regelmäßig neu erstellen. Wenn eine Instanz des DC-Agent-Diensts weiterhin ausgeführt wird, wird sie ihr „serviceConnectionPoint“-Objekt und den „sysvol“-Status regelmäßig neu erstellen.

1. Deinstallieren Sie die Kennwortschutz-Proxysoftware von allen Computern. Dieser Schritt erfordert **keinen** Neustart.
2. Deinstallieren Sie die DC-Agent-Software von allen Domänencontrollern. Dieser Schritt **erfordert** einen Neustart.
3. Entfernen Sie manuell alle Proxydienst-Verbindungspunkte in jedem Domänennamenkontext. Der Speicherort dieser Objekte kann möglicherweise mit dem folgenden Active Directory-Powershell-Befehl erkannt werden:

   ```Powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Lassen Sie nicht das Sternchen („*“) am Ende des Variablenwerts „$keywords“ aus.

   Die mit dem Befehl `Get-ADObject` gefundenen Ergebnisobjekte können dann an `Remove-ADObject` weitergeleitet oder manuell gelöscht werden. 

4. Entfernen Sie manuell alle DC-Agent-Verbindungspunkte in jedem Domänennamenkontext. Es gibt möglicherweise eines dieser Objekte pro Domänencontroller in der Gesamtstruktur, je nachdem, wie umfassend die Software der öffentlichen Vorschau bereitgestellt wurde. Der Speicherort dieser Objekte kann möglicherweise mit dem folgenden Active Directory-Powershell-Befehl erkannt werden:

   ```Powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Die mit dem Befehl `Get-ADObject` gefundenen Ergebnisobjekte können dann an `Remove-ADObject` weitergeleitet oder manuell gelöscht werden.

5. Entfernen Sie manuell den Konfigurationsstatus auf Gesamtstrukturebene. Der Konfigurationsstatus der Gesamtstruktur wird in einem Container im Active Directory-Konfigurationsnamenskontext beibehalten. Er kann wie folgt ermittelt und gelöscht werden:

   ```Powershell
   $passwordProtectonConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject -Recursive $passwordProtectonConfigContainer
   ```

6. Entfernen Sie manuell den gesamten auf SYSVOL bezogenen Status durch manuelles Löschen des folgenden Ordners und seines gesamten Inhalts:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Falls erforderlich, kann auch lokal auf einem bestimmten Domänencontroller auf diesen Pfad zugegriffen werden; der Standardspeicherort entspräche etwa dem folgenden Pfad:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Dieser Pfad ist anders, wenn die SYSVOL-Freigabe an einem nicht standardmäßigen Speicherort konfiguriert wurde.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu der Liste global gesperrter und benutzerdefinierter gesperrter Kennwörter finden Sie im Artikel [Beseitigen falscher Kennwörter in Ihrer Organisation](concept-password-ban-bad.md)
