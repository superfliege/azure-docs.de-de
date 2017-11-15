---
title: Konfigurieren von Azure MFA | Microsoft-Dokumentation
description: "Dies ist die Seite \"Azure Multi-Factor Authentication\", auf der beschrieben wird, was die nächsten Schritte mit Multi-Factor Authentication sind.  Dazu gehören Berichte, Betrugswarnung, Einmalumgehung, benutzerdefinierte Sprachnachrichten, Zwischenspeicherung, vertrauenswürdige IP-Adressen und App-Kennwörter."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2017
ms.author: joflore
ms.reviewer: alexwe
ms.openlocfilehash: 723bd7135a59bcc0bce648460f871a841a684d3c
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2017
---
# <a name="configure-azure-multi-factor-authentication-settings---public-preview"></a>Konfigurieren von Azure Multi-Factor Authentication-Einstellungen – öffentliche Vorschau

Dieser Artikel bietet Unterstützung bei der Verwaltung der Azure Multi-Factor Authentication, nachdem Sie nun bereit für die Ausführung sind.  Der Artikel umfasst eine Vielzahl von Themen, die Ihnen dabei helfen, Azure Multi-Factor Authentication optimal zu nutzen.  Nicht alle Funktionen sind in jeder [Version von Azure Multi-Factor Authentication](/multi-factor-authentication-get-started.md#what-features-do-i-need) verfügbar.

>[!NOTE]
>Die folgenden Einstellungen sind in der öffentlichen Vorschauversion im Azure-Portal verfügbar. Informationen zum Verwalten der Azure Multi-Factor Authentication-Einstellungen im pfweb-Portal finden Sie im Dokumentationsartikel [Configure Azure Multi-Factor Authentication settings](multi-factor-authentication-whats-next-pfweb.md) (Konfigurieren von Azure Multi-Factor Authentication-Einstellungen).

| Feature | Beschreibung | 
|:--- |:--- |
| [Benutzer sperren/zulassen](#block/unblock-users) |Mit „Benutzer sperren/zulassen“ kann verhindert werden, dass Benutzer Authentifizierungsanforderungen erhalten. |
| [Betrugswarnung](#fraud-alert) |Die Betrugswarnung kann so konfiguriert und eingerichtet werden, dass Ihre Benutzer betrügerische Versuche, auf ihre Ressourcen zuzugreifen, melden können. |
| [Einmalumgehung](#one-time-bypass) |Mit einer Einmalumgehung kann sich ein Benutzer ein einziges Mal authentifizieren, indem er die mehrstufige Authentifizierung "umgeht". |
| [Benutzerdefinierte Sprachnachrichten](#custom-voice-messages) |Mit benutzerdefinierten Sprachnachrichten können Sie Ihre eigenen Aufzeichnungen oder Begrüßungen mit mehrstufiger Authentifizierung verwenden. |
| [Zwischenspeichern](#caching-in-azure-multi-factor-authentication) |Durch Zwischenspeichern können Sie einen bestimmten Zeitraum festlegen, sodass nachfolgende Authentifizierungsversuche automatisch erfolgreich sind. |
| [Vertrauenswürdige IPs](#trusted-ips) |Mit vertrauenswürdigen IP-Adressen können Administratoren eines verwalteten Mandanten oder Verbundmandanten die Überprüfung in zwei Schritten für Benutzer umgehen, die sich über das lokale Intranet des Unternehmens anmelden. |
| [App-Kennwörter](#app-passwords) |Durch ein App-Kennwort kann eine Anwendung, die MFA nicht erkennt, Multi-Factor Authentication umgehen, und weiter ausgeführt werden. |
| [Speichern der Multi-Factor Authentication für gespeicherte Geräte und Browser](#remember-multi-factor-authentication-for-devices-that-users-trust) |Mit dieser Funktion können Sie Geräte für eine festgelegte Anzahl von Tagen speichern, nachdem ein Benutzer erfolgreich mit MFA angemeldet wurde. |
| [Auswählbare Verifizierungsmethoden](#selectable-verification-methods) |Ermöglicht Ihnen die Auswahl der Authentifizierungsmethoden, die Sie den Benutzern zur Verfügung stellen möchten. |

## <a name="blockunblock-users"></a>Benutzer sperren/zulassen
Mit „Benutzer sperren/zulassen“ kann verhindert werden, dass Benutzer Authentifizierungsanforderungen erhalten. Authentifizierungsversuche für gesperrte Benutzer werden automatisch verweigert. Gesperrte Benutzer bleiben ab Zeitpunkt der Sperrung 90 Tage lang gesperrt.

### <a name="block-a-user"></a>Sperren eines Benutzers
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **MFA Server** > **Benutzer sperren/zulassen**.
3. Klicken Sie auf **Hinzufügen**, um einen Benutzer zu sperren.
4. Wählen Sie die **Replikationsgruppe**, geben Sie den gesperrten Benutzernamen im Format **username@domain.com** ein, und fügen Sie einen Kommentar im Feld **Grund** hinzu.
5. Klicken Sie auf **Hinzufügen**, um das Sperren des Benutzers abzuschließen.

### <a name="unblock-a-user"></a>Zulassen eines Benutzers
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **MFA Server** > **Benutzer sperren/zulassen**.
3. Klicken Sie neben dem Benutzer, den Sie entsperren möchten, in der Spalte **Aktion** auf **Zulassen**.
4. Geben Sie im Feld **Grund für Entsperren** einen Kommentar ein.
5. Klicken Sie auf **Zulassen**, um das Entsperren des Benutzers abzuschließen.

## <a name="fraud-alert"></a>Betrugswarnung
Die Betrugswarnung kann so konfiguriert und eingerichtet werden, dass Ihre Benutzer betrügerische Versuche, auf ihre Ressourcen zuzugreifen, melden können.  Benutzer können entweder mit der mobilen App oder über ihre Rufnummer den Betrug melden.

### <a name="turn-on-fraud-alert"></a>Aktivieren einer Betrugswarnung
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **MFA Server** > **Betrugswarnung**.

   ![Betrugswarnung](./media/multi-factor-authentication-whats-next/fraudalert.png)

3. Setzen Sie **Benutzern die Ausgabe von Betrugswarnungen standardmäßig erlauben** auf **Ein**.
4. Wählen Sie **Speichern** aus.

### <a name="configuration-options"></a>Konfigurationsoptionen

- **Benutzer bei Betrugsmeldung sperren**: Wenn ein Benutzer einen Betrug meldet, wird sein Konto gesperrt.
- **Code zum Melden von Betrugsversuchen während der Begrüßung**: Wenn Benutzer einen Telefonanruf zur Ausführung der Überprüfung in zwei Schritten empfangen, drücken sie normalerweise die #-Taste, um ihre Anmeldung zu bestätigen. Wenn sie einen Betrug melden möchten, geben sie vor dem Drücken der #-TASTE einen Code ein. Dieser Code ist standardmäßig **0**, Sie können ihn jedoch anpassen.

> [!NOTE]
> In Microsofts Standardansage wird der Benutzer aufgefordert, zum Senden einer Betrugswarnung die Zeichenfolge „0#“ einzugeben. Wenn Sie einen anderen Code als „0“ verwenden wollen, sollten Sie eine benutzerdefinierte Ansage mit den passenden Anweisungen aufnehmen.

### <a name="view-fraud-reports"></a>Anzeigen von Betrugsberichten
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Wählen Sie das Verzeichnis, das Sie verwalten möchten. 
4. Wählen Sie **Konfigurieren** aus.
5. Wählen Sie unter „Multi-Factor Authentication“ die Option **Diensteinstellungen verwalten**.
6. Wählen Sie am unteren Rand der Seite „Diensteinstellungen“ **Portal aufrufen**.
7. Klicken Sie im Azure Multi-Factor Authentication-Verwaltungsportal unter „Einen Bericht anzeigen“ auf **Betrugswarnung**.
8. Geben Sie den Datumsbereich an, der im Bericht angezeigt werden soll. Sie können auch Benutzernamen, Telefonnummern und den Benutzerstatus angeben.
9. Klicken Sie auf **Run**(Ausführen). Daraufhin wird ein Bericht mit Betrugswarnungen angezeigt. Klicken Sie auf **Exportieren nach CSV**, wenn Sie den Bericht exportieren möchten.

## <a name="one-time-bypass"></a>Einmalumgehung
Mit einer Einmalumgehung kann sich ein Benutzer ein einziges Mal authentifizieren, ohne die Überprüfung in zwei Schritten auszuführen. Die Umgehung ist vorübergehend und läuft nach einer angegebenen Anzahl von Sekunden ab. Falls die mobile Anwendung oder das Telefon keine Benachrichtigung bzw. keinen Telefonanruf empfängt, können Sie eine Einmalumgehung aktivieren, damit der Benutzer auf die gewünschte Ressource zugreifen kann.

### <a name="create-a-one-time-bypass"></a>Erstellen einer Einmalumgehung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **MFA Server** > **Einmalumgehung**.

   ![Einmalumgehung](./media/multi-factor-authentication-whats-next/onetimebypass.png)
3. Wählen Sie **Hinzufügen**.
4. Aktivieren Sie bei Bedarf die Replikationsgruppe für diese Umgehung.
5. Geben Sie den Benutzernamen (in der Form username@domain.com), die Anzahl von Sekunden, für die die Umgehung gelten soll, und den Grund für die Umgehung ein. 
6. Wählen Sie **Hinzufügen**. Das Zeitlimit gilt sofort. Daher muss sich der Benutzer anmelden, bevor die Einmalumgehung abläuft. 

### <a name="view-the-one-time-bypass-report"></a>Anzeigen des Berichts für die Einmalumgehung
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Wählen Sie das Verzeichnis, das Sie verwalten möchten. 
4. Wählen Sie **Konfigurieren** aus.
5. Wählen Sie unter „Multi-Factor Authentication“ die Option **Diensteinstellungen verwalten**.
6. Wählen Sie am unteren Rand der Seite „Diensteinstellungen“ **Portal aufrufen**.
7. Klicken Sie im Azure Multi-Factor Authentication-Verwaltungsportal unter „Einen Bericht anzeigen“ auf **Einmalumgehung**.
8. Geben Sie den Datumsbereich an, der im Bericht angezeigt werden soll. Sie können auch Benutzernamen, Telefonnummern und den Benutzerstatus angeben.
9. Klicken Sie auf **Run**(Ausführen). Daraufhin wird ein Bericht mit Umgehungen angezeigt. Klicken Sie auf **Exportieren nach CSV**, wenn Sie den Bericht exportieren möchten.

## <a name="custom-voice-messages"></a>Benutzerdefinierte Sprachnachrichten
Mit benutzerdefinierten Sprachnachrichten können Sie Ihre eigenen Aufzeichnungen oder Begrüßungen für die Überprüfung in zwei Schritten verwenden. Diese können zusätzlich zu den Microsoft-Aufzeichnungen verwendet werden oder diese ersetzen.

Bevor Sie beginnen, sollten Sie Folgendes beachten:

* Die unterstützten Dateiformate sind WAV und MP3.
* Die maximale Dateigröße beträgt 5 MB.
* Authentifizierungsnachrichten sollten kürzer als 20 Sekunden sein. Bei längeren Nachrichten ist die Überprüfung unter Umständen nicht erfolgreich, da der Benutzer erst reagieren kann, wenn die Nachricht zu Ende ist. Dadurch tritt bei der Überprüfung ein Timeout auf.

### <a name="set-up-a-custom-message"></a>Einrichten einer benutzerdefinierten Nachricht

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **MFA Server** > **Einstellungen für Telefonanruf**.

   ![Einstellungen für Telefonanruf](./media/multi-factor-authentication-whats-next/phonecallsettings.png)

3. Wählen Sie **Begrüßung hinzufügen**.
4. Wählen Sie den Begrüßungstyp und die Sprache.
5. Wählen Sie eine MP3- oder WAV-Audiodatei zum Hochladen aus.
6. Wählen Sie **Hinzufügen**.

## <a name="caching-in-azure-multi-factor-authentication"></a>Zwischenspeichern in Azure Multi-Factor Authentication
Durch Zwischenspeichern können Sie einen bestimmten Zeitraum festlegen, sodass nachfolgende Authentifizierungsversuche in diesem Zeitraum automatisch erfolgreich sind. Dies wird hauptsächlich verwendet, wenn lokale Systeme, wie z. B. VPNs, mehrere Verifizierungsanfragen senden, während die erste Anfrage noch bearbeitet wird. Dadurch werden die nachfolgenden Anforderungen automatisch erfolgreich ausgeführt, nachdem die erste laufende Verifizierungsanfrage für den Benutzer erfolgreich ausgeführt wurde. 

Das Zwischenspeichern ist nicht für Anmeldungen bei Azure AD gedacht.

### <a name="set-up-caching"></a>Einrichten der Zwischenspeicherung 
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **MFA Server** > **Cacheregeln**.

   ![Cacheregeln](./media/multi-factor-authentication-whats-next/cachingrules.png)

4. Wählen Sie **Hinzufügen**.
5. Wählen Sie den Cachetyp aus der Dropdownliste aus, und geben Sie die maximale Aufbewahrungsdauer im Cache in Sekunden an. 
6. Wählen Sie bei Bedarf einen Authentifizierungstyp aus, und geben Sie eine Anwendung an. 
7. Wählen Sie **Hinzufügen**.


## <a name="trusted-ips"></a>Vertrauenswürdige IP-Adressen
Mit vertrauenswürdigen IP-Adressen der Azure MFA können Administratoren eines Mandanten oder Verbundmandanten die Überprüfung in zwei Schritten für Benutzer umgehen, die sich über das lokale Intranet des Unternehmens anmelden. Dieses Feature ist in der Vollversion von Azure Multi-Factor Authentication, aber nicht in der kostenlosen Version für Administratoren verfügbar. Weitere Informationen zum Beziehen der Vollversion von Azure Multi-Factor Authentication finden Sie unter [Azure Multi-Factor Authentication](multi-factor-authentication.md).

| Typen von Azure AD-Mandanten | Verfügbare vertrauenswürdige IP-Optionen |
|:--- |:--- |
| Verwaltet |<li>Bestimmte IP-Adressbereiche: Administratoren können einen Bereich von IP-Adressen angeben, die die Überprüfung in zwei Schritten für Benutzer umgehen können, die sich vom Intranet des Unternehmens aus anmelden.</li> |
| Im Verbund |<li>Alle Verbundbenutzer: Alle Verbundbenutzer, die sich von innerhalb des Unternehmen aus anmelden, umgehen die Überprüfung in zwei Schritten mithilfe eines von AD FS ausgestellten Anspruchs.</li><br><li>Bestimmte IP-Adressbereiche: Administratoren können einen Bereich von IP-Adressen angeben, die die Überprüfung in zwei Schritten für Benutzer umgehen können, die sich vom Intranet des Unternehmens aus anmelden. |

Diese Umgehung funktioniert nur von innerhalb des Intranets eines Unternehmens. Wenn Sie beispielsweise alle Verbundbenutzer ausgewählt haben und sich ein Benutzer von außerhalb des Unternehmensintranets anmeldet, muss sich dieser Benutzer mit der Überprüfung in zwei Schritten authentifizieren. Dies gilt auch, wenn der Benutzer einen AD FS-Anspruch vorweisen kann. 

**Endbenutzererfahrung innerhalb des Unternehmensnetzwerks:**

Wenn vertrauenswürdige IPs deaktiviert sind, ist für Browserflüsse die Überprüfung in zwei Schritten erforderlich. Für ältere Rich Client-Apps werden App-Kennwörter benötigt. 

Wenn vertrauenswürdige IPs aktiviert sind, ist die Überprüfung in zwei Schritten für Browserflüsse *nicht* erforderlich. Für ältere Rich Client-Apps werden *keine* App-Kennwörter benötigt (vorausgesetzt, der Benutzer hat nicht bereits ein App-Kennwort erstellt). Sobald ein App-Kennwort verwendet wird, ist es erforderlich. 

**Endbenutzererfahrung außerhalb des Unternehmensnetzwerks:**

Unabhängig davon, ob vertrauenswürdige IPs aktiviert sind, ist die Überprüfung in zwei Schritten für Browserflüsse erforderlich, und für ältere Rich Client-Apps werden App-Kennwörter benötigt. 

### <a name="to-enable-trusted-ips"></a>Aktivieren von vertrauenswürdigen IP-Adressen
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Wählen Sie das Verzeichnis, das Sie verwalten möchten. 
4. Wählen Sie **Konfigurieren** aus.
5. Wählen Sie unter „Multi-Factor Authentication“ die Option **Diensteinstellungen verwalten**.
6. Auf der Seite „Diensteinstellungen“ haben Sie unter „Vertrauenswürdige IPs“ zwei Optionen:
   
   * **Für Anforderungen von Partnerbenutzern, die aus meinem Intranet stammen**: Aktivieren Sie das Kontrollkästchen. Alle Verbundbenutzer, die sich vom Unternehmensnetzwerk aus anmelden, umgehen die Überprüfung in zwei Schritten mithilfe eines von AD FS ausgestellten Anspruchs. Stellen Sie sicher, dass AD FS über eine Regel zum Hinzufügen des Intranetanspruchs zum entsprechenden Datenverkehr verfügt. Sie sollten in AD FS die folgende Regel erstellen, wenn sie noch nicht vorhanden ist: „c:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);“



   * **Für Anforderungen aus einem bestimmten Bereich öffentlicher IPs**: Geben Sie mithilfe der CIDR-Notation die IP-Adressen in das Textfeld ein. Beispiel: xxx.xxx.xxx.0/24 für IP-Adressen im Bereich xxx.xxx.xxx. 1 – xxx.xxx.xxx. 254 oder xxx.xxx.xxx.xxx/32 für eine einzelne IP-Adresse. Sie können bis zu 50 IP-Adressbereiche eingeben. Benutzer, die sich über diese IP-Adressen anmelden, umgehen die Überprüfung in zwei Schritten.
7. Klicken Sie auf **Speichern**.
8. Sobald die Updates angewendet wurden, klicken Sie auf **Schließen**.

![Vertrauenswürdige IP-Adressen](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>App-Kennwörter
Einige Apps wie Office 2010 oder ältere Versionen und Apple Mail unterstützen die Überprüfung in zwei Schritten nicht. Sie sind nicht für die Annahme einer zweiten Überprüfung konfiguriert. Um diese Apps zu verwenden, müssen Sie „App-Kennwörter“ anstelle Ihres herkömmlichen Kennworts angeben. Mit App-Kennwörtern kann die Anwendung die Überprüfung in zwei Schritten umgehen und weiter ausgeführt werden.

> [!NOTE]
> Moderne Authentifizierung für Office 2013-Clients
> 
> Office 2013-Clients (einschließlich Outlook) und neuere Clients unterstützen moderne Authentifizierungsprotokolle und können für die Überprüfung in zwei Schritten aktiviert werden. Nach der Aktivierung sind für diese Clients keine App-Kennwörter erforderlich.  Weitere Informationen finden Sie unter [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/) („Öffentliche Preview für moderne Authentifizierung in Office 2013“, in englischer Sprache).

### <a name="important-things-to-know-about-app-passwords"></a>Wichtige Informationen zu App-Kennwörtern
Es folgt eine Liste mit wichtigen Informationen, die Sie über App-Kennwörter wissen sollten.

* Die Eingabe von App-Kennwörtern sollte in jeder App nur einmal erforderlich sein. So müssen sich Benutzer ihre Kennwörter nicht merken und jedes Mal erneut eingeben.
* Das eigentliche Kennwort wird automatisch erzeugt und nicht vom Benutzer festgelegt. Ein Grund hierfür ist, dass automatisch generierte Kennwörter sicherer und für einen Angreifer schwerer zu erraten sind.
* Pro Benutzer können maximal 40 Kennwörter festgelegt werden. 
* Anwendungen, die Kennwörter zwischenspeichern und in lokalen Szenarios nutzen, könnten fehlschlagen, weil das App-Kennwort außerhalb der Organisations-ID nicht bekannt ist. Ein Beispiel sind Exchange-E-Mails, die lokal vorhanden sind, bei denen sich die archivierten Nachrichten jedoch in der Cloud befinden. Das gleiche Kennwort funktioniert nicht.
* Nach der Aktivierung der mehrstufigen Authentifizierung in einem Benutzerkonto können App-Kennwörter mit den meisten Nicht-Browserclients wie Outlook und Lync verwendet werden, wobei administrative Aufgaben auch dann nicht mithilfe von App-Kennwörtern über Nicht-Browseranwendungen wie Windows Power Shell durchgeführt werden können, wenn der Benutzer über ein Administratorkonto verfügt.  Stellen Sie sicher, dass Sie zum Ausführen von PowerShell-Skripts ein Dienstkonto mit einem sicheren Kennwort erstellen und für dieses Konto die Überprüfung in zwei Schritten nicht aktivieren.

> [!WARNING]
> App-Kennwörter funktionieren nicht in Hybridumgebungen, in denen Clients sowohl mit lokalen AutoErmittlung-Endpunkten als auch mit solchen in der Cloud kommunizieren. Das liegt daran, dass Domänenkennwörter für die lokale Authentifizierung, und App-Kennwörter für die Authentifizierung in der Cloud erforderlich sind.

### <a name="naming-guidance-for-app-passwords"></a>Benennungsrichtlinien für App-Kennwörter
Die Namen der App-Kennwörter sollten auf das Gerät hinweisen, auf dem sie verwendet werden. Wenn Sie beispielsweise einen Laptop haben, der über Nicht-Browser-Apps wie Outlook, Word und Excel verfügt, erstellen Sie ein App-Kennwort mit dem Namen „Laptop“ und verwenden dieses App-Kennwort in diesen Anwendungen. Anschließend erstellen Sie ein weiteres App-Kennwort namens „Desktop“ für die gleichen Anwendungen auf dem Desktopcomputer. 

Microsoft empfiehlt, jeweils ein App-Kennwort pro Gerät (und nicht pro Anwendung) zu erstellen.

### <a name="federated-sso-app-passwords"></a>App-Kennwörter im Verbund (SSO)
Azure AD unterstützt den Verbund (Einmaliges Anmelden) mit lokalen Windows Server Active Directory Domain Services (AD DS). Wenn Ihre Organisation im Verbund mit Azure AD besteht und Sie Azure Multi-Factor Authentication verwenden möchten, sind die folgenden Informationen zu App-Kennwörtern wichtig für Sie. Dieser Abschnitt gilt nur für Verbundkunden (SSO).

* App-Kennwörter werden von Azure AD überprüft, sodass der Verbund umgangen wird. Der Verbund wird nur beim Einrichten von App-Kennwörtern aktiv verwendet.
* Im Gegensatz zum passiven Ablauf gehen wir bei Verbundbenutzern (SSO) nie zum Identitätsanbieter (Identity Provider, IdP). Die Kennwörter werden in der Organisations-ID gespeichert. Wenn der Benutzer das Unternehmen verlässt, muss diese Information zur Organisations-ID übertragen werden, und zwar mithilfe von DirSync in Echtzeit. Nach dem Deaktivieren oder Löschen von Konten kann die Synchronisierung bis zu drei Stunden dauern, sodass das Deaktivieren bzw. Löschen des App-Kennworts in Azure AD verzögert wird.
* Lokale Einstellungen für die Clientzugriffssteuerung werden vom App-Kennwort nicht berücksichtigt.
* Für das App-Kennwort ist keine lokale Funktion zur Protokollierung oder Überwachung der Authentifizierung verfügbar.
* In bestimmten erweiterten Architekturentwürfen ist bei der Verwendung der Überprüfung in zwei Schritten mit Clients möglicherweise eine Kombination aus Organisationsbenutzername/-kennwörtern und App-Kennwörtern erforderlich, je nachdem, wo die Authentifizierung stattfindet. Bei Clients, die sich bei einer lokalen Infrastruktur authentifizieren, verwenden Sie einen Organisationsbenutzernamen und ein Organisationskennwort. Für Clients, die bei Azure AD authentifizieren, verwenden Sie das App-Kennwort.

  Nehmen wir beispielsweise an, Sie verfügen über eine Architektur, die Folgendes umfasst:

  * Sie werden Ihre lokale Instanz von Active Directory mit Azure AD in Verbund setzen
  * Sie verwenden Exchange online.
  * Sie verwenden Lync, das speziell lokal ist
  * Sie verwenden Azure Multi-Factor Authentication,

  ![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

  In diesen Fällen müssen Sie Folgendes tun:

  * Verwenden Sie beim Anmelden bei Lync den Benutzernamen und das Kennwort Ihres Unternehmens.
  * Verwenden Sie ein App-Kennwort beim Versuch, über einen Outlook-Client auf das Adressbuch zuzugreifen, der die Verbindung mit Exchange Online herstellt.

### <a name="allow-app-password-creation"></a>Zulassen der Erstellung von App-Kennwörtern
Standardmäßig können keine Benutzer App-Kennwörter erstellen. Dieses Feature muss aktiviert sein. Gehen Sie wie folgt vor, um Benutzern die Erstellung von App-Kennwörtern zu ermöglichen:

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Wählen Sie das Verzeichnis, das Sie verwalten möchten. 
4. Wählen Sie **Konfigurieren** aus.
5. Wählen Sie unter „Multi-Factor Authentication“ die Option **Diensteinstellungen verwalten**.
6. Wählen Sie das Optionsfeld neben **Benutzern das Erstellen von App-Kennwörtern zum Anmelden bei Anwendungen gestatten, die nicht auf Browsern basieren**.

![App-Kennwörter erstellen](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Erstellen von App-Kennwörter
Benutzer können App-Kennwörter während ihrer ersten Registrierung erstellen. Am Ende des Registrierungsprozesses wird eine Option angezeigt, mit der sie App-Kennwörter erstellen können.

Benutzer können App-Kennwörter auch nach der Registrierung erstellen, indem sie ihre Einstellungen im Azure-Portal oder im Office 365-Portal erstellen. Weitere Informationen und detaillierte Schritte für Ihre Benutzer finden Sie unter [Welchen Zweck erfüllen App-Kennwörter bei Azure Multi-Factor Authentication?](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Speichern der Multi-Factor Authentication für Geräte, denen Benutzer vertrauen
Die Speicherung der Multi-Factor Authentication für Geräte und Browser, denen Benutzer vertrauen, ist eine kostenlose Funktion für alle MFA-Benutzer. Damit können Sie Benutzern die Möglichkeit geben, MFA für eine festgelegte Anzahl von Tagen zu umgehen, nachdem sie sich erfolgreich mithilfe von MFA angemeldet haben. Bei Verwendung dieser Option muss der Benutzer auf einem Gerät seltener eine Überprüfung in zwei Schritten durchführen, was zur Verbesserung der Verwendbarkeit beiträgt.

Ist ein Konto oder Gerät gefährdet, kann das Speichern von MFA für vertrauenswürdige Geräte jedoch die Sicherheit beeinträchtigen. Wenn ein Unternehmenskonto kompromittiert wird oder ein vertrauenswürdiges Gerät verloren geht oder gestohlen wird, sollten Sie die [Multi-Factor Authentication auf allen Geräten wiederherstellen](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Durch diese Aktion wird der vertrauenswürdige Status aller Geräte widerrufen, und der Benutzer muss wieder die Überprüfung in zwei Schritten ausführen. Sie können Ihre Benutzer auch anweisen, MFA auf ihren eigenen Geräten anhand der Anweisungen unter [Verwalten der Einstellungen für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted) wiederherzustellen.

### <a name="how-it-works"></a>So funktioniert's

Das Speichern der Multi-Factor Authentication funktioniert durch das Festlegen eines permanenten Cookies im Browser, wenn ein Benutzer das Kontrollkästchen „Die nächsten **X** Tage nicht erneut fragen“ aktiviert. Der Benutzer wird von diesem Browser bis zum Ablauf des Cookies nicht erneut zur MFA aufgefordert. Wenn der Benutzer einen anderen Browser auf dem gleichen Gerät öffnet oder seine Cookies löscht, wird er wieder zur Verifizierung aufgefordert. 

Das Kontrollkästchen „Die nächsten **X** Tage nicht erneut fragen“ wird in Nicht-Browser-Apps nicht angezeigt, unabhängig davon, ob sie die moderne Authentifizierung unterstützen. Diese Apps verwenden Aktualisierungstoken, die jede Stunde neue Zugriffstoken bereitstellen. Bei der Überprüfung eines Aktualisierungstokens prüft Azure AD, ob die Überprüfung in zwei Schritten zuletzt innerhalb der konfigurierten Anzahl von Tagen durchgeführt wurde. 

So reduziert sich durch das Speichern der MFA auf vertrauenswürdigen Geräten die Anzahl der Authentifizierungen für Web-Apps (die normalerweise jedes Mal dazu auffordern). Auf der anderen Seite erhöht sich jedoch die Anzahl der Authentifizierungen für Clients mit moderner Authentifizierung (die normalerweise alle 90 Tage dazu aufgefordert werden).

> [!NOTE]
>Dieses Feature ist nicht kompatibel mit dem AD FS-Feature „Angemeldet bleiben“, bei dem Benutzer die Überprüfung in zwei Schritten für AD FS über Azure MFA Server oder eine MFA-Lösung von Drittanbietern ausführen. Wenn Ihre Benutzer in AD FS „Angemeldet bleiben“ auswählen und ihr Gerät außerdem als für MFA vertrauenswürdig markieren, können sie sich nach Ablauf der Anzahl von Tagen, die unter „MFA speichern“ angegeben wurde, nicht mehr verifizieren. Azure AD fordert eine neue Überprüfung in zwei Schritten an, aber AD FS gibt ein Token mit dem ursprünglichen MFA-Anspruch und Datum zurück, statt die Überprüfung in zwei Schritten erneut durchzuführen. Dadurch entsteht eine Schleife bei der Überprüfung zwischen Azure AD und AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Aktivieren der Speicherung der mehrstufigen Authentifizierung
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Wählen Sie das Verzeichnis, das Sie verwalten möchten. 
4. Wählen Sie **Konfigurieren** aus.
5. Wählen Sie unter „Multi-Factor Authentication“ die Option **Diensteinstellungen verwalten**.
6. Aktivieren Sie auf der Seite „Diensteinstellungen“ unter „Geräteeinstellungen von Benutzern verwalten“ die Option **Benutzern das Speichern der mehrstufigen Authentifizierung auf vertrauenswürdigen Geräten ermöglichen**.
   ![Speichern von Geräten](./media/multi-factor-authentication-whats-next/remember.png)
7. Legen Sie fest, für wie viele Tage die vertrauenswürdigen Geräte die Überprüfung in zwei Schritten umgehen können. Der Standardwert ist 14 Tage.
8. Klicken Sie auf **Speichern**.
9. Klicken Sie auf **Schließen**.

### <a name="mark-a-device-as-trusted"></a>Markieren eines Geräts als vertrauenswürdig

Nach der Aktivierung dieser Funktion können Benutzer bei der Anmeldung ein Gerät als vertrauenswürdig markieren, indem sie **Nicht mehr nachfragen** aktivieren.

![Nicht mehr nachfragen (Screenshot)](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Auswählbare Verifizierungsmethoden
Sie können auswählen, welche Verifizierungsmethoden für Ihre Benutzer verfügbar sind. In der folgenden Tabelle finden Sie eine kurze Übersicht über jede Methode.

Wenn Ihre Benutzer Ihre Konten für MFA registrieren, wählen sie ihre bevorzugte Verifizierungsmethode aus den Optionen aus, die Sie zur Verfügung gestellt haben. Anleitungen zum Registrierungsprozess finden Sie unter [Einrichten meines Kontos für die Überprüfung in zwei Schritten](multi-factor-authentication-end-user-first-time.md)

| Methode | Beschreibung |
|:--- |:--- |
| Auf Telefon anrufen |Startet einen automatisierten Sprachanruf. Der Benutzer nimmt den Anruf an und drückt die #-Taste auf der Telefontastatur, um sich zu authentifizieren. Diese Telefonnummer wird nicht mit dem lokalen Active Directory synchronisiert. |
| Textnachricht an Telefon |Sendet eine Textnachricht mit einem Überprüfungscode. Der Benutzer wird aufgefordert, mit dem Überprüfungscode auf die Textnachricht zu antworten oder den Überprüfungscode auf der Anmeldeseite einzugeben. |
| Benachrichtigung über mobile App |Sendet eine Pushbenachrichtigung an Ihr Telefon oder registriertes Gerät. Der Benutzer zeigt die Benachrichtigung an und wählt **Überprüfen**, um die Überprüfung abzuschließen. <br>Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar. |
| Überprüfungscode von der mobilen App |Die Microsoft Authenticator-App generiert alle 30 Sekunden einen neuen OATH-Überprüfungscode. Der Benutzer gibt diesen Überprüfungscode auf der Anmeldeoberfläche ein.<br>Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar. |

### <a name="how-to-enabledisable-authentication-methods"></a>Aktivieren/Deaktivieren von Authentifizierungsmethoden
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Wählen Sie das Verzeichnis, das Sie verwalten möchten. 
4. Wählen Sie **Konfigurieren** aus.
5. Wählen Sie unter „Multi-Factor Authentication“ die Option **Diensteinstellungen verwalten**.
6. Aktivieren oder deaktivieren Sie auf der Seite „Diensteinstellungen“ unter „Überprüfungsoptionen“ die Optionen, die Sie verwenden bzw. nicht verwenden möchten.
   ![Überprüfungsoptionen](./media/multi-factor-authentication-whats-next/authmethods.png)
7. Klicken Sie auf **Speichern**.
8. Klicken Sie auf **Schließen**.

