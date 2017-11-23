---
title: "Schützen webbasierter und mobiler PaaS-Anwendungen mit Azure App Service | Microsoft-Dokumentation"
description: " Erfahren Sie mehr zu den bewährten Methoden in Azure App Service zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: dec45d91ad1a73306b3e2656dd9bf7fdbe456720
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2017
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-app-service"></a>Schützen webbasierter und mobiler PaaS-Anwendungen mit Azure App Service

In diesem Artikel erläutern wir eine Sammlung empfohlener Vorgehensweisen in [Azure App Services](https://azure.microsoft.com/services/app-service/) zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen. Diese empfohlenen Vorgehensweisen sind aus unseren Erfahrungen mit Azure und den Erfahrungen von Kunden wie Ihnen abgeleitet.

## <a name="azure-app-service"></a>Azure App Service
[Azure App Service](../app-service/app-service-web-overview.md) ist ein PaaS-Angebot, mit dem Sie webbasierte und mobile Apps für beliebige Plattformen oder Geräte erstellen und eine Verbindung mit Daten herstellen können, die in der Cloud oder lokal gespeichert sind. App Service umfasst die Webfunktionen und mobilen Funktionen, die wir vorher separat als Azure Websites und Azure Mobile Services bereitgestellt haben. Außerdem sind neue Funktionen zum Automatisieren von Geschäftsprozessen und Hosten von Cloud-APIs enthalten. Als einzelner integrierter Dienst stellt App Service einen umfangreichen Satz von Funktionen für mobile, Web- und Integrationsszenarien bereit.

## <a name="best-practices"></a>Bewährte Methoden

Befolgen Sie bei Verwendung von App Service diese bewährten Methoden:

- [Authentifizieren Sie sich über Azure Active Directory (AD)](../app-service/app-service-authentication-overview.md). App Service bietet einen OAuth 2.0-Dienst für Ihren Identitätsanbieter. In OAuth 2.0 liegt der Schwerpunkt auf der Vereinfachung der Cliententwicklung. Gleichzeitig werden bestimmte Autorisierungsabläufe für Webanwendungen, Desktopanwendungen und Mobiltelefone bereitgestellt. Azure AD verwendet OAuth 2.0, um Ihnen die Autorisierung des Zugriffs auf mobile und Webanwendungen zu ermöglichen.
- Schränken Sie den Zugriff auf Grundlage der Sicherheitsprinzipien „Need-to-know“ und „geringste Rechte“ ein. Das Einschränken des Zugriffs ist für Organisationen zwingend erforderlich, die Sicherheitsrichtlinien für den Datenzugriff durchsetzen möchten. Die rollenbasierte Zugriffssteuerung (RBAC) kann verwendet werden, um Benutzern, Gruppen und Anwendungen Berechtigungen für einen bestimmten Bereich zu erteilen. Weitere Informationen zum Gewähren des Zugriffs auf Anwendungen für Benutzer finden Sie unter [Erste Schritte mit der Zugriffsverwaltung](../active-directory/role-based-access-control-what-is.md).
- Schützen Sie Ihre Schlüssel. Ihre Sicherheitsfunktionen können noch so ausgereift sein, wenn Sie Ihre Abonnementschlüssel verlieren. Der Azure-Schlüsseltresor unterstützt Sie dabei, kryptografische Schlüssel und geheime Schlüssel zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Durch Verwenden von Key Vault können Sie Schlüssel und Geheimnisse (beispielsweise Authentifizierungsschlüssel, Schlüssel für Speicherkonten, Datenverschlüsselungsschlüssel, PFX-Dateien und Kennwörter) verschlüsseln, indem Sie Schlüssel verwenden, die durch Hardwaresicherheitsmodule (HSMs) geschützt werden. Zur Steigerung der Sicherheit können Sie Schlüssel in HSMs importieren oder in diesen generieren. Weitere Informationen finden Sie unter [Azure Key Vault](../key-vault/key-vault-whatis.md). Sie können auch Key Vault zum Verwalten Ihrer TLS-Zertifikate mit der automatischen Verlängerung verwenden.
- Schränken Sie eingehende Quell-IP-Adressen ein. Die [App Service-Umgebung](../app-service/environment/intro.md) verfügt über ein Feature zur Integration virtueller Netzwerke, mit dem Sie eingehende Quell-IP-Adressen über Netzwerksicherheitsgruppen (NSGs) einschränken können. Falls Sie sich mit Azure Virtual Networks (VNETs) noch nicht auskennen, hilft Ihnen vielleicht diese Beschreibung weiter: Es handelt sich um eine Funktion, mit der Sie viele Azure-Ressourcen in einem nicht über das Internet routbaren Netzwerk anordnen können, für das Sie den Zugriff kontrollieren. Weitere Informationen hierzu finden Sie unter [Integrieren Ihrer App in ein Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde Ihnen eine Sammlung empfohlener Vorgehensweisen in Azure App Service zum Schutz Ihrer webbasierten und mobilen PaaS-Anwendungen vorgestellt. Weitere Informationen zum Schutz Ihrer PaaS-Bereitstellungen finden Sie unter:

- [Schützen von PaaS-Bereitstellungen](security-paas-deployments.md)
- [Schützen webbasierter und mobiler PaaS-Anwendungen mit Azure SQL-Datenbank und SQL Data Warehouse](security-paas-applications-using-sql.md)
