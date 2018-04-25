---
title: Herstellen einer Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement
description: Erfahren Sie, wie Sie eine Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement herstellen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2018
ms.author: xiaofmao
ms.openlocfilehash: 9cbfb7dbf5272b3213d3791c0a7b6fb57109798f
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription"></a>Herstellen einer Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Beim Azure Storage-Explorer handelt es sich um eine eigenständige App, über die Sie unter Windows, macOS und Linux komfortabel mit Azure Stack Storage-Daten arbeiten können. Es stehen mehrere Tools zum Verschieben von Daten in und aus Azure Stack-Speicher zur Verfügung. Weitere Informationen finden Sie unter [Tools for Azure Stack Storage](azure-stack-storage-transfer.md) (Tools für Azure Stack-Speicher).

In diesem Artikel erfahren Sie, wie Sie die Verbindung zu Ihren Azure Stack-Speicherkonten mit Storage-Explorer herstellen. 

Falls Sie den Storage-Explorer noch nicht installiert haben, [laden Sie ihn herunter](http://www.storageexplorer.com/), und installieren Sie ihn.

Nachdem Sie die Verbindung mit Ihrem Azure Stack-Abonnement hergestellt haben, können Sie mithilfe der [Artikel zu Azure Storage-Explorer ](../../vs-azure-tools-storage-manage-with-storage-explorer.md) mit Ihren Azure Stack-Daten arbeiten. 

## <a name="prepare-an-azure-stack-subscription"></a>Vorbereiten eines Azure Stack-Abonnements

Damit der Storage-Explorer auf das Azure Stack-Abonnement zugreifen kann, benötigen Sie Zugriff auf den Desktop des Azure Stack-Hostcomputers oder eine VPN-Verbindung. Weitere Informationen zur Einrichtung einer VPN-Verbindung mit Azure Stack finden Sie unter [Connect with VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) (Herstellen einer VPN-Verbindung).

Für das Azure Stack Development Kit müssen Sie das Stammzertifikat der Azure Stack-Zertifizierungsstelle exportieren.

## <a name="export-and-import-azure-stack-certificates"></a>Exportieren und Importieren von Azure Stack-Zertifikaten

1. Öffnen Sie `mmc.exe` auf einem Azure Stack-Hostcomputer oder einem lokalen Computer, der über eine VPN-Verbindung mit Azure Stack verfügt. 

2. Wählen Sie unter **Datei** die Option **Snap-In hinzufügen/entfernen** aus, und fügen Sie anschließend **Zertifikate** zum Verwalten **des Benutzerkontos** hinzu.

3. Suchen Sie unter **Console Root\Certificated (Local Computer)\Trusted Root Certification Authorities\Certificates** nach **AzureStackSelfSignedRootCert**.

    ![Laden des Azure Stack-Stammzertifikats per „mmc.exe“][25]

4. Klicken Sie mit der rechten Maustaste auf das Zertifikat, wählen Sie **Alle Aufgaben** > **Exportieren** aus, und führen Sie dann die Anweisungen aus, um das Zertifikat mit **Base-64-codiert X.509 (.CER)** zu exportieren.  

    Das exportierte Zertifikat wird im nächsten Schritt verwendet.
5. Starten Sie den Storage-Explorer. Wenn das Dialogfeld **Verbindung mit Azure-Speicher herstellen** angezeigt wird, brechen Sie den Vorgang ab.

6. Zeigen Sie im Menü **Bearbeiten** auf **SSL-Zertifikate**, und klicken Sie anschließend auf **Zertifikate importieren**. Suchen Sie im Dialogfeld für die Dateiauswahl nach dem Zertifikat, das Sie im vorherigen Schritt exportiert haben, und öffnen Sie es.

    Nach dem Importieren werden Sie zum Neustarten des Storage-Explorers aufgefordert.

    ![Importieren des Zertifikats in den Storage-Explorer][27]

Jetzt können Sie eine Verbindung von Storage-Explorer mit einem Azure Stack-Abonnement herstellen.

## <a name="enable-target-azure-stack"></a>Aktivieren der Azure Stack-Zielinstanz

Klicken Sie nach dem Neustart des Storage-Explorers auf das Menü **Bearbeiten**, und vergewissern Sie sich, dass die Option **Target Azure Stack** (Azure Stack als Ziel) aktiviert ist. Sollte die Option nicht aktiviert sein, aktivieren Sie sie, und starten Sie den Storage-Explorer anschließend neu, damit die Änderung wirksam wird. Diese Konfiguration ist erforderlich, um die Kompatibilität mit Ihrer Azure Stack-Umgebung zu gewährleisten.

![Sicherstellen der Aktivierung von „Target Azure Stack“ (Azure Stack als Ziel)][28]

## <a name="sign-in-to-your-account"></a>Anmelden bei Ihrem Konto

### <a name="aad-backed-azure-stack"></a>Azure Stack mit AAD-Unterstützung

1. Wählen Sie im linken Bereich **Konten verwalten** aus. Alle Microsoft-Konten, bei denen Sie angemeldet sind, werden angezeigt.
2. Wählen Sie zum Herstellen einer Verbindung mit dem Azure Stack-Konto die Option **Konto hinzufügen** aus.

    ![Hinzufügen eines Azure Stack-Kontos][29]
3. Wählen Sie im Dialogfeld **Verbindung mit Azure-Speicher herstellen** unter **Azure-Umgebung** die Azure-Umgebung aus, die Ihrer Azure Stack-Instanz zugrunde liegt, und klicken Sie anschließend auf **Anmelden**.
4. Eingeben Ihrer Anmeldeinformationen im Anmeldefenster
5. Nach erfolgreicher Anmeldung mit einem Azure Stack-Konto werden im linken Bereich sowohl Ihre normalen Abonnements als auch Ihre Azure Stack-Abonnements angezeigt. Wählen Sie die Abonnements aus, die Sie verwenden möchten, und klicken Sie anschließend auf **Übernehmen**. (Durch Aktivieren oder Deaktivieren des Kontrollkästchens **Alle Abonnements** können Sie alle bzw. keine Azure Stack-Abonnements in der Liste auswählen.)

    ![Auswählen der Azure Stack-Abonnements nach dem Ausfüllen der Felder im Dialogfeld für die benutzerdefinierte Cloudumgebung][30]  
    Im linken Bereich werden die Speicherkonten angezeigt, die mit den ausgewählten Azure Stack-Abonnements verknüpft sind.

    ![Liste mit Speicherkonten, einschließlich Azure Stack-Abonnementkonten][31]

### <a name="adfs-backed-azure-stack"></a>Azure Stack mit ADFS-Unterstützung

1. Wählen Sie im linken Bereich **Konten verwalten** aus. Alle Microsoft-Konten, bei denen Sie angemeldet sind, werden angezeigt.
2. Wählen Sie zum Herstellen einer Verbindung mit dem Azure Stack-Konto die Option **Konto hinzufügen** aus.

    ![Hinzufügen eines Azure Stack-Kontos][29]
3. Wählen Sie im Dialogfeld **Verbindung mit Azure-Speicher herstellen** unter **Azure-Umgebung** die Option **Create Custom Environment** (Benutzerdefinierte Umgebung erstellen) aus, und klicken Sie anschließend auf **Weiter**.
4. Füllen Sie die Felder im Dialogfeld **Bei Azure Stack-Umgebung anmelden** aus, um sich mit dem Azure Stack-Konto anzumelden, das mindestens einem aktiven Azure Stack-Abonnement zugeordnet ist.  

    Im Anschluss finden Sie ausführliche Informationen zu den einzelnen Feldern:

    * **Umgebungsname**: Dieses Feld kann vom Benutzer angepasst werden.
    * **Azure Resource Manager resource endpoint** (Azure Resource Manager-Ressourcenendpunkt): Beispiele für Azure Resource Manager-Ressourcenendpunkte:
        * Für Cloudoperator:<br> https://adminmanagement.local.azurestack.external   
        * Für Mandanten:<br> https://management.local.azurestack.external
5. Nach erfolgreicher Anmeldung mit einem Azure Stack-Konto werden im linken Bereich die mit dem Konto verknüpften Azure Stack-Abonnements angezeigt. Wählen Sie die Azure Stack-Abonnements aus, die Sie verwenden möchten, und klicken Sie dann auf **Übernehmen**. (Durch Aktivieren oder Deaktivieren des Kontrollkästchens **Alle Abonnements** können Sie alle bzw. keine Azure Stack-Abonnements in der Liste auswählen.)

    ![Auswählen der Azure Stack-Abonnements nach dem Ausfüllen der Felder im Dialogfeld für die benutzerdefinierte Cloudumgebung][30]  
    Im linken Bereich werden die Speicherkonten angezeigt, die mit den ausgewählten Azure Stack-Abonnements verknüpft sind.

    ![Liste mit Speicherkonten, einschließlich Azure Stack-Abonnementkonten][31]

## <a name="next-steps"></a>Nächste Schritte
* [Erste Schritte mit dem Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure Stack-Speicher: Unterschiede und Überlegungen](azure-stack-acs-differences.md)


* Weitere Informationen zu Azure Storage finden Sie unter [Einführung in Microsoft Azure Storage](../../storage/common/storage-introduction.md).

[25]: ./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png
[26]: ./media/azure-stack-storage-connect-se/export-root-cert-azure-stack.png
[27]: ./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png
[28]: ./media/azure-stack-storage-connect-se/select-target-azure-stack.png
[29]: ./media/azure-stack-storage-connect-se/add-azure-stack-account.png
[30]: ./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png
[31]: ./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png
