---
title: Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory | Microsoft-Dokumentation
description: Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 3b99a82b59cbba22d30a4325e246c308a2042ad5
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136306"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory 

Dieser Artikel gilt nur für die folgenden Geräte: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Weitere Informationen zu Windows 10 und Windows Server 2016 finden Sie unter [Beheben von Problemen mit Geräten unter Windows 10 und Windows Server 2016 mit Hybrideinbindung in Azure Active Directory](device-management-troubleshoot-hybrid-join-windows-current.md).

In diesem Artikel wird vorausgesetzt, dass Sie [Geräte mit Hybrideinbindung in Azure Active Directory konfiguriert haben](device-management-hybrid-azuread-joined-devices-setup.md), um die folgenden Szenarien zu unterstützen:

- Gerätebasierter bedingter Zugriff

- [Unternehmensroaming von Einstellungen](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello for Business](active-directory-azureadjoin-passport-deployment.md) 





Dieser Artikel enthält Anleitungen zur Problembehandlung zum Beheben potenzieller Probleme.  

**Wichtige Informationen:** 

- Die maximale Anzahl von Geräten pro Benutzer ist gerätespezifisch. Beispiel: Wenn sich *jdoe* und *jharnett* auf einem Gerät anmelden, wird für jeden dieser Benutzer eine separate Registrierung (DeviceID) auf der Registerkarte **BENUTZER** erstellt.  

- Bei der anfänglichen Gerätekonfiguration für die Registrierung bzw. den Beitritt von Geräten wird zunächst eine Anmeldung oder Sperren/Entsperren versucht. Es kann eine Verzögerung von bis zu 5 Minuten auftreten, die durch eine Aufgabe der Aufgabenplanung ausgelöst wird. 

- Durch eine Neuinstallation des Betriebssystems oder durch manuelles Aufheben der Registrierungen wird in Azure AD möglicherweise eine neue Registrierung erstellt. Dies kann im Azure-Portal zu mehreren Einträgen auf der Registerkarte mit den Benutzerinformationen führen. 

- Stellen Sie sicher, dass [KB4284842](https://support.microsoft.com/en-us/help/4284842) installiert, falls Sie Windows 7 SP1 oder Windows Server 2008 R2 SP1 verwenden. Dieses Update verhindert zukünftige Authentifizierungsfehler, wenn Kunden den Zugriff auf geschützte Schlüssel nach dem Ändern des Kennworts verlieren.

## <a name="step-1-retrieve-the-registration-status"></a>Schritt 1: Abrufen des Registrierungsstatus 

**So überprüfen Sie den Registrierungsstatus:**  

1. Melden Sie sich mit dem Benutzerkonto an, das eine Hybrideinbindung in Azure AD ausgeführt hat.

2. Öffnen Sie die Eingabeaufforderung als Administrator. 

3. Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i` ein

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
    
    - Der angemeldete Benutzer ist kein Domänenbenutzer (sondern beispielsweise ein lokaler Benutzer). Eine Azure AD-Hybrideinbindung auf Geräten niedriger Ebene wird nur für Domänenbenutzer unterstützt.
    
    - „Autoworkplace.exe“ kann sich nicht unbeaufsichtigt bei Azure AD oder AD FS authentifizieren. Dies ist unter Umständen auf Probleme bei der ausgehenden Netzwerkverbindung mit den Azure AD-URLs zurückzuführen. Möglicherweise ist auch die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) für den Benutzer aktiviert/konfiguriert, und auf dem Verbundserver ist „WIAORMUTLIAUTHN“ nicht konfiguriert. Eine weitere Möglichkeit ist, dass die Seite der Startbereichsermittlung (Home Realm Discovery, HRD) auf eine Benutzerinteraktion wartet, wodurch **autoworkplace.exe** daran gehindert wird, unbeaufsichtigt ein Token abzurufen.
    
    - Ihre Organisation verwendet nahtloses einmaliges Azure AD-Anmelden, `https://autologon.microsoftazuread-sso.com` oder `https://aadg.windows.net.nsatc.net` ist nicht in den IE-Intraneteinstellungen des Geräts vorhanden, und **Updates der Statusleiste über ein Skript zulassen** ist nicht für die Intranetzone aktiviert.

- Ein Kontingent wurde erreicht

    ![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- Der Dienst antwortet nicht 

    ![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Die Statusinformationen finden Sie auch im Ereignisprotokoll unter **Anwendungs- und Dienstprotokoll\Microsoft-Workplace Join**.
  
**Die häufigsten Ursachen für Fehler bei der Azure AD-Hybrideinbindung sind:** 

- Der Computer ist weder mit dem internen Netzwerk Ihrer Organisation noch mit einem VPN mit Verbindung mit Ihrem lokalen AD-Domänencontroller verbunden.

- Sie sind über ein lokales Computerkonto bei ihrem Computer angemeldet. 

- Probleme bei der Dienstkonfiguration: 

  - Der Verbundserver wurde für die Unterstützung von **WIAORMULTIAUTHN** konfiguriert. 

  - Die Gesamtstruktur Ihres Computers hat kein Dienstverbindungspunkt-Objekt, das auf Ihren verifizierten Domänennamen in Azure AD verweist. 

  - Ein Benutzer hat den Grenzwert von Geräten erreicht. 

## <a name="next-steps"></a>Nächste Schritte

Bei Fragen lesen Sie unter [Häufig gestellte Fragen zur Geräteverwaltung](device-management-faq.md) nach.  
