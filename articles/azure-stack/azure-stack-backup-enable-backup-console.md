---
title: Aktivieren der Sicherung für Azure Stack über das Verwaltungsportal | Microsoft-Dokumentation
description: Aktivieren Sie den Dienst für die Infrastruktursicherung über das Verwaltungsportal, sodass Azure Stack bei einem Fehler wiederhergestellt werden kann.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: jeffgilb
ms.openlocfilehash: 0ef8247eba4605d3c8e5ef0992ce97bce989002e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075271"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Aktivieren der Sicherung für Azure Stack über das Verwaltungsportal
Aktivieren Sie den Dienst für die Infrastruktursicherung über das Verwaltungsportal, sodass Azure Stack Sicherungen generieren kann. Mit diesen Sicherungen können Sie bei einem [schwerwiegenden Fehler](.\azure-stack-backup-recover-data.md) Ihre Umgebung mittels einer Cloudwiederherstellung wiederherstellen. Die Cloudwiederherstellung soll sicherstellen, dass die Anbieter und Benutzer sich nach Abschluss der Wiederherstellung wieder im Portal anmelden können. Auf diese Weise können Benutzer ihre Abonnements wiederherstellen, einschließlich RBAC-Berechtigungen (Role-Based Access Control) und -Rollen, ursprünglichen Plänen, Angeboten und zuvor definierten Compute-, Speicher- und Netzwerkkontingenten.

Allerdings sichert der Infrastructure Backup-Dienst keine IaaS-VMs, Netzwerkkonfigurationen oder Speicherressourcen (z.B. Speicherkonten, Blobs, Tabellen). Daher werden Benutzern zuvor vorhandene Ressourcen nicht angezeigt, wenn sie sich nach einer Cloudwiederherstellung anmelden. PaaS-Ressourcen (Platform as a Service) und -Daten werden ebenfalls nicht vom Dienst gesichert. 

Administratoren und Benutzer sind für die Sicherung und Wiederherstellung von IaaS- und PaaS-Ressourcen verantwortlich, die separat von den Infrastruktursicherungsprozessen erfolgen muss. Informationen zum Sichern von IaaS- und PaaS-Ressourcen finden Sie unter folgenden Links:

- [Virtual Machines](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [App Service](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


> [!Note]  
> Bevor Sie die Sicherung über die Konsole aktivieren, müssen Sie den Sicherungsdienst konfigurieren. Sie können den Sicherungsdienst mithilfe von PowerShell konfigurieren. Weitere Informationen finden Sie unter [Aktivieren der Sicherung für Azure Stack mit PowerShell](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Aktivieren der Sicherung

1. Rufen Sie das Azure Stack-Verwaltungsportal unter [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external) auf.
2. Wählen Sie **Weitere Dienste** > **Infrastructure Backup** aus. Wählen Sie auf dem Blatt **Infrastructure Backup** die Option **Konfiguration** aus.

    ![Azure Stack – Backup Controller-Einstellungen](media\azure-stack-backup\azure-stack-backup-settings.png)zu erstellen und zu verwalten.

3. Geben Sie den Pfad zu dem **Sicherungsspeicherort** ein. Verwenden Sie für den Pfad zu einer auf einem separaten Gerät gehosteten Dateifreigabe eine UNC-Zeichenfolge (Universal Naming Convention). Eine UNC-Zeichenfolge gibt den Speicherort von Ressourcen (z.B. freigegebene Dateien oder Geräte) an. Für den Dienst können Sie eine IP-Adresse verwenden. Zur Sicherstellung der Verfügbarkeit der Sicherungsdaten nach einem Notfall sollte sich das Gerät an einem gesonderten Speicherort befinden.
    > [!Note]  
    > Anstelle der IP-Adresse können Sie einen vollqualifizierten Domänennamen (FQDN) verwenden, wenn in Ihrer Umgebung die Namensauflösung vom Azure Stack-Infrastrukturnetzwerk in die Unternehmensumgebung unterstützt wird.
4. Geben Sie den **Benutzernamen** mit der Domäne und dem Benutzernamen ein. Beispiel: `Contoso\administrator`.
5. Geben Sie das **Kennwort** für den Benutzer ein.
5. Geben Sie das Kennwort erneut ein, um das **Kennwort zu bestätigen**.
6. Geben Sie im Feld **Verschlüsselungsschlüssel** einen vorinstallierten Schlüssel an. Die Sicherungsdateien werden mit diesem Schlüssel verschlüsselt. Stellen Sie sicher, dass Sie diesen Schlüssel an einem sicheren Speicherort speichern. Nachdem Sie diesen Schlüssel zum ersten Mal festgelegt haben oder künftig ändern, kann er über diese Schnittstelle nicht mehr angezeigt werden. Weitere Informationen zum Generieren eines vorinstallierten Schlüssels finden Sie in den Skripts unter [Aktivieren der Sicherung für Azure Stack mit PowerShell](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Wählen Sie **OK** aus, um die Backup Controller-Einstellungen zu speichern.

Zum Ausführen einer Sicherung müssen Sie die Azure Stack-Tools herunterladen und anschließend das PowerShell-Cmdlet **Start-AzSBackup** auf dem Azure Stack-Verwaltungsknoten ausführen. Weitere Informationen finden Sie unter [Sichern von Azure Stack](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie eine Sicherung ausführen. Lesen Sie dazu die Informationen unter [Sichern von Azure Stack](azure-stack-backup-back-up-azure-stack.md ).
- Erfahren Sie, wie Sie sicherstellen, dass die Sicherung ausgeführt wurde. Lesen Sie dazu die Informationen unter [Bestätigen der erfolgreichen Sicherung im Verwaltungsportal](azure-stack-backup-back-up-azure-stack.md).
