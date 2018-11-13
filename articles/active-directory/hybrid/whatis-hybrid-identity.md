---
title: Herstellen einer Verbindung zwischen Active Directory und Azure Active Directory | Microsoft-Dokumentation
description: Azure AD Connect integriert Ihre lokalen Verzeichnisse in Azure Active Directory. Dadurch können Sie eine einzige Identität für in Azure AD integrierte in Office 365-, Azure- und SaaS-Anwendungen bereitstellen.
keywords: Einführung in Azure AD Connect, Übersicht über Azure AD Connect, was ist Azure AD Connect, Active Directory installieren
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/02/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2aca42c23cc213d5d7e451105052d5d5d697b77d
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979470"
---
# <a name="hybrid-identity-and-microsoft-identity-solutions"></a>Hybrididentität und Identitätslösungen von Microsoft
Hybrid-Identitätslösungen mit [Microsoft Azure Active Directory (Azure AD)](../../active-directory/fundamentals/active-directory-whatis.md) ermöglichen es Ihnen, lokale Verzeichnisobjekte mit Azure AD zu synchronisieren, während Sie Ihre Benutzer weiterhin lokal verwalten. Die erste Entscheidung bei der Planung der Synchronisierung Ihres lokalen Windows Server Active Directory mit Azure AD ist, ob Sie verwaltete Identitäten oder einen Identitätsverbund verwenden möchten. 

- **Verwaltete Identitäten:** Benutzerkonten und Gruppen werden über eine lokale Active Directory-Instanz synchronisiert, und die Benutzerauthentifizierung wird von Azure verwaltet.   
- **Verbundidentitäten** ermöglichen eine bessere Kontrolle der Benutzer, indem die Benutzerauthentifizierung aus Azure herausgetrennt und an einen vertrauenswürdigen, lokalen Identitätsanbieter delegiert wird. 

Es gibt mehrere Optionen für das Konfigurieren von Hybrididentitäten. Wenn Sie abwägen, welches Identitätsmodell den Anforderungen Ihrer Organisation am besten entspricht, müssen Sie auch Zeit, vorhandene Infrastruktur, Komplexität und Kosten berücksichtigen. Diese Faktoren unterscheiden sich für jede Organisation und möglicherweise auch über die Zeit. Wenn sich Ihre Anforderungen ändern, können Sie auch flexibel zu einem anderen Identitätsmodell wechseln.

## <a name="managed-identity"></a>Verwaltete Identität 

Eine verwaltete Identität ist die einfachste Möglichkeit, lokale Verzeichnisobjekte (Benutzer und Gruppen) in Azure AD zu synchronisieren. 

![Synchronisierte Hybrididentität](./media/whatis-hybrid-identity/managed.png)

Eine verwaltete Identität ist zwar die schnellste und einfachste Methode, Ihre Benutzer müssen jedoch weiterhin ein separates Kennwort für cloudbasierte Ressourcen verwalten. Um dies zu vermeiden, können Sie auch (optional) [einen Hash von Benutzerkennwörtern](how-to-connect-password-hash-synchronization.md) mit Ihrem Azure AD-Verzeichnis synchronisieren. Durch das Synchronisieren von Kennworthashes können Benutzer sich bei cloudbasierten Unternehmensressourcen mit demselben Benutzernamen und Kennwort anmelden, die sie lokal verwenden. Azure AD Connect überprüft in regelmäßigen Abständen Ihr lokales Verzeichnis auf Änderungen und hält Ihr Azure AD-Verzeichnis synchronisiert. Wenn ein Benutzerattribut oder Kennwort im lokalen Active Directory geändert wird, erfolgt auch eine automatische Aktualisierung in Azure AD. 

Für die meisten Organisationen, die lediglich die Benutzeranmeldung für Office 365, SaaS-Anwendungen und andere Azure AD-basierte Ressourcen aktivieren müssen, empfiehlt sich die Standardoption für die Kennworthashsynchronisierung. Wenn dies für Sie nicht funktioniert, müssen Sie sich zwischen Pass-Through-Authentifizierung und AD FS entscheiden.

