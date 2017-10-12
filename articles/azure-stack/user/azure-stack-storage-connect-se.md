---
title: Herstellen einer Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement
description: Erfahren Sie, wie Sie eine Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement herstellen.
services: azure-stack
documentationcenter: 
author: xiaofmao
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: xiaofmao
ms.openlocfilehash: 8cd52e9561f2ad92664c01959df8919aa56e8738
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription"></a>Herstellen einer Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Bei der Vorschauversion von Azure Storage-Explorer handelt es sich um eine eigenständige App, über die Sie unter Windows, macOS und Linux komfortabel mit Azure Stack-Speicherdaten arbeiten können. Es stehen mehrere Tools zum Verschieben von Daten in Azure Stack-Speicher und aus Azure Stack-Speicher heraus zur Verfügung. Weitere Informationen finden Sie unter [Tools for Azure Stack Storage](azure-stack-storage-transfer.md) (Tools für Azure Stack-Speicher).

In diesem Artikel erfahren Sie, wie Sie die Verbindung zu Ihren Azure Stack-Speicherkonten mit Storage-Explorer herstellen. 

Wenn Sie Storage-Explorer noch nicht installiert haben, [laden Sie ihn herunter](http://www.storageexplorer.com/), und installieren Sie ihn.

Nachdem Sie die Verbindung mit Ihrem Azure Stack-Abonnement hergestellt haben, können Sie mithilfe der [Artikel zu Azure Storage-Explorer ](../../vs-azure-tools-storage-manage-with-storage-explorer.md) mit Ihren Azure Stack-Daten arbeiten. 

## <a name="prepare-an-azure-stack-subscription"></a>Vorbereiten eines Azure Stack-Abonnements

Damit der Storage-Explorer auf das Azure Stack-Abonnement zugreifen kann, benötigen Sie Zugriff auf den Desktop des Azure Stack-Hostcomputers oder eine VPN-Verbindung. Weitere Informationen zur Einrichtung einer VPN-Verbindung mit Azure Stack finden Sie unter [Connect with VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) (Herstellen einer VPN-Verbindung).

Für das Azure Stack Development Kit müssen Sie das Stammzertifikat der Azure Stack-Zertifizierungsstelle exportieren.

### <a name="to-export-and-then-import-the-azure-stack-certificate"></a>So exportieren Sie das Azure Stack-Zertifikat und importieren es anschließend

1. Öffnen Sie `mmc.exe` auf einem Azure Stack-Hostcomputer oder einem lokalen Computer, der über eine VPN-Verbindung mit Azure Stack verfügt. 

2. Wählen Sie unter **Datei** die Option **Snap-In hinzufügen/entfernen** aus, und fügen Sie anschließend **Zertifikate** zum Verwalten des **Computerkontos** von **Lokaler Computer** hinzu.



3. Suchen Sie unter **Console Root\Certificated (Local Computer)\Trusted Root Certification Authorities\Certificates** (Konsolenstamm\Zertifiziert (Lokaler Computer)\Vertrauenswürdige Stammzertifizierungsstellen\Zertifikate) nach **AzureStackSelfSignedRootCert**.

    ![Laden des Azure Stack-Stammzertifikats per „mmc.exe“][25]

4. Klicken Sie mit der rechten Maustaste auf das Zertifikat, wählen Sie **Alle Aufgaben** > **Exportieren** aus, und führen Sie dann die Anweisungen aus, um das Zertifikat mit **Base-64-codiert X.509 (.CER)** zu exportieren.  

    Das exportierte Zertifikat wird im nächsten Schritt verwendet.
5. Starten Sie Storage-Explorer (Vorschau), und wenn das Dialogfeldfeld **Verbindung mit Azure-Speicher herstellen** angezeigt wird, brechen Sie es ab.

6. Zeigen Sie im Menü **Bearbeiten** auf **SSL-Zertifikate**, und klicken Sie anschließend auf **Zertifikate importieren**. Suchen Sie im Dialogfeld für die Dateiauswahl nach dem Zertifikat, das Sie im vorherigen Schritt exportiert haben, und öffnen Sie es.

    Nach dem Importieren werden Sie zum Neustarten des Speicher-Explorers aufgefordert.

    ![Importieren des Zertifikats in den Speicher-Explorer (Vorschau)][27]

Jetzt können Sie eine Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement herstellen.

### <a name="to-connect-an-azure-stack-subscription"></a>So stellen Sie eine Verbindung mit einem Azure Stack-Abonnement her


1. Wählen Sie nach dem Neustart des Speicher-Explorers (Vorschau) das Menü **Bearbeiten** aus, und vergewissern Sie sich, dass die Option **Target Azure Stack** (Azure Stack als Ziel) aktiviert ist. Sollte die Option nicht aktiviert sein, aktivieren Sie sie, und starten Sie den Speicher-Explorer anschließend neu, damit die Änderung wirksam wird. Diese Konfiguration ist erforderlich, um die Kompatibilität mit Ihrer Azure Stack-Umgebung zu gewährleisten.

    ![Sicherstellen der Aktivierung von „Target Azure Stack“ (Azure Stack als Ziel)][28]

7. Wählen Sie im linken Bereich **Konten verwalten** aus.  
    Alle Microsoft-Konten, bei denen Sie angemeldet sind, werden angezeigt.

8. Wählen Sie zum Herstellen einer Verbindung mit dem Azure Stack-Konto die Option **Konto hinzufügen** aus.

    ![Hinzufügen eines Azure Stack-Kontos][29]

9. Wählen Sie im Dialogfeld **Verbindung mit Azure-Speicher herstellen** unter **Azure-Umgebung** die Option **Azure Stack-Umgebung verwenden**, und klicken Sie dann auf **Weiter**.

10. Füllen Sie die Felder im Dialogfeld **Bei Azure Stack-Umgebung anmelden** aus, um sich mit dem Azure Stack-Konto anzumelden, das mindestens einem aktiven Azure Stack-Abonnement zugeordnet ist.  

    Im Anschluss finden Sie ausführliche Informationen zu den einzelnen Feldern:

    * **Umgebungsname**: Dieses Feld kann vom Benutzer angepasst werden.
    * **ARM resource endpoint** (ARM-Ressourcenendpunkt): Beispiele für Azure Resource Manager-Ressourcenendpunkte:

        * Für Cloudoperator:<br> https://adminmanagement.local.azurestack.external   
        * Für Mandanten:<br> https://management.local.azurestack.external
 
    * **Mandanten-ID**: Optional. Der Wert wird nur eingegeben, wenn das Verzeichnis angegeben werden muss.

12. Nach erfolgreicher Anmeldung mit einem Azure Stack-Konto werden im linken Bereich die mit dem Konto verknüpften Azure Stack-Abonnements angezeigt. Wählen Sie die Azure Stack-Abonnements aus, die Sie verwenden möchten, und klicken Sie dann auf **Übernehmen**. (Durch Aktivieren oder Deaktivieren des Kontrollkästchens **Alle Abonnements** können Sie alle bzw. keine Azure Stack-Abonnements in der Liste auswählen.)

    ![Auswählen der Azure Stack-Abonnements nach dem Ausfüllen der Felder im Dialogfeld für die benutzerdefinierte Cloudumgebung][30]  
    Im linken Bereich werden die Speicherkonten angezeigt, die mit den ausgewählten Azure Stack-Abonnements verknüpft sind.

    ![Liste mit Speicherkonten, einschließlich Azure Stack-Abonnementkonten][31]

## <a name="next-steps"></a>Nächste Schritte
* [Erste Schritte mit dem Storage-Explorer (Vorschau)](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure Stack-Speicher: Unterschiede und Überlegungen](azure-stack-acs-differences.md)


* Weitere Informationen zu Azure Storage finden Sie unter [Einführung in Microsoft Azure Storage](../../storage/common/storage-introduction.md).

[25]: ./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png
[26]: ./media/azure-stack-storage-connect-se/export-root-cert-azure-stack.png
[27]: ./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png
[28]: ./media/azure-stack-storage-connect-se/select-target-azure-stack.png
[29]: ./media/azure-stack-storage-connect-se/add-azure-stack-account.png
[30]: ./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png
[31]: ./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png
