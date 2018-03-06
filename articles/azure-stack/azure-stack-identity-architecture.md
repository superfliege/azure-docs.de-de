---
title: "Identitätsarchitektur für Azure Stack | Microsoft-Dokumentation"
description: "Es wird die Identitätsarchitektur beschrieben, die Sie mit Azure Stack verwenden können."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/22/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 0f3e28f7726afab02211902b5ba2e478ae8065df
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2018
---
# <a name="identity-architecture-for-azure-stack"></a>Identitätsarchitektur für Azure Stack
Bevor Sie einen Identitätsanbieter für die Verwendung mit Azure Stack auswählen, sollten Sie sich mit den wichtigen Unterschieden zwischen den Optionen von Azure Active Directory (Azure AD) und Active Directory-Verbunddienste (AD FS) vertraut machen. 

## <a name="capabilities-and-limitations"></a>Funktionen und Einschränkungen 
Es kann sein, dass Ihre Optionen, einschließlich der Unterstützung mehrerer Mandanten, durch die Wahl des Identitätsanbieters eingeschränkt sind. 

  

|Funktion oder Szenario        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|Verbindung mit dem Internet vorhanden     |Ja       |Optional|
|Unterstützung mehrerer Mandanten     |Ja       |Nein      |
|Marketplace-Syndikation       |Ja       |Ja. Die Nutzung des Tools für die [Marketplace-Offlinesyndikation](azure-stack-download-azure-marketplace-item.md#download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) ist erforderlich.|
|Unterstützung der Active Directory Authentication Library (ADAL) |Ja |Ja|
|Unterstützung von Tools wie der Azure-Befehlszeilenschnittstelle (CLI), Visual Studio (VS) und PowerShell  |Ja |Ja|
|Erstellen von Dienstprinzipalen über das Portal     |Ja |Nein |
|Erstellen von Dienstprinzipalen mit Zertifikaten      |Ja |Ja|
|Erstellen von Dienstprinzipalen mit Geheimnissen (Schlüsseln)    |Ja |Nein |
|Anwendungen können den Graph-Dienst verwenden           |Ja |Nein |
|Anwendungen können einen Identitätsanbieter zum Anmelden verwenden |Ja |Ja. Für Anwendungen muss ein Verbund mit Ihrer lokalen AD FS-Instanz eingerichtet werden. |

## <a name="topologies"></a>Topologien
Die folgenden Abschnitte enthalten Informationen zu Identitätstopologien, die Sie verwenden können.

### <a name="azure-ad--single-tenant"></a>Azure AD – ein Mandant 
Bei der Installation von Azure Stack und der Verwendung von Azure AD wird für Azure Stack standardmäßig eine Topologie mit nur einem Mandanten genutzt. 

Eine Topologie mit nur einem Mandanten ist geeignet, wenn Folgendes gilt:
- Alle Benutzer sind Teil desselben Mandanten.
- Ein Dienstanbieter hostet eine Azure Stack-Instanz für eine Organisation.  

![Azure Stack-Topologie: Topologie mit nur einem Mandanten für Azure AD](media/azure-stack-identity-architecture/single-tenant.png)

Für diese Topologie gilt Folgendes:
- Azure Stack registriert alle Anwendungen und Dienste unter demselben Azure AD-Mandantenverzeichnis. 
- Azure Stack authentifiziert nur die Benutzer und Anwendungen aus diesem Verzeichnis, einschließlich Token. 
- Identitäten für Administratoren (Cloudbetreiber) und Mandantenbenutzer befinden sich unter demselben Verzeichnismandanten. 
- Sie müssen den [Benutzer als Gast in das Mandantenverzeichnis einladen](azure-stack-identity-overview.md#guest-users), damit ein Benutzer aus einem anderen Verzeichnis auf diese Azure Stack-Umgebung zugreifen kann.  

### <a name="azure-ad--multi-tenant"></a>Azure AD – mehrere Mandanten (Mehrinstanzenfähigkeit)
Cloudbetreiber können Azure Stack so konfigurieren, dass der Zugriff auf Anwendungen mit Mandanten einer oder mehrerer Organisationen zulässig ist. Benutzer greifen über das Benutzerportal auf Anwendungen zu. In dieser Konfiguration ist das Administratorportal (vom Cloudbetreiber genutzt) auf Benutzer aus einem Verzeichnis beschränkt. 

Eine Topologie mit mehreren Mandanten ist geeignet, wenn Folgendes gilt:
- Ein Dienstanbieter möchte für Benutzer mehrerer Organisationen den Zugriff auf Azure Stack zulassen.

![Azure Stack-Topologie: Topologie mit mehreren Mandanten für Azure AD](media/azure-stack-identity-architecture/multi-tenant.png)

Für diese Topologie gilt Folgendes:
- Der Zugriff auf Ressourcen sollte pro Organisation erfolgen. 
- Für Benutzer einer Organisation sollte es nicht möglich sein, Benutzern, die sich außerhalb ihrer Organisation befinden, Zugriff auf Ressourcen zu gewähren.  
- Identitäten für Administratoren (Cloudbetreiber) können sich unter einem anderen Verzeichnismandanten als die Identitäten für Benutzer befinden. Diese Trennung sorgt auf Identitätsanbieterebene für Kontoisolation. 
 
### <a name="ad-fs"></a>AD FS  
Die AD FS-Topologie ist erforderlich, wenn eine der folgenden Bedingungen zutrifft:
- Azure Stack stellt keine Verbindung mit dem Internet her.
- Azure Stack kann eine Verbindung mit dem Internet herstellen, aber Sie wählen AD FS als Ihren Identitätsanbieter aus.
  
![Azure Stack-Topologie per AD FS](media/azure-stack-identity-architecture/adfs.png)

Für diese Topologie gilt Folgendes:
- Zur Unterstützung der Nutzung in der Produktion müssen Sie die integrierte Azure Stack AD FS-Instanz in eine vorhandene AD FS-Instanz integrieren, die auf Active Directory (AD) über eine Verbundvertrauensstellung basiert. 
- Sie können den Graph-Dienst mit Ihrer vorhandenen AD-Instanz in Azure Stack integrieren.  Sie können auch den OData-basierten Graph-API-Dienst verwenden, der mit der Azure AD-Graph-API konsistente APIs unterstützt.  

  Für die Interaktion mit Ihrer AD-Instanz benötigt die Graph-API Benutzeranmeldeinformationen der AD-Instanz, die über eine entsprechende Leseberechtigung verfügen. 
  - Die integrierte AD FS-Instanz basiert auf Server 2016. 
  - AD FS und AD müssen auf Server 2012 oder früher basieren. 
  
  Zwischen Ihrer AD-Instanz und der integrierten AD FS-Instanz sind Interaktionen nicht auf OpenID Connect beschränkt, und es können alle gegenseitig unterstützten Protokolle verwendet werden.  
  - Benutzerkonten werden auf Ihrer lokalen AD-Instanz erstellt und verwaltet.
  - Dienstprinzipale und Registrierungen für Anwendungen werden in der integrierten AD-Instanz verwaltet.



## <a name="next-steps"></a>Nächste Schritte
- [Übersicht über Identitäten](azure-stack-identity-overview.md)   
- [Azure Stack-Datencenterintegration: Identität](azure-stack-integrate-identity.md)