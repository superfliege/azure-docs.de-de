---
title: "Aktivieren der Sicherung für Azure Stack über das Verwaltungsportal | Microsoft-Dokumentation"
description: "Aktivieren Sie den Infrastructure Backup-Dienst über das Verwaltungsportal, sodass Azure Stack bei einem Fehler wiederhergestellt werden kann."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: a5a9757d871c343ba663862de7b6d75b9dd21c31
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Aktivieren der Sicherung für Azure Stack über das Verwaltungsportal

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Aktivieren Sie den Infrastructure Backup-Dienst über das Verwaltungsportal, sodass Azure Stack Sicherungen generieren kann. Mit diesen Sicherungen können Sie bei einem Fehler Ihre Umgebung wiederherstellen.

## <a name="enable-backup"></a>Aktivieren der Sicherung

1. Öffnen Sie das Azure Stack-Verwaltungsportal unter [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Wählen Sie **Weitere Dienste** > **Infrastructure Backup** aus. Wählen Sie auf dem Blatt **Infrastructure Backup** die Option **Konfiguration** aus.

    ![Azure Stack – Backup Controller-Einstellungen](media\azure-stack-backup\azure-stack-backup-settings.png)zu erstellen und zu verwalten.

3. Geben Sie den Pfad zu dem **Sicherungsspeicherort** ein. Für den Pfad zu einer auf einem separaten Gerät gehosteten Dateifreigabe müssen Sie eine UNC-Zeichenfolge (Universal Naming Convention) verwenden. Eine UNC-Zeichenfolge gibt den Speicherort von Ressourcen (z.B. freigegebene Dateien oder Geräte) an. Für den Dienst können Sie eine IP-Adresse verwenden. Zur Sicherstellung der Verfügbarkeit der Sicherungsdaten bei einem Notfall sollte sich das Gerät an einem gesonderten Speicherort befinden.
    > [!Note]  
    > Anstelle der IP-Adresse können Sie einen vollqualifizierten Domänennamen (FQDN) verwenden, wenn in Ihrer Umgebung die Namensauflösung vom Azure Stack-Infrastrukturnetzwerk in die Unternehmensumgebung unterstützt wird.
4. Geben Sie den **Benutzernamen** mit der Domäne und dem Benutzernamen ein. Beispiel: `Contoso\administrator`.
5. Geben Sie das **Kennwort** für den Benutzer ein.
5. Geben Sie das Kennwort erneut ein, um das **Kennwort zu bestätigen**.
6. Geben Sie im Feld **Verschlüsselungsschlüssel** einen vorinstallierten Schlüssel an. Die Sicherungsdateien werden mit diesem Schlüssel verschlüsselt. Stellen Sie sicher, dass Sie diesen Schlüssel an einem sicheren Speicherort speichern. Nachdem Sie diesen Schlüssel zum ersten Mal festgelegt haben oder künftig ändern, kann er über diese Schnittstelle nicht mehr angezeigt werden. Weitere Informationen zum Generieren eines vorinstallierten Schlüssels finden Sie in den Skripts unter [Aktivieren der Sicherung für Azure Stack mit PowerShell](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Wählen Sie **OK** aus, um die Backup Controller-Einstellungen zu speichern.

Zum Ausführen einer Sicherung müssen Sie die Azure Stack-Tools herunterladen und anschließend das PowerShell-Cmdlet **Start-AzSBackup** auf dem Azure Stack-Verwaltungsknoten ausführen. Weitere Informationen finden Sie unter [Sichern von Azure Stack](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Nächste Schritte

 - Informationen zum Ausführen einer Sicherung finden Sie unter [Sichern von Azure Stack](azure-stack-backup-back-up-azure-stack.md ).
- Informationen zum Überprüfen, ob eine Sicherung durchgeführt wurde, finden Sie unter [Bestätigen der erfolgreichen Sicherung im Verwaltungsportal](azure-stack-backup-back-up-azure-stack.md ).
