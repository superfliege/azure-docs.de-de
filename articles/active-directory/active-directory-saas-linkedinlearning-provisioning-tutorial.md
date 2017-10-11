---
title: "Lernprogramm: LinkedIn Learning für die automatische benutzerbereitstellung in Azure Active Directory konfigurieren | Microsoft Docs"
description: Informationen Sie zum Konfigurieren von Azure Active Directory automatisch bereitzustellen und deren Bereitstellung Benutzerkonten LinkedIn lernen.
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/15/2017
ms.author: asmalser-msft
ms.openlocfilehash: 5eb2b1594eedb2a135d7b8cd501a33d8264e136b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-linkedin-learning-for-automatic-user-provisioning"></a>Lernprogramm: Konfigurieren von LinkedIn Learning für die automatische Benutzerbereitstellung


Dieses Lernprogramm wird zur Darstellung der Schritte zum Ausführen in LinkedIn Learning und Azure AD, bereitzustellen und zu deren Bereitstellung aufheben Benutzerkonten aus Azure AD mit LinkedIn Learning benötigten. 

## <a name="prerequisites"></a>Voraussetzungen

In diesem Lernprogramm beschriebene Szenario wird davon ausgegangen, dass Sie bereits über Folgendes verfügen:

*   Azure Active Directory-Mandanten
*   Ein Mandant LinkedIn Learning 
*   Ein Administratorkonto im LinkedIn Learning mit Zugriff auf das Kontocenter LinkedIn

> [!NOTE]
> Azure Active Directory integriert mit LinkedIn Learning mithilfe der [SCIM](http://www.simplecloud.info/) Protokoll.

## <a name="assigning-users-to-linkedin-learning"></a>Zuweisen von Benutzern zu LinkedIn Learning

Azure Active Directory verwendet ein Konzept "Zuweisungen" aufgerufen, um zu bestimmen, welche Benutzer Zugriff auf ausgewählte apps erhalten sollen. Im Kontext der automatischen benutzerbereitstellung-Konto werden nur die Benutzer und Gruppen, die "zu einer Anwendung in Azure AD zugewiesen wurden" synchronisiert. 

Vor dem Konfigurieren und Aktivieren der Bereitstellung des Diensts, müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD den Benutzer darstellen, die Zugriff auf LinkedIn Learning benötigen. Nachdem entschieden, können Sie diese LinkedIn Learning Zuweisen von Benutzern zu anhand der Anweisungen hier:

[Weisen Sie einen Benutzer oder eine Gruppe zu einer Unternehmens-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-learning"></a>Wichtige Tipps zum Zuweisen von Benutzern zu LinkedIn Learning

*   Es wird empfohlen, ein einzelnes Azure AD-Benutzers LinkedIn Learning zum Testen der Konfiguration der Bereitstellung zugewiesen werden. Später können zusätzliche Benutzer und/oder Gruppen zugewiesen werden.

*   Beim Zuweisen eines Benutzers zu LinkedIn lernen müssen, wählen die **Benutzer** Rolle im Dialogfeld "Zuordnung". Die Rolle "Standardzugriffs" funktioniert nicht für die Bereitstellung.


## <a name="configuring-user-provisioning-to-linkedin-learning"></a>Konfiguration der benutzerbereitstellung LinkedIn lernen

Dieser Abschnitt führt Sie durch die Verbindung mit Ihrem Azure AD-API-Bereitstellung LinkedIn-Learning-SCIM-Benutzerkonto, und Konfigurieren des Bereitstellung-Diensts zum Erstellen, aktualisieren und Deaktivieren von Benutzerkonten in LinkedIn Learning basierend auf Benutzer und gruppenzuweisung in Azure AD zugewiesen.

> [!TIP]
> Wahlweise kann auch für LinkedIn Learning SAML-basierten einmaliges Anmelden aktiviert, gemäß der Anweisungen [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese zwei Funktionen ergänzen sich gegenseitig.


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-learning-in-azure-ad"></a>So konfigurieren Sie die automatische kontobereitstellung in LinkedIn Learning in Azure AD:


Der erste Schritt ist das Zugriffstoken LinkedIn abrufen. Wenn Sie ein Enterprise-Administrator sind, können Sie ein Zugriffstoken selbst bereitstellen. Wechseln Sie in Ihrem Kontocenter zu **Einstellungen &gt; globale Einstellungen** , und öffnen Sie die **SCIM Setup** Bereich.

> [!NOTE]
> Wenn Sie das Kontocenter direkt statt über einen Link zugreifen, können Sie es mithilfe der folgenden Schritte erreichen.

1)  Melden Sie sich am Center-Konto an.

2)  Wählen Sie **Admin &gt; Administratoreinstellungen** .

3)  Klicken Sie auf **erweiterte Integrationen** auf der linken Randleiste. Sie werden an das Kontocenter weitergeleitet.

