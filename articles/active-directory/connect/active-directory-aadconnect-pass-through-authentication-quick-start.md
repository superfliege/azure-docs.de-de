---
title: Azure AD-Passthrough-Authentifizierung – Schnellstart | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die ersten Schritte für die Azure AD-Passthrough-Authentifizierung (Azure Active Directory) ausführen.
services: active-directory
keywords: Azure AD Connect-Passthrough-Authentifizierung, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: billmath
ms.openlocfilehash: b592eb8ca43e5bf3eebe2b0c47d8f17dbec7b238
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory-Passthrough-Authentifizierung: Schnellstart

## <a name="deploy-azure-ad-pass-through-authentication"></a>Bereitstellen der Azure AD-Passthrough-Authentifizierung

Mit der Azure Active Directory-Passthrough-Authentifizierung (Azure AD) können sich Benutzer mit denselben Kennwörtern sowohl bei lokalen als auch bei cloudbasierten Anwendungen anmelden. Benutzer werden bei der Passthrough-Authentifizierung angemeldet, indem sie ihre Kennwörter direkt für Ihre lokale Active Directory-Instanz angeben.

>[!IMPORTANT]
>Wenn Sie dieses Feature über eine Vorschauversion nutzen, aktualisieren Sie die Vorschauversionen der Authentifizierungs-Agenten unbedingt anhand der Anweisungen unter [Azure Active Directory-Passthrough-Authentifizierung – Upgrade von Authentifizierungs-Agents der Vorschau](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).

Befolgen Sie diese Anweisungen, um die Passthrough-Authentifizierung bereitzustellen:

## <a name="step-1-check-the-prerequisites"></a>Schritt 1: Überprüfen der Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:

### <a name="in-the-azure-active-directory-admin-center"></a>Im Azure Active Directory Admin Center

1. Erstellen Sie in Ihrem Azure AD-Mandanten ein auf die Cloud beschränktes globales Administratorkonto. Auf diese Weise können Sie die Konfiguration Ihres Mandanten verwalten, falls Ihre lokalen Dienste ausfallen oder nicht verfügbar sind. Erfahren Sie, wie Sie ein [rein cloudbasiertes Konto für den globalen Administrator hinzufügen](../active-directory-users-create-azure-portal.md). Die Ausführung dieses Schritts ist sehr wichtig, damit sichergestellt ist, dass Sie für Ihren Mandanten nicht gesperrt werden.
2. Fügen Sie Ihrem Azure AD-Mandanten mindestens einen [benutzerdefinierten Domänennamen](../active-directory-domains-add-azure-portal.md) hinzu. Ihre Benutzer können sich mit einem dieser Domänennamen anmelden.

### <a name="in-your-on-premises-environment"></a>In Ihrer lokalen Umgebung

1. Identifizieren Sie einen Server mit Windows Server 2012 R2 oder höher, auf dem Azure AD Connect ausgeführt werden soll. Fügen Sie den Server derselben Active Directory-Gesamtstruktur wie die Benutzer hinzu, deren Kennwörter überprüft werden müssen.
2. Installieren Sie die [aktuelle Version von Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) auf dem Server, den Sie im vorherigen Schritt ausgewählt haben. Wenn Azure AD Connect bereits ausgeführt wird, vergewissern Sie sich, dass die Version 1.1.644.0 oder höher lautet.

    >[!NOTE]
    >Die Azure AD Connect-Versionen 1.1.557.0, 1.1.558.0, 1.1.561.0 und 1.1.614.0 weisen ein Problem in Bezug auf die Kennworthashsynchronisierung auf. Wenn Sie die Kennworthashsynchronisierung _nicht_ zusammen mit der Passthrough-Authentifizierung verwenden möchten, finden Sie Informationen dazu in den [Versionshinweisen zu Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470).

