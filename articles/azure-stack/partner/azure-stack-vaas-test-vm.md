---
title: Bereitstellen des lokalen Agents und Testen virtueller Computer mit Azure Stack Validation-as-a-Service | Microsoft-Dokumentation
description: Stellen Sie für Azure Stack Validation-as-a-Service den lokalen Agent bereit, und testen Sie virtuelle Computer.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 9d32c23f66563988d023df3bf6a33efa30237e57
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234156"
---
# <a name="deploy-the-local-agent-and-test-virtual-machines"></a>Bereitstellen des lokalen Agents und Testen virtueller Computer

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Hier erfahren Sie, wie Sie den lokalen VaaS-Agent (Validation-as-a-Service) zum Überprüfen Ihrer Hardware verwenden. Der lokale Agent muss vor dem Ausführen von Validierungstests in der zu überprüfenden Azure Stack-Lösung bereitgestellt werden.

> [!Note]  
> Sie müssen sicherstellen, dass die Internetverbindung des Computers, auf dem der lokale Agent ausgeführt wird, nicht unterbrochen wird. Der Computer darf nur für Benutzer zugänglich sein, die zur Verwendung von Azure Stack VaaS autorisiert sind.

So testen Sie die virtuellen Computer:

1. Installieren Sie den lokalen Agent.
2. Schleusen Sie Fehler in Ihr System ein.
3. Führen Sie den lokalen Agent aus.

## <a name="download-and-start-the-local-agent"></a>Herunterladen und Starten des lokalen Agents

Laden Sie den Agent auf einen geeigneten Computer in Ihrem Datencenter herunter, der nicht Teil des Azure Stack-Systems ist, aber Zugriff auf alle Azure Stack-Endpunkte hat.

### <a name="machine-prerequisites"></a>Voraussetzungen für Computer

Vergewissern Sie sich, dass Ihr Computer die folgenden Kriterien erfüllt:

- Zugriff auf alle Azure Stack-Endpunkte
- Installation von .NET 4.6 und PowerShell 5.0
- Mindestens 8 GB RAM
- Prozessoren mit mindestens acht Kernen
- Mindestens 200 GB Speicherplatz
- Stabile Verbindung mit dem Internet

Azure Stack ist das getestete System. Der Computer darf nicht Teil von Azure Stack sein oder in der Azure Stack-Cloud gehostet werden.

### <a name="download-and-install-the-agent"></a>Herunterladen und Installieren des Agents

1. Öffnen Sie auf dem Computer, den Sie zum Ausführen der Tests verwenden möchten, Windows PowerShell in einer Eingabeaufforderung mit erhöhten Rechten.
2. Führen Sie den folgenden Befehl aus, um den lokalen Agent herunterzuladen:

    ```PowerShell  
        Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.3.2.0.nupkg" -outfile "OnPremAgent.zip"
        Expand-Archive -Path ".\OnPremAgent.zip" -DestinationPath VaaSOnPremAgent.3.2.0 -Force
        Set-Location VaaSOnPremAgent.3.2.0\lib\net46
    ````

3. Führen Sie den folgenden Befehl aus, um die Abhängigkeiten des lokalen Agents zu installieren:

    ```PowerShell  
        $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
        Import-Module .\VaaSPreReqs.psm1 -Force
        Install-VaaSPrerequisites -AadTenantId <AadTenantId> `
        -ServiceAdminCreds <ServiceAdminCreds> `
        -ArmEndpoint https://adminmanagement.<ExternalFqdn> `
        -Region <Region>
    ````

    **Parameter**

    | Parameter | BESCHREIBUNG |
    | --- | --- |
    | aadServiceAdminUser | Der globale Administratorbenutzer für Ihren Azure AD-Mandanten. Beispiel: vaasadmin@contoso.onmicrosoft.com |
    | aadServiceAdminPassword | Das Kennwort für den globalen Administratorbenutzer |
    | AadTenantId | Azure AD-Mandanten-ID für das bei Validation-as-a-Service registrierte Azure-Konto |
    | ExternalFqdn | Sie finden den vollqualifizierten Domänennamen in der Konfigurationsdatei. Anweisungen finden Sie unter [Test parameters for validation as a service Azure Stack](azure-stack-vaas-parameters-test.md) (Testen von Parametern für Validation-as-a-Service in Azure Stack). |
    | Region | Die Region des Azure AD-Mandanten |

