In diesem Artikel wird vorausgesetzt, dass

1. Ein **Standort-zu-Standort-VPNs** oder ein **Expressroute** Verbindung zwischen Ihrem lokalen Netzwerk und Azure Virtual Network bereits eingerichtet wurde.
2. Ihr Benutzerkonto berechtigt ist, zu eine neue virtuellen Maschine in der Azure-Abonnement erstellen, die die virtuellen Computer in ein Failover ausgeführt wurde haben.
3. Ihr Abonnement verfügt über mindestens 4 Kerne verfügbar, um einen neuen Prozessserver virtuellen Computer zu starten.
4. Sie haben die **Passphrase des Konfigurationsservers** verfügbar.

> [!TIP]
> Stellen Sie sicher, dass Sie Port 443 des Konfigurationsservers (lokal ausgeführten) aus dem virtuellen Azure-Netzwerk herstellen, die die virtuellen Computer in ein Failover ausgeführt wurde haben können.
