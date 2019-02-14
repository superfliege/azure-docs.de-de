---
title: Problembehandlung beim Azure AD-Kennwortschutz (Vorschauversion)
description: Informationen zur allgemeinen Problembehandlung beim Azure AD-Kennwortschutz (Vorschauversion)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5727965373752d40e3ce508c1bc79046c2b3b70b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56177750"
---
# <a name="preview-azure-ad-password-protection-troubleshooting"></a>Vorschau: Problembehandlung beim Azure AD-Kennwortschutz

|     |
| --- |
| Der Azure AD-Kennwortschutz ist als öffentliche Vorschaufunktion für Azure Active Directory verfügbar. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Nach der Bereitstellung des Azure AD-Kennwortschutzes ist möglicherweise eine Problembehandlung erforderlich. In diesem Artikel werden einige allgemeine Problembehandlungsschritte ausführlich beschrieben, damit Sie diese besser verstehen können.

## <a name="weak-passwords-are-not-getting-rejected-as-expected"></a>Unsichere Kennwörter werden nicht wie erwartet abgelehnt

Dies kann mehrere mögliche Ursachen haben:

1. Der oder die DC-Agents haben noch keine Richtlinie heruntergeladen. Anzeichen hierfür sind 30001-Ereignisse im Admin-Ereignisprotokoll des DC-Agents.

    Mögliche Ursachen für dieses Problem:

    1. Die Gesamtstruktur ist noch nicht registriert.
    2. Der Proxy ist noch nicht registriert.
    3. Aufgrund von Problemen mit der Netzwerkkonnektivität kann der Proxydienst nicht mit Azure kommunizieren (überprüfen Sie die HTTP-Proxy-Anforderungen).

