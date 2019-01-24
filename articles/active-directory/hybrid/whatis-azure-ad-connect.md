---
title: Was sind Azure AD Connect und Connect Health? | Microsoft-Dokumentation
description: Beschreibt die Tools, die zum Synchronisieren und Überwachen Ihrer lokalen Umgebung mit Azure AD verwendet werden.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: get-started-article
ms.date: 11/28/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 1c18200bb36b75a07d7b26e3ea0016ec35efdd87
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54460498"
---
# <a name="what-is-azure-ad-connect"></a>Was ist Azure AD Connect?

Das Microsoft-Tool Azure AD Connect wurde entwickelt, um Sie beim Erreichen Ihrer Hybrididentitätsziele zu unterstützen.  Er zeichnet sich durch Folgendes aus:
    
- [Kennworthashsynchronisierung](whatis-phs.md): Eine Anmeldemethode, die einen Hash für das lokale AD-Kennwort eines Benutzers mit Azure AD synchronisiert.
- [Passthrough-Authentifizierung](how-to-connect-pta.md): Eine Anmeldemethode, die Benutzern die Verwendung des gleichen Kennworts lokal und in der Cloud ermöglicht, die zusätzliche Infrastruktur einer Verbundumgebung jedoch nicht erfordert.
- [Verbundintegration](how-to-connect-fed-whatis.md): Der Verbund ist eine optionale Komponente von Azure AD Connect und kann zum Konfigurieren einer Hybridumgebung mithilfe einer lokalen AD FS-Infrastruktur verwendet werden. Er bietet auch AD FS-Verwaltungsfunktionen wie die Zertifikaterneuerung und zusätzliche AD FS-Serverbereitstellungen.
- [Synchronisierung](how-to-connect-sync-whatis.md): Dieser Prozess ist verantwortlich für das Erstellen von Benutzern, Gruppen und anderen Objekten.  Er stellt auch sicher, dass Identitätsinformationen für Ihre lokalen Benutzer und Gruppen denen in der Cloud entsprechen.  Diese Synchronisierung umfasst auch Kennworthashes.
-   [Systemüberwachung](whatis-hybrid-identity-health.md): Azure AD Connect Health bietet eine stabile Überwachung und einen zentralen Speicherort im Azure-Portal, um diese Aktivität anzuzeigen. 


![Was ist Azure AD Connect?](./media/whatis-hybrid-identity/arch.png)



## <a name="what-is-azure-ad-connect-health"></a>Was ist Azure AD Connect Health?

Azure Active Directory (Azure AD) Connect Health bietet eine stabile Überwachung Ihrer lokalen Identitätsinfrastruktur. Mit Connect Health können Sie eine zuverlässige Verbindung mit Office 365 und Microsoft Online Services sicherstellen.  Diese Zuverlässigkeit wird durch Überwachungsfunktionen für Ihre wichtigen Identitätskomponenten erreicht. Darüber hinaus ermöglicht Connect Health den einfachen Zugriff auf die wichtigen Datenpunkte dieser Komponenten.

Die Informationen werden im [Azure AD Connect Health-Portal](https://aka.ms/aadconnecthealth)angezeigt. Im Azure AD Connect Health-Portal können Sie Warnungen, Leistungsüberwachungsdaten, Nutzungsanalysen und andere Informationen anzeigen. Azure AD Connect Health ermöglicht Ihnen an einem zentralen Ort einen Gesamtüberblick über die Integrität Ihrer wichtigsten Identitätskomponenten.

![Was ist Azure AD Connect Health?](./media/whatis-hybrid-identity-health/aadconnecthealth2.png)

## <a name="why-use-azure-ad-connect"></a>Gründe für die Verwendung von Azure AD Connect
Die Integration Ihrer lokalen Verzeichnisse in Azure AD steigert die Produktivität Ihrer Benutzer, da für den Zugriff auf die Cloud und lokale Ressourcen nur eine Identität benötigt wird. Benutzer und Organisationen können die folgenden Vorteile nutzen:

* Benutzer können eine einzelne Identität verwenden, um auf lokale Anwendungen und Clouddienste wie z. B. Office 365 zuzugreifen.
* Einzelnes Tool zum Bereitstellen einer einfachen Bereitstellungserfahrung für die Synchronisierung und Anmeldung.
* Bietet die neuesten Funktionen für Ihre Szenarien. Azure AD Connect ersetzt ältere Versionen von Identitätsintegrationstools, wie z. B. DirSync und Azure AD Sync. Weitere Informationen finden Sie unter [Vergleich von Tools für die Verzeichnisintegration für Hybrididentitäten](plan-hybrid-identity-design-considerations-tools-comparison.md).

## <a name="why-use-azure-ad-connect-health"></a>Gründe für die Verwendung von Azure AD Connect Health
Durch die Verwendung von Azure AD steigern Sie die Produktivität Ihrer Benutzer, da für den Zugriff auf Cloudressourcen und lokale Ressourcen nur eine Identität benötigt wird. Sicherzustellen, dass die Umgebung zuverlässig ist und Benutzer auf diese Ressourcen zugreifen können, wird zu einer Herausforderung.  Azure AD Connect Health unterstützt Sie bei der Überwachung Ihrer lokalen Identitätsinfrastruktur, liefert Ihnen wichtige Erkenntnisse und stellt so die Zuverlässigkeit der Umgebung sicher. Dieser Ansatz ist ähnlich unkompliziert wie das Installieren eines Agents auf Ihren lokalen Identitätsservern.


## <a name="next-steps"></a>Nächste Schritte

- [Voraussetzungen für Azure AD Connect](how-to-connect-install-prerequisites.md) 
- [Express-Einstellungen](how-to-connect-install-express.md)
- [Benutzerdefinierte Einstellungen](how-to-connect-install-custom.md)
- [Installieren des Azure AD Connect Health-Agents](how-to-connect-health-agent-install.md) 
