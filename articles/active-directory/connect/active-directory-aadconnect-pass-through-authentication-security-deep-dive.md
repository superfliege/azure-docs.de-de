---
title: "Azure Active Directory-Passthrough-Authentifizierung – Ausführliche Informationen zur Sicherheit | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie die Azure Active Directory-Passthrough-Authentifizierung (Azure AD) Ihre lokalen Konten schützt."
services: active-directory
keywords: "Passthrough-Authentifizierung mit Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: billmath
ms.openlocfilehash: 7a886cdb0c36008bdb66592a8d3428889739627e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-pass-through-authentication-security-deep-dive"></a>Azure Active Directory-Passthrough-Authentifizierung – ausführliche Informationen zur Sicherheit

In diesem Artikel wird die Funktionsweise der Passthrough-Authentifizierung ausführlich beschrieben. Der Schwerpunkt liegt auf den Sicherheitsaspekten des Features. Dieses Thema ist für Sicherheits- und IT-Administratoren, Chief Compliance und Security Officers und andere IT-Profis interessant, die in kleineren und mittleren Unternehmen oder großen Konzernen für die Bereiche IT-Sicherheit und Compliance verantwortlich sind.

Folgende Themen werden behandelt:
- Ausführliche technische Informationen zur Installation und Registrierung von Authentifizierungs-Agents
- Ausführliche technische Informationen zur Verschlüsselung von Kennwörtern bei der Benutzeranmeldung
- Sicherheit der Kanäle zwischen lokalen Authentifizierungs-Agents und Azure Active Directory (Azure AD)
- Ausführliche technische Informationen zum Schutz von Authentifizierungs-Agents während des Betriebs
- Andere sicherheitsbezogene Themen

## <a name="key-security-capabilities"></a>Wichtige Sicherheitsfunktionen

Dies sind die wichtigsten Sicherheitsaspekte des Features:
- Es basiert auf einer sicheren mehrinstanzenfähigen Architektur, die eine Isolation von Anmeldeanforderungen zwischen Mandanten ermöglicht.
- Lokale Kennwörter werden niemals in irgendeiner Form in der Cloud gespeichert.
- Lokale Authentifizierungs-Agents, die auf Anforderungen zur Kennwortvalidierung lauschen und darauf antworten, stellen aus Ihrem Netzwerk nur ausgehende Verbindungen her. Es ist nicht erforderlich, diese Authentifizierungs-Agents in einem Umkreisnetzwerk (DMZ) zu installieren.
- Nur Standardports (80 und 443) werden für die ausgehende Kommunikation von den Authentifizierungs-Agents mit Azure AD verwendet. In Ihrer Firewall müssen keine eingehenden Ports geöffnet werden. 
  - Port 443 wird für die gesamte authentifizierte ausgehende Kommunikation verwendet.
  - Port 80 wird nur zum Herunterladen der Zertifikatsperrlisten (Certificate Revocation Lists, CRLs) verwendet, um sicherzustellen, dass keine vom Feature verwendeten Zertifikate widerrufen wurden.
  - Die vollständige Liste mit den Netzwerkanforderungen finden Sie [hier](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites).
- Vom Benutzer während der Anmeldung angegebene Kennwörter werden in der Cloud verschlüsselt, bevor sie von lokalen Authentifizierungs-Agents zur Validierung für Active Directory akzeptiert werden.
- Der HTTPS-Kanal zwischen Azure AD und einem lokalen Authentifizierungs-Agent wird durch eine gegenseitige Authentifizierung geschützt.
- Es kann nahtlos in die Cloudschutzfunktionen von Azure AD, z.B. Richtlinien für bedingten Zugriff (einschließlich Multi-Factor Authentication), Identity Protection und Smart Lockout, integriert werden.

## <a name="components-involved"></a>Beteiligte Komponenten

