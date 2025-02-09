---
title: Unterstützte Verbindungen mit IT Service Management Connector in Azure Log Analytics | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen dazu, wie Sie Ihre ITSM-Produkte bzw. -Dienste mit dem IT Service Management-Connector (ITSMC) in Azure Monitor verbinden, um die ITSM-Arbeitselemente zentral zu überwachen und zu verwalten.
documentationcenter: ''
author: jyothirmaisuri
manager: riyazp
editor: ''
ms.assetid: 8231b7ce-d67f-4237-afbf-465e2e397105
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: v-jysur
ms.openlocfilehash: e52d78731615db8aacd5dba1f6b781adf4b7df53
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255198"
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector"></a>Verbinden von ITSM-Produkten/-Diensten mit dem ITSM-Connector
Dieser Artikel bietet Informationen dazu, wie Sie die Verbindung zwischen Ihrem ITSM-Produkt bzw. -Dienst und dem ITSM-Connector (ITSMC) in Log Analytics konfigurieren, um Arbeitselemente zentral zu verwalten. Weitere Informationen zu ITSMC finden Sie in der [Übersicht](../../azure-monitor/platform/itsmc-overview.md).

Die folgenden ITSM-Produkte und -Dienste werden unterstützt. Wählen Sie ein Produkt aus, um detaillierte Informationen dazu zu erhalten, wie Sie das Produkt mit ITSMC verbinden.

