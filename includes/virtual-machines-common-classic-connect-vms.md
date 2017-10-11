

![Virtuelle Computer in einer eigenständigen cloud-Dienst](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Wenn Sie Ihre virtuellen Computer in einem virtuellen Netzwerk platzieren, können Sie entscheiden, wie viele Cloud-Dienste, die Sie verwenden möchten Sätze von Lastenausgleich und Verfügbarkeit zu laden. Darüber hinaus können Sie die virtuellen Computer in Subnetzen auf die gleiche Weise organisieren, wie Ihr lokales Netzwerk und das virtuelle Netzwerk mit Ihrem lokalen Netzwerk herstellen. Beispiel:

![Virtuelle Computer in einem virtuellen Netzwerk](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Virtuelle Netzwerke sind die empfohlene Methode zum Verbinden virtueller Computer in Azure. Die bewährte Methode ist so konfigurieren Sie jede Ebene der Anwendung in einen separaten Cloud-Dienst. Allerdings müssen Sie in der gleichen Cloud-Dienst innerhalb der maximalen 200 Cloud-Dienste pro Abonnement bleiben einige virtuelle Maschinen aus unterschiedlichen Anwendungsstufen kombinieren. Um diese und andere Grenzwerte zu überprüfen, finden Sie unter [Azure-Abonnement und Dienstbeschränkungen, Kontingente und Einschränkungen](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Virtuelle Computer in einem virtuellen Netzwerk verbinden
So verbinden Sie virtuelle Computer in einem virtuellen Netzwerk:

1. Erstellen des virtuellen Netzwerks in die [Azure-Portal](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) , und geben Sie "klassischen Bereitstellung".
2. Erstellen Sie der Satz von Cloud-Dienste für Ihre Bereitstellung widerzuspiegeln Entwurfs für Verfügbarkeitsgruppen festlegen und den Lastenausgleich. Klicken Sie im Azure-Portal auf **neu > berechnen > Cloud-Dienst** für jeden Cloud-Dienst.

  Wie Sie die Cloud-Dienst-Details ausfüllen, wählen Sie die gleiche _Ressourcengruppe_ mit dem virtuellen Netzwerk verwendet.

3. Um jede neue virtuelle Maschine zu erstellen, klicken Sie auf **neu > berechnen**, wählen Sie dann die entsprechenden VM-Image aus dem **empfohlene apps aufgeführt**.

  Auf dem virtuellen Computer **Grundlagen** Blatt, das gleiche _Ressourcengruppe_ mit dem virtuellen Netzwerk verwendet.

  ![VM-Grundlagen Blatt, wenn Sie eine VNet verwenden](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. Wie Sie den virtuellen Computer ausfüllen **Einstellungen**, wählen Sie den richtigen _Cloud-Dienst_ oder _virtuelles Netzwerk_ für den virtuellen Computer.

  Azure wählt das andere Element basierend auf Ihrer Auswahl.

  ![Blatt "VM-Einstellungen" Wenn Sie eine VNet verwenden](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a>Verbinden Sie virtuelle Computer in einem eigenständigen Cloud-Dienst
So verbinden Sie virtuelle Computer in einem eigenständigen Cloud-Dienst:

1. Erstellen Sie den Cloud-Dienst in der [Azure-Portal](http://portal.azure.com). Klicken Sie auf **neu > berechnen > Cloud-Dienst**. Oder Sie können den Cloud-Dienst für Ihre Bereitstellung erstellen, bei der Erstellung des ersten virtuellen Computers.
2. Wenn Sie die virtuellen Computer erstellen, wählen Sie der gleichen Ressourcengruppe mit dem Cloud-Dienst verwendet.

  ![Hinzufügen eines virtuellen Computers zu einem vorhandenen Clouddienst](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  Wie Sie die VM-Details ausfüllen, wählen Sie den Namen des Clouddiensts, die im ersten Schritt erstellt haben.

  ![Auswählen eines Cloud-Diensts für eine virtuelle Maschine](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
