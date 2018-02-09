---
title: "Tutorial: Konfigurieren von Pingboard für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Pingboard konfigurieren."
services: active-directory
documentationcenter: 
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
ms.openlocfilehash: f74e890cf716cfd4bc7b41fcc4aa244969cafde5
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Pingboard für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte beschrieben, die Sie ausführen müssen, um die automatische Bereitstellung und Bereitstellungsaufhebung von Benutzerkonten in Azure Active Directory und Pingboard zu ermöglichen.

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Azure Active Directory-Mandant
*   Ein [Pro-Konto](https://pingboard.com/pricing) für einen Pingboard-Mandanten 
*   Ein Benutzerkonto in Pingboard mit Administratorberechtigungen 

> [!NOTE] 
> Die Azure Active Directory-Bereitstellungsintegration basiert auf der Pingboard-API(`https://your_domain.pingboard.com/scim/v2`), die für Ihr Konto verfügbar ist.

## <a name="assigning-users-to-pingboard"></a>Zuweisen von Benutzern zu Pingboard

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Anwendungen erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer synchronisiert, die einer Anwendung in Azure Active Directory zugewiesen wurden. 

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer in Azure Active Directory die Benutzer darstellen, die Zugriff auf Ihre Pingboard-App benötigen. Anschließend können Sie diese Benutzer Ihrer Pingboard-App zuweisen, indem Sie diese Anweisungen befolgen:

[Zuweisen eines Benutzers zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Pingboard

*   Es wird empfohlen, dass Sie zum Testen der Bereitstellungskonfiguration Pingboard einen einzelnen Azure Active Directory-Benutzer zuweisen. Später können weitere Benutzer zugewiesen werden.

## <a name="configuring-user-provisioning-to-pingboard"></a>Konfigurieren der Benutzerbereitstellung in Pingboard 

In diesem Abschnitt erfahren Sie, wie Sie eine Verbindung zwischen Azure Active Directory und der Pingboard-API zur Bereitstellung von Benutzerkonten herstellen und den Bereitstellungsdienst für das Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in Pingboard basierend auf der Benutzerzuweisung in Azure Active Directory konfigurieren.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Pingboard aktivieren. Befolgen Sie hierzu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-active-directory"></a>So konfigurieren Sie die automatische Bereitstellung von Benutzerkonten für Pingboard in Azure Active Directory

1)  Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory > Unternehmens-Apps > Alle Anwendungen**.

2) Suchen Sie über das Suchfeld nach Ihrer Pingboard-Instanz, wenn Sie Pingboard bereits für einmaliges Anmelden konfiguriert haben. Wählen Sie andernfalls **Hinzufügen**, und suchen Sie im Anwendungskatalog nach **Pingboard**. Wählen Sie **Pingboard** in den Suchergebnissen aus, und fügen Sie es Ihrer Anwendungsliste hinzu.

3)  Wählen Sie Ihre Pingboard-Instanz aus, und wählen Sie dann die Registerkarte **Bereitstellung**.

4)  Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

![Pingboard-Bereitstellung](./media/active-directory-saas-pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
5) Führen Sie im Abschnitt „Administratoranmeldeinformationen“ die folgenden Schritte aus:

* Geben Sie `https://your_domain.pingboard.com/scim/v2` ins Textfeld **Mandanten-URL** ein, und ersetzen Sie „your_domain“ durch die richtige Domäne.
* Melden Sie sich mit einem Administratorkonto bei [Pingboard](https://pingboard.com/) an.
* Klicken Sie auf „Add-Ons“ > „Integrationen“ > „Azure Active Directory“.
* Klicken Sie auf die Registerkarte „Konfigurieren“ und wählen Sie **Enable user provisioning from Azure** (Benutzerbereitstellung über Azure ermöglichen).
* Kopieren Sie das Token aus dem Feld **OAuth Bearer Token** (OAuth-Bearertoken), und geben Sie es ins Textfeld **Geheimes Token** ein.

6) Klicken Sie im Azure-Portal auf **Verbindung testen**, um sicherzustellen, dass Azure Active Directory eine Verbindung mit Ihrer Pingboard-App herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr Pingboard-Konto über Administratorberechtigungen verfügt, und wiederholen Sie den Schritt **Verbindung testen**.

7) Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das unten gezeigte Kontrollkästchen.

8) Klicken Sie auf **Speichern**. 

9) Wählen Sie im Abschnitt „Zuordnungen“ die Option **Azure Active Directory-Benutzer mit Pingboard synchronisieren**.

10) Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure Active Directory mit Pingboard synchronisiert werden sollen. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Pingboard für Updatevorgänge verwendet werden. Klicken Sie auf die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen. Weitere Informationen finden Sie unter [Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory](active-directory-saas-customizing-attribute-mappings.md).

11) Ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**, um den Azure Active Directory-Bereitstellungsdienst für Pingboard zu aktivieren.

12) Klicken Sie auf **Speichern**, um die Erstsynchronisierung der Benutzer zu starten, die Pingboard zugewiesen sind.

Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 20 Minuten erfolgen, solange der Dienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Bereitstellungsdienst in Ihrer Pingboard-App ausgeführt werden.

Weitere Informationen zum Lesen von Azure Active Directory-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Weitere Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurieren des einmaligen Anmeldens](active-directory-saas-pingboard-tutorial.md)
