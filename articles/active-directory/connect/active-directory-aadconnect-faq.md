---
title: Häufig gestellte Fragen zu Azure Active Directory Connect | Microsoft-Dokumentation
description: In diesem Artikel finden Sie häufig gestellte Fragen zu Azure AD Connect.
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
ms.date: 06/05/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 545766aa6c39fe678113297e584422872a6ded87
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008143"
---
# <a name="azure-active-directory-connect-faq"></a>Häufig gestellte Fragen zu Azure Active Directory Connect

## <a name="general-installation"></a>Allgemeine Installation
**F: Funktioniert die Installation, wenn für den globalen Administrator für Azure Active Directory (Azure AD) die zweistufige Authentifizierung aktiviert ist?**  
Seit Builds vom Februar 2016 wird dieses Szenario unterstützt.

**F: Gibt es eine Möglichkeit, Azure AD Connect unbeaufsichtigt zu installieren?**  
Die Azure AD Connect-Installation wird nur unterstützt, wenn Sie den Installations-Assistenten verwenden. Eine unbeaufsichtigte, automatische Installation wird nicht unterstützt.

**F: In meiner Gesamtstruktur ist eine Domäne nicht erreichbar. Wie installiere ich Azure AD Connect?**  
Seit Builds vom Februar 2016 wird dieses Szenario unterstützt.

**F: Funktioniert der Azure AD DS Health-Agent (Azure Active Directory Domain Services) unter Server Core?**  
Ja. Nach der Installation des Agents können Sie den Registrierungsprozess abschließen, indem Sie das folgende PowerShell-Cmdlet verwenden: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**F: Unterstützt Azure AD Connect die Synchronisierung aus zwei Domänen mit Azure AD?**  
Ja, dieses Szenario wird unterstützt. Weitere Informationen finden Sie unter [Mehrere Domänen](active-directory-aadconnect-multiple-domains.md).
 
**F: Können mehrere Connectors für dieselbe Active Directory-Domäne in Azure AD Connect verwendet werden?**  
Nein, mehrere Connectors für die gleichen AD-Domäne werden nicht unterstützt. 

**F: Kann ich die Azure AD Connect-Datenbank von der lokalen Datenbank auf eine SQL Server-Remoteinstanz verschieben?**   
Ja, in den folgenden Schritten finden Sie einen allgemeinen Leitfaden dazu. Wir arbeiten derzeit an einem ausführlicheren Dokument.
1. Sichern Sie die LocalDB-Datenbank ADSync.
Die einfachste Möglichkeit hierzu ist die Verwendung von SQL Server Management Studio, das auf demselben Computer wie Azure AD Connect installiert ist. Stellen Sie eine Verbindung mit *(localdb)\.\ADSync* her, und sichern Sie dann die Datenbank ADSync.

2. Stellen Sie die Datenbank ADSync in der SQL Server-Remoteinstanz wieder her.

3. Installieren Sie Azure AD Connect für die vorhandene [SQL-Remotedatenbank](active-directory-aadconnect-existing-database.md).
   Der Artikel veranschaulicht die Migration zu einer lokalen SQL-Datenbank. Wenn Sie zu einer Remote-SQL-Datenbank migrieren, müssen Sie in Schritt 5 dieses Vorgangs auch ein vorhandenes Dienstkonto eingeben, unter dem der Windows-Synchronisierungsdienst ausgeführt wird. Dieses Dienstkonto der Synchronisierungs-Engine wird hier beschrieben:
   
      **Verwenden eines bestehenden Dienstkontos:** Für Azure AD Connect wird standardmäßig ein virtuelles Dienstkonto für die Synchronisierungsdienste genutzt. Wenn Sie eine SQL Server-Remoteinstanz oder einen Proxy mit Authentifizierungsanforderung verwenden, verwenden Sie ein verwaltetes Dienstkonto oder ein Dienstkonto in der Domäne. Das Kennwort muss Ihnen bekannt sein. Geben Sie in diesen Fällen das zu verwendende Konto ein. Stellen Sie sicher, dass die Benutzer, die die Installation ausführen, Systemadministratoren in SQL sind, damit die Anmeldeinformationen für das Dienstkonto erstellt werden können. Weitere Informationen finden Sie unter [Azure AD Connect: Konten und Berechtigungen](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). 
   
      Mit dem neuesten Build kann der SQL-Administrator nun eine Out-of-Band-Datenbankbereitstellung ausführen, sodass die Datenbank anschließend vom Azure AD Connect-Administrator mit Datenbankbesitzerrechten installiert werden kann. Weitere Informationen finden Sie unter [Installieren von Azure AD Connect mit Berechtigungen eines delegierten SQL-Administrators](active-directory-aadconnect-sql-delegation.md).

