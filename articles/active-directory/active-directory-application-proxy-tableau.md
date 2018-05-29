---
title: Azure Active Directory-Anwendungsproxy und Tableau | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie mithilfe des Azure Active Directory-Anwendungsproxys (Azure AD) Remotezugriff für Ihre Tableau-Bereitstellung zur Verfügung gestellt werden kann.  zu erstellen und zu verwalten.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7fa8654e413ac337994d35afdce7bd3478127f64
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34070871"
---
# <a name="azure-active-directory-application-proxy-and-tableau"></a>Azure Active Directory-Anwendungsproxy und Tableau 

Durch die Partnerschaft von Azure Active Directory-Anwendungsproxy und Tableau soll sichergestellt werden, dass Sie mit dem Anwendungsproxy problemlos Remotezugriff für Ihre Tableau-Bereitstellung bereitstellen können. In diesem Artikel wird das Konfigurieren dieses Szenarios erläutert.  

## <a name="prerequisites"></a>Voraussetzungen 

Bei dem Szenario in diesem Artikel wird davon ausgegangen, dass folgende Voraussetzung erfüllt sind:

- [Tableau](https://onlinehelp.tableau.com/current/server/en-us/proxy.htm#azure) ist konfiguriert. 

- Ein [Anwendungsproxyconnector](manage-apps/application-proxy-enable.md) wurde installiert. 

 

## <a name="enabling-application-proxy-for-tableau"></a>Aktivieren des Anwendungsproxys für Tableau 

Wenn Sie den Anwendungsproxy für Tableau verwenden möchten, müssen Sie eine E-Mail an [aadapfeedback@microsoft.com](mailto:aadapfeedback@microsoft.com) senden, um die Aktivierung dieses Szenarios zu veranlassen.
Geben Sie in Ihrer E-Mail Folgendes an:

-   „Aktivierung des Anwendungsproxys für Tableau“ als Betreff
-   Ihre Mandanten-ID im Text    

Sie erhalten eine Bestätigung, sobald Sie die Anwendung verwenden können. In der Regel dauert dies ungefähr eine Woche. Während Sie auf die Bestätigung warten, können Sie jedoch Konfigurationen fertigstellen.


 

## <a name="publish-your-applications-in-azure"></a>Veröffentlichen Ihrer Anwendungen in Azure 

Um Tableau zu veröffentlichen, müssen Sie eine Anwendung im Azure-Portal veröffentlichen.

Für:

- Ausführliche Anweisungen zu den Schritten 1-8 finden Sie unter [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](manage-apps/application-proxy-publish-azure-portal.md). 
- Wie Sie Tableau-Werte für die Felder des App-Proxys ermitteln, erfahren Sie in der Dokumentation zu Tableau.  

**So veröffentlichen Sie Ihre App** 


1. Melden Sie sich als globaler Administrator beim [Azure-Portal](https://portal.azure.com) an. 

2. Wählen Sie **Azure Active Directory > Unternehmensanwendungen** aus. 

3. Wählen Sie oben auf dem Blatt **Hinzufügen** aus. 

4. Wählen Sie **Lokale Anwendung** aus. 

5. Füllen Sie die Pflichtfelder mit Informationen zur neuen App aus. Befolgen Sie diese Anleitung für die folgenden Einstellungen: 

    - **Interne URL**: Diese Anwendung muss über eine interne URL verfügen, die die Tableau-URL selbst ist. Beispiel: `https://adventure-works.tableau.com`. 

    - **Methode für die Vorauthentifizierung**: Azure Active Directory (empfohlen, aber nicht erforderlich). 

6. Wählen Sie oben auf dem Blatt **Hinzufügen** aus. Ihre Anwendung wird hinzugefügt, das Schnellstartmenü wird geöffnet. 

7. Wählen Sie im Schnellstartmenü **Zuweisen eines Benutzers zu Testzwecken** aus, und fügen Sie der Anwendung mindestens einen Benutzer hinzu. Stellen Sie sicher, dass dieses Testkonto auf die lokale Anwendung zugreifen kann. 

8. Wählen Sie **Zuweisen** aus, um die Zuweisung des Testbenutzers zu speichern. 

9. (Optional) Wählen Sie auf der Seite „App-Verwaltung“ die Option **Einmaliges Anmelden** aus. Wählen Sie **Integrierte Windows-Authentifizierung** im Dropdownmenü aus, und füllen Sie die erforderlichen Felder auf der Grundlage Ihrer Tableau-Konfiguration aus. Wählen Sie **Speichern**aus. 

 

## <a name="testing"></a>Testen 

Ihre Anwendung kann jetzt getestet werden. Greifen Sie auf die externe URL zu, die Sie zum Veröffentlichen von Tableau verwendet haben, und melden Sie sich als Benutzer an, der beiden Anwendungen zugewiesen ist.



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD-Anwendungsproxy finden Sie unter [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](manage-apps/application-proxy.md).

