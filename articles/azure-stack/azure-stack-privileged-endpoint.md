---
title: Verwenden des privilegierten Endpunkts in Azure Stack | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie den privilegierten Endpunkt (privileged endpoint, PEP) in Azure Stack (für einen Azure Stack-Operator) verwenden."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: mabrigg
ms.openlocfilehash: 34ad4d7038202bd5efa2b3c210571268a39bf278
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Verwenden des privilegierten Endpunkts in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Als Azure Stack-Operator sollten Sie das Administratorportal, PowerShell oder APIs des Azure Resource Manager für die meisten alltäglichen Verwaltungsaufgaben verwenden. Für einige weniger häufig ausgeführten Vorgänge müssen Sie jedoch den *privilegierten Endpunkt* (PEP) verwenden. Bei dem PEP handelt es sich um eine vorkonfigurierte Remote-PowerShell-Konsole, die Ihnen gerade so viele Funktionen zur Verfügung stellt, wie Sie zur Ausführung einer erforderlichen Aufgabe benötigen. Der Endpunkt nutzt [PowerShell JEA (Just Enough Administration)](https://docs.microsoft.com/en-us/powershell/jea/overview), um nur einen eingeschränkten Satz von Cmdlets verfügbar zu machen. Es wird ein Konto mit geringen Berechtigungen verwendet, um auf den PEP zuzugreifen und den eingeschränkten Satz von Cmdlets aufzurufen. Es sind keine Administratorkonten erforderlich. Zur Erhöhung der Sicherheit ist die Skripterstellung nicht zulässig.

Der PEP kann beispielsweise für Folgendes verwendet werden:

- Aufgaben auf niedriger Ebene durchführen, z.B. [Diagnoseprotokolle erfassen](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool).
- Durchführung zahlreicher Datacenter-Integrationsaufgaben für integrierte Systeme nach der Bereitstellung, z.B. Hinzufügen von DNS-Weiterleitungen nach der Bereitstellung, Einrichten der Microsoft Graph-Integration, der Integration von Active Directory-Verbunddiensten (AD FS), der Zertifikatrotation, usw.
- Zusammenarbeit mit dem Support zum Erhalt von temporärem, allgemeinem Zugriff für die eingehende Problembehandlung eines integrierten Systems.

Der PEP protokolliert jede Aktion, die Sie in der PowerShell-Sitzung ausführen (sowie die entsprechende Ausgabe). Dies bietet vollständige Transparenz und die komplette Überwachung von Vorgängen. Sie können diese Protokolldateien für zukünftige Überwachungen aufbewahren.

> [!NOTE]
> Im Azure Stack Development Kit (ASDK) können Sie einige der im PEP verfügbaren Befehle direkt über eine PowerShell-Sitzung auf dem Development Kit-Host ausführen. Es empfiehlt sich allerdings, einige Vorgänge (beispielsweise die Protokollsammlung) unter Verwendung des PEPs zu testen, da dies die einzige verfügbare Methode ist, mit der bestimmte Vorgänge in einer Umgebung mit integrierten Systemen ausgeführt werden können.

## <a name="access-the-privileged-endpoint"></a>Zugreifen auf den privilegierten Endpunkt

Der Zugriff auf den PEP erfolgt über eine PowerShell-Remotesitzung auf dem virtuellen Computer, der den PEP hostet. In der ASDK heißt dieser virtuelle Computer **AzS-ERCS01**. Wenn Sie ein integriertes System verwenden, gibt es drei Instanzen des PEPs. Aus Resilienzgründen werden diese jeweils auf einem virtuellen Computer („*Präfix*-ERCS01“, „*Präfix*-ERCS02“ und „*Präfix*-ERCS03“) auf unterschiedlichen Hosts ausgeführt. 

Bevor Sie dieses Verfahren für das integrierte System beginnen, vergewissern Sie sich, dass Sie anhand der IP-Adresse oder per DNS auf den PEP zugreifen können. Nach der erstmaligen Bereitstellung von Azure Stack können Sie nur über die IP-Adresse auf den PEP zugreifen, da die DNS-Integration noch nicht eingerichtet wurde. Ihr OEM-Hardwarehersteller stellt eine JSON-Datei namens **AzureStackStampDeploymentInfo** bereit, die die IP-Adressen des PEPs enthält.

Es wird empfohlen, nur über den Hardwarelebenszyklushost oder über einen dedizierten, sicheren Computer (beispielsweise eine [Arbeitsstation mit privilegiertem Zugriff](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)) eine Verbindung mit dem PEP herzustellen.

1. Greifen Sie auf Ihre Arbeitsstation mit privilegiertem Zugriff zu.

    - Führen Sie in einem integrierten System den folgenden Befehl aus, um den PEP als vertrauenswürdigen Host Ihrem Hardwarelebenszyklushost oder Ihrer Arbeitsstation mit privilegiertem Zugriff hinzuzufügen:

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Wenn Sie die ADSK ausführen, melden Sie sich im Development Kit-Host an.

2. Öffnen Sie auf Ihrem Hardwarelebenszyklushost oder Ihrer Arbeitsstation mit privilegiertem Zugriff eine Windows PowerShell-Sitzung mit erhöhten Rechten. Führen Sie die folgenden Befehle aus, um eine Remotesitzung auf dem virtuellen Computer einzurichten, der den PEP hostet:
 
    - In einem integrierten System:
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      Der Parameter `ComputerName` kann die IP-Adresse oder der DNS-Name eines der virtuellen Computer sein, die den PEP hosten. 
    - Wenn Sie die ADSK verwenden:
     
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName azs-ercs01`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, geben Sie Folgendes ein:

      - **Benutzername**: Geben Sie das CloudAdmin-Konto im Format **&lt;*Azure Stack-Domäne*&gt;\cloudadmin** an. (Für ASDK lautet der Benutzername **Azurestack\cloudadmin**.)
      - **Kennwort**: Geben Sie das gleiche Kennwort ein, das während der Installation für das Domänenadministratorkonto AzureStackAdmin bereitgestellt wurde.
    
3.  Nachdem Sie eine Verbindung hergestellt haben, ändert sich die Aufforderung abhängig von der Umgebung entweder in **[*IP-Adresse oder ERCS VM-Name*]: PS>** oder in **[Azs ercs01]: PS>**. Führen Sie `Get-Command` aus, um die Liste der verfügbaren Cmdlets anzuzeigen.

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
    - Test-AzureStack
    - Stop-AzureStack
    - Get-ClusterLog

## <a name="tips-for-using-the-privileged-endpoint"></a>Verwendungstipps für den privilegierten Endpunkt 

Beim PEP handelt es sich wie bereits erwähnt um einen Endpunkt vom Typ [PowerShell JEA](https://docs.microsoft.com/en-us/powershell/jea/overview). Ein JEA-Endpunkt bietet nicht nur eine Ebene mit hoher Sicherheit, sondern schränkt auch einige der grundlegenden PowerShell-Funktionen wie Skriptverwendung und Vervollständigung mittels TAB-TASTE ein. Wenn Sie einen Skriptvorgangstyp testen, schlägt der Vorgang mit Fehler **ScriptsNotAllowed** fehl. Dieses Verhalten wird erwartet.

Wenn Sie also beispielsweise die Parameterliste für ein bestimmtes Cmdlet erhalten möchten, führen Sie den folgenden Befehl aus:

```PowerShell
    Get-Command <cmdlet_name> -Syntax
```

Alternativ können Sie das Cmdlet [Import-PSSession](https://docs.microsoft.com/en-us/powershell/module/Microsoft.PowerShell.Utility/Import-PSSession?view=powershell-5.1) verwenden, um alle PEP-Cmdlets in die aktuelle Sitzung auf Ihrem lokalen Computer zu importieren. Dadurch stehen alle Cmdlets und Funktionen des PEPs auf Ihrem lokalen Computer zur Verfügung – zusammen mit der Vervollständigung mittels TAB-TASTE und der Skriptverwendung im Allgemeinen. 

Wenn Sie die PEP-Sitzung in Ihren lokalen Computer importieren möchten, führen Sie die folgenden Schritte aus:

1. Greifen Sie auf Ihre Arbeitsstation mit privilegiertem Zugriff zu.

    - Führen Sie in einem integrierten System den folgenden Befehl aus, um den PEP als vertrauenswürdigen Host Ihrem Hardwarelebenszyklushost oder Ihrer Arbeitsstation mit privilegiertem Zugriff hinzuzufügen:

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Wenn Sie die ADSK ausführen, melden Sie sich im Development Kit-Host an.

2. Öffnen Sie auf Ihrem Hardwarelebenszyklushost oder Ihrer Arbeitsstation mit privilegiertem Zugriff eine Windows PowerShell-Sitzung mit erhöhten Rechten. Führen Sie die folgenden Befehle aus, um eine Remotesitzung auf dem virtuellen Computer einzurichten, der den PEP hostet:
 
    - In einem integrierten System:
      ````PowerShell
        $cred = Get-Credential

        $session = New-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      Der Parameter `ComputerName` kann die IP-Adresse oder der DNS-Name eines der virtuellen Computer sein, die den PEP hosten. 
    - Wenn Sie die ADSK verwenden:
     
      ````PowerShell
       $cred = Get-Credential

       $session = New-PSSession -ComputerName azs-ercs01`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, geben Sie Folgendes ein:

      - **Benutzername**: Geben Sie das CloudAdmin-Konto im Format **&lt;*Azure Stack-Domäne*&gt;\cloudadmin** an. (Für ASDK lautet der Benutzername **Azurestack\cloudadmin**.)
      - **Kennwort**: Geben Sie das gleiche Kennwort ein, das während der Installation für das Domänenadministratorkonto AzureStackAdmin bereitgestellt wurde.

3. Importieren der PEP-Sitzung in Ihren lokalen Computer
    ````PowerShell 
        Import-PSSession $session
    ````
4. Nun können Sie in Ihrer lokalen PowerShell-Sitzung die Ergänzung mittels TAB-TASTE nutzen und wie gewohnt Skripts verwenden – zusammen mit allen Funktionen und Cmdlets des PEPs und ohne Beeinträchtigung der Sicherheit von Azure Stack. Viel Spaß!


## <a name="close-the-privileged-endpoint-session"></a>Schließen der Sitzung mit dem privilegierten Endpunkt

 Der PEP protokolliert wie bereits erwähnt jede Aktion, die Sie in der PowerShell-Sitzung ausführen, sowie die entsprechende Ausgabe. Schließen Sie die Sitzung mit dem `Close-PrivilegedEndpoint`-Cmdlet. Dieses Cmdlet schließt den Endpunkt ordnungsgemäß, und überträgt die Protokolldateien an eine externe Dateifreigabe für die Aufbewahrung.

Schließen der Endpunktsitzung:

1. Erstellen Sie eine externe Dateifreigabe, auf die der PEP zugreifen kann. In einer Development Kit-Umgebung können Sie nur eine Dateifreigabe auf dem Development Kit-Host erstellen.
2. Führen Sie das `Close-PrivilegedEndpoint`-Cmdlet aus. 
3. Sie werden aufgefordert, die Aufzeichnungsprotokolldateien auf einem Pfad zu speichern. Geben Sie die zuvor erstellte Dateifreigabe im Format &#92;&#92;*servername*&#92;*sharename* an. Wenn Sie keinen Pfad angeben, schlägt das Cmdlet fehl, und die Sitzung bleibt geöffnet. 

    ![Die Ausgabe des Cmdlet Close-PrivilegedEndpoint die anzeigt, wo Sie den Aufzeichnungszielpfad angeben](media/azure-stack-privileged-endpoint/closeendpoint.png)

Nachdem die Aufzeichnungsprotokolldateien erfolgreich in die Dateifreigabe übertragen wurden, werden sie automatisch vom PEP gelöscht. Wenn Sie mithilfe der Cmdlets `Exit-PSSession` oder `Exit` die PEP-Sitzung (oder einfach die PowerShell-Konsole) schließen, werden die Aufzeichnungsprotokolle nicht in eine Dateifreigabe übertragen. Sie verbleiben auf dem PEP. Wenn Sie das nächste Mal `Close-PrivilegedEndpoint` ausführen und eine Dateifreigabe einschließen, werden die Aufzeichnungsprotokolle aus vorherigen Sitzung(en) auch übertragen.

## <a name="next-steps"></a>Nächste Schritte
[Azure Stack-Diagnosetools](azure-stack-diagnostics.md)