3. Identifizieren Sie einen weiteren Server (mit Windows Server 2012 R2 oder höher), auf dem ein eigenständiger Authentifizierungs-Agent ausgeführt werden kann. Die Version des Authentifizierungs-Agents muss 1.5.193.0 oder höher sein. Dieser zusätzliche Server wird benötigt, um die Hochverfügbarkeit von Anmeldeanforderungen sicherzustellen. Fügen Sie den Server derselben Active Directory-Gesamtstruktur wie die Benutzer hinzu, deren Kennwörter überprüft werden müssen.
4. Wenn zwischen Ihren Servern und Azure AD eine Firewall eingerichtet wurde, konfigurieren Sie die folgenden Elemente:
   - Stellen Sie sicher, dass Authentifizierung-Agents *ausgehende* Anforderungen an Azure AD über die folgenden Ports senden können:
   
    | Portnummer | Wie diese verwendet wird |
    | --- | --- |
    | **80** | Herunterladen der Zertifikatssperrlisten (CRL) bei der Überprüfung des SSL-Zertifikats |
    | **443** | Verarbeitung der gesamten ausgehende Kommunikation mit dem Dienst |
   
    Wenn Ihre Firewall Regeln gemäß Ursprungsbenutzern erzwingt, öffnen Sie diese Ports für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden.
   - Wenn Ihre Firewall oder ihr Proxy DNS-Whitelisting zulässt, beschränken Sie Verbindungen mit **\*.msappproxy.net** und **\*.servicebus.windows.net** mittels Whitelist. Aktivieren Sie andernfalls den Zugriff auf die [IP-Adressbereiche für das Azure-Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=41653), die wöchentlich aktualisiert werden.
   - Ihre Authentifizierungs-Agents benötigen für den anfänglichen Registrierungsprozess Zugriff auf **login.windows.net** und **login.microsoftonline.com**. Öffnen Sie Ihre Firewall auch für diese URLs.
   - Geben Sie für die Überprüfung des Zertifikats folgende URLs frei: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80** und **www.microsoft.com:80**. Diese URLs werden für die Überprüfung des Zertifikats in Verbindung mit anderen Microsoft-Produkten verwendet. Daher haben Sie diese möglicherweise bereits freigegeben.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>Schritt 2: Aktivieren der Exchange ActiveSync-Unterstützung (optional)

Um Exchange ActiveSync-Unterstützung zu aktivieren, gehen Sie wie folgt vor:

1. Führen Sie mit [Exchange PowerShell](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) folgenden Befehl aus:
```
Get-OrganizationConfig | fl per*
```

2. Überprüfen Sie den Wert der `PerTenantSwitchToESTSEnabled`-Einstellung. Wenn der Wert **true** lautet, ist der Mandant ordnungsgemäß konfiguriert. Dies ist im Allgemeinen bei den meisten Kunden der Fall. Wenn der Wert **false** ist, führen Sie folgenden Befehl aus:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Überprüfen Sie, ob der Wert der `PerTenantSwitchToESTSEnabled`-Einstellung nun auf **true** festgelegt ist. Warten Sie eine Stunde, bevor Sie mit dem nächsten Schritt fortfahren.

Wenn bei diesem Schritt Probleme auftreten, lesen Sie unter [Behandlung von Problemen bei der Azure Active Directory-Passthrough-Authentifizierung](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues) nach.

## <a name="step-3-enable-the-feature"></a>Schritt 3: Aktivieren des Features