> [!TIP]
> Benutzerkennwörter werden in einer lokalen Instanz von Windows Server Active Directory in Form eines Hashwerts gespeichert, der das eigentliche Benutzerkennwort darstellt. Ein Hashwert ist das Ergebnis einer unidirektionalen mathematischen Funktion (des Hashalgorithmus). Es ist nicht möglich, das Ergebnis einer unidirektionalen Funktion in die Nur-Text-Version eines Kennworts umzukehren. Sie können einen Kennworthash nicht zur Anmeldung in Ihrem lokalen Netzwerk verwenden. Wenn Sie sich dafür entscheiden, Kennwörter zu synchronisieren, extrahiert Azure AD Connect Kennworthashes aus der lokalen Active Directory-Instanz und führt eine zusätzliche Sicherheitsverarbeitung für den Kennworthash durch, bevor er mit Azure AD synchronisiert wird. Die Kennworthashsynchronisierung kann auch mit dem Kennwortrückschreiben kombiniert werden, um es Benutzern zu ermöglichen, ihre Kennwörter in Azure AD selbst zurückzusetzen. Darüber hinaus können Sie einmaliges Anmelden (SSO) für Benutzer auf in die Domäne eingebundenen Computern aktivieren, die mit dem Unternehmensnetzwerk verbunden sind. Beim einmaligen Anmelden müssen aktivierte Benutzer nur einen Benutzernamen eingeben, um sicher auf Cloudressourcen zuzugreifen. 
>

## <a name="pass-through-authentication"></a>Passthrough-Authentifizierung

Die [Azure AD-Pass-Through-Authentifizierung](how-to-connect-pta.md) bietet eine einfache Kennwortüberprüfungslösung für auf Azure AD basierende Dienste mit Ihrer lokalen Active Directory-Instanz. Wenn die Sicherheits- und Compliancerichtlinien für Ihre Organisation das Senden von Benutzerkennwörtern selbst in Hashform nicht zulassen und Sie nur Desktop-SSO für in die Domäne eingebundene Geräte unterstützen müssen, sollten Sie die Pass-Through-Authentifizierung in Erwägung ziehen. Die Pass-Through-Authentifizierung erfordert keine Bereitstellung in der DMZ, sodass die Bereitstellungsinfrastruktur im Vergleich zu AD FS vereinfacht ist. Wenn Benutzer sich mithilfe von Azure AD anmelden, überprüft diese Authentifizierungsmethode die Benutzerkennwörter direkt anhand Ihrer lokalen Active Directory-Instanz.

![Passthrough-Authentifizierung](./media/whatis-hybrid-identity/pass-through-authentication.png)

Mit der Pass-Through-Authentifizierung wird keine komplexe Netzwerkinfrastruktur benötigt, und Sie müssen keine lokalen Kennwörter in der Cloud speichern. In Kombination mit einmaligem Anmelden bietet die Pass-Through-Authentifizierung eine wahrhaft integrierte Lösung für die Anmeldung bei Azure AD oder anderen Clouddiensten.

Die Pass-Through-Authentifizierung kann mit Azure AD Connect konfiguriert werden. Dabei wird über einen einfachen lokalen Agent auf Anforderungen nach Kennwortüberprüfung gelauscht. Der Agent lässt sich problemlos auf mehreren Computern bereitstellen, um für Hochverfügbarkeit und Lastenausgleich zu sorgen. Da die gesamte Kommunikation ausgehend ist, muss der Connector nicht in einer DMZ installiert werden. Folgende Servercomputeranforderungen gelten für den Connector:

- Windows Server 2012 R2 oder höher
- Mitglied einer Domäne in der Gesamtstruktur, über die Benutzer überprüft werden

## <a name="federated-identity-ad-fs"></a>Identitätsverbund (AD FS)

Zur besseren Steuerung des Zugriffs auf Office 365 und andere Clouddienste durch Benutzer können Sie die Verzeichnissynchronisierung mit einmaligem Anmelden (SSO) über [Active Directory-Verbunddienste (AD FS)](how-to-connect-fed-whatis.md) einrichten. Durch den Identitätsverbund der Anmeldevorgänge Ihrer Benutzer mit AD FS wird die Authentifizierung an einen lokalen Server delegiert, der die Anmeldeinformationen des Benutzers überprüft. In diesem Modell werden lokale Active Directory-Anmeldeinformationen niemals an Azure AD übergeben.

![Identitätsverbund](./media/whatis-hybrid-identity/federated-identity.png)

