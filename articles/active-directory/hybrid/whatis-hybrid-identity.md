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
ms.date: 09/18/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 40ac3ca92c65607df056b883608dde60d816143e
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181775"
---
# <a name="hybrid-identity-and-microsofts-identity-solutions"></a>Hybrididentität und die Identitätslösungen von Microsoft
Heutzutage werden Unternehmen und Konzerne immer mehr zu einer Mischung aus lokalen und Cloudanwendungen.  Die Verwaltung von Anwendungen und Benutzern, die lokal und in der Cloud Zugriff auf diese Anwendungen benötigen, ist ein schwieriges Szenario geworden.

Die Identitätslösungen von Microsoft decken sowohl lokale als auch cloudbasierte Funktionen ab, und es wird eine einzelne Benutzeridentität für die standortunabhängige Authentifizierung und Autorisierung gegenüber allen Ressourcen erstellt. Wir bezeichnen dies als Hybrididentität.

## <a name="what-is-azure-ad-connect"></a>Was ist Azure AD Connect?

Das Microsoft-Tool „Azure AD Connect“ wurde entwickelt, um Sie beim Erreichen Ihrer Hybrididentitätsziele zu unterstützen.  Dadurch können Sie für Ihre Benutzer eine einzige Identität für in Azure AD integrierte in Office 365-, Azure- und SaaS-Anwendungen bereitstellen.  Er zeichnet sich durch Folgendes aus:
    
- [Synchronisierung](how-to-connect-sync-whatis.md): Diese Komponente ist verantwortlich für das Erstellen von Benutzern, Gruppen und anderen Objekten. Sie stellt ebenfalls sicher, dass Identitätsinformationen für Ihre lokalen Benutzer und Gruppen denen in der Cloud entsprechen.  Sie ist auch für das Synchronisieren von Kennworthashes mit Azure AD verantwortlich.
-   [AD FS und Verbundintegration](how-to-connect-fed-whatis.md): Der Verbund ist eine optionale Komponente von Azure AD Connect und kann zum Konfigurieren einer Hybridumgebung mithilfe einer lokalen AD FS-Infrastruktur verwendet werden. Er bietet auch AD FS-Verwaltungsfunktionen wie die Zertifikaterneuerung und zusätzliche AD FS-Serverbereitstellungen.
-   [Passthrough-Authentifizierung](how-to-connect-pta.md): Dies ist eine weitere optionale Komponente, die Benutzern die Verwendung des gleichen Kennworts lokal und in der Cloud ermöglicht, die zusätzliche Infrastruktur einer Verbundumgebung jedoch nicht erfordert.
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


- [Hardware und Voraussetzungen](how-to-connect-install-prerequisites.md) 
- [Express-Einstellungen](how-to-connect-install-express.md)
- [Benutzerdefinierte Einstellungen](how-to-connect-install-custom.md)
- [Kennworthashsynchronisierung](how-to-connect-password-hash-synchronization.md)|
- [Passthrough-Authentifizierung](how-to-connect-pta.md)
- [Azure AD Connect und Verbund](how-to-connect-fed-whatis.md)
- [Installieren des Azure AD Connect Health-Agents](how-to-connect-health-agent-install.md) 
- [Azure AD Connect-Synchronisierung](how-to-connect-sync-whatis.md)
- [Versionsverlauf](reference-connect-version-history.md)
- [Vergleich von Tools für die Verzeichnisintegration](plan-hybrid-identity-design-considerations-tools-comparison.md)
- [Häufig gestellte Fragen zu Azure AD Connect](reference-connect-faq.md)









