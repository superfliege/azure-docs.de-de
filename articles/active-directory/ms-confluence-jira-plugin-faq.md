---
title: Plug-In zum einmaligen Anmelden mit Microsoft Azure Active Directory – FAQ | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und Microsoft Azure Active Directory single sign-on for JIRA konfigurieren.
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
ms.date: 02/06/2018
ms.author: jeedes
ms.openlocfilehash: 571fbd5078f66375f6e81cba2a790121366f9d60
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="microsoft-azure-active-directory-single-sign-on-plugin-faq"></a>Plug-In zum einmaligen Anmelden mit Microsoft Azure Active Directory – FAQ 

## <a name="1-whats-the-microsoft-sso-add-on"></a>1. Was ist das Add-On für Microsoft-SSO?

Dieses Add-On ermöglicht das einmalige Anmelden (Single Sign On, SSO) für JIRA von Atlassian (einschließlich JIRA Core, JIRA Software, JIRA Service Desk) und lokale Confluence-Software. Das Add-On funktioniert mit Azure AD als IdP.

## <a name="2-add-on-works-with-which-atlassian-products"></a>2. Mit welchen Atlassian-Produkten funktioniert das Add-On?

Derzeit funktioniert das Add-On mit lokalen Versionen von JIRA und Confluence.

## <a name="3-does-this-add-on-work-on-cloud-version"></a>3. Funktioniert dieses Add-On mit der Cloudversion?

Nein. Nur lokale Versionen von JIRA und Confluence werden unterstützt.

## <a name="4-which-versions-of-jira-and-confluence-are-supported"></a>4. Welche Versionen von JIRA und Confluence werden unterstützt?

Im Folgenden finden Sie die Liste der unterstützten Versionen:

* JIRA Core und Software: 6.0 bis 7.2.2 
* JIRA Service Desk: 3.0 bis 3.2 
* Confluence: 5.0 bis 5.10

## <a name="5-is-this-add-on-free-or-paid"></a>5. Ist dieses Add-On kostenlos oder kostenpflichtig?

Dies ist ein kostenloses Add-On und kann vom Atlassian-Marketplace aus installiert werden.

## <a name="6-do-i-need-to-restart-jiraconfluence-once-i-deploy-the-add-on"></a>6. Muss ich JIRA/Confluence nach dem Bereitstellen des Add-Ons neu starten?

Neustart nach der Bereitstellung ist nicht erforderlich. Sie können das Add-On sofort nach der Bereitstellung verwenden.

## <a name="7-how-do-i-get-support-for-the-add-on"></a>7. Wie erhalte ich Support für das Add-On?

Sie erreichen uns unter: <email>. Wir werden innerhalb von Stunden <> antworten. Sie können auch bei Microsoft ein Supportticket über den Azure-Portalkanal erstellen. Sie können uns auch unter <Number> zwischen <> Uhr und <> Uhr wochentags anrufen.

## <a name="8-would-this-add-on-work-on-mac-or-ubuntu-installation-of-jira-and-confluence"></a>8. Funktioniert dieses Add-On unter der Mac- oder Ubuntu-Installation von JIRA und Confluence?

Wir haben dieses Add-On nur mit 64-Bit-Windows-Serverinstallationen von JIRA und Confluence getestet.

## <a name="9-does-this-add-on-work-with-other-idps-than-azure-ad"></a>9. Funktioniert dieses Add-On mit anderen IdPs als Azure AD?

Nein. Das Add-On funktioniert nur mit Azure AD.

## <a name="10-what-version-of-saml-does-the-add-on-work-with"></a>10. Mit welcher SAML-Version funktioniert das Add-On?

Das Add-On funktioniert mit SAML 2.0.

## <a name="11-does-the-add-on-do-use-provisioning-as-well"></a>11. Verwendet das Add-On auch die Bereitstellung?

Nein. Derzeit stellt das Add-On nur SSO auf Basis von SAML 2.0 bereit. Der Benutzer muss vor der SSO-Anmeldung in der Anwendung bereitgestellt werden.

## <a name="12-are-cluster-versions-of-jira-and-confluence-supported-by-add-on"></a>12. Unterstützt das Add-On Clusterversionen von JIRA und Confluence?

Nein. Das Add-On funktioniert mit lokalen Versionen von JIRA und Confluence.

## <a name="13-would-this-plugin-work-with-http-version-of-jira-and-confluence"></a>13. Würde dieses Plug-In mit der HTTP-Version von JIRA und Confluence funktionieren?

Nein. Das Add-On funktioniert nur mit HTTPS-fähigen Installationen.

## <a name="14-do-i-need-to-buy-license-of-the-add-on"></a>14. Muss ich eine Lizenz für das Add-On kaufen?

Es ist ein kostenloses Add-On.