- [System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](#connect-provance-to-it-service-management-connector-in-azure)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-azure)

> [!NOTE]
> 
> Der ITSM-Connector kann nur eine Verbindung mit cloudbasierten ServiceNow-Instanzen. Lokale ServiceNow-Instanzen werden derzeit nicht unterstützt.

## <a name="connect-system-center-service-manager-to-it-service-management-connector-in-azure"></a>Verbinden von System Center Service Manager mit dem ITSM-Connector in Azure

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres System Center Service Manager-Produkts mit dem ITSMC in Azure.

### <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:

- ITSMC wurde installiert. Weitere Informationen: [Hinzufügen der IT Service Management Connector-Lösung](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Die Service Manager-Webanwendung (Web-App) ist bereitgestellt und konfiguriert. Informationen zur Web-App finden Sie [hier](#create-and-deploy-service-manager-web-app-service).
- Hybridverbindung wurde erstellt und konfiguriert. Weitere Informationen: [Konfigurieren der Hybridverbindung](#configure-the-hybrid-connection).
- Unterstützte Versionen von Service Manager:  2012 R2 oder 2016.
- Benutzerrolle:  [Erweiterter Operator](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Verbindungsverfahren

Verwenden Sie das folgende Verfahren zum Verbinden Ihrer System Center Service Manager-Instanz mit ITSMC:

1. Navigieren Sie im Azure-Portal zu **Alle Ressourcen**, und suchen Sie nach **ServiceDesk(YourWorkspaceName)** .

2.  Klicken Sie unter **ARBEITSBEREICHSDATENQUELLEN** auf **ITSM-Verbindungen**.

    ![Neue Verbindung](media/itsmc-connections/add-new-itsm-connection.png)

3. Klicken Sie oben im rechten Bereich auf **Hinzufügen**.

4. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **OK**, um die Verbindung zu erstellen.

> [!NOTE]
> 
> Alle diese Parameter sind erforderlich.

| **Feld** | **Beschreibung** |
| --- | --- |
| **Verbindungsname**   | Geben Sie einen Namen für die System Center Service Manager-Instanz ein, die Sie mit ITSMC verbinden möchten.  Diesen Namen verwenden Sie später beim Konfigurieren von Arbeitselementen in dieser Instanz/beim Anzeigen ausführlicher Protokollanalysen. |
| **Partner type** (Partnertyp)   | Wählen Sie **System Center Service Manager** aus. |
| **Server-URL**   | Geben Sie die URL der Service Manager-Web-App ein. Weitere Informationen zur Service Manager-Web-App finden Sie [hier](#create-and-deploy-service-manager-web-app-service).
| **Client-ID**   | Geben Sie die Client-ID ein, die Sie (mithilfe des automatischen Skripts) zum Authentifizieren der Web-App generiert haben. Weitere Informationen zum automatisierten Skript finden Sie [hier](../../azure-monitor/platform/itsmc-service-manager-script.md).|
| **Geheimer Clientschlüssel**   | Geben Sie den für diese ID generierten geheimen Clientschlüssel ein.   |
| **Daten synchronisieren**   | Wählen Sie die Service Manager-Arbeitselemente aus, die Sie über ITSMC synchronisieren möchten.  Diese Arbeitselemente werden in Log Analytics importiert. **Optionen:**  Incidents, Änderungsanforderungen.|
| **Datensynchronisierungsbereich** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximales Limit**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt Log Analytics die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert. |

![Service Manager-Verbindung](media/itsmc-connections/service-manager-connection.png)

**Bei erfolgreicher Verbindung und Synchronisierung**:

- Ausgewählte Arbeitselemente aus Service Manager werden in Azure **Log Analytics** importiert. Die Zusammenfassung dieser Arbeitselemente können Sie auf der IT Service Management Connector-Kachel anzeigen.

- Sie können Incidents über Log Analytics-Warnungen oder Protokolldatensätze oder über Azure-Warnungen in dieser Service Manager-Instanz erstellen.


Weitere Informationen: [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Erstellen und Bereitstellen des Service Manager-Web-App-Dienstes

Zur Verbindung der lokalen Service Manager-Instanz mit dem ITSMC in Azure hat Microsoft in GitHub eine Service Manager-Web-App erstellt.

Führen Sie die folgenden Schritte aus, um die ITSM-Web-App für Ihre Service Manager-Instanz einzurichten:

- **Bereitstellen der Web-App**: Stellen Sie die Web-App bereit, legen Sie die Eigenschaften fest, und führen Sie die Authentifizierung über Azure AD durch. Sie können die Web-App mithilfe des [automatisierten Skripts](../../azure-monitor/platform/itsmc-service-manager-script.md) von Microsoft bereitstellen.
- **Konfigurieren der Hybridverbindung** - [Konfigurieren Sie diese Verbindung](#configure-the-hybrid-connection)manuell.

#### <a name="deploy-the-web-app"></a>Bereitstellen der Web-App
Verwenden Sie das automatisierte [Skript](../../azure-monitor/platform/itsmc-service-manager-script.md) zum Bereitstellen der Web-App, Festlegen der Eigenschaften und Authentifizierung über Azure AD.

Führen Sie das Skript nach Bereitstellung der folgenden erforderlichen Details aus:

- Details zum Azure-Abonnement
- Ressourcengruppenname
- Location
- Details zum Service Manager-Server (Servername, Domäne, Benutzername und Kennwort)
- Präfix des Namens der Website für Ihre Web-App
- Service Bus-Namespace.

Das Skript erstellt die Web-App mit dem Namen, den Sie (zusammen mit einigen zusätzlichen Zeichenfolgen zum Sicherstellen der Eindeutigkeit) angegeben haben. Es generiert die **Web-App-URL**, **Client-ID** und das **Clientgeheimnis**.

Speichern Sie diese Werte, da Sie sie beim Erstellen einer Verbindung mit ITSMC verwenden.

**Überprüfen der Web-App-Installation**

1. Navigieren Sie zum **Azure-Portal** > **Ressourcen**.
2. Wählen Sie die Web-App aus, und klicken Sie auf **Einstellungen** > **Anwendungseinstellungen**.
3. Bestätigen Sie die Informationen zur Service Manager-Instanz, die Sie zum Zeitpunkt der App-Bereitstellung über das Skript angegeben haben.

### <a name="configure-the-hybrid-connection"></a>Konfigurieren der Hybridverbindung

Verwenden Sie das folgende Verfahren, um die Hybridverbindung zu konfigurieren, die die Service Manager-Instanz mit dem ITSMC in Azure verbindet.

1. Suchen Sie die Service Manager-Web-App unter **Azure-Ressourcen**.
2. Klicken Sie auf **Einstellungen** > **Netzwerk**.
3. Klicken Sie unter **Hybrid Connections** auf **Endpunkte der Hybridverbindung konfigurieren**.

    ![Hybridverbindung – Netzwerk](media/itsmc-connections/itsmc-hybrid-connection-networking-and-end-points.png)
4. Klicken Sie auf dem Blatt **Hybrid Connections** auf **Hybridverbindung hinzufügen**.

    ![Hybridverbindung hinzufügen](media/itsmc-connections/itsmc-new-hybrid-connection-add.png)

5. Klicken Sie auf dem Blatt **Hybridverbindungen hinzufügen** auf **Neue Hybridverbindung erstellen**.

    ![Neue Hybridverbindung](media/itsmc-connections/itsmc-create-new-hybrid-connection.png)

6. Geben Sie die folgenden Werte ein:

   - **Endpunktname**: Legen Sie einen Namen für die neue Hybridverbindung fest.
   - **Endpunkthost**: FQDN des Service Manager-Verwaltungsservers.
   - **Endpunktport**: Geben Sie 5724 ein.
   - **Service Bus-Namespace**: Verwenden Sie einen vorhandenen Service Bus-Namespace, oder erstellen Sie einen neuen.
   - **Standort:** Wählen Sie den Standort aus.
   - **Name**: Legen Sie einen Namen für den Service Bus fest, falls Sie ihn erstellen.

     ![Hybridverbindungswerte](media/itsmc-connections/itsmc-new-hybrid-connection-values.png)
6. Klicken Sie auf **OK**, um das Blatt **Hybridverbindung erstellen** zu schließen und mit der Erstellung der Hybridverbindung zu beginnen.

    Sobald die Hybridverbindung erstellt wurde, wird sie unter dem Blatt angezeigt.

7. Nachdem die Hybridverbindung erstellt wurde, wählen Sie die Verbindung aus, und klicken Sie auf **Ausgewählte Hybridverbindung hinzufügen**.

    ![Neue Hybridverbindung](media/itsmc-connections/itsmc-new-hybrid-connection-added.png)

#### <a name="configure-the-listener-setup"></a>Konfigurieren der Listener-Einrichtung

Verwenden Sie das folgende Verfahren, um die Listener-Einrichtung für die Hybridverbindung zu konfigurieren.

1. Klicken Sie auf dem Blatt **Hybrid Connections** auf **Download the Connection Manager** (Verbindungs-Manager herunterladen), und installieren Sie ihn auf dem Computer, auf dem die System Center Service Manager-Instanz ausgeführt wird.

    Nach Abschluss der Installation ist die Option **Hybrid Connection Manager UI** (Benutzeroberfläche des Managers für Hybridverbindungen) im **Startmenü** verfügbar.

2. Wenn Sie auf **Hybrid Connection Manager UI** (Benutzeroberfläche des Managers für Hybridverbindungen) klicken, werden Sie zur Eingabe Ihrer Azure-Anmeldeinformationen aufgefordert.

3. Melden Sie sich mit Ihren Azure-Anmeldeinformationen an, und wählen Sie Ihr Abonnement aus, in dem die Hybridverbindung erstellt wurde.

4. Klicken Sie auf **Speichern**.

Ihre Hybridverbindung wurde hergestellt.

![erfolgreiche Hybridverbindung](media/itsmc-connections/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]
> 
> Nach dem Herstellen der Hybridverbindung überprüfen testen Sie die Verbindung, indem Sie die bereitgestellte Service Manager-Web-App aufrufen. Stellen Sie sicher, dass die Verbindung erfolgreich ist, bevor Sie versuchen, in Azure eine Verbindung mit dem ITSMC herzustellen.

Die folgende Beispielabbildung zeigt die Details einer erfolgreichen Verbindung:

![Hybridverbindungstest](media/itsmc-connections/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-to-it-service-management-connector-in-azure"></a>Verbinden von ServiceNow mit dem ITSM-Connector in Azure

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres ServiceNow-Produkts mit dem ITSMC in Azure.

### <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:
- ITSMC wurde installiert. Weitere Informationen: [Hinzufügen der IT Service Management Connector-Lösung](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Von ServiceNow unterstützte Versionen: London, Kingston, Jakarta, Istanbul, Helsinki, Genf.

**ServiceNow-Administratoren müssen in ihrer ServiceNow-Instanz die folgenden Schritte ausführen**:
- Generieren der Client-ID und des geheimen Clientschlüssels für das ServiceNow-Produkt. Informationen zum Generieren der Client-ID und des geheimen Clientschlüssels finden Sie in dem für Sie zutreffenden Abschnitt:

    - [OAuth-Setup für London](https://docs.servicenow.com/bundle/london-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für Kingston](https://docs.servicenow.com/bundle/kingston-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für Jakarta](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für Istanbul](https://docs.servicenow.com/bundle/istanbul-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für Helsinki](https://docs.servicenow.com/bundle/helsinki-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für Geneva](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/security/task/t_SettingUpOAuth.html)


- Installieren der Benutzer-App für die Microsoft Log Analytics-Integration (ServiceNow-App). [Weitere Informationen](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1 )
- Erstellen der Benutzerrolle „Integration“ für die installierte Benutzer-App. Informationen zum Erstellen der Benutzerrolle „Integration“ finden Sie [hier](#create-integration-user-role-in-servicenow-app).

### <a name="connection-procedure"></a>**Verbindungsverfahren**
Verwenden Sie das folgende Verfahren, um eine ServiceNow-Verbindung zu erstellen:


1. Navigieren Sie im Azure-Portal zu **Alle Ressourcen**, und suchen Sie nach **ServiceDesk(YourWorkspaceName)** .

2.  Klicken Sie unter **ARBEITSBEREICHSDATENQUELLEN** auf **ITSM-Verbindungen**.
    ![Neue Verbindung](media/itsmc-connections/add-new-itsm-connection.png)

3. Klicken Sie oben im rechten Bereich auf **Hinzufügen**.

4. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **OK**, um die Verbindung zu erstellen.


> [!NOTE]
> Alle diese Parameter sind erforderlich.

| **Feld** | **Beschreibung** |
| --- | --- |
| **Verbindungsname**   | Geben Sie einen Namen für die ServiceNow-Instanz ein, die Sie mit ITSMC verbinden möchten.  Diesen Namen verwenden Sie später in Log Analytics beim Konfigurieren von Arbeitselementen in dieser ITSM-Instanz bzw. beim Anzeigen ausführlicher Protokollanalysen. |
| **Partner type** (Partnertyp)   | Wählen Sie **ServiceNow** aus. |
| **Benutzername**   | Geben Sie den Integrationsbenutzernamen ein, den Sie in der ServiceNow-App zur Unterstützung der Verbindung mit ITSMC erstellt haben. Weitere Informationen: [Erstellen der ServiceNow-App-Benutzerrolle](#create-integration-user-role-in-servicenow-app).|
| **Kennwort**   | Geben Sie das diesem Benutzernamen zugeordnete Kennwort ein. **Hinweis**: Benutzername und Kennwort werden nur zum Generieren von Authentifizierungstoken verwendet und werden nicht im ITSMC-Dienst gespeichert.  |
| **Server-URL**   | Geben Sie die URL der ServiceNow-Instanz ein, die Sie mit ITSMC verbinden möchten. |
| **Client-ID**   | Geben Sie die Client-ID ein, die Sie für die OAuth2-Authentifizierung verwenden möchten, die Sie zuvor generiert haben.  Weitere Informationen zum Generieren der Client-ID und des geheimen Clientschlüssels:   [OAuth-Setup](https://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Geheimer Clientschlüssel**   | Geben Sie den für diese ID generierten geheimen Clientschlüssel ein.   |
| **Datensynchronisierungsbereich**   | Wählen Sie die ServiceNow-Arbeitselemente aus, die Sie über den ITSMC mit Azure Log Analytics synchronisieren möchten.  Die ausgewählten Werte werden in Log Analytics importiert.   **Optionen:**  Incidents und Änderungsanforderungen.|
| **Daten synchronisieren** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximales Limit**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt der ITSMC die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert. |

![ServiceNow-Verbindung](media/itsmc-connections/itsm-connection-servicenow-connection-latest.png)

**Bei erfolgreicher Verbindung und Synchronisierung**:

- Ausgewählte Arbeitselemente aus der ServiceNow-Instanz werden in Azure **Log Analytics** importiert. Die Zusammenfassung dieser Arbeitselemente können Sie auf der IT Service Management Connector-Kachel anzeigen.

- Sie können Incidents über Log Analytics-Warnungen oder Protokolldatensätze oder über Azure-Warnungen in dieser ServiceNow-Instanz erstellen.

Weitere Informationen: [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-integration-user-role-in-servicenow-app"></a>Erstellen der Benutzerrolle „Integration“ in der ServiceNow-App

Gehen Sie dazu wie folgt vor:

1. Rufen Sie den [ServiceNow Store](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1) auf, und installieren Sie die **Benutzer-App für die ServiceNow- und Microsoft OMS-Integration** in Ihrer ServiceNow-Instanz.
   
   >[!NOTE]
   >Im Rahmen der laufenden Umstellung von der Microsoft Operations Management Suite (OMS) auf Azure Monitor wird OMS jetzt als Log Analytics bezeichnet.     
2. Nach der Installation suchen Sie in der linken Navigationsleiste der ServiceNow-Instanz nach dem Microsoft OMS-Integrator und wählen ihn aus.  
3. Klicken Sie auf **Installation Checklist** (Installationsprüfliste).

   Der Status **Nicht abgeschlossen** wird angezeigt, wenn die Benutzerrolle noch nicht erstellt wurde.

4. Geben Sie in den Textfeldern neben **Create integration user** (Integrationsbenutzer erstellen) den Benutzernamen für den Benutzer ein, der in Azure eine Verbindung mit dem ITSMC herstellen kann.
5. Geben Sie das Kennwort für diesen Benutzer ein, und klicken Sie auf **OK**.  

> [!NOTE]
> 
> Diese Anmeldeinformationen verwenden Sie zum Herstellen der ServiceNow-Verbindung in Azure.

Der neu erstellte Benutzer wird mit den zugewiesenen Standardrollen angezeigt.

**Standardrollen**:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.user
-   itil
-   template_editor
-   view_changer

Sobald der Benutzer erfolgreich erstellt wurde, wechselt der Status von **Check Installation Checklist** (Installationsprüfliste überprüfen) zu „Abgeschlossen“, und die Details der für die App erstellten Benutzerrolle werden aufgeführt.

> [!NOTE]
> 
> Der ITSM-Connector kann Vorfälle an ServiceNow senden, ohne dass andere Module auf Ihrer ServiceNow-Instanz installiert sind. Wenn Sie das EventManagement-Modul in Ihrer ServiceNow-Instanz verwenden und Ereignisse oder Warnungen mit dem Connector in ServiceNow erstellen möchten, fügen Sie dem Integrationsbenutzer die folgenden Rollen hinzu:
> 
>    - evt_mgmt_integration
>    - evt_mgmt_operator  


## <a name="connect-provance-to-it-service-management-connector-in-azure"></a>Verbinden von Provance mit dem ITSM-Connector in Azure

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres Provance-Produkts mit dem ITSMC in Azure.


### <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:


- ITSMC wurde installiert. Weitere Informationen: [Hinzufügen der IT Service Management Connector-Lösung](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Die Provance-App muss bei Azure AD registriert sein, und die Client-ID muss verfügbar gemacht werden. Ausführliche Informationen finden Sie unter [Konfigurieren der Active Directory-Authentifizierung](../../app-service/configure-authentication-provider-aad.md).

- Benutzerrolle:  Administrator.

### <a name="connection-procedure"></a>Verbindungsverfahren

Verwenden Sie das folgende Verfahren, um eine Provance-Verbindung zu erstellen:

1. Navigieren Sie im Azure-Portal zu **Alle Ressourcen**, und suchen Sie nach **ServiceDesk(YourWorkspaceName)** .

2.  Klicken Sie unter **ARBEITSBEREICHSDATENQUELLEN** auf **ITSM-Verbindungen**.
    ![Neue Verbindung](media/itsmc-connections/add-new-itsm-connection.png)

3. Klicken Sie oben im rechten Bereich auf **Hinzufügen**.

4. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **OK**, um die Verbindung zu erstellen.

> [!NOTE]
> 
> Alle diese Parameter sind erforderlich.

| **Feld** | **Beschreibung** |
| --- | --- |
| **Verbindungsname**   | Geben Sie einen Namen für die Provenance-Instanz ein, die Sie mit ITSMC verbinden möchten.  Diesen Namen verwenden Sie später beim Konfigurieren von Arbeitselementen in dieser ITSM-Instanz/beim Anzeigen ausführlicher Protokollanalysen. |
| **Partner type** (Partnertyp)   | Wählen Sie **Provance** aus. |
| **Benutzername**   | Geben Sie den Namen des Benutzers ein, der eine Verbindung mit ITSMC herstellen kann.    |
| **Kennwort**   | Geben Sie das diesem Benutzernamen zugeordnete Kennwort ein. **Hinweis:** Benutzername und Kennwort werden nur zum Generieren von Authentifizierungstoken verwendet und werden nicht im ITSMC-Dienst gespeichert._|
| **Server-URL**   | Geben Sie die URL der Provenance-Instanz ein, die Sie mit ITSMC verbinden möchten. |
| **Client-ID**   | Geben Sie die Client-ID für die Authentifizierung dieser Verbindung ein, die Sie in Ihrer Provance-Instanz generiert haben.  Weitere Informationen zur Client-ID finden Sie unter [Konfigurieren der Active Directory-Authentifizierung](../../app-service/configure-authentication-provider-aad.md). |
| **Datensynchronisierungsbereich**   | Wählen Sie die Provance-Arbeitselemente aus, die Sie über den ITSMC mit Azure Log Analytics synchronisieren möchten.  Diese Arbeitselemente werden in Log Analytics importiert.   **Optionen:**   Incidents, Änderungsanforderungen.|
| **Daten synchronisieren** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximales Limit**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt der ITSMC die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert.|

![Provance-Verbindung](media/itsmc-connections/itsm-connections-provance-latest.png)

**Bei erfolgreicher Verbindung und Synchronisierung**:

- Ausgewählte Arbeitselemente aus der Provance-Instanz werden in Azure **Log Analytics** importiert. Die Zusammenfassung dieser Arbeitselemente können Sie auf der IT Service Management Connector-Kachel anzeigen.

- Sie können Incidents über Log Analytics-Warnungen oder Protokolldatensätze oder über Azure-Warnungen in dieser Provance-Instanz erstellen.

Weitere Informationen: [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

## <a name="connect-cherwell-to-it-service-management-connector-in-azure"></a>Verbinden von Cherwell mit dem ITSM-Connector in Azure

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres Cherwell-Produkts mit dem ITSMC in Azure.

### <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:

- ITSMC wurde installiert. Weitere Informationen: [Hinzufügen der IT Service Management Connector-Lösung](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Client-ID wurde generiert. Weitere Informationen: [Generieren der Client-ID für Cherwell](#generate-client-id-for-cherwell).
- Benutzerrolle:  Administrator.

### <a name="connection-procedure"></a>Verbindungsverfahren

Verwenden Sie das folgende Verfahren, um eine Provance-Verbindung zu erstellen:

1. Navigieren Sie im Azure-Portal zu **Alle Ressourcen**, und suchen Sie nach **ServiceDesk(YourWorkspaceName)** .

2.  Klicken Sie unter **ARBEITSBEREICHSDATENQUELLEN** auf **ITSM-Verbindungen**.
    ![Neue Verbindung](media/itsmc-connections/add-new-itsm-connection.png)

3. Klicken Sie oben im rechten Bereich auf **Hinzufügen**.

4. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **OK**, um die Verbindung zu erstellen.

> [!NOTE]
> 
> Alle diese Parameter sind erforderlich.

| **Feld** | **Beschreibung** |
| --- | --- |
| **Verbindungsname**   | Geben Sie einen Namen für die Cherwell-Instanz ein, die Sie mit ITSMC verbinden möchten.  Diesen Namen verwenden Sie später beim Konfigurieren von Arbeitselementen in dieser ITSM-Instanz/beim Anzeigen ausführlicher Protokollanalysen. |
| **Partner type** (Partnertyp)   | Wählen Sie **Cherwell** aus. |
| **Benutzername**   | Geben Sie den Namen des Cherwell-Benutzers ein, der eine Verbindung mit ITSMC herstellen kann. |
| **Kennwort**   | Geben Sie das diesem Benutzernamen zugeordnete Kennwort ein. **Hinweis:** Benutzername und Kennwort werden nur zum Generieren von Authentifizierungstoken verwendet und werden nicht im ITSMC-Dienst gespeichert.|
| **Server-URL**   | Geben Sie die URL für die Cherwell-Instanz ein, die Sie mit ITSMC verbinden möchten. |
| **Client-ID**   | Geben Sie die Client-ID für die Authentifizierung dieser Verbindung ein, die Sie in Ihrer Cherwell-Instanz generiert haben.   |
| **Datensynchronisierungsbereich**   | Wählen Sie die Cherwell-Arbeitselemente aus, die Sie über ITSMC synchronisieren möchten.  Diese Arbeitselemente werden in Log Analytics importiert.   **Optionen:**  Incidents, Änderungsanforderungen. |
| **Daten synchronisieren** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximales Limit**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt der ITSMC die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert. |


![Provance-Verbindung](media/itsmc-connections/itsm-connections-cherwell-latest.png)

**Bei erfolgreicher Verbindung und Synchronisierung**:

- Ausgewählte Arbeitselemente aus der Cherwell-Instanz werden in Azure **Log Analytics** importiert. Die Zusammenfassung dieser Arbeitselemente können Sie auf der IT Service Management Connector-Kachel anzeigen.

- Sie können Incidents über Log Analytics-Warnungen oder Protokolldatensätze oder über Azure-Warnungen in dieser Cherwell-Instanz erstellen.

Weitere Informationen: [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="generate-client-id-for-cherwell"></a>Generieren der Client-ID für Cherwell

Führen Sie folgende Schritte aus, um die Client-ID/den Schlüssel für Cherwell zu generieren:

1. Melden Sie sich als Administrator bei Ihrer Cherwell-Instanz an.
2. Klicken Sie auf **Sicherheit** > **Edit REST API client settings (REST-API-Clienteinstellungen bearbeiten)** .
3. Wählen Sie **Create new client (Neuen Client erstellen)**  > **Geheimer Clientschlüssel**.

    ![Cherwell-Benutzer-ID](media/itsmc-connections/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Nächste Schritte
 - [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts)
