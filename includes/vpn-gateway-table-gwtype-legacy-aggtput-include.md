Die folgende Tabelle zeigt die gatewaytypen und die geschätzte aggregierten Durchsatz vom Gateway-SKU. Diese Tabelle gilt für den Ressourcen-Manager und dem klassischen Bereitstellungsmodelle. 

Preise unterscheidet sich zwischen Gateway-SKUs. Weitere Informationen finden Sie unter [VPN-Gateway – Preise](https://azure.microsoft.com/pricing/details/vpn-gateway).

Beachten Sie, dass das UltraPerformance-Gateway-SKU nicht in dieser Tabelle dargestellt wird. Informationen über die UltraPerformance SKU finden Sie unter der [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) Dokumentation.

|  | **VPN-Gateway-Durchsatz (1)** | **VPN-Gateway-max-IPsec-Tunnel (2)** | **ExpressRoute-Gateway-Durchsatz** | **VPN-Gateway und ExpressRoute gleichzeitig vorhanden sein** |
| --- | --- | --- | --- | --- |
| **Grundlegende SKU (3)(5)(6)** |100 Mbit/s |10 |500 Mbit/s (6) |Nein |
| **Standard-SKU (4)(5)** |100 Mbit/s |10 |1000 Mbit/s |Ja |
| **Hohe Leistung SKU (4)** |200 Mbit/s |30 |2000 Mbit/s |Ja |


(1) der VPN-Durchsatz ist eine grobe Schätzung, die auf der Grundlage der Messungen zwischen VNets in derselben Azure-Region. Es ist keinen garantierten Durchsatz für standortübergreifende Verbindungen über das Internet. Es ist die maximal mögliche Durchsatz Messung.

(2) die Anzahl der Tunnel finden Sie in VPNs als routebased festgelegt. Ein PolicyBased VPN kann nur ein Standort-zu-Standort-VPN-Tunnel unterstützen.

(3) BGP wird für die grundlegende SKU nicht unterstützt.

(4) PolicyBased-VPNs werden für dieser SKU nicht unterstützt. Sie werden nur die grundlegenden SKU unterstützt.

(5) aktiv / aktiv-S2S-VPN-Gateway-Verbindungen sind für dieser SKU nicht unterstützt. Aktiv / aktiv wird in der HighPerformance-SKU unterstützt.

(6) grundlegende SKU ist für die Verwendung mit ExpressRoute veraltet.
