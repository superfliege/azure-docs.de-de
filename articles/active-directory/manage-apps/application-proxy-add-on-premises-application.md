---
title: Hinzufügen einer lokalen App – Anwendungsproxy in Azure Active Directory | Microsoft-Dokumentation
description: Mit einem Anwendungsproxydienst in Azure Active Directory (Azure AD) können Benutzer auf lokale Anwendungen zugreifen, indem sie sich mit ihrem Azure AD-Konto anmelden. In diesem Tutorial erfahren Sie zunächst, wie Sie Ihre Umgebung auf die Verwendung des Anwendungsproxys vorbereiten, und dann wird im Azure-Portal Ihrem Azure AD-Mandanten eine lokale Anwendung hinzugefügt.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: tutorial
ms.date: 03/12/2019
ms.author: celested
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 29f2de6eb0171e5e1c792e8860a56f014dad501f
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58314823"
---
# <a name="tutorial-add-an-on-premises-application-for-remote-access-through-application-proxy-in-azure-active-directory"></a>Tutorial: Hinzufügen einer lokalen Anwendung für den Remotezugriff über den Anwendungsproxy in Azure Active Directory

Mit einem Anwendungsproxydienst in Azure Active Directory (Azure AD) können Benutzer auf lokale Anwendungen zugreifen, indem sie sich mit ihrem Azure AD-Konto anmelden. In diesem Tutorial wird Ihre Umgebung auf die Verwendung des Anwendungsproxys vorbereitet. Wenn Ihre Umgebung bereit ist, fügen Sie im Azure-Portal Ihrem Azure AD-Mandanten eine lokale Anwendung hinzu.

In diesem Tutorial wird Folgendes durchgeführt:

> [!div class="checklist"]
> * Öffnen von Ports für ausgehenden Datenverkehr und Zulassen des Zugriffs auf bestimmte URLs
> * Installieren des Connectors auf Ihrem Windows-Server und Registrierung mit dem Anwendungsproxy
> * Überprüfen der ordnungsgemäßen Installation und Registrierung des Connectors
> * Hinzufügen einer lokalen Anwendung zum Azure AD-Mandanten
> * Überprüfen, ob ein Testbenutzer sich über das Azure AD-Konto bei der Anwendung anmelden kann.

## <a name="before-you-begin"></a>Voraussetzungen

Zum Hinzufügen einer Anwendung zu Ihrem Mandanten benötigen Sie Folgendes:

