---
title: Häufig gestellte Fragen zum Azure Active Directory-SSO-Plug-In | Microsoft-Dokumentation
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zum Konfigurieren des einmaligen Anmeldens zwischen Azure Active Directory und JIRA/Confluence.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: jeedes
ms.openlocfilehash: eb7576c48d3836eec8051d849cefe4c5ec6658c9
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699205"
---
# <a name="faq-for-the-azure-active-directory-sso-plug-in"></a>Häufig gestellte Fragen zum Azure Active Directory-SSO-Plug-In

Im Anschluss finden Sie einige Antworten auf mögliche Fragen zu diesem Plug-In.

## <a name="what-does-the-plug-in-do"></a>Was macht das Plug-In?

Das Plug-In ermöglicht das einmalige Anmelden (Single Sign On, SSO) bei lokaler Atlassian JIRA- und Confluence-Software (einschließlich JIRA Core, JIRA Software, JIRA Service Desk). Das Plug-In funktioniert mit Azure Active Directory (Azure AD) als Identitätsanbieter (IdP).

## <a name="which-atlassian-products-does-the-plug-in-work-with"></a>Mit welchen Atlassian-Produkten ist das Plug-In einsetzbar?

Das Plug-In funktioniert mit lokalen Versionen von JIRA und Confluence.

## <a name="does-the-plug-in-work-on-cloud-versions"></a>Funktioniert dieses Plug-In mit Cloudversionen?

Nein. Das Plug-In unterstützt lokale Versionen von JIRA und Confluence.

## <a name="which-versions-of-jira-and-confluence-does-the-plug-in-support"></a>Welche Versionen von JIRA und Confluence unterstützt das Plug-In?

Das Plug-In unterstützt diese Versionen:

* Jira Core und Software: 6.0 bis 7.8
* JIRA Service Desk: 3.0 bis 3.2
* Confluence: 5.0 bis 5.10

## <a name="is-the-plug-in-free-or-paid"></a>Ist das Plug-In kostenlos oder kostenpflichtig?

Es ist ein kostenloses Add-On.

## <a name="do-i-need-to-restart-jira-or-confluence-after-i-deploy-the-plug-in"></a>Muss ich JIRA bzw. Confluence nach dem Bereitstellen des Plug-Ins neu starten?

Es ist kein Neustart erforderlich. Sie können das Plug-In sofort benutzen.

## <a name="how-do-i-get-support-for-the-plug-in"></a>Wie erhalte ich Support für das Plug-In?

Setzen Sie sich mit dem [Azure AD-SSO-Integrationsteam](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) in Verbindung, um Support für dieses Plug-In zu erhalten. Das Team antwortet innerhalb von 24 bis 48 Geschäftsstunden.

Sie können auch bei Microsoft ein Supportticket über den Azure-Portalkanal erstellen.

## <a name="would-the-plug-in-work-on-a-mac-or-ubuntu-installation-of-jira-and-confluence"></a>Funktioniert dieses Plug-In unter der Mac- oder Ubuntu-Installation von JIRA und Confluence?

Wir haben dieses Plug-In nur mit 64-Bit-Windows-Serverinstallationen von JIRA und Confluence getestet.

## <a name="does-the-plug-in-work-with-idps-other-than-azure-ad"></a>Funktioniert das Plug-In mit anderen IdPs als Azure AD?

Nein. Es funktioniert nur mit Azure AD.

## <a name="what-version-of-saml-does-the-plug-in-work-with"></a>Mit welcher SAML-Version funktioniert das Plug-In?

Es funktioniert mit SAML 2.0.

## <a name="does-the-plug-in-do-user-provisioning"></a>Führt das Plug-In eine Benutzerbereitstellung durch?

Nein. Das Plug-In stellt nur SSO auf Basis von SAML 2.0 bereit. Der Benutzer muss vor der SSO-Anmeldung in der Anwendung bereitgestellt werden.

## <a name="does-the-plug-in-support-cluster-versions-of-jira-and-confluence"></a>Unterstützt das Plug-In Clusterversionen von JIRA und Confluence?

Nein. Das Plug-In funktioniert mit lokalen Versionen von JIRA und Confluence.

## <a name="does-the-plug-in-work-with-http-versions-of-jira-and-confluence"></a>Funktioniert das Plug-In mit HTTP-Versionen von JIRA und Confluence?

Nein. Das Plug-In funktioniert nur mit HTTPS-fähigen Installationen.