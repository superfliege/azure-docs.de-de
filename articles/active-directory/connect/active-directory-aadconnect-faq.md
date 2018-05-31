---
title: Häufig gestellte Fragen zu Azure Active Directory Connect | Microsoft-Dokumentation
description: Auf dieser Seite finden Sie häufig gestellte Fragen zu Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: billmath
ms.openlocfilehash: 46a9bf47b4998c4d5be47f67556fbdb3ba7b71db
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34054093"
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Häufig gestellte Fragen zu Azure Active Directory Connect

## <a name="general-installation"></a>Allgemeine Installation
**F: Ist die Installation möglich, wenn der globale Azure AD-Administrator 2FA aktiviert hat?**  
Dies wird in den Builds ab Februar 2016 unterstützt.

**F: Gibt es eine Möglichkeit, Azure AD Connect unbeaufsichtigt zu installieren?**  
Azure AD Connect kann nur mit dem Installationsassistenten installiert werden. Eine unbeaufsichtigte und automatische Installation wird nicht unterstützt.

**F: In meiner Gesamtstruktur ist eine Domäne nicht erreichbar. Wie installiere ich Azure AD Connect?**  
Dies wird in den Builds ab Februar 2016 unterstützt.

**F: Funktioniert der AD DS Health-Agent im Serverkern?**  
Ja. Nach der Installation des Agents können Sie den Registrierungsprozess abschließen, indem Sie das folgende PowerShell-Cmdlet verwenden: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**F: Unterstützt AADConnect die Synchronisierung aus zwei Domänen mit Azure AD?**</br>
Ja, diese Möglichkeit wird unterstützt. Weitere Informationen finden Sie unter [Mehrere Domänen](active-directory-aadconnect-multiple-domains.md).
 
**F: Werden mehrere Connectors für dieselbe Active Directory-Domäne in Azure AD Connect unterstützt?**</br> Nein, dies wird nicht unterstützt. 

## <a name="network"></a>Netzwerk
**F: Die maximale Länge für offene Verbindungen in meinem Netzwerk wird durch eine Firewall, ein Netzwerkgerät o. ä. eingeschränkt. Wie hoch sollte der clientseitige Schwellenwert für die Zeitüberschreitung bei der Verwendung von Azure AD Connect sein?**  
Für Netzwerksoftware, physische Geräte und alle anderen Komponenten, durch die die maximale Länge von offenen Verbindungen eingeschränkt wird, sollte ein Schwellenwert von mindestens 5 Minuten (300 Sekunden) für die Konnektivität zwischen dem Server, auf dem der Azure AD Connect-Client installiert ist, und Azure Active Directory verwendet werden. Dies gilt auch für alle zuvor veröffentlichten Microsoft Identity-Synchronisierungswerkzeuge.

**F: Werden SLDs (einteilige Domänen) unterstützt?**  
Nein. Azure AD Connect unterstützt keine lokalen Gesamtstrukturen/Domänen mit SLDs.

**F: Werden Gesamtstrukturen mit nicht zusammenhängenden AD-Domänen unterstützt?**  
Nein. Azure AD Connect unterstützt keine lokalen Gesamtstrukturen, die nicht zusammenhängende Namespaces enthalten.

**F: Werden NetBios mit punktierten Namen unterstützt?**  
Nein. Azure AD Connect unterstützt keine lokalen Gesamtstrukturen/Domänen, deren NetBios-Name einen Punkt enthält.

**F: Wird eine reine IPv6-Umgebung unterstützt?**  
Nein, Azure AD Connect unterstützt keine reine IPv6-Umgebung.

## <a name="federation"></a>Verbund
**F: Was muss ich tun, wenn ich eine E-Mail erhalte, in der ich aufgefordert werde, mein Office 365-Zertifikat zu erneuern?**  
Befolgen Sie die Anweisungen, die Sie im Thema [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md) finden, um Ihr Zertifikat zu erneuern.