* Ein [Microsoft Azure AD Basic- oder Premium-Abonnement](https://azure.microsoft.com/pricing/details/active-directory) 
* Ein Konto als Anwendungsadministrator

### <a name="windows-server"></a>Windows-Server

Für die Verwendung des Anwendungsproxys benötigen Sie einen Windows-Server, auf dem Windows Server 2012 R2 oder höher ausgeführt wird. Sie müssen den Connector für den Anwendungsproxy auf dem Server installieren. Dieser Connectorserver muss eine Verbindung mit den Anwendungsproxydiensten in Azure sowie mit den lokalen Anwendungen herstellen können, die Sie veröffentlichen möchten.

Für hohe Verfügbarkeit in Ihrer Produktionsumgebung sollten Sie mehrere Windows-Server einsetzen. Für dieses Tutorial reicht ein Windows-Server aus.

#### <a name="recommendations-for-the-connector-server"></a>Empfehlungen für den Connectorserver

1. Platzieren Sie den Connectorserver physisch in der Nähe des Anwendungsservers, um die Leistung zwischen Connector und Anwendung zu optimieren. Weitere Informationen finden Sie unter [Aspekte der Netzwerktopologie bei Verwendung des Azure Active Directory-Anwendungsproxys](application-proxy-network-topology.md).

2. Der Connectorserver und die Webanwendungsserver müssen zu derselben Active Directory-Domäne angehören. Dass die Server zu derselben Domäne gehören, ist eine Voraussetzung für die Verwendung des einmaligen Anmeldens (Single Sign-On, SSO) mit integrierter Windows-Authentifizierung (IWA) und eingeschränkter Kerberos-Delegierung (Kerberos Constrained Delegation, KCD). Wenn der Connector- und Webanwendungsserver sich in unterschiedlichen Active Directory-Domänen befinden, müssen Sie die ressourcenbasierte Delegierung für einmaliges Anmelden verwenden. Weitere Informationen finden Sie unter [Eingeschränkte Kerberos-Delegierung für das einmalige Anmelden mit Anwendungsproxy](application-proxy-configure-single-sign-on-with-kcd.md).

#### <a name="software-requirements"></a>Softwareanforderungen

Auf dem Windows-Connectorserver muss TLS 1.2 aktiviert werden, bevor Sie den Anwendungsproxyconnector installieren. Vorhandene Connectors mit Versionen vor 1.5.612.0 nutzen bis auf Weiteres die früheren Versionen von TLS weiter. 

So aktivieren Sie TLS 1.2

1. Legen Sie die folgenden Registrierungsschlüssel fest:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

2. Neustarten des Servers

## <a name="prepare-your-on-premises-environment"></a>Vorbereiten Ihrer lokalen Umgebung

Zum Vorbereiten Ihrer Umgebung für den Azure AD-Anwendungsproxy, müssen Sie zuerst die Kommunikation mit Azure-Rechenzentren ermöglichen. Wenn sich eine Firewall im Pfad befindet, stellen Sie sicher, dass sie offen ist, sodass der Connector HTTPS-Anforderungen (TCP) an den Anwendungsproxy richten kann.

### <a name="open-ports"></a>Öffnen von Ports

Öffnen Sie die folgenden Ports für den **ausgehenden** Datenverkehr. 

   | Portnummer | Wie diese verwendet wird |
   | --- | --- |
   | 80 | Herunterladen der Zertifikatssperrlisten (CRL) bei der Überprüfung des SSL-Zertifikats |
   | 443 | Gesamte ausgehende Kommunikation mit dem Webanwendungsproxy-Dienst |

Wenn Ihre Firewall Datenverkehr gemäß Ursprungsbenutzern erzwingt, öffnen Sie auch die Ports 80 und 443 für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden.

Wenn Sie den Anwendungsproxy bereits verwenden, haben Sie möglicherweise eine ältere Version des Connectors installiert.  Installieren Sie mithilfe dieses Tutorials die neueste Version des Connectors. Ältere Versionen als 1.5.132.0 erfordern auch die folgenden offenen Ports: 5671, 8080, 9090-9091, 9350, 9352, 10100–10120. 

### <a name="allow-access-to-urls"></a>Zulassen des Zugriffs auf URLs

Lassen Sie den Zugriff auf die folgenden URLs zu:

| URL | Wie diese verwendet wird |
| --- | --- |
| \*.msappproxy.net<br>\*.servicebus.windows.net | Kommunikation zwischen dem Connector und dem Anwendungsproxy-Clouddienst |
| mscrl.microsoft.com:80<br>crl.microsoft.com:80<br>ocsp.msocsp.com:80<br>www.microsoft.com:80 | Azure verwendet diese URLs, um Zertifikate zu überprüfen. |
| login.windows.net<br>login.microsoftonline.com<br>secure.aadcdn.microsoftonline-p.com  | Der Connector verwendet diese URLs während der Registrierung. |

Wenn Ihre Firewall oder Ihr Proxy DNS-Whitelisting zulässt, können Sie Verbindungen mit „\*.msappproxy.net“ und „\*.servicebus.windows.net“ per Whitelist verwalten. Andernfalls müssen Sie den Zugriff auf die [IP-Adressbereiche für das Azure-Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=41653) zulassen. Die IP-Adressbereiche werden wöchentlich aktualisiert.

## <a name="install-and-register-a-connector"></a>Installieren und Registrieren eines Connectors

Um den Anwendungsproxy verwenden zu können, müssen Sie einen Connector auf jedem Windows-Server installieren, den Sie mit dem Anwendungsproxydienst verwenden. Der Connector ist ein Agent, der die aus dem lokalen Anwendungsserver ausgehende Verbindung mit dem Anwendungsproxy in Azure AD verwaltet. Sie können einen Connector auf Servern installieren, auf denen auch andere Authentifizierungs-Agents wie Azure AD Connect installiert sind.

So installieren Sie den Connector:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als Anwendungsadministrator des Verzeichnisses an, das den Anwendungsproxy verwendet. Wenn die Mandantendomäne also beispielsweise „contoso.com“ lautet, muss sich der Administrator als admin@contoso.com oder mit einem anderen Administratoraliasnamen in dieser Domäne anmelden.
2. Das aktuelle Verzeichnis wird unter Ihrem Benutzernamen in der rechten oberen Ecke angezeigt. Stellen Sie sicher, dass Sie bei dem Verzeichnis angemeldet sind, das den Anwendungsproxy verwendet. Wenn Sie Verzeichnisse wechseln möchten, wählen Sie das Symbol aus.
3. Klicken Sie im linken Blatt auf **Azure Active Directory** und dann auf **Anwendungsproxy**.
4. Klicken Sie auf **Connectordienst herunterladen**.
5. Lesen Sie die Vertragsbedingungen.  Wenn Sie bereit sind, klicken Sie auf **Bedingungen akzeptieren und herunterladen**.
6. Am unteren Rand des Fensters sehen Sie eine Aufforderung zum Herunterladen von **AADApplicationProxyConnectorInstaller.exe**. Klicken Sie auf **Ausführen**, um den Connector zu installieren. Ein Installations-Assistent wird geöffnet. 
7. Befolgen Sie die Anweisungen des Assistenten für die Installation. Wenn Sie aufgefordert werden, den Connector mit dem Anwendungsproxy für Ihren Azure AD-Mandanten zu registrieren, geben Sie Ihre Administratoranmeldeinformationen für die Anwendung ein.
    - Falls beim Internet Explorer (IE) **Verstärkte Sicherheitskonfiguration für IE** auf **Ein** festgelegt ist, wird der Registrierungsbildschirm unter Umständen nicht anzeigt. Befolgen Sie die Anweisungen in der Fehlermeldung, um Zugriff zu erhalten. Stellen Sie sicher, dass die erweiterte Sicherheit von Internet Explorer auf **Aus** gesetzt ist.

### <a name="general-remarks"></a>Allgemeine Hinweise

Wenn Sie bereits einen Connector installiert haben, installieren Sie ihn neu, um die neueste Version zu erhalten.

Wenn Sie mehrere Windows-Server für Ihre lokalen Anwendungen einsetzen möchten, müssen Sie den Connector auf jedem Server installieren und registrieren. Sie können die Connectors in Connectorgruppen organisieren. Weitere Informationen finden Sie unter [Veröffentlichen von Anwendungen in getrennten Netzwerken und an getrennten Standorten mithilfe von Connectorgruppen](application-proxy-connector-groups.md). 

Wenn Ihre Organisation für Verbindungen mit dem Internet Proxyserver verwendet, müssen Sie diese für den Anwendungsproxy konfigurieren.  Weitere Informationen finden Sie unter [Verwenden von vorhandenen lokalen Proxyservern](application-proxy-configure-connectors-with-proxy-servers.md). 

Informationen zu Connectors, deren Aktualisierung und Kapazitätsplanung finden Sie unter [Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-connectors.md). 

Installieren Sie immer den aktuellen Connector, wenn Sie die Qlik Sense-Anwendung verwenden. Für Qlik Sense werden WebSockets verwendet. Dies wird nur für die Connectorversion 1.5.612.0 oder höher unterstützt.

## <a name="verify-the-connector-installed-and-registered-correctly"></a>Überprüfen der ordnungsgemäßen Installation und Registrierung des Connectors

Sie können mithilfe des Azure-Portals oder Ihres Windows-Servers sicherstellen, dass ein neuer Connector ordnungsgemäß installiert ist.

### <a name="verify---azure-portal"></a>Überprüfen – Azure-Portal

So überprüfen Sie, ob der Connector ordnungsgemäß installiert und registriert ist:

1. Melden Sie sich bei Ihrem Mandantenverzeichnis im [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Azure Active Directory** und dann auf **Anwendungsproxy**. Alle Connectors und Connectorgruppen werden auf dieser Seite angezeigt. 
3. Wählen Sie einen Connector aus, um die Details zu überprüfen. Eine aktive grüne Beschriftung gibt an, dass Ihr Connector eine Verbindung mit dem Dienst herstellen kann. Jedoch auch wenn die Beschriftung grün ist, könnte ein Netzwerkproblem immer noch den Nachrichtenempfang des Connectors blockieren. 

    ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-connectors/app-proxy-connectors.png)

Weitere Hilfe bei der Installation eines Connectors finden Sie unter [Problem beim Installieren des Anwendungsproxy-Agent-Connectors](application-proxy-connector-installation-problem.md).

### <a name="verify---windows-server"></a>Überprüfen – Windows-Server

So überprüfen Sie, ob der Connector ordnungsgemäß installiert und registriert ist:

1. Öffnen Sie den Windows-Dienst-Manager, indem Sie auf die **Windows**-Taste klicken und *services.msc* eingeben.
2. Überprüfen Sie, ob die folgenden beiden Dienste den Status **Wird ausgeführt** haben.
   - **Microsoft AAD-Anwendungsproxyconnector** ermöglicht die Konnektivität.
   - Die **Aktualisierung des Microsoft AAD-Anwendungsproxyconnectors** ist ein automatischer Aktualisierungsdienst. Der Dienst sucht nach neuen Versionen des Connectors und aktualisiert den Connector bei Bedarf.

     ![Anwendungsproxy-Connectordienste – Screenshot](./media/application-proxy-enable/app_proxy_services.png)

3. Wenn die Dienste nicht den Status **Wird ausgeführt** haben, klicken Sie mit der rechten Maustaste auf jeden Dienst, und wählen Sie **Starten** aus. 

## <a name="add-an-on-premises-app-to-azure-ad"></a>Hinzufügen einer lokalen App zu Azure AD

Nun haben Sie Ihre Umgebung vorbereitet, einen Connector installiert und sind bereit, Azure AD lokale Anwendungen hinzuzufügen.  

1. Melden Sie sich als Administrator beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** > **Neue Anwendung** aus.

    ![Hinzufügen einer Unternehmensanwendung](./media/application-proxy-publish-azure-portal/add-app.png)

3. Wählen Sie **Alle** und dann **Lokale Anwendungen** aus.  

    ![Hinzufügen einer eigenen Anwendung](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Geben Sie auf dem Blatt **Fügen Sie Ihre eigene lokale Anwendung hinzu** die folgenden Informationen zu Ihrer Anwendung an:

    ![Konfigurieren Ihrer lokalen Anwendung](./media/application-proxy-add-on-premises-application/add-on-premises-app-with-application-proxy-updated.png)

    | Feld | BESCHREIBUNG |
    | :---- | :---------- |
    | **Name** | Der Name der Anwendung, der im Zugriffsbereich und im Azure-Portal angezeigt wird. |
    | **Interne URL** | Die URL zum Zugreifen auf die Anwendung innerhalb Ihres privaten Netzwerks. Sie können einen bestimmten Pfad auf dem Back-End-Server für die Veröffentlichung angeben, während der Rest des Servers nicht veröffentlicht wird. Auf diese Weise können Sie unterschiedliche Websites auf demselben Server als unterschiedliche Apps veröffentlichen und jeweils einen eigenen Namen und Zugriffsregeln vergeben.<br><br>Stellen Sie beim Veröffentlichen eines Pfads sicher, dass er alle erforderlichen Bilder, Skripts und Stylesheets für Ihre Anwendung enthält. Wenn sich die App beispielsweise unter „https:\//yourapp/app“ befindet und Bilder unter „https:\//yourapp/media“ genutzt werden, sollten Sie „https:\//yourapp/“ als Pfad veröffentlichen. Diese interne URL verfügt nicht über die Zielseite, die den Benutzern angezeigt wird. Weitere Informationen finden Sie unter [Festlegen einer benutzerdefinierten Startseite für veröffentlichte Apps](application-proxy-configure-custom-home-page.md). |
    | **Externe URL** | Die Adresse für den Benutzerzugriff auf die App von außerhalb Ihres Netzwerks. Wenn Sie die Anwendungsproxy-Standarddomäne nicht verwenden möchten, lesen Sie [Benutzerdefinierte Domänen im Azure AD-Anwendungsproxy](application-proxy-configure-custom-domain.md).|
    | **Vorauthentifizierung** | Gibt das Verfahren an, wie der Anwendungsproxy Benutzer überprüft, bevor diese Zugriff auf Ihre Anwendung erhalten.<br><br>**Azure Active Directory**: Der Anwendungsproxy leitet Benutzer an die Anmeldung mit Azure AD um. Hierbei werden deren Berechtigungen für das Verzeichnis und die Anwendung authentifiziert. Es wird empfohlen, diese Option als Standardwert aktiviert zu lassen, damit Sie Azure AD-Sicherheitsfunktionen wie bedingten Zugriff und Multi-Factor Authentication nutzen können. **Azure Active Directory** ist erforderlich, um die Anwendung mit Microsoft Cloud App Security zu überwachen.<br><br>**Passthrough**: Benutzer müssen sich nicht bei Azure Active Directory authentifizieren, um Zugriff auf die Anwendung zu erhalten. Sie können weiterhin Authentifizierungsanforderungen auf dem Back-End einrichten. |
    | **Connectorgruppe** | Connectors verarbeiten den Remotezugriff auf Ihre Anwendung, und Connectorgruppen helfen Ihnen, Connectors und Apps nach Region, Netzwerk oder Zweck zu organisieren. Wenn Sie noch keine Connectorgruppen erstellt haben, wird Ihre App zu **Standard** zugewiesen.<br><br>Wenn Ihre Anwendung WebSockets verwendet, um eine Verbindung herzustellen, muss die Version aller Connectors in der Gruppe 1.5.612.0 oder höher sein.|

5. Konfigurieren Sie bei Bedarf die Einstellungen unter **Zusätzliche Einstellungen**. Für die meisten Anwendungen sollten Sie diese Einstellungen im Standardzustand belassen. 

    | Feld | BESCHREIBUNG |
    | :---- | :---------- |
    | **Timeout der Back-End-Anwendung** | Legen Sie diesen Wert nur auf **Lang** fest, wenn Ihre Anwendung bei der Authentifizierung und dem Verbinden langsam ist. |
    | **Nur-HTTP-Cookie verwenden** | Legen Sie diesen Wert auf **Ja** fest, damit Anwendungsproxycookies das Flag HTTPOnly im HTTP-Antwortheader enthalten. Wenn Sie Remotedesktopdienste verwenden, sollten Sie diese Option auf **Nein** festlegen.|
    | **Sicheres Cookie verwenden**| Legen Sie diesen Wert auf **Ja** fest, um Cookies über einen sicheren Kanal zu übertragen, z.B. eine verschlüsselte HTTPS-Anforderung.
    | **Beständiges Cookie verwenden**| Behalten Sie für diesen Wert die Einstellung **Nein** bei. Diese Einstellung sollte nur für Anwendungen verwendet werden, die keine Cookies zwischen Prozessen freigeben können. Weitere Informationen zu Cookieeinstellungen finden Sie unter [Cookieeinstellungen für den Zugriff auf lokale Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).
    | **URLs in Headern übersetzen** | Behalten Sie für diesen Wert **Ja** bei, wenn Ihre Anwendung den ursprünglichen Hostheader nicht in der Authentifizierungsanforderung erfordert. |
    | **URLs im Hauptteil der Anwendung übersetzen** | Behalten Sie für diesen Wert **Nein** bei, wenn Sie nicht über hartcodierte HTML-Links zu anderen lokalen Anwendungen verfügen, und verwenden Sie keine benutzerdefinierten Domänen. Weitere Informationen finden Sie unter [Linkübersetzung mit dem Anwendungsproxy](application-proxy-configure-hard-coded-link-translation.md).<br><br>Legen Sie diesen Wert auf **Ja** fest, wenn Sie diese Anwendung mit Microsoft Cloud App Security (MCAS) überwachen möchten. Weitere Informationen finden Sie unter [Konfigurieren der Echtzeitüberwachung des Anwendungszugriffs mit Microsoft Cloud App Security und Azure Active Directory](application-proxy-integrate-with-microsoft-cloud-application-security.md). |
   
6. Wählen Sie **Hinzufügen**.

## <a name="test-the-application"></a>Testen der Anwendung

Sie können nun testen, ob die Anwendung richtig hinzugefügt wurde. Mit den folgenden Schritten fügen Sie der Anwendung ein Benutzerkonto hinzu und probieren aus, ob Sie sich anmelden können.

### <a name="add-a-user-for-testing"></a>Hinzufügen eines Benutzers zu Testzwecken

Bevor Sie der Anwendung einen Benutzer hinzufügen, stellen Sie sicher, dass das Benutzerkonto bereits über Berechtigungen zum Zugriff auf die Anwendung aus dem Unternehmensnetzwerk heraus verfügt.

So fügen Sie einen Testbenutzer hinzu:

1. Wählen Sie auf dem Blatt **Schnellstart** die Option **Benutzer zu Testzwecken zuweisen** aus.

    ![Zuweisen eines Benutzers zu Testzwecken](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Benutzer hinzufügen** aus.

    ![Hinzufügen eines Benutzers oder einer Gruppe](./media/application-proxy-publish-azure-portal/add-user.png)

3. Wählen Sie auf dem Blatt **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus, und wählen Sie dann das Konto aus, das Sie hinzufügen möchten. 
4. Wählen Sie **Zuweisen** aus.

### <a name="test-the-sign-on"></a>Testen des Anmeldens

So testen Sie das Anmelden bei der Anwendung:

1. Navigieren Sie in Ihrem Browser zur externen URL, die Sie beim Veröffentlichen konfiguriert haben. 
2. Der Startbildschirm sollte angezeigt werden.
3. Versuchen Sie, sich als der Benutzer anzumelden, den Sie im vorherigen Abschnitt erstellt haben.

    ![Testen der veröffentlichten App](./media/application-proxy-publish-azure-portal/test-app.png)

Informationen zur Problembehandlung finden Sie unter [Beheben von Problemen mit Anwendungsproxys und Fehlermeldungen](application-proxy-troubleshoot.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Ihre lokale Umgebung auf den Einsatz des Anwendungsproxys vorbereitet und dann den Anwendungsproxyconnector installiert und registriert. Als Nächstes fügen Sie Ihrem Azure AD-Mandanten eine Anwendung hinzu. Sie haben überprüft, ob ein Testbenutzer sich über das Azure AD-Konto bei der Anwendung anmelden kann.

Sie haben folgende Schritte ausgeführt:
> [!div class="checklist"]
> * Öffnen von Ports für ausgehenden Datenverkehr und Zulassen des Zugriffs auf bestimmte URLs
> * Installieren des Connectors auf Ihrem Windows-Server und Registrierung mit dem Anwendungsproxy
> * Überprüfen der ordnungsgemäßen Installation und Registrierung des Connectors
> * Hinzufügen einer lokalen Anwendung zum Azure AD-Mandanten
> * Überprüfen, ob ein Testbenutzer sich über das Azure AD-Konto bei der Anwendung anmelden kann.

Sie können die Anwendung jetzt für einmaliges Anmelden konfigurieren. Verwenden Sie den folgenden Link, um eine Anmeldemethode auszuwählen und auf Tutorials zum einmaligen Anmelden zuzugreifen. 

> [!div class="nextstepaction"]
>[Konfigurieren von einmaligem Anmelden](what-is-single-sign-on.md#choosing-a-single-sign-on-method)