4)  Klicken Sie auf **+ Add neue SCIM Konfiguration** und führen Sie das Verfahren durch Ausfüllen der jedes Feld.

> Wenn die automatische Zuweisung von Lizenzen nicht aktiviert ist, bedeutet dies, dass nur Benutzerdaten synchronisiert werden.

![LinkedIn Learning-Bereitstellung](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_1.PNG)

> Wenn Autolicense Zuweisung aktiviert ist, müssen Sie die Anwendungsinstanz und Lizenztyp zu beachten. Auf einer Lizenzen zugewiesen ist, zunächst Grundlage dienen, bis alle Lizenzen durchgeführt werden.

![LinkedIn Learning-Bereitstellung](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_2.PNG)

5)  Klicken Sie auf **generieren Token**. Daraufhin sollte das token Display-Zugriff unter der **Zugriffstoken** Feld.

6)  Speichern Sie das Zugriffstoken in der Zwischenablage oder des Computers vor dem Verlassen der Seite.

7) Als Nächstes melden Sie sich bei der [Azure-Portal](https://portal.azure.com), und navigieren Sie zu der **Azure Active Directory > Unternehmens-Apps > Alle Programme** Abschnitt.

8) Wenn Sie bereits LinkedIn Learning für einmaliges Anmelden konfiguriert haben, suchen Sie nach Ihrer Instanz von LinkedIn Learning mithilfe des Felds suchen. Wählen Sie andernfalls **hinzufügen** , suchen Sie nach **LinkedIn Learning** im Anwendungskatalog. Wählen Sie LinkedIn Learning in den Suchergebnissen, und fügen Sie es in der Liste der Anwendungen.

9)  Wählen Sie die Instanz von LinkedIn lernen, und wählen Sie dann die **Provisioning** Registerkarte.

10) Legen Sie die **Bereitstellungsmodus** auf **automatische**.

![LinkedIn Learning-Bereitstellung](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_3.PNG)

11)  Füllen Sie die folgenden Felder unter **Administratoranmeldeinformationen** :

* In der **-Mandanten-URL** geben https://api.linkedin.com.

* In der **geheime Sicherheitstoken** Feld, geben Sie das Zugriffstoken, die Sie in Schritt 1 generiert, und klicken Sie auf **Testverbindung** .

* Eine Erfolgsmeldung sollte auf den oberen Rand Ihres Portals angezeigt werden.

12) Geben Sie die e-Mail-Adresse einer Person oder Gruppe, die in der Bereitstellung fehlerbenachrichtigungen erhalten sollen die **e-Mail-Benachrichtigung** ein, und aktivieren Sie das Kontrollkästchen unten.

13) Klicken Sie auf **Speichern**. 

14) In der **Attributzuordnungen** Abschnitt, überprüfen Sie die Benutzer- und Attribute, die von Azure AD lernen LinkedIn synchronisiert werden. Beachten Sie, das die Attribute als ausgewählt **Abgleich** Eigenschaften werden verwendet, um die Benutzerkonten und Gruppen in LinkedIn Learning für Updatevorgänge übereinstimmen. Wählen Sie die Schaltfläche "Speichern", um alle Änderungen zu übernehmen.

![LinkedIn Learning-Bereitstellung](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_4.PNG)

15) Um den Azure AD-Dienst für das Lernen LinkedIn-Bereitstellung zu aktivieren, Ändern der **Status Bereitstellung** auf **auf** in der **Einstellungen** Abschnitt

16) Klicken Sie auf **Speichern**. 

Dadurch wird die erstsynchronisierung alle Benutzer und/oder Gruppen LinkedIn Learning im Abschnitt Benutzer und Gruppen zugewiesen werden gestartet. Beachten Sie, dass die erste Synchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr 20 Minuten auftreten, solange der Dienst ausgeführt wird. Sie können die **Synchronisierungsdetails** Abschnitt aus, um den Fortschritt überwachen sowie folgen Links auf die Bereitstellung der Berichte, die alle von der Bereitstellung-Dienst in Ihrer app LinkedIn lernen ausgeführten Aktionen beschreiben.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten von Konto benutzerbereitstellung für Unternehmens-Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Was ist Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory?](active-directory-appssoaccess-whatis.md)
