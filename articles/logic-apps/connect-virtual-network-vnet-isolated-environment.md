---
title: Verbinden mit Azure Virtual Networks über Azure Logic Apps
description: Um von Azure Logic Apps aus auf Azure Virtual Networks zuzugreifen, können Sie private, dedizierte und isolierte Integrationsdienstumgebungen erstellen, die Logik-Apps und andere Ressourcen vom öffentlichen oder „globalen“ Azure getrennt halten.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/25/2018
ms.openlocfilehash: 354c31014448b914b33d2bef5483efc78092f726
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47391920"
---
# <a name="create-isolated-environments-to-access-azure-virtual-networks-from-azure-logic-apps"></a>Erstellen isolierter Umgebungen für den Zugriff auf Azure Virtual Networks aus Azure Logic Apps

> [!NOTE]
> Diese Funktion befindet sich in der *privaten Vorschau*. Um den Zugriff anzufordern, [erstellen Sie Ihre Beitrittsanforderung hier](https://aka.ms/iseprivatepreview).

Für Integrationsszenarien, in denen Ihre Logik-Apps und Integrationskonten Zugriff auf ein [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) benötigen, können Sie eine [ *Integrationsdienstumgebung* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) erstellen, die eine Verknüpfung mit Ihrem virtuellen Netzwerk herstellt und den Logic Apps-Dienst in Ihrem Netzwerk bereitstellt. Beim Erstellen von Logik-Apps und Integrationskonten wählen Sie diese Integrationsdienstumgebung als deren Standort aus. Ihre Logik-Apps und Integrationskonten können dann direkt auf Ressourcen wie virtuelle Computer (VMs), Server, Systeme und Dienste in Ihrem virtuellen Netzwerk zugreifen. 

![Auswählen der Integrationsdienstumgebung](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Ihre Integrationsdienstumgebung ist eine private und isolierte Umgebung, die dedizierten Speicher und andere Ressourcen verwendet, die getrennt vom öffentlichen oder *globalen* Logic Apps-Dienst vorhanden sind. Diese Trennung trägt auch dazu bei, jegliche Auswirkungen anderer Azure-Mandanten auf die Leistung Ihrer Apps zu verringern. 

In diesem Artikel wird gezeigt, wie Sie die folgenden Aufgaben ausführen:

* Einrichten von Berechtigungen für Ihr Azure Virtual Network, damit die private Logic Apps-Instanz auf Ihr virtuelles Netzwerk zugreifen kann.

* Erstellen Ihrer Integrationsdienstumgebung. 

* Erstellen einer Logik-App, die in Ihrer Integrationsdienstumgebung ausgeführt werden kann. 

* Erstellen eines Integrationskontos für Ihre Logik-Apps in Ihrer Integrationsdienstumgebung.

Weitere Informationen zu Integrationsdienstumgebungen finden Sie unter [Zugriff auf Azure Virtual Network-Ressourcen aus isolierten Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich <a href="https://azure.microsoft.com/free/" target="_blank">für ein kostenloses Azure-Konto registrieren</a>. 

* Wenn Sie kein Azure Virtual Network besitzen, erfahren Sie, wie Sie [ein Azure Virtual Network erstellen](../virtual-network/quick-create-portal.md). 

  > [!IMPORTANT]
  > Zwar benötigen Sie kein Azure Virtual Network, um Ihre Umgebung zu erstellen, doch können Sie *nur* ein virtuelles Netzwerk als Peer für Ihre Umgebung auswählen, wenn Sie diese Umgebung erstellen. 

* Um Ihren Logik-Apps direkten Zugriff auf Ihre Azure Virtual Network zu gewähren, [richten Sie rollenbasierte Zugriffssteuerungsberechtigungen ein](#vnet-access), damit der Logic Apps-Dienst über die Berechtigungen für den Zugriff auf Ihr virtuelles Netzwerk verfügt. 

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="vnet-access"></a>

## <a name="set-virtual-network-permissions"></a>Einrichten von virtuellen Netzwerkberechtigungen

Wenn Sie Ihre Integrationsdienstumgebung erstellen, können Sie ein Azure Virtual Network als *Peer* für Ihre Umgebung auswählen. Sie können diesen Schritt, oder das *Peering*, jedoch nur ausführen, wenn Sie Ihre Umgebung erstellen. Diese Beziehung gestattet es dem Logic Apps-Dienst, direkt eine Verbindung mit Ressourcen in diesem virtuellen Netzwerk herzustellen, und gewährt Ihrer Umgebung den Zugriff auf diese Ressourcen. 

Bevor Sie Ihr virtuelles Netzwerk auswählen können, müssen Sie rollenbasierte Zugriffssteuerungsberechtigungen in Ihrem virtuellen Netzwerk einrichten. Um diese Aufgabe abzuschließen, müssen Sie dem Azure Logic Apps-Dienst bestimmte Rollen zuweisen.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrem virtuellen Netzwerk, und wählen Sie es aus. 

1. Wählen Sie im Menü Ihres virtuellen Netzwerks **Zugriffssteuerung (IAM)** aus. 

1. Wählen Sie unter **Zugriffssteuerung** die Option **Rollenzuweisung** aus, falls sie nicht bereits ausgewählt ist. Wählen Sie in der Symbolleiste **Rollenzuweisung** **Hinzufügen** aus. 

   ![Hinzufügen von Rollenzuweisungen](./media/connect-virtual-network-vnet-isolated-environment/set-up-role-based-access-control-vnet.png)

1. Richten Sie im Bereich **Berechtigungen hinzufügen** jede Rolle in dieser Tabelle für den Azure Logic Apps-Dienst ein. Stellen Sie sicher, dass Sie **Speichern** auswählen, nachdem Sie jede Rolle fertig gestellt haben:

   | Rolle | Zugriff zuweisen zu | Select | 
   |------|------------------|--------|
   | **Mitwirkender von virtuellem Netzwerk** | **Azure AD-Benutzer, -Gruppe oder -Anwendung** | **Azure Logic Apps** eingeben. Nachdem die Mitgliederliste angezeigt wird, wählen Sie denselben Wert aus. <p>**Tipp**: Wenn Sie diesen Dienst nicht finden, geben Sie die App-ID des Logic Apps-Diensts ein: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   | **Klassischer Mitwirkender** | **Azure AD-Benutzer, -Gruppe oder -Anwendung** | **Azure Logic Apps** eingeben. Nachdem die Mitgliederliste angezeigt wird, wählen Sie denselben Wert aus. <p>**Tipp**: Wenn Sie diesen Dienst nicht finden, geben Sie die App-ID des Logic Apps-Diensts ein: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   |||| 

   Beispiel: 

   ![Hinzufügen von Berechtigungen](./media/connect-virtual-network-vnet-isolated-environment/add-contributor-roles.png)

   Weitere Informationen zu den für das Peering erforderlichen Rollenberechtigungen finden Sie im Abschnitt [„Berechtigungen“ unter „Erstellen, Ändern oder Löschen eines Peerings virtueller Netzwerke“](../virtual-network/virtual-network-manage-peering.md#permissions). 

Wenn Ihr virtuelles Netzwerk über Azure ExpressRoute, Azure Point-to-Site VPN oder Azure Site-to-Site VPN verbunden ist, fahren Sie mit dem nächsten Abschnitt fort, damit Sie das erforderliche Gatewaysubnetz hinzufügen können. Andernfalls fahren Sie mit [Erstellen Ihrer Umgebung](#create-environment) fort.

<a name="add-gateway-subnet"></a>

## <a name="add-gateway-subnet-for-virtual-networks-with-expressroute-or-vpns"></a>Hinzufügen des Gatewaysubnetzes für virtuelle Netzwerke mit ExpressRoute oder VPNs

Nachdem Sie die vorherigen Schritte abgeschlossen haben, um Ihrer Integrationsdienstumgebung Zugriff auf ein Azure Virtual Network zu gewähren, das entweder über [Azure ExpressRoute](../expressroute/expressroute-introduction.md), [Azure Point-to-Site VPN](../vpn-gateway/point-to-site-about.md) oder [Azure Site-to-Site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) verbunden ist, müssen Sie außerdem Ihrem virtuellen Netzwerk ein [ *Gatewaysubnetz* ](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsub) hinzufügen:

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrem virtuellen Netzwerk, und wählen Sie es aus. Wählen Sie im Menü Ihres virtuellen Netzwerks **Subnetze** aus, und wählen Sie dann **Gatewaysubnetz** > **OK** aus.

   ![Gatewaysubnetz hinzufügen](./media/connect-virtual-network-vnet-isolated-environment/add-gateway-subnet.png)

1. Erstellen Sie jetzt eine [ *Routingtabelle*](../virtual-network/manage-route-table.md), die Sie dem Gatewaysubnetz zuordnen, das Sie zuvor erstellt haben.

   1. Wählen Sie im Hauptmenü von Azure **Ressource erstellen** > 
   **Netzwerk** > **Routingtabelle** aus.

      ![Erstellen einer Routingtabelle](./media/connect-virtual-network-vnet-isolated-environment/create-route-table.png)

   1. Geben Sie Informationen zu der Routingtabelle an, z.B. den Namen, Ihr zu verwendendes Azure-Abonnement, die Azure-Ressourcengruppe und den Standort. Stellen Sie sicher, dass die Eigenschaft **BGP-Routenverteilung** auf **Aktiviert** festgelegt ist, und wählen Sie dann **Erstellen** aus.

      ![Bereitstellen von Routingtabellendetails](./media/connect-virtual-network-vnet-isolated-environment/enter-route-table-information.png)

   1. Wählen Sie im Routingtabellenmenü **Subnetze** aus, und wählen Sie dann **Zuordnen** aus. 

      ![Verbinden der Routingtabelle mit dem Subnetz](./media/connect-virtual-network-vnet-isolated-environment/associate-route-table.png)

   1. Wählen Sie **Virtuelles Netzwerk** und dann Ihr virtuelles Netzwerk aus.
   
   1. Wählen Sie **Subnetz** und dann Ihr zuvor erstelltes Gatewaysubnetz aus.

   1. Wählen Sie **OK** aus, wenn Sie fertig sind.

1. Wenn Sie ein Point-to-Site VPN haben, führen Sie auch diese Schritte aus:

   1. Suchen Sie in Azure die Gatewayressource Ihres virtuellen Netzwerks, und wählen Sie sie aus.

   1. Wählen Sie im Menü des Gateways **Point-to-Site-Konfiguration** aus. 
   und wählen Sie dann **VPN-Client herunterladen** aus, damit Sie die aktuellste VPN-Clientkonfiguration haben.

      ![Herunterladen des neuesten VPN-Clients](./media/connect-virtual-network-vnet-isolated-environment/download-vpn-client.png)

Sie sind jetzt fertig mit dem Einrichten eines Gatewaysubnetzes für virtuelle Netzwerke, die ExpressRoute, Point-to-Site VPNs oder Site-to-Site VPNs verwenden. Um das Erstellen Ihrer Integrationsdienstumgebung fortzusetzen, befolgen Sie die nächsten Schritte.

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Erstellen Ihrer Integrationsdienstumgebung

Befolgen Sie zum Erstellen Ihrer Integrationsdienstumgebung diese Schritte:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Azure-Hauptmenü **Ressource erstellen** aus.

   ![Neue Ressource erstellen](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Geben Sie in das Suchfeld „Integrationsdienstumgebung“ als Ihren Filter ein.
Wählen Sie in der Ergebnisliste **Integrationsdienstumgebung (Preview)** und dann **Erstellen** aus.

   ![Auswählen von „Integrationsdienstumgebung“](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   ![Auswählen von „Erstellen“](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Geben Sie diese Details für Ihre Umgebung an:

   ![Angeben von Umgebungsdetails](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Eigenschaft | Erforderlich | Wert | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **Name** | JA | <*Umgebungsname*> | Der Name für Ihre Umgebung | 
   | **Abonnement** | JA | <*Name des Azure-Abonnements*> | Das für Ihre Umgebung zu verwendende Azure-Abonnement | 
   | **Ressourcengruppe** | JA | <*Name der Azure-Ressourcengruppe*> | Die Azure-Ressourcengruppe, in der Sie Ihre Umgebung erstellen möchten. |
   | **Location** | JA | <*Azure-Datencenterregion*> | Die Azure-Datencenterregion, in der Informationen zu Ihrer Umgebung gespeichert werden sollen. |
   | **Peer-VNET** | Nein  | <*Azure-VNET-Name*> | Das Azure Virtual Network, das Ihrer Umgebung als *Peer* zugeordnet werden soll, damit Logik-Apps in dieser Umgebung auf Ihr virtuelles Netzwerk zugreifen können. Bevor Sie diese Beziehung erstellen können, stellen Sie sicher, dass Sie bereits [rollenbasierte Zugriffssteuerung in Ihrem virtuellen Netzwerk für Azure Logic Apps eingerichtet haben](#vnet-access). <p>**Wichtig**: Obwohl ein virtuelles Netzwerk nicht erforderlich ist, können Sie ein virtuelles Netzwerk *nur*auswählen, wenn Sie Ihre Umgebung erstellen. | 
   | **Peeringname** | Ja, mit einem ausgewählten virtuellen Netzwerk | <*Peeringname*> | Der Name für die Peerbeziehung | 
   | **VNET-IP-Bereich** | Ja, mit einem ausgewählten virtuellen Netzwerk | <*IP-Adressbereich*> | Der IP-Adressbereich zum Erstellen von Ressourcen in Ihrer Umgebung. Dieser Bereich muss das [CIDR-Format (Classless Inter-Domain Routing, klassenloses domänenübergreifendes Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) verwenden, z. B. 10.0.0.1/16, und erfordert einen Class B-Adressraum. Der Bereich darf nicht innerhalb des Adressraums für das im **Peer-VNET** ausgewählte virtuelle Netzwerk vorhanden sein, noch innerhalb anderer privater IP-Adressen, mit denen das Peernetzwerk verbunden ist, sei es mittels Peering oder über Gateways. <p><p>**Wichtig**: Sie können diesen Adressbereich *nicht mehr ändern*, nachdem Sie Ihre Umgebung erstellt haben. |
   |||||
   
1. Wenn Sie fertig sind, wählen Sie **Erstellen** aus. 

   Azure beginnt mit dem Bereitstellen Ihrer Umgebung, aber dieser Prozess kann *bis zu zwei Stunden* dauern, bevor er abgeschlossen ist. 
   Um den Bereitstellungsstatus zu überprüfen, wählen Sie in Ihrer Azure-Symbolleiste das Symbol „Benachrichtigungen“ aus, wodurch der Benachrichtigungsbereich geöffnet wird.

   ![Überprüfen des Bereitstellungsstatus](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Wenn die Bereitstellung erfolgreich abgeschlossen wurde, zeigt Azure diese Benachrichtigung an:

   ![Bereitstellung erfolgreich](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

1. Um Ihre Umgebung anzuzeigen, wählen Sie **Zu Ressource wechseln** aus, wenn Azure nach Abschluss der Bereitstellung nicht automatisch zu Ihrer Umgebung wechselt.  

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Erstellen einer Logik-App – Integrationsdienstumgebung

Um Logik-Apps zu erstellen, die Ihre Integrationsdienstumgebung verwenden, führen Sie die üblichen Schritte in [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) aus, jedoch mit folgenden Unterschieden und Überlegungen: 

* Wenn Sie Ihre Logik-App erstellen, listet die Eigenschaft **Standort** Ihre Integrationsdienstumgebungen unter **Integrationsdienstumgebungen** auf, zusammen mit den verfügbaren Regionen. Wählen Sie Ihre Integrationsdienstumgebung aus, statt einer Region, z. B.:

  ![Auswählen der Integrationsdienstumgebung](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Sie können dieselben Integrationen wie den HTTP-Trigger oder die HTTP-Aktion verwenden, die in derselben Integrationsdienstumgebung wie die übergeordnete Logik-App ausgeführt werden. Connectors mit der Bezeichnung **ISE** werden auch in derselben Integrationsdienstumgebung wie die übergeordnete Logik-App ausgeführt. Connectors ohne die Bezeichnung **ISE** werden im globalen Logic Apps-Dienst ausgeführt.

  ![Auswählen der ISE-Connectors](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Wenn Sie zuvor Ihre Integrationsdienstumgebung mit einem Azure Virtual Network als Peer eingerichtet haben, können die Logik-Apps in Ihrer Integrationsdienstumgebung direkt auf Ressourcen in diesem virtuellen Netzwerk zugreifen. Bei lokalen Systemen in einem virtuellen Netzwerk, das mit einer Integrationsdienstumgebung verknüpft ist, können Logik-Apps direkt auf diese Systeme zugreifen, indem sie eins der folgenden Elemente verwenden: 

  * ISE-Connector für dieses System, z.B. SQL Server

  * HTTP-Aktion 

  * Benutzerdefinierter Connector

  Bei lokalen Systemen, die sich nicht in einem virtuellen Netzwerk befinden oder keine Integrationsdienstumgebungs-Connectors haben, können Sie immer noch eine Verbindung herstellen, nachdem Sie [das lokale Datengateway eingerichtet haben und es verwenden](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Erstellen des Integrationskontos – Integrationsdienstumgebung

Um ein Integrationskonto mit Logik-Apps in einer Integrationsdienstumgebung zu verwenden, muss dieses Integrationskonto *dieselbe Umgebung* wie die Logik-Apps verwenden. Logik-Apps in einer Integrationsdienstumgebung können nur auf Integrationskonten in derselben Integrationsdienstumgebung verweisen. 

Führen Sie zum Erstellen eines Integrationskontos, das eine Integrationsdienstumgebung verwendet, die üblichen Schritte in [Erstellen von Integrationskonten](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) aus, mit Ausnahme der Eigenschaft **Standort**, die nun Ihre Integrationsdienstumgebungen unter **Integrationsdienstumgebungen** aufführt, zusammen mit den verfügbaren Regionen. Wählen Sie Ihre Integrationsdienstumgebung aus, statt einer Region, z. B.:

![Auswählen der Integrationsdienstumgebung](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps-Forum</a>.
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die <a href="http://aka.ms/logicapps-wish" target="_blank">Website für Logic Apps-Benutzerfeedback</a>.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Erfahren Sie mehr über die [Integration virtueller Netzwerke für Azure-Dienste](../virtual-network/virtual-network-for-azure-services.md).
