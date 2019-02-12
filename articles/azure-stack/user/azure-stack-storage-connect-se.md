---
title: Herstellen einer Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement oder -Speicherkonto | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement herstellen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 01/24/2019
ms.openlocfilehash: 9debcd121cbbde626758abccfe838abda12ad840
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55822832"
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription-or-a-storage-account"></a>Herstellen einer Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement oder -Speicherkonto

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

In diesem Artikel erfahren Sie, wie Sie mit Storage-Explorer eine Verbindung mit Ihren Azure Stack-Abonnements und -Speicherkonten herstellen. Beim Azure Storage-Explorer handelt es sich um eine eigenständige App, über die Sie unter Windows, macOS und Linux komfortabel mit Azure Stack-Speicherdaten arbeiten können.

> [!NOTE]  
> Es stehen mehrere Tools zum Verschieben von Daten in und aus Azure Stack-Speicher zur Verfügung. Weitere Informationen finden Sie unter [Tools for Azure Stack Storage](azure-stack-storage-transfer.md) (Tools für Azure Stack-Speicher).

Falls Sie den Storage-Explorer noch nicht installiert haben, [laden Sie ihn herunter](http://www.storageexplorer.com/), und installieren Sie ihn.

Nachdem Sie die Verbindung mit Ihrem Azure Stack-Abonnement oder -Speicherkonto hergestellt haben, können Sie mithilfe der [Artikel zu Azure Storage-Explorer ](../../vs-azure-tools-storage-manage-with-storage-explorer.md) mit Ihren Azure Stack-Daten arbeiten. 

## <a name="prepare-for-connecting-to-azure-stack"></a>Vorbereiten für die Verbindung mit Azure Stack

Damit der Storage-Explorer auf das Azure Stack-Abonnement direkt zugreifen kann, benötigen Sie Zugriff auf Azure Stack oder eine VPN-Verbindung. Weitere Informationen zur Einrichtung einer VPN-Verbindung mit Azure Stack finden Sie unter [Connect with VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) (Herstellen einer VPN-Verbindung).

Für das Azure Stack Development Kit (ASDK) müssen Sie das Stammzertifikat der Azure Stack-Zertifizierungsstelle exportieren.

> [!Note]  
> Verwenden Sie für das ASDK nicht das Stammzertifikat (CA.cer), das während des Setupprozesses erstellt wurde, wenn Sie über ein VPN eine Verbindung mit dem ASDK herstellen.  Dies ist ein DER-codiertes (Distinguished Encoding Rules) Zertifikat, das es dem Storage-Explorer nicht erlaubt, Ihre Azure Stack-Abonnements abzurufen. Gehen Sie wie folgt vor, um ein Base64-codiertes Zertifikat zur Verwendung mit dem Storage-Explorer zu exportieren.

### <a name="export-and-then-import-the-azure-stack-certificate"></a>Exportieren und anschließendes Importieren des Azure Stack-Zertifikats

Exportieren Sie das Azure Stack-Zertifikat für das ASDK, und importieren Sie es anschließend. Bei integrierten Systemen ist das Zertifikat öffentlich signiert. Daher ist dieser Schritt bei der Einrichtung einer Storage-Explorer-Verbindung mit einem integrierten Azure Stack-System nicht erforderlich.

1. Öffnen Sie `mmc.exe` auf einem Azure Stack-Hostcomputer oder einem lokalen Computer, der über eine VPN-Verbindung mit Azure Stack verfügt. 

2. Wählen Sie unter **Datei** die Option **Snap-In hinzufügen/entfernen** aus, und fügen Sie anschließend **Zertifikate** zum Verwalten **des Benutzerkontos** hinzu.

3.  Unter **Console Root\Certificated (Local Computer)\Trusted Root Certification Authorities\Certificates**. Suchen Sie **AzureStackSelfSignedRootCert**.

    ![Laden des Azure Stack-Stammzertifikats per „mmc.exe“](./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png)

4. Klicken Sie mit der rechten Maustaste auf das Zertifikat, wählen Sie **Alle Aufgaben** > **Exportieren** aus, und führen Sie dann die Anweisungen aus, um das Zertifikat mit **Base-64-codiert X.509 (.CER)** zu exportieren.

    Das exportierte Zertifikat wird im nächsten Schritt verwendet.

5. Starten Sie den Storage-Explorer. Wenn das Dialogfeld **Verbindung mit Azure-Speicher herstellen** angezeigt wird, brechen Sie den Vorgang ab.

6. Zeigen Sie im Menü **Bearbeiten** auf **SSL-Zertifikate**, und wählen Sie anschließend **Zertifikate importieren**. Suchen Sie im Dialogfeld für die Dateiauswahl nach dem Zertifikat, das Sie im vorherigen Schritt exportiert haben, und öffnen Sie es.

    Nach dem Importieren des Zertifikats werden Sie zum Neustarten des Storage-Explorers aufgefordert.

    ![Importieren des Zertifikats in den Storage-Explorer](./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png)

7. Klicken Sie nach dem Neustart des Storage-Explorers auf das Menü **Bearbeiten**, und vergewissern Sie sich, dass die Option **Target Azure Stack** (Azure Stack als Ziel) aktiviert ist. Sollte die Option nicht aktiviert sein, aktivieren Sie **Target Azure Stack** (Azure Stack als Ziel), und starten Sie den Storage-Explorer anschließend neu, damit die Änderung wirksam wird. Diese Konfiguration ist erforderlich, um die Kompatibilität mit Ihrer Azure Stack-Umgebung zu gewährleisten.

    ![Sicherstellen der Aktivierung von „Target Azure Stack“ (Azure Stack als Ziel)](./media/azure-stack-storage-connect-se/target-azure-stack.png)

## <a name="connect-to-an-azure-stack-subscription-with-azure-ad"></a>Herstellen einer Verbindung mit einem Azure Stack-Abonnement mit Azure AD

Verwenden Sie die folgenden Schritte, um den Storage-Explorer mit einem Azure Stack-Abonnement zu verbinden, das zu einem Azure AD-Konto (Azure Active Directory) gehört.

1. Wählen Sie im linken Bereich des Storage-Explorers die Option **Konten verwalten** aus. 
    Alle von Ihnen angemeldeten Microsoft-Abonnements werden angezeigt.

2. Wählen Sie zum Herstellen einer Verbindung mit dem Azure Stack-Abonnement die Option **Konto hinzufügen** aus.

    ![Hinzufügen eines Azure Stack-Kontos](./media/azure-stack-storage-connect-se/add-azure-stack-account.png)

3. Wählen Sie im Dialogfeld „Verbindung mit Azure-Speicher herstellen“ unter **Azure-Umgebung** die Option **Azure** oder **Azure China** (abhängig vom verwendeten Azure Stack-Konto), und klicken Sie dann auf **Anmelden**, um sich mit dem Azure Stack-Konto anzumelden, das mit mindestens einem aktiven Azure Stack-Abonnement verknüpft ist.

    ![Herstellen einer Verbindung mit Azure-Speicher](./media/azure-stack-storage-connect-se/azure-stack-connect-to-storage.png)

4. Nach erfolgreicher Anmeldung mit einem Azure Stack-Konto werden im linken Bereich die mit dem Konto verknüpften Azure Stack-Abonnements angezeigt. Wählen Sie die Azure Stack-Abonnements aus, die Sie verwenden möchten, und klicken Sie dann auf **Übernehmen**. (Durch Aktivieren oder Deaktivieren des Kontrollkästchens **Alle Abonnements** können Sie alle bzw. keine Azure Stack-Abonnements in der Liste auswählen.)

    ![Auswählen der Azure Stack-Abonnements nach dem Ausfüllen der Felder im Dialogfeld für die benutzerdefinierte Cloudumgebung](./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png)

    Im linken Bereich werden die Speicherkonten angezeigt, die mit den ausgewählten Azure Stack-Abonnements verknüpft sind.

    ![Liste mit Speicherkonten, einschließlich Azure Stack-Abonnementkonten](./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png)

## <a name="connect-to-an-azure-stack-subscription-with-ad-fs-account"></a>Herstellen einer Verbindung mit einem Azure Stack-Abonnement mit einem AD FS-Konto

> [!Note]  
> Der Bereich für die AD FS-Anmeldung (Active Directory-Verbunddienste) unterstützt Storage-Explorer 1.2.0 oder neuere Versionen mit Azure Stack 1804 oder einem neueren Update.
Verwenden Sie die folgenden Schritte, um Storage-Explorer mit einem Azure Stack-Abonnement zu verbinden, das zu einem AD FS-Konto gehört.

1. Wählen Sie **Konten verwalten**. Im Explorer werden die Microsoft-Abonnements aufgeführt, an denen Sie sich angemeldet haben.
2. Wählen Sie zum Herstellen einer Verbindung mit dem Azure Stack-Abonnement die Option **Konto hinzufügen**.

    ![Konto hinzufügen](media/azure-stack-storage-connect-se/add-an-account.png)

3. Klicken Sie auf **Weiter**. Wählen Sie im Dialogfeld „Verbindung mit Azure-Speicher herstellen“ unter **Azure-Umgebung** die Option **Use Custom Environment** (Benutzerdefinierte Umgebung verwenden), und klicken Sie anschließend auf **Weiter**.

    ![Herstellen einer Verbindung mit Azure Storage](media/azure-stack-storage-connect-se/connect-to-azure-storage.png)

4. Geben Sie die erforderlichen Informationen in der benutzerdefinierten Azure Stack-Umgebung ein. 

    | Feld | Notizen |
    | ---   | ---   |
    | Umgebungsname | Dieses Feld kann vom Benutzer angepasst werden. |
    | Azure Resource Manager-Endpunkt | Die Beispiele von Azure Resource Manager-Ressourcenendpunkten des Azure Stack Development Kit.<br>Für Bediener: https://adminmanagement.local.azurestack.external <br> Für Benutzer: https://management.local.azurestack.external |

    Wenn Sie in einem integrierten Azure Stack-System arbeiten und Ihren Verwaltungsendpunkt nicht kennen, können Sie sich an den entsprechenden Bediener wenden.

    ![Konto hinzufügen](./media/azure-stack-storage-connect-se/custom-environments.png)

5. Wählen Sie **Anmelden**, um eine Verbindung mit dem Azure Stack-Konto herzustellen, das mindestens einem aktiven Azure Stack-Abonnement zugeordnet ist.



6. Wählen Sie die Azure Stack-Abonnements aus, die Sie verwenden möchten. Wählen Sie **Übernehmen**.

    ![Kontenverwaltung](./media/azure-stack-storage-connect-se/account-management.png)

    Im linken Bereich werden die Speicherkonten angezeigt, die mit den ausgewählten Azure Stack-Abonnements verknüpft sind.

    ![Liste mit den zugeordneten Abonnements](./media/azure-stack-storage-connect-se/list-of-associated-subscriptions.png)

## <a name="connect-to-an-azure-stack-storage-account"></a>Herstellen einer Verbindung mit einem Azure Stack-Speicherkonto

Sie können auch eine Verbindung zu einem Azure Stack-Speicherkonto herstellen, indem Sie den Namen des Speicherkontos und das Schlüsselpaar verwenden.

1. Wählen Sie im linken Bereich des Storage-Explorers „Konten verwalten“ aus. Alle Microsoft-Konten, bei denen Sie angemeldet sind, werden angezeigt.

    ![Konto hinzufügen](./media/azure-stack-storage-connect-se/azure-stack-sub-add-an-account.png)

2. Wählen Sie zum Herstellen einer Verbindung mit dem Azure Stack-Abonnement die Option **Konto hinzufügen** aus.

    ![Konto hinzufügen](./media/azure-stack-storage-connect-se/azure-stack-use-a-storage-and-key.png)

3. Wählen Sie im Fenster „Verbindung mit Azure-Speicher herstellen“ die Option **Speicherkontoname und -schlüssel verwenden**.

4. Geben Sie Ihren Kontonamen in das Textfeld **Kontoname** ein, fügen Sie den Kontoschlüssel in das Textfeld **Kontoschlüssel** ein, wählen Sie **Anderer (unten eingeben)** in **Domäne der Speicherendpunkte**, und geben Sie den Azure Stack-Endpunkt ein.

    Ein Azure Stack-Endpunkt besteht aus zwei Teilen: dem Namen einer Region und der Azure Stack-Domäne. Im Azure Stack Development Kit ist der Standardendpunkt **local.azurestack.external**. Wenn Sie sich über Ihren Endpunkt nicht sicher sind, wenden Sie sich an den Cloudadministrator.

    ![Anfügen von Name und Schlüssel](./media/azure-stack-storage-connect-se/azure-stack-attach-name-and-key.png)

5. Wählen Sie **Verbinden**aus.
6. Nachdem das Speicherkonto erfolgreich angefügt wurde, wird es angezeigt. (Dabei wird **(Extern, Anderes)** an den Speicherkontonamen angehängt.)

    ![VMWINDISK](./media/azure-stack-storage-connect-se/azure-stack-vmwindisk.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit dem Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure Stack-Speicher: Unterschiede und Überlegungen](azure-stack-acs-differences.md)
* Weitere Informationen zu Azure Storage finden Sie unter [Einführung in Microsoft Azure Storage](../../storage/common/storage-introduction.md).