**F: Für die vertrauende Seite für Office 365 habe ich „Vertrauende Seite automatisch aktualisieren“ festgelegt. Muss ich bestimmte Maßnahmen ergreifen, wenn mein Tokensignaturzertifikat automatisch verlängert wird?**  
Befolgen Sie die Anweisungen im Artikel [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Environment
**F: Kann der Server nach der Installation von Azure AD umbenannt werden?**  
Nein. Wenn Sie den Namen des Servers ändern, kann das Synchronisierungsmodul keine Verbindung zur SQL-Datenbank herstellen, und der Dienst kann nicht gestartet werden.

## <a name="identity-data"></a>Identitätsdaten
**F: Warum entspricht das UPN (UserPrincipalName)-Attribut in Azure AD dem lokalen UPN?**  
Informationen hierzu finden Sie in diesen Artikeln:

* [Benutzernamen in Office 365, Azure oder Intune entsprechen nicht dem lokalen UPN oder der alternativen Anmelde-ID.](https://support.microsoft.com/en-us/kb/2523192)
* [Änderungen werden nicht mehr vom Azure Active Directory-Synchronisierungstool synchronisiert, wenn der UPN eines Benutzerkontos für die Verbindung mit einer anderen Verbunddomäne geändert wird.](https://support.microsoft.com/en-us/kb/2669550)

Sie können Azure AD auch so konfigurieren, dass das Synchronisierungsmodul den userPrincipalName wie unter [Features des Azure AD Connect-Synchronisierungsdiensts](active-directory-aadconnectsyncservice-features.md)beschrieben aktualisiert.

**F: Ist es möglich, Soft-Match-Zuordnungen von lokalen AD-Gruppen-/Kontaktobjekten mit vorhandenen Azure AD-Gruppen-/Kontaktobjekten vorzunehmen?**  
Ja, diese Zuordnungen basieren auf der Proxyadresse.  Soft Match wird nur für E-Mail-aktivierte Gruppen unterstützt.

**F: Ist es möglich, das ImmutableId-Attribut manuell für vorhandene Azure AD-Gruppen-/Kontaktobjekte festzulegen, um eine Hard-Match-Zuordnung zu lokalen AD-Gruppen-/Kontaktobjekten durchzuführen?**  
Nein, dies wird derzeit nicht unterstützt.

## <a name="custom-configuration"></a>Benutzerdefinierte Konfiguration
**F: Wo sind die PowerShell-Cmdlets für Azure AD Connect dokumentiert?**  
Mit Ausnahme der auf dieser Website dokumentierten Cmdlets werden keine PowerShell-Cmdlets in Azure AD Connect zur Verwendung für Kunden unterstützt.

**F: Kann ich mithilfe von „Serverexport/Serverimport“ in *Synchronization Service Manager* die Konfiguration zwischen Servern verschieben?**  
Nein. Da bei dieser Option nicht alle Einstellungen abgerufen werden, sollte sie nicht verwendet werden. Verwenden Sie stattdessen den Assistenten zum Erstellen der Basiskonfiguration auf dem zweiten Server und verwenden Sie den Synchronisierungsregel-Editor, um PowerShell-Skripts zum Verschieben benutzerdefinierter Regeln zwischen Servern zu erstellen. Siehe [Swing-Migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**F: Können Kennwörter für die Azure-Anmeldeseite zwischengespeichert werden, und kann dies verhindert werden, da sie ein Kennworteingabeelement mit dem Attribut „autocomplete = "false"“ enthält?**</br>
Wir unterstützen derzeit keine Änderung der HTML-Attribute des Kennworteingabefelds, einschließlich des AutoVervollständigen-Tags. Wir arbeiten derzeit an einem Feature, das benutzerdefiniertes Javascript zulässt, sodass Sie dem Kennwortfeld beliebige Attribute hinzufügen können. Dies sollte Ende 2017 verfügbar sein.

**F: Auf der Azure-Anmeldeseite werden Benutzernamen für Benutzer angezeigt, die sich zuvor erfolgreich angemeldet haben.  Kann dieses Verhalten deaktiviert werden?**</br>
Wir unterstützen derzeit keine Änderung der HTML-Attribute der Anmeldeseite. Wir arbeiten derzeit an einem Feature, das benutzerdefiniertes Javascript zulässt, sodass Sie dem Kennwortfeld beliebige Attribute hinzufügen können. Dies sollte Ende 2017 verfügbar sein.

**F: Gibt es eine Möglichkeit, gleichzeitige Sitzungen zu verhindern?**</br>
Nein.

## <a name="auto-upgrade"></a>Automatisches Upgrade

**F: Worin bestehen die Vorteile und Folgen der Verwendung des automatischen Upgrades?**</br>
Allen Kunden wird empfohlen, das automatische Upgrade für ihre Azure AD Connect-Installation zu aktivieren. Die Vorteile bestehen darin, dass sie immer die neuesten Patches erhalten, einschließlich Sicherheitsupdates für Sicherheitsrisiken, die in Azure AD Connect gefunden wurden. Der Upgradeprozess verläuft reibungslos und erfolgt automatisch, sobald eine neue Version verfügbar ist. Mit jeder neuen Version unterstützen wir durch das automatische Upgrade viele Tausende Azure AD Connect-Kunden.

Beim Prozess des automatischen Upgrades wird zunächst immer überprüft, ob eine Installation berechtigt für das automatische Upgrade ist (dazu gehört die Suche nach benutzerdefinierten Änderungen an Regeln, nach spezifischen Umgebungsfaktoren usw.). In diesem Fall wird das Upgrade ausgeführt und getestet. Wenn die Tests ergeben, dass ein Upgrade nicht erfolgreich ausgeführt wurde, wird automatisch die vorherige Version wiederhergestellt.

Je nach der Größe der Umgebung kann der Prozess mehrere Stunden dauern. Während das Upgrade ausgeführt wird, erfolgt keine Synchronisierung zwischen Windows Server AD und Azure AD.

**F: In einer E-Mail wurde mir mitgeteilt, dass das automatische Upgrade nicht mehr funktioniert und dass ich eine neue Version installieren muss. Warum ist dies erforderlich?**</br>
Letztes Jahr haben wir eine Version von Azure AD Connect veröffentlicht, durch die unter bestimmten Umständen das Feature für automatisches Upgrade auf Ihrem Server deaktiviert wurde. Dieses Problem wurde in Azure AD Connect Version 1.1.750.0 behoben, die Ende des letzten Monats veröffentlicht wurde. Kunden, die möglicherweise von diesem Problem betroffen sind, müssen das Upgrade auf die neueste Version von Azure AD Connect manuell durchführen, um das Problem zu beheben. Für das manuelle Upgrade müssen Sie die neueste Version der Datei „AADConnect.msi“ herunterladen und ausführen.
 
-  Wenn Ihre aktuelle Version älter als 1.1.750.0 ist, müssen Sie das Upgrade auf die neueste Version durchführen, die Sie [hier herunterladen können](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
- Wenn Sie Azure AD Connect 1.1.750.0 oder eine neuere Version installiert haben, müssen Sie keine Aktion ausführen, um das Problem zu beheben, da Sie bereits die Version mit der entsprechenden Fehlerkorrektur verwenden. 

**F: In einer E-Mail wurde mir mitgeteilt, dass ein Upgrade auf die neueste Version erforderlich ist, um das automatische Upgrade erneut zu aktivieren. Ich verwende Version 1.1.654.0. Muss ich das Upgrade durchführen?** </br>    
Ja, Sie müssen ein Upgrade auf Version 1.1.750 oder eine neuere Version durchführen, um das automatische Upgrade erneut zu aktivieren. Unter folgendem Link erhalten Sie Informationen zum Upgrade auf eine neuere Version.

**F: In einer E-Mail wurde mir mitgeteilt, dass ein Upgrade auf die neueste Version erforderlich ist, um das automatische Upgrade erneut zu aktivieren. Ich habe das automatische Upgrade mithilfe von PowerShell aktiviert. Muss ich dennoch die neueste Version installieren?**</br>    
Ja, Sie müssen ein Upgrade auf Version 1.1.750.0 oder eine neuere Version durchführen. Durch die Aktivierung des Diensts für das automatische Upgrade mithilfe von PowerShell wird das in Versionen vor 1.1.750 gefundene Problem des automatischen Upgrades nicht behoben.

**F: Ich möchte ein Upgrade auf eine neuere Version durchführen, weiß aber nicht, wer Azure AD Connect installiert hat, und verfüge nicht über den Benutzernamen und das Kennwort.  Werden diese Angaben benötigt?**</br>
Für ein Upgrade von Azure AD Connect müssen Sie den Benutzernamen und das Kennwort nicht kennen, die ursprünglich verwendet wurden. Dazu kann jedes Azure AD-Konto mit der Rolle „Globaler Administrator“ verwendet werden.

**F: Wie kann ich feststellen, welche Version von Azure AD Connect ich verwende?**</br>   
Um zu überprüfen, welche Version von Azure AD Connect auf Ihrem Server installiert ist, wechseln Sie zur Systemsteuerung. Unter „Programme > Programme und Funktionen“ können Sie die installierte Version von Microsoft Azure AD Connect einsehen:

![Version](media/active-directory-aadconnect-faq/faq1.png)

**F: Wie führe ich ein Upgrade auf die neueste Version von AADConnect durch?**</br>    
In diesem [Artikel](active-directory-aadconnect-upgrade-previous-version.md) wird erläutert, wie ein Upgrade auf eine neuere Version durchgeführt wird. 

**F: Wir haben bereits letztes Jahr ein Upgrade auf die neueste Version von AADConnect durchgeführt. Müssen wir erneut ein Upgrade durchführen?**</br> Das Azure AD Connect-Team nimmt häufig Aktualisierungen an diesem Dienst vor. Außerdem ist es wichtig, dass auf Ihrem Server immer die neueste Version installiert ist, sodass Sie von Fehlerkorrekturen und Sicherheitsupdates, aber auch von neuen Features profitieren können. Wenn Sie das automatische Upgrade aktivieren, wird die Softwareversion automatisch aktualisiert. Informationen zum Verlauf der Versionsveröffentlichungen von Azure AD Connect finden Sie unter diesem [Link](active-directory-aadconnect-version-history.md).

**F: Wie lange dauert das Durchführen des Upgrades, und was sind die Auswirkungen für meine Benutzer?**</br>    
Die erforderliche Zeit für das Upgrade hängt von der Größe Ihres Mandanten ab. Für größere Organisationen empfiehlt sich die Durchführung am Abend oder am Wochenende. Beachten Sie, dass während des Upgrades keine Synchronisierungsvorgänge erfolgen.

**F: Ich bin der Meinung, dass ich das Upgrade auf AADConnect durchgeführt habe, im Office-Portal wird aber weiterhin „DirSync“ angezeigt.  Wie kommt das?**</br>    
Das Office-Team arbeitet daran, dass bei den Aktualisierungen des Office-Portals der aktuelle Produktname angezeigt wird. Welches Synchronisierungstool Sie verwenden, wird nicht angezeigt.

**F: Ich habe den Status des automatischen Upgrades überprüft, er lautet „Angehalten“. Warum wurde der Dienst angehalten? Muss ich ihn aktivieren?**</br>     
Eine frühere Version enthielt einen Fehler, der unter bestimmten Umständen dazu führte, dass der Status des automatischen Upgrades auf „Angehalten“ gesetzt wurde. Die manuelle Aktivierung ist technisch möglich, würde aber mehrere komplexe Schritte erfordern. Am besten installieren Sie die neueste Version von Azure AD Connect.

**F: In meinem Unternehmen gelten strenge Anforderungen im Hinblick auf das Change Management, und ich möchte steuern, wann die Übermittlung stattfindet. Kann ich steuern, wann das automatische Upgrade gestartet wird?**</br> Nein, derzeit steht kein solches Feature zur Verfügung. Wir werden dies für eine künftige Version auswerten.

**F: Erhalte ich eine E-Mail, wenn beim automatischen Upgrade Fehler auftreten? Woher weiß ich, dass es erfolgreich ausgeführt wurde?**</br>     
Sie erhalten keine Benachrichtigung in Bezug auf das Ergebnis des Upgrades. Wir werden dies für eine künftige Version auswerten.

**F: Veröffentlichen Sie eine Zeitachse im Hinblick auf die geplante Übermittlung von automatischen Upgrades?**</br>    
Das automatische Upgrade ist der erste Schritt unseres Releaseprozesses einer neueren Version, d.h., sobald eine neue Version zur Verfügung steht, werden automatische Upgrades übertragen. Neuere Versionen von Azure AD Connect werden in der [Azure AD-Roadmap](../../active-directory/whats-new.md) vorab angekündigt.

**F: Wird mit dem automatischen Upgrade AAD Connect Health aktualisiert?**</br>   Ja, mit dem automatischen Upgrade wird auch AAD Connect Health aktualisiert.

**F: Wird das automatische Upgrade auch für AAD Connect-Server im Stagingmodus durchgeführt?**</br>   
Nein, für einen Azure AD Connect-Server im Stagingmodus kann kein automatisches Upgrade durchgeführt werden.

**F: Wie soll ich vorgehen, wenn beim automatischen Upgrade Fehler auftreten und der AAD Connect-Server nicht gestartet wird?**</br>   
In seltenen Fällen wird der Azure AD Connect-Dienst nach der Durchführung des Upgrades nicht gestartet. Starten Sie in diesen Fällen den Server neu. Das Problem wird dadurch normalerweise behoben. Wenn der Azure AD Connect-Dienst weiterhin nicht gestartet wird, öffnen Sie ein Supportticket. Die entsprechende Beschreibung finden Sie unter diesem [Link](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/). 

**F: Ich bin mir über die Auswirkungen des Upgrades auf eine neuere Version von Azure AD Connect nicht sicher. Können Sie mich anrufen, um mir bei der Durchführung des Upgrades zu helfen?**</br>
Wenn Sie Hilfe bei der Durchführung eines Upgrades auf eine neuere Version von Azure AD Connect benötigen, öffnen Sie bitte ein Supportticket. Entsprechende Informationen finden Sie unter diesem [Link](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/).

## <a name="troubleshooting"></a>Problembehandlung
**F: Wie erhalte ich Hilfe zu Azure AD Connect?**

[Durchsuchen der Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Suchen Sie in der Microsoft Knowledge Base (KB) nach technischen Lösungen für häufige Probleme im Zusammenhang mit der Unterstützung von Azure AD Connect.

[Microsoft Azure Active Directory-Foren](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Sie können in der Community nach technischen Fragen und Antworten suchen oder eine eigene Frage stellen, indem Sie [hier](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)klicken.

[Erhalten von Support für Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

* Verwenden Sie diesen Link, um Support über das Azure-Portal zu erhalten.

