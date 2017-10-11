Sie können mit einem virtuellen Computer verbinden, die für Ihr VNet bereitgestellt wird, indem Sie eine Remotedesktopverbindung mit Ihrem virtuellen Computer erstellen. Die beste Möglichkeit, vergewissern Sie sich zunächst, dass Sie eine Verbindung zu Ihrem virtuellen Computer herstellen können, ist über ihren privaten IP-Adresse, anstatt Computernamens eine Verbindung herstellen. Auf diese Weise werden Sie testen, um festzustellen, ob Sie können eine Verbindung herstellen, nicht, ob namensauflösung ordnungsgemäß konfiguriert ist.

1. Suchen Sie die private IP-Adresse ein. Sie können die private IP-Adresse eines virtuellen Computers auf verschiedene Weise finden. Unten werden die Schritte für das Azure-Portal und PowerShell gezeigt.

  - Azure-Portal – suchen Sie den virtuellen Computer im Azure-Portal. Anzeigen der Eigenschaften für den virtuellen Computer an. Die private IP-Adresse aufgeführt wird.

  - PowerShell - aus Ihrer Ressourcengruppen im Beispiel zum Anzeigen einer Liste von virtuellen Computern und privaten IP-Adressen verwenden. Sie müssen nicht in diesem Beispiel ändern, bevor Sie ihn verwenden.

    ```powershell
    $VMs = Get-AzureRmVM
    $Nics = Get-AzureRmNetworkInterface | Where VirtualMachine -ne $null

    foreach($Nic in $Nics)
    {
      $VM = $VMs | Where-Object -Property Id -eq $Nic.VirtualMachine.Id
      $Prv = $Nic.IpConfigurations | Select-Object -ExpandProperty PrivateIpAddress
      $Alloc = $Nic.IpConfigurations | Select-Object -ExpandProperty PrivateIpAllocationMethod
      Write-Output "$($VM.Name): $Prv,$Alloc"
    }
    ```

2. Stellen Sie sicher, dass Sie Ihr VNet über die VPN-Verbindung verbunden sind.
3. Open **Remote Desktop Connection** Geben Sie "RDP" oder "Remotedesktopverbindung" in das Suchfeld auf der Taskleiste ein, wählen Sie dann eine Remotedesktopverbindung. Sie können auch eine Remotedesktopverbindung mit dem "Mstsc"-Befehl in PowerShell öffnen. 
4. Geben Sie in Remote Desktop Connection die private IP-Adresse des virtuellen Computers aus. Sie können "Optionen anzeigen", passen Sie die zusätzliche Einstellungen, und verbinden klicken.

### <a name="to-troubleshoot-an-rdp-connection-to-a-vm"></a>Um eine RDP-Verbindung zu einem virtuellen Computer zu beheben.

Wenn Sie Probleme beim Herstellen einer Verbindung zu einem virtuellen Computer über eine VPN-Verbindung auftreten, überprüfen Sie Folgendes:

- Stellen Sie sicher, dass Ihre VPN-Verbindung erfolgreich ist.
- Stellen Sie sicher, dass Sie mit der privaten IP-Adresse für den virtuellen Computer herstellen.
- Wenn Sie eine Verbindung mit dem virtuellen Computer mit der privaten IP-Adresse, jedoch nicht den Computernamen herstellen können, stellen Sie sicher, dass Sie DNS ordnungsgemäß konfiguriert haben. Weitere Informationen zur Funktionsweise der namensauflösung für virtuelle Computer finden Sie unter [namensauflösung für virtuelle Computer](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
- Weitere Informationen zu RDP-Verbindungen finden Sie unter [Remotedesktopverbindungen zu einer virtuellen Maschine beheben](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).