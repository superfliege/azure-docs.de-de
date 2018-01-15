---
title: "Azure AD SSPR mit Kennwortrückschreiben | Microsoft-Dokumentation"
description: "Verwenden von Azure AD und Azure AD Connect zum Rückschreiben von Kennwörtern in ein lokales Verzeichnis"
services: active-directory
keywords: "Active Directory-Kennwortverwaltung, Kennwortverwaltung, Self-Service-Kennwortzurücksetzung in Azure AD"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: b4a14d3c79f93988eeac1525da09cf70dc2de634
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="password-writeback-overview"></a>Übersicht über die Kennwortrückschreibung

Mit Kennwortrückschreiben können Sie Azure Active Directory (Azure AD) so konfigurieren, dass Kennwörter in die lokale Active Directory-Instanz zurückgeschrieben werden. Kennwortrückschreiben bewirkt, dass es nicht mehr erforderlich ist, eine komplizierte lokale Lösung für Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) einzurichten und zu verwalten. Gleichzeitig bietet diese Funktion Ihren Benutzern eine bequeme, cloudbasierte Möglichkeit zum Zurücksetzen ihrer lokalen Kennwörter – unabhängig davon, wo sie sich gerade befinden. Kennwortrückschreiben ist eine Komponente von [Azure Active Directory Connect](./connect/active-directory-aadconnect.md), die von den aktuellen Abonnenten von [Azure Active Directory Premium-Editionen](active-directory-whatis.md) aktiviert und verwendet werden kann.

Kennwortrückschreiben bietet die folgenden Features:

