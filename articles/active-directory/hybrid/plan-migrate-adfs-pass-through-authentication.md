---
title: 'Azure AD Connect: Migrieren vom Verbund zur Pass-Through-Authentifizierung für Azure AD | Microsoft-Dokumentation'
description: Hier erhalten Sie Informationen zum Verschieben Ihrer Hybrididentitätsumgebung vom Verbund zur Pass-Through-Authentifizierung.
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
ms.openlocfilehash: c7d236769d5e9adca0402affc2d0eccdf78a6837
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107751"
---
# <a name="migrate-from-federation-to-pass-through-authentication-for-azure-ad"></a>Migrieren vom Verbund zur Pass-Through-Authentifizierung für Azure AD
Das folgende Dokument enthält Anleitungen zum Verschieben von AD FS zur Pass-Through-Authentifizierung.

>[!NOTE]
>Eine herunterladbare Kopie dieses Dokuments finden Sie [hier](https://aka.ms/ADFSTOPTADPDownload).

## <a name="prerequisites-for-pass-through-authentication"></a>Voraussetzungen für die Pass-Through-Authentifizierung
Die folgenden Voraussetzungen müssen vor der Migration erfüllt sein:
### <a name="update-azure-ad-connect"></a>Aktualisieren von Azure AD Connect

Für die erfolgreiche Durchführung der Schritte für die Migration zur Pass-Through-Authentifizierung sollte mindestens [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) Version 1.1.819.0 vorhanden sein. Diese Version enthält wesentliche Änderungen an der Art und Weise, wie die Konvertierung der Anmeldung durchgeführt wird, und reduziert die Gesamtzeit für die Migration vom Verbund zur Cloudauthentifizierung von Stunden auf Minuten.

> [!IMPORTANT]
> Veraltete Dokumentationen, Tools und Blogs zeigen, dass die Benutzerkonvertierung ein notwendiger Schritt bei der Konvertierung von Domänen von „Verbund“ in „Verwaltet“ ist. Die **Konvertierung von Benutzern** ist nicht mehr erforderlich, und Microsoft arbeitet daran, die Dokumentation und Tools zu aktualisieren, um dies zu berücksichtigen.

Befolgen Sie diese [Updateanweisungen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version), um Azure AD Connect auf die neueste Version zu aktualisieren.

### <a name="plan-authentication-agent-number-and-placement"></a>Planen der Anzahl und Platzierung von Authentifizierungs-Agents

Die Pass-Through-Authentifizierung erfolgt durch die Bereitstellung von einfachen Agents auf dem Azure AD Connect-Server und auf Ihren lokalen Windows Server-Instanzen. Installieren Sie die Agents so nah wie möglich bei Ihren Active Directory-Domänencontrollern, um die Latenz zu verringern.

Für die meisten Kunden sind zwei oder drei Authentifizierungs-Agents ausreichend, um die erforderliche Kapazität und Hochverfügbarkeit zu erzielen. In einem Mandanten können zudem nur bis zu zwölf Agents registriert werden. Der erste Agent wird immer auf dem Azure AD Connect-Server installiert. Weitere Informationen zu den Einschränkungen und Bereitstellungsoptionen für Agents finden Sie in [diesem Artikel zur Schätzung des Netzwerkdatenverkehrs und zur Leistung](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-current-limitations).

### <a name="plan-migration-method"></a>Planen der Migrationsmethode

Es gibt zwei Methoden, um von der Verbundauthentifizierung zur Pass-Through-Authentifizierung und zum nahtlosen einmaligen Anmelden zu migrieren. Welche Methode Sie verwenden, hängt davon ab, wie AD FS ursprünglich konfiguriert wurde.

- **Verwenden von Azure AD Connect:** Wenn AD FS ursprünglich mit Azure AD Connect konfiguriert wurde, *muss* die Änderung in die Pass-Through-Authentifizierung als Anmeldeverfahren für den Benutzer über den Azure AD Connect-Assistenten durchgeführt werden.

Wenn Sie Azure AD Connect verwenden, wird das Cmdlet „Set-MsolDomainAuthentication“ automatisch für Sie ausgeführt, wenn Sie die Benutzeranmeldeverfahren ändern. Daher können Sie nicht steuern, ob alle verifizierten Verbunddomänen in Ihrem Azure AD-Mandanten freigegeben werden.  
‎  
> [!NOTE]
> Zum jetzigen Zeitpunkt können Sie nicht vermeiden, dass alle Domänen Ihres Mandanten aus dem Verbund entfernt werden, wenn Sie die Benutzeranmeldung in die Pass-Through-Authentifizierung ändern, wenn AAD Connect ursprünglich zur Konfiguration von AD FS für Sie verwendet wurde.  
‎
- **Verwenden von Azure AD Connect mit PowerShell:** Diese Methode kann nur verwendet werden, wenn AD FS ursprünglich nicht mit Azure AD Connect konfiguriert wurde. Sie müssen die Benutzeranmeldeverfahren über den Azure AD Connect-Assistenten noch ändern. Der Hauptunterschied besteht jedoch darin, dass das Cmdlet „Set-MsolDomainAuthentication“ nicht automatisch ausgeführt wird, da es keine Kenntnis von Ihrer AD FS-Farm hat, und Sie somit die volle Kontrolle darüber haben, welche Domänen in welcher Reihenfolge konvertiert werden.

Führen Sie die Schritte im folgenden Abschnitt aus, um die Methoden besser kennenzulernen.

#### <a name="verify-current-user-sign-in-settings"></a>Überprüfen der aktuellen Benutzeranmeldungseinstellungen

Überprüfen Sie Ihre aktuellen Einstellungen für die Benutzeranmeldung, indem Sie sich mit einem globalen Administratorkonto beim Azure AD-Portal [https://aad.portal.azure.com](https://aad.portal.azure.com/) anmelden. 

Überprüfen Sie im Abschnitt **Benutzeranmeldung**, ob **Verbund** auf **Aktiviert** und **Nahtlose einmalige Anmeldung** und **Pass-Through-Authentifizierung** auf **Deaktiviert** festgelegt ist. 

![Bild 5](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image1.png)

#### <a name="verify-how-federation-was-configured"></a>Überprüfen der Konfiguration des Verbunds

   1. Rufen Sie Ihren Azure AD Connect-Server auf, und starten Sie Azure AD Connect. Klicken Sie anschließend auf **Konfigurieren**.
   2. Klicken Sie auf dem Bildschirm **Zusätzliche Aufgaben** auf die Option **Aktuelle Konfiguration anzeigen** und anschließend auf **Weiter**.</br>
   ![Bild 31](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image2.png)</br>
   3. Scrollen Sie auf dem Bildschirm **Lösung prüfen** nach unten zu **Active Directory-Verbunddienste (AD FS)**.</br>
   ‎Wenn die AD FS-Konfiguration in diesem Abschnitt vorhanden ist, können Sie sicher davon ausgehen, dass AD FS ursprünglich über Azure AD Connect konfiguriert wurde. Daher kann die Konvertierung Ihrer Domäne(n) von Verbunddomänen in verwaltete Domänen über die Option **Benutzeranmeldung ändern** von Azure AD Connect gesteuert werden. Dieser Vorgang wird im Abschnitt **Option 1: Konfigurieren der Pass-Through-Authentifizierung mithilfe von Azure AD Connect** gesteuert werden.  
‎
   4. Wenn die Active Directory-Verbunddienste (AD FS) in den aktuellen Einstellungen nicht angezeigt werden, müssen Sie die Domänen über PowerShell manuell von „Verbund“ in „Verwaltet“ konvertieren. Dieser Vorgang wird im Abschnitt **Option 2: Wechsel vom Verbund zur Pass-Through-Authentifizierung mithilfe von Azure AD Connect und PowerShell** beschrieben.

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


Überprüfen Sie alle Einstellungen, die möglicherweise angepasst wurden, in Ihrer Entwurfs- und Bereitstellungsdokumentation des Verbunds, insbesondere **PreferredAuthenticationProtocol**, **SupportsMfa** und **PromptLoginBehavior**.

Weitere Informationen zu den Auswirkungen dieser Einstellungen finden Sie im Verlauf des Artikels.

[Unterstützung des Parameters „prompt=login“ der Active Directory-Verbunddienste (AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)  
‎[Set-MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> Wenn der Wert „SupportsMfa“ derzeit auf TRUE festgelegt ist, bedeutet dies, dass Sie eine lokale MFA-Lösung verwenden, um eine zweite Faktorabfrage in den Benutzerauthentifizierungsflow einzufügen. Dies funktioniert für Azure AD-Authentifizierungsszenarios nicht mehr. Sie müssen stattdessen den (cloudbasierten) Azure MFA-Dienst verwenden, um die gleiche Funktion auszuführen. Bewerten Sie Ihre MFA-Anforderungen sorgfältig, bevor Sie fortfahren, und stellen Sie sicher, dass Sie wissen, wie Sie Azure MFA, die Auswirkungen auf die Lizenzierung und den Registrierungsprozess für Endbenutzer einsetzen können, bevor Sie Ihre Domänen konvertieren.

#### <a name="backup-federation-settings"></a>Sichern von Verbundeinstellungen

Obwohl während dieses Vorgangs keine Änderungen an anderen vertrauenden Seiten auf Ihrer AD FS-Farm vorgenommen werden, sollten Sie sicherzustellen, dass Sie über eine aktuelle gültige Sicherung Ihrer AD FS-Farm verfügen, die wiederhergestellt werden kann. Dafür können Sie das kostenlose [AD FS Rapid Restore Tool](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool) von Microsoft verwenden. Mit diesem Tool kann AD FS gesichert und entweder für eine vorhandene oder eine neue Farm wiederhergestellt werden.

Wenn Sie das „AD FS Rapid Restore Tool“ nicht verwenden möchten, sollten Sie mindestens die Vertrauensstellungen der vertrauenden Seite „Microsoft Office 365 Identity Platform“ und alle zugeordneten benutzerdefinierten Anspruchsregeln exportieren, die Sie möglicherweise hinzugefügt haben. Hierzu können das folgende PowerShell-Beispiel verwenden.

``` PowerShell
(Get-AdfsRelyingPartyTrust -Name "Microsoft Office 365 Identity Platform") | Export-CliXML "C:\temp\O365-RelyingPartyTrust.xml"
```

## <a name="deployment-considerations-and-ad-fs-usage"></a>Überlegungen zur Bereitstellung und Verwendung von AD FS

### <a name="validate-your-current-ad-fs-usage"></a>Überprüfen Ihrer aktuellen AD FS-Nutzung

Vor dem Konvertieren von „Verbund“ in „Verwaltet“ sollten Sie genau überprüfen, wie Sie AD FS derzeit für Azure AD/Office 365 und andere Anwendungen ( Vertrauensstellungen der vertrauenden Seite) verwenden. Dabei sollten Sie insbesondere die folgende Tabelle berücksichtigen:

| Wenn| Dann |
|-|-|
| Sie behalten AD FS für diese anderen Anwendungen bei.| Sie werden sowohl AD FS als auch Azure AD verwenden und müssen daher die Benutzerfreundlichkeit berücksichtigen. Benutzer müssen sich möglicherweise in einigen Szenarios zweimal authentifizieren: einmal für Azure AD (wo anschließend das einmalige Anmelden für andere Anwendungen wie Office 365 verwendet wird) und erneut für alle Anwendungen, die noch an AD FS als Vertrauensstellungen der vertrauenden Seite gebunden sind. |
| AD FS ist stark angepasst und hängt von bestimmten Anpassungseinstellungen in der Datei „onload.js“ ab, die nicht in Azure AD dupliziert werden können (z. B. haben Sie die Anmeldung so verändert, dass Benutzer nur ein SamAccountName-Format für ihren Benutzernamen und keinen Benutzerprinzipalnamen eingeben müssen, oder es liegt ein starkes Branding für die Anmeldung vor).| Sie müssen überprüfen, ob Ihre aktuellen Anpassungsanforderungen von Azure AD erfüllt werden können, bevor Sie fortfahren. Weitere Informationen und Anleitungen finden Sie in den Abschnitten „AD FS-Branding“ und „AD FS-Anpassung“.|
| Sie blockieren ältere Authentifizierungsclients über AD FS.| Ersetzen Sie die Steuerelemente zum Blockieren älterer Authentifizierungsclients, die derzeit in AD FS vorhanden sind, durch eine Kombination aus der [bedingten Zugriffssteuerung für ältere Authentifizierungsmethoden](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) und den [Clientzugriffsregeln in Exchange Online](https://aka.ms/EXOCAR). |
| Ihre Benutzer müssen MFA bei der Authentifizierung bei AD FS für die lokale MFA-Serverlösung ausführen.| Sie können keine MFA-Abfrage über die lokale MFA-Lösung in den Authentifizierungsablauf für eine verwaltete Domäne einfügen. Nach der Konvertierung der Domäne können Sie dafür jedoch den MFA-Dienst verwenden. Wenn Ihre Benutzer Azure MFA derzeit nicht verwenden, ist hierfür ein einmaliger Registrierungsschritt für Endbenutzer erforderlich, auf den Sie sich und Ihre Endbenutzer vorbereiten müssen. |
| Sie verwenden derzeit Zugriffssteuerungsrichtlinien (AuthZ-Regeln) in AD FS, um den Zugriff auf Office 365 zu steuern.| Ersetzen Sie diese durch die entsprechenden [Azure AD-Richtlinien für den bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) und die [Clientzugriffsregeln für Exchange Online](https://aka.ms/EXOCAR).|

### <a name="considerations-for-common-ad-fs-customizations"></a>Überlegungen zu allgemeinen AD FS-Anpassungen

#### <a name="inside-corporate-network-claim"></a>InsideCorporateNetwork-Anspruch

Der InsideCorporateNetwork-Anspruch wird von AD FS ausgegeben, wenn die Benutzerauthentifizierung innerhalb des Unternehmensnetzwerks stattfindet. Dieser Anspruch kann dann an Azure AD weitergeleitet und verwendet werden, um die Multi-Factor-Authentifizierung basierend auf dem Netzwerkstandort der Benutzer zu umgehen. Weitere Informationen dazu, wie Sie feststellen können, ob diese Einstellung derzeit in AD FS aktiviert ist, finden Sie unter [Vertrauenswürdige IP-Adressen für Verbundbenutzer](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud).

Der InsideCorporateNetwork-Anspruch ist nicht mehr verfügbar, sobald Sie Ihre Domänen in die Pass-Through-Authentifizierung konvertiert haben. [Benannte Orte in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) können verwendet werden, um diese Funktionalität zu ersetzen.

Wenn benannte Orte konfiguriert wurden, müssen alle Richtlinien für den bedingten Zugriff, die so konfiguriert sind, dass die Netzwerkadressen für alle vertrauenswürdigen Orte oder für durch MFA bestätigte IP-Adressen ein- oder ausgeschlossen werden, aktualisiert werden, um die neu erstellten benannten Orte anzugeben.

Weitere Informationen zu den Standortbedingungen beim bedingten Zugriff finden Sie in [Standorte für den bedingten Zugriff in Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).

#### <a name="hybrid-azure-ad-joined-devices"></a>Mit Azure AD Hybrid Join verknüpfte Geräte

Wenn Sie ein Gerät mit Azure AD verknüpfen, können Sie bedingte Zugriffsregeln erstellen, durch die erzwungen wird, dass Geräte Ihren Zugriffsstandards für Sicherheit- und Konformität entsprechen, und es Benutzern ermöglichen, sich bei einem Gerät mit einem Geschäfts-, Schul- oder Unikonto der Organisation statt mit einem persönlichen Konto anzumelden. Über Azure AD Hybrid Join können Sie Ihre mit die AD-Domäne verknüpften Geräte mit Azure AD verknüpfen. Ihre Verbundumgebung wurde möglicherweise mit diesem Feature konfiguriert.

Um sicherzustellen, dass Hybrid Join auch nach der Konvertierung Ihrer Domänen in die Pass-Through-Authentifizierung weiterhin für alle neuen Geräte funktioniert, die mit der Domäne verknüpft sind, muss Azure AD Connect dafür konfiguriert sein, Active Directory-Computerkonten für Windows 10-Clients mit Azure AD zu synchronisieren. Für Computerkonten unter Windows 7 und Windows 8 verwendet Hybrid Join das nahtlose einmalige Anmelden, um den Computer in Azure AD zu registrieren. Sie müssen diese nicht wie bei Windows 10-Geräten synchronisieren. Sie müssen jedoch eine aktualisierte workplacejoin.exe-Datei (über eine MSI-Datei) für diese Downlevelclients bereitstellen, damit sie sich mit dem nahtlosen einmaligen Anmelden registrieren können. [Laden Sie die MSI-Datei herunter](https://www.microsoft.com/download/details.aspx?id=53554).

Weitere Informationen zu dieser Voraussetzung finden Sie unter [How to configure hybrid Azure Active Directory joined devices (Konfigurieren von mit Azure Active Directory verknüpften Hybridgeräten)](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup).

#### <a name="branding"></a>Branding

Ihre Organisation hat möglicherweise [Ihre AD FS-Anmeldeseiten](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) angepasst, um Informationen anzuzeigen, die für die Organisation relevanter sind. Wenn dies der Fall ist, sollten Sie entsprechende [Anpassungen auch für die Azure AD-Anmeldeseite in Betracht ziehen](https://docs.microsoft.com/azure/active-directory/customize-branding).

Es sind zwar ähnliche Anpassungen verfügbar, aber es können einige visuelle Änderungen auftreten. Sie sollten Ihre Endbenutzer auf diese zu erwartenden Änderungen vorbereiten.

> [!NOTE]
> Unternehmensbranding ist nur verfügbar, wenn Sie die Premium- oder Basic-Lizenz für Azure AD erworben haben oder über eine Office 365-Lizenz verfügen.

## <a name="planning-for-smart-lockout"></a>Planen der intelligenten Sperre

Durch die intelligente Sperre von Azure AD werden Ihre Systeme vor Brute-Force-Angriffen auf Ihre Kennwörter geschützt, und es wird verhindert, dass das lokale Active Directory-Konto gesperrt werden kann, wenn die Pass-Through-Authentifizierung verwendet und eine Gruppenrichtlinie für das Sperren von Konten in Active Directory festgelegt wird. 

Weitere Informationen zur intelligenten Sperre und deren Konfiguration finden Sie [hier](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout).

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

Ein wichtiger Teil der Planung von Bereitstellung und Support ist es, sicherzustellen, dass Ihre Endbenutzer proaktiv über Änderungen bzw. darüber, welche Maßnahmen sie ergreifen müssen, informiert werden. 

Nachdem sowohl die Pass-Through-Authentifizierung als auch das nahtlose einmalige Anmelden bereitgestellt wurden, ändert sich die Anmeldung für den Endbenutzer, wenn dieser auf Office 365 und andere zugehörige Ressourcen zugreift, die durch Azure AD authentifiziert wurden. Benutzern außerhalb des Netzwerks wird nun nur noch die Azure AD-Anmeldeseite angezeigt, anstatt auf die formularbasierte Seite weitergeleitet zu werden, die von den externen Proxyservern der Webanwendung angezeigt wird.

Es gibt mehrere Elemente zur Planung Ihrer Kommunikationsstrategie. Das umfasst:

* Das Benachrichtigen der Benutzer über kommende und veröffentlichte Funktionen über:
  * E-Mails und andere Kanäle für die interne Kommunikation
  * Visuelle Aushänge, wie z. B. Poster
  * Direkte oder andere Kommunikation der Geschäftsleitung
* Das Festlegen, wer die Kommunikation anpasst und wer sie wann versendet

## <a name="implementing-your-solution"></a>Implementieren Ihrer Lösung

Nachdem Sie Ihre Lösung geplant haben, können Sie sie jetzt implementieren. Die Implementierung umfasst die folgenden Komponenten:

   1. Vorbereiten der nahtlosen einmaligen Anmeldung
   2. Ändern der Anmeldemethode in „Pass-Through-Authentifizierung“ und Aktivieren des nahtlosen einmaligen Anmeldens

## <a name="step-1--prepare-for-seamless-sso"></a>Schritt 1: Vorbereitungen für das nahtlose einmalige Anmelden

Damit Ihre Geräte das nahtlose einmalig Anmelden verwenden können, müssen Sie den Intranetzoneneinstellungen der Benutzer mithilfe der Gruppenrichtlinie in Active Directory eine Azure AD-URL hinzufügen.

Der Browser berechnet standardmäßig anhand der URL automatisch die richtige Zone (Internet oder Intranet). Beispielsweise ist „http://contoso/“ der Intranetzone und „http://intranet.contoso.com/“ der Internetzone zugeordnet (da die URL einen Punkt enthält). Browser senden keine Kerberos-Tickets an Cloudendpunkte wie die Azure AD-URL, sofern Sie die URL nicht explizit zur Intranetzone des Browsers hinzufügen.

Führen Sie die [Schritte](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) aus, um mit der Veröffentlichung der erforderlichen Änderungen für Ihre Geräte zu beginnen.

> [!IMPORTANT]
> Wenn Sie diese Änderung vornehmen, erfolgen keine Änderungen an der Art und Weise, wie sich Benutzer bei Azure AD anmelden. Es ist jedoch wichtig, dass diese Konfiguration auf alle Ihre Geräte angewendet wird, bevor Sie mit Schritt 3 fortfahren. Beachten Sie außerdem, dass Benutzer, die sich auf Geräten anmelden, auf denen diese Konfiguration noch nicht implementiert wurde, nur einen Benutzernamen und ein Kennwort eingeben müssen, um sich bei Azure AD anzumelden.

## <a name="step-2--change-sign-in-method-to-pta-and-enable-seamless-sso"></a>Schritt 2: Ändern der Anmeldemethode in „Pass-Through-Authentifizierung“ und Aktivieren des nahtlosen einmaligen Anmeldens

### <a name="option-a-configuring-pta-by-using-azure-ad-connect"></a>Option A: Konfigurieren der Pass-Through-Authentifizierung mithilfe von Azure AD Connect

Verwenden Sie diese Methode, wenn AD FS ursprünglich mit Azure AD Connect konfiguriert wurde. Sie können diese Methode nicht verwenden, wenn AD FS ursprünglich nicht mit Azure AD Connect konfiguriert wurde.

> [!IMPORTANT]
> Beachten Sie, dass alle Domänen von „Verbund“ in „Verwaltet“ konvertiert werden, wenn sie diese Schritte befolgen. Weitere Informationen hierzu finden Sie im Abschnitt „Planen der Migrationsmethode“.[](#_Plan_Migration_Method)

Zunächst müssen Sie die Anmeldemethode ändern:

   1. Öffnen Sie den Assistenten auf dem Azure AD Connect-Server.
   2. Klicken Sie auf **Benutzeranmeldung ändern** und dann auf **Weiter**. 
   3. Geben Sie auf dem Bildschirm **Mit Azure AD verbinden** den Benutzernamen und das Kennwort eines globalen Administrators ein.
   4. Ändern Sie auf dem Bildschirm **Benutzeranmeldung** das Optionsfeld von **Verbund mit AD FS** in **Pass-Through-Authentifizierung**, und klicken Sie auf **Einmaliges Anmelden aktivieren** und anschließend auf **Weiter**.
   5. Geben Sie auf dem Bildschirm „Einmaliges Anmelden aktivieren“ die Anmeldeinformationen des Domänenadministratorkontos ein, und klicken Sie auf „Weiter“.  

   > [!NOTE]
   > Für die Aktivierung des nahtlosen einmaligen Anmeldens sind Anmeldeinformationen des Domänenadministrators erforderlich, da der Vorgang die folgenden Aktionen durchführt, für die diese erhöhten Berechtigungen erforderlich sind. Die Anmeldeinformationen des Domänenadministrators werden weder in Azure AD Connect noch in Azure AD gespeichert. Sie werden nur verwendet, um das Feature zu aktivieren und werden anschließend wieder verworfen.
   >
   > * Ein Computerkonto namens „AZUREADSSOACC“ (das für Azure AD steht) wird in Ihrem lokalen Active Directory-Dienst (AD) erstellt.
   > * Der Kerberos-Entschlüsselungsschlüssel des Computerkontos wird sicher für Azure AD freigegeben.
   > * Darüber hinaus werden zwei Kerberos-Dienstprinzipalnamen (SPNs) erstellt, um die zwei URLs darzustellen, die während der Azure AD-Anmeldung verwendet werden.
   > * Die Anmeldeinformationen des Domänenadministrators werden weder in Azure AD Connect noch in Azure AD gespeichert. Sie werden nur verwendet, um das Feature zu aktivieren und werden anschließend wieder verworfen.

   6. Vergewissern Sie sich im Bildschirm **Bereit zur Konfiguration**, dass das Kontrollkästchen **Start Synchronization process when configuration completes** (Synchronisierung starten, wenn die Konfiguration abgeschlossen ist) aktiviert ist. Klicken Sie anschließend auf **Konfigurieren**.</br>
   ![Bild](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image8.png)</br>
   7. Öffnen Sie das **Azure AD-Portal**, und klicken Sie auf **Azure Active Directory** und dann auf **Azure AD Connect**.
   8. Stellen Sie sicher, dass **Verbund** auf „Aktiviert“ festgelegt ist und **Nahtloses einmaliges Anmelden** und **Pass-Through-Authentifizierung** auf **Aktiviert** festgelegt sind.</br>
   ![Bild](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image9.png)</br>

Als Nächstes müssen Sie zusätzliche Authentifizierungsmethoden bereitstellen. Öffnen Sie das **Azure-Portal**, navigieren Sie zu **Azure Active Directory > Azure AD Connect**, und klicken Sie auf **Pass-Through-Authentifizierung**.

Klicken Sie auf der Seite **Pass-Through-Authentifizierung** auf „Herunterladen“. Klicken Sie auf dem Agent-Bildschirm **Herunterladen** auf **Accept terms and download** (Bedingungen akzeptieren und Download starten).

Der Download der zusätzlichen Authentifizierungs-Agents wird dann gestartet. Installieren Sie den sekundären Authentifizierungs-Agent auf einem mit einer Domäne verknüpften Server. 

> [!NOTE]
> Der erste Agent wird immer auf dem Azure AD Connect-Server als Teil der Änderungen an der Konfiguration installiert, die im Abschnitt „Benutzeranmeldung“ von Azure AD Connect vorgenommen werden. Alle zusätzlichen Authentifizierungs-Agents sollten auf einem separaten Server installiert werden. Es wird empfohlen, zwei bis drei zusätzliche Authentifizierungs-Agents bereitzustellen. 

Führen Sie die Installation des Authentifizierungs-Agents aus. Während der Installation müssen Sie die Anmeldeinformationen eines **globalen Administratorkontos** angeben.

![Bild 1243067202](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image11.png)

![Bild 1243067210](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image12.png)

Sobald der Authentifizierungs-Agent installiert wurde, können Sie zur Integritätsseite „Pass-Through-Authentifizierung“ für Agents zurückkehren, um den Status der zusätzlichen Agents zu überprüfen.


Wechseln Sie zu „Testen“ und „Nächste Schritte“.

> [!IMPORTANT]
> Überspringen Sie den Abschnitt „Option B: Wechsel vom Verbund zur Pass-Through-Authentifizierung mithilfe von Azure AD Connect und PowerShell“, da diese Schritte in diesem Fall nicht angewendet werden.  

### <a name="option-b---switch-from-federation-to-pta-using-azure-ad-connect-and-powershell"></a>Option B: Wechsel vom Verbund zur Pass-Through-Authentifizierung mithilfe Azure AD Connect und PowerShell

Verwenden Sie diese Option, wenn Ihr Verbund ursprünglich nicht mithilfe von Azure AD Connect konfiguriert wurde. Zunächst müssen Sie die Pass-Through-Authentifizierung aktivieren:

   1. Öffnen Sie den Assistenten auf dem Azure AD Connect-Server.
   2. Klicken Sie auf **Benutzeranmeldung ändern** und dann auf **Weiter**.
   3. Geben Sie auf dem Bildschirm **Mit Azure AD verbinden** den Benutzernamen und das Kennwort eines globalen Administrators ein.
   4. Ändern Sie auf dem Bildschirm **Benutzeranmeldung** das Optionsfeld von **Nicht konfigurieren** in **Pass-Through-Authentifizierung**, und klicken Sie auf **Einmaliges Anmelden aktivieren** und anschließend auf **Weiter**.
   5. Geben Sie auf dem Bildschirm **Einmaliges Anmelden aktivieren** die Anmeldeinformationen des Domänenadministratorkontos ein, und klicken Sie auf **Weiter**.

   > [!NOTE]
   > Für die Aktivierung des nahtlosen einmaligen Anmeldens sind Anmeldeinformationen des Domänenadministrators erforderlich, da der Vorgang die folgenden Aktionen durchführt, für die diese erhöhten Berechtigungen erforderlich sind. Die Anmeldeinformationen des Domänenadministrators werden weder in Azure AD Connect noch in Azure AD gespeichert. Sie werden nur verwendet, um das Feature zu aktivieren und werden anschließend wieder verworfen.
   >
   > * Ein Computerkonto namens „AZUREADSSOACC“ (das für Azure AD steht) wird in Ihrem lokalen Active Directory-Dienst (AD) erstellt.
   > * Der Kerberos-Entschlüsselungsschlüssel des Computerkontos wird sicher für Azure AD freigegeben.
   > * Darüber hinaus werden zwei Kerberos-Dienstprinzipalnamen (SPNs) erstellt, um die zwei URLs darzustellen, die während der Azure AD-Anmeldung verwendet werden.

   6. Vergewissern Sie sich im Bildschirm **Bereit zur Konfiguration**, dass das Kontrollkästchen **Start Synchronization process when configuration completes** (Synchronisierung starten, wenn die Konfiguration abgeschlossen ist) aktiviert ist. Klicken Sie anschließend auf **Konfigurieren**.</br>
   ‎![Bild](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image18.png)</br>
   Wenn Sie auf „Konfigurieren“ klicken, werden folgende Schritte durchgeführt:
   * Der erste Agent für die Pass-Through-Authentifizierung wird installiert.
   * Das Pass-Through-Feature wird aktiviert.
   * Das nahtlose einmalige Anmelden wird aktiviert.  
   7. Stellen Sie sicher, dass **Verbund** weiterhin auf **Aktiviert** festgelegt ist und **Nahtloses einmaliges Anmelden** und **Pass-Through-Authentifizierung** nun ebenfalls auf „Aktiviert“ festgelegt sind.
   ![Bild 2](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image19.png)
   8. Klicken Sie auf **Pass-Through-Authentifizierung**, und überprüfen Sie, ob der Status **Aktiv** lautet.</br>
   
   Wenn der Authentifizierungs-Agent nicht aktiv ist, sollten Sie diese [Schritte zur Problembehandlung](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-pass-through-authentication) durchführen, bevor Sie mit der Domänenkonversation im nächsten Schritt fortfahren. Es kann zu einem Authentifizierungsausfall kommen, wenn Sie Ihre Domänen konvertieren, bevor Sie überprüft haben, ob die Agents für die Pass-Through-Authentifizierung erfolgreich installiert wurden und ob deren Status im Azure-Portal als „Aktiv“ angezeigt wird.  
   9. Stellen Sie als Nächstes zusätzliche Authentifizierungs-Agents bereit. Öffnen Sie das **Azure-Portal**, navigieren Sie zu **Azure Active Directory** > **Azure AD Connect**, und klicken Sie auf **Pass-Through-Authentifizierung**.
   10. Klicken Sie auf der Seite **Pass-Through-Authentifizierung** auf **Herunterladen**. Klicken Sie auf dem Agent-Bildschirm **Herunterladen** auf **Accept terms and download** (Bedingungen akzeptieren und Download starten).
   
   Der Download der zusätzlichen Authentifizierungs-Agents wird dann gestartet. Installieren Sie den sekundären Authentifizierungs-Agent auf einem mit einer Domäne verknüpften Server.

  > [!NOTE]
  > Der erste Agent wird immer auf dem Azure AD Connect-Server als Teil der Änderungen an der Konfiguration installiert, die im Abschnitt „Benutzeranmeldung“ von Azure AD Connect vorgenommen werden. Alle zusätzlichen Authentifizierungs-Agents sollten auf einem separaten Server installiert werden. Es wird empfohlen, zwei bis drei zusätzliche Authentifizierungs-Agents bereitzustellen.
  
   11. Führen Sie die Installation des Authentifizierungs-Agents aus. Während der Installation müssen Sie die Anmeldeinformationen eines **globalen Administratorkontos** angeben.</br>
   ![Bild 1243067215](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image23.png)</br>
   ![Bild 1243067216](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image24.png)</br>
   12. Sobald der Authentifizierungs-Agent installiert wurde, können Sie zur Integritätsseite „Pass-Through-Authentifizierung“ für Agents zurückkehren, um den Status der zusätzlichen Agents zu überprüfen.

An dieser Stelle ist „Verbund“ immer noch aktiviert und kann für Ihre Domänen verwendet werden. Damit Sie mit der Bereitstellung fortfahren können, muss jede Domäne von „Verbund“ in „Verwaltet“ konvertiert werden, sodass Authentifizierungsanforderungen für die Domäne über die Pass-Through-Authentifizierung verarbeitet werden.

Nicht alle Domänen müssen gleichzeitig konvertiert werden. Sie können mit einer Testdomäne auf Ihrem Produktionsmandanten oder mit der Domäne mit der geringsten Anzahl von Benutzern beginnen.

Die Konvertierung erfolgt mithilfe des Azure AD PowerShell-Moduls.

   1. Öffnen Sie **PowerShell**, und melden Sie sich bei Azure AD mit einem **globalen Administratorkonto** an.
   2. Führen Sie folgenden Befehl aus, um die erste Domäne zu konvertieren:
 
 ``` PowerShell
 Set-MsolDomainAuthentication -Authentication Managed -DomainName <domainname>
 ```
 
   3. Öffnen Sie das **Azure AD-Portal**, und klicken Sie auf **Azure Active Directory** und dann auf **Azure AD Connect**.  
   4. Sobald Sie alle Verbunddomänen konvertiert haben, sollten Sie überprüfen, ob **Verbund** auf „Deaktiviert“ festgelegt ist und **Nahtloses einmaliges Anmelden** und **Pass-Through-Authentifizierung** auf **Aktiviert** festgelegt sind.</br>
   ![Bild](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image26.png)</br>

## <a name="testing-and-next-steps"></a>Testen und nächste Schritte

### <a name="test-pass-through-authentication"></a>Testen der Pass-Through-Authentifizierung 

Als Ihr Mandant mit der Einstellung „Verbund“ verwendet wurde, wurden die Benutzer von der Azure AD-Anmeldeseite zu Ihrer AD FS-Umgebung umgeleitet. Da der Mandant jetzt so konfiguriert ist, dass anstelle von „Verbund“ die Pass-Through-Authentifizierung verwendet wird, werden die Benutzer nicht mehr zu AD FS umgeleitet, sondern melden sich direkt über die Azure AD-Anmeldeseite an.

Öffnen Sie Internet Explorer im InPrivate-Modus, um zu vermeiden, dass Sie über das nahtlose einmalige Anmelden automatisch angemeldet werden, und navigieren Sie zur Office 365-Anmeldeseite ([https://portal.office.com](https://portal.office.com/)). Geben Sie den **UPN** des Benutzers ein, und klicken Sie auf **Weiter**. Stellen Sie sicher, dass Sie den UPN eines Hybridbenutzers eingeben, der von Ihrem lokalen Active Directory synchronisiert wurde und der zuvor zu einem Verbund gehörte. Dem Benutzer wird ein Bildschirm angezeigt, auf dem er seinen Benutzernamen und das Kennwort eingeben kann.

![Bild 18](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image27.png)

![Bild 19](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image28.png)

Nachdem Sie das Kennwort eingeben haben, sollten Sie zum Office 365-Portal umgeleitet werden.

![Bild 23](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image29.png)

### <a name="test-seamless-single-sign-on"></a>Testen des nahtlosen einmaligen Anmeldens

   1. Melden Sie bei einem mit einer Domäne verknüpften Computer an, der mit dem Unternehmensnetzwerk verbunden ist. 
   2. Öffnen Sie **Internet Explorer** oder **Chrome**, und navigieren Sie zu einer der folgenden URLs:   
      ‎  
      ‎[https://myapps.microsoft.com/contoso.com](https://myapps.microsoft.com/contoso.com) [https://myapps.microsoft.com/contoso.onmicrosoft.com](https://myapps.microsoft.com/contoso.onmicrosoft.com) (ersetzen Sie „Contoso“ durch Ihre Domäne).

      Der Benutzer wird schnell auf die Azure AD-Anmeldeseite weitergeleitet. Dort wird die Meldung „Anmeldung wird versucht“ angezeigt, und der Benutzer und sollte nicht zur Eingabe des Benutzernamens oder Kennworts aufgefordert werden.</br>
   ![Bild 24](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image30.png)</br>
   3. Anschließend wird der Benutzer umgeleitet und erfolgreich beim Zugriffsbereich angemeldet:

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

## <a name="roll-over-the-seamless-sso-kerberos-decryption-key"></a>Durchführen des Rollovers des Kerberos-Entschlüsselungsschlüssels für das einmalige Anmelden

Es ist wichtig, häufig ein Rollover des Kerberos-Entschlüsselungsschlüssels des AZUREADSSOACC-Computerkontos durchzuführen, das Azure AD repräsentiert und in Ihrer lokalen AD-Gesamtstruktur erstellt wurde. Es wird empfohlen, den Entschlüsselungsschlüssel von Kerberos mindestens alle 30 Tage zu verschieben, um die Anpassung daran vorzunehmen, wie die Mitglieder der Active Directory-Domäne Kennwortänderungen übermitteln. Da dem AZUREADSSOACC-Computerkonto kein zugeordnetes Gerät angefügt ist, muss der Rollover manuell durchgeführt werden.

Führen Sie diese Schritte auf dem lokalen Server aus, auf dem Sie Azure AD Connect ausführen, um den Rollover des Kerberos-Entschlüsselungsschlüssels einzuleiten.

[Wie kann ich einen Rollover des Kerberos-Entschlüsselungsschlüssels des AZUREADSSOACC-Computerkontos durchführen?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq)

## <a name="monitoring-and-logging"></a>Überwachung und Protokollierung

Die Server, auf denen die Authentifizierungs-Agents ausgeführt werden, sollten überwacht werden, um die Verfügbarkeit der Lösung zu gewährleisten. Die Authentifizierungs-Agents stellen nicht nur allgemeine Leistungsindikatoren für die Server bereit, sondern machen auch Leistungsobjekte verfügbar, die verwendet werden können, um Authentifizierungsstatistiken und -fehler nachzuvollziehen.

Authentifizierungs-Agents protokollieren sämtliche Vorgänge in den Windows-Ereignisprotokollen unter „Application and Service Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin“.

Bei Bedarf können Sie Protokolle für die Problembehandlung aktivieren.

Weitere Informationen zur Überwachung und Protokollierung finden Sie [hier](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-Pass-through-authentication).

## <a name="next-steps"></a>Nächste Schritte

- [Entwurfskonzepte für Azure AD Connect](plan-connect-design-concepts.md)
- [Auswählen der richtigen Authentifizierung](https://docs.microsoft.com/azure/security/azure-ad-choose-authn)
- [Unterstützte Topologien](plan-connect-design-concepts.md)