Allgemeine Informationen zur Azure AD-Sicherheit für die Bereiche Betrieb, Service und Daten finden Sie im [Trust Center](https://azure.microsoft.com/support/trust-center/). Die folgenden Komponenten sind beteiligt, wenn die Passthrough-Authentifizierung für die Benutzeranmeldung verwendet wird:
- **Azure AD STS**: Ein zustandsloser Sicherheitstokendienst (Security Token Service, STS) zum Verarbeiten von Anmeldeanforderungen und Ausstellen von Sicherheitstoken für Browser, Clients oder Dienste von Benutzern nach Bedarf.
- **Azure Service Bus**: Ermöglicht die cloudfähige Kommunikation mit Messagingfunktionen für Unternehmen und die Weiterleitung der Kommunikation, um Sie bei der Verbindungsherstellung für Ihre lokalen Lösungen mit der Cloud zu unterstützen.
- **Azure AD Connect-Authentifizierungs-Agent (kurz: Authentifizierungs-Agent)**: Eine lokale Komponente, die auf Anforderungen zur Kennwortvalidierung lauscht und darauf antwortet.
- **Azure SQL-Datenbank**: Enthält Informationen zu den Authentifizierungs-Agents Ihrer Mandanten, z.B. die Metadaten und Verschlüsselungsschlüssel.
- **Active Directory (AD)**: Ihre lokale Active Directory-Instanz, unter der Ihre Benutzerkonten (und die dazugehörigen Kennwörter) gespeichert sind.

## <a name="installation-and-registration-of-authentication-agents"></a>Installation und Registrierung von Authentifizierungs-Agents

Authentifizierungs-Agents werden installiert und unter Azure AD registriert, wenn Sie die [Passthrough-Authentifizierung mit Azure AD Connect aktivieren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-2-enable-the-feature) oder [zusätzliche Authentifizierungs-Agents hinzufügen, um die Hochverfügbarkeit von Anmeldeanforderungen sicherzustellen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-4-ensure-high-availability). Es sind drei Phasen erforderlich, um einen Authentifizierungs-Agent in Betrieb zu nehmen:

- Installation des Authentifizierungs-Agents
- Registrierung des Authentifizierungs-Agents
- Initialisierung des Authentifizierungs-Agents

Diese Phasen werden in den folgenden Abschnitten ausführlich beschrieben.

### <a name="authentication-agent-installation"></a>Installation des Authentifizierungs-Agents

Nur globale Administratoren können einen Authentifizierungs-Agent auf einem lokalen Server installieren (mit Azure AD Connect oder eigenständig). Bei der Installation werden der Liste unter **Systemsteuerung > Programme > Programme und Funktionen** diesen beiden neuen Einträge hinzugefügt:
- Die eigentliche Authentifizierungs-Agent-Anwendung. Sie wird mit der Berechtigung [Netzwerkdienst](https://msdn.microsoft.com/library/windows/desktop/ms684272.aspx) ausgeführt.
- Die Updater-Anwendung für die automatische Aktualisierung des Authentifizierungs-Agents. Sie wird mit der Berechtigung [Lokales System](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) ausgeführt.


### <a name="authentication-agent-registration"></a>Registrierung des Authentifizierungs-Agents

Nachdem Sie den Authentifizierungs-Agent installiert haben, muss er unter Azure AD registriert werden. In Azure AD wird jedem Authentifizierungs-Agent hierfür ein eindeutiges Zertifikat zur digitalen Identität zugewiesen, das für die sichere Kommunikation mit Azure AD verwendet werden kann.

Beim Registrierungsverfahren wird der Authentifizierungs-Agent auch an Ihren Mandanten gebunden, damit Azure AD weiß, dass der jeweilige Authentifizierungs-Agent der einzige autorisierte Agent ist, der die Anforderungen zur Kennwortvalidierung für Ihren Mandanten verarbeiten kann. Dieses Verfahren wird für jeden neuen Authentifizierungs-Agent wiederholt, den Sie registrieren.

Hier ist beschrieben, wie Authentifizierungs-Agents die Registrierung unter Azure AD durchführen:

![Agent-Registrierung](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta1.png)

1. Azure AD fordert zuerst einen globalen Administrator an, um die Anmeldung mit dessen Anmeldeinformationen durchzuführen. Während der Anmeldung beschafft der Authentifizierungs-Agent ein Zugriffstoken, das er im Namen des globalen Administrators nutzen kann.
2. Anschließend generiert der Authentifizierungs-Agent ein Schlüsselpaar: einen öffentlichen Schlüssel und einen privaten Schlüssel.
    - Dieses Schlüsselpaar wird mithilfe der Standardverschlüsselung **RSA 2048 Bit** generiert.
    - Der private Schlüssel verlässt niemals den lokalen Server, auf dem der Authentifizierungs-Agent installiert wurde.
3.  Der Authentifizierungs-Agent sendet per HTTPS eine „registration“-Anforderung an Azure AD, die die folgenden Komponenten enthält:
    - Das in Schritt 1 beschaffte Zugriffstoken.
    - Den in Schritt 2 generierten öffentlichen Schlüssel.
    - Eine **Zertifikatsignieranforderung** (Certificate Signing Request, CSR). Sie dient zum Durchführen der Bewerbung für ein Zertifikat zur digitalen Identität mit Azure AD als Zertifizierungsstelle.
4. Azure AD überprüft das Zugriffstoken in der Registrierungsanforderung und stellt sicher, dass die Anforderung von einem globalen Administrator stammt.
5. Azure AD signiert dann ein Zertifikat zur digitalen Identität und sendet es zurück an den Authentifizierungs-Agent.
    - Das Zertifikat wird über die **Stammzertifizierungsstelle (CA) von Azure AD** signiert. Beachten Sie, dass diese Zertifizierungsstelle _nicht_ im Store mit den **vertrauenswürdigen Stammzertifizierungsstellen**  von Windows enthalten ist.
    - Der Antragsteller des Zertifikats (**Distinguished Name, DN**) ist auf Ihre **Mandanten-ID** festgelegt. Dies ist eine GUID, mit der Ihr Mandant eindeutig identifiziert wird. Die Nutzung des Zertifikats ist somit nur auf Ihren Mandanten beschränkt.
6. Azure AD speichert den öffentlichen Schlüssel des Authentifizierungs-Agents in einer Azure SQL-Datenbank, auf die nur Azure AD Zugriff hat.
7. Das Zertifikat (ausgestellt in Schritt 5) wird auf dem lokalen Server im **Windows-Zertifikatspeicher** gespeichert (Speicherort: **[CERT_SYSTEM_STORE_LOCAL_MACHINE](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_LOCAL_MACHINE)**) und sowohl vom Authentifizierungs-Agent als auch von der Updater-Anwendung verwendet.

### <a name="authentication-agent-initialization"></a>Initialisierung des Authentifizierungs-Agents

Wenn der Authentifizierungs-Agent gestartet wird – entweder zum ersten Mal nach der Registrierung oder nach einem Serverneustart –, muss eine Möglichkeit zur sicheren Kommunikation mit dem Azure AD-Dienst bestehen, und Anforderungen zur Kennwortvalidierung müssen akzeptiert werden.

![Agent-Initialisierung](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta2.png)

Authentifizierungs-Agents werden wie folgt initialisiert:



1. Der Authentifizierungs-Agent wird gestartet, indem eine ausgehende „Bootstrap“-Anforderung an Azure AD gesendet wird. 
    - Diese Bootstrap-Anforderung erfolgt über Port 443 und einen gegenseitig authentifizierten HTTPS-Kanal (mit demselben Zertifikat, das bei der Registrierung des Authentifizierungs-Agents ausgestellt wurde).
2. Azure AD reagiert auf diese Bootstrap-Anforderung, indem ein **Zugriffsschlüssel** für eine Azure Service Bus-Warteschlange bereitgestellt wird, die für Ihren Mandanten eindeutig ist (per Mandanten-ID identifiziert).
3. Der Authentifizierungs-Agent stellt eine dauerhafte ausgehende HTTPS-Verbindung (über Port 443) mit der Warteschlange her. 
    - Nun kann er Anforderungen zur Kennwortvalidierung abrufen und verarbeiten.

Wenn Sie unter Ihrem Mandanten mehrere Authentifizierungs-Agents registriert haben, wird anhand des Initialisierungsverfahrens sichergestellt, dass dafür jeweils eine Verbindung mit derselben Azure Service Bus-Warteschlange hergestellt wird. 

## <a name="processing-sign-in-requests"></a>Verarbeiten von Anmeldeanforderungen 

Im folgenden Diagramm ist dargestellt, wie Anforderungen zur Benutzeranmeldung bei der Passthrough-Authentifizierung verarbeitet werden.

![Verarbeiten der Anmeldung](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta3.png)

Bei der Passthrough-Authentifizierung wird eine Anforderung zur Benutzeranmeldung wie folgt verarbeitet: 

1. Der Benutzer versucht, auf eine Anwendung zuzugreifen (z.B. Outlook Web App unter [https://outlook.office365.com/owa](https://outlook.office365.com/owa)).
2. Wenn der Benutzer nicht bereits angemeldet ist, leitet die Anwendung den Browser auf die Anmeldeseite von Azure AD um.
3. Der Azure AD-Sicherheitstokendienst (STS) reagiert mit der Seite für die Benutzeranmeldung.
4. Der Benutzer gibt auf der Seite „Azure AD-Anmeldung“ seinen Benutzernamen und sein Kennwort ein und klickt anschließend auf die Schaltfläche „Anmelden“.
5. Der Benutzername und das Kennwort werden in einer HTTPS POST-Anforderung an den Azure AD STS gesendet.
6. Der Azure AD STS ruft öffentliche Schlüssel für alle Authentifizierungs-Agents, die unter Ihrem Mandanten registriert sind, aus der Azure SQL-Datenbank ab und verschlüsselt damit die Kennwörter. 
    - Es werden „N“ verschlüsselte Kennwortwerte für „N“ Authentifizierungs-Agents erstellt, die unter Ihrem Mandanten registriert sind.
7. Der Azure AD STS reiht die Anforderung zur Kennwortvalidierung (Benutzername und verschlüsselte Kennwortwerte) in die Azure Service Bus-Warteschlange für Ihren Mandanten ein.
8. Da die initialisierten Authentifizierungs-Agents über eine dauerhafte Verbindung mit der Azure Service Bus-Warteschlange verfügen, ruft einer der verfügbaren Authentifizierungs-Agents die Anforderung zur Kennwortvalidierung ab.
9. Der Authentifizierungs-Agent ermittelt den verschlüsselten Kennwortwert für den dazugehörigen öffentlichen Schlüssel (per Bezeichner) und entschlüsselt ihn mit dem entsprechenden privaten Schlüssel.
10. Der Authentifizierungs-Agent versucht, den Benutzernamen und das Kennwort für Ihre lokale Active Directory-Instanz zu validieren, indem die **[Win32 LogonUser-API](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx)** verwendet wird (mit Festlegung des Parameters **dwLogonType** auf **LOGON32_LOGON_NETWORK**). 
    - Dies ist dieselbe API, die von Active Directory-Verbunddienste (AD FS) in einem Szenario mit Verbundanmeldung zum Anmelden genutzt wird.
11. Der Authentifizierungs-Agent erhält das Ergebnis von Active Directory (Erfolg, Benutzername oder Kennwort fehlerhaft, Kennwort abgelaufen, Benutzer gesperrt usw.).
12. Der Authentifizierungs-Agent leitet das Ergebnis über einen ausgehenden gegenseitig authentifizierten HTTPS-Kanal (Port 443) zurück an den Azure AD STS. Bei der gegenseitigen Authentifizierung wird dasselbe Zertifikat verwendet, das zuvor während der Registrierung für den Authentifizierungs-Agent ausgestellt wurde.
13. Der Azure AD STS überprüft, ob das Ergebnis mit der jeweiligen Anmeldeanforderung auf Ihrem Mandanten korreliert.
14. Der Azure AD STS fährt gemäß Konfiguration mit dem Anmeldeverfahren fort. Wenn die Kennwortvalidierung beispielsweise erfolgreich war, kann der Benutzer zum Durchführen der Multi-Factor Authentication aufgefordert oder zurück an die Anwendung geleitet werden.

## <a name="operational-security-of-authentication-agents"></a>Betriebssicherheit von Authentifizierungs-Agents

Um sicherzustellen, dass der Betrieb der Passthrough-Authentifizierung geschützt bleibt, erneuert Azure AD die Zertifikate in regelmäßigen Abständen. Diese Erneuerungen von Azure AD werden nicht von den Authentifizierungs-Agents selbst gesteuert.

![Betriebssicherheit](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta4.png)

Ein Authentifizierungs-Agent erneuert die Vertrauensstellung mit Azure AD wie folgt:

1. Der Authentifizierungs-Agent pingt Azure AD in regelmäßigen Abständen (nach einigen Stunden), um zu überprüfen, ob der Zeitpunkt für die Erneuerung des Zertifikats gekommen ist. 
    - Diese Überprüfung wird über einen HTTPS-Kanal mit gegenseitiger Authentifizierung durchgeführt (mit demselben Zertifikat, das während der Registrierung ausgestellt wurde).
2. Wenn der Dienst angibt, dass es Zeit für die Erneuerung ist, generiert der Authentifizierungs-Agent ein neues Schlüsselpaar: einen öffentlichen Schlüssel und einen privaten Schlüssel.
    - Diese Schlüssel werden mit der Standardverschlüsselung **RSA 2048 Bit** generiert.
    - Der private Schlüssel verlässt den lokalen Server niemals.
3. Der Authentifizierungs-Agent sendet per HTTPS dann eine „certificate renewal“-Anforderung an Azure AD, die die folgenden Komponenten enthält:
    - Das vorhandene Zertifikat (abgerufen vom Speicherort **CERT_SYSTEM_STORE_LOCAL_MACHINE** im Windows-Zertifikatspeicher). An diesem Verfahren ist kein globaler Administrator beteiligt, sodass kein Zugriffstoken für den globalen Administrator erforderlich ist.
    - Den in Schritt 2 generierten öffentlichen Schlüssel.
    - Eine **Zertifikatsignieranforderung** (Certificate Signing Request, CSR). Sie dient zum Durchführen der Bewerbung für ein neues Zertifikat zur digitalen Identität mit Azure AD als Zertifizierungsstelle.
4. Azure AD überprüft das vorhandene Zertifikat in der Anforderung für die Zertifikaterneuerung. Anschließend wird überprüft, ob die Anforderung von einem Authentifizierungs-Agent stammt, der unter Ihrem Mandanten registriert ist.
5. Wenn das vorhandene Zertifikat noch gültig ist, signiert Azure AD ein neues Zertifikat zur digitalen Identität und sendet das neue Zertifikat zurück an den Authentifizierungs-Agent. 
6. Falls das vorhandene Zertifikat abgelaufen ist, löscht Azure AD den Authentifizierungs-Agent aus der Liste mit den registrierten Authentifizierungs-Agents Ihres Mandanten. Anschließend muss ein globaler Administrator einen neuen Authentifizierungs-Agent manuell installieren und registrieren.
    - Das Zertifikat wird über die **Stammzertifizierungsstelle (CA) von Azure AD** signiert.
    - Der Antragsteller des Zertifikats (**Distinguished Name, DN**) wird auf Ihre **Mandanten-ID** festgelegt. Dies ist eine GUID, mit der Ihr Mandant eindeutig identifiziert wird. Das Zertifikat gilt also nur für Ihren Mandanten.
6. Azure AD speichert den „neuen“ öffentlichen Schlüssel des Authentifizierungs-Agents in einer Azure SQL-Datenbank, auf die nur Azure AD selbst Zugriff hat. Der „alte“ öffentliche Schlüssel, der dem Authentifizierungs-Agent zugeordnet ist, wird für ungültig erklärt.
7. Anschließend wird das neue Zertifikat (in Schritt 5 ausgestellt) auf dem Server im **Windows-Zertifikatspeicher** gespeichert (Speicherort: **[CERT_SYSTEM_STORE_CURRENT_USER](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_CURRENT_USER)**).
    - Da das Verfahren zur Erneuerung der Vertrauensstellung nicht interaktiv durchgeführt wird (ohne Vorhandensein eines globalen Administrators), verfügt der Authentifizierungs-Agent nicht mehr über Zugriff zum Aktualisieren des vorhandenen Zertifikats am Speicherort **CERT_SYSTEM_STORE_LOCAL_MACHINE**. Beachten Sie, dass das Zertifikat selbst am Speicherort **CERT_SYSTEM_STORE_LOCAL_MACHINE** während dieses Verfahrens nicht entfernt wird.
8. Das neue Zertifikat wird von jetzt an für die Authentifizierung verwendet. Bei jeder nachfolgenden Erneuerung des Zertifikats wird das Zertifikat am Speicherort **CERT_SYSTEM_STORE_LOCAL_MACHINE** ersetzt.

## <a name="auto-update-of-authentication-agents"></a>Automatische Aktualisierung von Authentifizierungs-Agents

Mit der Updater-Anwendung wird der Authentifizierungs-Agent automatisch aktualisiert, wenn eine neue Version veröffentlicht wird. Es werden damit aber keine Anforderungen zur Kennwortvalidierung für Ihren Mandanten verarbeitet. 

Azure AD hostet die neue Version der Software als signiertes **Windows Installer-Paket (MSI)**. Das MSI-Paket wird per [Microsoft Authenticode](https://msdn.microsoft.com/library/ms537359.aspx) mit **SHA256** als Digest-Algorithmus signiert. 

![Automatische Aktualisierung](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta5.png)

Ein Authentifizierungs-Agent wird wie folgt automatisch aktualisiert:

1. Der Updater pingt Azure AD in regelmäßigen Abständen (einmal pro Stunde), um zu überprüfen, ob eine neue Version des Authentifizierungs-Agent verfügbar ist. 
    - Diese Überprüfung wird über einen HTTPS-Kanal mit gegenseitiger Authentifizierung durchgeführt (mit demselben Zertifikat, das während der Registrierung ausgestellt wurde). Der Authentifizierungs-Agent und der Updater nutzen das auf dem Server gespeicherte Zertifikat gemeinsam.
2. Wenn eine neue Version verfügbar ist, gibt Azure AD das signierte MSI-Paket zurück an den Updater.
3. Der Updater überprüft, ob das MSI-Paket von Microsoft signiert ist.
4. Der Updater führt das MSI-Paket aus. Bei dieser Aktion werden die folgenden Schritte ausgeführt (Hinweis: Der Updater wird mit der Berechtigung [Lokales System](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) ausgeführt):
    - Der Authentifizierungs-Agent-Dienst wird beendet.
    - Die neue Version des Authentifizierungs-Agent wird auf dem Server installiert.
    - Der Authentifizierungs-Agent-Dienst wird neu gestartet.

>[!NOTE]
>Wenn Sie unter Ihrem Mandanten mehrere Authentifizierungs-Agents registriert haben, erneuert Azure AD die dazugehörigen Zertifikate nicht gleichzeitig und führt auch keine Aktualisierung dafür durch. Stattdessen wird dies von Azure AD nach und nach durchgeführt, um die Hochverfügbarkeit der Anmeldeanforderungen sicherzustellen.


## <a name="next-steps"></a>Nächste Schritte
- [**Aktuelle Einschränkungen**](active-directory-aadconnect-pass-through-authentication-current-limitations.md): Informationen zu den unterstützten und nicht unterstützten Szenarien
- [**Schnellstart**](active-directory-aadconnect-pass-through-authentication-quick-start.md): Einrichten und Ausführen der Passthrough-Authentifizierung mit Azure AD
- [**Smart Lockout**](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): Konfigurieren der Smart Lockout-Funktion für Ihren Mandanten zum Schutz der Benutzerkonten
- [**Funktionsweise**](active-directory-aadconnect-pass-through-authentication-how-it-works.md): Grundlegende Funktionsweise der Passthrough-Authentifizierung mit Azure AD
- [**Häufig gestellte Fragen:**](active-directory-aadconnect-pass-through-authentication-faq.md) Antworten auf häufig gestellte Fragen
- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Beheben häufig auftretender Probleme mit dieser Funktion
- [**Nahtlose SSO mit Azure AD**](active-directory-aadconnect-sso.md): Informationen zu dieser Ergänzungsfunktion

