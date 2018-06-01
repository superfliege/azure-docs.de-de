---
title: Bereitstellen von Azure Blockchain Workbench
description: Bereitstellen von Azure Blockchain Workbench
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/17/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 484c7a17fec4ee94e3170e93eb1438af688d101e
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
ms.locfileid: "34303942"
---
# <a name="deploy-azure-blockchain-workbench"></a>Bereitstellen von Azure Blockchain Workbench

Azure Blockchain Workbench wird mithilfe einer Projektmappenvorlage im Azure Marketplace bereitgestellt. Die Vorlage vereinfacht die Bereitstellung von Komponenten, die für die Erstellung von Blockchainanwendungen benötigt werden. Nach der Bereitstellung bietet Blockchain Workbench Zugriff auf Client-Apps zum Erstellen und Verwalten von Benutzern und Blockchainanwendungen.

Weitere Informationen zu den Komponenten der Blockchain Workbench finden Sie unter [Azure Blockchain Workbench-Architektur](blockchain-workbench-architecture.md).

## <a name="prepare-for-deployment"></a>Vorbereiten der Bereitstellung

Blockchain Workbench ermöglicht die Bereitstellung eines Blockchainledgers zusammen mit einer Gruppe relevanter Azure-Dienste, die am häufigsten für die Erstellung einer blockchainbasierten Anwendung verwendet wird. Bei der Bereitstellung von Blockchain Workbench werden die folgenden Azure-Dienste innerhalb einer Ressourcengruppe im Azure-Abonnement bereitgestellt.

* 1 Event Grid-Thema
* 1 Service Bus-Namespace
* 1 Application Insights
* 1 SQL-Datenbank (Standard S0)
* 2 App Services-Dienste (Standard)
* 2 Azure Key Vault-Instanzen
* 2 Azure Storage-Konten (Standard LRS)
* 2 VM-Skalierungsgruppen (für Validierungssteuerelement und Workerknoten)
* 2 virtuelle Netzwerke (einschließlich Lastenausgleichsmodul, Netzwerksicherheitsgruppe und öffentliche IP-Adresse für jedes virtuelle Netzwerk)
* Optional: Azure Monitor

Im Folgenden wird eine Beispielbereitstellung gezeigt, die in der Ressourcengruppe **myblockchain** erstellt wurde.

![Beispielbereitstellung](media/blockchain-workbench-deploy/example-deployment.png)

