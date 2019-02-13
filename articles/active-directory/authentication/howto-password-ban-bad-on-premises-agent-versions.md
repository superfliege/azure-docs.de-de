---
title: Verlauf der Versionsveröffentlichungen des lokalen Azure AD-Kennwortschutz-Agents
description: Dokumentiert den Verlauf der Versionsveröffentlichungen und Verhaltensänderungen.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: bcf5176728b520cae5d31750384f316efe244b7e
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2019
ms.locfileid: "55663620"
---
# <a name="preview--azure-ad-password-protection-agent-version-history"></a>Vorschau:  Versionsverlauf des Azure AD-Kennwortschutz-Agents

|     |
| --- |
| Azure AD-Kennwortschutz ist eine öffentliche Previewfunktion von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="12650"></a>1.2.65.0

Veröffentlichungsdatum: 1.2.2019

Änderungen:

* DC-Agent und Proxydienst werden jetzt unter Server Core unterstützt. Die Mindestanforderungen an das Betriebssystem sind unverändert: Windows Server 2012 für DC-Agents und Windows Server 2012 R2 für Proxys.
* Die Cmdlets Register-AzureADPasswordProtectionProxy und Register-AzureADPasswordProtectionForest unterstützen jetzt gerätecodebasierte Azure-Authentifizierungsmodi.
* Das Cmdlet Get-AzureADPasswordProtectionDCAgent ignoriert fehlerhafte und/oder ungültige Dienstverbindungspunkte. Dies behebt den Fehler, bei dem Domänencontroller manchmal mehrmals in der Ausgabe aufgeführt wurden.
* Das Cmdlet Get-AzureADPasswordProtectionSummaryReport ignoriert fehlerhafte und/oder ungültige Dienstverbindungspunkte. Dies behebt den Fehler, bei dem Domänencontroller manchmal mehrmals in der Ausgabe aufgeführt wurden.
* Das Proxy-PowerShell-Modul wird jetzt von „%ProgramFiles%\WindowsPowerShell\Modules“ registriert. Die Umgebungsvariable PSModulePath des Computers wird nicht mehr geändert.
* Das neue Cmdlet Get-AzureADPasswordProtectionProxy wurde hinzugefügt, um bei der Ermittlung von registrierten Proxys in einer Gesamtstruktur oder Domäne zu helfen.
* Der DC-Agent verwendet einen neuen Ordner in der SYSVOL-Freigabe für die Replikation von Kennwortrichtlinien und anderen Dateien.

   Alter Speicherort des Ordners:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Neuer Speicherort des Ordners:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   (Diese Änderung wurde vorgenommen, um falsch positive Warnungen zu verwaisten Gruppenrichtlinienobjekten zu vermeiden.)

   > [!NOTE]
   > Es erfolgt keine Migration oder Freigabe von Daten zwischen dem alten und dem neuen Ordner. Ältere Versionen des DC-Agents verwenden weiterhin den alten Speicherort, bis sie auf diese oder eine neuere Version aktualisiert werden. Sobald alle DC-Agents in Version 1.2.65.0 oder höher ausgeführt werden, kann der alte SYSVOL-Ordner manuell gelöscht werden.

* Der DC-Agent und der Proxydienst erkennen und löschen jetzt beschädigte Kopien von ihren jeweiligen Dienstverbindungspunkten.
* Jeder DC-Agent löscht regelmäßig beschädigte und veraltete DC-Agent- und Proxydienst-Verbindungspunkte in seiner Domäne. DC-Agent- und Proxydienst-Verbindungspunkte werden als veraltet betrachtet, wenn der Heartbeat-Zeitstempel älter als sieben Tage ist.
* Der DC-Agent erneuert nun das Zertifikat der Gesamtstruktur nach Bedarf.
* Der Proxydienst erneuert nun das Proxyzertifikat nach Bedarf.
* Updates am Kennwort-Überprüfungsalgorithmus: Die Liste der global gesperrten Kennwörter und die Liste der kundenspezifischen gesperrten Kennwörter (sofern konfiguriert) werden vor Kennwortüberprüfungen kombiniert. Ein bestimmtes Kennwort kann nun abgelehnt werden (Fehler oder nur Überwachung), wenn es Token sowohl aus der globalen als auch aus der kundenspezifischen Liste enthält. Die Ereignisprotokolldokumentation wurde entsprechend aktualisiert. Weitere Informationen finden Sie unter [Überwachen des Kennwortschutzes für Azure AD](howto-password-ban-bad-on-premises-monitor.md).
* Fehlerbehebungen bei Leistung und Stabilität
* Verbesserte Protokollierung.

