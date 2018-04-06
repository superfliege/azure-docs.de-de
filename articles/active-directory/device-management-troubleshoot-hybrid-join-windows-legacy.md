---
title: Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory | Microsoft-Dokumentation
description: Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 0d21a8848222c4b09723e22d2d51ec43b2154553
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory 

Dieses Thema gilt nur für die folgenden Geräte: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Weitere Informationen zu Windows 10 und Windows Server 2016 finden Sie unter [Beheben von Problemen mit Geräten unter Windows 10 und Windows Server 2016 mit Hybrideinbindung in Azure Active Directory](device-management-troubleshoot-hybrid-join-windows-current.md).

In diesem Thema wird vorausgesetzt, dass Sie [Geräte mit Hybrideinbindung in Azure Active Directory konfiguriert haben](device-management-hybrid-azuread-joined-devices-setup.md), um die folgenden Szenarien zu unterstützen:

- Gerätebasierter bedingter Zugriff

- [Unternehmensroaming von Einstellungen](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello for Business](active-directory-azureadjoin-passport-deployment.md) 





Dieses Thema enthält Anleitungen zur Problembehandlung zum Beheben potenzieller Probleme.  

**Wichtige Informationen:** 

- Die maximale Anzahl von Geräten pro Benutzer ist gerätespezifisch. Beispiel: Wenn *jdoe* und *jharnett* sich auf einem Gerät anmelden, wird für jeden dieser Benutzer eine separate Registrierung (DeviceID) auf der Registerkarte **USER** erstellt.  

- Bei der anfänglichen Gerätekonfiguration für die Registrierung bzw. den Beitritt von Geräten wird zunächst eine Anmeldung oder Sperren/Entsperren versucht. Es kann eine Verzögerung von bis zu 5 Minuten auftreten, die durch eine Aufgabe der Aufgabenplanung ausgelöst wird. 

- Durch eine Neuinstallation des Betriebssystems oder durch manuelles Aufheben der Registrierung und erneutes Registrieren wird in Azure AD möglicherweise eine neue Registrierung erstellt. Dies kann im Azure-Portal zu mehreren Einträgen auf der Registerkarte mit den Benutzerinformationen führen. 

## <a name="step-1-retrieve-the-registration-status"></a>Schritt 1: Abrufen des Registrierungsstatus 

**So überprüfen Sie den Registrierungsstatus:**  

1. Öffnen Sie die Eingabeaufforderung als Administrator. 

2. Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"` ein

Dieser Befehl zeigt ein Dialogfeld an, das Ihnen ausführliche Informationen zum Einbindungsstatus bietet.

![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Schritt 2: Bewerten des Status des Azure AD-Hybridbeitritts 

Wenn die Hybrideinbindung in Azure AD nicht erfolgreich war, enthält das Dialogfeld Informationen zum aufgetretenen Problem.

**Folgende Probleme treten am häufigsten auf:**

- Ein falsch konfigurierter AD FS oder Azure AD

    ![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Sie sind nicht als Domänenbenutzer angemeldet

    ![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)
    
    Dieses Problem kann aus verschiedenen Gründen auftreten:
    
    1. Der angemeldete Benutzer ist kein Domänenbenutzer (sondern beispielsweise ein lokaler Benutzer). Eine Azure AD-Hybrideinbindung auf Geräten niedriger Ebene wird nur für Domänenbenutzer unterstützt.
    
    2. „Autoworkplace.exe“ kann sich aus irgendeinem Grund nicht unbeaufsichtigt bei Azure AD oder AD FS authentifizieren. Einige mögliche Gründe: Probleme bei ausgehenden Netzwerkverbindungen mit Azure AD-URLs (überprüfen Sie die Voraussetzungen) oder MFA ist für den Benutzer aktiviert/konfiguriert, aber WIAORMUTLIAUTHN ist auf dem Verbundserver nicht konfiguriert (überprüfen Sie die Konfigurationsschritte). Eine weitere Möglichkeit ist, dass die Seite der Startbereichsermittlung (Home Realm Discovery, HRD) auf eine Benutzerinteraktion wartet, wodurch „Autoworkplace.exe“ daran gehindert wird, unbeaufsichtigt ein Token abzurufen.
    
    3. Wenn die Organisation das nahtlose einmalige Anmelden von Azure AD verwendet, ist die folgende URL in den IE-Intraneteinstellungen des Geräts nicht vorhanden:
    
       - https://autologon.microsoftazuread-sso.com

    
       Die Einstellung „Aktualisierungen der Statusleiste per Skript zulassen“ muss für die Intranetzone aktiviert sein.

- Ein Kontingent wurde erreicht

    ![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- Der Dienst antwortet nicht 

    ![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Die Statusinformationen finden Sie auch im Ereignisprotokoll unter **Anwendungs- und Dienstprotokoll\Microsoft-Workplace Join**.
  
**Die häufigsten Ursachen für Fehler bei der Azure AD-Hybrideinbindung sind:** 

- Der Computer befindet sich nicht im internen Netzwerk der Organisation oder im VPN und besitzt keine Verbindung mit einem lokalen AD-Domänencontroller.

- Sie sind über ein lokales Computerkonto bei ihrem Computer angemeldet. 

- Probleme bei der Dienstkonfiguration: 

  - Der Verbundserver wurde für die Unterstützung von **WIAORMULTIAUTHN** konfiguriert. 

  - Es gibt kein Dienstverbindungspunkt-Objekt, das auf Ihren verifizierten Domänennamen in Azure AD in der Active Directory-Gesamtstruktur verweist, zu der der Computer gehört.

  - Ein Benutzer hat den Grenzwert von Geräten erreicht. 

## <a name="next-steps"></a>Nächste Schritte

Bei Fragen lesen Sie unter [Häufig gestellte Fragen zur Geräteverwaltung](device-management-faq.md) nach.  
