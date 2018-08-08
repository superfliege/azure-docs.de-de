---
title: Konfigurieren von in Azure Active Directory eingebundenen Hybridgeräten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure Active Directory eingebundene Hybridgeräte konfigurieren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 1aa5c0f259fdb5fce449bc86f9cf293801b4770a
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39368668"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Tutorial: Konfigurieren der Azure Active Directory-Hybrideinbindung für Verbunddomänen

Ähnlich wie ein Benutzer wird ein Gerät zu einer weiteren Identität, die Sie schützen sowie zum Schutz Ihrer Ressourcen jederzeit und überall verwenden möchten. Dieses Ziel können Sie erreichen, indem Sie die Identitäten Ihrer Geräte mit einer der folgenden Methoden in Azure AD bereitstellen:

- Azure AD-Einbindung
- Azure AD-Hybrideinbindung
- Azure AD-Registrierung

Durch das Bereitstellten Ihrer Geräte in Azure AD maximieren Sie die Produktivität Ihrer Benutzer durch einmaliges Anmelden (SSO) für Ihre gesamten Cloud- und lokalen Ressourcen. Gleichzeitig können Sie den Zugriff auf Ihre Cloud- und lokalen Ressourcen durch [bedingten Zugriff](../active-directory-conditional-access-azure-portal.md) sichern.

In diesem Tutorial erfahren Sie, wie die Azure AD-Hybrideinbindung für Geräte konfiguriert wird, die über ADFS verbunden wurden.

> [!div class="checklist"]
> * Konfigurieren einer Azure AD-Hybrideinbindung
> * Aktivieren von kompatiblen Windows-Geräten
> * Überprüfen der Registrierung
> * Problembehandlung


## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie mit Folgendem vertraut sind:

-  [Einführung in die Geräteverwaltung in Azure Active Directory](../device-management-introduction.md)

-  [Planen der Implementierung einer Azure Active Directory-Hybrideinbindung](hybrid-azuread-join-plan.md)



Für die Konfiguration des Szenarios in diesem Tutorials benötigen Sie Folgendes:

- Windows Server 2012 R2 mit AD FS

- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) ab Version 1.1.819.0 
 

Ab Version 1.1.819.0 bietet Azure AD Connect einen Assistenten für die Konfiguration der Azure AD-Hybrideinbindung. Mit dem Assistenten können Sie den Konfigurationsprozess erheblich vereinfachen. Der entsprechende Assistent:

- Konfiguriert die Dienstverbindungspunkte (SCP) für die Geräteregistrierung

- Sichert Ihre vorhandene Azure AD-Vertrauensstellung der vertrauenden Seite

- Aktualisiert die Anspruchsregeln in Ihrer Azure AD-Vertrauensstellung

Die Konfigurationsschritte in diesem Artikel basieren auf diesen Assistenten. Wenn Sie eine ältere Version von Azure AD Connect installiert haben, müssen Sie sie mindestens auf 1.1.819 aktualisieren. Wenn das Installieren der aktuellen Version von Azure AD Connect keine Option für Sie ist, informieren Sie sich über die [manuelle Konfiguration der Geräteregistrierung](../device-management-hybrid-azuread-joined-devices-setup.md).

für die Azure AD-Hybrideinbindung ist erforderlich, dass die Geräte innerhalb des Netzwerks Ihrer Organisation auf die folgenden Microsoft-Ressourcen zugreifen können:  

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- STS Ihrer Organisation (Verbunddomänen)
- https://autologon.microsoftazuread-sso.com (Wenn Sie das nahtlose einmalige Anmelden verwenden oder verwenden möchten)

Wenn Ihre Organisation Internetzugriff über einen ausgehenden Proxy erfordert, können Sie ab Windows 10 1709 Proxyeinstellungen auf dem Computer mithilfe eines Gruppenrichtlinienobjekts (GPO) konfigurieren. Wenn Ihr Computer unter einem älteren Betriebssystem als Windows 10 1709 ausgeführt wird, müssen Sie den Web Proxy Auto-Discovery-Dienst (WPAD) implementieren, um Windows 10-Computer für die Geräteregistrierung bei Azure AD zu aktivieren. 

