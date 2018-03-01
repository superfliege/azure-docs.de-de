---
title: "Hinzufügen einer mehrinstanzenfähigen Anwendung zum Azure AD-Anwendungskatalog | Microsoft-Dokumentation"
description: "Erläutert, wie Sie Ihre benutzerdefiniert entwickelte mehrinstanzenfähige Anwendung im Azure AD-Anwendungskatalog auflisten können."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: jeedes
ms.openlocfilehash: f29f7cbf118d4d70c1ea2cca174ff0cf0ba9bd14
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a>Hinzufügen einer mehrinstanzenfähigen Anwendung zum Azure AD-Anwendungskatalog

## <a name="what-is-the-azure-ad-application-gallery"></a>Was ist der Azure AD-Anwendungskatalog?

Azure AD ist ein cloudbasierter Identitätsdienst. [Azure AD-App-Katalog](https://azure.microsoft.com/marketplace/active-directory/all/) ist ein allgemeiner Store, in dem alle Anwendungsconnectors für einmaliges Anmelden und Benutzerbereitstellung veröffentlicht werden. Unsere gemeinsamen Kunden, die Azure AD als Identitätsanbieter einsetzen, suchen nach verschiedenen SaaS-Anwendungsconnectors, die hier veröffentlicht werden. IT-Administratoren fügen Connectors aus dem App-Katalog hinzu, konfigurieren diese und verwenden sie für einmaliges Anmelden und Bereitstellungen. Azure AD unterstützt alle wichtigen Verbundprotokolle wie SAML 2.0, OpenID Connect, OAuth und WS-Fed für das einmalige Anmelden. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Wenn Ihre Anwendung SAML oder OpenIDConnect unterstützt
Wenn Sie über eine mehrinstanzenfähige Anwendung verfügen, die Sie dem Azure AD-Anwendungskatalog hinzufügen möchten, müssen Sie zunächst sicherstellen, dass Ihre Anwendung eine der folgenden Technologien zum einmaligen Anmelden unterstützt:

1. **OpenID Connect** – Erstellen Sie die mehrinstanzenfähige Anwendung in Azure AD, und implementieren Sie das [Azure AD-Genehmigungsframework](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) für Ihre Anwendung. Senden Sie die Anmeldeanforderung an einen gemeinsamen Endpunkt, damit jeder Kunde der Anwendung zustimmen kann. Sie können den Benutzerzugriff des Kunden anhand der Mandanten-ID und der im Token erhaltenen UPN des Benutzers steuern. Übermitteln Sie die Anwendung, wie in diesem [Artikel](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) beschrieben.

2. **SAML**: Wenn Ihre Anwendung SAML 2.0 unterstützt, können wir die Anwendung im Katalog listen. Anweisungen finden Sie [hier](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing)

Wenn Ihre Anwendung einen dieser Modi für das einmalige Anmelden unterstützt und Sie Ihre mehrinstanzenfähige Anwendung dem Azure AD-Anwendungskatalog hinzufügen möchten, führen Sie die Schritte in [diesem](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) Artikel aus. 

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Wenn Ihre Anwendung keine SAML- oder OpenIDConnect-Unterstützung bietet
Auch wenn Ihre Anwendung keinen dieser Modi unterstützt, können wir sie mittels einmaligem Anmelden per Kennwort unserem Katalog hinzufügen.

**Kennwortbasiertes SSO** – Erstellen Sie eine Webanwendung mit HTML-Anmeldeseite, um das [kennwortbasierte einmalige Anmelden](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis) zu konfigurieren. Das kennwortbasierte einmalige Anmelden (auch als „Password Vaulting“ oder „Kennworttresor“ bezeichnet) ermöglicht es Ihnen, den Benutzerzugriff und die Kennwörter für Webanwendungen zu verwalten, die keinen Identitätsverbund unterstützen. Es ist auch für Szenarien nützlich, in denen mehrere Benutzer ein Konto gemeinsam verwenden müssen, wie z. B. bei den App-Konten für die sozialen Medien Ihrer Organisation. 

Wenn Sie Ihre Anwendung mit dieser Technologie listen möchten, senden Sie die Anforderung wie in [diesem](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) Artikel beschrieben.

## <a name="escalations"></a>Eskalationen

Bei Eskalationen wenden Sie sich per E-Mail an das [Azure AD SSO-Integrationsteam](<mailto:SaaSApplicationIntegrations@service.microsoft.com>), das sich umgehend bei Ihnen meldet.

## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen Ihrer Anwendung zum Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
