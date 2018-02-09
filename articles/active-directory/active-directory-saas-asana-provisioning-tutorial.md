---
title: "Tutorial: Konfigurieren von Asana für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Asana konfigurieren."
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
ms.date: 01/26/2018
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 4f3bd11a99f43d6405ea285a7a283179d561f92a
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Asana für die automatische Benutzerbereitstellung

Dieses Tutorial zeigt Ihnen die Schritte, die Sie in Asana und Azure AD ausführen müssen, um Benutzerkonten von Azure AD in Asana automatisch bereitzustellen bzw. deren Bereitstellung automatisch aufzuheben.

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure Active Directory-Mandanten
*   Einen Asana-Mandanten, für den mindestens der [Enterprise-Tarif](https://www.asana.com/pricing) aktiviert ist 
*   Ein Benutzerkonto in Asana mit Administratorberechtigungen 

> [!NOTE] 
> Die Azure AD-Bereitstellungsintegration basiert auf der [Asana-API](https://app.asana.com/api/1.0/scim/Users), die für Asana verfügbar ist.

## <a name="assigning-users-to-asana"></a>Zuweisen von Benutzern zu Asana

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden. 

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer in Azure AD die Benutzer darstellen, die Zugriff auf Ihre Asana-App benötigen. Anschließend können Sie diese Benutzer Ihrer Asana-App zuweisen, indem Sie die folgenden Anweisungen befolgen:

[Zuweisen eines Benutzers zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Asana

*   Es wird empfohlen, Asana einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können weitere Benutzer zugewiesen werden.

## <a name="configuring-user-provisioning-to-asana"></a>Konfigurieren der Benutzerbereitstellung in Asana 

In diesem Abschnitt erfahren Sie, wie Sie eine Verbindung zwischen Azure AD und der Asana-API zur Bereitstellung von Benutzerkonten herstellen und den Bereitstellungsdienst für das Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in Asana basierend auf der Benutzerzuweisung in Azure AD konfigurieren.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Asana aktivieren. Befolgen Sie hierzu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>So konfigurieren Sie die automatische Bereitstellung von Benutzerkonten für Asana in Azure AD:

1)  Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory > Unternehmens-Apps > Alle Anwendungen**.

2) Wenn Sie Asana bereits für einmaliges Anmelden konfiguriert haben, suchen Sie über das Suchfeld nach Ihrer Asana-Instanz. Wählen Sie andernfalls **Hinzufügen**, und suchen Sie im Anwendungskatalog nach **Asana**. Wählen Sie **Asana** in den Suchergebnissen aus, und fügen Sie es Ihrer Anwendungsliste hinzu.

3)  Wählen Sie Ihre Asana-Instanz aus, und wählen Sie dann die Registerkarte **Bereitstellung**.

4)  Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

![Bereitstellen von Asana](./media/active-directory-saas-asana-provisioning-tutorial/asanaazureprovisioning.png)

5) Befolgen Sie im Abschnitt „Administratoranmeldeinformationen“ die folgenden Anweisungen, um das Token zu generieren, und geben Sie es im Textfeld **Geheimes Token** ein.

* Melden Sie sich mit dem Administratorkonto bei [Asana](https://app.asana.com) an.
* Klicken Sie auf der oberen Leiste auf das Profilfoto, und wählen Sie die aktuellen Einstellungen für den Organisationsnamen aus.
* Navigieren Sie zur Registerkarte „Service Accounts“ (Dienstkonten).
* Klicken Sie auf „Add Service Account“ (Dienstkonto hinzufügen).
* Aktualisieren Sie ggf. Name, Informationen und Profilfoto, kopieren Sie das **Token**, und klicken Sie auf „Save Changes“ (Änderungen speichern).

6) Klicken Sie im Azure-Portal auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Ihrer Asana-App herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr Asana-Konto über Administratorberechtigungen verfügt, und wiederholen Sie den Schritt **Verbindung testen**.

7) Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das unten gezeigte Kontrollkästchen.

8) Klicken Sie auf **Speichern**. 

9) Wählen Sie im Abschnitt „Zuordnungen“ die Option **Azure Active Directory-Benutzer mit Asana synchronisieren**.

10) Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Asana synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Asana für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen. Ausführliche Informationen finden Sie unter [Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory](active-directory-saas-customizing-attribute-mappings.md).

11) Um den Azure AD-Bereitstellungsdienst für Asana zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

12) Klicken Sie auf **Speichern**. 

Dadurch wird die Erstsynchronisierung aller Benutzer gestartet, die Asana im Abschnitt „Benutzer“ zugewiesen sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 20 Minuten erfolgen, solange der Dienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Bereitstellungsdienst in Ihrer Asana-App ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Weitere Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurieren des einmaligen Anmeldens](active-directory-saas-asana-tutorial.md)
