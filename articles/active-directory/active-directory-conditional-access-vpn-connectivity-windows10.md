---
title: "Bedingter Azure Active Directory-Zugriff für VPN-Verbindungen (Vorschauversion) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie der bedingte Azure Active Directory-Zugriff für VPN-Verbindungen funktioniert. "
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: e9dadb3291ee760e7b05caedfa6b4128be77aa7d
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-conditional-access-for-vpn-connectivity-preview"></a>Bedingter Azure Active Directory-Zugriff für VPN-Verbindungen (Vorschauversion)

Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) können Sie präzise steuern, wie autorisierte Benutzer auf Ihre Ressourcen zugreifen. Der bedingte Azure AD-Zugriff für Verbindungen über ein virtuelles privates Netzwerk (VPN) ermöglicht den Schutz Ihrer VPN-Verbindungen.


Führen Sie zum Konfigurieren des bedingten Zugriffs für VPN-Verbindungen folgende Schritte aus: 

1.  Konfigurieren Ihres VPN-Servers
2.  Konfigurieren Ihres VPN-Clients
3.  Konfigurieren der Richtlinie für den bedingten Zugriff


## <a name="before-you-begin"></a>Voraussetzungen

In diesem Thema wird vorausgesetzt, dass Sie mit folgenden Themen vertraut sind:

