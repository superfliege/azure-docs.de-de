## <a name="scenario"></a>Szenario
Zur Veranschaulichung der Vorgehensweise: Erstellen von UDRs verwenden dieses Dokument die folgenden Szenario.

![IMAGE-BESCHREIBUNG](./media/virtual-network-create-udr-scenario-include/figure1.png)

In diesem Szenario erstellen Sie eine UDR für die *Front-End-Subnetz* und eine andere UDR für die *Back-End-Subnetz* , wie im folgenden beschrieben: 

* **UDR-Front-End-**. Die front-End-UDR für gelten die *Front-End-* Subnetz und eine Route enthalten:    
  * **RouteToBackend**. Diese Route sendet alle Datenverkehr an den Back-End-Subnetz an, die **FW1** virtuellen Computer.
* **UDR-Back-End-**. Die Back-End UDR für gelten die *Back-End-* Subnetz und eine Route enthalten:    
  * **RouteToFrontend**. Diese Route sendet alle Datenverkehr auf dem front-End-Subnetz an, die **FW1** virtuellen Computer.

Die Kombination aus diesen Routen wird sichergestellt, dass alle Datenverkehr aus einem Subnetz in einen anderen weitergeleitet werden die **FW1** virtuellen Computer, der als virtuelles Gerät verwendet wird. Sie müssen auch einschalten IP-Weiterleitung für diesen virtuellen Computer, um sicherzustellen, dass sie Datenverkehr auf anderen virtuellen Computern empfangen kann.

