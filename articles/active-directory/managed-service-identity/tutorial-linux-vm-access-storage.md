---
title: Verwenden einer verwalteten Identität für die Linux-VM für den Zugriff auf Azure Storage
description: In diesem Tutorial wird erläutert, wie Sie eine vom System zugewiesene verwaltete Identität auf einem virtuellen Linux-Computer verwenden, um auf Azure Storage zuzugreifen.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: daveba
ms.openlocfilehash: fb67d1eea588d96129c4b58a8c1b2f569c9663bf
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2018
ms.locfileid: "37904406"
---
# <a name="tutorial-use-a-linux-vms-managed-identity-to-access-azure-storage"></a>Tutorial: Verwenden einer verwalteten Identität für die Linux-VM für den Zugriff auf Azure Storage 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


In diesem Tutorial erfahren Sie, wie Sie eine verwaltete Identität für die Linux-VM für den Zugriff auf Azure Storage erstellen und verwenden. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Linux-Computers in einer neuen Ressourcengruppe
> * Aktivieren einer verwalteten Dienstidentität auf einem virtuellen Linux-Computer (VM)
> * Erstellen eines Blobcontainers im Speicherkonto
> * Gewähren Sie der verwalteten Identität der Linux-VM Zugriff auf einen Azure Storage-Container.
> * Erhalten eines Zugriffstokens und seine Verwendung zum Aufrufen von Azure Storage

> [!NOTE]
> Die Azure Active Directory-Authentifizierung für Azure Storage liegt in der Public Preview-Version vor.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com), bevor Sie fortfahren.

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Für die Ausführung der CLI-Beispielskripts in diesem Tutorial stehen Ihnen zwei Möglichkeiten zur Verfügung:

- Verwenden Sie [Azure Cloud Shell](~/articles/cloud-shell/overview.md) entweder vom Azure-Portal aus oder über die Schaltfläche **Ausprobieren**, die sich in der rechten oberen Ecke eines jeden Codeblocks befindet.
- [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Linux-Computers in einer neuen Ressourcengruppe

In diesem Abschnitt erstellen Sie einen virtuellen Linux-Computer, für den später eine verwaltete Identität gewährt wird.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
2. Wählen Sie **Compute** und dann **Ubuntu Server 16.04 LTS**.
3. Geben Sie die Informationen zum virtuellen Computer ein. Wählen Sie unter **Authentifizierungstyp** die Option **Öffentlicher SSH-Schlüssel** oder **Kennwort**. Mit den erstellten Anmeldeinformationen können Sie sich auf dem virtuellen Computer anmelden.

   ![Bereich „Grundeinstellungen“ zum Erstellen eines virtuellen Computers](../media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Wählen Sie in der Liste **Abonnement** ein Abonnement für den virtuellen Computer aus.
5. Um eine neue Ressourcengruppe auszuwählen, in der der virtuelle Computer erstellt werden soll, klicken Sie auf **Ressourcengruppe** > **Neu erstellen**. Wenn Sie fertig sind, klicken Sie auf **OK**.
6. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. Behalten Sie im Bereich „Einstellungen“ die Standardwerte bei, und klicken Sie auf **OK**.

## <a name="enable-managed-identity-on-your-vm"></a>Aktivieren der MSI auf dem virtuellen Computer

Eine VM-MSI ermöglicht es Ihnen, Zugriffstoken aus Azure AD abzurufen, ohne Anmeldeinformationen in Ihren Code einfügen zu müssen. Im Hintergrund werden durch das Aktivieren der MSI auf einem virtuellen Computer über das Azure-Portal zwei Vorgänge ausgelöst: Der virtuelle Computer wird bei Azure AD registriert, um eine verwaltete Identität zu erstellen, und die Identität wird auf dem virtuellen Computer konfiguriert.

1. Navigieren Sie zu der Ressourcengruppe des neuen virtuellen Computers, und wählen Sie den virtuellen Computer aus, den Sie im vorherigen Schritt erstellt haben.
2. Klicken Sie in der Kategorie **Einstellungen** auf **Konfiguration**.
3. Wählen Sie zum Aktivieren der MSI **Ja** aus.
4. Klicken Sie zum Anwenden der Konfiguration auf **Speichern**. 

## <a name="create-a-storage-account"></a>Speicherkonto erstellen 

In diesem Abschnitt erstellen Sie ein Speicherkonto. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **+ Ressource erstellen**.
2. Wählen Sie die Option **Speicher** und anschließend **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** aus.
3. Geben Sie unter **Name** einen Namen für das Speicherkonto ein.  
4. **Bereitstellungsmodell** und **Kontoart** sollten jeweils auf **Resource Manager** und **Storage (universell, Version 1)** festgelegt werden. 
5. Stellen Sie sicher, dass **Abonnement** und **Ressourcengruppe** dem entsprechen, was Sie bei der Erstellung Ihrer VM im vorherigen Schritt angegeben haben.
6. Klicken Sie auf **Create**.

    ![Erstellen eines neuen Speicherkontos](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Erstellen eines Blobcontainers und Hochladen einer Datei in das Speicherkonto

Da Dateien Blob Storage erfordern, müssen wir einen Blobcontainer erstellen, in dem die Datei gespeichert wird. Anschließend laden Sie eine Datei in den Blobcontainer im neuen Speicherkonto hoch.

1. Navigieren Sie zurück zum neu erstellten Speicherkonto.
2. Klicken Sie unter **Blob-Dienst** auf **Container**.
3. Klicken Sie oben auf der Seite auf **+ Container**.
4. Geben Sie unter **Neuer Container** einen Namen für den Container ein, und behalten Sie unter **Öffentliche Zugriffsebene** den Standardwert bei.

    ![Erstellen eines Speichercontainers](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Erstellen Sie mit einem Editor Ihrer Wahl eine Datei mit dem Titel *hall_ welt.txt* auf dem lokalen Computer.  Öffnen Sie die Datei, fügen Sie den Text „Hallo Welt! :)“ (ohne Anführungszeichen) hinzu, und speichern Sie ihn. 

6. Laden Sie die Datei in den neu erstellten Container hoch, indem Sie auf den Containernamen und dann auf **Hochladen** klicken.
7. Klicken Sie im Bereich **Blob hochladen** unter **Dateien** auf das Ordnersymbol und navigieren Sie zur Datei **hallo_welt.txt** auf dem lokalen Computer. Wählen Sie dann die Datei aus, und klicken Sie auf **Hochladen**.

    ![Hochladen einer Textdatei](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>Gewähren des Zugriffs auf einen Azure-Speichercontainer für Ihre VM 

Sie können die verwaltete Identität des virtuellen Computers verwenden, um die Daten in Azure Storage Blob abzurufen.   

1. Navigieren Sie zurück zum neu erstellten Speicherkonto.  
2. Klicken Sie im linken Bereich auf den Link **Zugriffssteuerung (IAM)**.  
3. Klicken Sie dann oben auf der Seite auf **+ Hinzufügen**, um dem virtuellen Computer eine neue Rollenzuweisung hinzuzufügen.
4. Wählen Sie unter **Rolle** in der Dropdownliste **Storage-Blobdatenleser (Vorschau)** aus. 
5. Wählen Sie in der nächsten Dropdownliste unter **Zugriff zuweisen zu** die Option **Virtueller Computer** aus.  
6. Stellen Sie im nächsten Schritt sicher, dass das richtige Abonnement in der Dropdownliste **Abonnement** aufgeführt ist, und legen Sie dann für **Ressourcengruppe** die Option **Alle Ressourcengruppen** fest.  
7. Wählen Sie unter **Auswählen** Ihren virtuellen Computer aus, und klicken Sie dann auf **Speichern**.

    ![Zuweisen von Berechtigungen](~/articles/active-directory/managed-service-identity/media/tutorial-linux-vm-access-storage/access-storage-perms.png)

## <a name="get-an-access-token-and-use-it-to-call-azure-storage"></a>Erhalten eines Zugriffstokens und seine Verwendung zum Aufrufen von Azure Storage

Azure Storage unterstützt die Azure AD-Authentifizierung nativ, sodass Zugriffstoken, die mit der MSI abgerufen wurden, direkt angenommen werden können. Dieser Umstand ist Teil der Azure Storage-Integration in Azure AD und unterscheidet sich vom Bereitstellen von Anmeldeinformationen in der Verbindungszeichenfolge.

Um die folgenden Schritte abzuschließen, müssen Sie von der zuvor erstellten VM aus arbeiten. Außerdem benötigen Sie einen SSH-Client, der eine Verbindung damit herstellt. Wenn Sie Windows verwenden, können Sie den SSH-Client im [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) verwenden. Wenn Sie Hilfe beim Konfigurieren der SSH-Clientschlüssel benötigen, lesen Sie die Informationen unter [Vorgehensweise: Verwenden von SSH-Schlüsseln mit Windows in Azure](~/articles/virtual-machines/linux/ssh-from-windows.md) oder [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Navigieren Sie im Azure-Portal zu **Virtuelle Computer**, wechseln Sie zu Ihrem virtuellen Linux-Computer, und klicken Sie dann auf der Seite **Übersicht** auf **Verbinden**. Kopieren Sie die Zeichenfolge, um eine Verbindung mit Ihrem virtuellen Computer herzustellen.
2. **Verbinden** Sie den virtuellen Computer mit dem gewünschten SSH-Client. 
3. Übermitteln Sie im Terminalfenster mit der cURL eine Anforderung an den lokalen Endpunkt der verwalteten Identität zum Abrufen eines Zugriffstokens für Azure Storage.
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fstorage.azure.com%2F' -H Metadata:true
    ```
4. Greifen Sie nun mithilfe des Zugriffstokens auf Azure Storage zu, z.B. zum Lesen der Inhalte der Beispieldatei, die Sie zuvor in den Container hochgeladen haben. Ersetzen Sie `<STORAGE ACCOUNT>`, `<CONTAINER NAME>` und `<FILE NAME>` durch die Werte, die Sie zuvor angegeben haben, und `<ACCESS TOKEN>` durch den im vorherigen Schritt zurückgegebenen Token.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME> -H "x-ms-version: 2017-11-09" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Die Antwort enthält den Inhalt der Datei:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine verwaltete Identität der Linux-VM für den Zugriff auf Azure Storage aktivieren können.  Weitere Informationen zu Azure Storage finden Sie hier:

> [!div class="nextstepaction"]
> [Azure Storage](/azure/storage/common/storage-introduction)
