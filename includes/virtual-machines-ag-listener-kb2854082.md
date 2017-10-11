Als Nächstes, wenn alle Server auf dem Cluster Windows Server 2008 R2 oder Windows Server 2012 ausgeführt werden, Sie müssen sicherstellen, dass das Hotfix [KB2854082](http://support.microsoft.com/kb/2854082) auf alle auf lokalen Servern oder virtuellen Azure-Computern, die Teil des Clusters installiert ist. Alle Server oder virtuellen Computer, der den Cluster, aber nicht in der verfügbarkeitsgruppe muss auch dieses Hotfix installiert sein.

In der Remotedesktopsitzung für jeden der Clusterknoten herunterladen [KB2854082](http://support.microsoft.com/kb/2854082) in ein lokales Verzeichnis. Installieren Sie dann nacheinander den Hotfix auf jedem Clusterknoten. Wenn der Clusterdienst derzeit auf dem Clusterknoten ausgeführt wird, wird der Server am Ende der Hotfixinstallation neu gestartet.

> [!WARNING]
> Der Clusterdienst beenden oder Neustarten des Servers wirkt sich auf den quorumzustand des Clusters und der verfügbarkeitsgruppe, und es möglicherweise dazu führen, dass des Clusters offline geschaltet. Um die hohe Verfügbarkeit des Clusters während der Installation zu gewährleisten, stellen Sie sicher, dass:
> 
> * Der Cluster befindet sich im optimalen quorumzustand. 
> * Vor der Installation des Hotfixes auf einem beliebigen Knoten sind alle Clusterknoten online.
> * Bevor Sie den Hotfix auf einem anderen Knoten im Cluster installieren, das Hotfix zum Ausführen bis zum Abschluss auf einem Knoten, einschließlich vollständig Neustart des Servers ab.
> 
> 

