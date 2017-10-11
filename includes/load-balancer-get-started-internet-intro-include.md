Ein Azure-Lastenausgleich ist ein Lastenausgleich Layer-4 (TCP, UDP). Der Load Balancer bietet hohe Verfügbarkeit, indem eingehender Datenverkehr auf fehlerfrei Dienstinstanzen in der Cloud-Dienste oder virtuelle Computer in einer Load Balancer verteilt. Azure-Lastenausgleich kann auch diese Dienste auf mehreren Ports, mehrere IP-Adressen oder beides darstellen.

Sie können einen Lastenausgleich zu konfigurieren:

* Laden Sie die eingehenden Internetdatenverkehrs Lastenausgleich auf virtuellen Computern (VMs). Wir verweisen auf ein Lastenausgleich in diesem Szenario ein [Internetzugriff Lastenausgleichsmodul](../articles/load-balancer/load-balancer-internet-overview.md).
* Lastenausgleich für Datenverkehr zwischen VMs in einem virtuellen Netzwerk (VNet), zwischen virtuellen Computern in Cloud-Diensten oder zwischen lokalen Computern und VMs in einem standortübergreifenden virtuellen Netzwerk. Wir verweisen auf ein Lastenausgleich in diesem Szenario ein [interner Lastenausgleich (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Externe Datenverkehr mit einer bestimmten VM-Instanz weitergeleitet werden.