> [!WARNING]
> Zeitlich begrenzte Funktionalität: Der DC-Agent-Dienst in diesem Release (1.2.65.0) beendet die Verarbeitung von Anforderungen zur Kennwortvalidierung ab dem 1. September 2019.  DC-Agent-Dienste in früheren Versionen (siehe Liste weiter unten) beenden die Verarbeitung ab dem 1. Juli 2019. Der DC-Agent-Dienst in allen Versionen protokolliert in den beiden Monaten vor diesen Terminen 10021-Ereignisse im Administratorereignisprotokoll. Im bevorstehenden Release mit allgemeiner Verfügbarkeit werden alle zeitlichen Einschränkungen entfernt. Der Proxy-Agent-Dienst ist in keiner Version zeitlich begrenzt, sollte jedoch trotzdem auf die neueste Version aktualisiert werden, um nachfolgende Fehlerbehebungen und Optimierungen nutzen zu können.

## <a name="12250"></a>1.2.25.0

Veröffentlichungsdatum: 1.11.2018

Fehlerbehebungen:

* DC-Agent- und Proxydienst sollten nicht mehr aufgrund von Zertifikatvertrauensfehlern fehlschlagen.
* DC-Agent- und Proxydienst weisen zusätzliche Fixes für FIPS-konforme Computer auf.
* Der Proxydienst funktioniert jetzt in einer reinen TLS 1.2-Netzwerkumgebung ordnungsgemäß.
* Kleinere Fehlerbehebungen bei Leistung und Stabilität.
* Verbesserte Protokollierung.

Änderungen:

* Die mindestens erforderliche Betriebssystemebene für den Proxydienst ist jetzt Windows Server 2012 R2. Die mindestens erforderliche Betriebssystemebene für den DC-Agent-Dienst bleibt Windows Server 2012.
* Der Proxydienst erfordert jetzt .NET Version 4.6.2.
* Der Algorithmus für die Kennwortüberprüfung verwendet eine erweiterte Zeichennormalisierungstabelle. Dies kann dazu führen, dass Kennwörter abgelehnt werden, die in früheren Versionen akzeptiert wurden.

## <a name="12100"></a>1.2.10.0

Veröffentlichungsdatum: 17.8.2018

Fehlerbehebungen:

* Register-AzureADPasswordProtectionProxy und Register-AzureADPasswordProtectionForest unterstützen jetzt mehrstufige Authentifizierung.
* Register-AzureADPasswordProtectionProxy erfordert eine WS2012-Domänencontroller oder höher in der Domäne, um Verschlüsselungsfehler zu vermeiden.
* Der DC-Agent-Dienst ist zuverlässiger beim Anfordern einer neuen Kennwortrichtlinie von Azure beim Starten.
* Der DC-Agent-Dienst fordert jede Stunde eine neue Kennwortrichtlinie von Azure an, falls erforderlich, führt dies aber jetzt zu einer zufällig ausgewählten Startzeit aus.
* Der DC-Agent-Dienst verursacht keine unbestimmte Verzögerung mehr bei einer neuen DC-Ankündigung, wenn er vor seiner Höherstufung als Replikat auf einem Server installiert wurde.
* Der DC-Agent-Dienst respektiert jetzt die Konfigurationseinstellung „Kennwortschutz für Windows Server Active Directory aktivieren“.
* Beide DC-Agent- und Proxy-Installationsprogramme unterstützen jetzt ein direktes Upgrade beim Upgrade auf zukünftige Versionen.

> [!WARNING]
> Ein direktes Upgrade von Version 1.1.10.3 wird nicht unterstützt und führt zu einem Installationsfehler. Für ein Upgrade auf Version 1.2.10 oder höher müssen Sie zuerst die DC-Agent- und Proxydienst-Software vollständig deinstallieren und dann die neue Version von Grund auf neu installieren. Eine erneute Registrierung des Azure AD-Kennwortschutz-Proxydiensts ist erforderlich.  Es ist nicht erforderlich, die Gesamtstruktur erneut zu registrieren.

> [!NOTE]
> Direkte Upgrades der DC-Agent-Software machen einen Neustart erforderlich.

* Der DC-Agent und Proxy-Dienst unterstützen nun die Ausführung auf einem Server, der für die ausschließliche Verwendung FIPS-konformer Algorithmen konfiguriert ist.
* Kleinere Fehlerbehebungen bei Leistung und Stabilität.
* Verbesserte Protokollierung.

## <a name="11103"></a>1.1.10.3

Veröffentlichungsdatum: 15.6.2018

Erste öffentliche Vorschauversion

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen des Kennwortschutzes für Azure AD](howto-password-ban-bad-on-premises-deploy.md)
