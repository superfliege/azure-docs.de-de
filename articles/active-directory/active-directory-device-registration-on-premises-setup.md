---
title: Lokales Einrichten des bedingten Zugriffs in Azure Active Directory | Microsoft-Dokumentation
description: Eine Schritt-für-Schritt-Anleitung, die zeigt, wie Sie mithilfe der Active Directory-Verbunddienste (AD FS) unter Windows Server 2012 R2 den bedingten Zugriff auf lokale Anwendungen ermöglichen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: devices
ms.assetid: 6ae9df8b-31fe-4d72-9181-cf50cfebbf05
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.custom: seohack1
ms.openlocfilehash: b1711b86042c74eba47fe1cfa41bbbd36ceaf127
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248757"
---
# <a name="setting-up-on-premises-conditional-access-by-using-azure-active-directory-device-registration"></a>Einrichten des lokalen bedingten Zugriffs mithilfe der Azure Active Directory-Geräteregistrierung

Wenn Sie von Benutzern verlangen, ihre persönlichen Geräte mithilfe des Geräteregistrierungsdiensts mit Azure Active Directory (Azure AD) über den Arbeitsplatz einzubinden, können deren Geräte als Ihrer Organisation bekannt markiert werden. Im Folgenden wird eine Schritt-für-Schritt-Anleitung aufgeführt, die zeigt, wie Sie mithilfe der Active Directory-Verbunddienste (AD FS) unter Windows Server 2012 R2 den bedingten Zugriff auf lokale Anwendungen ermöglichen.

