---
title: Azure AD Connect in Microsoft Cloud Deutschland
description: Azure AD Connect integriert Ihre lokalen Verzeichnisse in Azure Active Directory. Dadurch können Sie eine einzige Identität für in Azure AD integrierte in Office 365-, Azure- und SaaS-Anwendungen bereitstellen.
keywords: Einführung in Azure AD Connect, Übersicht über Azure AD Connect, was ist Azure AD Connect, Active Directory installieren, Deutschland, Black Forest
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 5ccfab71edf9f5fd61191ae8bd4ef5421a055e23
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164621"
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect in Microsoft Cloud Deutschland – Öffentliche Vorschau
## <a name="introduction"></a>Einführung
Azure AD Connect ermöglicht die Synchronisierung zwischen Ihrer lokalen Active Directory-Instanz und Azure Active Directory.
Derzeit müssen viele Szenarien in [Microsoft Cloud Deutschland](https://azure.microsoft.com/global-infrastructure/germany/
) vom Bediener durchgeführt werden. Beachten Sie bei der Nutzung von Microsoft Cloud Deutschland folgende Informationen:

* Die folgenden URLs müssen auf einem Proxyserver geöffnet werden, damit die Synchronisierung erfolgreich durchgeführt werden kann:
  
  * *.microsoftonline.de
  * *.windows.net
  * * Zertifikatsperrlisten
* Wenn Sie sich bei Ihrem Azure AD-Verzeichnis anmelden, müssen Sie ein Konto in der Domäne „onmicrosoft.de“ verwenden.

 
## <a name="download"></a>Download
Sie können Azure AD Connect über das Blatt „Azure AD Connect“ im Portal herunterladen.  In der Anleitung unten ist angegeben, wie Sie das Blatt „Azure AD Connect“ finden.

### <a name="the-azure-ad-connect-blade"></a>Blatt „Azure AD Connect“
Nach Ihrer Anmeldung beim Azure-Portal:

1. Wählen Sie „Durchsuchen“.
2. Wählen Sie „Azure Active Directory“.
3. Wählen Sie „Azure AD Connect“.

Die folgenden Details werden angezeigt:

![Blatt „Azure AD Connect“](./media/reference-connect-germany/germany1.png)

In der folgenden Tabelle sind die Funktionen des Blatts beschrieben.

| Titel | BESCHREIBUNG |
| --- | --- |
| SYNCHRONISIERUNGSSTATUS |Gibt an, ob die Synchronisierung aktiviert oder deaktiviert ist. |
| LETZTE SYNCHRONISIERUNG |Gibt den Zeitpunkt der letzten erfolgreichen Synchronisierung an. |
| PARTNERDOMÄNEN |Gibt die Anzahl der derzeit konfigurierten Partnerdomänen an. |

## <a name="installation"></a>Installation
Die Dokumentation zur Installation von Azure AD Connect finden Sie [hier](how-to-connect-install-roadmap.md).

## <a name="advanced-features-and-additional-information"></a>Erweiterte Funktionen und zusätzliche Informationen
Falls Sie weitere Informationen zu benutzerdefinierten Einstellungen oder erweiterten Konfigurationseinstellungen benötigen, sollten Sie sich den Artikel [Hybrididentität und Identitätslösungen von Microsoft](whatis-hybrid-identity.md) ansehen. Diese Seite enthält Informationen und Links zu weiteren Anleitungen.

