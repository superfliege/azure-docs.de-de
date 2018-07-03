| Ressource | Standardlimit |
| --- | :--- |
| Max. Knoten pro Cluster | 100 |
| Max. Pods pro Knoten ([grundlegende Netzwerke mit Kubernetes][basic-networking]) | 110 |
| Max. Pods pro Knoten ([erweiterte Netzwerke mit Azure CNI][advanced-networking]) | 30<sup>1</sup> |
| Max. Cluster pro Abonnement | 20<sup>2</sup> |

<sup>1</sup> Dieser Wert kann über die ARM-Vorlagenbereitstellung angepasst werden. Beispiele finden Sie [hier][arm-deployment-example].<br />
<sup>2</sup> Erstellen Sie eine [Azure Supportanfrage][azure-support], um eine Erhöhung des Grenzwerts anzufordern.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[arm-deployment-example]: https://github.com/Azure/AKS/blob/master/examples/vnet/02-aks-custom-vnet.json#L64-L69