> [!NOTE]
> Wenn Geräte verwendet werden, die über den Azure Active Directory-Geräteregistrierungsdienst registriert und mit Richtlinien für den bedingten Zugriff konfiguriert sind, wird eine Office 365-Lizenz oder eine Azure AD Premium-Lizenz benötigt. Dies gilt für Richtlinien, die von AD FS in lokalen Ressourcen erzwungen werden.
> 
> Weitere Informationen zu den Szenarien für den bedingten Zugriff auf lokale Ressourcen finden Sie unter [Arbeitsplatzbeitritt von einem beliebigen Gerät für SSO und die nahtlose zweistufige Authentifizierung bei allen Unternehmensanwendungen](https://technet.microsoft.com/library/dn280945.aspx).

Diese Funktionen sind für Kunden verfügbar, die eine Azure Active Directory Premium-Lizenz erwerben.

## <a name="supported-devices"></a>Unterstützte Geräte

* Windows 7-Geräte, die einer Domäne angehören
* Persönliche Windows 8.1-Geräte und Windows 8.1-Geräte, die einer Domäne angehören
* iOS 6 und höher, für den Safari-Browser
* Android 4.0 oder höher, Telefonversion Samsung GS3 oder höher, Tabletversion Samsung Galaxy Note 2 oder höher

## <a name="scenario-prerequisites"></a>Voraussetzungen für das Szenario

* Abonnement für Office 365 oder Azure Active Directory Premium
* Azure Active Directory-Mandant
* Windows Server Active Directory (Windows Server 2008 oder höher)
* Aktualisiertes Schema in Windows Server 2012 R2
* Lizenz für Azure Active Directory Premium
* Windows Server 2012 R2-Verbunddienste, konfiguriert für SSO in Azure AD
* Webanwendungsproxy für Windows Server 2012 R2 
* Microsoft Azure Active Directory Connect (Azure AD Connect) [(Azure AD Connect herunterladen)](https://www.microsoft.com/download/details.aspx?id=47594)
* Überprüfte Domäne

## <a name="known-issues-in-this-release"></a>Bekannte Probleme in dieser Version

* Gerätebasierte Richtlinien für den bedingten Zugriff erfordern das Rückschreiben von Geräteobjekten aus Azure Active Directory nach Active Directory. Es kann bis zu drei Stunden dauern, bis Geräteobjekte nach Active Directory zurückgeschrieben werden.
* iOS 7-Geräte fordern den Benutzer immer auf, während der Clientzertifikatauthentifizierung ein Zertifikat auszuwählen.
* Einige Versionen von iOS 8 vor iOS 8.3 funktionieren nicht.

## <a name="scenario-assumptions"></a>Annahmen für das Szenario

Bei diesem Szenario wird davon ausgegangen, dass Sie über eine Hybridumgebung verfügen, die aus einem Azure AD-Mandanten und einer lokalen Active Directory-Instanz besteht. Diese Mandanten sollten mit Azure AD Connect, mit einer überprüften Domäne und mit AD FS für SSO verbunden sein. Mithilfe der folgenden Checkliste können Sie Ihre Umgebung entsprechend den Anforderungen konfigurieren.

## <a name="checklist-prerequisites-for-conditional-access-scenario"></a>Checkliste: Voraussetzungen für Szenarien mit bedingtem Zugriff

Verbinden Sie Ihren Azure AD-Mandanten mit der lokalen Active Directory-Instanz.

## <a name="configure-azure-active-directory-device-registration-service"></a>Konfigurieren des Azure Active Directory-Geräteregistrierungsdiensts

Verwenden Sie diese Anleitung, um den Azure Active Directory-Geräteregistrierungsdienst für Ihre Organisation bereitzustellen und zu konfigurieren.

In diesem Handbuch wird vorausgesetzt, dass Sie Windows Server Active Directory konfiguriert und Microsoft Azure Active Directory abonniert haben. Weitere Informationen finden Sie in den Voraussetzungen weiter oben.

Wenn Sie den Azure Active Directory-Geräteregistrierungsdienst für Ihren Azure Active Directory-Mandanten bereitstellen möchten, führen Sie die in der folgenden Checkliste aufgeführten Aufgaben in der angegebenen Reihenfolge durch. Wenn ein Hyperlink auf ein grundlegendes Thema verweist, kehren Sie nach der Durchsicht des grundlegenden Themas zu dieser Checkliste zurück, um die verbleibenden Aufgaben durchzuführen. Einige Aufgaben erfordern einen Überprüfungsschritt für das Szenario, mit dem bestätigt werden kann, ob der Schritt erfolgreich abgeschlossen wurde.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Teil 1: Aktivieren der Azure Active Directory-Geräteregistrierung

Führen Sie die Schritte in der Checkliste durch, um den Azure Active Directory-Geräteregistrierungsdienst zu aktivieren und zu konfigurieren.

| Aufgabe | Verweis | 
| --- | --- |
| Aktivieren Sie die Geräteregistrierung in Ihrem Azure Active Directory-Mandanten, damit Geräte dem Arbeitsplatz beitreten können. Azure Multi-Factor Authentication ist standardmäßig nicht für den Dienst aktiviert. Es wird jedoch empfohlen, bei der Registrierung eines Geräts Multi-Factor Authentication zu verwenden. Stellen Sie vor der Aktivierung von Multi-Factor Authentication im Active Directory-Registrierungsdienst sicher, dass AD FS für einen MFA-Anbieter (Multi-Factor Authentication) konfiguriert ist. |[Aktivieren der Azure Active Directory-Geräteregistrierung](active-directory-device-registration-get-started.md)| 
|Geräte ermitteln den Azure Active Directory-Geräteregistrierungsdienst, indem sie nach bekannten DNS-Einträgen suchen. Konfigurieren Sie den DNS-Eintrag für Ihr Unternehmen so, dass Geräte den Azure Active Directory-Geräteregistrierungsdienst ermitteln können. |[Konfigurieren der Ermittlung für die Azure Active Directory-Geräteregistrierung](active-directory-device-registration-get-started.md)| 


## <a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Teil 2: Bereitstellen und Konfigurieren von Windows Server 2012 R2 Active Directory-Verbunddiensten und Einrichten einer Partnerbeziehung mit Azure AD

| Aufgabe | Verweis |
| --- | --- |
| Stellen Sie die Active Directory Domain Services mit den Windows Server 2012 R2-Schemaerweiterungen bereit. Es ist nicht erforderlich, ein Upgrade Ihrer Domänencontroller auf Windows Server 2012 R2 auszuführen. Das Upgrade des Schemas ist die einzige Anforderung. |[Durchführen eines Upgrades des Active Directory Domain Services-Schemas](#upgrade-your-active-directory-domain-services-schema) |
| Geräte ermitteln den Azure Active Directory-Geräteregistrierungsdienst, indem sie nach bekannten DNS-Einträgen suchen. Konfigurieren Sie den DNS-Eintrag für Ihr Unternehmen so, dass Geräte den Azure Active Directory-Geräteregistrierungsdienst ermitteln können. |[Vorbereiten von Unterstützungsgeräten für Active Directory](#prepare-your-active-directory-to-support-devices) |

## <a name="part-3-enable-device-writeback-in-azure-ad"></a>Teil 3: Aktivieren des Geräterückschreibens in Azure AD

| Aufgabe | Verweis |
| --- | --- |
| Führen Sie Teil 2 von „Aktivieren des Geräterückschreibens in Azure AD Connect“ durch. Danach können Sie zu den Anweisungen dieses Handbuchs zurückkehren. |[Aktivieren des Geräterückschreibens in Azure AD Connect](hybrid/how-to-connect-device-writeback.md) |

## <a name="optional-part-4-enable-multi-factor-authentication"></a>[Optional] Teil 4: Aktivieren von Multi-Factor Authentication

Es wird dringend empfohlen, eine der verschiedenen Optionen für Multi-Factor Authentication zu konfigurieren. Wenn Sie Multi-Factor Authentication durchsetzen möchten, lesen Sie [Auswählen der Sicherheitslösung Multi-Factor Authentication](authentication/concept-mfa-whichversion.md). Darin ist eine Beschreibung der einzelnen Lösungen enthalten, und außerdem Links mit hilfreichen Informationen zum Konfigurieren der Lösung Ihrer Wahl.

## <a name="part-5-verification"></a>Teil 5: Überprüfung

Die Bereitstellung ist nun abgeschlossen, sodass Sie einige Szenarien ausprobieren können. Folgen Sie den unten angegebenen Links, um mit dem Dienst zu experimentieren und sich mit den Funktionen vertraut zu machen.

| Aufgabe | Verweis |
| --- | --- |
| Verknüpfen Sie einige Geräte mit dem Arbeitsplatz, indem Sie den Azure Active Directory-Geräteregistrierungsdienst verwenden. Sie können iOS-, Windows- und Android-Geräte verknüpfen. |[Beitritt zum Arbeitsplatz für Geräte mithilfe des Azure Active Directory-Geräteregistrierungsdiensts](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Zeigen Sie registrierte Geräte mithilfe des Administratorportals an, und aktivieren bzw. deaktivieren Sie sie. In dieser Aufgabe zeigen Sie einige registrierte Geräte mit dem Administratorportal an. |[Überblick über den Azure Active Directory-Geräteregistrierungsdienst](active-directory-device-registration-get-started.md) |
| Stellen Sie sicher, dass Geräteobjekte aus Azure Active Directory in Windows Server Active Directory zurückgeschrieben werden. |[Überprüfen, ob registrierte Geräte nach Active Directory zurückgeschrieben werden](#verify-registered-devices-are-written-back-to-active-directory) |
| Da Benutzer ihre Geräte jetzt registrieren können, können Sie Richtlinien für den Anwendungszugriff in AD FS erstellen, bei denen nur registrierte Geräte zugelassen sind. In dieser Aufgabe erstellen Sie eine Regel für den Anwendungszugriff und eine benutzerdefinierte Meldung „Zugriff verweigert“. |[Erstellen einer Anwendungszugriffsrichtlinie und einer benutzerdefinierten Meldung „Zugriff verweigert“](#create-an-application-access-policy-and-custom-access-denied-message) |

## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Integrieren von Azure Active Directory in die lokale Active Directory-Instanz

**Siehe:**

- [Integrieren Ihrer lokalen Verzeichnisse in Azure Active Directory](hybrid/whatis-hybrid-identity.md) (konzeptionelle Informationen)

- [Benutzerdefinierte Installation von Azure AD Connect](hybrid/how-to-connect-install-custom.md) (Installationsanweisungen)


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Ausführen eines Upgrades des Schemas der Active Directory Domain Services

> [!NOTE]
> Nach dem Upgrade des Active Directory-Schemas kann der Prozess nicht rückgängig gemacht werden. Es wird empfohlen, das Upgrade zuerst in einer Testumgebung durchzuführen.
> 

1. Melden Sie sich bei Ihrem Domänencontroller mit einem Konto an, das sowohl über Rechte eines Unternehmensadministrators als auch über Rechte eines Schemaadministrators verfügt.
1. Kopieren Sie das Verzeichnis **[media] \support\adprep** und die Unterverzeichnisse in einen Ihrer Active Directory-Domänencontroller (**[media]** steht hierbei für den Pfad zum Windows Server 2012 R2-Installationsdatenträger).
1. Navigieren Sie über eine Eingabeaufforderung zum Verzeichnis **adprep**, und führen Sie **adprep.exe/forestprep** aus. Folgen Sie den Anweisungen auf dem Bildschirm, um das Upgrade des Schemas abzuschließen.

## <a name="prepare-your-active-directory-to-support-devices"></a>Vorbereiten von Active Directory für die Unterstützung von Geräten

> [!NOTE]
> Dies ist ein einmaliger Vorgang, den Sie ausführen müssen, um Ihre Active Directory-Gesamtstruktur für die Unterstützung von Geräten vorzubereiten. Für die Durchführung dieses Vorgangs müssen Sie mit den Berechtigungen eines Unternehmensadministrators angemeldet sein, und Ihre Active Directory-Gesamtstruktur muss das Windows Server 2012 R2-Schema aufweisen.
> 


### <a name="prepare-your-active-directory-forest"></a>Vorbereiten der Active Directory-Gesamtstruktur

1. Öffnen Sie auf Ihrem Verbundserver ein Windows PowerShell-Befehlsfenster, und geben Sie dann **Initialize-ADDeviceRegistration** ein. 
1. Wenn Sie aufgefordert werden, einen Wert für **ServiceAccountName**einzugeben, geben Sie den Namen des Dienstkontos ein, das Sie als Dienstkonto für AD FS ausgewählt haben. Wenn es sich um ein gMSA-Konto handelt, geben Sie das Konto im Format **Domäne\Kontoname$** ein. Verwenden Sie das Format **Domäne\Kontoname**, um ein Domänenkonto anzugeben.

### <a name="enable-device-authentication-in-ad-fs"></a>Aktivieren der Geräteauthentifizierung in AD FS

1. Öffnen Sie auf Ihrem Verbundserver die AD FS-Verwaltungskonsole, und navigieren Sie zu **AD FS** > **Authentifizierungsrichtlinien**.
1. Wählen Sie im Bereich **Aktionen** die Option **Globale primäre Authentifizierung bearbeiten**.
1. Aktivieren Sie **Geräteauthentifizierung aktivieren**, und klicken Sie dann auf **OK**.
1. Standardmäßig entfernt AD FS regelmäßig nicht verwendete Geräte aus Active Directory. Deaktivieren Sie diese Aufgabe, wenn Sie den Azure Active Directory-Geräteregistrierungsdienst verwenden, damit Geräte in Azure verwaltet werden können.

### <a name="disable-unused-device-cleanup"></a>Deaktivieren der Bereinigung nicht verwendeter Geräte

Öffnen Sie auf Ihrem Verbundserver ein Windows PowerShell-Befehlsfenster, und geben Sie dann **Set-AdfsDeviceRegistration -MaximumInactiveDays 0** ein.

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Vorbereiten von Azure AD Connect für das Geräterückschreiben

Führen Sie „Teil 1: Vorbereiten von Azure AD Connect“ durch.

## <a name="join-devices-to-your-workplace-by-using-azure-active-directory-device-registration-service"></a>Beitritt zum Arbeitsplatz für Geräte mithilfe des Azure Active Directory-Geräteregistrierungsdiensts

### <a name="join-an-ios-device-by-using-azure-active-directory-device-registration"></a>Verknüpfen eines iOS-Geräts mithilfe der Azure Active Directory-Geräteregistrierung

Die Azure Active Directory-Geräteregistrierung verwendet einen drahtlosen Profilregistrierungsvorgang für iOS-Geräte. Dieser Vorgang wird gestartet, wenn der Benutzer über Safari eine Verbindung mit der Profilregistrierungs-URL herstellt. Die URL weist das folgende Format auf:

`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname>`

In diesem Fall steht `yourdomainname` für den Domänennamen, den Sie mit Azure Active Directory konfiguriert haben. Wenn Sie z.B. den Domänennamen „contoso.com“ verwenden, lautet die URL folgendermaßen:

`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

Diese URL kann auf unterschiedlichste Weise an die Benutzer kommuniziert werden. Beispielsweise wird als Vorgehensweise empfohlen, diese URL in einer benutzerdefinierten Anwendungsmeldung „Zugriff verweigert“ in AD FS zu veröffentlichen. Diese Informationen sind im folgenden Abschnitt enthalten: [Erstellen einer Anwendungszugriffsrichtlinie und einer benutzerdefinierten Meldung „Zugriff verweigert“](#create-an-application-access-policy-and-custom-access-denied-message).

### <a name="join-a-windows-81-device-by-using-azure-active-directory-device-registration"></a>Verknüpfen eines Windows 8.1-Geräts mithilfe der Azure Active Directory-Geräteregistrierung

1. Wählen Sie auf dem Windows 8.1-Gerät **PC-Einstellungen** > **Netzwerk** > **Arbeitsplatz**.
1. Geben Sie Ihren Benutzernamen im UPN-Format ein (z.B. **dan@contoso.com**).
1. Wählen Sie **Beitreten**.
1. Melden Sie sich bei Aufforderung mit Ihren Anmeldeinformationen an. Das Gerät ist jetzt verknüpft.

### <a name="join-a-windows-7-device-by-using-azure-active-directory-device-registration"></a>Verknüpfen eines Windows 7-Geräts mithilfe der Azure Active Directory-Geräteregistrierung

Zum Registrieren von in eine Domäne eingebundenen Windows 7-Geräten müssen Sie das [Softwarepaket für die Geräteregistrierung](https://www.microsoft.com/download/details.aspx?id=53554) bereitstellen.

Anweisungen zur Verwendung des Pakets finden Sie unter [Windows Installer-Pakete für Computer ohne Windows 10](devices/hybrid-azuread-join-control.md#control-windows-down-level-devices).

## <a name="verify-that-registered-devices-are-written-back-to-active-directory"></a>Überprüfen, ob registrierte Geräte nach Active Directory zurückgeschrieben werden

Sie können mithilfe von „LDP.exe“ oder des ADSI-Editors Geräteobjekte anzeigen und sicherstellen, dass diese zurück in Ihr Active Directory-Verzeichnis geschrieben wurden. Beide Tools sind in den Administratortools von Active Directory verfügbar.

Standardmäßig werden Geräteobjekte, die aus Azure Active Directory zurückgeschrieben werden, in derselben Domäne platziert wie in die Ihrer AD FS-Farm.

`CN=RegisteredDevices,defaultNamingContext`

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Erstellen einer Anwendungszugriffsrichtlinie und einer benutzerdefinierten Meldung „Zugriff verweigert“

Stellen Sie sich folgendes Szenario vor: Sie erstellen eine Anwendungsvertrauensstellung der vertrauenden Seite in AD FS und konfigurieren eine Ausstellungsautorisierungsregel , die nur registrierte Geräte zulässt. Nun dürfen ausschließlich registrierte Geräte auf die Anwendung zugreifen. 

Um den Zugriff auf die Anwendung für Ihre Benutzer zu vereinfachen, konfigurieren Sie eine benutzerdefinierte Meldung „Zugriff verweigert“, die Anweisungen zum Hinzufügen von Geräten enthält. Jetzt steht Ihren Benutzern ein nahtloses Verfahren zum Registrieren ihrer Geräte zur Verfügung, mit dem sie auf eine Anwendung zugreifen können.

Die folgenden Schritte zeigen, wie dieses Szenario implementiert wird.

> [!NOTE]
> Dieser Abschnitt setzt voraus, dass Sie bereits eine Vertrauensstellung der vertrauenden Seite für Ihre Anwendung in AD FS konfiguriert haben.
> 

1. Öffnen Sie das AD FS-Tool MMC, und wählen Sie dann **AD FS** > **Vertrauensstellungen** > **Vertrauensstellungen der vertrauenden Seite**.
1. Suchen Sie nach der Anwendung, für die diese neue Zugriffsregel gelten soll. Klicken Sie mit der rechten Maustaste auf die Anwendung, und wählen Sie dann **Anspruchsregeln bearbeiten** aus.
1. Klicken Sie auf die Registerkarte **Ausstellungsautorisierungsregeln**, und wählen Sie dann **Regel hinzufügen** aus.
1. Wählen Sie in der Dropdownliste **Anspruchsregelvorlage** die Option **Benutzern den Zugriff auf Grundlage eines eingehenden Anspruchs gewähren oder verweigern** aus. Klicken Sie anschließend auf **Weiter**.
1. Geben Sie im Feld **Anspruchsregelname** Folgendes ein: **Zugriff von registrierten Geräten zulassen**.
1. Wählen Sie in der Dropdownliste **Eingehender Anspruchstyp** den Eintrag **Ist registrierter Benutzer** aus.
1. Geben Sie im Feld **Wert des eingehenden Anspruchs** Folgendes ein: **true**.
1. Wählen Sie das Optionsfeld **Zugriff auf Benutzer mit diesem eingehenden Anspruch zulassen** aus.
1. Wählen Sie **Fertig stellen** und dann **Anwenden**.
1. Entfernen Sie alle Regeln, die weitreichendere Berechtigungen gewähren als die Regel, die Sie erstellt haben. Entfernen Sie beispielsweise die Standardregel **Allen Benutzern Zugriff gewähren**.

Ihre Anwendung ist nun so konfiguriert, dass der Zugriff nur erlaubt wird, wenn der Benutzer ein registriertes Gerät verwendet, das dem Arbeitsplatz hinzugefügt wurde. Weitere erweiterte Zugriffsrichtlinien finden Sie unter [Verwalten von Risiken mit zusätzlicher mehrstufiger Authentifizierung für sensible Anwendungen](https://technet.microsoft.com/library/dn280949.aspx).

Im nächsten Schritt konfigurieren Sie eine benutzerdefinierte Fehlermeldung für Ihre Anwendung. Diese Fehlermeldung informiert Benutzer, dass sie ihr Gerät dem Arbeitsplatz hinzufügen müssen, bevor sie auf die Anwendung zugreifen dürfen. Sie können eine benutzerdefinierte Anwendungsmeldung „Zugriff verweigert“ mithilfe von benutzerdefiniertem HTML und PowerShell erstellen.

Öffnen Sie auf Ihrem Verbundserver ein PowerShell-Befehlsfenster, und geben Sie dann den folgenden Befehl ein. Sie müssen Teile des Befehls durch Elemente ersetzen, die für Ihr System spezifisch sind:

`Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage`

Sie müssen Ihr Gerät registrieren, bevor Sie auf diese Anwendung zugreifen können.

**Wenn Sie ein iOS-Gerät verwenden, verwenden Sie diesen Link, um Ihr Gerät zu verknüpfen**:

`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com`

Dieses iOS-Gerät mit Ihrem Arbeitsplatz verknüpfen

Wenn Sie ein Windows 8.1-Gerät verwenden, können Sie Ihr Gerät über **PC-Einstellungen**> **Netzwerk** > **Arbeitsplatz** verknüpfen.

In den vorhergehenden Befehlen gibt **Name der Vertrauensstellung der vertrauenden Seite** den Namen des Vertrauensstellungsobjekts der vertrauenden Seite Ihrer Anwendung in AD FS an.
**yourdomain.com** steht für den Domänennamen, den Sie mit Azure Active Directory konfiguriert haben (z.B. „contoso.com“).
Stellen Sie sicher, dass alle Zeilenumbrüche (sofern vorhanden) aus dem HTML-Inhalt entfernt wurden, den Sie an das Cmdlet **Set-AdfsRelyingPartyWebContent** übergeben.

Wenn Benutzer nun über ein Gerät auf Ihre Anwendung zugreifen, das nicht mit dem Azure Active Directory-Geräteregistrierungsdienst registriert ist, wird ihnen ein Fehler angezeigt.
