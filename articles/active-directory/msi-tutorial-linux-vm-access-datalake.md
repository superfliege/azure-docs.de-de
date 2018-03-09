---
title: "Verwenden einer verwalteten Dienstidentität für einen virtuellen Linux-Computer für den Zugriff auf Azure Data Lake Store"
description: "Dieses Tutorial veranschaulicht die Verwendung einer verwalteten Dienstidentität (Managed Service Identity, MSI) für einen virtuellen Linux-Computer für den Zugriff auf Azure Data Lake Store."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: skwan
ms.openlocfilehash: fdf1c49c644a97ff2f66a8b217ee54896ed54131
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
---
# <a name="use-managed-service-identity-for-a-linux-vm-to-access-azure-data-lake-store"></a>Verwenden einer verwalteten Dienstidentität für einen virtuellen Linux-Computer für den Zugriff auf Azure Data Lake Store

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine verwaltete Dienstidentität für einen virtuellen Linux-Computer für den Zugriff auf Azure Data Lake Store verwenden. Azure verwaltet von Ihnen erstellte Identitäten automatisch über MSI. Sie können MSI verwenden, um sich bei Diensten mit Unterstützung für die Azure Active Directory-Authentifizierung (Azure AD) zu authentifizieren, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Aktivieren von MSI auf einem virtuellen Linux-Computer 
> * Gewähren des Zugriffs auf Azure Data Lake Store durch Ihren virtuellen Computer
> * Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwenden dieses Zugriffstokens für den Zugriff auf Azure Data Lake Store

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Linux-Computers in einer neuen Ressourcengruppe