Diese auch als Verbundidentität bezeichnete Anmeldemethode gewährleistet, dass die Benutzerauthentifizierung ausschließlich lokal gesteuert wird, und sie ermöglicht es Administratoren, striktere Ebenen der Zugriffssteuerung zu implementieren. Ein Identitätsverbund mit AD FS ist die komplizierteste Option, da hierbei zusätzliche Server in Ihrer lokalen Umgebung bereitgestellt werden müssen. Beim Identitätsverbund müssen Sie außerdem rund um die Uhr Support für die Active Directory- und AD FS-Infrastruktur anbieten. Dieses hohe Maß an Support ist erforderlich, da sich Benutzer bei Ausfällen Ihres lokalen Internetzugriffs, Ihrer Domänencontroller oder Ihrer AD FS-Server nicht bei Clouddiensten anmelden können.

> [!TIP]
> Wenn Sie sich für einen Verbund mit Active Directory Federation Services (AD FS) entscheiden, können Sie optional die Kennworthashsynchronisierung als zusätzliche Sicherheitsmaßnahme für den Fall aktivieren, dass Ihre AD FS-Infrastruktur ausfällt.
>

## <a name="common-scenarios-and-recommendations"></a>Gängige Szenarien und Empfehlungen

Im Folgenden sind einige verbreitete Hybrididentitäts- und Zugriffsverwaltungsszenarien mit Empfehlungen zur Auswahl der jeweils geeigneten Hybrididentitätsoptionen aufgeführt.

|Ziel|PHS und SSO<sup>1</sup>| PTA und SSO<sup>2</sup> | AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Automatisches Synchronisieren neuer Benutzer-, Kontakt- und Gruppenkonten, die in meiner lokalen Active Directory-Instanz erstellt werden, mit der Cloud|![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)| ![Empfohlen](./media/whatis-hybrid-identity/ic195031.png) |![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)|
|Einrichten meines Mandanten für Office 365-Hybridszenarien|![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)| ![Empfohlen](./media/whatis-hybrid-identity/ic195031.png) |![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)|
|Ermöglichen der Anmeldung und des Zugriffs auf Clouddienste für meine Benutzer mit ihrem lokalen Kennwort|![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)| ![Empfohlen](./media/whatis-hybrid-identity/ic195031.png) |![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)|
|Implementieren des einmaligen Anmeldens mit Anmeldeinformationen des Unternehmens|![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)| ![Empfohlen](./media/whatis-hybrid-identity/ic195031.png) |![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)|
|Sicherstellen, dass keine Kennworthashes in der Cloud gespeichert werden| |![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)|![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)|
|Aktivieren der Multi-Factor Authentication-Cloudlösungen| |![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)|![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)|
|Aktivieren der lokalen Multi-Factor Authentication-Lösungen| | |![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)|
|Unterstützung von Smartcard-Authentifizierung für meine Benutzer<sup>4</sup>| | |![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)|
|Anzeigen von Benachrichtigungen zum Kennwortablauf im Office-Portal und auf dem Windows 10-Desktop| | |![Empfohlen](./media/whatis-hybrid-identity/ic195031.png)|

> <sup>1</sup> Kennworthashsynchronisierung mit einmaligem Anmelden
>
> <sup>2</sup> Pass-Through-Authentifizierung und einmaliges Anmelden 
>
> <sup>3</sup> Einmalige Verbundanmeldung mit AD FS
>
> <sup>4</sup> AD FS kann in Ihre Unternehmens-PKI integriert werden, damit die Anmeldung mithilfe von Zertifikaten möglich ist. Bei diesen Zertifikaten kann es sich um Softzertifikate handeln, die über vertrauenswürdige Bereitstellungskanäle bereitgestellt werden, etwa MDM-, GPO- oder Smartcard-Zertifikate (einschließlich PIV/CAC-Karten) oder Hello for Business (cert-trust). Weitere Informationen zur Unterstützung der Smartcard-Authentifizierung finden Sie in [diesem Blog](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/).
>

## <a name="what-is-azure-ad-connect"></a>Was ist Azure AD Connect?

