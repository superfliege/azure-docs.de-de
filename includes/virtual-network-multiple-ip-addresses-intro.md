> [!div class="op_single_selector"]
> * [Verwaltungsportal](../articles/virtual-network/virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](../articles/virtual-network/virtual-network-multiple-ip-addresses-powershell.md)
> * [CLI 2.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli.md)
> * [CLI 1.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli-nodejs.md)
> * [Vorlage](../articles/virtual-network/virtual-network-multiple-ip-addresses-template.md)
>

Ein Azure-virtuellen Computer (VM) verfügt über eine oder mehrere Netzwerkschnittstellen (NIC) angefügt ist. Eine beliebige NIC kann eine oder mehrere statische oder dynamische öffentliche und private IP-Adressen zugewiesen haben. Einen virtuellen Computer mehrere IP-Adressen zuweisen, können die folgenden Funktionen:

* Mehrere Websites oder Diensten mit verschiedenen IP-Adressen und SSL-Zertifikate auf einem einzelnen Server hostet.
* Dient als einem virtuellen Netzwerk-Gerät, z. B. eine Firewall oder Lastenausgleich.
* Die Fähigkeit, jeden privaten IP-Adressen für eine beliebige der NICs mit einem Azure-Lastenausgleich Back-End-Pool hinzufügen. Früher konnte nur die primäre IP-Adresse für die primäre NIC an einen Back-End-Pool hinzugefügt werden. Weitere Informationen zum Lastenausgleich bei mehreren IP-Konfigurationen finden Sie die [Lastenausgleich für mehrere IP-Konfigurationen](../articles/load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Artikel.

Jede NIC an einen virtuellen Computer angefügt wurde mindestens ein IP-Konfigurationen, die zugeordnet ist. Jede Konfiguration wird eine statische oder dynamische private IP-Adresse zugewiesen. Jede Konfiguration möglicherweise auch eine öffentliche IP-Adressenressource zugeordnet ist. Eine öffentliche IP-Adressressource ist entweder eine dynamische oder statische öffentliche IP-Adresse zugewiesen. Weitere Informationen zu IP-Adressen in Azure finden Sie die [IP-Adressen in Azure](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) Artikel. 

Es ist ein Limit, wie viele private IP-Adressen zugewiesen werden kann, an eine NIC aus. Zudem besteht ein Limit, wie viele öffentliche IP-Adressen, die in einem Azure-Abonnement verwendet werden kann. Finden Sie unter der [Azure schränkt](../articles/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) Artikel finden Sie Details.
