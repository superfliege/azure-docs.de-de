---
title: Verwalten von Verbundzertifikaten in Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Ablaufdatum für Verbundzertifikate anpassen und Zertifikate erneuern, die in Kürze ablaufen.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: barbkess
ms.reviewer: jeedes
ms.openlocfilehash: 0f6e690bc80ae8004fba4faf53c0403b0cb7edd9
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51035335"
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Verwalten von Zertifikaten für die einmalige Verbundanmeldung in Azure Active Directory
In diesem Artikel werden häufige gestellte Fragen im Zusammenhang mit den Zertifikaten behandelt, die Azure Active Directory (Azure AD) erstellt, um die einmalige Verbundanmeldung (Single Sign-On, SSO) für Ihre SaaS-Anwendungen einzurichten. Fügen Sie Anwendungen aus dem Azure AD-App-Katalog oder über Anwendungsvorlagen hinzu, die nicht aus dem Katalog stammen. Konfigurieren der Anwendung mit der Option für Verbund-SSO.

Dieser Artikel ist für Apps relevant, die über einen SAML-Verbund für Azure AD-SSO konfiguriert sind, wie im folgenden Beispiel gezeigt:

![Azure AD – einmaliges Anmelden](./media/manage-certificates-for-federated-single-sign-on/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Automatisch generiertes Zertifikat für Katalog- und Nicht-Kataloganwendungen
Wenn Sie eine neue Anwendung aus dem Katalog hinzufügen und die SAML-basierte Anmeldung konfigurieren, generiert Azure AD ein drei Jahre lang gültiges Zertifikat für die Anwendung. Sie können dieses Zertifikat aus dem Abschnitt **SAML-Signaturzertifikat** herunterladen. Bei Kataloganwendungen wird in diesem Abschnitt möglicherweise eine Option angezeigt, über die Sie je nach Anforderung der Anwendung das Zertifikat oder Metadaten herunterladen können.

![Azure AD-SSO](./media/manage-certificates-for-federated-single-sign-on/saml_certificate_download.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>Anpassen des Ablaufdatums für das Verbundzertifikat und Rollover zu einem neuen Zertifikat
Standardmäßig wird für Zertifikate eine Gültigkeitsdauer von drei Jahren festgelegt. Führen Sie die folgenden Schritte aus, um ein anderes Ablaufdatum für das Zertifikat auszuwählen.
In den Screenshots wurde Salesforce als Beispiel verwendet, diese Schritte können jedoch für jede beliebige SaaS-Verbund-App verwendet werden.

1. Klicken Sie im [Azure-Portal](https://aad.portal.azure.com) im linken Bereich auf **Unternehmensanwendung** und dann auf der Seite **Übersicht** auf **Neue Anwendung**:

   ![Öffnen des SSO-Konfigurations-Assistenten](./media/manage-certificates-for-federated-single-sign-on/enterprise_application_new_application.png)

2. Suchen Sie nach der Kataloganwendung, und wählen Sie die Anwendung aus, die Sie hinzufügen möchten. Wenn Sie die gewünschte Anwendung nicht finden können, fügen Sie die Anwendung über die Option **Nicht-Kataloganwendung** hinzu. Dieses Feature ist nur in der SKU Azure AD Premium (P1 und P2) verfügbar.

    ![Azure AD-SSO](./media/manage-certificates-for-federated-single-sign-on/add_gallery_application.png)

3. Klicken Sie im linken Bereich auf den Link **Einmaliges Anmelden**, und ändern Sie den **Modus** in **SAML-basierte Anmeldung**. In diesem Schritt wird das drei Jahre lang gültige Zertifikat für Ihre Anwendung generiert.

4. Um ein neues Zertifikat zu erstellen, klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf den Link **Neues Zertifikat erstellen**.

    ![Neues Zertifikat generieren](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

5. Durch Klicken auf den Link **Neues Zertifikat erstellen** wird das Kalendersteuerelement geöffnet. Sie können ein beliebiges Datum inklusive Uhrzeit in einem Zeitraum von bis zu drei Jahren festlegen. Der ausgewählte Wert für Datum und Uhrzeit ist das neue Ablaufdatum mit Uhrzeit für das neue Zertifikat. Klicken Sie auf **Speichern**.

    ![Herunterladen und Hochladen des Zertifikats](./media/manage-certificates-for-federated-single-sign-on/certifcate_date_selection.PNG)

6. Das neue Zertifikat steht jetzt zum Download bereit. Klicken Sie auf den Link **Zertifikat**, um das Zertifikat herunterzuladen. An diesem Punkt ist Ihr Zertifikat noch nicht aktiv. Wenn Sie jetzt einen Rollover zu diesem Zertifikat ausführen möchten, aktivieren Sie das Kontrollkästchen **Als neues Zertifikat festlegen**, und klicken Sie auf **Speichern**. Von diesem Zeitpunkt an verwendet Azure AD das neue Zertifikat zum Signieren der Antwort.

7.  Informationen zum Hochladen des Zertifikats in Ihre spezifische SaaS-Anwendung erhalten Sie unter dem Link **Tutorial zur Anzeige der Anwendungskonfiguration**.

## <a name="certificate-expiration-notification-email"></a>E-Mail-Benachrichtigung zum Zertifikatablauf

Azure AD sendet 60, 30 und 7 Tage vor Ablauf eines SAML-Zertifikats eine Benachrichtigung per E-Mail. So geben Sie die E-Mail-Adresse an, an die die Benachrichtigung gesendet werden soll:

- Navigieren Sie auf der Seite für einmaliges Anmelden der Azure Active Directory-Anwendung zum Feld für die E-Mail-Benachrichtigung.
- Geben Sie die E-Mail-Adresse ein, an die die E-Mail-Benachrichtigung zum Zertifikatablauf gesendet werden soll. Für dieses Feld wird standardmäßig die E-Mail-Adresse des Administrators verwendet, der die Anwendung hinzugefügt hat.

Sie erhalten die Benachrichtigungs-E-Mail von aadnotification@microsoft.com. Um zu verhindern, dass die E-Mail in Ihren Spamordner umgeleitet wird, müssen Sie die E-Mail-Adresse Ihren Kontakten hinzufügen. 

## <a name="renew-a-certificate-that-will-soon-expire"></a>Erneuern eines in Kürze ablaufenden Zertifikats
Die folgenden Schritte zur Erneuerung sollten zu keinen wesentlichen Ausfallzeiten für Ihre Benutzer führen. In den Screenshots in diesem Abschnitt wurde Salesforce als Beispiel verwendet, diese Schritte können jedoch für jede beliebige SaaS-Verbund-App angewendet werden.

1. Generieren Sie auf der Seite **Einmaliges Anmelden** für die **Azure Active Directory**-Anwendung das neue Zertifikat für Ihre Anwendung. Klicken Sie hierzu im Abschnitt **SAML-Signaturzertifikat** auf den Link **Neues Zertifikat erstellen**.

    ![Neues Zertifikat generieren](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

2. Wählen Sie das gewünschte Ablaufdatum inklusive Uhrzeit für das neue Zertifikat aus, und klicken Sie dann auf die Schaltfläche **Speichern**. Indem Sie ein Datum auswählen, das sich mit dem vorhandenen Zertifikat überschneidet, stellen Sie sicher, dass Ausfallzeiten aufgrund von Zertifikatablauf begrenzt sind. 

3. Wenn Sie App ein automatisches Rollover des Zertifikats ausführen kann, legen Sie das neue Zertifikat als „aktiv“ fest.  Melden Sie sich bei der App an, um zu überprüfen, ob sie funktioniert.

4. Wenn die App das neue Zertifikat nicht automatisch auswählt, aber mehr als ein Signaturzertifikat verarbeiten kann, laden Sie das neue Zertifikat in die App hoch, bevor das alte abläuft. Markieren Sie das neue Zertifikat dann im Portal als aktives Zertifikat. 

5. Wenn die App nur jeweils ein Zertifikat verarbeiten kann, wählen Sie ein Zeitfenster für den App-Ausfall aus, und laden Sie das neue Zertifikat herunter. Laden Sie das Zertifikat dann in die App hoch, und markieren Sie es im Azure-Portal als neues Zertifikat. 
   
6. Um das neue Zertifikat in Azure AD zu aktivieren, aktivieren Sie das Kontrollkästchen **Als neues Zertifikat festlegen**, und klicken Sie dann oben auf der Seite auf die Schaltfläche **Speichern**. Hierdurch wird ein Rollover zum neuen Zertifikat in Azure AD durchgeführt. Der Status des Zertifikats ändert sich von **Neu** in **Aktiv**. Von diesem Zeitpunkt an verwendet Azure AD das neue Zertifikat zum Signieren der Antwort. 
   
    ![Neues Zertifikat generieren](./media/manage-certificates-for-federated-single-sign-on/new_certificate_download.png)

## <a name="related-articles"></a>Verwandte Artikel
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](../saas-apps/tutorial-list.md)
* [Anwendungsverwaltung in Azure Active Directory](what-is-application-management.md)
* [Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory](what-is-single-sign-on.md)
* [Problembehandlung bei SAML-basiertem einmaligem Anmelden](../develop/howto-v1-debug-saml-sso-issues.md)