- [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md)
- [VPN und bedingter Zugriff](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Informationen zur Implementierung dieses Features durch Microsoft finden Sie unter [Enhancing remote access in Windows 10 with an automatic VPN profile](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile) (Erweitern des Remotezugriffs unter Windows 10 mit einem automatischen VPN-Profil).   


## <a name="prerequisites"></a>Voraussetzungen

Um den bedingten Azure Active Directory-Zugriff für VPN-Verbindungen konfigurieren zu können, benötigen Sie einen konfigurierten VPN-Server. 



## <a name="step-1-configure-your-vpn-server"></a>Schritt 1: Konfigurieren Ihres VPN-Servers 

In diesem Schritt werden Stammzertifikate für die VPN-Authentifizierung mit Azure AD konfiguriert. Erforderliche Schritte zum Konfigurieren des bedingten Zugriffs für VPN-Verbindungen:

1. Erstellen eines VPN-Zertifikats über das Azure-Portal
2. Herunterladen des VPN-Zertifikats
2. Bereitstellen des Zertifikats auf Ihrem VPN-Server

In Azure AD wird das VPN-Zertifikat zum Signieren von Zertifikaten verwendet, die für Windows 10-Clients ausgestellt werden, wenn die Authentifizierung bei Azure AD für VPN-Verbindungen erfolgt. Bei dem vom Windows 10-Client angeforderten Token handelt es sich um ein Zertifikat, das dann für die Anwendung (in diesem Fall: der VPN-Server) bereitgestellt wird.

![Herunterladen des Zertifikats für den bedingten Zugriff](./media/active-directory-conditional-access-vpn-connectivity-windows10/06.png)

Im Azure-Portal können Sie zwei Zertifikate erstellen, um einen reibungslosen Übergang zu gewährleisten, wenn ein Zertifikat in Kürze abläuft. Beim Erstellen eines Zertifikats können Sie festlegen, ob es sich um das primäre Zertifikat handelt, das während der Authentifizierung zum Signieren des Zertifikats für die Verbindung verwendet wird.

So erstellen Sie ein VPN-Zertifikat

1. Melden Sie sich als globaler Administrator beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im linken Menü auf **Azure Active Directory**. 

    ![Wählen Sie „Azure Active Directory“.](./media/active-directory-conditional-access-vpn-connectivity-windows10/01.png)

3. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Verwalten** auf **Bedingter Zugriff**.

    ![Auswählen von „Bedingter Zugriff“](./media/active-directory-conditional-access-azure-portal-get-started/02.png)

4. Klicken Sie auf der Seite **Bedingter Zugriff** im Abschnitt **Verwalten** auf **VPN-Konnektivität (Vorschau)**.

    ![Auswählen von „VPN-Konnektivität“](./media/active-directory-conditional-access-vpn-connectivity-windows10/03.png)

5. Klicken Sie auf der Seite **VPN-Konnektivität** auf **Neues Zertifikat**.

    ![Auswählen von „Neues Zertifikat“](./media/active-directory-conditional-access-vpn-connectivity-windows10/04.png)

6. Führen Sie auf der Seite **Neu** die folgenden Schritte aus:

    ![„Dauer auswählen“ und „Primär“](./media/active-directory-conditional-access-vpn-connectivity-windows10/05.png)

    a. Wählen Sie unter **Dauer auswählen** die Option **1 Jahr** aus.

    b. Legen Sie **Primär** auf **Ja** fest.

    c. Klicken Sie auf **Erstellen**.

7. Klicken Sie auf der Seite „VPN-Konnektivität“ auf **Zertifikat herunterladen**.


Nun können Sie Ihr neu erstelltes Zertifikat auf Ihrem VPN-Server bereitstellen. Fügen Sie auf dem VPN-Server das heruntergeladene Zertifikat als *vertrauenswürdige Stammzertifizierungsstelle für die VPN-Authentifizierung* hinzu.

Fügen Sie bei Windows-RRAS-basierten Bereitstellungen das Stammzertifikat auf Ihrem NPS-Server durch Ausführen der folgenden Befehle dem Speicher *Enterprise NTauth* hinzu:

1. `certutil -dspublish <CACERT> RootCA`
2. `certutil -dspublish <CACERT> NtAuthCA`



## <a name="step-2-configure-your-vpn-client"></a>Schritt 2: Konfigurieren Ihres VPN-Clients 

In diesem Schritt konfigurieren Sie Ihr VPN-Clientkonnektivitätsprofil gemäß den Angaben unter [VPN und bedingter Zugriff](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).


## <a name="step-3-configure-your-conditional-access-policy"></a>Schritt 3: Konfigurieren der Richtlinie für den bedingten Zugriff

In diesem Abschnitt erfahren Sie, wie Sie Ihre Richtlinie zum bedingten Zugriff für VPN-Konnektivität konfigurieren.


1. Klicken Sie auf der Seite **Bedingter Zugriff** auf der Symbolleiste am oberen Rand auf **Hinzufügen**.

    ![Auswählen von „Hinzufügen“ auf der Seite „Bedingter Zugriff“](./media/active-directory-conditional-access-vpn-connectivity-windows10/07.png)

2. Geben Sie auf der Seite **Neu** im Feld **Name** einen Namen für Ihre Richtlinie ein. Geben Sie beispielsweise **VPN policy** ein.

    ![Hinzufügen des Namens für die Richtlinie auf der Seite „Bedingter Zugriff“](./media/active-directory-conditional-access-vpn-connectivity-windows10/08.png)

5. Klicken Sie im Abschnitt **Zuweisung** auf **Benutzer und Gruppen**.

    ![Auswählen von „Benutzer und Gruppen“](./media/active-directory-conditional-access-vpn-connectivity-windows10/09.png)

6. Führen Sie auf der Seite **Benutzer und Gruppen** die folgenden Schritte aus:

    ![Auswählen eines Testbenutzers](./media/active-directory-conditional-access-vpn-connectivity-windows10/10.png)

    a. Wählen Sie **Benutzer und Gruppen auswählen**.

    b. Klicken Sie auf **Auswählen**.

    c. Wählen Sie auf der Seite **Auswählen** Ihren Testbenutzer aus, und klicken Sie dann auf **Auswählen**.

    d. Klicken Sie auf der Seite **Benutzer und Gruppen** auf **Fertig**.

7. Führen Sie auf der Seite **Neu** die folgenden Schritte aus:

    ![Auswählen der Cloud-Apps](./media/active-directory-conditional-access-vpn-connectivity-windows10/11.png)

    a. Klicken Sie im Abschnitt **Zuweisungen** auf **Cloud-Apps**.

    b. Klicken Sie auf der Seite **Cloud-Apps** auf **Apps auswählen** und anschließend auf **Auswählen**.

    c. Geben Sie auf der Seite **Auswählen** im Feld **Anwendungen** die Zeichenfolge **vpn** ein.

    d. Wählen Sie **VPN-Server** aus.

    e. Klicken Sie auf **Auswählen**.


13. Klicken Sie zum Öffnen der Seite **Gewährung** auf der Seite **Neu** im Abschnitt **Steuerelemente** auf **Gewährung**.

    ![Auswählen von „Gewährung“](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. Führen Sie auf der Seite **Gewährung** die folgenden Schritte aus:

    ![Auswählen von „Erfordern von Multi-Factor Authentication“](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Wählen Sie **Erfordern von Multi-Factor Authentication**.

    b. Klicken Sie auf **Auswählen**.

15. Klicken Sie auf der Seite **Neu** unter **Richtlinie aktivieren** auf **Ein**.

    ![Richtlinie aktivieren](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. Klicken Sie auf der Seite **Neu** auf **Erstellen**.



## <a name="next-steps"></a>Nächste Schritte

Informationen zur Implementierung dieses Features durch Microsoft finden Sie unter [Enhancing remote access in Windows 10 with an automatic VPN profile](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile) (Erweitern des Remotezugriffs unter Windows 10 mit einem automatischen VPN-Profil).    