2. Der Erwingungsmodus der Kennwortrichtlinie ist noch auf „Überwachung“ eingestellt. Wenn dies der Fall ist, konfigurieren Sie den Modus im Azure AD-Kennwortschutzportal neu, und legen Sie „Erzwingen“ fest. Informationen hierzu finden Sie unter [Aktivieren des Kennwortschutzes](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

3. Die Kennwortrichtlinie wurde deaktiviert. Wenn dies der Fall ist, konfigurieren Sie die Kennwortrichtlinie im Azure AD-Kennwortschutzportal erneut, und aktivieren Sie sie. Informationen hierzu finden Sie unter [Aktivieren des Kennwortschutzes](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

4. Der Kennwortüberprüfungsalgorithmus funktioniert möglicherweise erwartungsgemäß. Informationen hierzu finden Sie unter [Auswerten von Kennwörtern](concept-password-ban-bad.md#how-are-passwords-evaluated).

## <a name="directory-services-repair-mode"></a>Reparaturmodus für Verzeichnisdienste

Wenn der Domänencontroller im Reparaturmodus für Verzeichnisdienste gestartet wird, erkennt dies der DC-Agent-Dienst und veranlasst, unabhängig von der derzeit aktiven Richtlinienkonfiguration, die Deaktivierung aller Kennwortüberprüfungs- oder -erzwingungsaktivitäten.

## <a name="emergency-remediation"></a>Notfallbereinigung

Wenn der DC-Agent-Dienst Probleme verursacht, kann er sofort heruntergefahren werden. Die DC-Agent-Kennwortfilter-DLL versucht weiterhin, den nicht ausgeführten Dienst aufzurufen, und protokolliert die Warnungsereignisse (10012, 10013), aber alle eingehenden Kennwörter werden währenddessen akzeptiert. Der DC-Agent-Dienst kann dann auch bei Bedarf über den Dienststeuerungs-Manager von Windows mit dem Starttyp „Disabled“ konfiguriert werden.

Als weitere Problembehandlungsmaßnahme könnten Sie im Azure AD-Kennwortschutzportal den Aktivierungmodus auf „Nein“ festlegen. Nachdem die aktualisierte Richtlinie heruntergeladen wurde, wechseln alle DC-Agent-Dienste in einen Ruhemodus, in dem alle Kennwörter unverändert akzeptiert werden. Weitere Informationen finden Sie unter [Erzwingungsmodus](howto-password-ban-bad-on-premises-operations.md#enforce-mode).

## <a name="domain-controller-demotion"></a>Herabstufung des Domänencontrollers

Dies wird unterstützt, um einen Domänencontroller herabzustufen, der noch die DC-Agent-Software ausführt. Administratoren sollten jedoch bedenken, dass die DC-Agent-Software während der Herabstufung weiterhin die Durchsetzung der aktuellen Kennwortrichtlinie erzwingt. Das neue lokale Kennwort des Administratorkontos (angegeben als Teil des Herabstufungsvorgangs) wird wie jedes andere Kennwort überprüft. Microsoft empfiehlt, als Teil eines DC-Herabstufungsvorgangs sichere Kennwörter für lokale Administratorkonten auszuwählen. Jedoch kann die Überprüfung des neuen lokalen Kennworts des Administratorkontos durch die DC-Agent-Software bereits vorhandene Herabstufungsvorgänge stören.

Wenn die Herabstufung erfolgreich abgeschlossen, der Domänencontroller neu gestartet wurde und wieder als normaler Mitgliedsserver ausgeführt wird, kehrt die DC-Agent-Software zur Ausführung im passiven Modus zurück. Sie kann dann zu einem beliebigen Zeitpunkt deinstalliert werden.

## <a name="removal"></a>Entfernen

Wenn die Software der öffentlichen Vorschau deinstalliert und jeder zugehörige Status in Domänen und Gesamtstruktur bereinigt werden soll, kann diese Aufgabe mit den folgenden Schritten erzielt werden:

> [!IMPORTANT]
> Es ist wichtig, diese Schritte der Reihe nach auszuführen. Wenn eine Instanz des Proxydiensts weiterhin ausgeführt wird, wird sie ihr „serviceConnectionPoint“-Objekt regelmäßig neu erstellen. Wenn eine Instanz des DC-Agent-Diensts weiterhin ausgeführt wird, wird sie ihr „serviceConnectionPoint“-Objekt und den „sysvol“-Status regelmäßig neu erstellen.

1. Deinstallieren Sie die Proxysoftware von allen Computern. Dieser Schritt erfordert **keinen** Neustart.
2. Deinstallieren Sie die DC-Agent-Software von allen Domänencontrollern. Dieser Schritt **erfordert** einen Neustart.
3. Entfernen Sie manuell alle Proxydienst-Verbindungspunkte in jedem Domänennamenkontext. Der Speicherort dieser Objekte kann möglicherweise mit dem folgenden Active Directory-PowerShell-Befehl erkannt werden:

   ```PowerShell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Lassen Sie nicht das Sternchen („*“) am Ende des Variablenwerts „$keywords“ aus.

   Die mit dem Befehl `Get-ADObject` gefundenen Ergebnisobjekte können dann an `Remove-ADObject` weitergeleitet oder manuell gelöscht werden. 

4. Entfernen Sie manuell alle DC-Agent-Verbindungspunkte in jedem Domänennamenkontext. Es gibt möglicherweise eines dieser Objekte pro Domänencontroller in der Gesamtstruktur, je nachdem, wie umfassend die Software der öffentlichen Vorschau bereitgestellt wurde. Der Speicherort dieser Objekte kann möglicherweise mit dem folgenden Active Directory-PowerShell-Befehl erkannt werden:

   ```PowerShell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Die mit dem Befehl `Get-ADObject` gefundenen Ergebnisobjekte können dann an `Remove-ADObject` weitergeleitet oder manuell gelöscht werden.

   Lassen Sie nicht das Sternchen („*“) am Ende des Variablenwerts „$keywords“ aus.

5. Entfernen Sie manuell den Konfigurationsstatus auf Gesamtstrukturebene. Der Konfigurationsstatus der Gesamtstruktur wird in einem Container im Active Directory-Konfigurationsnamenskontext beibehalten. Er kann wie folgt ermittelt und gelöscht werden:

   ```PowerShell
   $passwordProtectionConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject -Recursive $passwordProtectionConfigContainer
   ```

6. Entfernen Sie manuell den gesamten auf SYSVOL bezogenen Status durch manuelles Löschen des folgenden Ordners und seines gesamten Inhalts:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   Falls erforderlich, kann auch lokal auf einem bestimmten Domänencontroller auf diesen Pfad zugegriffen werden; der Standardspeicherort entspräche etwa dem folgenden Pfad:

   `%windir%\sysvol\domain\Policies\AzureADPasswordProtection`

   Dieser Pfad ist anders, wenn die SYSVOL-Freigabe an einem nicht standardmäßigen Speicherort konfiguriert wurde.

## <a name="next-steps"></a>Nächste Schritte

[Häufig gestellte Fragen zum Azure AD-Kennwortschutz](howto-password-ban-bad-on-premises-faq.md)

Weitere Informationen zu der Liste global gesperrter und benutzerdefinierter gesperrter Kennwörter finden Sie im Artikel [Beseitigen falscher Kennwörter in Ihrer Organisation](concept-password-ban-bad.md)
