---
title: Erstellen eines Hostpools für die Vorschauversion von Windows Virtual Desktop mit PowerShell – Azure
description: Es wird beschrieben, wie Sie in Windows Virtual Desktop (Vorschauversion) mit PowerShell-Cmdlets einen Hostpool erstellen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/05/2019
ms.author: helohr
ms.openlocfilehash: 2af9df4771d58f2288820dad8ef8d7ac84deb8ae
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59258469"
---
# <a name="create-a-host-pool-with-powershell"></a>Erstellen eines Hostpools mit PowerShell

Hostpools sind eine Sammlung identischer virtueller Computer innerhalb von Mandantenumgebungen mit Windows Virtual Desktop (Vorschauversion). Jeder Hostpool kann eine App-Gruppe enthalten, mit der Benutzer genau wie auf einem physischen Desktop interagieren können.

## <a name="use-your-powershell-client-to-create-a-host-pool"></a>Verwenden Ihres PowerShell-Clients zum Erstellen eines Hostpools

Zunächst müssen Sie das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), um es in Ihrer PowerShell-Sitzung verwenden zu können.

Führen Sie das folgende Cmdlet aus, um sich an der Windows Virtual Desktop-Umgebung anzumelden:

```powershell
Add-RdsAccount -DeploymentUrl https://rdbroker.wvd.microsoft.com
```

Führen Sie anschließend das folgende Cmdlet aus, um den Kontext für Ihre Mandantengruppe festzulegen. Falls Sie den Namen der Mandantengruppe nicht kennen, befindet sich Ihr Mandant wahrscheinlich in der „Standardmandantengruppe“, und Sie können dieses Cmdlet überspringen.

```powershell
Set-RdsContext -TenantGroupName <tenantgroupname>
```

Führen Sie als Nächstes dieses Cmdlet aus, um für Ihren Windows Virtual Desktop-Mandanten einen neuen Hostpool zu erstellen:

```powershell
New-RdsHostPool -TenantName <tenantname> -Name <hostpoolname>
```

Führen Sie das nächste Cmdlet aus, um ein Registrierungstoken zum Autorisieren eines Sitzungshosts zu erstellen und den Hostpool einzubinden und in einer neuen Datei auf Ihrem lokalen Computer zu speichern. Sie können mit dem Parameter „-ExpirationHours“ angeben, wie lange das Registrierungstoken gültig ist.

```powershell
New-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname> -ExpirationHours <number of hours> | Select-Object -ExpandProperty Token > <PathToRegFile>
```

Führen Sie anschließend dieses Cmdlet aus, um Azure Active Directory-Benutzer der Desktop-App-Standardgruppe für den Hostpool hinzuzufügen.

```powershell
Add-RdsAppGroupUser -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName "Desktop Application Group" -UserPrincipalName <userupn>
```

Das Cmdlet **Add-RdsAppGroupUser** weist keine Unterstützung für das Hinzufügen von Sicherheitsgruppen auf und fügt der App-Gruppe nur jeweils einen Benutzer hinzu. Falls Sie der App-Gruppe mehrere Benutzer hinzufügen möchten, können Sie das Cmdlet mit den passenden Benutzerprinzipalnamen erneut ausführen.

