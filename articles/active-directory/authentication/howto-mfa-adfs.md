---
title: Schützen von Cloudressourcen mit Azure MFA und AD FS | Microsoft-Dokumentation
description: Auf dieser Seite zur Azure Multi-Factor Authentication werden die ersten Schritte mit Azure MFA und AD FS in der Cloud beschrieben.
services: multi-factor-authentication
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 0927fc67-8090-4fdd-913a-b3cfed3fbe77
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: joflore
ms.openlocfilehash: 3389dcd36eb4012e863335c34d5727c3530e2c61
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Sichern von Cloud-Ressourcen mit Azure Multi-Factor Authentication und AD FS
Wenn Ihre Organisation über einen Verbund mit Azure Active Directory verfügt, können Sie Multi-Factor Authentication oder Active Directory Federation Services (AD FS) zum Sichern von Ressourcen verwenden, auf die über Azure AD zugegriffen wird. Führen Sie die folgenden Verfahren aus, um Azure Active Directory-Ressourcen mit Azure Multi-Factor Authentication oder Active Directory-Verbunddiensten zu sichern.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Schützen von Azure AD-Ressourcen mit AD FS
Zum Schützen Ihrer Cloudressource richten Sie eine Anspruchsregel ein, damit Active Directory-Verbunddienste (AD FS)den multipleauthn-Anspruch ausgibt, wenn ein Benutzer die zweistufige Überprüfung erfolgreich durchführt. Dieser Anspruch wird an Azure AD übergeben. Die Schritte werden im folgenden Verfahren veranschaulicht:


1. Öffnen Sie die AD FS-Verwaltung.
2. Wählen Sie auf der linken Seite die Option **Vertrauensstellungen der vertrauenden Seite**.
3. Klicken Sie mit der rechten Maustaste auf **Microsoft Office 365 Identity Platform**, und wählen Sie **Anspruchsregeln bearbeiten**.

   ![Cloud](./media/howto-mfa-adfs/trustedip1.png)

4. Klicken Sie unter „Ausstellungstransformationsregeln“ auf **Regel hinzufügen**.

   ![Cloud](./media/howto-mfa-adfs/trustedip2.png)

5. Wählen Sie im Assistenten zum Hinzufügen von Transformationsanspruchsregeln im Dropdownmenü die Option **Passthrough oder eingehenden Anspruch filtern**, und klicken Sie auf **Weiter**.

   ![Cloud](./media/howto-mfa-adfs/trustedip3.png)

6. Benennen Sie die Regel. 
7. Wählen Sie **Authentifizierungsmethodenreferenzen** als eingehenden Anspruchstyp aus.
8. Wählen Sie **Durchlauf aller Anspruchswerte**.
    ![Assistent zum Hinzufügen von Transformationsanspruchsregeln ](./media/howto-mfa-adfs/configurewizard.png)
9. Klicken Sie auf **Fertig stellen**. Schließen Sie die AD FS-Verwaltungskonsole.

## <a name="trusted-ips-for-federated-users"></a>Vertrauenswürdige IPs für Partnerbenutzer
Mit vertrauenswürdigen IPs können Administratoren die zweistufige Überprüfung für bestimmte IP-Adressen oder Partnerbenutzer umgehen, deren Anfragen aus dem eigenen Intranet stammen. In den folgenden Abschnitten wird beschrieben, wie Sie vertrauenswürdige IPs für die Azure Multi-Factor Authentication mit Partnerbenutzern konfigurieren und die zweistufige Überprüfung umgehen, wenn eine Anforderung aus dem Intranet eines Partnerbenutzers stammt. Hierzu wird für AD FS die Verwendung eines Passthrough-Elements oder für die Filterung einer Vorlage für einen eingehenden Anspruch mit dem Anspruchstyp „Innerhalb des Unternehmensnetzwerks“ konfiguriert.

In diesem Beispiel wird Office 365 für die Vertrauensstellungen der vertrauenden Seite verwendet.

### <a name="configure-the-ad-fs-claims-rules"></a>Konfigurieren der AD FS-Anspruchsregeln
Als Erstes müssen wir die AD FS-Ansprüche konfigurieren. Erstellen Sie zwei Anspruchsregeln: eine für den Anspruchstyp „Innerhalb des Unternehmensnetzwerks“ und eine weitere zur Aufrechterhaltung der Anmeldung von Benutzern.

1. Öffnen Sie die AD FS-Verwaltung.
2. Wählen Sie auf der linken Seite die Option **Vertrauensstellungen der vertrauenden Seite**.
3. Klicken Sie mit der rechten Maustaste auf **Microsoft Office 365 Identity Platform**, und wählen Sie **Anspruchsregeln bearbeiten**.
   ![Cloud](./media/howto-mfa-adfs/trustedip1.png)
4. Klicken Sie unter „Ausstellungstransformationsregeln“ auf **Regel hinzufügen**.
   ![Cloud](./media/howto-mfa-adfs/trustedip2.png)
5. Wählen Sie im Assistenten zum Hinzufügen von Transformationsanspruchsregeln im Dropdownmenü die Option **Passthrough oder eingehenden Anspruch filtern**, und klicken Sie auf **Weiter**.
   ![Cloud](./media/howto-mfa-adfs/trustedip3.png)
6. Geben Sie der Regel im Feld neben „Anspruchsregelname“ einen Namen. Beispiel: InsideCorpNet.
7. Wählen Sie in der Dropdownliste neben „Eingehender Anspruchstyp“ die Option **Innerhalb des Unternehmensnetzwerks**.
   ![Cloud](./media/howto-mfa-adfs/trustedip4.png)
8. Klicken Sie auf **Fertig stellen**.
9. Klicken Sie unter „Ausstellungstransformationsregeln“ auf **Regel hinzufügen**.
10. Wählen Sie im Assistenten zum Hinzufügen von Transformationsanspruchsregeln in der Dropdownliste die Option **Ansprüche mit benutzerdefinierter Regel senden**, und klicken Sie auf **Weiter**.
11. Geben Sie im Feld unter „Anspruchsregelname:“ den Text *Benutzeranmeldung aufrechterhalten* ein.
12. Geben Sie in das Feld für benutzerdefinierte Regeln Folgendes ein:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Cloud](./media/howto-mfa-adfs/trustedip5.png)
13. Klicken Sie auf **Fertig stellen**.
14. Klicken Sie auf **Anwenden**.
15. Klicken Sie auf **OK**.
16. Schließen Sie die AD FS-Verwaltung.

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Konfigurieren vertrauenswürdiger IPs der Azure Multi-Factor Authentication mit Partnerbenutzern
Da die Ansprüche jetzt vorhanden sind, können wir vertrauenswürdige IPs konfigurieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.com) an.
2. Wählen Sie **Azure Active Directory** > **Bedingter Zugriff** > **Benannte Orte**.
3. Wählen Sie auf dem Blatt **Bedingter Zugriff – Benannte Orte** die Option **Durch MFA bestätigte IPs konfigurieren**.

   ![Bedingter Azure AD-Zugriff – Benannte Orte – Durch MFA bestätigte IPs konfigurieren](./media/howto-mfa-adfs/trustedip6.png)

4. Wählen Sie auf der Seite „Diensteinstellungen“ unter **Vertrauenswürdige IPs** die Option **Für Anforderungen von Partnerbenutzern in meinem Intranet die mehrstufige Authentifizierung überspringen**.  
5. Klicken Sie auf **Speichern**.

Fertig! An diesem Punkt sollten Office 365-Partnerbenutzer nur MFA verwenden müssen, wenn ein Anspruch von außerhalb des Unternehmensintranets stammt.
