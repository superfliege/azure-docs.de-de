---
title: Zugreifen auf virtuelle Azure-Netzwerke über Azure Logic Apps
description: Diese Übersicht zeigt, wie isolierte Logik-Apps mit virtuellen Azure-Netzwerken aus Integrationsdienstumgebungen (Integration Service Environments, ISEs) verbunden werden können, die private und dedizierte Ressourcen nutzen.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 9546b8ca33ef7da2d570b547446858e2a4099234
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393140"
---
# <a name="access-to-azure-virtual-network-resources-from-isolated-azure-logic-apps"></a>Zugriff auf Azure Virtual Network-Ressourcen über isolierte Azure Logic Apps-Instanzen

> [!NOTE]
> Diese Funktion befindet sich in der *privaten Vorschau*. Um den Zugriff anzufordern, [erstellen Sie Ihre Beitrittsanforderung hier](https://aka.ms/iseprivatepreview).

Manchmal benötigen Ihre Logik-Apps und Integrationskonten Zugriff auf gesicherte Ressourcen wie virtuelle Computer (VMs) und andere Systeme oder Dienste innerhalb eines [virtuellen Azure-Netzwerks](../virtual-network/virtual-networks-overview.md). Um diesen Zugriff zu ermöglichen, können Sie [eine *Integrationsdienstumgebung* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) als Speicherort für die Erstellung Ihrer Logik-Apps und Integrationskonten erstellen. 

![Auswählen der Integrationsdienstumgebung](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Bei der Erstellung einer ISE wird eine private und isolierte Logic Apps-Instanz in Ihrem virtuellen Azure-Netzwerk bereitgestellt. Die private Instanz verwendet dedizierte Ressourcen wie z.B. Speicher und wird getrennt vom öffentlichen „globalen“ Logic Apps-Dienst ausgeführt. Diese Trennung trägt auch dazu bei, mögliche Auswirkungen anderer Azure-Mandanten auf die Leistung Ihrer Apps oder den [Effekt einer zu hohen Aktivität](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors) zu verringern. 

In dieser Übersicht wird beschrieben, wie die Erstellung einer ISE Logik-Apps und Integrationskonten dabei unterstützt, direkt auf Ressourcen in Ihrem virtuellen Azure-Netzwerk zuzugreifen. Außerdem werden die Unterschiede zwischen einer ISE und dem globalen Logic Apps-Dienst herausgestellt.

<a name="difference"></a>

## <a name="isolated-versus-global"></a>„Isoliert“ im Vergleich zu „global“

Wenn Sie in Azure eine Integrationsdienstumgebung (ISE) erstellen, können Sie ein virtuelles Azure-Netzwerk als *Peer* für Ihre Umgebung auswählen. Azure stellt eine private Instanz des Logic Apps-Diensts in Ihrem virtuellen Netzwerk bereit, sodass Sie über eine isolierte Umgebung verfügen, in der Sie Ihre Logik-Apps auf dedizierten Ressourcen erstellen und ausführen können. Wenn Sie eine Logik-App erstellen, können Sie diese Umgebung als Speicherort Ihrer App auswählen, wodurch die Logik-App auch direkten Zugriff auf die Ressourcen in Ihrem virtuellen Netzwerk erhält.  

Logik-Apps in einer Integrationsdienstumgebung bieten die gleiche Benutzeroberfläche und ähnliche Funktionen wie im globalen Logic Apps-Dienst. Sie können nicht nur die gleichen integrierten Aktionen und Connectors verwenden, die vom globalen Logic Apps-Dienst zur Verfügung gestellt werden, sondern auch aus Connectors wählen, die ISE-Versionen bereitstellen. Zum Beispiel bieten die folgenden Standardconnectors Versionen an, die in einer Integrationsdienstumgebung ausgeführt werden können:
 
* Azure Blob Storage, File Storage und Table Storage
* Azure-Warteschlangen
* Azure-Servicebus
* FTP
* SSH FTP (SFTP)
* SQL Server
* AS2, X12 und EDIFACT

Der Unterschied zwischen ISE und Nicht-ISE-Connectors liegt darin, wo die Trigger und Aktionen ausgeführt werden:

* Wenn Sie integrierte Trigger und Aktionen wie z.B. HTTP in Ihrer ISE verwenden, führen Sie diese Trigger und Aktionen immer in der gleichen ISE wie Ihre Logik-App aus. 

* Für Connectors, die zwei Versionen anbieten: Eine Version wird in einer ISE ausgeführt, während die andere Version im globalen Logic Apps-Dienst läuft.  

  Connectors mit der Bezeichnung **ISE** werden immer in der gleichen ISE wie Ihre Logik-App ausgeführt. Connectors ohne die Bezeichnung **ISE** werden im globalen Logic Apps-Dienst ausgeführt. 

  ![Auswählen der ISE-Connectors](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

* Connectors, die Sie in Ihrer ISE konfigurieren, sind auch für die Verwendung im globalen Logic Apps-Dienst verfügbar. 

> [!IMPORTANT]
> Für Logik-Apps, integrierte Aktionen und Connectors, die in Ihrer ISE ausgeführt werden, gilt ein anderer als der nutzungsbasierte Tarif. Weitere Informationen hierzu finden Sie unter [Logic Apps – Preise](../logic-apps/logic-apps-pricing.md).

<a name="vnet-access"></a>

## <a name="permissions-for-virtual-network-access"></a>Berechtigungen für den Zugriff auf virtuelle Netzwerke

Wenn Sie eine Integrationsdienstumgebung (ISE) erstellen, können Sie ein virtuelles Azure-Netzwerk als *Peer* für Ihre Umgebung auswählen. Sie können jedoch *ausschließlich* diese Beziehung oder *Peering* einrichten, wenn Sie Ihre ISE erstellen. Diese Beziehung gewährt der ISE Zugriff auf Ressourcen in Ihrem virtuellen Netzwerk, wodurch sich dann Logik-Apps in dieser ISE direkt mit Ressourcen in Ihrem virtuellen Netzwerk verbinden können. Bei lokalen Systemen in einem virtuellen Netzwerk, das mit einer Integrationsdienstumgebung verknüpft ist, können Logik-Apps direkt auf diese Systeme zugreifen, indem sie eins der folgenden Elemente verwenden: 

* ISE-Connector für dieses System, z.B. SQL Server

* HTTP-Aktion 

* Benutzerdefinierter Connector

Bei lokalen Systemen, die sich nicht in einem virtuellen Netzwerk befinden oder keine Integrationsdienstumgebungs-Connectors aufweisen, können Sie immer noch eine Verbindung herstellen, nachdem Sie [das lokale Datengateway eingerichtet haben und es verwenden](../logic-apps/logic-apps-gateway-install.md).

Bevor Sie ein virtuelles Azure-Netzwerk als Peer für Ihre Umgebung auswählen können, müssen Sie in Ihrem virtuellem Netzwerk Berechtigungen für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für den Azure Logic Apps-Dienst einrichten. Hierbei ist es erforderlich, dass Sie dem Azure Logic Apps-Dienst die Rollen **Netzwerkmitwirkender** und **Mitwirkender für klassische Netzwerke** zuweisen. Weitere Informationen zu den für das Peering erforderlichen Rollenberechtigungen finden Sie im Abschnitt [„Berechtigungen“ unter „Erstellen, Ändern oder Löschen eines Peerings virtueller Netzwerke“](../virtual-network/virtual-network-manage-peering.md#permissions).

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Integrationskonten mit ISE

Sie haben die Möglichkeit, Integrationskonten mit Logik-Apps zu verwenden, die in einer Integrationsdienstumgebung (ISE) ausgeführt werden. Dabei müssen diese Integrationskonten *dieselbe ISE* wie die verknüpften Logik-Apps verwenden. Logik-Apps in einer ISE können nur auf die Integrationskonten verweisen, die sich in derselben ISE befinden. Wenn Sie ein Integrationskonto erstellen, können Sie Ihre ISE als Speicherort für Ihr Integrationskonto auswählen.

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps-Forum</a>.
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die <a href="http://aka.ms/logicapps-wish" target="_blank">Website für Logic Apps-Benutzerfeedback</a>.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [eine Verbindung mit virtuellen Azure-Netzwerken über isolierte Logik-Apps herstellen können](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).
* Erfahren Sie mehr über [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Erfahren Sie mehr über die [Integration virtueller Netzwerke für Azure-Dienste](../virtual-network/virtual-network-for-azure-services.md).
