---
title: Konfigurieren des einmaligen Anmeldens bei einer Anwendungsproxyanwendung | Microsoft-Dokumentation
description: Schnelles Konfigurieren des einmaligen Anmeldens bei einer Anwendungsproxyanwendung
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 91bbbf6cced8dc3ade4906961d68a99f810d9fd9
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956985"
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Konfigurieren des einmaligen Anmeldens bei einer Anwendungsproxyanwendung

Das einmalige Anmelden (Single Sign-On, SSO) gestattet Benutzern den Zugriff auf eine Anwendung, ohne sich mehrfach authentifizieren zu müssen. Es ermöglicht, dass eine einzelne Authentifizierung in der Cloud bei Azure Active Directory erfolgt. Der Dienst oder Connector kann dann die Identität des Benutzers annehmen, um alle weiteren Authentifizierungsaufgaben der Anwendung abzuschließen.

## <a name="how-to-configure-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
Stellen Sie zum Konfigurieren des einmaligen Anmeldens zunächst sicher, dass Ihre Anwendung für die Vorauthentifizierung über Azure Active Directory konfiguriert ist. Wechseln Sie für diese Konfiguration zu **Azure Active Directory** -&gt; **Unternehmensanwendungen** -&gt; **Alle Anwendungen** -&gt; Ihre Anwendung **-&gt; Anwendungsproxy**. Auf dieser Seite befindet sich das Feld „Vorauthentifizierung“. Stellen Sie sicher, dass hier der Wert „Azure Active Directory“ festgelegt ist. 

Weitere Informationen zu den Methoden zur Vorauthentifizierung finden Sie unter Schritt 4 des [Dokuments zur App-Veröffentlichung](application-proxy-publish-azure-portal.md).

   ![Methode zur Vorauthentifizierung im Azure-Portal](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Konfigurieren der Modi für einmaliges Anmelden für Anwendungsproxyanwendungen
Konfigurieren Sie den bestimmten Typ des einmaligen Anmeldens. Die Anmeldemethoden sind auf Basis des Authentifizierungstyps klassifiziert, der von der Back-End-Anwendung verwendet wird. App-Proxyanwendungen unterstützen drei Arten von Anmeldungen:

-   **Kennwortbasierte Anmeldung**: Die kennwortbasierte Anmeldung kann für jede Anwendung verwendet werden, die Felder für Benutzername und Kennwort für die Anmeldung verwendet. Die Konfigurationsschritte finden Sie unter [Konfigurieren des einmaligen Anmeldens per Kennwort für eine Azure AD-Kataloganwendung](configure-password-single-sign-on-gallery-applications.md).

-   **Integrierte Windows-Authentifizierung**: Für Anwendungen, die die integrierte Windows-Authentifizierung (IWA) verwenden, wird das einmalige Anmelden durch die eingeschränkte Kerberos-Delegierung (KCD) ermöglicht. Mit dieser Methode erhalten Anwendungsproxyconnectors in Active Directory die Berechtigung, die Identität von Benutzern anzunehmen und in deren Auftrag Token zu senden und zu empfangen. Informationen zur KCD-Konfiguration finden Sie der [Dokumentation zum einmaligen Anmelden mit KCD](application-proxy-configure-single-sign-on-with-kcd.md).

-   **Headerbasierte Anmeldung**: Die headerbasierte Anmeldung wird über eine Partnerschaft ermöglicht und erfordert zusätzliche Konfigurationsschritte. Ausführliche Informationen zur Partnerschaft sowie schrittweise Anweisungen zum Konfigurieren des einmaligen Anmeldens bei einer Anwendung, die Header zur Authentifizierung verwendet, finden Sie in der [Dokumentation zu PingAccess für Azure AD](application-proxy-configure-single-sign-on-with-ping-access.md).

Jede dieser Optionen können Sie finden, indem Sie in „Unternehmens-Apps“ zu Ihrer Anwendung wechseln und im linken Menü die Seite **Einmaliges Anmelden** öffnen. Beachten Sie, dass möglicherweise nicht alle Optionen angezeigt werden, wenn Ihre Anwendung im alten Portal erstellt wurde.

Auf dieser Seite befindet sich auch eine zusätzliche Option für einmaliges Anmelden: Anmeldung über Link. Diese Option wird auch vom Anwendungsproxy unterstützt. Mit dieser Option wird jedoch nicht das einmalige Anmelden zur Anwendung hinzugefügt. Dies bedeutet, dass für die Anwendung das einmalige Anmelden möglicherweise bereits über einen anderen Dienst implementiert ist, z. B. Active Directory-Verbunddienste. 

Mit dieser Option kann einen Administrator einen Link zu einer Anwendung erstellen, zu dem Benutzer zuerst gelangen, wenn sie auf die Anwendung zugreifen. Ist eine Anwendung beispielsweise zum Authentifizieren von Benutzern mit Active Directory Federation Services 2.0 konfiguriert, kann ein Administrator mit der Option „Anmeldung über Link“ einen entsprechenden Link im Zugriffsbereich erstellen.

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](application-proxy-configure-single-sign-on-with-kcd.md)