Führen Sie das folgende Cmdlet aus, um das Registrierungstoken in eine Variable zu exportieren. Sie verwenden sie später unter [Registrieren der virtuellen Computer unter dem Windows Virtual Desktop-Hostpool (Vorschauversion)](#register-the-virtual-machines-to-the-windows-virtual-desktop-preview-host-pool).

```powershell
$token = (Export-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname>).Token
```

## <a name="create-virtual-machines-for-the-host-pool"></a>Erstellen von virtuellen Computern für den Hostpool

Sie können jetzt einen virtuellen Azure-Computer erstellen, der in Ihren Windows Virtual Desktop-Hostpool eingebunden werden kann.

Sie können einen virtuellen Computer auf unterschiedliche Arten erstellen:

- [Erstellen eines virtuellen Computers aus einem Azure-Katalogimage](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Erstellen eines virtuellen Computers aus einem verwalteten Image](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Erstellen eines virtuellen Computers aus einem nicht verwalteten Image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

## <a name="prepare-the-virtual-machines-for-windows-virtual-desktop-preview-agent-installations"></a>Vorbereiten der virtuellen Computer für Installationen von Windows Virtual Desktop-Agents (Vorschauversion)

Sie müssen die folgenden Schritte zum Vorbereiten Ihrer virtuellen Computer ausführen, bevor Sie die Windows Virtual Desktop-Agents installieren und die virtuellen Computer für Ihren Windows Virtual Desktop-Hostpool registrieren können:

- Sie müssen für den Computer den Domänenbeitritt durchführen. Auf diese Weise können eingehende Windows Virtual Desktop-Benutzer von ihrem Azure Active Directory-Konto dem Active Directory-Konto zugeordnet werden, und ihnen kann der Zugriff auf den virtuellen Computer erfolgreich gewährt werden.
- Sie müssen die Rolle „Remotedesktop-Sitzungshost (RDSH)“ installieren, wenn auf dem virtuellen Computer ein Windows Server-Betriebssystem ausgeführt wird. Mit der RDSH-Rolle können die Windows Virtual Desktop-Agents richtig installiert werden.

Führen Sie auf jedem virtuellen Computer die folgenden Schritte aus, damit der Domänenbeitritt erfolgreich ist:

1. [Stellen Sie eine Verbindung mit dem virtuellen Computer her](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine), indem Sie die Anmeldeinformationen verwenden, die Sie beim Erstellen des virtuellen Computers angegeben haben.
2. Starten Sie auf dem virtuellen Computer die **Systemsteuerung**, und wählen Sie **System**.
3. Wählen Sie **Computername**, **Einstellungen ändern** und dann **Ändern...**.
4. Wählen Sie **Domäne**, und geben Sie dann die Active Directory-Domäne im virtuellen Netzwerk ein.
5. Authentifizieren Sie sich mit einem Domänenkonto, das über Berechtigungen für in die Domäne eingebundene Computer verfügt.

## <a name="register-the-virtual-machines-to-the-windows-virtual-desktop-preview-host-pool"></a>Registrieren der virtuellen Computer für den Windows Virtual Desktop-Hostpool (Vorschauversion)

Das Registrieren der virtuellen Computer für einen Windows Virtual Desktop-Hostpool ist so einfach wie das Installieren der Windows Virtual Desktop-Agents.

Führen Sie auf jedem virtuellen Computer die folgenden Schritte aus, um die Windows Virtual Desktop-Agents zu registrieren:

1. [Stellen Sie eine Verbindung mit dem virtuellen Computer her](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine), indem Sie die Anmeldeinformationen verwenden, die Sie beim Erstellen des virtuellen Computers angegeben haben.
2. Führen Sie den Download und die Installation des Windows Virtual Desktop-Agents durch.
   - Laden Sie den [Windows Virtual Desktop-Agent](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv) herunter.
   - Klicken Sie mit der rechten Maustaste auf das heruntergeladene Installationsprogramm, und wählen Sie **Eigenschaften**, **Blockierung aufheben** und dann **OK**. So wird festgelegt, dass Ihr System dem Installationsprogramm vertraut.
   - Führen Sie das Installationsprogramm aus. Wenn Sie vom Installationsprogramm nach dem Registrierungstoken gefragt werden, können Sie den Wert eingeben, den Sie über das Cmdlet **Export-RdsRegistrationInfo** erhalten haben.
3. Führen Sie den Download und die Installation des Bootloaders für Windows Virtual Desktop-Agents durch.
   - Führen Sie den Download des [Bootloaders für Windows Virtual Desktop-Agents](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH) durch.
   - Klicken Sie mit der rechten Maustaste auf das heruntergeladene Installationsprogramm, und wählen Sie **Eigenschaften**, **Blockierung aufheben** und dann **OK**. So wird festgelegt, dass Ihr System dem Installationsprogramm vertraut.
   - Führen Sie das Installationsprogramm aus.
4. Installieren bzw. aktivieren Sie den parallelen Stapel für Windows Virtual Desktop. Die Schritte unterscheiden sich je nach der Betriebssystemversion, die auf dem virtuellen Computer verwendet wird.
   - Bei Verwendung des Betriebssystems Windows Server 2016 auf dem virtuellen Computer:
     - Laden Sie den [parallelen Stapel für Windows Virtual Desktop](https://go.microsoft.com/fwlink/?linkid=2084270) herunter.
     - Klicken Sie mit der rechten Maustaste auf das heruntergeladene Installationsprogramm, und wählen Sie **Eigenschaften**, **Blockierung aufheben** und dann **OK**. So wird festgelegt, dass Ihr System dem Installationsprogramm vertraut.
     - Führen Sie das Installationsprogramm aus.
   - Bei Verwendung von Windows 10 1809 oder höher bzw. Windows Server 2019 oder höher als Betriebssystem des virtuellen Computers:
     - Laden Sie das [Skript](https://go.microsoft.com/fwlink/?linkid=2084268) herunter, um den parallelen Stapel zu aktivieren.
     - Klicken Sie mit der rechten Maustaste auf das heruntergeladene Skript, und wählen Sie **Eigenschaften**, **Blockierung aufheben** und dann **OK**. So wird festgelegt, dass Ihr System dem Skript vertraut.
     - Suchen Sie im Menü **Start** nach „Windows PowerShell ISE“, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Als Administrator ausführen**.
     - Wählen Sie **Datei** und dann **Öffnen…**. Suchen Sie in den heruntergeladenen Dateien nach dem PowerShell-Skript, und öffnen Sie es.
     - Wählen Sie die grüne Wiedergabeschaltfläche aus, um das Skript auszuführen.

>[!IMPORTANT]
>Sie sollten den eingehenden Port 3389 auf Ihren virtuellen Computern nicht öffnen, um Ihre Windows Virtual Desktop-Umgebung in Azure zu schützen. Für Windows Virtual Desktop ist es nicht erforderlich, dass der eingehende Port 3389 offen ist, damit Benutzer auf die virtuellen Computer des Hostpools zugreifen können. Wenn Sie Port 3389 zur Problembehandlung öffnen müssen, sollten Sie [Just-In-Time-Zugriff auf virtuelle Computer](https://docs.microsoft.com/en-us/azure/security-center/security-center-just-in-time) verwenden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun einen Hostpool erstellt haben, können Sie ihn mit RemoteApps füllen. Weitere Informationen zum Verwalten von Apps in Windows Virtual Desktop finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Tutorial: Verwalten von App-Gruppen](./manage-app-groups.md)