In diesem Tutorial wird ein neuer virtueller Linux-Computer erstellt. Sie können MSI auch auf einem vorhandenen virtuellen Computer aktivieren.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
2. Wählen Sie **Compute** und dann **Ubuntu Server 16.04 LTS**.
3. Geben Sie die Informationen zum virtuellen Computer ein. Wählen Sie unter **Authentifizierungstyp** die Option **Öffentlicher SSH-Schlüssel** oder **Kennwort**. Mit den erstellten Anmeldeinformationen können Sie sich auf dem virtuellen Computer anmelden.

   ![Bereich „Grundeinstellungen“ zum Erstellen eines virtuellen Computers](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Wählen Sie in der Liste **Abonnement** ein Abonnement für den virtuellen Computer aus.
5. Um eine neue Ressourcengruppe auszuwählen, in der der virtuelle Computer erstellt werden soll, klicken Sie auf **Ressourcengruppe** > **Neu erstellen**. Wenn Sie fertig sind, klicken Sie auf **OK**.
6. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. Behalten Sie im Bereich „Einstellungen“ die Standardwerte bei, und klicken Sie auf **OK**.

## <a name="enable-msi-on-your-vm"></a>Aktivieren von MSI auf dem virtuellen Computer

Sie können MSI für einen virtuellen Computer verwenden, um Zugriffstoken aus Azure AD abzurufen, ohne Anmeldeinformationen in Ihren Code einfügen zu müssen. Durch das Aktivieren von MSI wird die MSI-VM-Erweiterung auf dem virtuellen Computer installiert, und MSI wird im Azure Resource Manager aktiviert.  

1. Wählen Sie unter **Virtueller Computer** den virtuellen Computer aus, auf dem Sie MSI aktivieren möchten.
2. Wählen Sie im linken Bereich die Option **Konfiguration**.
3. Die Option **Verwaltete Dienstidentität** wird angezeigt. Klicken Sie zum Registrieren und Aktivieren von MSI auf **Ja**. Klicken Sie zum Deaktivieren auf **Nein**.
   ![Auswahl „Registrierung bei Azure Active Directory“](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)
4. Wählen Sie **Speichern**aus.
5. Wenn Sie überprüfen möchten, welche Erweiterungen sich auf diesem virtuellen Linux-Computer befinden, klicken Sie auf **Erweiterungen**. Wenn MSI aktiviert ist, wird **ManagedIdentityExtensionforLinux** in der Liste angezeigt.

   ![Liste der Erweiterungen](media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Gewähren des Zugriffs auf Azure Data Lake Store durch Ihre VM

Sie können Ihrem virtuellen Computer nun Zugriff auf Dateien und Ordner in Azure Data Lake Store gewähren. Für diesen Schritt können Sie eine vorhandene Data Lake Store-Instanz verwenden oder eine neue erstellen. Um im Azure-Portal eine Data Lake Store-Instanz zu erstellen, führen Sie diesen [Schnellstart zu Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) durch. Es gibt in der [Dokumentation zu Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) auch Schnellstarts, in denen die Azure-Befehlszeilenschnittstelle und Azure PowerShell verwendet werden.

Erstellen Sie in Data Lake Store einen neuen Ordner, und gewähren Sie MSI die Berechtigungen zum Lesen, Schreiben und Ausführen von Dateien in diesem Ordner:

1. Wählen Sie im Azure-Portal im linken Bereich **Data Lake Store**.
2. Wählen Sie die Data Lake Store-Instanz aus, die Sie verwenden möchten.
3. Klicken Sie auf der Befehlsleiste auf **Daten-Explorer**.
4. Der Stammordner der Data Lake Store-Instanz ist ausgewählt. Klicken Sie auf der Befehlsleiste auf **Zugriff**.
5. Wählen Sie **Hinzufügen**.  Geben Sie im Feld **Auswählen** den Namen Ihres virtuellen Computers ein, z.B. **DevTestVM**. Klicken Sie in den Suchergebnissen auf Ihren virtuellen Computer und dann auf **Auswählen**.
6. Klicken Sie auf **Berechtigungen auswählen**.  Wählen Sie **Lesen** und **Ausführen** aus, und fügen Sie die Berechtigungen **diesem Ordner** und **nur als Zugriffsberechtigung** hinzu. Klicken Sie auf **OK**.  Die Berechtigung sollte erfolgreich hinzugefügt werden.
7. Schließen Sie den Bereich **Zugriff**.
8. Erstellen Sie für dieses Tutorial einen neuen Ordner. Klicken Sie auf der Befehlsleiste auf **Neuer Ordner**, und nennen Sie den neuen Ordner beispielsweise **TestFolder**.  Klicken Sie auf **OK**.
9. Wählen Sie den erstellten Ordner aus, und klicken Sie dann auf der Befehlsleiste auf **Zugriff**.
10. Klicken Sie wie in Schritt 5 auf **Hinzufügen**. Geben Sie im Feld **Auswählen** den Namen Ihres virtuellen Computers ein. Klicken Sie in den Suchergebnissen auf Ihren virtuellen Computer und dann auf **Auswählen**.
11. Klicken Sie wie in Schritt 6 auf **Berechtigungen auswählen**. Wählen Sie **Lesen**, **Schreiben** und **Ausführen** aus, und fügen Sie diese Berechtigungen **diesem Ordner** als **Ein Zugriffsberechtigungseintrag und ein Standardberechtigungseintrag** hinzu. Klicken Sie auf **OK**.  Die Berechtigung sollte erfolgreich hinzugefügt werden.

MSI kann nun alle Vorgänge für Dateien in dem erstellten Ordner ausführen. Weitere Informationen zum Verwalten des Zugriffs auf Data Lake Store finden Sie unter [Zugriffssteuerung in Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

## <a name="get-an-access-token-and-call-the-data-lake-store-file-system"></a>Abrufen eines Zugriffstokens und Aufrufen des Data Lake Store-Dateisystems

Azure Data Lake Store unterstützt die Azure AD-Authentifizierung nativ, sodass Zugriffstoken, die über MSI abgerufen wurden, direkt angenommen werden können. Für die Authentifizierung im Data Lake Store-Dateisystem übermitteln Sie ein Zugriffstoken, das von Azure AD für Ihren Endpunkt des Data Lake Store-Dateisystems ausgestellt wurde. Das Zugriffstoken ist in einem Autorisierungsheader im Format „Bearer \<ZUGRIFFSTOKENWERT\>“ enthalten.  Weitere Informationen zur Unterstützung von Data Lake Store für die Azure AD-Authentifizierung finden Sie unter [Authentifizierung bei Data Lake Store mithilfe von Azure Active Directory](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory).

In diesem Tutorial authentifizieren Sie sich bei der REST-API für das Data Lake Store-Dateisystem mit REST-Anforderungen in cURL.

> [!NOTE]
> Die Client-SDKs für das Data Lake Store-Dateisystem unterstützen noch keine verwalteten Dienstidentitäten.

Zum Ausführen dieser Schritte benötigen Sie einen SSH-Client. Wenn Sie Windows verwenden, können Sie den SSH-Client im [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) verwenden. Wenn Sie Hilfe beim Konfigurieren der SSH-Clientschlüssel benötigen, finden Sie unter [Verwenden von SSH-Schlüsseln mit Windows in Azure](../virtual-machines/linux/ssh-from-windows.md) oder [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](../virtual-machines/linux/mac-create-ssh-keys.md) weitere Informationen.

1. Navigieren Sie im Portal zu Ihrem virtuellen Linux-Computer. Klicken Sie unter **Übersicht** auf **Verbinden**.  
2. Stellen Sie mithilfe des gewünschten SSH-Clients eine Verbindung mit dem virtuellen Computer her. 
3. Stellen Sie im Terminalfenster mit cURL eine Anforderung an den lokalen MSI-Endpunkt, um ein Zugriffstoken für das Data Lake Store-Dateisystem abzurufen. Der Ressourcenbezeichner für Data Lake Store lautet „https://datalake.azure.net/“.  Es ist wichtig, den abschließenden Schrägstrich in den Ressourcenbezeichner einzuschließen.
    
   ```bash
   curl http://localhost:50342/oauth2/token --data "resource=https://datalake.azure.net/" -H Metadata:true   
   ```
    
   Eine erfolgreiche Antwort gibt das Zugriffstoken zurück, das Sie zur Authentifizierung bei Data Lake Store verwenden:

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1508119757",
    "not_before":"1508115857",
    "resource":"https://datalake.azure.net/",
    "token_type":"Bearer"}
   ```

4. Stellen Sie mithilfe von cURL eine Anforderung an den REST-Endpunkt Ihres Data Lake Store-Dateisystems, um die Ordner im Stammordner aufzulisten. Damit können Sie ganz einfach überprüfen, ob alles richtig konfiguriert ist. Kopieren Sie den Wert des Zugriffstokens aus dem vorherigen Schritt. Es ist wichtig, dass die Zeichenfolge „Bearer“ im Autorisierungsheader mit dem Großbuchstaben „B“ beginnt. Sie finden den Namen Ihrer Data Lake Store-Instanz im Abschnitt **Übersicht** im Bereich **Data Lake Store** im Azure-Portal.

   ```bash
   curl https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -H "Authorization: Bearer <ACCESS_TOKEN>"
   ```
    
   Eine erfolgreiche Antwort sieht wie folgt aus:

   ```bash
   {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY","blockSize":0,"accessTime":1507934941392,"modificationTime":1508105430590,"replication":0,"permission":"770","owner":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071","group":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071"}]}}
   ```

5. Jetzt können Sie versuchen, eine Datei in Ihre Data Lake Store-Instanz hochzuladen. Erstellen Sie zunächst eine Datei zum Hochladen.

   ```bash
   echo "Test file." > Test1.txt
   ```

6. Stellen Sie mit cURL eine Anforderung an den REST-Endpunkt Ihres Data Lake Store-Dateisystems, um die Datei in den zuvor erstellten Ordner hochzuladen. Zum Upload gehört eine Umleitung, der cURL automatisch folgt. 

   ```bash
   curl -i -X PUT -L -T Test1.txt -H "Authorization: Bearer <ACCESS_TOKEN>" 'https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/<FOLDER_NAME>/Test1.txt?op=CREATE' 
   ```

    Eine erfolgreiche Antwort sieht wie folgt aus:

   ```bash
   HTTP/1.1 100 Continue
   HTTP/1.1 307 Temporary Redirect
   Cache-Control: no-cache, no-cache, no-store, max-age=0
   Pragma: no-cache
   Expires: -1
   Location: https://mytestadls.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE&write=true
   x-ms-request-id: 756f6b24-0cca-47ef-aa12-52c3b45b954c
   ContentLength: 0
   x-ms-webhdfs-version: 17.04.22.00
   Status: 0x0
   X-Content-Type-Options: nosniff
   Strict-Transport-Security: max-age=15724800; includeSubDomains
   Date: Sun, 15 Oct 2017 22:10:30 GMT
   Content-Length: 0
       
   HTTP/1.1 100 Continue
       
   HTTP/1.1 201 Created
   Cache-Control: no-cache, no-cache, no-store, max-age=0
   Pragma: no-cache
   Expires: -1
   Location: https://mytestadls.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE&write=true
   x-ms-request-id: af5baa07-3c79-43af-a01a-71d63d53e6c4
   ContentLength: 0
   x-ms-webhdfs-version: 17.04.22.00
   Status: 0x0
   X-Content-Type-Options: nosniff
   Strict-Transport-Security: max-age=15724800; includeSubDomains
   Date: Sun, 15 Oct 2017 22:10:30 GMT
   Content-Length: 0
   ```

Mithilfe anderer APIs für das Data Lake Store-Dateisystem können Sie Dateien anfügen und herunterladen und viele andere Aufgaben durchführen.

Glückwunsch! Sie haben sich mit MSI für einen virtuellen Linux-Computer beim Data Lake Store-Dateisystem authentifiziert.

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](../active-directory/msi-overview.md).
- Für Verwaltungsaufgaben verwendet Data Lake Store den Azure Resource Manager.  Weitere Informationen zum Verwenden von MSI zur Authentifizierung bei Resource Manager finden Sie unter [Verwenden der verwalteten Dienstidentität (Managed Service Identity, MSI) mit einer Linux-VM für den Zugriff auf Azure Resource Manager](https://docs.microsoft.com/azure/active-directory/msi-tutorial-linux-vm-access-arm).
- Erfahren Sie mehr über die [Authentifizierung bei Data Lake Store mithilfe von Azure Active Directory](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory).
- Erfahren Sie mehr über [Dateisystemvorgänge in Azure Data Lake Store per REST-API](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-operations-rest-api) oder [WebHDFS-Dateisystem-APIs](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis).
- Erfahren Sie mehr über die [Zugriffssteuerung in Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.