Das Microsoft-Tool Azure AD Connect wurde entwickelt, um Sie beim Erreichen Ihrer Hybrididentitätsziele zu unterstützen.  Dadurch können Sie für Ihre Benutzer eine einzige Identität für in Azure AD integrierte in Office 365-, Azure- und SaaS-Anwendungen bereitstellen.  Er zeichnet sich durch Folgendes aus:
    
- [Synchronisierung](how-to-connect-sync-whatis.md): Diese Komponente ist verantwortlich für das Erstellen von Benutzern, Gruppen und anderen Objekten. Er stellt ebenfalls sicher, dass Identitätsinformationen für Ihre lokalen Benutzer und Gruppen denen in der Cloud entsprechen.  Sie ist auch für das Synchronisieren von Kennworthashes mit Azure AD verantwortlich.
- [Kennworthashsynchronisierung](how-to-connect-password-hash-synchronization.md): Eine optionale Komponente, mit der Benutzer lokal und in der Cloud dasselbe Kennwort verwenden können, indem ein Hash des Benutzerkennworts mit Azure AD synchronisiert wird.
-   [AD FS und Verbundintegration](how-to-connect-fed-whatis.md): Der Verbund ist eine optionale Komponente von Azure AD Connect und kann zum Konfigurieren einer Hybridumgebung mithilfe einer lokalen AD FS-Infrastruktur verwendet werden. Er bietet auch AD FS-Verwaltungsfunktionen wie die Zertifikaterneuerung und zusätzliche AD FS-Serverbereitstellungen.
-   [Passthrough-Authentifizierung](how-to-connect-pta.md): Dies ist eine weitere optionale Komponente, die Benutzern die Verwendung des gleichen Kennworts lokal und in der Cloud ermöglicht, die zusätzliche Infrastruktur einer Verbundumgebung jedoch nicht erfordert.
-   [PingFederate und Verbundintegration](how-to-connect-install-custom.md#configuring-federation-with-pingfederate): Eine weitere Verbundoption, mit der Sie PingFederate als Identitätsanbieter nutzen können.
-   [Systemüberwachung](whatis-hybrid-identity-health.md): Azure AD Connect Health bietet eine stabile Überwachung und einen zentralen Speicherort im Azure-Portal, um diese Aktivität anzuzeigen. 


![Was ist Azure AD Connect?](./media/whatis-hybrid-identity/arch.png)



## <a name="what-is-azure-ad-connect-health"></a>Was ist Azure AD Connect Health?

Azure Active Directory (Azure AD) Connect Health unterstützt Sie bei der Überwachung Ihrer lokalen Identitätsinfrastruktur und Synchronisierungsdienste und vermittelt Ihnen wichtige Einblicke. Sie können für eine zuverlässige Verbindung mit Office 365 und Microsoft Online Services sorgen, indem Sie Überwachungsfunktionen für Ihre wichtigen Identitätskomponenten bereitstellen, z.B. Active Directory Federation Services (AD FS), Azure AD Connect-Server (Synchronisierungsmodul), Active Directory-Domänencontroller usw. Außerdem sind die wichtigen Datenpunkte dieser Komponenten hierbei leicht zugänglich, sodass Sie Informationen zur Nutzung und andere wichtige Einblicke erhalten und fundierte Entscheidungen treffen können.

Die Informationen werden im [Azure AD Connect Health-Portal](https://aka.ms/aadconnecthealth)angezeigt. Im Azure AD Connect Health-Portal können Sie Warnungen, Leistungsüberwachungsdaten, Nutzungsanalysen und andere Informationen anzeigen. Azure AD Connect Health ermöglicht Ihnen an einem zentralen Ort einen Gesamtüberblick über die Integrität Ihrer wichtigsten Identitätskomponenten.

![Was ist Azure AD Connect Health?](./media/whatis-hybrid-identity-health/aadconnecthealth2.png)


Azure AD Connect Health umfasst immer mehr Funktionen und ermöglicht Ihnen an einem zentralen Dashboard einen Gesamtüberblick über Ihre Identitätskomponenten. Sie sorgen für eine stabilere, fehlerfreie und besser integrierte Umgebung, damit Ihre Benutzer ihre Aufgaben schneller erledigen können.


## <a name="why-use-azure-ad-connect"></a>Gründe für die Verwendung von Azure AD Connect
Die Integration Ihrer lokalen Verzeichnisse in Azure AD steigert die Produktivität Ihrer Benutzer, da für den Zugriff auf die Cloud und lokale Ressourcen nur eine Identität benötigt wird. Benutzer und Organisationen können die folgenden Vorteile nutzen:

* Benutzer können eine einzelne Identität verwenden, um auf lokale Anwendungen und Clouddienste wie z. B. Office 365 zuzugreifen.
* Einzelnes Tool zum Bereitstellen einer einfachen Bereitstellungserfahrung für die Synchronisierung und Anmeldung.
* Bietet die neuesten Funktionen für Ihre Szenarien. Azure AD Connect ersetzt ältere Versionen von Identitätsintegrationstools, wie z. B. DirSync und Azure AD Sync. Weitere Informationen finden Sie unter [Vergleich von Tools für die Verzeichnisintegration für Hybrididentitäten](plan-hybrid-identity-design-considerations-tools-comparison.md).

## <a name="why-use-azure-ad-connect-health"></a>Gründe für die Verwendung von Azure AD Connect Health
Beim Integrieren Ihrer lokalen Verzeichnisse in Azure AD steigern Sie die Produktivität Ihrer Benutzer, da für den Zugriff auf die Cloud und lokale Ressourcen nur eine Identität benötigt wird. Allerdings erfordert diese Integration, dass diese Umgebung fehlerfrei bleibt, sodass Benutzer von jedem Gerät verlässlich auf lokale ebenso wie auf Cloudressourcen zugreifen können. Azure AD Connect Health unterstützt Sie beim Überwachen und Gewinnen von Einblicken in Ihre lokale Identitätsverwaltungsinfrastruktur, die für den Zugriff auf Office 365 oder andere Azure AD-Anwendungen genutzt wird. Dieser Ansatz ist ähnlich unkompliziert wie das Installieren eines Agents auf Ihren lokalen Identitätsservern.

### <a name="azure-ad-connect-health-for-ad-fshow-to-connect-health-adfsmd"></a>[Azure AD Connect Health für AD FS](how-to-connect-health-adfs.md)
Azure AD Connect Health für AD FS unterstützt AD FS 2.0 unter Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 und Windows Server 2016. Außerdem wird die Überwachung der AD FS-Proxy- oder Webanwendungsproxy-Server zur Authentifizierung für den Extranetzugriff unterstützt. Mit einer einfachen und schnellen Installation des Integritäts-Agents bietet Ihnen Azure AD Connect Health für AD FS eine Reihe von wichtigen Funktionen.

#### <a name="key-benefits-and-best-practices"></a>Hauptvorteile und bewährte Methoden

- *Erweiterte Sicherheit*
  - [Extranetsperrtrends](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)
  - [Bericht zu Anmeldefehlern](how-to-connect-health-adfs.md#risky-ip-report-public-preview) 
  - [Schutz der Privatsphäre](reference-connect-health-user-privacy.md)    
- *Benachrichtigung über alle[kritischen ADFS-Systemprobleme](how-to-connect-health-alert-catalog.md#alerts-for-active-directory-federation-services)*
    - Serverkonfiguration und -verfügbarkeit 
    - [Leistung und Konnektivität](how-to-connect-health-adfs.md#performance-monitoring-for-ad-fs) 
  - Regelmäßige Wartung    
- *Einfache Bereitstellung und Verwaltung*
  - Schnelle [Agent-Installation](how-to-connect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs) 
  - Automatisches Agent-Upgrade auf aktuelle Version 
  - Verfügbarkeit von Daten im Portal innerhalb weniger Minuten    
- *Umfangreiche [Nutzungsmetriken](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)* 
  - Nutzung der besten Anwendungen
  - Netzwerkadressen und TCP-Verbindung
  - Tokenanforderungen pro Server    
- *Große Benutzerfreundlichkeit* 
  - Dashboard wie im Azure-Portal
  - [Benachrichtigungen per E-Mail](how-to-connect-health-adfs.md#alerts-for-ad-fs)    

#### <a name="feature-highlight"></a>Feature-Highlight

*   Überwachung mit Warnungen als Information, wenn die Integrität von AD FS und der AD FS-Proxyserver nicht gewährleistet ist
*   E-Mail-Benachrichtigungen für kritische Warnungen
*   Trends in Leistungsdaten, nützlich für die Kapazitätsplanung von AD FS
*   Nutzungsanalysen für AD FS-Anmeldungen mit Pivotierung (Apps, Benutzer, Netzwerkadressen usw.)
*   Berichte für AD FS, z. B. Top-50-Benutzer mit fehlerhaften Anmeldeversuchen per Benutzername/Kennwort über die letzte IP-Adresse
*   Bericht über riskante IP-Adressen für AD FS-Anmeldefehler

Weitere Informationen finden Sie unter [Verwenden von Azure AD Connect Health mit AD FS](how-to-connect-health-adfs.md).

### <a name="azure-ad-connect-health-for-synchow-to-connect-health-syncmd"></a>[Azure AD Connect Health für die Synchronisierung](how-to-connect-health-sync.md)
Azure AD Connect Health für die Synchronisierung überwacht die Synchronisierungen zwischen Ihrem lokalen Active Directory und Azure AD. Azure AD Connect Health für die Synchronisierung stellt die folgenden wichtigen Funktionen bereit:

* Überwachung mit Warnungen als Information, wenn die Integrität von Azure AD Connect-Servern (Synchronisierungsmodul) nicht gewährleistet ist
* E-Mail-Benachrichtigungen für kritische Warnungen
* Synchronisierung von betrieblichen Informationen, z.B. Latenzdiagramme für Synchronisierungsvorgänge und Trends von unterschiedlichen Vorgängen wie Hinzufügungen, Aktualisierungen und Löschungen
* Schnellübersicht über Synchronisierungseigenschaften und letzten erfolgreichen Export zu Azure AD
* Für Berichte zu Fehlern bei der Synchronisierung auf Objektebene \(ist Azure AD Premium nicht erforderlich\)

Weitere Informationen finden Sie unter [Überwachen der Azure AD Connect-Synchronisierung mit Azure AD Connect Health](how-to-connect-health-sync.md).

### <a name="azure-ad-connect-health-for-ad-dshow-to-connect-health-addsmd"></a>[Azure AD Connect Health für AD DS](how-to-connect-health-adds.md)
Azure AD Connect Health for Active Directory Domain Services (AD DS) ermöglicht die Überwachung für Domänencontroller, die unter Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 und Windows Server 2016 installiert sind. Mit der Health-Agent-Installation können Sie Ihre lokale AD DS-Umgebung direkt über die Cloud überwachen. Azure AD Connect Health für AD DS stellt die folgenden wichtigen Funktionen bereit:

* Überwachung von Warnungen, um zu erkennen, wann Domänencontroller Fehler aufweisen, und von E-Mail-Benachrichtigungen für kritische Warnungen
* Das Domänencontroller-Dashboard mit einer Kurzübersicht über den Integritäts- und Betriebsstatus Ihrer Domänencontroller
* Das Replikationsstatus-Dashboard mit den aktuellen Replikationsinformationen und Links zu Problembehandlungsanleitungen, wenn Fehler erkannt werden
* Schneller Zugriff von jedem Ort auf Leistungsdaten-Graphen gängiger Leistungsindikatoren für Problembehandlung und Überwachung

Weitere Informationen finden Sie unter [Verwenden von Azure AD Connect Health mit AD DS](how-to-connect-health-adds.md).

## <a name="next-steps"></a>Nächste Schritte


- [Voraussetzungen für Azure AD Connect](how-to-connect-install-prerequisites.md) 
- [Express-Einstellungen](how-to-connect-install-express.md)
- [Benutzerdefinierte Einstellungen](how-to-connect-install-custom.md)
- [Implementieren der Kennworthashsynchronisierung mit Azure AD Connect](how-to-connect-password-hash-synchronization.md)|
- [Passthrough-Authentifizierung](how-to-connect-pta.md)
- [Azure AD Connect und Verbund](how-to-connect-fed-whatis.md)
- [Installieren des Azure AD Connect Health-Agents](how-to-connect-health-agent-install.md) 
- [Azure AD Connect-Synchronisierung](how-to-connect-sync-whatis.md)
- [Versionsverlauf](reference-connect-version-history.md)
- [Vergleich von Tools für die Verzeichnisintegration](plan-hybrid-identity-design-considerations-tools-comparison.md)
- [Häufig gestellte Fragen zu Azure AD Connect](reference-connect-faq.md)









