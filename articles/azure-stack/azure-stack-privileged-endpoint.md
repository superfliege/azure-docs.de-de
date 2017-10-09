---
title: Verwenden des privilegierten Endpunkts in Azure Stack | Microsoft-Dokumentation
description: "Zeigt die Verwendung des privilegierten Endpunkts in Azure Stack (für einen Azure Stack-Operator)."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: twooley
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 9769b12064216680bb1b2db8c1fd7449927c7771
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Verwenden des privilegierten Endpunkts in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stapel Development Kit*

Als Azure Stack-Operator sollten Sie das Administratorportal, PowerShell oder APIs des Azure Resource Manager für die meisten alltäglichen Verwaltungsaufgaben verwenden. Für einige weniger häufig ausgeführten Vorgänge müssen Sie jedoch den *privilegierten Endpunkt* verwenden. Dieser Endpunkt ist eine vorkonfigurierte Remote-PowerShell-Konsole, die Ihnen gerade ausreichende Funktionen zur Verfügung stellt, damit Sie eine erforderliche Aufgabe ausführen können. Der Endpunkt nutzt PowerShell JEA (Just Enough Administration), um nur einen eingeschränkten Satz von Cmdlets verfügbar zu machen. Es wird ein Konto mit geringen Berechtigungen verwendet, um auf den privilegierten Endpunkt zuzugreifen und den eingeschränkten Satz von Cmdlets aufzurufen. Es sind keine Administratorkonten erforderlich. Zur Erhöhung der Sicherheit ist die Skripterstellung nicht zulässig.

Sie können den privilegierten Endpunkt verwenden, um Aufgaben wie die folgenden auszuführen:

- Aufgaben auf niedriger Ebene durchführen, z.B. [Diagnoseprotokolle erfassen](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool).
- Zahlreiche Datacenter-Integrationsaufgaben für integrierte Systeme nach der Bereitstellung ausführen. Z.B. das Hinzufügen von Weiterleitungen für Domain Name System (DNS) nach der Bereitstellung, das Einrichten der Graph-Integration, die Integration von Active Directory-Verbunddiensten (AD FS), Zertifikatrotation, usw.
- Zusammenarbeit mit dem Support zum Erhalt von temporärem, allgemeinem Zugriff für die eingehende Problembehandlung eines integrierten Systems. 

Der privilegierte Endpunkt protokolliert jede Aktion (und die entsprechende Ausgabe), die Sie in der PowerShell-Sitzung ausführen. Dies bietet vollständige Transparenz und die komplette Überwachung von Vorgängen. Sie können diese Protokolldateien für zukünftige Überwachungszwecke beibehalten.

> [!NOTE]
> In Azure Stack Development Kit (ASDK) können Sie einige der Befehle im privilegierten Endpunkt direkt aus einer PowerShell-Sitzung auf dem Development Kit-Host ausführen. Sie sollten jedoch einige Vorgänge unter Verwendung des privilegierten Endpunkts testen, z.B. die Protokollsammlung, da dies die einzige verfügbare Methode ist, mit der bestimmte Vorgänge in einer Umgebung mit integrierten Systemen ausgeführt werden können.

## <a name="access-the-privileged-endpoint"></a>Zugreifen auf den privilegierten Endpunkt

Sie greifen durch eine PowerShell-Remotesitzung auf dem virtuellen Computer auf den privilegierte Endpunkt zu. Der virtuelle Computer hostet den privilegierten Endpunkt. In der ASDK heißt dieser virtueller Computer AzS-ERCS01. Wenn Sie ein integriertes System verwenden, gibt es drei Instanzen des privilegierten Endpunkts. Jede wird jeweils auf einem virtuellen Computer (*Präfix*-ERCS01, *Präfix*-ERCS02 oder *Präfix*-ERCS03) auf verschiedenen Hosts für Resilienz ausgeführt . 

Bevor Sie dieses Verfahren für das integrierte System beginnen, stellen Sie sicher, dass Sie durch die IP-Adresse oder durch DNS auf einen privilegierten Endpunkt zugreifen können. Nach der erstmaligen Bereitstellung von Azure Stack können Sie nur über die IP-Adresse auf den privilegierten Endpunkt zugreifen, da die DNS-Integration noch nicht eingerichtet wurde. Ihr OEM-Hardwarehersteller wird Ihnen eine JSON-Datei mit dem Namen „AzureStackStampDeploymentInfo“ zur Verfügung stellen, die die IP-Adressen des privilegierte Endpunkts enthält.

Es wird empfohlen, nur über den Hardwarelebenszyklushost oder über einen dedizierten, gesperrten Computer, z.B. eine [Arbeitsstation mit privilegiertem Zugriff](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations), eine Verbindung mit dem privilegierten Endpunkt herzustellen.

1. Führen Sie in Abhängigkeit Ihrer Umgebung einen der folgenden Vorgänge aus:

    - Führen Sie in einem integrieren System den folgenden Befehl aus, um den privilegierten Endpunkt als einen vertrauenswürdigen Host zu Ihrem Hardwarelebenszyklushost oder Ihrer Arbeitsstation mit privilegiertem Zugriff hinzuzufügen.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Wenn Sie die ADSK ausführen, melden Sie sich im Development Kit-Host an.

