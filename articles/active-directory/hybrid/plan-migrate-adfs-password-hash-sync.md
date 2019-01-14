---
title: 'Azure AD Connect: Migrieren vom Verbund zur Kennworthashsynchronisierung für Azure AD | Microsoft-Dokumentation'
description: Informationen zum Verschieben von Ihre Hybrididentitätsumgebung vom Verbund zur Kennworthashsynchronisierung.
services: active-directory
author: billmath
manager: mtillman
ms.reviewer: martincoetzer
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/13/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a14e630c23af3e0228bf4806851f29cfab199215
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54103977"
---
# <a name="migrate-from-federation-to-password-hash-synchronization-for-azure-ad"></a>Migrieren vom Verbund zur Kennworthashsynchronisierung für Azure AD
Das folgende Dokument enthält Anleitungen zum Verschieben von AD FS zur Kennworthashsynchronisierung.

>[!NOTE]
>Eine herunterladbare Kopie dieses Dokuments finden Sie [hier](https://aka.ms/ADFSTOPHSDPDownload).


## <a name="prerequisites-for-the-migration"></a>Voraussetzungen für die Migration 
Die folgenden Voraussetzungen müssen vor dem Migrieren erfüllt sein:
### <a name="update-azure-ad-connect"></a>Aktualisieren von Azure AD Connect

Für die erfolgreiche Durchführung der Schritte für die Migration zur Pass-Through-Authentifizierung sollte mindestens [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) Version 1.1.819.0 vorhanden sein. Diese Version enthält wesentliche Änderungen an der Art und Weise, wie die Konvertierung der Anmeldung durchgeführt wird, und reduziert die Gesamtzeit für die Migration vom Verbund zur Cloudauthentifizierung von Stunden auf Minuten.

> [!IMPORTANT]
> Veraltete Dokumentationen, Tools und Blogs zeigen, dass die Benutzerkonvertierung ein notwendiger Schritt bei der Konvertierung von Domänen von „Verbund“ in „Verwaltet“ ist. Beachten Sie, dass die Konvertierung von Benutzern nicht mehr erforderlich ist und Microsoft daran arbeitet, die Dokumentation und Tools zu aktualisieren, um dies zu berücksichtigen.

Um Azure AD Connect auf die neueste Version zu aktualisieren, folgen Sie diesen [Updateanweisungen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

### <a name="password-hash-synchronization-required-permissions"></a>Für die Kennworthashsynchronisierung erforderliche Berechtigungen

Azure AD Connect kann mithilfe von Express-Einstellungen oder der benutzerdefinierten Installation konfiguriert werden. Wenn Sie die Option „Benutzerdefinierte Installation“ verwendet haben, sind die [benötigten Berechtigungen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-accounts-permissions) für die Kennworthashsynchronisierung möglicherweise nicht vorhanden.

Um Kennworthashes synchronisieren zu können, benötigt das AD DS-Dienstkonto von Azure AD Connect die folgenden Berechtigungen.

* Verzeichnisänderungen replizieren

* Verzeichnisänderungen replizieren: Alle

Jetzt ist ein guter Zeitpunkt, um zu überprüfen, ob diese Berechtigungen für alle Bereiche vorhanden sind.

### <a name="plan-migration-method"></a>Planmigrationsmethode

Es gibt zwei Methoden, um von der Verbundauthentifizierung zu Kennworthashsynchronisierung und zum nahtlosen einmaligen Anmelden zu migrieren. Welche Methode Sie verwenden, hängt davon ab, wie AD FS ursprünglich konfiguriert wurde. 



- **Option A: Verwenden von Azure AD Connect**. Wenn AD FS ursprünglich mit Azure AD Connect konfiguriert wurde, muss die Änderung an der Kennworthashsynchronisierung als Anmeldeverfahren für den Benutzer über den Azure AD Connect-Assistenten durchgeführt werden.   
Wenn Sie Azure AD Connect verwenden, wird das Cmdlet „Set-MsolDomainAuthentication“ automatisch für Sie ausgeführt, wenn Sie die Benutzeranmeldeverfahren ändern, und Sie haben daher keine Kontrolle darüber, ob alle verifizierten Verbunddomänen in Ihrem Azure AD-Mandanten freigegeben werden.

> [!NOTE]
> Zum jetzigen Zeitpunkt können Sie nicht vermeiden, dass alle Domänen Ihres Mandanten aus dem Verbund entfernt werden, wenn Sie die Benutzeranmeldung auf Kennworthashsynchronisierung ändern, wenn AAD Connect ursprünglich zur Konfiguration von AD FS für Sie verwendet wurde.  



- **Option B: Verwenden von Azure AD Connect mit PowerShell**. Diese Methode kann nur verwendet werden, wenn AD FS ursprünglich nicht mit Azure AD Connect konfiguriert wurde. Sie müssen die Benutzeranmeldeverfahren über den Azure AD Connect-Assistenten noch ändern. Der Hauptunterschied besteht jedoch darin, dass das Cmdlet „Set-MsolDomainAuthentication“ nicht automatisch ausgeführt wird, da es keine Kenntnis von Ihrer AD FS-Farm hat, und Sie somit die volle Kontrolle darüber haben, welche Domänen in welcher Reihenfolge konvertiert werden.

Führen Sie die Schritte im folgenden Abschnitt aus, um die Methoden besser kennenzulernen.

#### <a name="verify-current-user-sign-in-settings"></a>Überprüfen der aktuellen Benutzeranmeldungseinstellungen

Überprüfen Sie Ihre aktuellen Einstellungen für die Benutzeranmeldung, indem Sie sich mit einem globalen Administratorkonto am Azure AD-Portal [https://aad.portal.azure.com](https://aad.portal.azure.com/) anmelden.

Überprüfen Sie im Abschnitt „Benutzeranmeldung“, dass „Verbund“ aktiviert und die „Nahtlose einmalige Anmeldung“ und „Pass-Through-Authentifizierung“ deaktiviert sind. Überprüfen Sie auch den Status der Kennwortsynchronisierung, der als deaktiviert angezeigt werden soll, sofern sie nicht zuvor aktiviert wurde.

![Bild 5](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image1.png)

#### <a name="verify-azure-ad-connect-configuration"></a>Überprüfen der Azure AD Connect-Konfiguration

   1. Rufen Sie Ihren Azure AD Connect-Server auf, und starten Sie Azure AD Connect. Wählen Sie anschließend „Konfigurieren“ aus. 
   2. Wählen Sie im Bildschirm „Zusätzliche Aufgaben“ die Option „Aktuelle Konfiguration anzeigen“ und anschließend „Weiter“.</br>
   ![Bild 31](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image2.png)</br>
   
   3. Überprüfen Sie im Bildschirm „Lösung prüfen“ den Status der Kennwortsynchronisierung.</br> 

   Wenn die Kennworthashsynchronisierung derzeit deaktiviert ist, müssen Sie die Schritte in dieser Anleitung befolgen, um sie zu aktivieren. Wenn für die Kennworthashsynchronisierung aktiviert ist, können Sie den Abschnitt [Schritt 1: Aktivieren der Kennworthashsynchronisierung](#step-1--enable-password-hash-synchronization) in dieser Anleitung überspringen.
   4. Scrollen Sie im Bildschirm „Lösung prüfen“ nach unten zu „Active Directory-Verbunddienste (AD FS)“</br>
 
   Wenn Sie die AD FS-Konfiguration in diesem Abschnitt finden, können Sie sicher davon ausgehen, dass AD FS ursprünglich über Azure AD Connect konfiguriert wurde, und daher kann die Konvertierung Ihrer Domäne(n) von Verbunddomänen in verwaltete Domänen über die Option „Benutzeranmeldung verwalten“ von Azure AD Connect gesteuert werden. Dieser Prozess ist im Abschnitt **Option A – Wechsel vom Verbund zur Kennworthashsynchronisierung mit Azure AD Connect** beschrieben.
   5. Wenn Sie die Active Directory-Verbunddienste in den aktuellen Einstellungen nicht finden, müssen Sie die Domänen manuell von „Verbund“ in „Verwaltet“ über PowerShell konvertieren. Das ist im Abschnitt **Option B – Wechsel vom Verbund zur Kennworthashsynchronisierung mithilfe von Azure AD Connect und PowerShell** beschrieben.

### <a name="document-current-federation-settings"></a>Dokumentieren der aktuellen Verbundeinstellungen

Die aktuelle Verbundeinstellung können Sie über die Ausführung des Cmdlets „Get-MsolDomainFederationSettings“ aufrufen.

Der Befehl lautet:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName YourDomain.extention | fl *
```

Beispiel: 

``` PowerShell
Get-MsolDomainFederationSettings -DomainName Contoso.com | fl *
```
Überprüfen Sie alle Einstellungen, die möglicherweise an Ihre Entwurfs- und Bereitstellungsdokumentation des Verbunds angepasst wurden, insbesondere „PreferredAuthenticationProtocol“, „SupportsMfa“ und „PromptLoginBehavior“.

Weitere Informationen zu den Auswirkungen dieser Einstellungen finden Sie unten.

[Unterstützung des Parameters „prompt=login“ der Active Directory-Verbunddienste (AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)  
‎[Set-MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> Wenn der Wert „SupportsMfa“ derzeit auf TRUE festgelegt ist, bedeutet dies, dass Sie eine lokale MFA-Lösung verwenden, um eine zweite Faktorabfrage in den Benutzerauthentifizierungsflow einzufügen. Dies funktioniert für Azure AD-Authentifizierungsszenarios nicht mehr. Sie müssen stattdessen den (cloudbasierten) Azure MFA-Dienst verwenden, um die gleiche Funktion auszuführen. Bewerten Sie Ihre MFA-Anforderungen sorgfältig, bevor Sie fortfahren, und stellen Sie sicher, dass Sie verstehen, wie Sie Azure MFA, die Auswirkungen auf die Lizenzierung und den Registrierungsprozess für Endbenutzer nutzen können, bevor Sie Ihre Domänen konvertieren. Ein detaillierter Bereitstellungsleitfaden für Azure MFA finden Sie unter [https://aka.ms/deploymentplans](https://aka.ms/deploymentplans).

#### <a name="backup-federation-settings"></a>Sichern von Verbundeinstellungen

Obwohl während dieses Vorgangs keine Änderungen an anderen vertrauenden Seiten auf Ihrer AD FS-Farm vorgenommen werden, sollten Sie sicherzustellen, dass Sie über eine aktuelle gültige Sicherung Ihrer AD FS-Farm verfügen, die wiederhergestellt werden kann. Dafür können Sie das kostenlose [AD FS Rapid Restore-Tool](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool) von Microsoft verwenden. Mit diesem Tool kann AD FS gesichert und entweder für eine vorhandene oder eine neue Farm wiederhergestellt werden.

Wenn Sie das AD FS Rapid Restore-Tool nicht verwenden möchten, sollten Sie mindestens die Vertrauensstellungen der vertrauenden Seite „Microsoft Office 365 Identity Platform“ und alle zugeordneten benutzerdefinierten Anspruchsregeln exportieren, die Sie möglicherweise hinzugefügt haben. Hierzu können das folgende PowerShell-Beispiel verwenden.

``` PowerShell
(Get-AdfsRelyingPartyTrust -Name "Microsoft Office 365 Identity Platform") | Export-CliXML "C:\temp\O365-RelyingPartyTrust.xml"
```

## <a name="deployment-considerations-and-ad-fs-usage"></a>Überlegungen zur Bereitstellung und Verwendung von AD FS

### <a name="validate-your-current-ad-fs-usage"></a>Überprüfen Ihrer aktuellen AD FS-Nutzung

Vor dem Konvertieren von „Verbund“ in „Verwaltet“ sollten Sie genau überprüfen, wie Sie AD FS derzeit für Azure AD/Office 365 und andere Anwendungen ( Vertrauensstellungen der vertrauenden Seite) verwenden. Dabei sollten Sie insbesondere die folgende Tabelle berücksichtigen:

| Wenn| Dann |
|-|-|
| Sie behalten AD FS für diese anderen Anwendungen bei.| Sie werden sowohl AD FS als auch Azure AD verwenden und müssen daher die Benutzerfreundlichkeit berücksichtigen. Benutzer müssen sich möglicherweise in einigen Szenarien zweimal authentifizieren, einmal für Azure AD (wo sie das einmalige Anmelden oder höher für andere Anwendungen wie Office 365 erhalten) und erneut für alle Anwendungen, die noch an AD FS als Vertrauensstellungen der vertrauenden Seite gebunden sind. |
| AD FS ist stark angepasst und hängt von bestimmten Anpassungseinstellungen in der Datei onload.js ab, die nicht in Azure AD dupliziert werden können (z.B. haben Sie die Anmeldeerfahrung so geändert, dass Benutzer nur ein SamAccountName-Format für ihren Benutzernamen eingeben und nicht ein UPN, oder ein starkes Branding für die Anmeldeerfahrung haben).| Sie müssen überprüfen, ob Ihre aktuellen Anpassungsanforderungen von Azure AD erfüllt werden können, bevor Sie fortfahren. Weitere Informationen und Anleitungen finden Sie in den Abschnitten „AD FS-Branding“ und „AD FS-Anpassung“.|
| Sie blockieren ältere Authentifizierungsclients über AD FS.| Ersetzen Sie die Steuerelemente zum Blockieren älterer Authentifizierungsclients, die derzeit in AD FS vorhanden sind, durch eine Kombination aus der [bedingten Zugriffssteuerung für ältere Authentifizierungsmethoden](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) und den [Clientzugriffsregeln in Exchange Online](https://aka.ms/EXOCAR).|
| Ihre Benutzer müssen MFA bei der Authentifizierung bei AD FS für die lokale MFA-Serverlösung ausführen.| Sie können keine MFA-Abfrage über die lokale MFA-Lösung in den Authentifizierungsablauf für eine verwaltete Domäne einfügen. Nach der Konvertierung der Domäne können Sie jedoch dafür den MFA-Dienst verwenden. Wenn Benutzer Azure MFA derzeit nicht verwenden, dann handelt es sich um einen einmaligen Registrierungsschritt für Endbenutzer, auf den Sie sich vorbereiten und mit Ihren Endbenutzern kommunizieren müssen.|
| Sie verwenden derzeit Zugriffssteuerungsrichtlinien (AuthZ-Regeln) in AD FS, um den Zugriff auf Office 365 zu steuern.| Ersetzen Sie diese durch die entsprechenden [Azure AD-Richtlinien für den bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) und die [Clientzugriffsregeln für Exchange Online](https://aka.ms/EXOCAR).|

### <a name="considerations-for-common-ad-fs-customizations"></a>Überlegungen zu allgemeinen AD FS-Anpassungen

#### <a name="inside-corporate-network-claim"></a>InsideCorporateNetwork-Anspruch

Der InsideCorporateNetwork-Anspruch wird von AD FS ausgegeben, wenn die Benutzerauthentifizierung innerhalb des Unternehmensnetzwerks stattfindet. Dieser Anspruch kann dann an Azure AD weitergeleitet und verwendet werden, um die Multi-Factor-Authentifizierung basierend auf dem Netzwerkstandort der Benutzer zu umgehen. Anweisungen, um festzustellen, ob dies derzeit in AD FS aktiviert ist, finden Sie in [Vertrauenswürdige IP-Adressen für Verbundbenutzer](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud).

Der InsideCorporateNetwork-Anspruch wird nicht mehr verfügbar sein, sobald Sie Ihre Domänen in die Kennworthashsynchronisierung konvertiert haben. [Benannte Orte in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) können verwendet werden, um diese Funktionalität zu ersetzen.

Wenn benannte Orte konfiguriert wurden, müssen alle Richtlinien für den bedingten Zugriff, die so konfiguriert sind, dass die Netzwerkadressen für alle vertrauenswürdigen Orte oder für durch MFA bestätigte IP-Adressen ein- oder ausgeschlossen werden, aktualisiert werden, um die neu erstellten benannten Orte anzugeben.

Weitere Informationen zu den Standortbedingungen beim bedingten Zugriff finden Sie in [Standorte für den bedingten Zugriff in Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).

#### <a name="hybrid-azure-ad-joined-devices"></a>In Azure AD eingebundene Hybridgeräte

Wenn Sie ein Gerät in Azure AD einbinden, können Sie bedingte Zugriffsregeln erstellen, die Geräte, die Ihren Zugriffsstandards entsprechen, aus Sicherheits- und Compliance-Gründen durchsetzen, und Benutzern ermöglichen, sich bei einem Gerät mit einem organisatorischen Arbeits- oder Schulkonto anstelle eines persönlichen Kontos anzumelden. Mit in Azure AD eingebundenen Hybridgeräten können Sie Ihre in die AD-Domäne eingebunden Geräte in Azure AD einbinden. Ihre Verbundumgebung wurde möglicherweise mit diesem Feature konfiguriert.

Um sicherzustellen, dass Hybrid Join auch nach der Konvertierung Ihrer Domänen in die Kennworthashsynchronisierung weiterhin für alle neuen Geräte funktioniert, die mit der Domäne verbunden sind, muss Azure AD Connect konfiguriert sein, um Active Directory-Computerkonten für Windows 10-Clients mit Azure AD zu synchronisieren. Für Computerkonten unter Windows 7 und Windows 8 verwendet Hybrid Join das nahtlose einmalige Anmelden, um den Computer in Azure AD zu registrieren, und Sie müssen sie nicht wie bei Windows 10-Geräten synchronisieren. Sie müssen jedoch eine aktualisierte workplacejoin.exe-Datei (über eine MSI-Datei) für diese Downlevelclients bereitstellen, damit sie sich mit dem nahtlosen einmaligen Anmelden registrieren können. [Laden Sie die .msi-Datei herunter](https://www.microsoft.com/download/details.aspx?id=53554). 

Weitere Informationen finden Sie unter [Konfigurieren von in Azure Active Directory eingebundenen Hybridgeräten](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup).

#### <a name="branding"></a>Branding

Ihre Organisation hat möglicherweise [Ihre AD FS-Anmeldeseiten](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) angepasst, um Informationen anzuzeigen, die für die Organisation relevanter sind. Wenn dies der Fall ist, sollten Sie entsprechende [Anpassungen auch für die Azure AD-Anmeldeseite in Betracht ziehen](https://docs.microsoft.com/azure/active-directory/customize-branding).

Es sind zwar ähnliche Anpassungen verfügbar, aber es können einige visuelle Änderungen auftreten. Sie sollten Ihre Endbenutzer auf diese zu erwartenden Änderungen vorbereiten.

> [!NOTE]
> Unternehmensbranding ist nur verfügbar, wenn Sie die Premium- oder Basic-Lizenz für Azure AD erworben haben oder über eine Office 365-Lizenz verfügen.

## <a name="planning-deployment-and-support"></a>Planen der Bereitstellung und des Supports

### <a name="plan-the-maintenance-window"></a>Planen des Wartungsfensters

Während die Domänenkonvertierung selbst relativ schnell erfolgt, kann Azure AD noch für einen Zeitraum von bis zu 4 Stunden nach Abschluss der Domänenkonvertierung einige Authentifizierungsanforderungen an Ihre AD FS-Server senden. Während dieses vierstündigen Fensters und abhängig von verschiedenen Caches auf der Dienstseite werden diese Authentifizierungen möglicherweise nicht von Azure AD akzeptiert, und die Benutzer erhalten ein Fehlermeldung, da sie sich weiterhin erfolgreich für AD FS authentifizieren können, aber Azure AD den von einem Benutzer ausgestellten Token nicht mehr akzeptiert, da diese Verbundvertrauensstellung entfernt wurde.

> [!NOTE]
> Dies wirkt sich nur auf Benutzer aus, die während dieses Fensters nach der Konvertierung über einen Browser auf die Dienste zugreifen, bis der Cache der Dienstseite geleert wird. Ältere Clients (Exchange ActiveSync, Outlook 2010/2013) sollten nicht betroffen sein, da Exchange Online einen Cache mit den Anmeldeinformationen für einen Zeitraum aufbewahrt, der zur stillen Neuauthentifizierung des Benutzers verwendet wird, ohne dass dieser zu AD FS zurückkehren muss. Die auf dem Gerät für diese Clients gespeicherten Anmeldeinformationen werden verwendet, um sich nach dem Löschen dieses Caches im Hintergrund neu zu authentifizieren, sodass Benutzer aufgrund der Domänenkonvertierung keine Aufforderung zur Eingabe des Kennworts erhalten sollten. Umgekehrt verwenden diese für moderne Authentifizierungsclients (Office 2013/2016, iOS- und Android-Apps) ein gültiges Aktualisierungstoken, um neue Zugriffstoken für den weiteren Zugriff auf Ressourcen zu erhalten, anstatt zu AD FS zurückzukehren. Dadurch sind diese vor Aufforderungen zur Eingabe des Kennworts geschützt und funktionieren weiterhin ohne zusätzliche Konfigurationen.

> [!IMPORTANT]
> Fahren Sie Ihre AD FS-Umgebung nicht herunter, und entfernen Sie die Vertrauensstellung der vertrauenden Seite in Office 365 nicht, bis Sie überprüft haben, ob alle Benutzer erfolgreich mit der Cloudauthentifizierung authentifiziert wurden.

### <a name="plan-for-rollback"></a>Planen der Zurücksetzung

Wenn ein größeres Problem erkannt wird und nicht schnell gelöst werden kann, können Sie die Lösung wieder auf „Verbund“ zurücksetzen. Es ist wichtig, zu planen, was zu tun ist, wenn Ihre Bereitstellung nicht wie gewünscht verläuft. Wenn die Konvertierung von Domänen oder Benutzern während der Bereitstellung fehlschlägt oder Sie zu „Verbund“ zurückkehren müssen, müssen Sie wissen, wie Sie jeglichen Ausfall minimieren und die Auswirkungen auf Ihre Benutzer reduzieren können.

#### <a name="rolling-back"></a>Zurücksetzen

Die entsprechenden Bereitstellungsdetails finden Sie in der Dokumentation zum Verbundentwurf und der Bereitstellung. Der Prozess muss Folgendes umfassen:

* Konvertieren von verwalteten Domänen in Verbunddomänen mithilfe von „Convert-MSOLDomainToFederated“ 

* Konfigurieren zusätzlicher Anspruchsregeln (falls erforderlich)

### <a name="plan-change-communications"></a>Planen der Kommunikation von Änderungen

Ein wichtiger Teil der Planung von Bereitstellung und Support ist es, sicherzustellen, dass Ihre Endbenutzer proaktiv über die Änderungen informiert werden und darüber, was auftreten kann bzw. welche Maßnahmen sie ergreifen müssen. 

Nachdem sowohl die Kennworthashsynchronisierung als auch das nahtlose einmalige Anmelden bereitgestellt wurden, ändert sich die Anmeldeerfahrung für den Endbenutzer beim Zugriff auf Office 365 und andere zugehörige Ressourcen, die durch Azure AD authentifiziert wurden. Benutzer außerhalb des Netzwerks sehen nun nur noch die Azure AD-Anmeldeseite, anstatt auf die formularbasierte Seite weitergeleitet zu werden, die von den externen Proxyservern der Webanwendung angezeigt wird.

Es gibt mehrere Elemente zur Planung Ihrer Kommunikationsstrategie. Das umfasst:

* Das Benachrichtigen der Benutzer über kommende und veröffentlichte Funktionen über:
  * E-Mails und andere Kanäle für die interne Kommunikation
  * Visuelle Aushänge, wie z. B. Poster
  * Direkte oder andere Kommunikation der Geschäftsleitung
* Das Festlegen, wer die Kommunikation anpasst und wer sie wann versendet

## <a name="implementing-your-solution"></a>Implementieren Ihrer Lösung

Nachdem Sie Ihre Lösung geplant haben, können Sie sie jetzt implementieren. Die Implementierung umfasst die folgenden Komponenten:

1. Aktivieren der Kennworthashsynchronisierung

2. Vorbereiten der nahtlosen einmaligen Anmeldung

3. Ändern der Anmeldemethode in die Kennworthashsynchronisierung und Aktivieren des nahtlosen einmaligen Anmeldens

### <a name="step-1--enable-password-hash-synchronization"></a>Schritt 1 – Aktivieren der Kennworthashsynchronisierung

Der erste Schritt zur Implementierung dieser Lösung ist die Aktivierung der Kennworthashsynchronisierung im Azure AD Connect-Assistenten. Die Kennworthashsynchronisierung ist eine optionale Funktion, die in Verbundumgebungen ohne Auswirkungen auf den Authentifizierungsablauf aktiviert werden kann. In diesem Fall startet Azure AD Connect das Synchronisieren von Kennworthashes ohne Auswirkungen auf Benutzer, die sich mithilfe einer Verbundanmeldung anmelden.

Aus diesem Grund empfehlen wir, diesen Schritt als Vorbereitungsaufgabe durchzuführen, bevor Sie Ihre Domänenanmeldemethode ändern. Dadurch haben Sie ausreichend Zeit, um zu überprüfen, ob die Kennworthashsynchronisierung ordnungsgemäß funktioniert.

So aktivieren Sie die Kennworthashsynchronisierung:

   1. Öffnen Sie den Assistenten auf dem Azure AD Connect-Server, und wählen Sie „Konfigurieren“ aus.
   2. Wählen Sie „Synchronisierungsoptionen anpassen“ und anschließend „Weiter“ aus.
   3. Geben Sie im Bildschirm „Mit Azure AD verbinden“ den Benutzernamen und das Kennwort eines globalen Administrators ein.
   4. Klicken Sie im Bildschirm „Verzeichnisse verbinden“ auf „Weiter“.
   5. Klicken Sie im Bildschirm „Filtern von Domänen und Organisationseinheiten“ auf „Weiter“.
   6. Wählen Sie im Bildschirm „Optionale Features“ die Option „Kennwortsynchronisierung“ und anschließend „Weiter“ aus.
   ![Bild 21](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image6.png)</br>
   7. Wählen Sie auf allen verbleibenden Bildschirmen „Weiter“ bzw. „Konfigurieren“ auf dem letzten Bildschirm aus.
   8. Azure AD Connect wird das Synchronisieren von Kennworthashes bei der nächsten Synchronisierung starten.

Sobald die Kennworthashsynchronisierung aktiviert ist, werden die Kennworthashes für alle Benutzer im Azure AD Connect Synchronisationsumfang neu gehasht und in Azure AD geschrieben. Je nach Anzahl der Benutzer kann dieser Vorgang einige Minuten oder auch einige Stunden dauern.

Für Planungszwecke sollten Sie davon ausgehen, dass ca. 20.000 Benutzer in einer Stunde verarbeitet werden können.

Um zu überprüfen, ob die Kennworthashsynchronisierung ordnungsgemäß funktioniert, verwenden Sie die Aufgabe zur Fehlerbehebung im Azure AD Connect-Assistenten.

   1. Öffnen Sie mit der Option „Als Administrator ausführen“ eine neue Windows PowerShell-Sitzung auf Ihrem Azure AD Connect-Server.
   2. Führen Sie „Set-ExecutionPolicy RemoteSigned“ oder „Set-ExecutionPolicy Unrestricted“ aus.
   3. Starten Sie den Azure AD Connect-Assistenten.
   4. Navigieren Sie zur Seite „Weitere Aufgaben“, wählen Sie „Problembehandlung“ aus, und klicken Sie auf „Weiter“.
   5. Klicken Sie auf der Seite „Problembehandlung“ auf „Starten“, um das Menü zur Problembehandlung in PowerShell zu starten.
   6. Wählen Sie im Hauptmenü „Problembehandlung“ bei der Kennworthashsynchronisierung.
   7. Wählen Sie im Untermenü Die „Kennworthashsynchronisierung“ funktioniert überhaupt nicht.

Bei Problemen verwenden Sie die Informationen in [diesem Artikel](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization) zur Problembehandlung.

### <a name="step-2--prepare-for-seamless-sso"></a>Schritt 2 – Vorbereiten für das nahtlose einmalige Anmelden

Damit Ihre Geräte das nahtlose einmalig Anmelden verwenden können, müssen Sie den Intranet-Zoneneinstellungen der Benutzer mithilfe der Gruppenrichtlinie in Active Directory eine Azure AD-URL hinzufügen.

Der Browser berechnet standardmäßig anhand der URL automatisch die richtige Zone (Internet oder Intranet). Beispielsweise ist „http://contoso/“ der Intranetzone und „http://intranet.contoso.com/“ der Internetzone zugeordnet (da die URL einen Punkt enthält). Browser senden keine Kerberos-Tickets an Cloudendpunkte wie die Azure AD-URL, sofern Sie die URL nicht explizit zur Intranetzone des Browsers hinzufügen.

Führen Sie die [Schritte](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) aus, um mit der Veröffentlichung der erforderlichen Änderungen für Ihre Geräte zu beginnen.

> [!IMPORTANT]
> Wenn Sie diese Änderung vornehmen, erfolgen keine Änderungen an der Art und Weise, wie sich Benutzer bei Azure AD anmelden. Es ist jedoch wichtig, dass diese Konfiguration auf alle Ihre Geräte angewendet wird, bevor Sie mit Schritt 3 fortfahren. Beachten Sie auch, dass Benutzer, die sich auf Geräten anmelden, die diese Konfiguration nicht erhalten haben, lediglich Benutzername und Kennwort eingeben müssen, um sich bei Azure AD anzumelden.

### <a name="step-3--change-sign-in-method-to-phs-and-enable-seamless-sso"></a>Schritt 3 – Ändern der Anmeldemethode in Kennworthashsynchronisierung und Aktivieren des nahtlosen einmaligen Anmeldens

#### <a name="option-a---switch-from-federation-to-phs-by-using-azure-ad-connect"></a>Option A – Wechseln vom Verbund zur Kennworthashsynchronisierung mit Azure AD Connect

Verwenden Sie diese Methode, wenn AD FS ursprünglich mit Azure AD Connect konfiguriert wurde. Sie können diese Methode nicht verwenden, wenn AD FS nicht ursprünglich mit Azure AD Connect konfiguriert wurde. Ändern Sie zuerst die **Benutzeranmeldemethode**.

   1. Öffnen Sie den Assistenten auf dem Azure AD Connect-Server.
   2. Wählen Sie „Benutzeranmeldung ändern“ und dann „Weiter“ aus.
   ![Bild 27](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image7.png)</br>
   3. Geben Sie im Bildschirm **Mit Azure AD verbinden** den Benutzernamen und das Kennwort eines **globalen Administrators** ein.
   4. Ändern Sie im Bildschirm **Benutzeranmeldung** das Optionsfeld von „Verbund mit AD FS“ in „Kennworthashsynchronisierung“, und stellen Sie sicher, dass das Kontrollkästchen „Benutzerkonten nicht konvertieren“ aktiviert ist, da dies ein veralteter Schritt ist und aus einer zukünftigen Version von AAD Connect entfernt wird. Wählen Sie außerdem „Einmaliges Anmelden aktivieren“ und anschließend **Weiter**.
   ![Bild 29](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image8.png)</br>
   
   > [!NOTE]
   > Ab Azure AD Connect Version 1.1.880.0 ist das Kontrollkästchen „Nahtloses einmaliges Anmelden“ standardmäßig aktiviert.
   
   > [!IMPORTANT]
   > Sie können die Warnungen, dass die Benutzerkonvertierung und die vollständige Kennworthashsynchronisierung für die Konvertierung von Verbund- zu Cloudauthentifizierung erforderlich sind, ignorieren. Bitte beachten Sie, dass diese Schritte nicht mehr erforderlich sind, da zukünftige Versionen von Azure AD Connect keine Möglichkeit bieten werden, Benutzer zu konvertieren. Wenn diese Warnungen weiterhin angezeigt werden, überprüfen Sie, ob Sie die neueste Version von Azure AD Connect und dieser Anleitung verwenden. Weitere Informationen hierzu finden Sie unter [Aktualisieren von Azure AD](#_Update_Azure_AD).
   
   5. Geben Sie auf dem Bildschirm „Einmaliges Anmelden aktivieren“ die Anmeldeinformationen des Domänenadministratorkontos ein, und wählen Sie „Weiter“ aus.
   ![Bild 35](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image9.png)</br>
   
   > [!NOTE]
   > Für die Aktivierung des nahtlosen einmaligen Anmeldens sind Anmeldeinformationen des Domänenadministrators erforderlich, da der Prozess die folgenden Aktionen durchführt, die diese erhöhten Berechtigungen erfordern. Die Anmeldeinformationen des Domänenadministrators werden weder in Azure AD Connect noch in Azure AD gespeichert. Sie werden nur verwendet, um das Feature zu aktivieren und werden anschließend wieder verworfen.
   >  * Ein Computerkonto namens „AZUREADSSOACC“ (das für Azure AD steht) wird in Ihrem lokalen Active Directory-Dienst (AD) erstellt.
   >  * Der Kerberos-Entschlüsselungsschlüssel des Computerkontos wird sicher für Azure AD freigegeben.
   >  * Darüber hinaus werden zwei Kerberos-Dienstprinzipalnamen (SPNs) erstellt, um die zwei URLs darzustellen, die während der Azure AD-Anmeldung verwendet werden.
   >  * Die Anmeldeinformationen des Domänenadministrators werden weder in Azure AD Connect noch in Azure AD gespeichert. Sie werden nur verwendet, um das Feature zu aktivieren und werden nach erfolgreichem Abschluss wieder verworfen.
   
   6. Vergewissern Sie sich im Bildschirm „Bereit zur Konfiguration“, dass das Kontrollkästchen „Synchronisierungsprozess starten, wenn die Konfiguration abgeschlossen ist“ aktiviert ist. Klicken Sie anschließend auf „Konfigurieren“.
   ![Bild 36](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image10.png)</br>
   
   > [!IMPORTANT]
   > Zu diesem Zeitpunkt werden alle Ihre Verbunddomänen auf verwaltete Authentifizierung umgestellt, die nun die Kennworthashsynchronisierung als Methode zur Authentifizierung nutzt.
       
   7. Öffnen Sie im Azure AD-Portal, wählen Sie „Azure Active Directory“ und dann „Azure AD Connect“.
   8. Stellen Sie sicher, dass „Verbund“ deaktiviert, aber „Nahtloses einmaliges Anmelden“ und „Kennwortsynchronisierung“ aktiviert sind.  
  ![Bild 37](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image11.png)</br>
   9. Wechseln Sie zu [„Testen“ und „Nächste Schritte“](#testing-and-next-steps).
   
   > [!IMPORTANT]
   > Überspringen Sie den Abschnitt „Option B – Wechsel vom Verbund zur Kennworthashsynchronisierung mithilfe von Azure AD Connect und PowerShell“, da die Schritte in diesem Abschnitt nicht angewendet werden.  

#### <a name="option-b---switch-from-federation-to-phs-using-azure-ad-connect-and-powershell"></a>Option B – Wechsel vom Verbund zur Kennworthashsynchronisierung mithilfe Azure AD Connect und PowerShell

Verwenden Sie diese Option, wenn Ihr Verbund ursprünglich nicht mithilfe von Azure AD Connect konfiguriert wurde.

Im Rahmen dieses Prozesses aktivieren Sie das nahtlose einmalige Anmelden und stellen Ihre Domänen von „Verbund“ auf „Verwaltet“ um.

   1. Öffnen Sie den Assistenten auf dem Azure AD Connect-Server.
   2. Wählen Sie „Benutzeranmeldung ändern“ und dann „Weiter“ aus. 
   3. Geben Sie im Bildschirm „Mit Azure AD verbinden“ den Benutzernamen und das Kennwort eines globalen Administrators ein.
   4. Ändern Sie im Bildschirm „Benutzeranmeldung“ das Optionsfeld von „Nicht konfigurieren“ in „Kennworthashsynchronisierung“, wählen Sie „Einmaliges Anmelden aktivieren“ und anschließend „Weiter“ aus.
   
   Vor der Änderung: ![Bild 20](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image12.png)</br>

   Nach der Änderung:  
   ![Bild 22](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image13.png)</br>
   
   > [!NOTE]
   > Ab Azure AD Connect Version 1.1.880.0 ist das Kontrollkästchen „Nahtloses einmaliges Anmelden“ standardmäßig aktiviert.
   
   5. Geben Sie auf dem Bildschirm „Einmaliges Anmelden aktivieren“ die Anmeldeinformationen des Domänenadministratorkontos ein, und wählen Sie „Weiter“ aus.
   
   > [!NOTE]
   > Für die Aktivierung des nahtlosen einmaligen Anmeldens sind Anmeldeinformationen des Domänenadministrators erforderlich, da der Vorgang die folgenden Aktionen durchführt, für die diese erhöhten Berechtigungen erforderlich sind. Die Anmeldeinformationen des Domänenadministrators werden weder in Azure AD Connect noch in Azure AD gespeichert. Sie werden nur verwendet, um das Feature zu aktivieren und werden anschließend wieder verworfen.
   > * Ein Computerkonto namens „AZUREADSSOACC“ (das für Azure AD steht) wird in Ihrem lokalen Active Directory-Dienst (AD) erstellt.
   > * Der Kerberos-Entschlüsselungsschlüssel des Computerkontos wird sicher für Azure AD freigegeben.
   > * Darüber hinaus werden zwei Kerberos-Dienstprinzipalnamen (SPNs) erstellt, um die zwei URLs darzustellen, die während der Azure AD-Anmeldung verwendet werden.
   
   6. Vergewissern Sie sich im Bildschirm „Bereit zur Konfiguration“, dass das Kontrollkästchen „Synchronisierungsprozess starten, wenn die Konfiguration abgeschlossen ist“ aktiviert ist. Klicken Sie anschließend auf „Konfigurieren“.
   ![Bild 41](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image15.png)</br>
   Bei der Auswahl von „Konfigurieren“ wird das nahtlose einmalige Anmelden gemäß den Schritten in der Vorschauversion konfiguriert. Die Konfiguration der Kennworthashsynchronisierung wird nicht geändert, da diese zuvor aktiviert wurde.
   
   > [!IMPORTANT]
   > An dieser Stelle werden keine Änderungen daran vorgenommen, wie sich die Benutzer anmelden.  
   
   7. Überprüfen Sie im Azure AD-Portal, dass „Verbund“ noch immer aktiviert ist und „Nahtloses einmaliges Anmelden“ aktiviert ist.
   ![Bild 42](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image16.png)

#### <a name="convert-domains-from-federated-to-managed"></a>Konvertieren von Domänen von Verbunddomänen in verwaltete Domänen

An dieser Stelle ist „Verbund“ immer noch aktiviert und kann für Ihre Domänen verwendet werden. Um mit der Bereitstellung fortzufahren, muss jede Domäne von „Verbund“ in „Verwaltet“ konvertiert werden, um die Benutzerauthentifizierung über Kennworthashsynchronisierung zu erzwingen.

> [!IMPORTANT]
> Nicht alle Domänen müssen gleichzeitig konvertiert werden, Sie können mit einer Testdomäne auf Ihrem Produktionsmandanten oder der Domäne mit der geringsten Anzahl von Benutzern beginnen.

Die Konvertierung erfolgt mithilfe des Azure AD PowerShell-Moduls.

   1. Öffnen Sie PowerShell, und melden Sie sich bei Azure AD mit einem globalen Administratorkonto an.  
   2. Verwenden Sie den folgenden Befehl, um die erste Domäne zu konvertieren:  
   
   ``` PowerShell
   Set-MsolDomainAuthentication -Authentication Managed -DomainName <domainname>
   ```
   
   3. Öffnen Sie im Azure AD-Portal, wählen Sie „Azure Active Directory“ und dann „Azure AD Connect“.
   4. Stellen Sie mithilfe des folgenden Befehls sicher, das die Domäne in eine verwaltete Domäne konvertiert wurde:
   
   ``` PowerShell
   Get-MsolDomain -DomainName <domainname>
   ```

## <a name="testing-and-next-steps"></a>Testen und nächste Schritte

### <a name="test-authentication-with-phs"></a>Testen der Authentifizierung mit Kennworthashsynchronisierung

Als Ihr Mandant mit „Verbund“ gearbeitet hat, wurden die Benutzer von der Azure AD-Anmeldeseite in Ihre AD FS-Umgebung umgeleitet. Da der Mandant jetzt so konfiguriert ist, dass anstelle von „Verbund“ die Kennworthashsynchronisierung verwendet wird, werden die Benutzer nicht mehr zu AD FS umgeleitet, sondern melden sich direkt über die Azure AD-Anmeldeseite an.

Öffnen Sie Internet Explorer im InPrivate-Modus, um zu vermeiden, dass Sie über das nahtlose einmalige Anmelden automatisch angemeldet werden, um navigieren Sie zur Office 365-Anmeldeseite ([https://portal.office.com](https://portal.office.com/)). Geben Sie den UPN des Benutzers ein, und klicken Sie auf „Weiter“. Stellen Sie sicher, dass Sie den UPN eines Hybridbenutzers eingeben, der von Ihrem lokalen Active Directory synchronisiert wurde und der zuvor zu einem Verbund gehörte. Dem Benutzer wird ein Bildschirm angezeigt, auf dem er seinen Benutzernamen und das Kennwort eingeben kann.

![Bild 9](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image18.png)

![Bild 12](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image19.png)

Nachdem Sie das Kennwort eingeben haben, sollten Sie zum Office 365-Portal umgeleitet werden.

![Bild 17](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image20.png)

### <a name="test-seamless-single-sign-on"></a>Testen des nahtlosen einmaligen Anmeldens

Melden Sie bei einem in der Domäne eingebundenen Computer an, der mit dem Unternehmensnetzwerk verbunden ist. Öffnen Sie den Internet Explorer, und navigieren Sie zu einer der folgenden URLs:  
  
[https://myapps.microsoft.com/contoso.com](https://myapps.microsoft.com/contoso.com) [https://myapps.microsoft.com/contoso.onmicrosoft.com](https://myapps.microsoft.com/contoso.onmicrosoft.com) (ersetzen Sie „Contoso“ durch Ihre Domäne).

Der Benutzer wird kurz auf die Azure AD-Anmeldeseite weitergeleitet und erhält die Meldung „Anmeldung wird versucht“ und sollte weder nach einem Benutzernamen noch nach einem Kennwort gefragt werden.

![Bild 24](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image21.png)

Anschließend wird der Benutzer umgeleitet und beim Zugriffsbereich angemeldet:

> [!NOTE]
> Das nahtlose einmalige Anmelden funktioniert für Office 365-Dienste, die Domänenhinweise unterstützten (z. B myapps.microsoft.com/contoso.com). Das Office 365-Portal (portal.office.com) unterstützt derzeit keine Domänenhinweise. Daher wird erwartet, dass Benutzer ihren UPN eingeben müssen. Sobald der UPN eingegeben wurde, kann das nahtlose einmalige Anmelden das Kerberos-Ticket im Namen des Benutzers abrufen und den Benutzer anmelden, ohne dass ein Kennwort eingegeben werden muss. 

> [!TIP]
> Erwägen Sie die Bereitstellung von [Azure AD Hybrid Join auf Windows 10](https://docs.microsoft.com/azure/active-directory/device-management-introduction), um das einmalige Anmelden zu verbessern.

### <a name="removal-of-the-relying-party-trust"></a>Entfernen der Vertrauensstellungen der vertrauenden Seite

Nachdem Sie überprüft habe, ob alle Benutzer und Clients erfolgreich über Azure AD authentifiziert werden konnten, können Sie die Vertrauensstellungen der vertrauenden Seite von Office 365 sicher entfernen.

Wenn AD FS nicht für andere Zwecke verwendet wird (andere Vertrauensstellungen für vertrauende Seiten wurden konfiguriert), kann AD FS jetzt sicher außer Betrieb genommen werden.

### <a name="rollback"></a>Rollback

Wenn ein größeres Problem erkannt wird und nicht schnell gelöst werden kann, können Sie die Lösung wieder auf „Verbund“ zurücksetzen.

Die entsprechenden Bereitstellungsdetails finden Sie in der Dokumentation zum Verbundentwurf und der Bereitstellung. Der Prozess muss Folgendes umfassen:

* Konvertieren von verwalteten Domänen in Verbunddomänen mithilfe von „Convert-MSOLDomainToFederated“

* Konfigurieren zusätzlicher Anspruchsregeln (falls erforderlich)

### <a name="enable-synchronization-of-userprincipalname-updates"></a>Aktivieren der Synchronisierung von userPrincipalName-Updates

Früher wurden Updates des userPrincipalName-Attributs mithilfe des Synchronisierungsdiensts vom lokalen Standort blockiert, sofern nicht die beiden folgenden Bedingungen erfüllt waren:

* Der Benutzer ist verwaltet (kein Verbundbenutzer).

* Dem Benutzer wurde keine Lizenz zugewiesen.

Anweisungen zum Überprüfen oder Aktivieren dieses Features finden Sie in folgendem Artikel:

[Synchronisieren von userPrincipalName-Updates](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsyncservice-features)

### <a name="troubleshooting"></a>Problembehandlung

Ihr Supportteam muss wissen, wie jegliche Authentifizierungsprobleme behoben werden können, die entweder während oder nach dem Wechsel von „Verbund“ zu „Verwaltet“ auftreten können. Verwenden Sie die folgende Dokumentation zur Problembehandlung, um Ihrem Supportteam zu helfen, sich mit den allgemeinen Schritten zur Problembehandlung und den entsprechenden Maßnahmen vertraut zu machen, die Ihnen helfen können, das Problem zu isolieren und zu beheben.

[Problembehandlung bei der Azure Active Directory-Kennworthashsynchronisierung](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization)

[Problembehandlung beim nahtlosen einmaligen Anmelden mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sso)  

## <a name="roll-over-the-seamless-sso-kerberos-decryption"></a>Durchführen des Rollovers für das nahtlose einmalige Anmelden für die Kerberos-Entschlüsselung

Es ist wichtig, häufig ein Rollover des Kerberos-Entschlüsselungsschlüssels des AZUREADSSOACC-Computerkontos durchzuführen, das Azure AD repräsentiert und in Ihrer lokalen AD-Gesamtstruktur erstellt wurde. Wir empfehlen Ihnen dringend, den Entschlüsselungsschlüssel von Kerberos mindestens alle 30 Tage zu verschieben, um die Anpassung daran vorzunehmen, wie die Mitglieder der Active Directory-Domäne Kennwortänderungen übermitteln. Da dem AZUREADSSOACC-Computerkonto kein zugeordnetes Gerät angefügt ist, muss der Rollover manuell durchgeführt werden.

Führen Sie diese Schritte auf dem lokalen Server aus, auf dem Sie Azure AD Connect ausführen, um den Rollover des Kerberos-Entschlüsselungsschlüssels einzuleiten.

[Wie kann ich ein Rollover des Kerberos-Entschlüsselungsschlüssels des AZUREADSSOACC-Computerkontos durchführen?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq)

## <a name="next-steps"></a>Nächste Schritte

- [Entwurfskonzepte für Azure AD Connect](plan-connect-design-concepts.md)
- [Auswählen der richtigen Authentifizierung](https://docs.microsoft.com/azure/security/azure-ad-choose-authn)
- [Unterstützte Topologien](plan-connect-design-concepts.md)
