## <a name="scenario"></a>Szenario
Zur Veranschaulichung der Vorgehensweise: Erstellen von Netzwerksicherheitsgruppen verwenden dieses Dokument die folgenden Szenario.

![VNet-Szenario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

In diesem Szenario erstellen Sie eine NSG für jedes Subnetz in der **TestVNet** virtuelles Netzwerk, wie unten beschrieben: 

* **NSG-Front-End-**. Die front-End-NSG für gelten die *Front-End-* Subnetz und zwei Regeln enthalten:    
  * **RDP-Regel**. Diese Regel ermöglicht die RDP-Datenverkehr an die *Front-End-* Subnetz.
  * **Web-Regel**. Mit dieser Regel wird HTTP-Datenverkehr zum Zulassen der *Front-End-* Subnetz.
* **NSG-Back-End-**. Die Back-End NSG für gelten die *Back-End-* Subnetz und zwei Regeln enthalten:    
  * **SQL-Regel**. Mit dieser Regel können SQL-Datenverkehr nur über die *Front-End-* Subnetz.
  * **Web-Regel**. Mit dieser Regel verweigert alle Internet gebunden Datenverkehr von der *Back-End-* Subnetz.

Die Kombination der folgenden Regeln erstellen, einem DMZ-ähnliches Szenario, in dem das Back-End-Subnetz eingehende Datenverkehr kann nur für SQL empfangen werden, aus dem front-End-Subnetz und keinen Zugriff auf das Internet hat, während die Front-End-Subnetz mit dem Internet kommunizieren kann, und nur eingehende HTTP-Anforderungen empfängt.

