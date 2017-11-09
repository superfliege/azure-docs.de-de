---
title: "Verwenden einer Windows-VM-MSI für den Zugriff auf Azure Data Lake Store"
description: "Dieses Tutorial veranschaulicht die Verwendung einer Windows-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) für den Zugriff auf Azure Data Lake Store."
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/31/2017
ms.author: skwan
ms.openlocfilehash: 28b3ec7640e1f126d2ef310130e94c84b68ffa75
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-data-lake-store"></a>Verwenden einer Windows-VM-MSI für den Zugriff auf Azure Data Lake Store

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine verwaltete Dienstidentität (Managed Service Identity, MSI) für einen virtuellen Windows-Computer für den Zugriff auf einen Azure Data Lake Store verwenden. Verwaltete Dienstidentitäten werden von Azure automatisch verwaltet und ermöglichen Ihnen die Authentifizierung für Dienste, die die Azure AD-Authentifizierung unterstützen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Aktivieren von MSI auf einem virtuellen Windows-Computer 
> * Gewähren des Zugriffs auf eine Azure Data Lake Store-Instanz durch Ihre VM
> * Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwenden dieses Zugriffstokens für den Zugriff auf Azure Data Lake Store

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Windows-Computers in einer neuen Ressourcengruppe

In diesem Tutorial wird ein neuer virtueller Windows-Computer erstellt.  Sie können MSI auch auf einem vorhandenen virtuellen Computer aktivieren.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
2. Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**. 
3. Geben Sie die Informationen zum virtuellen Computer ein. Der **Benutzername** und das **Kennwort**, die hier erstellt werden, sind die Anmeldeinformationen, die Sie für die Anmeldung beim virtuellen Computer verwenden.
4. Wählen Sie in der Dropdownliste das richtige **Abonnement** für den virtuellen Computer aus.
5. Um eine neue **Ressourcengruppe** auszuwählen, in der der virtuelle Computer erstellt werden soll, wählen Sie **Neu erstellen**. Klicken Sie zum Abschluss auf **OK**.
6. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. Behalten Sie auf der Seite „Einstellungen“ die Standardwerte bei, und klicken Sie auf **OK**.

   ![Alternativer Bildtext](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Aktivieren von MSI auf dem virtuellen Computer 

Eine VM-MSI ermöglicht es Ihnen, Zugriffstoken aus Azure AD abzurufen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen. Durch das Aktivieren von MSI weisen Sie Azure an, eine verwaltete Identität für den virtuellen Computer zu erstellen. Im Hintergrund werden durch das Aktivieren von MSI zwei Vorgänge ausgelöst: Auf dem virtuellen Computer wird die MSI-VM-Erweiterung installiert, und MSI wird im Azure Resource Manager aktiviert.

1. Wählen Sie den **virtuellen Computer** aus, auf dem Sie MSI aktivieren möchten.  
2. Klicken Sie in der links angezeigten Navigationsleiste auf **Konfiguration**. 
3. Die Option **Verwaltete Dienstidenität** wird angezeigt. Wählen Sie zum Registrieren und Aktivieren von MSI die Option **Ja** oder zum Deaktivieren „Nein“. 
4. Achten Sie darauf, zum Speichern der Konfiguration auf **Speichern** zu klicken.  
   ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Wenn Sie überprüfen möchten, welche Erweiterungen sich auf diesem virtuellen Computer befinden, klicken Sie auf **Erweiterungen**. Wenn MSI aktiviert ist, wird **ManagedIdentityExtensionforWindows** in der Liste angezeigt.

   ![Alternativer Bildtext](media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Gewähren des Zugriffs auf Azure Data Lake Store durch Ihre VM

Sie können Ihrem virtuellen Computer nun Zugriff auf Dateien und Ordner in einer Azure Data Lake Store-Instanz gewähren.  Für diesen Schritt können Sie eine vorhandene Data Lake Store-Instanz verwenden oder eine neue erstellen.  Um im Azure-Portal eine neue Data Lake Store-Instanz zu erstellen, führen Sie diesen [Schnellstart zu Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) durch. Es gibt in der [Dokumentation zu Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) auch Schnellstarts, in denen die Azure-Befehlszeilenschnittstelle und Azure PowerShell verwendet werden.

Erstellen Sie in Ihrer Data Lake Store-Instanz einen neuen Ordner, und gewähren Sie Ihrer VM-MSI die Berechtigungen zum Lesen, Schreiben und Ausführen von Dateien in diesem Ordner:

1. Klicken Sie im Azure-Portal im linken Navigationsbereich auf **Data Lake Store**.
2. Klicken Sie auf die Data Lake Store-Instanz, die Sie für dieses Tutorial verwenden möchten.
3. Klicken Sie auf der Befehlsleiste auf **Daten-Explorer**.
4. Der Stammordner der Data Lake Store-Instanz ist ausgewählt.  Klicken Sie auf der Befehlsleiste auf **Zugriff**.
5. Klicken Sie auf **Hinzufügen**.  Geben Sie im Feld **Auswählen** den Namen Ihres virtuellen Computers ein, z.B. **DevTestVM**.  Klicken Sie in den Suchergebnissen auf Ihren virtuellen Computer, um ihn zu markieren, und klicken Sie dann auf **Auswählen**.
6. Klicken Sie auf **Berechtigungen auswählen**.  Wählen Sie **Lesen** und **Ausführen** aus, und fügen Sie die Berechtigungen **diesem Ordner** und **nur als Zugriffsberechtigung** hinzu.  Klicken Sie auf **OK**.  Die Berechtigung sollte erfolgreich hinzugefügt werden.
7. Schließen Sie das Blatt **Zugriff**.
8. Erstellen Sie für dieses Tutorial einen neuen Ordner.  Klicken Sie auf der Befehlsleiste auf **Neuer Ordner**, und benennen Sie den neuen Ordner z.B. mit **TestFolder**.  Klicken Sie auf **OK**.
9. Klicken Sie auf den Ordner, den Sie erstellt haben, und klicken Sie auf der Befehlsleiste auf **Zugriff**.
10. Klicken Sie wie in Schritt 5 auf **Hinzufügen**, geben Sie im Feld **Auswählen** den Namen Ihres virtuellen Computers ein, wählen Sie ihn aus, und klicken Sie dann auf **Auswählen**.
11. Klicken Sie wie in Schritt 6 auf **Berechtigungen auswählen**, wählen Sie **Lesen**, **Schreiben** und **Ausführen** aus, und fügen Sie diese Berechtigungen **diesem Ordner** als **Ein Zugriffsberechtigungseintrag und ein Standardberechtigungseintrag** hinzu.  Klicken Sie auf **OK**.  Die Berechtigung sollte erfolgreich hinzugefügt werden.

Ihre VM-MSI kann nun alle Vorgänge für Dateien in dem erstellten Ordner ausführen.  Weitere Informationen zum Verwalten des Zugriffs auf Data Lake Store finden Sie in diesem Artikel zur [Zugriffssteuerung in Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

## <a name="get-an-access-token-using-the-vm-msi-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>Abrufen eines Zugriffstokens mithilfe der VM-MSI und Verwenden dieses Zugriffstokens für den Aufruf des Azure Data Lake Store-Dateisystems

Azure Data Lake Store unterstützt die Azure AD-Authentifizierung nativ, sodass Zugriffstoken, die mit der MSI abgerufen wurden, direkt angenommen werden können.  Für die Authentifizierung im Data Lake Store-Dateisystem übermitteln Sie ein Zugriffstoken, das von Azure AD für Ihren Endpunkt des Data Lake Store-Dateisystems ausgestellt wurde, in einem Autorisierungsheader im Format „Bearer <ZUGRIFFSTOKENWERT>“.  Weitere Informationen zur Unterstützung von Data Lake Store für die Azure AD-Authentifizierung finden Sie unter [Authentifizierung bei Data Lake Store mithilfe von Azure Active Directory](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory).

> [!NOTE]
> Die Client-SDKs für das Data Lake Store-Dateisystem unterstützen noch keine verwalteten Dienstidentitäten (MSIs).  Dieses Tutorial wird aktualisiert, wenn die Unterstützung im SDK hinzugefügt wird.

In diesem Tutorial authentifizieren Sie sich bei der REST-API für das Data Lake Store-Dateisystem mit REST-Anforderungen in PowerShell. Um die VM-MSI für die Authentifizierung zu verwenden, müssen Sie die Anforderungen auf dem virtuellen Computer auszuführen.

1. Navigieren Sie im Portal zu **Virtuelle Computer**, wechseln Sie dann zu Ihrem virtuellen Windows-Computer, und klicken Sie in der **Übersicht** auf **Verbinden**.
2. Geben Sie Ihren **Benutzernamen** und Ihr **Kennwort** ein, das Sie beim Erstellen des virtuellen Windows-Computers hinzugefügt haben. 
3. Sie haben nun eine **Remotedesktopverbindung** mit dem virtuellen Computer erstellt. Öffnen Sie jetzt **PowerShell** in der Remotesitzung. 
4. Erstellen Sie mithilfe des PowerShell-Befehls `Invoke-WebRequest` eine Anforderung an den lokalen MSI-Endpunkt, um ein Zugriffstoken für Azure Data Lake Store zu erhalten.  Der Ressourcenbezeichner für Data Lake Store lautet „https://datalake.azure.net/“.  Data Lake führt eine Prüfung auf genaue Übereinstimmung der Ressourcen-ID durch, bei der auf der abschließende Schrägstrich wichtig ist.

   ```powershell
   $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://datalake.azure.net/"} -Headers @{Metadata="true"}
   ```
    
   Konvertieren Sie die Antwort aus einem JSON-Objekt in ein PowerShell-Objekt. 
    
   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```

   Extrahieren Sie das Zugriffstoken aus der Antwort.
    
   ```powershell
   $AccessToken = $content.access_token
   ```

5. Führen Sie mithilfe des PowerShell-Befehls „Invoke-WebRequest“ eine Anforderung an den REST-Endpunkt Ihrer Data Lake Store-Instanz durch, um die Ordner im Stammordner aufzulisten.  Damit können Sie ganz einfach überprüfen, ob alles richtig konfiguriert ist.  Es ist wichtig, dass die Zeichenfolge „Bearer“ im Autorisierungsheader mit dem Großbuchstaben „B“ beginnt.  Sie finden den Namen Ihrer Data Lake Store-Instanz im Abschnitt **Übersicht** auf dem Data Lake Store-Blatt im Azure-Portal.

   ```powershell
   Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -Headers @{Authorization="Bearer $AccessToken"}
   ```

   Eine erfolgreiche Antwort sieht wie folgt aus:

   ```powershell
   StatusCode        : 200
   StatusDescription : OK
   Content           : {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY", "blockSize":0,"accessTime":1507934941392, "modificationTime":1507944835699,"replication":0, "permission":"770","ow..."
   RawContent        : HTTP/1.1 200 OK
                       Pragma: no-cache
                       x-ms-request-id: b4b31e16-e968-46a1-879a-3474aa7d4528
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosniff
                       Strict-Transport-Security: ma...
   Forms             : {}
   Headers           : {[Pragma, no-cache], [x-ms-request-id, b4b31e16-e968-46a1-879a-3474aa7d4528],
                       [x-ms-webhdfs-version, 17.04.22.00], [Status, 0x0]...}
   Images            : {}
   InputFields       : {}
   Links             : {}
   ParsedHtml        : System.__ComObject
   RawContentLength  : 556
   ```

6. Jetzt können Sie versuchen, eine Datei in Ihre Data Lake Store-Instanz hochzuladen.  Erstellen Sie zunächst eine Datei zum Hochladen.

   ```powershell
   echo "Test file." > Test1.txt
   ```

7. Erstellen Sie mit dem PowerShell-Befehl `Invoke-WebRequest` eine Anforderung an den REST-Endpunkt Ihrer Data Lake Store-Instanz, um die Datei in den zuvor erstellten Ordner hochzuladen.  Diese Anforderung wird in zwei Schritten durchgeführt.  Im ersten Schritt übermitteln Sie die Anforderung und erhalten eine Umleitung an den Speicherort, an den die Datei hochgeladen werden soll.  Im zweiten Schritt führen Sie den eigentlichen Upload der Datei durch.  Denken Sie daran, den Namen des Ordners und der Datei ordnungsgemäß anzupassen, wenn Sie andere Werte als in diesem Tutorial verwenden. 

   ```powershell
   $HdfsRedirectResponse = Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Wenn Sie den Wert von `$HdfsRedirectResponse` untersuchen, sollte dieser der folgenden Antwort ähneln:

   ```powershell
   PS C:\> $HdfsRedirectResponse

   StatusCode        : 307
   StatusDescription : Temporary Redirect
   Content           : {}
   RawContent        : HTTP/1.1 307 Temporary Redirect
                       Pragma: no-cache
                       x-ms-request-id: b7ab492f-b514-4483-aada-4aa0611d12b3
                       ContentLength: 0
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosn...
   Headers           : {[Pragma, no-cache], [x-ms-request-id, b7ab492f-b514-4483-aada-4aa0611d12b3], 
                       [ContentLength, 0], [x-ms-webhdfs-version, 17.04.22.00]...}
   RawContentLength  : 0
   ```

   Schließen Sie den Upload ab, indem Sie eine Anforderung an den Umleitungsendpunkt senden:

   ```powershell
   Invoke-WebRequest -Uri $HdfsRedirectResponse.Headers.Location -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Eine erfolgreiche Antwort sieht wie folgt aus:

   ```powershell
   StatusCode        : 201
   StatusDescription : Created
   Content           : {}
   RawContent        : HTTP/1.1 201 Created
                       Pragma: no-cache
                       x-ms-request-id: 1e70f36f-ead1-4566-acfa-d0c3ec1e2307
                       ContentLength: 0
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosniff
                       Strict...
   Headers           : {[Pragma, no-cache], [x-ms-request-id, 1e70f36f-ead1-4566-acfa-d0c3ec1e2307],
                       [ContentLength, 0], [x-ms-webhdfs-version, 17.04.22.00]...}
   RawContentLength  : 0
   ```

Mithilfe anderer APIs für das Data Lake Store-Dateisystem können Sie Dateien anfügen und herunterladen und viele andere Aufgaben durchführen.

Glückwunsch!  Sie haben sich mit einer VM-MSI beim Data Lake Store-Dateisystem authentifiziert.

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](../active-directory/msi-overview.md).
- Für Verwaltungsaufgaben verwendet Data Lake Store den Azure Resource Manager.  Weitere Informationen zum Verwenden einer VM-MSI zur Authentifizierung bei Resource Manager finden Sie unter [Verwenden der verwalteten Dienstidentität (Managed Service Identity, MSI) mit einer Linux-VM für den Zugriff auf Azure Resource Manager](https://docs.microsoft.com/azure/active-directory/msi-tutorial-linux-vm-access-arm).
- Erfahren Sie mehr über die [Authentifizierung bei Data Lake Store mithilfe von Azure Active Directory](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory).
- Erfahren Sie mehr über [Dateisystemvorgänge in Azure Data Lake Store per REST-API](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-operations-rest-api) oder [WebHDFS-Dateisystem-APIs](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis).
- Erfahren Sie mehr über die [Zugriffssteuerung in Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.