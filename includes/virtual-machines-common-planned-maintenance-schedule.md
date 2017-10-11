

## <a name="multi-and-single-instance-vms"></a>Mehrere und Einzelinstanz-VMs
Viele Kunden, die unter Azure Anzahl es wichtig, dass sie planen können, wenn virtuelle Maschinen geplante Wartung aufgrund der Downtime--unterzogen werden erfolgt ca. 15 Minuten--, die während der Wartung. Verfügbarkeitsgruppen können Sie steuern möchten, wenn bereitgestellten VMs geplante Wartung erhalten.

Es gibt zwei mögliche Konfigurationen für virtuelle Computer in Azure ausgeführt wird. Virtuelle Computer werden entweder als mit mehreren Instanzen oder eine Instanz konfiguriert. Wenn virtuelle Computer in einer verfügbarkeitsgruppe sind, werden sie als mit mehreren Instanzen konfiguriert. Beachten Sie, sogar einzelne virtuelle Computer können in einem verfügbarkeitssatz bereitgestellt werden, damit sie als mit mehreren Instanzen behandelt werden. Wenn virtuelle Computer nicht in einer verfügbarkeitsgruppe sind, werden sie als einzelne Instanz konfiguriert.  Weitere Informationen zu Verfügbarkeitsgruppen finden Sie unter [Verwalten der Verfügbarkeit von Ihrer virtuellen Windows-Computern](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) oder [Verwalten der Verfügbarkeit von Ihrer virtuellen Linux-Computer](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Geplante Wartungsupdates zu einer Instanz und mit mehreren Instanzen virtueller Computer geschehen, getrennt. Neukonfigurieren Ihre virtuellen Computer werden Einzelinstanz-(Wenn sie mit mehreren Instanzen sind) oder mit mehreren Instanzen (Wenn sie eine Instanz sind) werden, können Sie steuern, wenn Sie virtuelle Maschinen, die geplante Wartung erhalten. Finden Sie unter [geplante Wartung für virtuelle Azure Linux-Computer](../articles/virtual-machines/linux/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [geplante Wartung für Windows Azure-VMs](../articles/virtual-machines/windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ausführliche Informationen über geplante Wartung für virtuelle Azure-Computer.

## <a name="for-multi-instance-configuration"></a>Für die Konfiguration mit mehreren Instanzen
Sie können auswählen, zu die Zeit geplante Wartung Ihre virtuellen Computer, die in einer Verfügbarkeitsgruppe Konfiguration bereitgestellt werden auswirkt, diesen virtuellen Computern von Verfügbarkeitsgruppen zu entfernen.

1. Eine e-Mail-Nachricht wird an Sie sieben Kalendertage vor der geplanten Wartung mit Ihren virtuellen Computern in einer Konfiguration mit mehreren Instanzen gesendet. Die Abonnement-IDs und Namen der betroffenen virtuellen Computer mit mehreren Instanzen sind im Textkörper der e-Mail enthalten.
2. Bei den sieben Tagen können Sie die Zeit auswählen, die Ihrer Instanzen aktualisiert werden, durch das Entfernen der VMs mit mehreren Instanzen in dieser Region aus ihrer verfügbarkeitsgruppe. Durch diese Änderung in der Konfiguration wird einen Neustart aus, wie die virtuelle Maschine, von einem physischen Host verschoben wird, für die Wartung zu einem anderen physischen Host, die für die Wartung ausgerichtet ist nicht vorgesehen.
3. Sie können den virtuellen Computer aus seiner verfügbarkeitsgruppe im Azure-Portal entfernen.

   1. Wählen Sie im Portal den virtuellen Computer aus der Verfügbarkeitsgruppe zu entfernen.  

   2. Klicken Sie unter **Einstellungen**, klicken Sie auf **verfügbarkeitsgruppe**.

      ![Auswahl der Verfügbarkeitsgruppe](./media/virtual-machines-planned-maintenance-schedule/availabilitysetselection.png)

   3. In der verfügbarkeitsgruppe im Dropdown-Menü festgelegt ist, wählen Sie "Nicht Teil einer verfügbarkeitsgruppe."

      ![Aus Gruppe entfernen](./media/virtual-machines-planned-maintenance-schedule/availabilitysetwarning.png)

   4. Klicken Sie im oberen Bereich auf **speichern**. Klicken Sie auf **Ja** zu bestätigen, dass diese Aktion startet den virtuellen Computer neu.

   >[!TIP]
   >Sie können den virtuellen Computer mit mehreren Instanzen später konfigurieren, dazu eine der aufgelisteten Verfügbarkeitsgruppen.

4. VMs aus Verfügbarkeitsgruppen entfernt Einzelinstanz-Hosts verschoben und werden nicht während der geplanten Wartung Verfügbarkeit festgelegt Konfigurationen aktualisiert.
5. Sobald das Update auf Verfügbarkeit festgelegt VMs (gemäß Zeitplan beschrieben, die in der ursprünglichen e-Mail) abgeschlossen ist, sollten Sie die virtuellen Computer wieder für ihre Verfügbarkeitsgruppen hinzufügen. Immer Teil einer verfügbarkeitsgruppe die virtuellen Computer als mit mehreren Instanzen konfiguriert und führt zu einem Neustart. In der Regel nach Updates für alle mit mehreren Instanzen über die gesamte Azure-Umgebung ausgeführt werden, folgt Einzelinstanz-Wartung.

Entfernen einer virtuellen Maschine aus einer verfügbarkeitsgruppe kann auch erzielt werden mithilfe von Azure PowerShell:

```
Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Remove-AzureAvailabilitySet | Update-AzureVM
```

## <a name="for-single-instance-configuration"></a>Für die Einzelinstanz-Konfiguration
Sie können auswählen, zu die Zeit geplante Wartung Sie virtuelle Computer in einer Einzelinstanz-Konfiguration wirkt sich auf die durch diese VMs in Verfügbarkeitsgruppen hinzufügen.

Schritt für Schritt

1. Eine e-Mail-Nachricht wird an Sie sieben Kalendertage vor der geplanten Wartung auf virtuellen Computern in einer Konfiguration mit Einzelinstanz-gesendet. Die Abonnement-IDs und Namen der betroffenen Einzelinstanz-VMs sind im Textkörper der e-Mail enthalten.
2. Bei den sieben Tagen können Sie die Zeit auswählen, die die Instanz neu gestartet wurde, von einer verfügbarkeitsgruppe, die in dieser Region ist Ihre Einzelinstanz-virtuellen Computer hinzugefügt. Durch diese Änderung in der Konfiguration wird einen Neustart aus, wie die virtuelle Maschine, von einem physischen Host verschoben wird, für die Wartung zu einem anderen physischen Host, die für die Wartung ausgerichtet ist nicht vorgesehen.
3. Führen Sie die hier bereitgestellten Anweisungen zum Hinzufügen von vorhandener virtuellen Computern zu Verfügbarkeitsgruppen, die mit der Azure-Portal und die Azure PowerShell. (Siehe die Azure PowerShell-Beispiel, das diesen Schritten).
4. Sobald diese VMs als mit mehreren Instanzen neu konfiguriert werden, auf Einzelinstanz-VMs von der geplanten Wartung ausgeschlossen.
5. Nach Abschluss der Einzelinstanz-VM-Update (gemäß Zeitplan in der ursprünglichen e-Mail) können Sie die virtuellen Computer auf Einzelinstanz-zurückgeben, die virtuellen Computer von ihre Verfügbarkeitsgruppen zu entfernen.

Hinzufügen eines virtuellen Computers zu einer verfügbarkeitsgruppe auch kann mithilfe von Azure PowerShell erreicht werden:

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

<!--Anchors-->



<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
