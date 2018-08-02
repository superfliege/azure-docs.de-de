---
title: 'Azure AD Connect: Passthrough-Authentifizierung – Häufig gestellte Fragen | Microsoft-Dokumentation'
description: In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen zur Passthrough-Authentifizierung von Azure Active Directory.
services: active-directory
keywords: Passthrough-Authentifizierung mit Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, einmaliges Anmelden
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 8b5f62daf2b43453aadb0373171bc98f96494688
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215066"
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory Passthrough-Authentifizierung: Häufig gestellte Fragen

In diesem Artikel werden häufig gestellte Fragen zur Passthrough-Authentifizierung von Azure Active Directory (Azure AD) behandelt. Der Inhalt wird bei Bedarf aktualisiert.

## <a name="which-of-the-methods-to-sign-in-to-azure-ad-pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs-should-i-choose"></a>Welche der Methoden für die Anmeldung bei Azure AD (Passthrough-Authentifizierung, Kennworthashsynchronisierung oder Active Directory-Verbunddienste (AD FS)) sollte ich auswählen?

Einen Vergleich der verschiedenen Azure AD-Anmeldemethoden und, wie Sie die richtige Anmeldemethode für Ihr Unternehmen wählen, finden Sie in [diesem Leitfaden](https://docs.microsoft.com/azure/security/azure-ad-choose-authn).

## <a name="is-pass-through-authentication-a-free-feature"></a>Ist die Passthrough-Authentifizierung ein kostenloses Feature?

Die Passthrough-Authentifizierung ist ein kostenloses Feature. Sie benötigen für die Verwendung keine kostenpflichtigen Editionen von Azure AD.

## <a name="is-pass-through-authentication-available-in-the-microsoft-azure-germany-cloudhttpwwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Ist die Passthrough-Authentifizierung in der Cloud [Microsoft Azure Deutschland](http://www.microsoft.de/cloud-deutschland) und der Cloud [Azure Government](https://azure.microsoft.com/features/gov/) verfügbar?

Nein. Die Passthrough-Authentifizierung ist nur in der weltweiten Instanz von Azure AD verfügbar.

## <a name="does-conditional-accessactive-directory-conditional-access-azure-portalmd-work-with-pass-through-authentication"></a>Funktioniert der [bedingte Zugriff](../active-directory-conditional-access-azure-portal.md) mit der Passthrough-Authentifizierung?

Ja. Alle Funktionen mit bedingtem Zugriff, einschließlich Azure Multi-Factor Authentication, funktionieren mit der Passthrough-Authentifizierung.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Unterstützt die Passthrough-Authentifizierung eine alternative Anmelde-ID (Alternate ID) anstelle von „UserPrincipalName“ als Benutzernamen?

Ja. Die Passthrough-Authentifizierung unterstützt `Alternate ID` als Benutzername, wenn dies in Azure AD Connect konfiguriert ist. Weitere Informationen finden Sie unter [Benutzerdefinierte Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md). Nicht alle Office 365-Anwendungen unterstützen `Alternate ID`. Angaben zur Supporterklärung finden Sie in der Dokumentation der jeweiligen Anwendung.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Kann die Kennworthashsynchronisierung als Fallback für die Passthrough-Authentifizierung verwendet werden?

Nein. Bei der Passthrough-Authentifizierung wird _kein_ automatisches Failover auf die Kennworthashsynchronisierung ausgeführt. Sie dient nur als Fallback für [Szenarios, die zurzeit noch nicht von der Passthrough-Authentifizierung unterstützt werden](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). Um Benutzeranmeldefehler zu vermeiden, sollten Sie die Passthrough-Authentifizierung für [Hochverfügbarkeit](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability) konfigurieren.

## <a name="can-i-install-an-azure-ad-application-proxymanage-appsapplication-proxymd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Kann ich einen [Azure AD-Anwendungsproxy](../manage-apps/application-proxy.md)-Connector auf demselben Server wie dem eines Passthrough-Authentifizierungs-Agents installieren?

Ja. Die umbenannten Versionen des Passthrough-Authentifizierungs-Agents (Versionen 1.5.193.0 oder höher) unterstützen diese Konfiguration.

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Welche Versionen von Azure AD Connect und dem Passthrough-Authentifizierungs-Agent werden benötigt?

Damit dieses Feature funktioniert, benötigen Sie Version 1.1.486.0 oder höher für Azure AD Connect und 1.5.58.0 oder höher für den Passthrough-Authentifizierungs-Agent. Installieren Sie die gesamte Software auf Servern mit Windows Server 2012 R2 oder höher.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication"></a>Was geschieht, wenn das Kennwort des Benutzers abgelaufen ist und er versucht, sich mit der Passthrough-Authentifizierung anzumelden?

Wenn Sie das [Kennwortrückschreiben](../user-help/active-directory-passwords-update-your-own-password.md) für einen bestimmten Benutzer konfiguriert haben und der Benutzer sich mit der Passthrough-Authentifizierung anmeldet, kann er sein Kennwort ändern oder zurücksetzen. Die Kennwörter werden erwartungsgemäß in das lokale Active Directory zurückgeschrieben.

Wenn Sie das Kennwortrückschreiben für einen bestimmten Benutzer nicht konfiguriert haben oder der Benutzer keine gültige Azure AD hat, kann der Benutzer sein Kennwort in der Cloud nicht aktualisieren. Das Kennwort lässt sich nicht aktualisieren, auch wenn es bereits abgelaufen ist. Der Benutzer sieht dann folgende Meldung: „Eine Kennwortänderung auf dieser Website wird von Ihrer Organisation nicht gestattet. Ändern Sie Ihr Kennwort anhand der von Ihrer Organisation empfohlenen Methode, oder bitten Sie Ihren Administrator um Hilfe. Der Benutzer oder der Administrator muss das Kennwort im lokalen Active Directory zurücksetzen.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Wie schützt die Passthrough-Authentifizierung vor Brute-Force-Kennwortangriffen?

Weitere Informationen finden Sie unter [Azure Active Directory-Passthrough-Authentifizierung: Smart Lockout](../authentication/howto-password-smart-lockout.md).

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Was kommunizieren Passthrough-Authentifizierungs-Agents über die Ports 80 und 443?

- Die Authentifizierungs-Agents stellen HTTPS-Anforderungen für alle Funktionsvorgänge über Port 443 aus.
- Die Authentifizierungs-Agents stellen HTTP-Anforderungen über Port 80, um die SSL-Zertifikatsperrlisten (CRLs) herunterzuladen.

     >[!NOTE]
     >In kürzlich veröffentlichten Updates wurde die Zahl der Ports, die für diese Funktion erforderlich sind, gesenkt. Wenn Sie eine ältere Version von Azure AD Connect oder des Authentifizierungs-Agents verwenden, halten Sie auch folgende Ports offen: 5671, 8080, 9090, 9091, 9350, 9352 und 10100-10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Können die Passthrough-Authentifizierungs-Agents über einen ausgehenden Webproxyserver kommunizieren?

Ja. Wenn in Ihrer lokalen Umgebung WPAD (Web Proxy Auto-Discovery) aktiviert ist, versuchen die Authentifizierungs-Agents automatisch, einen Webproxyserver im Netzwerk zu finden und zu verwenden.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Kann ich zwei oder mehr Passthrough-Authentifizierungs-Agents auf dem gleichen Server installieren?

Nein, Sie können nur einen Passthrough-Authentifizierungs-Agent auf einem einzelnen Server installieren. Wenn Sie die Passthrough-Authentifizierung für Hochverfügbarkeit konfigurieren möchten, befolgen Sie die Anweisungen unter [Azure Active Directory-Passthrough-Authentifizierung: Schnellstart](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability).

## <a name="how-do-i-remove-a-pass-through-authentication-agent"></a>Gewusst wie: Entfernen eines Passthrough-Authentifizierungs-Agents

Solange ein Passthrough-Authentifizierungs-Agent ausgeführt wird, bleibt er aktiv und verarbeitet fortlaufend Anmeldeanforderungen von Benutzern. Wenn Sie einen Authentifizierungs-Agent deinstallieren möchten, wechseln Sie zu **Systemsteuerung > Programme > Programme und Funktionen**, und deinstallieren Sie die beiden Programme **Microsoft Azure AD Connect-Authentifizierungs-Agent** und **Microsoft Azure AD Connect Agent Updater**.

Nach Abschluss des vorherigen Schritts sehen Sie auf dem Blatt „Passthrough-Authentifizierung“ im [Azure Active Directory-Admin Center](https://aad.portal.azure.com) den Authentifizierungs-Agent als **Inaktiv**. Dies entspricht dem _erwarteten_ Verhalten. Der Authentifizierungs-Agent wird nach einigen Tagen automatisch aus der Liste entfernt.

## <a name="i-already-use-ad-fs-to-sign-in-to-azure-ad-how-do-i-switch-it-to-pass-through-authentication"></a>Ich verwende bereits AD FS für die Anmeldung bei Azure AD. Wie kann ich zur Passthrough-Authentifizierung wechseln?

Wenn Sie von AD FS (oder andere Verbundtechnologien nutzen) zur Passthrough-Authentifizierung migrieren, wird dringend empfohlen, dem ausführlichen [Leitfaden zur Bereitstellung](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx) zu folgen.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-active-directory-environment"></a>Kann ich die Passthrough-Authentifizierung in einer Active Directory-Umgebung mit mehreren Gesamtstrukturen verwenden?

Ja. Umgebungen mit mehreren Gesamtstrukturen werden unterstützt, wenn Gesamtstruktur-Vertrauensstellungen zwischen Ihren Active Directory-Gesamtstrukturen bestehen und das Namensuffixrouting ordnungsgemäß konfiguriert ist.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Wie viele Passthrough-Authentifizierungs-Agents muss ich installieren?

Mit der Installation von mehreren Passthrough-Authentifizierungs-Agents wird für [Hochverfügbarkeit](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability) gesorgt. Es wird aber kein deterministischer Lastenausgleich zwischen den Authentifizierungs-Agents bereitgestellt.

Berücksichtigen Sie die Spitzenlast und durchschnittliche Last in Bezug auf die Anmeldeanforderungen, die Sie für Ihren Mandanten erwarten. Als Richtwert gilt, dass ein einzelner Authentifizierungs-Agent auf einem Standardserver mit einer CPU mit vier Kernen und 16 GB RAM pro Sekunde 300 bis 400 Authentifizierungen verarbeiten kann.

Um den Netzwerkverkehr abzuschätzen, verwenden Sie die folgende Anleitung zur Skalierung:
- Jede Anforderung hat eine Nutzlastgröße von (0,5 K + 1 K * anz_agents) Bytes, d. h. Daten von Azure AD zum Authentifizierungs-Agent. Hier gibt „anz_agents“ die Anzahl der für Ihren Mandanten registrierten Authentifizierung-Agents an.
- Jede Antwort hat eine Nutzlastgröße von 1 K Bytes, d. h. Daten vom Authentifizierungs-Agent zu Azure AD.

Für die meisten Kunden reichen zwei oder drei Authentifizierungs-Agents insgesamt aus, um Hochverfügbarkeit und genügend Kapazität zu erzielen. Es wird empfohlen, die Authentifizierungs-Agents in der Nähe Ihres Domänencontrollers zu installieren, um die Anmeldungslatenz zu verbessern.

>[!NOTE]
>In einem System können maximal 12 Authentifizierungs-Agents pro Mandant installiert werden.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Kann ich den ersten Passthrough-Authentifizierungs-Agent auf einem anderen Server als demjenigen mit Azure AD Connect installieren?

Nein, dieses Szenario wird _nicht_ unterstützt.

## <a name="how-can-i-disable-pass-through-authentication"></a>Wie kann ich die Passthrough-Authentifizierung deaktivieren?

Führen Sie den Azure AD Connect-Assistenten erneut aus, und ändern Sie die Anmeldemethode für Benutzer von der Passthrough-Authentifizierung in eine andere Methode. Diese Änderung deaktiviert die Passthrough-Authentifizierung für den Mandanten und deinstalliert den Authentifizierungs-Agent vom Server. Auf anderen Servern müssen Sie die Authentifizierungs-Agents manuell deinstallieren.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Was geschieht, wenn ich einen Passthrough-Authentifizierungs-Agent deinstalliere?

Wenn Sie einen Passthrough-Authentifizierungs-Agents auf einem Server deinstallieren, werden vom Server keine Anmeldeanforderungen mehr angenommen. Um eine Unterbrechung der Anmeldefunktion für Benutzer zu vermeiden, stellen Sie sicher, dass ein anderer Authentifizierungs-Agent ausgeführt wird, bevor Sie einen Passthrough-Authentifizierungs-Agent deinstallieren.

## <a name="next-steps"></a>Nächste Schritte
- [Aktuelle Einschränkungen:](active-directory-aadconnect-pass-through-authentication-current-limitations.md) Informationen zu den unterstützten und nicht unterstützten Szenarien
- [Schnellstart:](active-directory-aadconnect-pass-through-authentication-quick-start.md) Aktivieren und Ausführen der Passthrough-Authentifizierung von Azure AD
- [Migrieren von AD FS zur Passthrough-Authentifizierung:](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx) Ein detaillierter Leitfaden zur Migration von AD FS (oder anderen Verbundtechnologien) zur Passthrough-Authentifizierung.
- [Smart Lockout:](../authentication/howto-password-smart-lockout.md) Konfigurieren der Smart Lockout-Funktion für Ihren Mandanten, um Benutzerkonten zu schützen
- [Technische Einzelheiten:](active-directory-aadconnect-pass-through-authentication-how-it-works.md) Informationen zur Funktionsweise der Passthrough-Authentifizierung
- [Problembehandlung:](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) Informationen zum Beheben von allgemeinen Problemen, die es bei der Passthrough-Authentifizierung geben kann
- [Ausführliche Informationen zur Sicherheit:](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) Technische Informationen zur Passthrough-Authentifizierung
- [Nahtloses SSO mit Azure AD](active-directory-aadconnect-sso.md): Informationen zu dieser Ergänzungsfunktion
- [UserVoice:](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Anfordern neuer Features über das Azure Active Directory-Forum

