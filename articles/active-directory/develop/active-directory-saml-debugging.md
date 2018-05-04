---
title: Debuggen des SAML-basierten einmaligen Anmeldens bei Anwendungen in Azure Active Directory | Microsoft Docs
description: 'Hier erfahren Sie, wie Sie das SAML-basierte einmalige Anmelden bei Anwendungen in Azure Active Directory debuggen. '
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 55ff6b7a70bcdcceacb1484f9969337f9853ce50
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Debuggen des SAML-basierten einmaligen Anmeldens bei Anwendungen in Azure Active Directory

Beim Debuggen einer SAML-basierten Anwendungsintegration ist es oft hilfreich, ein Tool wie [Fiddler](http://www.telerik.com/fiddler) zu verwenden, um sich die SAML-Anforderung und die SAML-Antwort mit dem SAML-Token anzusehen, das für eine Anwendung ausgestellt wurde. 

![Fiddler][1]

Die Probleme hängen häufig mit einer fehlerhaften Azure Active Directory- oder Anwendungskonfiguration zusammen. Überprüfen Sie je nachdem, wo der Fehler auftritt, entweder die SAML-Anforderung oder die Antwort:

- Der Fehler tritt auf der Anmeldeseite meines Unternehmens auf. [Links zum Abschnitt]
- Der Fehler tritt nach der Anmeldung auf der Seite der Anwendung auf. [Links zum Abschnitt]

## <a name="i-see-an-error-in-my-company-sign-in-page"></a>Der Fehler tritt auf der Anmeldeseite meines Unternehmens auf.

Navigieren Sie zu [Probleme beim Anmelden bei einer Kataloganwendung, die für einmaliges Anmelden im Verbund konfiguriert ist](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML). Dort finden Sie den Fehlercode und die entsprechenden Lösungsschritte.

Wenn der Fehler auf der Anmeldeseite Ihres Unternehmens auftritt, benötigen Sie zum Debuggen des Problems die Fehlermeldung und die SAML-Anforderung.

### <a name="how-can-i-get-the-error--message"></a>Wo finde ich die Fehlermeldung?

Die Fehlermeldung finden Sie rechts unten auf der Seite. Dort wird ein Fehler mit folgenden Informationen angezeigt:

- Korrelations-ID
- Zeitstempel
- Meldung

![Error][2] 

 
Die Korrelations-ID und der Zeitstempel bilden einen eindeutigen Bezeichner, mit dem Sie nach den entsprechenden Back-End-Protokollen für den Anmeldefehler suchen können. Diese Werte werden bei der Erstellung einer Supportanfrage an Microsoft benötigt, um die Lösungsfindung zu beschleunigen.

Die Meldung gibt Aufschluss über die Ursache für das Anmeldeproblem. 


### <a name="how-can-i-review-the-saml-request"></a>Wie kann ich die SAML-Anforderung überprüfen?

Die von der Anwendung an Azure Active Directory gesendete SAML-Anforderung ist in der Regel die letzte HTTP 200-Antwort von [https://login.microsoftonline.com](https://login.microsoftonline.com).
 
Die SAML-Anforderung können Sie mithilfe von Fiddler anzeigen. Markieren Sie hierzu die Zeile, und navigieren Sie anschließend im rechten Bereich zu **Inspectors > WebForms**. Klicken Sie mit der rechten Maustaste auf den Wert **SAMLResponse**, und klicken Sie anschließend auf **Send to TextWizard** (An „TextWizard“ senden). Wählen Sie dann im Menü **Transform** (Transformieren) die Option **From Base64** (Aus Base64) aus, um das Token zu decodieren und seinen Inhalt anzuzeigen. 

Sie können den Wert aus der SAML-Anforderung auch kopieren und einen anderen Base64-Decoder verwenden.

    <samlp:AuthnRequest
    xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
    ID="id6c1c178c166d486687be4aaf5e482730"
    Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
    Destination=https://login.microsoftonline.com/<Tenant-ID>/saml2
    AssertionConsumerServiceURL="https://contoso.applicationname.com/acs"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
    </samlp:AuthnRequest>

Überprüfen Sie in der decodierten SAML-Anforderung Folgendes:

1. Das Ziel (**Destination**) in der SAML-Anforderung muss der URL des SAML-Diensts für einmaliges Anmelden aus Azure Active Directory entsprechen.
 
2. Der Aussteller (**Issuer**) in der SAML Anforderung muss dem **Bezeichner** entsprechen, den Sie für die Anwendung in Azure Active Directory konfiguriert haben. Azure AD sucht anhand des Ausstellers nach einer Anwendung in Ihrem Verzeichnis.

3. **AssertionConsumerServiceURL** gibt an, wo die Anwendung den Empfang des SAML-Tokens von Azure Active Directory erwartet. Sie können diesen Wert in Azure Active Directory konfigurieren, er ist jedoch nicht zwingend erforderlich, wenn er Teil der SAML-Anforderung ist.


## <a name="i-see-an-error-on-the-applications-page-after-signing-in"></a>Der Fehler tritt nach der Anmeldung auf der Seite der Anwendung auf.

In diesem Szenario akzeptiert die Anwendung die von Azure AD gesendete Antwort nicht. Wenden Sie sich mit der Antwort von Azure AD, die das SAML-Token enthält, an den Hersteller der Anwendung, um zu ermitteln, was fehlt oder nicht korrekt ist. 

### <a name="how-can-i-get-and-review-the-saml-response"></a>Wie kann ich die SAML-Antwort abrufen und überprüfen?

Die Azure AD-Antwort mit dem SAML-Token folgt in der Regel auf eine HTTP 302-Umleitung von https://login.microsoftonline.com und wird an die konfigurierte **Antwort-URL** der Anwendung gesendet. 

Sie können das SAML-Token anzeigen, indem Sie diese Zeile auswählen und anschließend im rechten Bereich zur Registerkarte **Inspectors > WebForms** (Inspectors > WebForms) navigieren. Klicken Sie dort mit der rechten Maustaste auf den Wert **SAMLResponse**, und wählen Sie **Send to TextWizard** (An „TextWizard“ senden). Klicken Sie dann im Menü **Transform** (Transformieren) auf **From Base64** (Aus Base64), um das Token zu decodieren und seinen Inhalt anzuzeigen. 

Sie können den Wert aus der **SAML-Anforderung** auch kopieren und einen anderen Base64-Decoder verwenden.

Weitere Informationen zu möglicherweise fehlenden oder falschen Angaben in der SAML-Antwort finden Sie unter [Probleme beim Anmelden bei einer Kataloganwendung, die für einmaliges Anmelden im Verbund konfiguriert ist](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML).

Informationen zum Überprüfen der SAML Antwort finden Sie im Artikel [SAML-Protokoll für einmaliges Anmelden](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML#response).


## <a name="related-articles"></a>Verwandte Artikel
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Anpassen ausgestellter Ansprüche im SAML-Token für vorintegrierte Apps](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png
[2]: ../media/active-directory-saml-debugging/error.png
