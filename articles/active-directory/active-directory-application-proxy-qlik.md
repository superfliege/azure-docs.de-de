---
title: Azure AD-App-Proxy und Qlik Sense | Microsoft-Dokumentation
description: Aktivieren Sie den Anwendungsproxy über das Azure-Portal, und installieren Sie die Connectors für den Reverseproxy.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/03/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: e10fefdd3bd46aeb90fd2cfc82d4fee3b17d867b
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="application-proxy-and-qlik-sense"></a>Anwendungsproxy und Qlik Sense 
Die Partnerschaft von Azure Active Directory-Anwendungsproxy und Qlik Sense soll sicherstellen, dass Sie mit dem Anwendungsproxy problemlos Remotezugriff für Ihre Qlik Sense-Bereitstellung bereitstellen können.  

## <a name="prerequisites"></a>Voraussetzungen 
Im weiteren Verlauf dieses Szenarios wird davon ausgegangen, dass Sie Folgendes durchgeführt haben:
 
- [Qlik Sense](https://community.qlik.com/docs/DOC-19822) konfiguriert. 
- [Einen Anwendungsproxyconnector installiert.](active-directory-application-proxy-enable.md#install-and-register-a-connector) 
 
## <a name="publish-your-applications-in-azure"></a>Veröffentlichen Ihrer Anwendungen in Azure 
Um Qlik Sense zu veröffentlichen, müssen Sie zwei Anwendungen in Azure veröffentlichen.  

### <a name="application-1"></a>Anwendung 1: 
Führen Sie diese Schritte aus, um Ihre App zu veröffentlichen. Eine ausführlichere Beschreibung der Schritte 1 bis 8 finden Sie unter [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](application-proxy-publish-azure-portal.md). 


1. Melden Sie sich als globaler Administrator beim Azure-Portal an. 
2. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** aus. 
3. Wählen Sie oben auf dem Blatt **Hinzufügen** aus. 
4. Wählen Sie **Lokale Anwendung** aus. 
5.       Füllen Sie die Pflichtfelder mit Informationen zur neuen App aus. Befolgen Sie diese Anleitung für die folgenden Einstellungen: 
    - **Interne URL**: Diese Anwendung muss über eine interne URL verfügen, die die Qlik Sense-URL selbst ist. Beispiel: **https&#58;//demo.qlikemm.com:4244** 
    - **Methode für die Vorauthentifizierung** : Azure Active Directory (empfohlen, aber nicht erforderlich) 
1.       Klicken Sie unten auf dem Blatt auf **Hinzufügen**. Ihre Anwendung wird hinzugefügt, das Schnellstartmenü wird geöffnet. 
2.       Wählen Sie im Schnellstartmenü **Zuweisen eines Benutzers zu Testzwecken** aus, und fügen Sie der Anwendung mindestens einen Benutzer hinzu. Stellen Sie sicher, dass dieses Testkonto auf die lokale Anwendung zugreifen kann. 
3.       Wählen Sie **Zuweisen** aus, um die Zuweisung des Testbenutzers zu speichern. 
4.       (Optional) Wählen Sie auf dem Blatt „App-Verwaltung“ die Option „Einmaliges Anmelden“ aus. Wählen Sie **Eingeschränkte Kerberos-Delegierung** im Dropdownmenü aus, und füllen Sie die erforderlichen Felder auf der Grundlage Ihrer Qlik-Konfiguration aus. Wählen Sie **Speichern**aus. 

### <a name="application-2"></a>Anwendung 2: 
Führen Sie die gleichen Schritte aus wie für Anwendung 1 – mit folgenden Ausnahmen: 

**Schritt 5**: Die interne URL sollte jetzt die QlikSense-URL mit dem von der Anwendung verwendeten Authentifizierungsport sein. Die Standardeinstellung ist **4244** für HTTPS und 4248 für HTTP. Beispiel: **https&#58;//demo.qlik.com:4244**</br></br> 
**Schritt 10:** Richten Sie nicht SSO ein, und lassen Sie das **einmalige Anmelden deaktiviert**
 
 
## <a name="testing"></a>Testen 
Ihre Anwendung kann jetzt getestet werden. Greifen Sie auf die externe URL zu, die Sie zum Veröffentlichen von Qlik Sense in Anwendung 1 verwendet haben, und melden Sie sich als Benutzer an, der beiden Anwendungen zugewiesen ist.  

## <a name="next-steps"></a>Nächste Schritte

- [Veröffentlichen von Anwendungen mit dem Anwendungsproxy](application-proxy-publish-azure-portal.md)
- [Veröffentlichen von Anwendungen in getrennten Netzwerken und an getrennten Standorten mit Connectorgruppen](active-directory-application-proxy-connectors-azure-portal.md).
