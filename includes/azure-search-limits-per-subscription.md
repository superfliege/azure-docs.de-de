Sie können mehrere Dienste im Rahmen eines Abonnements, die jeweils auf eine bestimmte Schicht, die nur durch die Anzahl der Dienste, die auf jeder Ebene zulässig beschränkt bereitgestellt erstellen. Beispielsweise können Sie bis zu 12 Dienste auf der grundlegenden Ebene und einem anderen 12 Dienste auf der Ebene S1 innerhalb desselben Abonnements erstellen. Weitere Informationen zu Dienstebenen finden Sie unter [wählen Sie eine SKU oder die Ebene für Azure Search](../articles/search/search-sku-tier.md).

Maximale diensteinschränkungen können auf Anforderung ausgelöst werden. Wenn Sie weitere Dienste innerhalb des gleichen Abonnements benötigen, wenden Sie sich an Azure-Support.

| Ressource | Free | Einfach | S1 | S2 | S3 | S3 HD <sup>1</sup> |
| --- | --- | --- | --- | --- | --- | --- |
| Maximale services |1 |12 |12 |6 |6 |6 |
| Die maximalen Dezimalstellen in "su" <sup>2</sup> |N/A <sup>3</sup> |3 SU <sup>4</sup> |36 "SU" |36 "SU" |36 "SU" |36 "SU" |

<sup>1</sup> S3 HD unterstützt keine [Indexer](../articles/search/search-indexer-overview.md) zu diesem Zeitpunkt. 

<sup>2</sup> sucheinheiten ("su") sind die Einheiten, die zugeordnet wurden, entweder als Abrechnung ein *Replikat* oder ein *Partition*. Sie benötigen beide Ressourcen für Speicher, Indizierung und Abfragevorgänge. Weitere Informationen finden Sie Informationen zu wie sucheinheiten berechnet werden, sowie ein Diagramm gültige Kombinationen, die unter der maximalen Anzahl bleiben, finden Sie unter [skalieren Ressourcenebenen für Abfrage- und arbeitsauslastungen](../articles/search/search-capacity-planning.md). 

<sup>3</sup> frei basieren auf freigegebenen Ressourcen von mehreren Abonnenten verwendet. Auf dieser Ebene sind keine dedizierten Ressourcen für einen einzelnen Abonnenten. Aus diesem Grund wird die maximalen Dezimalstellen als nicht verfügbar markiert.

<sup>4</sup> Basic verfügt über eine feste Partition. Auf dieser Ebene werden zusätzliche SUs zum Zuweisen von mehrere Replikate für verbesserte abfragearbeitsauslastungen.

