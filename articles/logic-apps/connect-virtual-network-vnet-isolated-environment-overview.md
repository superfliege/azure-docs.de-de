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
ms.date: 03/11/2019
ms.openlocfilehash: b53cd54afdf6243769602971ab77145cfa9ba9cc
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57758776"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Zugreifen auf Ressourcen virtueller Azure-Netzwerke über Azure Logic Apps mit Integrationsdienstumgebungen (ISEs)

> [!NOTE]
> Diese Funktion befindet sich derzeit in der [*öffentlichen Vorschau*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Manchmal benötigen Ihre Logik-Apps und Integrationskonten Zugriff auf gesicherte Ressourcen wie virtuelle Computer (VMs) und andere Systeme oder Dienste innerhalb eines [virtuellen Azure-Netzwerks](../virtual-network/virtual-networks-overview.md). Um diesen Zugriff einzurichten, können Sie [eine *Integrationsdienstumgebung* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) zum Ausführen Ihrer Logik-Apps und Integrationskonten erstellen. Bei der Erstellung einer ISE stellt Azure eine private und isolierte Instanz des Logic Apps-Diensts in Ihrem virtuellen Azure-Netzwerk bereit. Diese private Instanz verwendet dedizierte Ressourcen wie z. B. Speicher und wird getrennt vom öffentlichen „globalen“ Logic Apps-Dienst ausgeführt. Die Trennung Ihrer isolierten privaten Instanz von der öffentlichen globalen Instanz trägt auch dazu bei, die Auswirkungen zu verringern, die andere Azure-Mandanten auf die Leistung Ihrer Apps haben könnten. Hierbei spricht man auch vom [„Noisy-Neighbor“-Problem](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors).

Wenn Sie nach dem Erstellen Ihrer ISE Ihre Logik-App oder Ihr Integrationskonto erstellen, können Sie Ihre ISE als Standort Ihrer Logik-App oder Ihres Integrationskontos auswählen:

![Auswählen der Integrationsdienstumgebung](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Ihre Logik-App kann nun unter Verwendung eines der folgenden Elemente direkt auf Systeme zugreifen, die sich innerhalb Ihres virtuellen Netzwerks befinden oder mit diesem verbunden sind:

* Ein ISE-Connector mit Versionsangabe für dieses System, z.B. SQL Server
* Ein integrierter Trigger oder eine integrierte Aktion, z.B. der HTTP-Trigger oder die HTTP-Aktion
* Ein benutzerdefinierter Connector

In dieser Übersicht wird detailliert beschrieben, wie eine ISE Logik-Apps und Integrationskonten ermöglicht, direkt auf Ihr virtuelles Azure-Netzwerk zuzugreifen. Außerdem werden die Unterschiede zwischen einer ISE und dem globalen Logic Apps-Dienst herausgestellt.
Bei lokalen Systemen, die nicht mit einem virtuellen Netzwerk verbunden sind oder keine ISE-Connectors aufweisen, stellen Sie eine Verbindung mit diesen Systemen her, indem Sie [das lokale Datengateway einrichten und verwenden](../logic-apps/logic-apps-gateway-install.md).

> [!IMPORTANT]
> Für Logik-Apps, integrierte Aktionen und Connectors, die in Ihrer ISE ausgeführt werden, gilt ein anderer als der nutzungsbasierte Tarif. Weitere Informationen hierzu finden Sie unter [Logic Apps – Preise](../logic-apps/logic-apps-pricing.md).

<a name="difference"></a>

## <a name="isolated-versus-global"></a>„Isoliert“ im Vergleich zu „global“

Wenn Sie in Azure eine Integrationsdienstumgebung (ISE) erstellen, können Sie das virtuelle Azure-Netzwerk auswählen, in das Sie Ihre ISE *einfügen* möchten. Azure fügt dann eine private Instanz des Logic Apps-Diensts in Ihr virtuelles Netzwerk ein bzw. stellt diese im virtuellen Netzwerk bereit. Dadurch wird eine isolierte Umgebung erstellt, in der Sie Logik-Apps auf dedizierten Ressourcen erstellen und ausführen können. Wenn Sie Ihre Logik-App erstellen, wählen Sie Ihre ISE als Speicherort Ihrer App aus, wodurch die Logik-App direkten Zugriff auf Ihr virtuelles Netzwerk und die Ressourcen in diesem Netzwerk erhält.

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
