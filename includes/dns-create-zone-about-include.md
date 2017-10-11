Eine DNS-Zone wird verwendet, um die DNS-Einträge für einen bestimmten Domänennamen hosten. Um Ihre Domäne in Azure DNS-hosting zu starten, müssen Sie eine DNS-Zone für diesen Domänennamen zu erstellen. Jeder DNS-Eintrags für Ihre Domäne wird dann innerhalb dieser DNS-Zone erstellt.

Die Domäne "contoso.com" kann beispielsweise mehrere DNS-Einträge, z. B. "mail.contoso.com" (für einen Mailserver) und "www.contoso.com" (für eine Website) enthalten.

Wenn eine DNS-Zone in Azure DNS zu erstellen:

* Der Name der Zone muss innerhalb der Ressourcengruppe eindeutig sein, und die Zone darf nicht bereits vorhanden sein. Andernfalls schlägt der Vorgang fehl.
* Die gleichen Zonennamen kann in einer anderen Ressourcengruppe oder ein anderes Azure-Abonnement wiederverwendet werden.
* Wenn mehrere Zonen denselben Namen tragen, wird jede Instanz Serveradressen anderen Namen zugewiesen. Bei der Domänennamen-Registrierungsstelle kann nur ein Satz von Adressen konfiguriert werden.

> [!NOTE]
> Sie müssen nicht so erstellen Sie eine DNS-Zone mit diesen Domänennamen in Azure-DNS einen Domänennamen besitzen. Allerdings müssen Sie Besitzer der Domäne zum Konfigurieren von Azure DNS-Namenserver als den richtigen Namenservern für den Domänennamen bei der Domänennamen-Registrierungsstelle sind.
> 
> Weitere Informationen finden Sie unter [delegieren eine Domäne in Azure DNS](../articles/dns/dns-domain-delegation.md).