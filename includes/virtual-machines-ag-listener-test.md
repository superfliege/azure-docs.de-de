In diesem Schritt testen Sie den verfügbarkeitsgruppenlistener mithilfe einer Clientanwendung, die sich im selben Netzwerk ausgeführt wird.

Clientkonnektivität gelten die folgenden Anforderungen:

* Clientverbindungen mit dem Listener müssen von Computern ausgehen, die in einer anderen Cloud-Dienst als dem befinden, die die verfügbarkeitsreplikate hostet.
* Wenn die AlwaysOn-Replikate in unterschiedlichen Subnetzen befinden, müssen Clients angeben *MultisubnetFailover = True* in der Verbindungszeichenfolge angegeben. Diese Bedingung führt zu parallelen Verbindungsversuchen mit Replikaten in den verschiedenen Subnetzen. Dieses Szenario umfasst eine regionsübergreifende AlwaysOn-Verfügbarkeit gruppenbereitstellung.

Ein Beispiel ist die Verbindung mit dem Listener von einem virtuellen Computer im gleichen virtuellen Azure-Netzwerk (aber nicht, die ein Replikat hostet). Eine einfache Möglichkeit zum Ausführen dieses Tests werden versuchen, SQL Server Management Studio an den Listener der verfügbarkeitsgruppe zu verbinden. Eine weitere einfache Methode zum Ausführen ist [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx)wie folgt:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> Wenn der Wert EndpointPort ist *1433*, Sie sind nicht erforderlich, im Aufruf angegeben. Der vorherige Aufruf wird davon ausgegangen, dass der Clientcomputer mit der gleichen Domäne angehört und der Aufrufer Berechtigungen für die Datenbank gewährt wurde mithilfe der Windows-Authentifizierung.
> 
> 

Wenn Sie den Listener getestet haben, achten Sie darauf, dass Sie ein Failover der verfügbarkeitsgruppe, um sicherzustellen, dass Clients eine Verbindung mit dem Listener failoverübergreifend herstellen können.

