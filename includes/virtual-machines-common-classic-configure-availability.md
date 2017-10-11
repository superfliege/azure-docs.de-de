


Eine verfügbarkeitsgruppe unterstützt halten Sie Ihre virtuellen Computer verfügbar, während der Ausfallzeit auf, wie z. B. während der Wartung. Zwei oder mehr ähnlich konfigurierte virtuelle Computer in einem verfügbarkeitssatz platzieren, erstellt die Redundanz zum Aufrechterhalten der Verfügbarkeit von Anwendungen oder Dienste, die dem virtuellen Computer ausgeführt wird. Weitere Informationen dazu, wie dies funktioniert, finden Sie unter [Verwalten der Verfügbarkeit von virtuellen Computern][Manage the availability of virtual machines].

Es ist eine bewährte Methode, die Verfügbarkeitsgruppen und Endpunkten mit Lastenausgleich verwenden, um sicherzustellen, dass Ihre Anwendung immer verfügbar und effizient ausgeführt wird. Weitere Informationen zu Endpunkten mit Lastenausgleich, finden Sie unter [Lastenausgleich für Azure-Infrastrukturdienste][Load balancing for Azure infrastructure services].

Sie können die klassische virtuelle Computer in einer verfügbarkeitsgruppe mit einer der beiden Optionen hinzufügen:

* [Option 1: Erstellen einer virtuellen Maschine und einer verfügbarkeitsgruppe, die zur gleichen Zeit][Option 1: Create a virtual machine and an availability set at the same time]. Klicken Sie dann fügen Sie neue virtuelle Computer auf die Gruppe hinzu, wenn Sie diese virtuellen Computer erstellen.
* [Option 2: Hinzufügen eine vorhandene virtuellen Maschine zu einem verfügbarkeitssatz][Option 2: Add an existing virtual machine to an availability set].

> [!NOTE]
> Im klassischen Modell müssen virtuelle Maschinen, die Sie in derselben verfügbarkeitsgruppe aufnehmen möchten zum selben Clouddienst gehören.
> 
> 

## <a id="createset"></a>Option 1: Erstellen einer verfügbarkeitsgruppe, die zur gleichen Zeit und eines virtuellen Computers
Hierzu können Sie die Azure-Portal oder die Azure PowerShell-Befehle verwenden.

Das Azure Portal verwenden zu können:

1. Wenn Sie dies noch nicht getan haben, melden Sie sich bei der [Azure-Portal](https://portal.azure.com).
2. Klicken Sie im hubmenü auf **+ neu**, und klicken Sie dann auf **VM**.
   
    ![Der ALT-Bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. Wählen Sie den Marketplace-VM-Image, die, den Sie verwenden möchten. Sie können auch einen Linux- oder Windows virtuellen Computer erstellen.
4. Für den ausgewählten virtuellen Computer, stellen Sie sicher, dass das Bereitstellungsmodell, um festgelegt ist **klassischen** , und klicken Sie dann auf **erstellen**
   
    ![Der ALT-Bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. Geben Sie Name des virtuellen Computers, Benutzername und Kennwort (für Windows-Computer) oder öffentlichen SSH-Schlüssel (für Linux-Computer). 
6. Wählen Sie die Größe des virtuellen Computers, und klicken Sie dann auf **wählen** um den Vorgang fortzusetzen.
7. Wählen Sie **optionale Konfiguration > verfügbarkeitsgruppe**, und wählen Sie den verfügbarkeitssatz, den Sie die virtuelle Maschine hinzufügen möchten.
   
    ![Der ALT-Bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. Überprüfen Sie Ihre Konfigurationseinstellungen. Wenn Sie fertig sind, klicken Sie auf **erstellen**.
9. Während Azure den virtuellen Computer erstellt, können Sie den Fortschritt unter **VMs** im Menü "Hub".

Um Azure PowerShell-Befehle für virtuellen Computer in Azure erstellen und Hinzufügen einer neuen oder vorhandenen verfügbarkeitsgruppe verwenden, finden Sie unter [verwenden Azure PowerShell zum Erstellen und Vorkonfigurieren von Windows-basierten virtuellen Maschinen](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a id="addmachine"></a>Option 2: hinzufügen eine vorhandene virtuellen Maschine zu einer verfügbarkeitsgruppe
Im Azure-Portal können Sie vorhandene klassische virtuelle Computer zu einer vorhandenen verfügbarkeitsgruppe festgelegt, oder erstellen Sie für sie ein neues Konto hinzufügen. (Bedenken Sie, die die virtuellen Computer in derselben verfügbarkeitsgruppe müssen zum selben Cloud-Dienst gehören.) Die Schritte sind nahezu identisch. Mit der Azure PowerShell können Sie die virtuelle Maschine zu einer vorhandenen verfügbarkeitsgruppe hinzufügen.

1. Wenn Sie dies nicht bereits getan haben, melden Sie sich an den [Azure-Portal](https://portal.azure.com).
2. Klicken Sie im Hubmenü auf **virtuelle Computer (klassisch)**.
   
    ![Der ALT-Bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. Wählen Sie aus der Liste der virtuellen Computer den Namen des virtuellen Computers an, die Sie hinzufügen möchten.
4. Wählen Sie **verfügbarkeitsgruppe** von der virtuellen Maschine **Einstellungen**.
   
    ![Der ALT-Bildtext](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. Wählen Sie die verfügbarkeitsgruppe, der Sie die virtuelle Maschine hinzufügen möchten. Der virtuelle Computer müssen zum selben Clouddienst als verfügbarkeitsgruppe gehören.
   
    ![Der ALT-Bildtext](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. Klicken Sie auf **Speichern**.

Um Azure PowerShell-Befehle zu verwenden, öffnen Sie eine Azure PowerShell-Sitzung auf Administratorebene, und führen Sie den folgenden Befehl aus. Die Platzhalter (z. B. &lt;VmCloudServiceName&gt;), ersetzen Sie alles innerhalb der Anführungszeichen, einschließlich der Zeichen, die mit den richtigen Namen < und >.

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> Der virtuelle Computer möglicherweise neu gestartet werden, um der verfügbarkeitsgruppe hinzugefügt zu beenden.
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at the same time]: #createset
[Option 2: Add an existing virtual machine to an availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage the availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

