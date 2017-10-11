Es ist wichtig zu beachten, dass es zwei Möglichkeiten zum Konfigurieren eines verfügbarkeitsgruppenlisteners in Azure gibt. Die Verfahren unterscheiden sich in den Typ des Azure-Lastenausgleich, die Sie verwenden, wenn Sie den Listener erstellen. Die folgende Tabelle beschreibt die Unterschiede:

| Load Balancer-Typ | Implementierung | Zu verwenden in folgenden Fällen: |
| --- | --- | --- |
| **Externe** |Verwendet die *öffentliche virtuelle IP-Adresse* des Cloud-Diensts, der den virtuellen Computern (VMs) hostet. |Sie müssen auf den Listener von außerhalb des virtuellen Netzwerks, einschließlich der über das Internet zugreifen. |
| **Interne** |Verwendet eine *internen Load Balancer* mit einer privaten Adresse für den Listener. |Sie können den Listener nur innerhalb der gleichen virtuellen Netzwerk zugreifen. Dieser Zugriff umfasst Standort-zu-Standort-VPN-Hybrid-Szenarien. |

> [!IMPORTANT]
> Für einen Listener, der den Cloud-Dienst öffentliche VIP-Adresse (externen Lastenausgleich), solange der Client verwendet, Listener und Datenbanken befinden sich in derselben Azure-Region, werden Sie nicht ausgehenden Gebühren anfallen. Andernfalls, keine Daten zurückgegeben, die über den Listener gilt Ausgang, und es ist mit normalen Datenübertragung Raten abgerechnet. 
> 
> 

Eine ILB kann nur für virtuelle Netzwerke mit regionalem Umfang konfiguriert werden. Eine ILB können keine bestehende virtuelle Netzwerke, die für eine Affinitätsgruppe konfiguriert wurden. Weitere Informationen finden Sie unter [Übersicht über die interne Load Balancer](../articles/load-balancer/load-balancer-internal-overview.md).