Der Befehl lädt ein PIR-Image (Betriebssystem-VHD) und eine Kopie aus einem Azure-Blobspeicher in Ihren Azure Stack-Speicher herunter. 

![Herunterladen der erforderlichen Komponenten](media/installingprereqs.png)

> [!Note]  
> Wenn die Geschwindigkeit beim Herunterladen dieser Images niedrig ist, laden Sie sie einzeln auf eine lokale Dateifreigabe herunter, und geben Sie den Parameter **-LocalPackagePath** *FileShareOrLocalPath* an. Weitere Informationen zum PIR-Download finden Sie unter [Problembehandlung für Validation-as-a-Service](azure-stack-vaas-troubleshoot.md) im Abschnitt [Handle slow network connectivity](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity) (Behandeln langsamer Netzwerkverbindungen).

## <a name="fault-injection"></a>Einschleusen von Fehlern

Azure Stack ist auf Resilienz ausgelegt und in der Lage, mehrere Arten von Software- und Hardwarefehlern zu tolerieren. Die Fehlereinschleusung erhöht die Fehlerrate im System. Dank dieser Erhöhung können Sie Probleme früher erkennen und dadurch die Anzahl von Vorfällen reduzieren, die zu Systemausfällen führen.

Führen Sie die folgenden Befehle aus, um Fehler in Ihr System einzuschleusen:

1. Öffnen Sie Windows PowerShell in einer Eingabeaufforderung mit erhöhten Rechten.

2. Führen Sie den folgenden Befehl aus:

    ```PowerShell  
        Import-Module .\VaaSPreReqs.psm1 -Force
        Install-ServiceFabricSDK Install-ServiceFabricSDK
    ```

## <a name="run-the-agent"></a>Ausführen des Agents

1. Öffnen Sie Windows PowerShell in einer Eingabeaufforderung mit erhöhten Rechten.

2. Führen Sie den folgenden Befehl aus:

    ````PowerShell  
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u <VaaSUserId> -t <VaaSTenantId>
    ````

      **Parameter**  
    
    | Parameter | BESCHREIBUNG |
    | --- | --- |
    | VaaSUserId | Benutzer-ID zur Anmeldung beim VaaS-Portal (etwa UserName@Contoso.com) |
    | VaaSTenantId | Azure AD-Mandanten-ID für das bei Validation-as-a-Service registrierte Azure-Konto |

    > [!Note]  
    > Beim Ausführen des Agents muss als aktuelles Arbeitsverzeichnis der Speicherort der ausführbaren Datei für den Task-Engine-Host **Microsoft.VaaSOnPrem.TaskEngineHost.exe** verwendet werden.

Werden keine Fehler angezeigt, war der lokale Agent erfolgreich. Der folgende Beispieltext wird im Konsolenfenster angezeigt:

`Heartbeat Callback at 11/8/2016 4:45:38 PM`

![Gestarteter Agent](media/startedagent.png)

Ein Agent wird anhand seines Namens eindeutig identifiziert. Standardmäßig wird der vollqualifizierte Domänenname (Fully Qualified Domain Name, FQDN) des Computers verwendet, auf dem er gestartet wurde. Sie müssen das Fenster minimieren, um versehentliche Klicks im Fenster zu vermeiden, da durch eine Fokusänderung alle anderen Aktionen angehalten werden.

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen einer neuen Azure Stack-Lösung](azure-stack-vaas-validate-solution-new.md)  
- Wenn die Internetverbindung langsam oder unzuverlässig ist, können Sie das PIR-Image herunterladen. Weitere Informationen finden Sie unter [Handle slow network connectivity](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity) (Behandeln langsamer Netzwerkverbindungen).
- Lesen Sie mehr über [Azure Stack Validation-as-a-Service](https://docs.microsoft.com/azure/azure-stack/partner).
