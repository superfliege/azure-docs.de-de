---
title: 'Tutorial: Konfigurieren von Pingboard für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Pingboard konfigurieren.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 8fa98ff4cee206894c35f84cca3f3b4c43e2e5ce
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Pingboard für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte beschrieben, die Sie ausführen müssen, um die automatische Bereitstellung und Bereitstellungsaufhebung von Benutzerkonten in Azure Active Directory (Azure AD) und Pingboard zu ermöglichen.

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure AD-Mandanten
*   Ein [Pro-Konto](https://pingboard.com/pricing) für einen Pingboard-Mandanten 
*   Ein Benutzerkonto in Pingboard mit Administratorrechten 

> [!NOTE] 
> Die Azure AD-Bereitstellungsintegration basiert auf der [Pingboard-API](`https://your_domain.pingboard.com/scim/v2`), die für Ihr Konto verfügbar ist.

## <a name="assign-users-to-pingboard"></a>Zuweisen von Benutzern zu Pingboard

Azure AD ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf ausgewählte Anwendungen erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden. 

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer in Azure AD Zugriff auf Ihre Pingboard-App benötigen. Anschließend können Sie diese Benutzer Ihrer Pingboard-App zuweisen, indem Sie diese Anweisungen befolgen:

[Zuweisen eines Benutzers zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Pingboard

Es wird empfohlen, Pingboard einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können weitere Benutzer zugewiesen werden.

## <a name="configure-user-provisioning-to-pingboard"></a>Konfigurieren der Benutzerbereitstellung in Pingboard 

Dieser Abschnitt führt Sie durch das Verbinden Ihres Azure AD mit der Pingboard-API zum Bereitstellen von Benutzerkonten. Außerdem erfahren Sie, wie Sie den Bereitstellungsdienst zum Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in Pingboard auf der Grundlage von Benutzerzuweisungen in Azure AD konfigurieren.

> [!TIP]
> Um das SAML-basierte einmalige Anmelden für Pingboard zu aktivieren, befolgen Sie hierzu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-ad"></a>So konfigurieren Sie die automatische Bereitstellung von Benutzerkonten für Pingboard in Azure AD

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory** > **Unternehmens-Apps** > **Alle Anwendungen**.

2. Suchen Sie über das Suchfeld nach Ihrer Pingboard-Instanz, wenn Sie Pingboard bereits für einmaliges Anmelden konfiguriert haben. Wählen Sie andernfalls **Hinzufügen**, und suchen Sie im Anwendungskatalog nach **Pingboard**. Wählen Sie **Pingboard** in den Suchergebnissen aus, und fügen Sie es Ihrer Anwendungsliste hinzu.

3. Wählen Sie Ihre Pingboard-Instanz und dann die Registerkarte **Bereitstellung** aus.

4. Legen Sie **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Pingboard-Bereitstellung](./media/active-directory-saas-pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
5. Führen Sie im Abschnitt **Administratoranmeldeinformationen** die folgenden Schritte aus:

    a. Geben Sie `https://your_domain.pingboard.com/scim/v2` unter **Mandanten-URL** ein, und ersetzen Sie „your_domain“ durch die richtige Domäne.

    b. Melden Sie sich mit Ihrem Administratorkonto bei [Pingboard](https://pingboard.com/) an.

    c. Wählen Sie **Add-Ons** > **Integrations (Integrationen)** > **Azure Active Directory** aus.

    d. Navigieren Sie zur Registerkarte **Configure** (Konfigurieren), und wählen Sie **Enable user provisioning from Azure** (Benutzerbereitstellung über Azure ermöglichen) aus.

    e. Kopieren Sie das Token unter **OAuth Bearer Token** (OAuth-Bearertoken), und geben Sie es in das Feld **Geheimes Token** ein.

6. Wählen Sie im Azure-Portal die Option **Verbindung testen** aus, um sich zu vergewissern, dass Azure AD eine Verbindung mit Ihrer Pingboard-App herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr Pingboard-Konto über Administratorrechte verfügt, und wiederholen Sie den Schritt **Verbindung testen**.

7. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll. Aktivieren Sie dann das darunter befindliche Kontrollkästchen.

8. Wählen Sie **Speichern**aus. 

9. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Pingboard synchronisieren** aus.

10. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Pingboard synchronisiert werden sollen. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Pingboard für Updatevorgänge verwendet werden. Wählen Sie **Speichern** aus, um Ihre Änderungen zu committen. Weitere Informationen finden Sie unter [Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory](active-directory-saas-customizing-attribute-mappings.md).

11. Um den Azure AD-Bereitstellungsdienst für Pingboard zu aktivieren, ändern Sie im Abschnitt **Einstellungen** den **Bereitstellungsstatus** in **Ein**.

12. Klicken Sie auf **Speichern**, um die Erstsynchronisierung der Benutzer zu starten, die Pingboard zugewiesen sind.

Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Dienst ausgeführt wird. Überwachen Sie anhand des Abschnitts **Synchronisierungsdetails** den Fortschritt, und navigieren Sie über Links zu Protokollen zur Bereitstellungsaktivität. In den Protokollen werden sämtliche Aktionen beschrieben, die vom Bereitstellungsdienst für Ihre Pingboard-App ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps im Azure-Portal](active-directory-enterprise-apps-manage-provisioning.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md)
* [Konfigurieren von einmaligem Anmelden](active-directory-saas-pingboard-tutorial.md)
