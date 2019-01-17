---
title: Zugreifen auf virtuelle Azure-Netzwerke über Azure Logic Apps mit Integrationsdienstumgebungen (ISEs)
description: In dieser Übersicht wird beschrieben, wie Logik-Apps mit Integrationsdienstumgebungen (ISEs) auf virtuelle Azure-Netzwerke (VNETs) zugreifen können.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 12/03/2018
ms.openlocfilehash: 2b11d74436907380811acda3b7427ebe8011afb4
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2019
ms.locfileid: "54061022"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Zugreifen auf Ressourcen virtueller Azure-Netzwerke über Azure Logic Apps mit Integrationsdienstumgebungen (ISEs)

> [!NOTE]
> Diese Funktion befindet sich in der *privaten Vorschau*. Um den Zugriff anzufordern, [erstellen Sie Ihre Beitrittsanforderung hier](https://aka.ms/iseprivatepreview).

Manchmal benötigen Ihre Logik-Apps und Integrationskonten Zugriff auf gesicherte Ressourcen wie virtuelle Computer (VMs) und andere Systeme oder Dienste innerhalb eines [virtuellen Azure-Netzwerks](../virtual-network/virtual-networks-overview.md). Um diesen Zugriff einzurichten, können Sie [eine *Integrationsdienstumgebung* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) zum Ausführen Ihrer Logik-Apps und Integrationskonten erstellen. 

![Auswählen der Integrationsdienstumgebung](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Bei der Erstellung einer ISE wird eine private und isolierte Logic Apps-Instanz in Ihrem virtuellen Azure-Netzwerk bereitgestellt. Diese private Instanz verwendet dedizierte Ressourcen wie z. B. Speicher und wird getrennt vom öffentlichen „globalen“ Logic Apps-Dienst ausgeführt. Diese Trennung trägt auch dazu bei, mögliche Auswirkungen anderer Azure-Mandanten auf die Leistung Ihrer Apps oder den [Effekt einer zu hohen Aktivität](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors) zu verringern. 

In dieser Übersicht wird beschrieben, wie eine ISE Logik-Apps und Integrationskonten ermöglicht, direkt auf Ihr virtuelles Azure-Netzwerk zuzugreifen. Außerdem werden die Unterschiede zwischen einer ISE und dem globalen Logic Apps-Dienst herausgestellt.

<a name="difference"></a>

## <a name="isolated-versus-global"></a>„Isoliert“ im Vergleich zu „global“

Wenn Sie in Azure eine Integrationsdienstumgebung (ISE) erstellen, wählen Sie das virtuelle Azure-Netzwerk aus, in das Sie Ihre ISE *einfügen* möchten. Azure stellt eine private Instanz des Logic Apps-Dienst in Ihrem virtuellen Netzwerk bereit. Dadurch wird eine isolierte Umgebung erstellt, in der Sie Logik-Apps auf dedizierten Ressourcen erstellen und ausführen können. Wenn Sie eine Logik-App erstellen, wählen Sie diese Umgebung als Speicherort Ihrer App aus, wodurch die Logik-App direkten Zugriff auf die Ressourcen im virtuellen Netzwerk erhält. 

Logik-Apps in einer Integrationsdienstumgebung bieten die gleiche Benutzeroberfläche und ähnliche Funktionen wie im globalen Logic Apps-Dienst. Sie können nicht nur die gleichen integrierten Aktionen und Connectors im globalen Logic Apps-Dienst verwenden, sondern auch ISE-spezifische Connectors. Zum Beispiel bieten die folgenden Standardconnectors Versionen an, die in einer Integrationsdienstumgebung ausgeführt werden können:
 
* Azure Blob Storage, File Storage und Table Storage
* Azure Queues, Azure Service Bus, Azure Event Hubs und IBM MQ
* FTP und SFTP-SSH
* SQL Server, SQL Data Warehouse, Azure Cosmos DB
* AS2, X12 und EDIFACT

Der Unterschied zwischen ISE- und Nicht-ISE-Connectors liegt darin, wo die Trigger und Aktionen ausgeführt werden:

* Sie können in Ihrer ISE die integrierten Trigger und Aktionen wie für HTTP verwenden, die in derselben Integrationsdienstumgebung wie Ihre Logik-App ausgeführt werden. 

* Für Connectors, die zwei Versionen anbieten, wird eine Version in einer ISE ausgeführt, während die andere Version im globalen Logic Apps-Dienst läuft.  

  Connectors mit der Bezeichnung **ISE** werden immer in der gleichen ISE wie Ihre Logik-App ausgeführt. Connectors ohne die Bezeichnung **ISE** werden im globalen Logic Apps-Dienst ausgeführt. 

  ![Auswählen der ISE-Connectors](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

* In einer ISE ausgeführte Connectors sind auch im globalen Logic Apps-Dienst verfügbar. 

> [!IMPORTANT]
> Für Logik-Apps, integrierte Aktionen und Connectors, die in Ihrer ISE ausgeführt werden, gilt ein anderer als der nutzungsbasierte Tarif. Weitere Informationen hierzu finden Sie unter [Logic Apps – Preise](../logic-apps/logic-apps-pricing.md).

<a name="vnet-access"></a>

## <a name="permissions-for-virtual-network-access"></a>Berechtigungen für den Zugriff auf virtuelle Netzwerke

Wenn Sie eine Integrationsdienstumgebung (ISE) erstellen, wählen Sie ein virtuelles Azure-Netzwerk aus, in das Sie Ihre Umgebung *einfügen*. Beim Einfügen wird eine private Instanz des Logic Apps-Diensts in Ihrem virtuellen Netzwerk bereitgestellt. Dadurch erhalten Sie eine isolierte Umgebung, in der Sie Logik-Apps auf dedizierten Ressourcen erstellen und ausführen können. Bei der Erstellung Ihrer Logik-Apps wählen Sie Ihre ISE als Speicherort für Ihre Apps aus. Diese Logik-Apps können dann direkt auf das virtuelle Netzwerk zugreifen und sich mit Ressourcen in diesem Netzwerk verbinden. 

Bei Systemen, die mit einem virtuellen Netzwerk verbunden sind, fügen Sie eine Integrationsdienstumgebung in dieses virtuelle Netzwerk ein. So können Ihre Logik-Apps direkt auf diese Systeme zugreifen, indem sie eins der folgenden Elemente verwenden: 

* ISE-Connector für dieses System, z.B. SQL Server

* HTTP-Aktion 

* Benutzerdefinierter Connector

Bei lokalen Systemen, die nicht mit einem virtuellen Netzwerk verbunden sind oder keine ISE-Connectors aufweisen, stellen Sie eine Verbindung mit diesen Systemen her, indem Sie [das lokale Datengateway einrichten und verwenden](../logic-apps/logic-apps-gateway-install.md).

Bevor Sie ein virtuelles Azure-Netzwerk auswählen können, um darin Ihre Umgebung einzufügen, müssen Sie in Ihrem virtuellem Netzwerk Berechtigungen für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für den Azure Logic Apps-Dienst einrichten. Hierbei ist es erforderlich, dass Sie dem Azure Logic Apps-Dienst die Rollen **Netzwerkmitwirkender** und **Mitwirkender für klassische Netzwerke** zuweisen.
Informationen zum Einrichten dieser Berechtigungen finden Sie unter [Herstellen einer Verbindung mit virtuellen Netzwerken mithilfe von isolierten Umgebungen](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#vnet-access).

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Integrationskonten mit ISE

In einer Integrationsdienstumgebung (ISE) haben Sie die Möglichkeit, Integrationskonten mit Logik-Apps zu verwenden. Diese Integrationskonten müssen jedoch auf *dieselbe ISE* wie die verknüpften Logik-Apps zugreifen. Logik-Apps in einer ISE können nur auf die Integrationskonten verweisen, die sich in derselben ISE befinden. Wenn Sie ein Integrationskonto erstellen, können Sie Ihre ISE als Speicherort für Ihr Integrationskonto auswählen.

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps-Forum</a>.
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die <a href="https://aka.ms/logicapps-wish" target="_blank">Website für Logic Apps-Benutzerfeedback</a>.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [eine Verbindung mit virtuellen Azure-Netzwerken über isolierte Logik-Apps herstellen können](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).
* Erfahren Sie mehr über [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Erfahren Sie mehr über die [Integration virtueller Netzwerke für Azure-Dienste](../virtual-network/virtual-network-for-azure-services.md).