Wenn Ihre Organisation Internetzugriff über einen authentifizierten ausgehenden Proxy erfordert, müssen Sie sicherstellen, dass Ihre Windows 10-Computer erfolgreich beim ausgehenden Proxy authentifiziert werden können. Da Windows 10-Computer die Geräteregistrierung mithilfe von Computerkontext ausführen, muss die Authentifizierung bei ausgehenden Proxys mithilfe von Computerkontext konfiguriert werden. Erkunden Sie sich beim Anbieter Ihres ausgehenden Proxys nach den Konfigurationsanforderungen. 


## <a name="configure-hybrid-azure-ad-join"></a>Konfigurieren einer Azure AD-Hybrideinbindung

Zum Konfigurieren einer Azure AD-Hybrideinbindung mithilfe von Azure AD Connect benötigen Sie Folgendes:

- Die Anmeldeinformationen eines globalen Administrators für Ihren Azure AD-Mandanten.  

- Die Anmeldeinformationen eines Unternehmensadministrators für jede Gesamtstruktur.

- Die Anmeldeinformationen Ihres AD FS-Administrators. 


**Konfigurieren einer Azure AD-Hybrideinbindung mithilfe von Azure AD Connect:**

1. Starten Sie Azure AD Connect, und klicken Sie dann auf **Konfigurieren**.

    ![Willkommen](./media/hybrid-azuread-join-federated-domains/11.png)

2. Wählen Sie auf der Seite **Zusätzliche Aufgaben** die Option **Geräteoptionen konfigurieren**, und klicken Sie dann auf **Weiter**. 

    ![Zusätzliche Aufgaben](./media/hybrid-azuread-join-federated-domains/12.png)

3. Klicken Sie auf der Seite **Übersicht** auf **Weiter**. 

    ![Übersicht](./media/hybrid-azuread-join-federated-domains/13.png)

4. Geben Sie auf der Seite **Mit Azure AD verbinden** die Anmeldeinformationen eines globalen Administrators für Ihren Azure AD-Mandanten ein und klicken Sie auf **Weiter**.   

    ![Stellen Sie eine Verbindung mit Azure AD her.](./media/hybrid-azuread-join-federated-domains/14.png)

5. Wählen Sie auf der Seite **Geräteoptionen** die Option **Konfigurieren einer Azure AD-Hybrideinbindung** aus, und klicken Sie dann auf **Weiter**. 

    ![Geräteoptionen](./media/hybrid-azuread-join-federated-domains/15.png)

6. Führen Sie auf der Seite **SCP** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**: 

    ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

    a. Wählen Sie die Gesamtstruktur aus.

    b. Wählen Sie den Authentifizierungsdienst aus.

    c. Klicken Sie auf **Hinzufügen**, um die Anmeldeinformationen eines Unternehmensadministrators einzugeben.


7. Wählen Sie auf der Seite **Gerätebetriebssysteme** die von den Geräten in Ihrer Active Directory-Umgebung verwendeten Betriebssysteme aus, und klicken Sie dann auf **Weiter**. 

    ![Gerätebetriebssystem](./media/hybrid-azuread-join-federated-domains/17.png)

8. Geben Sie auf der Seite **Verbundkonfiguration** die Anmeldeinformationen Ihres AD FS-Administrators ein, und klicken Sie dann auf **Weiter**. 

    ![Verbundkonfiguration](./media/hybrid-azuread-join-federated-domains/18.png)

9. Klicken Sie auf der Seite **Bereit zur Konfiguration** auf **Konfigurieren**. 

    ![Bereit zur Konfiguration](./media/hybrid-azuread-join-federated-domains/19.png)

10. Klicken Sie auf der Seite **Konfiguration abgeschlossen** auf **Beenden**. 

    ![Konfiguration abgeschlossen](./media/hybrid-azuread-join-federated-domains/20.png)




## <a name="enable-windows-down-level-devices"></a>Aktivieren von kompatiblen Windows-Geräten

