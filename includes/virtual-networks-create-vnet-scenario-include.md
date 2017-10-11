## <a name="scenario"></a>Szenario
Zur Veranschaulichung der Vorgehensweise: Erstellen Sie ein VNet und Subnetze verwenden dieses Dokument die folgenden Szenario.

![VNet-Szenario](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

In diesem Szenario erstellen Sie eine VNet mit dem Namen **TestVNet** mit einem reservierten CIDR-Block von **192.168.0.0./16**. Ihr VNet enthält die folgenden Subnetze: 

* **Front-End-**mit **192.168.1.0/24** als seine CIDR-Block.
* **Back-End-**mit **192.168.2.0/24** als seine CIDR-Block.