Aktivieren Sie die Passthrough-Authentifizierung über [Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>Sie können die Passthrough-Authentifizierung auf dem primären Server oder dem Stagingserver von Azure AD Connect aktivieren. Es wird empfohlen, sie auf dem primären Server zu aktivieren.

Wählen Sie den [benutzerdefinierten Installationspfad](active-directory-aadconnect-get-started-custom.md), wenn Sie Azure AD Connect zum ersten Mal installieren. Wählen Sie auf der Seite **Benutzeranmeldung** die Option **Passthrough-Authentifizierung** als **Anmeldemethode** aus. Nach erfolgreicher Durchführung wird auf demselben Server wie Azure AD Connect ein Passthrough-Authentifizierungs-Agent installiert. Darüber hinaus wird die Passthrough-Authentifizierungsfunktion auf Ihrem Mandanten aktiviert.

![Azure AD Connect: Benutzeranmeldung](./media/active-directory-aadconnect-sso/sso3.png)

Wenn Sie Azure AD Connect bereits installiert haben (per [Expressinstallation](active-directory-aadconnect-get-started-express.md) oder [benutzerdefinierter Installation](active-directory-aadconnect-get-started-custom.md)), wählen Sie in Azure AD Connect die Aufgabe **Benutzeranmeldung ändern** aus, und klicken Sie auf **Weiter**. Wählen Sie anschließend **Passthrough-Authentifizierung** als Anmeldemethode aus. Nach erfolgreicher Durchführung wird auf demselben Server wie Azure AD Connect ein Passthrough-Authentifizierungs-Agent installiert, und das Feature wird auf Ihrem Mandanten aktiviert.

![Azure AD Connect: Benutzeranmeldung ändern](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Die Passthrough-Authentifizierung ist ein Feature auf Mandantenebene. Wenn Sie es aktivieren, wirkt sich dies auf die Anmeldung der Benutzer in _allen_ verwalteten Domänen Ihres Mandanten aus. Wenn Sie von Active Directory-Verbunddienste (AD FS) zur Passthrough-Authentifizierung wechseln, sollten Sie mindestens zwölf Stunden warten, bevor Sie Ihre AD FS-Infrastruktur herunterfahren. Diese Wartezeit soll sicherstellen, dass Benutzer sich während des Übergangs weiterhin bei Exchange ActiveSync anmelden können.

## <a name="step-4-test-the-feature"></a>Schritt 4: Testen des Features

Befolgen Sie diese Anweisungen, um zu überprüfen, ob die Passthrough-Authentifizierung ordnungsgemäß aktiviert ist:

1. Melden Sie sich mit den Anmeldeinformationen des globalen Administrators für Ihren Mandanten beim [Azure Active Directory Admin Center](https://aad.portal.azure.com) an.
2. Wählen Sie im linken Bereich die Option **Azure Active Directory** aus.
3. Wählen Sie **Azure AD Connect** aus.
4. Überprüfen Sie, ob das Feature **Passthrough-Authentifizierung** als **aktiviert** angezeigt wird.
5. Wählen Sie **Passthrough-Authentifizierung** aus. Im Bereich **Passthrough-Authentifizierung** werden die Server aufgelistet, auf denen Ihre Authentifizierungs-Agents installiert sind.

![Azure Active Directory Admin Center: Azure AD Connect-Bereich](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory Admin Center: Bereich „Passthrough-Authentifizierung“](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

Jetzt können sich Benutzer aus allen verwalteten Domänen Ihres Mandanten mit der Passthrough-Authentifizierung anmelden. Benutzer von Verbunddomänen melden sich aber weiterhin mithilfe der AD FS oder einem anderen Verbundanbieter an, den Sie zuvor konfiguriert haben. Wenn Sie eine Verbunddomäne in eine verwaltete Domäne konvertieren, melden sich alle Benutzer dieser Domäne automatisch mit der Passthrough-Authentifizierung an. Benutzer, die auf die Cloud beschränkt sind, sind von der Passthrough-Authentifizierungsfunktion nicht betroffen.

## <a name="step-5-ensure-high-availability"></a>Schritt 5: Sicherstellen der Hochverfügbarkeit

Wenn Sie die Bereitstellung der Passthrough-Authentifizierung in einer Produktionsumgebung planen, müssen Sie mindestens einen eigenständigen Authentifizierungs-Agent installieren. Installieren Sie Authentifizierungs-Agents auf _anderen_ Servern als dem, auf dem Azure AD Connect ausgeführt wird. Mit dieser Einrichtung erzielen Sie Hochverfügbarkeit für Anforderungen zur Benutzeranmeldung.

Befolgen Sie diese Anweisungen zum Herunterladen der Authentifizierungs-Agent-Software:

1. Zum Herunterladen der neuesten Version des Authentifizierungs-Agents (Version 1.5.193.0 oder höher) melden Sie sich mit den Anmeldeinformationen des globalen Administrators für Ihren Mandanten beim [Azure Active Directory-Admin Center](https://aad.portal.azure.com) an.
2. Wählen Sie im linken Bereich die Option **Azure Active Directory** aus.
3. Klicken Sie auf **Azure AD Connect**, **Passthrough-Authentifizierung** und dann auf **Agent herunterladen**.
4. Klicken Sie auf die Schaltfläche **Bedingungen akzeptieren und herunterladen**.

![Azure Active Directory Admin Center: Schaltfläche zum Herunterladen des Authentifizierungs-Agents](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Azure Active Directory Admin Center: Bereich „Agent herunterladen“](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>Sie können die Authentifizierungs-Agent-Software auch [hier](https://aka.ms/getauthagent) direkt herunterladen. Lesen und akzeptieren Sie die [Nutzungsbedingungen](https://aka.ms/authagenteula) für den Authentifizierungs-Agent, _bevor_ Sie ihn installieren.

Es gibt zwei Methoden zum Bereitstellen eines eigenständigen Authentifizierungs-Agents:

Die erste Methode ist die interaktive Bereitstellung, indem einfach die heruntergeladene ausführbare Authentifizierungs-Agent-Datei ausgeführt wird, und Sie bei Aufforderung die Anmeldeinformationen des globalen Administrators Ihres Mandanten angeben.

Zweitens können Sie auch ein unbeaufsichtigtes Bereitstellungsskript erstellen und ausführen. Dies ist hilfreich, wenn Sie mehrere Authentifizierungs-Agents gleichzeitig bereitstellen oder Authentifizierungs-Agents auf Windows-Servern installieren möchten, auf denen keine Benutzeroberfläche aktiviert ist bzw. auf die Sie nicht mit Remotedesktop zugreifen können. Hier finden Sie die Anweisungen für diesen Ansatz:

1. Führen Sie den folgenden Befehl aus, um einen Authentifizierungs-Agent zu installieren: `AADConnectAuthAgentSetup.exe REGISTERCONNECTOR="false" /q`.
2. Sie können den Authentifizierungs-Agent mithilfe von Windows PowerShell bei unserem Dienst registrieren. Erstellen Sie ein PowerShell-Anmeldeinformationsobjekt (`$cred`) mit einem globalen Administratorbenutzernamen und -kennwort für Ihren Mandanten. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei *\<username\>* und *\<password\>* durch die entsprechenden Werte:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
3. Wechseln Sie zu **C:\Programme\Microsoft Azure AD Connect Authentication Agent**, und führen Sie das folgende Skript unter Verwendung des zuvor erstellten Objekts `$cred` aus:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft Azure AD Connect Authentication Agent\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature PassthroughAuthentication

## <a name="next-steps"></a>Nächste Schritte
- [Smart Lockout:](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) Konfigurieren der Smart Lockout-Funktion für Ihren Mandanten, um Benutzerkonten zu schützen
- [Aktuelle Einschränkungen:](active-directory-aadconnect-pass-through-authentication-current-limitations.md) Informationen zu den unterstützten und nicht unterstützten Szenarien mit Passthrough-Authentifizierung
- [Technische Einzelheiten:](active-directory-aadconnect-pass-through-authentication-how-it-works.md) Informationen zur Funktionsweise des Features für die Passthrough-Authentifizierung
- [Häufig gestellte Fragen:](active-directory-aadconnect-pass-through-authentication-faq.md) Antworten auf häufig gestellte Fragen
- [Problembehandlung:](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) Informationen zum Beheben von allgemeinen Problemen, die mit der Funktion für Passthrough-Authentifizierung auftreten können
- [Ausführliche Informationen zur Sicherheit:](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) Technische Informationen zur Passthrough-Authentifizierung
- [Nahtloses SSO mit Azure AD:](active-directory-aadconnect-sso.md) Informationen zu dieser Ergänzungsfunktion
- [UserVoice:](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Anfordern neuer Features über das Azure Active Directory-Forum