Aus Gründen der Einfachheit wird empfohlen, dass Benutzer, die Azure AD Connect installieren, Systemadministratoren in SQL sind. Mit aktuellen Builds können Sie jetzt jedoch delegierte SQL-Administratoren verwenden, wie unter [Installieren von Azure AD Connect mit delegierten SQL-Administratorberechtigungen](active-directory-aadconnect-sql-delegation.md) beschrieben.

## <a name="network"></a>Netzwerk
**F: Die Zeit für offene Verbindungen in meinem Netzwerk wird durch eine Firewall, ein Netzwerkgerät o.ä. eingeschränkt. Was sollte der clientseitige Schwellenwert für die Zeitüberschreitung bei Verwendung von Azure AD Connect sein?**  
Für Netzwerksoftware, physische Geräte und andere Komponenten, durch die die maximale Dauer von offenen Verbindungen eingeschränkt wird, sollte ein Schwellenwert von mindestens 5 Minuten (300 Sekunden) für die Konnektivität zwischen dem Server, auf dem der Azure AD Connect-Client installiert ist, und Azure Active Directory verwendet werden. Diese Empfehlung gilt auch für alle zuvor veröffentlichten Microsoft-Identitätssynchronisierungswerkzeuge.

**F: Werden einteilige Domänen (SLDs) unterstützt?**  
Von dieser Netzwerkkonfiguration wird zwar dringend abgeraten ([siehe Artikel](https://support.microsoft.com/en-us/help/2269810/microsoft-support-for-single-label-domains)), die Verwendung der Azure AD Connect-Synchronisierung mit einer einteiligen Domäne wird jedoch unterstützt, solange die Netzwerkkonfiguration für diese ordnungsgemäß funktioniert.

**F: Werden Gesamtstrukturen mit nicht zusammenhängenden AD-Domänen unterstützt?**  
Nein. Azure AD Connect unterstützt keine lokalen Gesamtstrukturen mit nicht zusammenhängenden Namespaces.

**F: Werden „punktierte“ NetBIOS-Namen unterstützt?**  
Nein. Azure AD Connect unterstützt keine lokalen Gesamtstrukturen oder Domänen, deren NetBIOS-Name einen Punkt (.) enthält.

**F: Wird eine reine IPv6-Umgebung unterstützt?**  
Nein, Azure AD Connect unterstützt keine reine IPv6-Umgebung.

## <a name="federation"></a>Verbund
**F: Was muss ich tun, wenn ich eine E-Mail erhalte, in der ich aufgefordert werde, mein Office 365-Zertifikat zu erneuern?**  
Anleitungen zur Erneuerung des Zertifikats finden Sie unter [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md).

**F: Für die vertrauende Seite für Office 365 habe ich „Update für vertrauende Seite automatisch ausführen“ festgelegt. Muss ich bestimmte Maßnahmen ergreifen, wenn mein Tokensignaturzertifikat automatisch verlängert wird?**  
Befolgen Sie die Anweisungen im Artikel [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Environment
**F: Kann der Server nach der Installation von Azure AD umbenannt werden?**  
Nein. Nach dem Ändern des Servernamens kann das Synchronisierungsmodul keine Verbindung mit der SQL-Datenbank-Instanz mehr herstellen, und der Dienst kann nicht gestartet werden.

## <a name="identity-data"></a>Identitätsdaten
**F: Warum entspricht das userPrincipalName-Attribut (UPN) in Azure AD nicht dem lokalen UPN?**  
Informationen finden Sie in diesen Artikeln:

* [Benutzernamen in Office 365, Azure oder Intune entsprechen nicht dem lokalen UPN oder der alternativen Anmelde-ID](https://support.microsoft.com/en-us/kb/2523192)
* [Änderungen werden nicht mehr vom Azure Active Directory-Synchronisierungstool synchronisiert, wenn der UPN eines Benutzerkontos für die Verbindung mit einer anderen Verbunddomäne geändert wird](https://support.microsoft.com/en-us/kb/2669550)

Sie können Azure AD auch so konfigurieren, dass das Synchronisierungsmodul den UPN wie unter [Features des Azure AD Connect-Synchronisierungsdiensts](active-directory-aadconnectsyncservice-features.md)beschrieben aktualisiert.

**F: Ist es möglich, Soft-Match-Zuordnungen von lokalen Azure AD-Gruppen- oder -Kontaktobjekten mit vorhandenen Azure AD-Gruppen- oder -Kontaktobjekten vorzunehmen?**  
Ja, diese Soft-Match-Zuordnungen basieren auf der Proxyadresse. Soft Match wird nur für E-Mail-aktivierte Gruppen unterstützt.

**F: Ist es möglich, das ImmutableId-Attribut manuell für vorhandene Azure AD-Gruppen- oder -Kontaktobjekte festzulegen, um eine Hard-Match-Zuordnung zu lokalen Azure AD-Gruppen- oder -Kontaktobjekten durchzuführen?**  
Nein, die manuelle Festlegung des ImmutableId-Attributs für vorhandene Azure AD-Gruppen- oder -Kontaktobjekte, um eine Hard-Match-Zuordnung durchzuführen, wird derzeit nicht unterstützt.

## <a name="custom-configuration"></a>Benutzerdefinierte Konfiguration
**F: Wo sind die PowerShell-Cmdlets für Azure AD Connect dokumentiert?**  
Mit Ausnahme der auf dieser Website dokumentierten Cmdlets werden keine PowerShell-Cmdlets in Azure AD Connect zur Verwendung für Kunden unterstützt.

**F: Kann ich mithilfe der Option „Serverexport/Serverimport“ in Synchronization Service Manager die Konfiguration zwischen Servern verschieben?**  
Nein. Da bei dieser Option nicht alle Einstellungen abgerufen werden, sollte sie nicht verwendet werden. Verwenden Sie stattdessen den Assistenten zum Erstellen der Basiskonfiguration auf dem zweiten Server, und verwenden Sie den Synchronisierungsregel-Editor, um PowerShell-Skripts zum Verschieben benutzerdefinierter Regeln zwischen Servern zu erstellen. Weitere Informationen finden Sie unter [Swing-Migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**F: Können Kennwörter für die Azure-Anmeldeseite zwischengespeichert werden, und kann diese Zwischenspeicherung verhindert werden, da sie ein Kennworteingabeelement mit dem Attribut *autocomplete = "false"* enthält?**  
Derzeit wird keine Änderung der HTML-Attribute des Felds **Kennwort**, einschließlich des AutoVervollständigen-Tags, unterstützt. Wir arbeiten derzeit an einem Feature, das benutzerdefiniertes JavaScript zulässt, sodass Sie dem Feld **Kennwort** beliebige Attribute hinzufügen können.

**F: Auf der Azure-Anmeldeseite werden Benutzernamen von Benutzern angezeigt, die sich zuvor erfolgreich angemeldet haben. Kann dieses Verhalten deaktiviert werden?**  
Derzeit wird keine Änderung der HTML-Attribute des Eingabefelds **Kennwort**, einschließlich des AutoVervollständigen-Tags, unterstützt. Wir arbeiten derzeit an einem Feature, das benutzerdefiniertes JavaScript zulässt, sodass Sie dem Feld **Kennwort** beliebige Attribute hinzufügen können.

**F: Gibt es eine Möglichkeit, gleichzeitige Sitzungen zu verhindern?**  
Nein.

## <a name="auto-upgrade"></a>Automatisches Upgrade

**F: Worin bestehen die Vorteile und Folgen der Verwendung des automatischen Upgrades?**  
Allen Kunden wird empfohlen, das automatische Upgrade für ihre Azure AD Connect-Installation zu aktivieren. Der Vorteil besteht darin, dass sie so immer die neuesten Patches erhalten, einschließlich Sicherheitsupdates für Sicherheitsrisiken, die in Azure AD Connect gefunden wurden. Der Upgradeprozess verläuft reibungslos und erfolgt automatisch, sobald eine neue Version verfügbar ist. Viele Tausende Azure AD Connect-Kunden verwenden das automatische Upgrade mit jeder neuen Version.

Beim automatischen Upgrade wird immer zuerst überprüft, ob eine Installation für das automatische Upgrade geeignet ist. Wenn sie geeignet ist, wird das Upgrade ausgeführt und getestet. Dieser Vorgang umfasst auch die Suche nach benutzerdefinierten Änderungen an Regeln und bestimmten Umgebungsfaktoren. Wenn die Tests ergeben, dass ein Upgrade nicht erfolgreich ausgeführt wurde, wird automatisch die vorherige Version wiederhergestellt.

Abhängig von der Größe der Umgebung kann dieser Vorgang mehrere Stunden dauern. Während des Upgrades erfolgt keine Synchronisierung zwischen Windows Server Active Directory und Azure AD.

**F: In einer E-Mail wurde mir mitgeteilt, dass das automatische Upgrade nicht mehr funktioniert und ich eine neue Version installieren muss. Warum ist dies erforderlich?**  
Im letzten Jahr haben wir eine Version von Azure AD Connect veröffentlicht, durch die unter bestimmten Umständen das Feature für automatisches Upgrade auf Ihrem Server deaktiviert wurde. Wir haben das Problem in Azure AD Connect Version 1.1.750.0 behoben. Wenn Sie von dem Problem betroffen sind, können Sie sie es mithilfe eines PowerShell-Skripts zum Reparieren oder durch ein manuelles Upgrade auf die neueste Version von Azure AD Connect beheben. 

Um das PowerShell-Skript ausführen, [laden Sie das Skript herunter](https://aka.ms/repairaadconnect), und führen Sie es auf Ihrem Azure AD Connect-Server in einem administrativen PowerShell-Fenster aus. Informationen zum Ausführen des Skripts erhalten Sie in [diesem kurzen Video](https://aka.ms/repairaadcau).

Für das manuelle Upgrade müssen Sie die neueste Version der Datei „AADConnect.msi“ herunterladen und ausführen.
 
-  Wenn Ihre aktuelle Version älter als 1.1.750.0 ist, [laden Sie es herunter, und führen Sie ein Upgrade auf die neueste Version durch](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
- Wenn Ihre Azure AD Connect-Version 1.1.750.0 oder höher ist, sind keine weiteren Aktionen erforderlich. Sie verwenden bereits die Version, die die Korrektur des automatischen Upgrades enthält. 

**F: In einer E-Mail wurde mir mitgeteilt, dass ein Upgrade auf die neueste Version erforderlich ist, um das automatische Upgrade erneut zu aktivieren. Ich verwende Version 1.1.654.0. Muss ich das Upgrade durchführen?**  
Ja, Sie müssen ein Upgrade auf Version 1.1.750.0 oder eine neuere Version durchführen, um das automatische Upgrade erneut zu aktivieren. [Laden Sie das Upgrade auf die neueste Version herunter, und führen Sie es aus](https://www.microsoft.com/en-us/download/details.aspx?id=47594).

**F: In einer E-Mail wurde mir mitgeteilt, dass ein Upgrade auf die neueste Version erforderlich ist, um das automatische Upgrade erneut zu aktivieren. Muss ich die neueste Version auch dann installieren, wenn ich das automatische Upgrade mithilfe von PowerShell aktiviert habe?**  
Ja, Sie müssen ein Upgrade auf Version 1.1.750.0 oder eine neuere Version durchführen. Durch die Aktivierung des Diensts für das automatische Upgrade mithilfe von PowerShell wird das in Versionen vor 1.1.750.0 gefundene Problem des automatischen Upgrades nicht behoben.

**F: Ich möchte ein Upgrade auf eine neuere Version durchführen, weiß aber nicht, wer Azure AD Connect installiert hat, und verfüge nicht über den Benutzernamen und das Kennwort. Werden diese Angaben benötigt?**
Sie müssen den Benutzernamen und das Kennwort, die ursprünglich für das Upgrade von Azure AD Connect verwendet wurden, nicht kennen. Verwenden Sie ein beliebiges Azure AD-Konto, das über die globale Administratorrolle verfügt.

**F: Wie kann ich feststellen, welche Version von Azure AD Connect ich verwende?**  
Um zu überprüfen, welche Version von Azure AD Connect auf Ihrem Server installiert ist, wechseln Sie zur Systemsteuerung. Durch Auswählen von **Programme** > **Programme und Features** können Sie die installierte Version von Microsoft Azure AD Connect einsehen wie hier gezeigt:

![Azure AD Connect-Version in der Systemsteuerung](media/active-directory-aadconnect-faq/faq1.png)

**F: Wie führe ich ein Upgrade auf die aktuelle Version von Azure AD Connect aus?**  
Informationen zum Durchführen eines Upgrades auf die neueste Version finden Sie unter [Azure AD Connect: Aktualisieren von einer früheren Version auf die aktuelle Version](active-directory-aadconnect-upgrade-previous-version.md). 

**F: Wir haben bereits im letzten Jahr ein Upgrade auf die neueste Version von Azure AD Connect durchgeführt. Müssen wir erneut ein Upgrade durchführen?**  
Das Azure AD Connect-Team nimmt häufig Aktualisierungen am Dienst vor. Um von Fehlerbehebungen und Sicherheitsupdates sowie neuen Features zu profitieren, ist es wichtig, den Server auf dem neuesten Stand zu halten. Wenn Sie das automatische Upgrade aktivieren, wird die Softwareversion automatisch aktualisiert. Informationen zum Verlauf der Versionsveröffentlichungen von Azure AD Connect finden Sie unter [Azure AD Connect: Versionsveröffentlichungsverlauf](active-directory-aadconnect-version-history.md).

**F: Wie lange dauert das Durchführen des Upgrades, und was sind die Auswirkungen für meine Benutzer?**  
Die für das Upgrade erforderliche Zeit hängt von der Größe Ihres Mandanten ab. Für größere Organisationen ist es möglicherweise am besten, das Upgrade abends oder am Wochenende durchzuführen. Während des Upgrades erfolgen keine Synchronisierungsvorgänge.

**F: Ich bin der Meinung, dass ich das Upgrade auf Azure AD Connect durchgeführt habe, im Office-Portal wird aber weiterhin „DirSync“ erwähnt. Warum?**  
Das Office-Team arbeitet daran, dass bei den Aktualisierungen des Office-Portals der aktuelle Produktname angezeigt wird. Welches Synchronisierungstool Sie verwenden, wird nicht angezeigt.

**F: Der Status des automatischen Upgrades lautet bei mir „Angehalten“. Warum wurde der Dienst angehalten? Muss ich ihn aktivieren?**  
Eine frühere Version enthielt einen Fehler, der unter bestimmten Umständen dazu führte, dass der Status des automatischen Upgrades auf „Angehalten“ festgelegt wurde. Das manuelle Aktivieren des automatischen Upgrades ist technisch möglich, umfasst jedoch mehrere komplexe Schritte. Installieren Sie am besten die aktuelle Version von Azure AD Connect.

**F: In meinem Unternehmen gelten strenge Anforderungen im Hinblick auf das Change Management, und ich möchte die Kontrolle darüber haben, wann die Übermittlung stattfindet. Kann ich steuern, wann das automatische Upgrade gestartet wird?**  
Nein, es gibt im Moment kein solches Feature. Das Feature wird derzeit für eine künftige Version ausgewertet.

**F: Erhalte ich eine E-Mail, wenn beim automatischen Upgrade Fehler auftreten? Woher weiß ich, dass es erfolgreich ausgeführt wurde?**  
Sie werden über das Ergebnis des Upgrades nicht benachrichtigt. Das Feature wird derzeit für eine künftige Version ausgewertet.

**F: Veröffentlichen Sie eine Zeitachse im Hinblick auf die geplante Übermittlung von automatischen Upgrades?**  
Das automatische Upgrade ist der erste Schritt im Prozess der Freigabe einer neueren Version. Immer wenn ein neues Release vorliegt, werden automatisch Upgrades gepusht. Neuere Versionen von Azure AD Connect werden in der [Azure AD-Roadmap](../fundamentals/whats-new.md) vorab angekündigt.

**F: Wird mit dem automatischen Upgrade auch Azure AD Connect Health aktualisiert?**  
Ja, mit dem automatischen Upgrade wird auch Azure AD Connect Health aktualisiert.

**F: Wird das automatische Upgrade auch für Azure AD Connect-Server im Stagingmodus durchgeführt?**  
Ja, ein automatisches Upgrade kann für einen Azure AD Connect-Server im Stagingmodus durchgeführt werden.

**F: Wie soll ich vorgehen, wenn beim automatischen Upgrade Fehler auftreten und der Azure AD Connect-Server nicht gestartet wird?**  
In seltenen Fällen wird der Azure AD Connect-Dienst nach dem Durchführen des Upgrades nicht gestartet. Starten Sie in diesen Fällen den Server neu. Das Problem wird dadurch normalerweise behoben. Wenn der Azure AD Connect-Dienst weiterhin nicht gestartet wird, öffnen Sie ein Supportticket. Weitere Informationen finden Sie unter [Erstellen einer Serviceanfrage zum Kontaktieren des Office 365-Supports](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/). 

**F: Ich bin mir über die Auswirkungen des Upgrades auf eine neuere Version von Azure AD Connect nicht sicher. Können Sie mich anrufen, um mir bei der Durchführung des Upgrades zu helfen?**  
Wenn Sie Hilfe bei der Durchführung eines Upgrades auf eine neuere Version von Azure AD Connect benötigen, öffnen Sie ein Supportticket, wie unter [Erstellen einer Serviceanfrage zum Kontaktieren des Office 365-Supports](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) beschrieben.

## <a name="troubleshooting"></a>Problembehandlung
**F: Wie erhalte ich Hilfe zu Azure AD Connect?**

[Durchsuchen der Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Suchen Sie in der Knowledge Base nach technischen Lösungen für häufige Probleme im Zusammenhang mit der Unterstützung von Azure AD Connect.

[Azure Active Directory-Foren](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Suchen Sie nach technischen Fragen und Antworten, oder stellen Sie selbst Fragen, indem Sie die [Azure AD-Community](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required) besuchen.

[Erhalten von Support für Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)