* **Bereitstellung von Feedback ohne Verzögerung**: Kennwortrückschreiben ist ein synchroner Vorgang. Ihre Benutzer werden sofort benachrichtigt, wenn ihre Kennwörter nicht der Richtlinie entsprechen oder das Zurücksetzen oder Ändern des Kennworts aus irgendeinem Grund nicht möglich war.
* **Unterstützung für Kennwortzurücksetzungen für Benutzer, die Active Directory Federation Services (AD FS) oder andere Verbundtechnologien nutzen**: Solange die Verbundbenutzerkonten mit Ihrem Azure AD-Mandanten synchronisiert sind, können die Benutzer durch das Kennwortrückschreiben ihre lokalen Active Directory-Kennwörter über die Cloud verwalten.
* **Unterstützung für Kennwortzurücksetzungenen für Benutzer, die eine** Kennworthashsynchronisierung[ verwenden](./connect/active-directory-aadconnectsync-implement-password-synchronization.md): Wenn der Dienst für die Kennwortzurücksetzung ermittelt, dass ein synchronisiertes Benutzerkonto für die Kennworthashsynchronisierung aktiviert ist, werden das lokale Kennwort und das Cloudkennwort für dieses Konto simultan zurückgesetzt.
* **Unterstützung von Kennwortänderungen über den Zugriffsbereich und Office 365**: Wenn Verbundbenutzer oder Benutzer mit Kennwortsynchronisierung ihre abgelaufenen oder noch nicht abgelaufenen Kennwörter ändern möchten, werden diese Kennwörter in die lokale Active Directory-Umgebung zurückgeschrieben.
* **Unterstützung für Kennwortrückschreiben, wenn die Kennwörter im Azure-Portal von einem Administrator zurückgesetzt werden**: Wenn ein Administrator das Kennwort eines Benutzers im [Azure-Portal](https://portal.azure.com) zurücksetzt und der Benutzer ein Verbundkonto oder ein Konto mit Kennwortsynchronisierung verwendet, wird das vom Administrator ausgewählte Kennwort auch im lokalen Active Directory festgelegt. Diese Funktionalität wird im Office-Verwaltungsportal derzeit nicht unterstützt.
* **Erzwingung von lokalen Active Directory-Kennwortrichtlinien:**: Wenn ein Benutzer sein Kennwort zurücksetzt, wird sichergestellt, dass es die lokalen Active Directory-Richtlinien erfüllt, bevor es für dieses Verzeichnis übernommen wird. Diese Überprüfung umfasst Prüfen des Verlaufs, der Komplexität, des Alters, der Kennwortfilter und aller weiteren Kennwortbeschränkungen, die Sie im lokalen Active Directory definiert haben.
* **Keine Anforderung, dass irgendeine eingehende Firewallregel vorhanden sein muss**: Kennwortrückschreiben verwendet ein Azure Service Bus Relay als zugrunde liegenden Kommunikationskanal. Sie müssen keine eingehenden Ports in der Firewall öffnen, damit dieses Feature funktioniert.
* **Keine Unterstützung für Benutzerkonten in geschützten Gruppen, die im lokalen Active Directory vorliegen**: Weitere Informationen zu geschützten Gruppen finden Sie unter [Geschützte Konten und Gruppen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>Funktionsweise der Kennwortrückschreibung

Wenn Verbundbenutzer oder Benutzer mit Kennworthashsynchronisierung ihr Kennwort in der Cloud zurücksetzen oder ändern, geschieht Folgendes:

1. Es wird überprüft, über welche Art von Kennwort der Benutzer verfügt. Wenn festgestellt wird, dass das Kennwort lokal verwaltet wird:
   * Es wird überprüft, ob der Dienst für Rückschreiben aktiviert ist und ausgeführt wird. Ist dies der Fall, kann der Benutzer den Vorgang fortsetzen.
   * Wenn der Dienst für Rückschreiben nicht aktiviert ist, wird dem Benutzer mitgeteilt, dass sein Kennwort derzeit nicht zurückgesetzt werden kann.
2. Im nächsten Schritt muss der Benutzer sich erfolgreich authentifizieren und gelangt dann zur Seite **Kennwort zurücksetzen**.
3. Der Benutzer wählt ein neues Kennwort aus und bestätigt es.
4. Wenn der Benutzer **Senden** auswählt, wird das Klartextkennwort mit einem symmetrischen Schlüssel verschlüsselt, der beim Einrichten des Kennwortrückschreibens erstellt wurde.
5. Nachdem das Kennwort verschlüsselt wurde, wird es in eine Nutzlast eingeschlossen, die über einen HTTPS-Kanal an das mandantenspezifische Service Bus Relay gesendet wird (dieses wurde ebenfalls beim Einrichten der Kennwortrückschreibung festgelegt). Dieses Relay wird durch ein zufällig generiertes Kennwort geschützt, das nur der lokalen Installation bekannt ist.
6. Sobald die Nachricht den Service Bus erreicht hat, wird der Endpunkt für die Kennwortzurücksetzung automatisch aktiviert, und der Endpunkt erkennt, dass eine Anforderung zur Zurücksetzung aussteht.
7. Der Dienst sucht dann nach dem Benutzer, indem er das Cloudankerattribut verwendet. Damit diese Suche erfolgreich ist:

    * Das Benutzerobjekt muss im Active Directory-Connectorbereich vorhanden sein.
    * Das Benutzerobjekt muss mit dem entsprechenden Metaverseobjekt (MV-Objekt) verknüpft sein.
    * Das Benutzerobjekt muss mit dem entsprechenden Azure Active Directory-Connectorobjekt verknüpft sein.
    * Für die Verknüpfung zwischen dem Active Directory-Connectorobjekt und dem MV-Objekt muss die Synchronisierungsregel `Microsoft.InfromADUserAccountEnabled.xxx` festgelegt sein. <br> <br>
    Sobald der Aufruf aus der Cloud eingetroffen ist, verwendet das Synchronisierungsmodul das **cloudAnchor**-Attribut, um das Azure Active Directory-Connectorbereichsobjekt nachzuschlagen. Es folgt dann dem Link zurück zum MV-Objekt und folgt anschließend dem Link zurück zum Active Directory-Objekt. Da mehrere Active Directory-Objekte (Multi-Gesamtstruktur) für denselben Benutzer vorliegen können, verwendet das Synchronisierungsmodul die `Microsoft.InfromADUserAccountEnabled.xxx`-Verbindung, um das richtige Objekt auszuwählen.

    > [!Note]
    > Als Ergebnis dieser Logik muss Azure AD Connect mit dem PDC-Emulator (primärer Domänencontroller) kommunizieren können, damit das Kennwortrückschreiben funktioniert. Wenn Sie dies manuell aktivieren müssen, können Sie Azure AD Connect mit dem PDC-Emulator verbinden. Klicken Sie mit der rechten Maustaste auf die Eigenschaften (**properties**) des Active Directory-Synchronisierungsconnectors, und wählen Sie dann **configure directory partitions** aus. Suchen Sie nun nach dem Abschnitt **domain controller connection settings**, und aktivieren Sie das Kontrollkästchen **only use preferred domain controllers**. Auch wenn der bevorzugte Domänencontroller kein PDC-Emulator ist, versucht Azure AD Connect für das Kennwortrückschreiben eine Verbindung mit dem PDC herzustellen.

8. Nachdem das Benutzerkonto ermittelt ist, wird versucht, das Kennwort direkt in der geeigneten Active Directory-Gesamtstruktur zurückzusetzen.
9. Wenn die Kennwortzurücksetzung erfolgreich war, wird der Benutzer darüber informiert, dass das Kennwort geändert wurde.
    > [!NOTE]
    > Wenn das Kennwort des Benutzers mithilfe der Kennwortsynchronisierung mit Azure AD synchronisiert wird, kann es vorkommen, dass die lokale Kennwortrichtlinie schwächer ist als die Kennwortrichtlinie der Cloud. In diesem Fall wird weiterhin die lokale Kennwortrichtlinie erzwungen und zusätzlich Kennworthashsynchronisierung verwendet, um den Hash dieses Kennworts zu synchronisieren. Diese Richtlinie stellt das Erzwingen Ihrer lokalen Richtlinie in der Cloud sicher, unabhängig davon, ob Sie für die Bereitstellung von einmaligem Anmelden Kennwortsynchronisierung oder Verbund verwenden.

10. Tritt beim Zurücksetzen des Kennworts ein Fehler auf, wird dem Benutzer eine Fehlermeldung angezeigt, und der Benutzer kann es erneut versuchen. Der Vorgang kann aus folgenden Gründen fehlschlagen:
    * Der Dienst war nicht verfügbar.
    * Das ausgewählte Kennwort entsprach nicht den Richtlinien der Organisation.
    * Der Benutzer wurde nicht im lokalen Active Directory gefunden.

    Es gibt für viele dieser Fälle eine spezielle Meldung, die den Benutzer darüber informiert, was zur Problemlösung unternommen werden kann.

## <a name="configure-password-writeback"></a>Konfigurieren von Kennwortrückschreiben

Es empfiehlt sich, das automatische Update von [Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md) zu verwenden, wenn Sie das Kennwortrückschreiben nutzen möchten.

DirSync und Azure AD Sync werden nicht mehr als Möglichkeit zum Aktivieren von Kennwortrückschreiben unterstützt. Weitere Informationen, die Ihnen bei der Umstellung nützlich sein können, finden Sie unter [Upgrade von DirSync und Azure AD Sync](connect/active-directory-aadconnect-dirsync-deprecated.md).

Für die folgenden Schritte wird davon ausgegangen, dass Sie Azure AD Connect bereits in Ihrer Umgebung konfiguriert haben, indem Sie die [Express](./connect/active-directory-aadconnect-get-started-express.md)- oder [benutzerdefinierten](./connect/active-directory-aadconnect-get-started-custom.md) Einstellungen verwendet haben.

1. Um das Kennwortrückschreiben zu konfigurieren und zu aktivieren, melden Sie sich bei Ihrem Azure AD Connect-Server an, und starten Sie den **Azure AD Connect**-Konfigurations-Assistenten.
2. Wählen Sie auf der Seite **Willkommen** die Option **Konfigurieren** aus.
3. Wählen Sie auf der Seite **Weitere Aufgaben** die Option **Synchronisierungsoptionen anpassen** aus, und wählen Sie dann **Weiter** aus.
4. Geben Sie auf der Seite **Mit Azure AD verbinden** die Anmeldeinformationen eines globalen Administrators ein, und wählen Sie dann **Weiter** aus.
5. Wählen Sie auf den Seiten **Verzeichnisse verbinden** und **Domänen-/OE-Filterung** die Schaltfläche **Weiter** aus.
6. Aktivieren Sie auf der Seite **Optionale Features** das Kontrollkästchen neben **Kennwortrückschreiben**, und wählen Sie **Weiter** aus.
   ![Aktivieren des Kennwortrückschreibens in Azure AD Connect][Writeback]
7. Wählen Sie auf der Seite **Bereit zur Konfiguration** die Option **Konfigurieren** aus, und warten Sie, bis der Vorgang abgeschlossen ist.
8. Wenn Sie sehen, dass die Konfiguration beendet ist, wählen Sie **Beenden** aus.

Informationen zu allgemeinen Aufgaben zur Problembehandlung im Zusammenhang mit dem Kennwortrückschreiben finden Sie im Artikel zur Problembehandlung im Abschnitt [Problembehandlung: Kennwortrückschreiben](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback).

## <a name="active-directory-permissions"></a>Active Directory-Berechtigungen

Für das in Azure AD Connect angegebene Konto müssen die folgenden Elemente festgelegt sein, wenn SSPR möglich sein soll:

* **Zurücksetzen des Kennworts** 
* **Kennwort ändern** 
* **Schreibberechtigungen** auf `lockoutTime`  
* **Schreibberechtigungen** auf `pwdLastSet`
* **Erweiterte Rechte** für eines der folgenden Elemente:
   * Das Stammobjekt von *jeder Domäne* in dieser Gesamtstruktur
   * Der Benutzerorganisationseinheiten, für die SSPR möglich sein soll

Wenn Sie nicht sicher sind, auf welches Konto sich das beschriebene Konto bezieht, öffnen Sie die Azure Active Directory Connect-Konfigurationsoberfläche, und wählen Sie die Option **Aktuelle Konfiguration anzeigen** aus. Das Konto, dem Sie Berechtigungen hinzufügen müssen, wird unter **Synchronisierte Verzeichnisse** angezeigt.

Wenn Sie diese Berechtigungen festlegen, kann das MA-Dienstkonto für jede Gesamtstruktur Kennwörter im Namen der Benutzerkonten innerhalb dieser Gesamtstruktur verwalten. 

> [!IMPORTANT]
> Wenn Sie diese Berechtigungen nicht gewähren, erhalten die Benutzer – obwohl das Zurückschreiben scheinbar ordnungsgemäß konfiguriert ist – Fehler bei dem Versuch, ihre lokalen Kennwörter über die Cloud zu verwalten.
>

> [!NOTE]
> Es kann bis zu einer Stunde oder länger dauern, bis diese Berechtigungen an alle Objekte in Ihrem Verzeichnis repliziert wurden.
>

Um die entsprechenden Berechtigungen für das Kennwortrückschreiben einzurichten, führen Sie die folgenden Schritte aus:

1. Öffnen Sie „Active Directory-Benutzer und -Computer“ mit einem Konto, das geeignete Berechtigungen für die Domänenverwaltung hat.
2. Vergewissern Sie sich, dass im Menü **Ansicht** die Option **Erweiterte Features** aktiviert ist.
3. Klicken Sie im linken Bereich mit der rechten Maustaste auf das Objekt, das den Stamm der Domäne repräsentiert, und wählen Sie **Eigenschaften** > **Sicherheit** > **Erweitert** aus.
4. Wählen Sie auf der Registerkarte **Berechtigungen** die Option **Hinzufügen** aus.
5. Wählen Sie das Konto aus, dem Berechtigungen zugewiesen werden sollen (vom Azure AD Connect-Setup).
6. Wählen Sie in der Dropdownliste **Gilt für** den Eintrag **Nachfolgerbenutzerobjekte** aus.
7. Aktivieren Sie unter **Berechtigungen** die Kontrollkästchen für folgende Optionen:
    * **Zurücksetzen des Kennworts**
    * **Kennwort ändern**
    * **lockoutTime schreiben**
    * **pwdLastSet schreiben**
8. Wählen Sie auf **Übernehmen/OK** aus, um die Änderungen zu übernehmen und alle geöffneten Dialogfelder zu schließen.

## <a name="licensing-requirements-for-password-writeback"></a>Lizenzierungsanforderungen für das Kennwortrückschreiben

Informationen zur Lizenzierung finden Sie unter [Erforderliche Lizenzen für das Kennwortrückschreiben](active-directory-passwords-licensing.md#licenses-required-for-password-writeback) oder auf den folgenden Websites:

* [Website mit Preisen für Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-that-are-supported-for-password-writeback"></a>Für das Kennwortrückschreiben unterstützte lokale Authentifizierungsmodi

Das Kennwortrückschreiben wird für folgende Arten von Benutzerkennwörtern unterstützt:

* **Reine Cloudbenutzer**: In diesem Szenario ist das Kennwortrückschreiben nicht relevant, da kein lokales Kennwort vorhanden ist.
* **Benutzer mit synchronisiertem Kennwort**: Kennwortrückschreiben wird unterstützt.
* **Verbundbenutzer**: Kennwortrückschreiben wird unterstützt.
* **Benutzer mit Passthrough-Authentifizierung**: Kennwortrückschreiben wird unterstützt.

### <a name="user-and-admin-operations-that-are-supported-for-password-writeback"></a>Für das Kennwortrückschreiben unterstützte Benutzer- und Administratorvorgänge

Kennwörter werden in den folgenden Situationen zurückgeschrieben:

* **Unterstützte Vorgänge für Endbenutzer**
  * Jegliche freiwillige Self-Service-Kennwortänderung durch einen Endbenutzer
  * Jegliche erzwungene Self-Service-Kennwortänderung durch einen Endbenutzer, beispielsweise der Ablauf des Kennworts
  * Jegliche Self-Service-Kennwortzurücksetzung durch einen Endbenutzer über das [Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com)
* **Unterstützte Vorgänge für Administratoren**
  * Jegliche freiwillige Self-Service-Kennwortänderung durch einen Administrator
  * Jegliche erzwungene Self-Service-Kennwortänderung durch einen Administrator, beispielsweise der Ablauf des Kennworts
  * Jegliche Self-Service-Kennwortzurücksetzung durch einen Administrator über das [Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com)
  * Jegliche durch einen Administrator initiierte Endbenutzer-Kennwortzurücksetzung über das [Azure-Portal](https://portal.azure.com)

### <a name="user-and-admin-operations-that-are-not-supported-for-password-writeback"></a>Für das Kennwortrückschreiben nicht unterstützte Benutzer- und Administratorvorgänge

Kennwörter werden in folgenden Situationen *nicht* zurückgeschrieben:

* **Nicht unterstützte Vorgänge für Endbenutzer**
  * Jegliches Zurücksetzen des eigenen Kennworts durch einen Endbenutzer über PowerShell Version 1, Version 2 oder die Azure AD-Graph-API
* **Nicht unterstützte Vorgänge für Administratoren**
  * Jegliche durch einen Administrator initiierte Endbenutzer-Kennwortzurücksetzung über das [Office-Verwaltungsportal](https://portal.office.com)
  * Jegliche durch einen Administrator initiierte Endbenutzer-Kennwortzurücksetzung mithilfe von PowerShell Version 1, Version 2 oder der Azure AD-Graph-API

Wir arbeiten daran, diese Einschränkungen zu entfernen, ein genauer Zeitplan steht jedoch noch nicht fest.

## <a name="password-writeback-security-model"></a>Sicherheitsmodell für die Kennwortrückschreibung

Das Kennwortrückschreiben ist ein äußerst sicherer Dienst. Zum Schutz Ihrer Informationen wird ein vierstufiges Sicherheitsmodell angewendet, das sich wie folgt beschreiben lässt:

* **Mandantenspezifisches Service Bus Relay**
  * Beim Einrichten des Diensts wird ein mandantenspezifisches Service Bus Relay eingerichtet, das durch ein zufällig generiertes, sicheres Kennwort geschützt wird, auf das Microsoft keinen Zugriff hat.
* **Nicht zugänglicher, sicherer Kryptografieschlüssel für die Kennwortverschlüsselung**
  * Nach der Erstellung des Service Bus Relays wird ein sicherer symmetrischer Schlüssel erstellt, mit dem das Kennwort verschlüsselt wird, bevor es gesendet wird. Dieser Schlüssel liegt ausschließlich im Speicher für geheime Schlüssel Ihres Unternehmens in der Cloud vor, der streng geschützt und überwacht wird, wie jedes andere Kennwort im Verzeichnis.
* **Transport Layer Security (TLS) nach Industriestandard**
  1. Beim Zurücksetzen oder Ändern eines Kennworts in der Cloud wird das Klartextkennwort mit Ihrem öffentlichen Schlüssel verschlüsselt.
  2. Dieses verschlüsselte Kennwort wird in eine HTTPS-Nachricht eingefügt, die über einen über SSL-Zertifikate von Microsoft verschlüsselten Kanal an Ihr Service Bus Relay gesendet wird.
  3. Nachdem die Nachricht beim Service Bus eingetroffen ist, wird Ihr lokaler Agent reaktiviert und in Service Bus mithilfe des sicheren Kennworts authentifiziert, das zuvor generiert wurde.
  4. Der lokale Agent liest die verschlüsselte Nachricht und entschlüsselt sie mit dem von uns generierten privaten Schlüssel.
  5. Der lokale Agent versucht, das Kennwort über die SetPassword-API von AD DS festzulegen. In diesem Schritt kann Ihre lokale Active Directory-Kennwortrichtlinie (so z.B. die Komplexität, das Alter, der Verlauf, die Filter usw.) in der Cloud erzwungen werden.
* **Richtlinien zum Nachrichtenablauf** 
  * Falls die Nachricht im Service Bus verbleibt, weil der lokale Dienst nicht verfügbar ist, kommt es nach wenigen Minuten zu einem Timeout, und die Nachricht wird entfernt. Das Timeout und Entfernen der Nachricht erhöhen die Sicherheit noch weiter.

### <a name="password-writeback-encryption-details"></a>Verschlüsselungsdetails für das Kennwortrückschreiben

Nachdem ein Benutzer eine Kennwortzurücksetzung übermittelt hat, durchläuft die Zurücksetzungsanforderung verschiedene Verschlüsselungsschritte, bevor sie in Ihrer lokalen Umgebung eintrifft. Diese Verschlüsselungsschritte stellen maximale Dienstzuverlässigkeit und -sicherheit sicher. Die Schritte lassen sich wie folgt beschreiben:

* **Schritt 1: Kennwortverschlüsselung mit 2048-Bit-RSA-Schlüssel**: Nachdem ein Benutzer ein zurückzuschreibendes Kennwort an die lokale Umgebung übermittelt hat, wird dieses Kennwort mit einem 2048-Bit-RSA-Schlüssel verschlüsselt.
* **Schritt 2: Verschlüsselung auf Paketebene mit AES-GCM**: Das gesamte Paket (bestehend aus Kennwort und den erforderlichen Metadaten) wird mithilfe von AES-GCM verschlüsselt. Diese Verschlüsselung verhindert, dass jemand mit direktem Zugriff auf den zugrunde liegenden ServiceBus-Kanal den Inhalt anzeigen oder manipulieren kann.
* **Schritt 3: Abwicklung der gesamten Kommunikation über TLS/SSL**: Die gesamte Kommunikation mit ServiceBus wird über einen TLS/SSL-Kanal abgewickelt. Diese Verschlüsselung schützt den Inhalt vor nicht autorisierten Dritten.
* **Automatischer Schlüsselrollover alle sechs Monate**: Alle sechs Monate oder jedes Mal, nachdem das Kennwortrückschreiben deaktiviert und anschließend in Azure AD Connect wieder aktiviert wurde. Es wird automatisch ein Rollover für alle Schlüssel ausgeführt, um maximale Sicherheit zu gewährleisten.

### <a name="password-writeback-bandwidth-usage"></a>Bandbreitennutzung für das Kennwortrückschreiben

Kennwortrückschreiben ist ein Dienst, für den wenig Bandbreite erforderlich ist und der nur unter folgenden Umständen Anforderungen zurück an den lokalen Agent sendet:

* Zwei Nachrichten werden gesendet, wenn das Feature über Azure AD Connect aktiviert oder deaktiviert wird.
* Während der gesamten Ausführungsdauer des Diensts wird als Diensttakt alle fünf Minuten eine Nachricht gesendet.
* Zwei Nachrichten werden jedes Mal gesendet, wenn ein neues Kennwort übermittelt wird:
    * Die erste Nachricht ist eine Anforderung zum Ausführen des Vorgangs.
    * Die zweite Nachricht enthält das Ergebnis des Vorgangs und wird in den folgenden Situationen gesendet:
        * Bei jeder Übermittlung eines neuen Kennworts während einer Self-Service-Kennwortzurücksetzung durch Benutzer
        * Bei jeder Übermittlung eines neuen Kennworts während einer Benutzerkennwortänderung
        * Bei jeder Übermittlung eines neuen Kennworts während einer vom Administrator initiierten Benutzerkennwortzurücksetzung (nur über die Azure-Administratorportale)

#### <a name="message-size-and-bandwidth-considerations"></a>Überlegungen zur Nachrichtengröße und Bandbreite

Die Größe von jeder der zuvor beschriebenen Nachrichten liegt in der Regel unter 1 KB. Selbst bei extrem hoher Auslastung nutzt der Dienst zum Kennwortrückschreiben höchstens eine Bandbreite von einigen wenigen Kilobits pro Sekunde. Da jede Nachricht in Echtzeit und nur dann gesendet wird, wenn dies für eine Kennwortaktualisierung erforderlich ist, und die Nachrichten ziemlich klein sind, ist die Bandbreitennutzung der Rückschreibfunktion zu klein, um messbare Auswirkungen zu haben.

## <a name="next-steps"></a>Nächste Schritte

* [Erfolgreiches Rollout der Self-Service-Kennwortzurücksetzung](active-directory-passwords-best-practices.md)
* [Ich habe mein Azure AD-Kennwort vergessen. Was nun?](active-directory-passwords-update-your-own-password.md)
* [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md)
* [Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung](active-directory-passwords-licensing.md)
* [Bereitstellen der Kennwortzurücksetzung ohne erforderliche Endbenutzerregistrierung](active-directory-passwords-data.md)
* [Authentifizierungsmethoden](active-directory-passwords-how-it-works.md#authentication-methods)
* [Welche Richtlinienoptionen stehen mit SSPR zur Verfügung?](active-directory-passwords-policy.md)
* [Wie melde ich eine Aktivität bei SSPR?](active-directory-passwords-reporting.md)
* [Welche Optionen sind für SSPR verfügbar, und was bedeuten sie?](active-directory-passwords-how-it-works.md)
* [Anscheinend ist ein Fehler aufgetreten. Wie behebe ich Probleme mit SSPR?](active-directory-passwords-troubleshoot.md)
* [Ich habe eine Frage, die nicht an einer anderen Stelle abgedeckt wurde.](active-directory-passwords-faq.md)

[Writeback]: ./media/active-directory-passwords-writeback/enablepasswordwriteback.png "Aktivieren des Kennwortrückschreibens in Azure AD Connect"