2. Öffnen Sie auf Ihrem Hardwarelebenszyklushost oder Ihrer Arbeitsstation mit privilegiertem Zugriff eine Windows PowerShell-Sitzung mit erhöhten Rechten. Führen Sie die folgenden Befehle aus, um eine Remotesitzung auf dem virtuellen Computer herzustellen, der den privilegierten Endpunkt hostet:
 
    - In einem integrierten System:
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      Der `ComputerName`-Parameter kann die IP-Adresse oder der DNS-Name eines der virtuellen Computer sein, die einen privilegierten Endpunkt hosten. 
    - Wenn Sie die ADSK verwenden:
     
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName azs-ercs01`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, geben Sie Folgendes ein:

      - **Benutzername**: Geben Sie das CloudAdmin-Konto im Format **&lt;*Azure Stack-Domäne*&gt;\cloudadmin** an. (Für ASDK lautet der Benutzername **Azurestack\cloudadmin**.)
      - **Kennwort**: Geben Sie das gleiche Kennwort ein, das während der Installation für das Domänenadministratorkonto AzureStackAdmin bereitgestellt wurde.
    
3.  Nachdem Sie eine Verbindung hergestellt haben, ändert sich die Aufforderung zu **[*IP-Adresse oder ERCS VM-Name*]: PS > ** oder **[Azs ercs01]: PS>**, abhängig von der Umgebung. Führen Sie `Get-Command` aus, um die Liste der verfügbaren Cmdlets anzuzeigen.

    ![Die Ausgabe des Cmdlet „Get-Command“ zeigt die Liste der verfügbaren Befehle](media/azure-stack-privileged-endpoint/getcommandoutput.png)

    Viele dieser Cmdlets sind nur für integrierte Systemumgebungen vorgesehen (z.B. die Cmdlets im Zusammenhang mit der Datacenter-Integration). In der ASDK wurden die folgenden Cmdlets überprüft:

    - Clear-Host
    - Close-PrivilegedEndpoint
    - Exit-PSSession
    - Get-AzureStackLog
    - Get-AzureStackStampInformation
    - Get-Command
    - Get-FormatData
    - Get-Help
    - Get-ThirdPartyNotices
    - Measure-Object
    - New-CloudAdminUser
    - Out-Default
    - Remove-CloudAdminUser
    - Select-Object
    - Set-CloudAdminUserPassword
    - Stop-AzureStack
    - Get-ClusterLog

4.  Da Skripts nicht zulässig sind, können nicht Sie die Vervollständigung mit der TAB-Taste für Parameterwerte verwenden. Um die Liste der Parameter für ein bestimmtes Cmdlet zu erhalten, führen Sie den folgenden Befehl aus:

    ````PowerShell
    Get-Command <cmdlet_name> -Syntax
    ```` 
    Wenn Sie einen Skriptvorgangstyp testen, schlägt der Vorgang mit Fehler **ScriptsNotAllowed** fehl. Dieses Verhalten wird erwartet.

## <a name="close-the-privileged-endpoint-session"></a>Schließen der Sitzung mit dem privilegierten Endpunkt

 Wie bereits erwähnt, protokolliert der privilegierte Endpunkt jede Aktion (und die entsprechende Ausgabe), die Sie in der PowerShell-Sitzung ausführen. Schließen Sie die Sitzung mit dem `Close-PrivilegedEndpoint`-Cmdlet. Dieses Cmdlet schließt den Endpunkt ordnungsgemäß, und überträgt die Protokolldateien an eine externe Dateifreigabe für die Aufbewahrung.

Schließen der Endpunktsitzung:

1. Erstellen Sie eine externe Dateifreigabe, die vom privilegierten Endpunkt zugänglich ist. In einer Development Kit-Umgebung können Sie nur eine Dateifreigabe auf dem Development Kit-Host erstellen.
2. Führen Sie das `Close-PrivilegedEndpoint`-Cmdlet aus. 
3. Sie werden aufgefordert, die Aufzeichnungsprotokolldateien auf einem Pfad zu speichern. Geben Sie die zuvor erstellte Dateifreigabe im Format &#92;&#92;*servername*&#92;*sharename* an. Wenn Sie keinen Pfad angeben, schlägt das Cmdlet fehl, und die Sitzung bleibt geöffnet. 

    ![Die Ausgabe des Cmdlet Close-PrivilegedEndpoint die anzeigt, wo Sie den Aufzeichnungszielpfad angeben](media/azure-stack-privileged-endpoint/closeendpoint.png)

Nachdem die Aufzeichnungsprotokolldateien erfolgreich in die Dateifreigabe übertragen werden, werden sie automatisch aus dem privilegierten Endpunkt gelöscht. Wenn Sie die Sitzung des privilegierten Endpunkts mithilfe der Cmdlets `Exit-PSSession` oder `Exit` oder einfach die PowerShell-Konsole schließen, werden die Aufzeichnungsprotokolle nicht an eine Dateifreigabe übertragen. Sie bleiben im privilegierten Endpunkt. Wenn Sie das nächste Mal `Close-PrivilegedEndpoint` ausführen und eine Dateifreigabe einschließen, werden die Aufzeichnungsprotokolle aus vorherigen Sitzung(en) auch übertragen.

## <a name="next-steps"></a>Nächste Schritte
[Azure Stack-Diagnosetools](azure-stack-diagnostics.md)