Die Kosten für Blockchain Workbench stellen eine Aggregation der Kosten der zugrunde liegenden Azure-Dienste dar. Preise zu Azure-Dienste können mit dem [Preisrechner](https://azure.microsoft.com/pricing/calculator/) berechnet werden.

Vor der Bereitstellung von Azure Blockchain Workbench müssen mehrere Voraussetzungen erfüllt sein. Zu den Voraussetzungen gehören die Azure AD-Konfiguration und Anwendungsregistrierungen.

### <a name="blockchain-workbench-api-app-registration"></a>Blockchain Workbench-API-App-Registrierung

Die Blockchain Workbench-Bereitstellung erfordert die Registrierung einer Azure AD-Anwendung. Sie benötigen einen Azure Active Directory (Azure AD)-Mandanten, um die App zu registrieren. Sie können einen bestehenden Mandanten verwenden oder einen neuen Mandanten anlegen. Wenn Sie einen bestehenden Azure AD-Mandanten verwenden, benötigen Sie ausreichende Berechtigungen, um Anwendungen innerhalb eines Azure AD-Mandanten zu registrieren. Die Anwendungsregistrierungen müssen sich im Mandanten des Abonnementadministrators befinden, in dem die Workbench bereitgestellt ist. Weitere Informationen über Azure AD-Mandanten finden Sie unter [Einrichten eines Azure Active Directory-Mandanten](../active-directory/develop/active-directory-howto-tenant.md) und [Integrieren von Anwendungen in Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie oben rechts Ihr Konto aus, und wechseln Sie zum gewünschten Azure AD-Mandanten. Der Mandant sollte der Mandant des Abonnementadministrators sein, in dem die Workbench installiert ist. Zudem müssen Sie über ausreichende Berechtigungen verfügen, um Anwendungen zu registrieren.
3. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus. Wählen Sie **App-Registrierungen** > **Registrierung einer neuen Anwendung**.

    ![App-Registrierung](media/blockchain-workbench-deploy/app-registration.png)

4. Geben Sie einen **Namen** und eine **Anmelde-URL** an. Sie können Platzhalterwerte verwenden, da die Werte während der Bereitstellung geändert werden. 

    ![Erstellen der App-Registrierung](media/blockchain-workbench-deploy/app-registration-create.png)

    |Einstellung  | Wert  |
    |---------|---------|
    |NAME | `Blockchain API` |
    |Anwendungstyp |Web-App/API|
    |Anmelde-URL | `https://blockchainapi` |

5. Wählen Sie **Erstellen**, um die Azure AD-Anwendung zu registrieren.

### <a name="modify-application-manifest"></a>Ändern des Anwendungsmanifests

Als Nächstes müssen Sie das Anwendungsmanifest für die Verwendung von Anwendungsrollen innerhalb von Azure AD ändern, um Blockchain Workbench-Administratoren festzulegen.  Weitere Informationen zu Anwendungsmanifesten finden Sie unter [Azure Active Directory-Anwendungsmanifest](../active-directory/develop/active-directory-application-manifest.md).

1. Wählen Sie für die Anwendung, die Sie registriert haben, im Detailbereich der registrierten Anwendung **Manifest** aus.
2. Generieren Sie eine GUID. Sie können den PowerShell-Befehl `[guid]::NewGuid()` oder Onlinetools verwenden, um eine GUID zu generieren. 
3. Sie werden den Abschnitt **appRoles** des Manifests aktualisieren. Wählen Sie im Bereich „Manifest bearbeiten“ die Option **Bearbeiten**, und ersetzen Sie `"appRoles": []` mit der gegebenen JSON-Datei. Stellen Sie sicher, dass Sie den Wert für das Feld **ID** durch die von Ihnen generierte GUID ersetzen. 

    ``` json
    "appRoles": [
         {
           "allowedMemberTypes": [
             "User",
             "Application"
           ],
           "displayName": "Administrator",
           "id": "<A unique GUID>",
           "isEnabled": true,
           "description": "Blockchain Workbench administrator role allows creation of applications, user to role assignments, etc.",
           "value": "Administrator"
         }
       ],
    ```

    ![Bearbeiten des Manifests](media/blockchain-workbench-deploy/edit-manifest.png)

    > [!IMPORTANT]
    > Der Wert **Administrator** wird benötigt, um Blockchain Workbench-Administratoren zu identifizieren.

4.  Klicken Sie auf **Speichern**, um die Änderungen am Anwendungsmanifest zu speichern.

### <a name="add-graph-api-required-permissions"></a>Hinzufügen der erforderlichen Berechtigungen für die Graph-API

Der Benutzer muss zulassen, dass die API-Anwendung auf das Verzeichnis zuzugreifen darf. Legen Sie die folgenden erforderlichen Berechtigungen für die API-Anwendung fest:

1. Wählen Sie in der Registrierung der Blockchain-API-App **Einstellungen > Erforderliche Berechtigungen > API auswählen > Microsoft Graph**.

    ![Auswählen einer API](media/blockchain-workbench-deploy/client-app-select-api.png)

    Klicken Sie auf **Auswählen**.

2. Wählen Sie in **Zugriff gewähren** unter **Anwendungsberechtigungen** die Option **Vollständige Profile aller Benutzer lesen**.

    ![Gewähren des Zugriffs](media/blockchain-workbench-deploy/client-app-read-perms.png)

    Klicken Sie auf **Auswählen**, und dann auf **Fertig**.

3. Wählen Sie unter **Erforderliche Berechtigungen** die Option **Berechtigungen erteilen**, und wählen Sie dann zur Bestätigung **Ja**.

   ![Erteilen von Berechtigungen](media/blockchain-workbench-deploy/client-app-grant-permissions.png)

   Durch das Erteilen von Berechtigungen kann die Blockchain Workbench auf Benutzer im Verzeichnis zugreifen. Die Leseberechtigung ist erforderlich, um Mitglieder in der Blockchain Workbench zu suchen und hinzuzufügen.

### <a name="add-graph-api-key-to-application"></a>Hinzufügen eines Graph-API-Schlüssels zur Anwendung

Blockchain Workbench verwendet Azure AD als Hauptsystem für die Identitätsverwaltung für Benutzer, die mit Blockchainanwendungen interagieren. Damit Blockchain Workbench auf Azure AD zugreifen und Benutzerinformationen wie Namen und E-Mails abrufen kann, müssen Sie einen Zugriffsschlüssel hinzufügen. Blockchain Workbench verwendet den Schlüssel für die Authentifizierung bei Azure AD.

1. Wählen Sie für die Anwendung, die Sie registriert haben, im Detailbereich der registrierten Anwendung **Einstellungen** aus.
2. Wählen Sie **Schlüssel** aus.
3. Fügen Sie einen neuen Schlüssel hinzu, indem Sie eine **Beschreibung** des Schlüssels angeben und den Wert für **Gültig bis** wählen. 

    ![Erstellen eines Schlüssels](media/blockchain-workbench-deploy/app-key-create.png)

    |Einstellung  | Wert  |
    |---------|---------|
    | BESCHREIBUNG | `Service` |
    | Expires | Wählen Sie eine Ablaufdauer. |

4. Wählen Sie **Speichern**aus. 
5. Kopieren Sie den Wert des Schlüssels, und speichern Sie ihn für später. Sie brauchen ihn für die Bereitstellung.

    > [!IMPORTANT]
    >  Wenn Sie den Schlüssel für die Bereitstellung nicht speichern, müssen Sie einen neuen Schlüssel generieren. Sie können den Schlüsselwert später nicht mehr aus dem Portal abrufen.

### <a name="get-application-id"></a>Abrufen der Anwendungs-ID

Die Anwendungs-ID und die Mandateninformationen werden für die Bereitstellung benötigt. Sammeln und speichern Sie die Informationen für die Verwendung während der Bereitstellung.

1. Wählen Sie für die Anwendung, die Sie registriert haben, **Einstellungen** > **Eigenschaften** aus.
2.  Kopieren und speichern Sie im Bereich **Eigenschaften** die folgenden Werte für die spätere Verwendung während der Bereitstellung.

    ![API-App-Eigenschaften](media/blockchain-workbench-deploy/app-properties.png)

    | Zu speichernde Einstellung  | Verwendung bei der Bereitstellung |
    |------------------|-------------------|
    | Anwendungs-ID | Azure Active Directory-Setup > Anwendungs-ID |

### <a name="get-tenant-domain-name"></a>Abrufen des Domänennamens des Mandanten

Sammeln und speichern Sie den Domänennamen des Active Directory-Mandanten, unter dem die Anwendungen registriert sind. 

Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus. Wählen Sie **Benutzerdefinierte Domänennamen**. Kopieren und speichern Sie den Domänennamen.

![Domänenname](media/blockchain-workbench-deploy/domain-name.png)

## <a name="deploy-blockchain-workbench"></a>Bereitstellen von Blockchain Workbench

Wenn die erforderlichen Schritte abgeschlossen sind, können Sie die Blockchain Workbench bereitstellen. In den folgenden Abschnitten werden die Schritte zur Bereitstellung des Frameworks beschrieben.

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2.  Wählen Sie oben rechts Ihr Konto aus, und wechseln Sie zum gewünschten Azure AD-Mandanten, in dem Sie Azure Blockchain Workbench bereitstellen möchten.
3.  Wählen Sie im linken Bereich **Ressource erstellen**. Suchen Sie nach `Azure Blockchain Workbench` in der Suchleiste **Marketplace durchsuchen**. 

    ![Marketplace-Suchleiste](media/blockchain-workbench-deploy/marketplace-search-bar.png)

4.  Wählen Sie **Azure Blockchain Workbench** aus.

    ![Marketplace-Suchergebnisse](media/blockchain-workbench-deploy/marketplace-search-results.png)

4.  Klicken Sie auf **Erstellen**.
5.  Nehmen Sie die grundlegenden Einstellungen vor.

    ![Erstellen der Azure Blockchain Workbench](media/blockchain-workbench-deploy/blockchain-workbench-settings-basic.png)

    | Einstellung | BESCHREIBUNG  |
    |---------|--------------|
    | Ressourcenpräfix | Kurzer eindeutiger Bezeichner für Ihre Bereitstellung. Dieser Wert wird als Grundlage bei der Benennung von Ressourcen verwendet. |
    | VM-Benutzername | Der Benutzername wird als Administrator für alle virtuellen Computer (VMs) verwendet. |
    | Authentifizierungsart | Wählen Sie diese aus, wenn Sie ein Kennwort oder einen Schlüssel für die Verbindung mit VMs nutzen möchten. |
    | Password | Das Kennwort wird für die Verbindung mit VMs verwendet. |
    | SSH | Verwenden Sie einen öffentlichen RSA-Schlüssel im einzeiligen Format beginnend mit **ssh-rsa**, oder verwenden Sie das mehrzeilige PEM-Format. Sie können SSH-Schlüssel mit `ssh-keygen` unter Linux und OS X oder mit PuTTYGen unter Windows erzeugen. Weitere Informationen zu SSH-Schlüsseln finden Sie unter [Verwenden von SSH-Schlüsseln mit Windows in Azure](../virtual-machines/linux/ssh-from-windows.md). |
    | Datenbankkennwort / Datenbankkennwort bestätigen | Geben Sie das Kennwort für den Zugriff auf die im Rahmen der Bereitstellung erstellte Datenbank an. |
    | Bereitstellungsregion | Geben Sie an, wo die Blockchain Workbench-Ressourcen bereitgestellt werden sollen. Für optimale Verfügbarkeit sollte dies der Einstellung **Ort** entsprechen. |
    | Abonnement | Geben Sie das Azure-Abonnement an, das Sie für Ihre Bereitstellung verwenden möchten. |
    | Ressourcengruppen | Erstellen Sie eine Ressourcengruppe, indem Sie **Neu erstellen** auswählen, und geben Sie einen eindeutigen Namen für die Ressourcengruppe ein. |
    | Speicherort | Geben Sie die Region an, in der Sie das Framework bereitstellen möchten. |

6.  Wählen Sie **OK**, um die Konfiguration der Grundeinstellungen fertig zu stellen.

7.  Schließen Sie das **Azure Active Directory-Setup** ab.

    ![Azure AD-Setup](media/blockchain-workbench-deploy/blockchain-workbench-settings-aad.png)

    | Einstellung | BESCHREIBUNG  |
    |---------|--------------|
    | Domänenname | Verwenden Sie den Azure AD-Mandanten, der im Abschnitt [Abrufen des Domänennamens des Mandanten](#get-tenant-domain-name) als Voraussetzung gesammelt wurde. |
    | Anwendungs-ID | Verwenden Sie die Anwendungs-ID aus der Blockchain-Client-App-Registrierung, die im Abschnitt [Abrufen der Anwendungs-ID](#get-application-id) als Voraussetzung gesammelt wurde. |
    | Anwendungsschlüssel | Verwenden Sie den Anwendungsschlüssel aus der Blockchain-Client-App-Registrierung, der im Abschnitt [Hinzufügen eines Graph-API-Schlüssels zur Anwendung](#add-graph-api-key-to-application) als Voraussetzung gesammelt wurde. |


8.  Klicken Sie auf **OK**, um die Konfiguration der Azure AD-Parameter fertig zu stellen.

9.  Schließen Sie die Einstellungen für **Netzwerkgröße und -leistung** ab.

    ![Netzwerk- und Leistungseinstellungen](media/blockchain-workbench-deploy/blockchain-workbench-settings-network.png)

    | Einstellung | BESCHREIBUNG  |
    |---------|--------------|
    | Anzahl von Blockchainknoten | Wählen Sie die Anzahl der Knoten von Ethereum PoA-Validierungssteuerelementen, die in Ihrem Netzwerk bereitgestellt werden sollen. |
    | Speicherleistung | Wählen Sie die bevorzugte VM-Speicherleistung für Ihr Blockchainnetzwerk. |
    | Größe des virtuellen Computers | Wählen Sie die bevorzugte VM-Größe für Ihr Blockchainnetzwerk. |

10. Wählen Sie **OK**, um den Abschnitt zu Netzwerkgröße und -leistung fertig zu stellen.

11. Schließen Sie die **Azure Monitor**-Einstellungen ab.

    ![Azure Monitor](media/blockchain-workbench-deploy/blockchain-workbench-settings-oms.png)

    | Einstellung | BESCHREIBUNG  |
    |---------|--------------|
    | Überwachung | Wählen Sie, ob Sie Azure Monitor zur Überwachung Ihres Blockchainnetzwerks aktivieren möchten. |
    | Herstellen einer Verbindung mit vorhandener Log Analytics-Instanz | Wählen Sie, ob Sie eine vorhandene Log Analytics-Instanz verwenden oder eine neue Instanz erstellen möchten. Bei Verwendung einer vorhandenen Instanz geben Sie Ihre Arbeitsbereichs-ID und Ihren Primärschlüssel ein. |

12. Klicken Sie auf **OK**, um den Abschnitt zu Azure Monitor fertig zu stellen.

13. Überprüfen Sie die Zusammenfassung, um sicherzustellen, dass die eingegebenen Parameter richtig sind.

    ![Zusammenfassung](media/blockchain-workbench-deploy/blockchain-workbench-summary.png)

14. Wählen Sie **Erstellen** aus, um den Bedingungen zuzustimmen und Ihre Azure Blockchain Workbench bereitzustellen.

Die Bereitstellung kann bis zu 90 Minuten dauern. Den Fortschritt können Sie im Azure-Portal überwachen. Wählen Sie in der neu erstellten Ressourcengruppe **Bereitstellungen > Übersicht**, um den Status der bereitgestellten Artefakte anzuzeigen.

## <a name="blockchain-workbench-web-url"></a>Blockchain Workbench-Web-URL

Wenn die Bereitstellung der Blockchain Workbench abgeschlossen ist, sind Ihre Blockchain Workbench-Ressourcen in einer neuen Ressourcengruppe enthalten. Der Zugriff auf die Blockchain Workbench-Dienste erfolgt über eine Web-URL. In den folgenden Schritten wird beschrieben, wie Sie die Web-URL des bereitgestellten Frameworks abrufen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im linken Navigationsbereich **Ressourcengruppen** aus.
3. Wählen Sie den Namen der Ressourcengruppe, den Sie bei der Bereitstellung der Blockchain Workbench angegeben haben.
4. Klicken Sie auf die Spaltenüberschrift **TYP**, um die Liste alphabetisch nach Typ zu sortieren.
5. Es gibt zwei Ressourcen vom Typ **App-Dienst**. Wählen Sie die Ressource vom Typ **App-Dienst** *ohne* das Suffix „-api“.

    ![Liste der App-Dienste](media/blockchain-workbench-deploy/resource-group-list.png)

6.  Kopieren Sie im Abschnitt **Zusammenfassung** zum App-Dienst den **URL**-Wert, der die Web-URL darstellt, in Ihre bereitgestellte Blockchain Workbench.

    ![Zusammenfassung zum App-Dienst](media/blockchain-workbench-deploy/app-service.png)

Informationen zum Verknüpfen eines benutzerdefinierten Domänennamens mit Blockchain Workbench finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-App in Azure App Services, der Traffic Manager verwendet](../app-service/web-sites-traffic-manager-custom-domain-name.md).

## <a name="configuring-the-reply-url"></a>Konfiguration der Antwort-URL

Nachdem die Azure Blockchain Workbench bereitgestellt wurde, muss im nächsten Schritt sichergestellt werden, dass die Azure AD-Clientanwendung für die korrekte **Antwort-URL** der bereitgestellten Blockchain Workbench-Web-URL registriert ist.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Vergewissern Sie sich, dass Sie sich im Mandanten befinden, bei dem Sie die Azure AD-Clientanwendung registriert haben.
3. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus. Wählen Sie **App-Registrierungen** aus.
4. Wählen Sie die Azure AD-Clientanwendung aus, die Sie im Abschnitt für die Voraussetzungen registriert haben.
5. Wählen Sie **Einstellungen > Antwort-URLs** aus.
6. Geben Sie die Haupt-Web-URL der Azure Blockchain Workbench-Bereitstellung an, die Sie im Abschnitt zur **Azure Blockchain Workbench-Web-URL** abgerufen haben. Die Antwort-URL weist das Präfix `https://` auf. Zum Beispiel, `https://myblockchain2-7v75.azurewebsites.net`

    ![Antwort-URLs](media/blockchain-workbench-deploy/configure-reply-url.png)

7. Wählen Sie **Speichern** aus, um die Clientregistrierung zu aktualisieren.

## <a name="remove-a-deployment"></a>Entfernen einer Bereitstellung

Wenn eine Bereitstellung nicht mehr benötigt wird, können Sie eine Bereitstellung durch Löschen der Blockchain Workbench-Ressourcengruppe entfernen.

1. Navigieren Sie im Azure-Portal im linken Navigationsbereich zu **Ressourcengruppe**, und wählen Sie die Ressourcengruppe aus, die gelöscht werden soll. 
2. Wählen Sie die Option **Ressourcengruppe löschen**. Überprüfen Sie den Löschvorgang, indem Sie den Ressourcengruppennamen eingeben und auf **Löschen** klicken.

    ![Ressourcengruppe löschen](media/blockchain-workbench-deploy/delete-resource-group.png)

## <a name="next-steps"></a>Nächste Schritte

Mithilfe der Anleitung in diesem Artikel haben Sie Azure Blockchain Workbench bereitgestellt. Informationen zum Erstellen einer Blockchainanwendung finden Sie in der nächsten Anleitung.

> [!div class="nextstepaction"]
> [Erstellen einer Blockchainanwendung in Azure Blockchain Workbench](blockchain-workbench-create-app.md)