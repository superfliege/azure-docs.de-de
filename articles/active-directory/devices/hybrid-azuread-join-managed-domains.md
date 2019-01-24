---
title: Konfigurieren der Azure Active Directory-Hybrideinbindung für verwaltete Domänen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Azure Active Directory-Hybrideinbindung für verwaltete Domänen konfigurieren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/08/2019
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: cf4d142adeff5157a7291e0bac65700bb2e9c8c5
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54446917"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Tutorial: Konfigurieren der Azure Active Directory-Hybrideinbindung für verwaltete Domänen

Ähnlich wie ein Benutzer wird ein Gerät zu einer weiteren Identität, die Sie schützen sowie zum Schutz Ihrer Ressourcen jederzeit und überall verwenden können. Dafür können Sie die Identitäten Ihrer Geräte mit einer der folgenden Methoden in Azure AD bereitstellen:

- Azure AD-Einbindung
- Azure AD-Hybrideinbindung
- Azure AD-Registrierung

Durch das Bereitstellten Ihrer Geräte in Azure AD maximieren Sie die Produktivität Ihrer Benutzer durch einmaliges Anmelden (SSO) für Ihre gesamten Cloud- und lokalen Ressourcen. Gleichzeitig können Sie den Zugriff auf Ihre Cloud- und lokalen Ressourcen durch [bedingten Zugriff](../active-directory-conditional-access-azure-portal.md) sichern.

In diesem Tutorial erfahren Sie, wie die Azure AD-Hybrideinbindung für Geräte in verwalteten Domänen konfiguriert wird.

> [!div class="checklist"]
> * Konfigurieren der Hybrid-Azure AD-Einbindung
> * Aktivieren von kompatiblen Windows-Geräten
> * Überprüfen der eingebundenen Geräte 
> * Problembehandlung 


## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie mit Folgendem vertraut sind:
    
-  [Einführung in die Geräteverwaltung in Azure Active Directory](../device-management-introduction.md)
    
-  [Planen der Implementierung einer Azure Active Directory-Hybrideinbindung](hybrid-azuread-join-plan.md)

-  [Steuern der Azure AD-Hybrideinbindung für Ihre Geräte](hybrid-azuread-join-control.md)
  

Für die Konfiguration des Szenarios in diesem Artikel benötigen Sie Folgendes:

- Ein lokales Active Directory (AD) mit Schemalevel 85 oder höher. Weitere Informationen finden Sie unter [Aktualisieren des Active Directory-Schemas](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-device-based-conditional-access-on-premises#upgrade-your-active-directory-schema).

- Die [neueste Version von Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 oder höher) muss installiert sein. 

Vergewissern Sie sich, dass Azure AD die Computerobjekte der Geräte für die Azure AD-Hybrideinbindung mit Azure AD synchronisiert. Wenn die Computerobjekte zu bestimmten Organisationseinheiten (OEs) gehören, müssen diese OEs auch für die Synchronisierung in Azure AD Connect konfiguriert werden.

Ab Version 1.1.819.0 bietet Azure AD Connect einen Assistenten für die Konfiguration der Azure AD-Hybrideinbindung. Mit dem Assistenten können Sie den Konfigurationsprozess erheblich vereinfachen. Der entsprechende Assistent konfiguriert die Dienstverbindungspunkte (SCP) für die Geräteregistrierung.

Die Konfigurationsschritte in diesem Artikel basieren auf diesen Assistenten. 

für die Azure AD-Hybrideinbindung ist erforderlich, dass die Geräte innerhalb des Netzwerks Ihrer Organisation auf die folgenden Microsoft-Ressourcen zugreifen können:  

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- https://autologon.microsoftazuread-sso.com (Wenn Sie das nahtlose einmalige Anmelden verwenden oder verwenden möchten)

Wenn für Ihre Organisation Internetzugriff über einen ausgehenden Proxy erforderlich ist, können Sie ab Windows 10 1709 [mithilfe eines Gruppenrichtlinienobjekts (GPO) Proxyeinstellungen auf Ihrem Computer konfigurieren](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/). Wenn Ihr Computer unter einem älteren Betriebssystem als Windows 10 1709 ausgeführt wird, müssen Sie den Web Proxy Auto-Discovery-Dienst (WPAD) implementieren, um Windows 10-Computer für die Geräteregistrierung bei Azure AD zu aktivieren. 

Wenn Ihre Organisation Internetzugriff über einen authentifizierten ausgehenden Proxy erfordert, müssen Sie sicherstellen, dass Ihre Windows 10-Computer erfolgreich beim ausgehenden Proxy authentifiziert werden können. Da Windows 10-Computer die Geräteregistrierung mithilfe von Computerkontext ausführen, muss die Authentifizierung bei ausgehenden Proxys mithilfe von Computerkontext konfiguriert werden. Erkundigen Sie sich beim Anbieter Ihres ausgehenden Proxys nach den Konfigurationsanforderungen. 



## <a name="configure-hybrid-azure-ad-join"></a>Konfigurieren der Hybrid-Azure AD-Einbindung

Zum Konfigurieren einer Azure AD-Hybrideinbindung mithilfe von Azure AD Connect benötigen Sie Folgendes:

- Die Anmeldeinformationen eines globalen Administrators für Ihren Azure AD-Mandanten.  

- Die Anmeldeinformationen eines Unternehmensadministrators für jede Gesamtstruktur.


**Konfigurieren einer Azure AD-Hybrideinbindung mithilfe von Azure AD Connect:**

1. Starten Sie Azure AD Connect, und klicken Sie dann auf **Konfigurieren**.

    ![Willkommen](./media/hybrid-azuread-join-managed-domains/11.png)

2. Wählen Sie auf der Seite **Zusätzliche Aufgaben** die Option **Geräteoptionen konfigurieren**, und klicken Sie dann auf **Weiter**. 

    ![Zusätzliche Aufgaben](./media/hybrid-azuread-join-managed-domains/12.png)

3. Klicken Sie auf der Seite **Übersicht** auf **Weiter**. 

    ![Übersicht](./media/hybrid-azuread-join-managed-domains/13.png)

4. Geben Sie auf der Seite **Mit Azure AD verbinden** die Anmeldeinformationen eines globalen Administrators für Ihren Azure AD-Mandanten ein.  

    ![Stellen Sie eine Verbindung mit Azure AD her.](./media/hybrid-azuread-join-managed-domains/14.png)

5. Wählen Sie auf der Seite **Geräteoptionen** die Option **Konfigurieren einer Azure AD-Hybrideinbindung** aus, und klicken Sie dann auf **Weiter**. 

    ![Geräteoptionen](./media/hybrid-azuread-join-managed-domains/15.png)

6. Führen Sie auf der Seite **SCP** für jede Gesamtstruktur, in der Azure AD Connect das SCP konfigurieren soll, die folgenden Schritte aus, und klicken Sie dann auf **Weiter**: 

    ![SCP](./media/hybrid-azuread-join-managed-domains/16.png)

    a. Wählen Sie die Gesamtstruktur aus.

    b. Wählen Sie den Authentifizierungsdienst aus.

    c. Klicken Sie auf **Hinzufügen**, um die Anmeldeinformationen eines Unternehmensadministrators einzugeben.


7. Wählen Sie auf der Seite **Gerätebetriebssysteme** die von den Geräten in Ihrer Active Directory-Umgebung verwendeten Betriebssysteme aus, und klicken Sie dann auf **Weiter**. 

    ![Gerätebetriebssystem](./media/hybrid-azuread-join-managed-domains/17.png)


8. Klicken Sie auf der Seite **Bereit zur Konfiguration** auf **Konfigurieren**. 

    ![Bereit zur Konfiguration](./media/hybrid-azuread-join-managed-domains/19.png)

9. Klicken Sie auf der Seite **Konfiguration abgeschlossen** auf **Beenden**. 

    ![Konfiguration abgeschlossen](./media/hybrid-azuread-join-managed-domains/20.png)




## <a name="enable-windows-down-level-devices"></a>Aktivieren von kompatiblen Windows-Geräten

Wenn es sich bei einigen Ihrer in die Domäne eingebundenen Geräte um kompatible Windows-Geräte handelt, gehen Sie wie folgt vor:

- Geräteeinstellungen aktualisieren
 
- Konfigurieren der lokalen Intraneteinstellungen für die Geräteregistrierung

- Konfigurieren des nahtlosen einmaligen Anmeldens (Single Sign-On, SSO)

- Steuern kompatibler Windows-Geräte 


### <a name="update-device-settings"></a>Geräteeinstellungen aktualisieren 

Zum Registrieren von kompatiblen Windows-Geräten müssen Sie sicherstellen, dass die Geräteeinstellungen festgelegt sind, mit der Benutzer Geräte in Azure AD registrieren können. Im Azure-Portal finden Sie diese Einstellung unter:

`Home > [Name of your tenant] > Devices - Device settings`  


    
Die folgende Richtlinie muss auf **Alle** festgelegt sein: **Benutzer dürfen ihre Geräte für Azure AD registrieren**

![Registrieren von Geräten](media/hybrid-azuread-join-managed-domains/23.png)



### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Konfigurieren der lokalen Intraneteinstellungen für die Geräteregistrierung

Um die Einbindung in Hybrid-Azure AD für Ihre kompatiblen Windows-Geräte erfolgreich abzuschließen und Zertifikataufforderungen bei der Authentifizierung von Geräten gegenüber Azure AD zu vermeiden, können Sie eine Richtlinie auf Ihre in die Domäne eingebundenen Geräte übertragen, mit der die folgende URL in Internet Explorer der Zone „Lokales Intranet“ hinzugefügt wird:

- `https://device.login.microsoftonline.com`

- `https://autologon.microsoftazuread-sso.com`.

Außerdem müssen Sie in der lokalen Intranetzone des Benutzers die Option **Aktualisierungen der Statusleiste per Skript zulassen** aktivieren.


### <a name="configure-seamless-sso"></a>Konfigurieren des nahtlosen einmaligen Anmeldens

Sie müssen außerdem [nahtloses SSO konfigurieren](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso-quick-start#step-2-enable-the-feature), um die Azure AD-Hybrideinbindung Ihrer kompatiblen Windows-Geräte in einer verwalteten Domäne erfolgreich abzuschließen, die als Authentifizierungsmethode für die Azure AD-Cloud Passthrough-Authentifizierung (PTA) oder Kennworthashsynchronisierung (Password Hash Sync, PHS) verwendet. 


### <a name="control-windows-down-level-devices"></a>Steuern kompatibler Windows-Geräte 

Zum Registrieren von kompatiblen Windows-Geräten müssen Sie ein Windows Installer-Paket (.msi) aus dem Download Center herunterladen und installieren. Klicken Sie [hier](hybrid-azuread-join-control.md#control-windows-down-level-devices), um weitere Informationen zu erhalten. 


## <a name="verify-the-registration"></a>Überprüfen der Registrierung

Zum Überprüfen des Geräteregistrierungsstatus in Ihrem Azure-Mandanten können Sie das Cmdlet **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** im **[Azure Active Directory PowerShell-Modul](/powershell/azure/install-msonlinev1?view=azureadps-2.0)** verwenden.

Bei Verwendung des Cmdlets **Get-MSolDevice** zur Überprüfung der Dienstdetails:

- Ein Objekt mit der **Geräte-ID**, die der ID auf dem Windows-Client entspricht, muss vorhanden sein.
- Der Wert für **DeviceTrustType** muss **Domänenbeitritt** sein. Dies entspricht dem Status **Hybrid in Azure AD eingebunden** auf der Seite „Geräte“ im Azure AD-Portal.
- Für Geräte, die für den bedingten Zugriff verwendet werden, muss **Aktiviert** den Wert **True** und **DeviceTrustLevel** den Wert **Verwaltet** haben. 


**So überprüfen Sie die Dienstdetails:**

1. Öffnen Sie **Windows PowerShell** als Administrator.

2. Geben Sie `Connect-MsolService` ein, um die Verbindung mit Ihrem Azure-Mandanten herzustellen.  

3. Geben Sie `get-msoldevice -deviceId <deviceId>`ein.

6. Vergewissern Sie sich, dass **Aktiviert** auf **True** festgelegt ist.





## <a name="troubleshoot-your-implementation"></a>Problembehandlung bei der Implementierung

Wenn bei der Azure AD-Hybrideinbindung für in Domänen eingebundene Windows-Geräte Probleme auftreten, finden Sie weitere Informationen unter:

- [Problembehandlung für in Azure AD eingebundene aktuelle Windows-Hybridgeräte](troubleshoot-hybrid-join-windows-current.md)
- [Problembehandlung für in Azure AD eingebundene kompatible Windows-Hybridgeräte](troubleshoot-hybrid-join-windows-legacy.md)


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfigurieren der Azure Active Directory-Hybrideinbindung für Verbunddomänen](hybrid-azuread-join-federated-domains.md)
> [Manuelles Konfigurieren der Azure Active Directory-Hybrideinbindung](hybrid-azuread-join-manual-steps.md)