Wenn es sich bei einigen Ihrer in die Domäne eingebundenen Geräte um kompatible Windows-Geräte handelt, gehen Sie wie folgt vor:

- Geräteeinstellungen aktualisieren
 
- Konfigurieren der lokalen Intraneteinstellungen für die Geräteregistrierung


### <a name="update-device-settings"></a>Geräteeinstellungen aktualisieren 

Zum Registrieren von kompatiblen Windows-Geräten müssen Sie sicherstellen, dass die Geräteeinstellungen festgelegt sind, mit der Benutzer Geräte in Azure AD registrieren können. Im Azure-Portal finden Sie diese Einstellung unter:

`Home > [Name of your tenant] > Devices - Device settings`  


    
Die folgende Richtlinie muss auf **Alle** festgelegt sein: **Benutzer dürfen ihre Geräte für Azure AD registrieren**

![Registrieren von Geräten](./media/hybrid-azuread-join-federated-domains/23.png)


### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Konfigurieren der lokalen Intraneteinstellungen für die Geräteregistrierung

Um die Azure AD-Hybrideinbindung Ihrer kompatiblen Windows-Geräte erfolgreich abzuschließen und Zertifikataufforderungen bei der Authentifizierung von Geräten bei Azure AD zu vermeiden, können Sie eine Richtlinie auf Ihre in die Domäne eingebundenen Geräte übertragen, um die folgende URL in Internet Explorer den lokalen Intranetzonen hinzuzufügen:

- `https://device.login.microsoftonline.com`

- `https://device.login.microsoftonline.com`

- Der Sicherheitstokendienst Ihrer Organisation (STS – Verbunddomänen)

- `https://autologon.microsoftazuread-sso.com` (für nahtloses einmaliges Anmelden).

Außerdem müssen Sie in der lokalen Intranetzone des Benutzers die Option **Aktualisierungen der Statusleiste per Skript zulassen** aktivieren.



## <a name="verify-the-registration"></a>Überprüfen der Registrierung

Zum Überprüfen des Geräteregistrierungsstatus in Ihrem Azure-Mandanten können Sie das Cmdlet **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** im **[Azure Active Directory PowerShell-Modul](/powershell/azure/install-msonlinev1?view=azureadps-2.0)** verwenden.

Bei Verwendung des Cmdlets **Get-MSolDevice** zur Überprüfung der Dienstdetails:

- Ein Objekt mit der **Geräte-ID**, die der ID auf dem Windows-Client entspricht, muss vorhanden sein.
- Der Wert für **DeviceTrustType** muss **Domänenbeitritt** sein. Dies entspricht dem Status **Hybrid in Azure AD eingebunden** auf der Seite „Geräte“ im Azure AD-Portal.
- Der Wert für **Aktiviert** muss für Geräte, die für den bedingten Zugriff verwendet werden, **True** sein. 


**So überprüfen Sie die Dienstdetails:**

1. Öffnen Sie **Windows PowerShell** als Administrator.

2. Geben Sie `Connect-MsolService` ein, um die Verbindung mit Ihrem Azure-Mandanten herzustellen.  

3. Geben Sie `get-msoldevice -deviceId <deviceId>`ein.

6. Vergewissern Sie sich, dass **Aktiviert** auf **True** festgelegt ist.





## <a name="troubleshoot-your-implementation"></a>Problembehandlung bei der Implementierung

Wenn bei der Azure AD-Hybrideinbindung für in Domänen eingebundene Windows-Geräte Probleme auftreten, finden Sie weitere Informationen unter:

- [Problembehandlung für in Azure AD eingebundene aktuelle Windows-Hybridgeräte](../device-management-troubleshoot-hybrid-join-windows-current.md)
- [Problembehandlung für in Azure AD eingebundene kompatible Windows-Hybridgeräte](../device-management-troubleshoot-hybrid-join-windows-legacy.md)



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfigurieren der Azure Active Directory-Hybrideinbindung für verwaltete Domänen](hybrid-azuread-join-managed-domains.md)
> [Manuelles Konfigurieren der Azure Active Directory-Hybrideinbindung](../device-management-hybrid-azuread-joined-devices-setup.md)




<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
