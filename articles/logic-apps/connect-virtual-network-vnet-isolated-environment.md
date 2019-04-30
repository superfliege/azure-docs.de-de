---
title: Herstellen einer Verbindung mit virtuellen Azure-Netzwerken in Azure Logic Apps über eine Integrationsdienstumgebung (ISE)
description: Erstellen Sie eine Integrationsdienstumgebung (Integration Service Environment, ISE), damit Logik-Apps und Integrationskonten auf virtuelle Azure-Netzwerke (VNETs) zugreifen und dabei trotzdem privat und vom öffentlichen oder globalen Azure-Dienst isoliert bleiben können.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: 8cbc02f80244b02b397162309fa5ae047f3f460a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59995998"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Herstellen einer Verbindung mit virtuellen Azure-Netzwerken in Azure Logic Apps mithilfe einer Integrationsdienstumgebung

> [!NOTE]
> Diese Funktion befindet sich derzeit in der [*öffentlichen Vorschau*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Für Szenarios, in denen Ihre Logik-Apps und Integrationskonten Zugriff auf ein [virtuelles Azure-Netzwerk](../virtual-network/virtual-networks-overview.md) benötigen, erstellen Sie eine [*Integrationsdienstumgebung* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Eine Integrationsdienstumgebung ist eine private und isolierte Umgebung, die dedizierten Speicher und andere Ressourcen verwendet, die vom öffentlichen oder „globalen“ Logic Apps-Dienst getrennt bleiben. Diese Trennung trägt auch dazu bei, jegliche Auswirkungen anderer Azure-Mandanten auf die Leistung Ihrer Apps zu verringern. Ihre ISE wird in Ihr virtuelles Azure-Netzwerk *eingefügt*. Daraufhin wird der Logic Apps-Dienst in Ihrem virtuellen Netzwerk bereitgestellt. Wählen Sie diese Integrationsdienstumgebung beim Erstellen einer Logik-App oder eines Integrationskontos als Standort aus. Ihre Logik-App bzw. Ihr Integrationskonto kann dann direkt auf Ressourcen wie virtuelle Computer (VMs), Server, Systeme und Dienste in Ihrem virtuellen Netzwerk zugreifen.

![Auswählen der Integrationsdienstumgebung](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

In diesem Artikel wird gezeigt, wie Sie die folgenden Aufgaben ausführen:

* Einrichten von Ports in Ihrem virtuellen Azure-Netzwerk, damit der Datenverkehr durch Ihre Integrationsdienstumgebung über Subnetze in Ihrem virtuellen Netzwerk übertragen werden kann.

* Erstellen Ihrer Integrationsdienstumgebung.

* Erstellen einer Logik-App, die in Ihrer Integrationsdienstumgebung ausgeführt werden kann.

* Erstellen eines Integrationskontos für Ihre Logik-Apps in Ihrer Integrationsdienstumgebung.

Weitere Informationen zu Integrationsdienstumgebungen finden Sie unter [Zugriff auf Azure Virtual Network-Ressourcen aus Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich <a href="https://azure.microsoft.com/free/" target="_blank">für ein kostenloses Azure-Konto registrieren</a>.

  > [!IMPORTANT]
  > Für Logik-Apps, integrierte Aktionen und Connectors, die in Ihrer ISE ausgeführt werden, gilt ein anderer als der nutzungsbasierte Tarif. Weitere Informationen hierzu finden Sie unter [Logic Apps – Preise](../logic-apps/logic-apps-pricing.md).

* Ein [virtuelles Azure-Netzwerk](../virtual-network/virtual-networks-overview.md). Wenn Sie kein virtuelles Netzwerk besitzen, erfahren Sie, wie Sie [ein virtuelles Azure-Netzwerk erstellen](../virtual-network/quick-create-portal.md). 

  * Ihr virtuelles Netzwerk muss vier *leere* Subnetze aufweisen, um Ressourcen in Ihrer ISE bereitstellen und erstellen zu können. Sie können diese Subnetze im Voraus erstellen oder warten, bis Sie Ihre ISE erstellt haben, in der Sie Subnetze parallel erstellen können. Erfahren Sie mehr über die [Voraussetzungen für Subnetze](#create-subnet). 
  
    > [!NOTE]
    > Wenn Sie [ExpressRoute](../expressroute/expressroute-introduction.md) verwenden, das eine private Verbindung mit Microsoft Cloud Services bereitstellt, müssen Sie [eine Routentabelle](../virtual-network/manage-route-table.md) erstellen, die die folgende Route enthält, und diese Tabelle mit jedem von Ihrer ISE verwendeten Subnetz verknüpfen:
    > 
    > **Name**: <*Routenname*><br>
    > **Adresspräfix**: 0.0.0.0/0<br>
    > **Nächster Hop:** Internet

  * Achten Sie darauf, [dass Ihr virtuelles Netzwerk diese Ports zur Verfügung stellt](#ports), damit Ihre ISE ordnungsgemäß funktioniert und der Zugriff darauf gewährleistet ist.

* Wenn Sie benutzerdefinierte DNS-Server für Ihr virtuelles Azure-Netzwerk verwenden möchten, [richten Sie diese Server gemäß dieser Schritte ein](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md), bevor Sie Ihre ISE in Ihrem virtuellen Netzwerk bereitstellen. Andernfalls müssen Sie bei jeder Änderung Ihres DNS-Servers auch Ihre Integrationsdienstumgebung neu starten, was eine Funktion ist, die in der öffentlichen Vorschau der Integrationsdienstumgebung verfügbar ist.

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="ports"></a>

## <a name="set-up-network-ports"></a>Einrichten von Netzwerkports

Damit Ihre Integrationsdienstumgebung ordnungsgemäß funktioniert und der Zugriff darauf gewährleistet ist, müssen bestimmte Ports in Ihrem virtuellen Netzwerk verfügbar sein. Wenn einer dieser Ports nicht verfügbar sein sollte, verlieren Sie möglicherweise den Zugriff auf Ihre Integrationsdienstumgebung, was zu Funktionsstörungen führen kann. Wenn Sie eine Integrationsdienstumgebung in einem virtuellen Netzwerk verwenden, ist ein häufiges Einrichtungsproblem, dass ein oder mehrere Ports blockiert sind. Für Verbindungen zwischen Ihrer Integrationsdienstumgebung und dem Zielsystem kann der von Ihnen verwendete Connector auch eigene Portanforderungen aufweisen. Wenn Sie beispielsweise über den FTP-Connector mit einem FTP-System kommunizieren, stellen Sie sicher, dass der Port, den Sie auf diesem FTP-System verwenden, wie z.B. Port 21 zum Senden von Befehlen, verfügbar ist.

Sie können [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md) für diese Subnetze einrichten, [indem Sie den Datenverkehr in Subnetzen filtern](../virtual-network/tutorial-filter-network-traffic.md), um den Datenverkehr in den Subnetzen des virtuellen Netzwerks zu steuern, in denen Sie Ihre Integrationsdienstumgebung bereitstellen. In diesen Tabellen werden die Ports in Ihrem virtuellen Netzwerk beschrieben, die Ihre Integrationsdienstumgebung verwendet, und wo diese Ports verwendet werden. Die [Resource Manager-Diensttags](../virtual-network/security-overview.md#service-tags) stellen eine Gruppe von IP-Adresspräfixen dar, deren Aufgabe es ist, die Komplexität bei der Erstellung von Sicherheitsregeln zu verringern.

> [!IMPORTANT]
> Für die interne Kommunikation in Ihren Subnetzen ist es für die ISE erforderlich, dass Sie alle Ports in diesen Subnetzen öffnen.

| Zweck | Direction | Ports | Quelldiensttag | Zieldiensttag | Notizen |
|---------|-----------|-------|--------------------|-------------------------|-------|
| Datenverkehr aus Azure Logic Apps | Ausgehend | 80 und 443 | VirtualNetwork | Internet | Der Port hängt vom externen Dienste ab, mit dem der Logic Apps-Dienst kommuniziert. |
| Azure Active Directory | Ausgehend | 80 und 443 | VirtualNetwork | AzureActiveDirectory | |
| Azure Storage-Abhängigkeit | Ausgehend | 80 und 443 | VirtualNetwork | Storage | |
| Kommunikation zwischen Subnetzen | Ein- und ausgehend | 80 und 443 | VirtualNetwork | VirtualNetwork | Für die Kommunikation zwischen Subnetzen |
| Datenverkehr zu Azure Logic Apps | Eingehend | 443 | Internet  | VirtualNetwork | Die IP-Adresse des Computers oder Diensts, der einen Anforderungstrigger oder Webhook aufruft, der in Ihrer Logik-App vorhanden ist. Wenn Sie diesen Port schließen oder blockieren, werden HTTP-Aufrufe Ihrer Logik-Apps mit Anforderungstriggern verhindert.  |
| Ausführungsverlauf einer Logik-App | Eingehend | 443 | Internet  | VirtualNetwork | Die IP-Adresse des Computers, über den Sie den Ausführungsverlauf der Logik-App anzeigen. Wenn Sie diesen Port schließen oder blockieren, können Sie den Ausführungsverlauf zwar anzeigen, aber Sie können weder Ein- noch Ausgaben für die Schritte im Ausführungsverlauf anzeigen. |
| Verbindungsverwaltung | Ausgehend | 443 | VirtualNetwork  | Internet | |
| Veröffentlichen von Diagnoseprotokollen und Metriken | Ausgehend | 443 | VirtualNetwork  | AzureMonitor | |
| Kommunikation über Azure Traffic Manager | Eingehend | 443 | AzureTrafficManager | VirtualNetwork | |
| Logic Apps-Designer: dynamische Eigenschaften | Eingehend | 454 | Internet  | VirtualNetwork | Anforderungen werden von den [eingehenden IP-Adressen des Logic Apps-Zugriffsendpunkts in dieser Region gesendet](../logic-apps/logic-apps-limits-and-config.md#inbound). |
| Abhängigkeit von der App Service-Verwaltung | Eingehend | 454 und 455 | AppServiceManagement | VirtualNetwork | |
| Bereitstellen von Connectors | Eingehend | 454 und 3443 | Internet  | VirtualNetwork | Zum Bereitstellen und Aktualisieren von Connectors erforderlich. Wenn Sie diesen Port schließen oder blockieren, führt dies zu Fehlern bei ISE-Bereitstellungen, und es können keine Connectorupdates oder -fixes durchgeführt werden. |
| Azure SQL-Abhängigkeiten | Ausgehend | 1433 | VirtualNetwork | SQL |
| Azure Resource Health | Ausgehend | 1886 | VirtualNetwork | Internet | Zum Veröffentlichen des Integritätsstatus in Resource Health |
| API Management: Verwaltungsendpunkt | Eingehend | 3443 | APIManagement  | VirtualNetwork | |
| Abhängigkeit von Richtlinie zum Anmelden bei Event Hub und Überwachungs-Agent | Ausgehend | 5672 | VirtualNetwork  | EventHub | |
| Zugriff auf Azure Cache for Redis-Instanzen zwischen Rolleninstanzen | Eingehend <br>Ausgehend | 6379 – 6383 | VirtualNetwork  | VirtualNetwork | Damit die ISE mit Azure Cache for Redis funktioniert, müssen Sie die [Ports für ausgehenden und eingehenden Datenverkehr öffnen, die in den häufig gestellten Fragen zu Azure Cache for Redis beschrieben werden](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Azure Load Balancer | Eingehend | * | AzureLoadBalancer | VirtualNetwork |  |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Erstellen Ihrer Integrationsdienstumgebung

Befolgen Sie zum Erstellen Ihrer Integrationsdienstumgebung diese Schritte:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Azure-Hauptmenü **Ressource erstellen** aus.

   ![Neue Ressource erstellen](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Geben Sie in das Suchfeld „Integrationsdienstumgebung“ als Ihren Filter ein.
Wählen Sie in der Ergebnisliste **Integrationsdienstumgebung (Preview)** und dann **Erstellen** aus.

   ![Auswählen von „Integrationsdienstumgebung“](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   ![Auswählen von „Erstellen“](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Geben Sie diese Details für Ihre Umgebung an, und wählen Sie dann **Bewerten + erstellen** aus, z. B.:

   ![Angeben von Umgebungsdetails](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Eigenschaft | Erforderlich | Value | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **Abonnement** | Ja | <*Name des Azure-Abonnements*> | Das für Ihre Umgebung zu verwendende Azure-Abonnement |
   | **Ressourcengruppe** | Ja | <*Name der Azure-Ressourcengruppe*> | Die Azure-Ressourcengruppe, in der Sie Ihre Umgebung erstellen möchten. |
   | **Name der Integrationsdienstumgebung** | Ja | <*Umgebungsname*> | Der Name für Ihre Umgebung |
   | **Location** | Ja | <*Azure-Datencenterregion*> | Die Azure-Datencenterregion, in der Sie Ihre Umgebung bereitstellen. |
   | **Zusätzliche Kapazität** | Ja | 0, 1, 2, 3 | Die Anzahl der für diese ISE-Ressource zu verwendenden Verarbeitungseinheiten. Weitere Informationen zum Hinzufügen von Kapazität nach dem Erstellen finden Sie im Abschnitt zum [Hinzufügen von Kapazität](#add-capacity) in diesem Artikel. |
   | **Virtuelles Netzwerk** | Ja | <*Azure-virtual-network-name*> | Das virtuelle Azure-Netzwerk, in das Sie Ihre Umgebung einfügen möchten, damit Logik-Apps in dieser Umgebung auf Ihr virtuelles Netzwerk zugreifen können. Wenn Sie über kein Netzwerk verfügen, können Sie hier eins erstellen. <p>**Wichtig**: Sie können diese Einfügung *nur* einmalig durchführen, wenn Sie Ihre ISE erstellen. Bevor Sie diese Beziehung jedoch erstellen können, stellen Sie sicher, dass Sie bereits rollenbasierte Zugriffssteuerung in Ihrem virtuellen Netzwerk für Azure Logic Apps eingerichtet haben. |
   | **Subnetze** | Ja | <*subnet-resource-list*> | Eine ISE erfordert vier *leere* Subnetze zum Erstellen von Ressourcen in Ihrer Umgebung. Um jedes Subnetz zu erstellen, [führen Sie die Schritte unter dieser Tabelle aus](#create-subnet).  |
   |||||

   <a name="create-subnet"></a>

   **Erstellen eines Subnetzes**

   Ihre ISE erfordert vier *leere* Subnetze, die an keinen Dienst delegiert wurden, um eine Ressource in Ihrer Umgebung zu erstellen. 
   Sie können diese Subnetzadressen *nicht mehr ändern*, nachdem Sie Ihre Umgebung erstellt haben. Jedes Subnetz muss diese Kriterien erfüllen:

   * Einen Namen, der mit einem alphabetischen Zeichen oder Unterstrich beginnt und keines der folgenden Zeichen beinhaltet: `<`, `>`, `%`, `&`, `\\`, `?`, `/`

   * Das Format [Classless Inter-Domain Routing (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) und einen Class B-Adressraum

   * Es muss mindestens `/27` im Adressbereich verwenden, weil jedes Subnetz *mindestens* 32 Adressen aufweisen muss. Beispiel: 

     * `10.0.0.0/27` hat 32 Adressen, da 2<sup>(32 – 27)</sup> 2<sup>5</sup> oder 32 ist.

     * `10.0.0.0/24` hat 256 Adressen, da 2<sup>(32 – 24)</sup> 2<sup>8</sup> oder 256 ist.

     * `10.0.0.0/28` hat nur 16 Adressen und ist somit zu klein, da 2<sup>(32 – 28)</sup> 2<sup>4</sup> oder 16 ist.

     Weitere Informationen zum Berechnen der Adressen finden Sie unter [Übersicht für IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Wenn Sie [ExpressRoute](../expressroute/expressroute-introduction.md) verwenden, müssen Sie daran denken, [eine Routentabelle](../virtual-network/manage-route-table.md) zu erstellen, die die folgende Route enthält, und diese Tabelle mit jedem von Ihrer ISE verwendeten Subnetz verknüpfen:

     **Name**: <*Routenname*><br>
     **Adresspräfix**: 0.0.0.0/0<br>
     **Nächster Hop:** Internet

   1. Wählen Sie unter der Liste **Subnetze** den Eintrag **Subnetzkonfiguration verwalten** aus.

      ![Verwalten der Subnetzkonfiguration](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. Wählen Sie im Bereich **Subnetze** den Eintrag **Subnetz** aus.

      ![Hinzufügen des Subnetzes](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. Geben Sie im Bereich **Subnetz hinzufügen** diese Informationen an.

      * **Name**: Der Name für Ihr Subnetz.
      * **Adressbereich (CIDR-Block)**: Der Bereich Ihres Subnetzes in Ihrem virtuellen Netzwerk und im CIDR-Format

      ![Hinzufügen von Subnetzdetails](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. Wählen Sie **OK** aus, wenn Sie fertig sind.

   1. Wiederholen Sie diese Schritte für drei weitere Subnetze.

1. Nachdem Azure Ihre ISE-Informationen erfolgreich überprüft hat, wählen Sie **Erstellen** aus, z. B.:

   ![Wählen Sie nach der erfolgreicher Überprüfung „Erstellen“ aus.](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure beginnt mit der Bereitstellung Ihrer Umgebung, aber dieser Prozess *könnte* bis zu zwei Stunden dauern, bevor er abgeschlossen ist. 
   Um den Bereitstellungsstatus zu überprüfen, wählen Sie in Ihrer Azure-Symbolleiste das Symbol „Benachrichtigungen“ aus, wodurch der Benachrichtigungsbereich geöffnet wird.

   ![Überprüfen des Bereitstellungsstatus](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Wenn die Bereitstellung erfolgreich abgeschlossen wurde, zeigt Azure diese Benachrichtigung an:

   ![Bereitstellung erfolgreich](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   > [!NOTE]
   > Schlägt die Bereitstellung fehl, oder wenn Sie Ihre ISE löschen, *könnte* Azure bis zu eine Stunde benötigen, bis Ihre Subnetze freigegeben werden. Daher müssen Sie möglicherweise warten, bevor Sie diese Subnetzen in einer anderen ISE wiederverwenden können.

1. Um Ihre Umgebung anzuzeigen, wählen Sie **Zu Ressource wechseln** aus, wenn Azure nach Abschluss der Bereitstellung nicht automatisch zu Ihrer Umgebung wechselt.  

<a name="add-capacity"></a>

### <a name="add-capacity"></a>Hinzufügen einer Kapazität

Ihre ISE-Basiseinheit hat eine feste Kapazität. Wenn Sie mehr Durchsatz benötigen, können Sie weitere Skalierungseinheiten hinzufügen. Sie können basierend auf Leistungsmetriken oder einer bestimmten Anzahl an Verarbeitungseinheiten die Kapazität automatisch skalieren. Wenn Sie die automatische Skalierung basierend auf Metriken durchführen möchten, können Sie sich für verschiedene Kriterien entscheiden und Schwellenwerte für diese Kriterien festlegen.

1. Suchen Sie im Azure-Portal nach Ihrer ISE.

1. Klicken Sie im Hauptmenü Ihrer ISE auf **Übersicht**, um Leistungsmetriken für Ihre ISE anzuzeigen.

1. Wählen Sie unter **Einstellungen** **Horizontal hochskalieren**, um die automatische Skalierung einzurichten. Wählen Sie auf der Registerkarte **Konfigurieren** **Autoskalierung aktivieren** aus.

1. Wählen Sie im Bereich **Standard** entweder **Basierend auf einer Metrik skalieren** oder **Auf eine bestimmte Anzahl von Instanzen skalieren** aus.

1. Geben Sie eine Anzahl der Verarbeitungseinheiten zwischen 0 und einschließlich 3 ein, wenn Sie sich für die instanzbasierte Option entscheiden. Befolgen Sie für die metrikbasierte Option die folgenden Schritte:

   1. Wählen Sie im Bereich **Standard** **Regel hinzufügen** aus.

   1. Richten Sie im Bereich **Horizontal hochskalieren** Ihre Kriterien und die entsprechenden Maßnahmen beim Auslösen der Regel ein.

   1. Klicken Sie auf **Hinzufügen**, wenn Sie fertig sind.

1. Speichern Sie unbedingt die Änderungen, wenn Sie fertig sind.

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Erstellen einer Logik-App – Integrationsdienstumgebung

Um Logik-Apps zu erstellen, die Ihre Integrationsdienstumgebung verwenden, führen Sie die Schritte in [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) aus, jedoch mit folgenden Unterschieden: 

* Wenn Sie Ihre Logik-App erstellen, wählen Sie unter der Eigenschaft **Standort** Ihre Integrationsdienstumgebung im Abschnitt **Integrationsdienstumgebungen** aus, z. B.:

  ![Auswählen der Integrationsdienstumgebung](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Sie können dieselben integrierten Trigger und Aktionen (wie z.B. HTTP) verwenden, die in derselben ISE wie Ihre Logik-App ausgeführt werden. Connectors mit der Bezeichnung **ISE** werden auch in derselben Integrationsdienstumgebung wie Ihre Logik-App ausgeführt. Connectors ohne die Bezeichnung **ISE** werden im globalen Logic Apps-Dienst ausgeführt.

  ![Auswählen der ISE-Connectors](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Nachdem Sie Ihre Integrationsdienstumgebung in ein virtuelles Azure-Netzwerk eingefügt haben, können die Logik-Apps in Ihrer Integrationsdienstumgebung direkt auf Ressourcen in diesem virtuellen Netzwerk zugreifen. Bei lokalen Systemen, die mit einem virtuellen Netzwerk verbunden sind, fügen Sie eine Integrationsdienstumgebung in dieses Netzwerk ein, damit Ihre Logik-Apps direkt auf diese Systeme zugreifen können, indem sie eins der folgenden Elemente verwenden: 

  * ISE-Connector für dieses System, z.B. SQL Server
  
  * HTTP-Aktion 
  
  * Benutzerdefinierter Connector

  Für lokale Systeme, die sich nicht in einem virtuellen Netzwerk befinden oder die keine Integrationsdienstumgebungs-Connectors aufweisen, [richten Sie zuerst das lokale Datengateway ein](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Erstellen des Integrationskontos – Integrationsdienstumgebung

Um ein Integrationskonto mit Logik-Apps in einer Integrationsdienstumgebung zu verwenden, muss dieses Integrationskonto *dieselbe Umgebung* wie die Logik-Apps verwenden. Logik-Apps in einer Integrationsdienstumgebung können nur auf Integrationskonten in derselben Integrationsdienstumgebung verweisen. 

Führen Sie zum Erstellen eines Integrationskontos, das eine Integrationsdienstumgebung verwendet, die Schritte in [Erstellen von Integrationskonten](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) aus, mit Ausnahme der Eigenschaft **Standort**, an deren Stelle nun der Abschnitt **Integrationsdienstumgebungen** angezeigt wird. Wählen Sie stattdessen Ihre Integrationsdienstumgebung aus, anstelle einer Region, z. B.:

![Auswählen der Integrationsdienstumgebung](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps-Forum</a>.
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die <a href="https://aka.ms/logicapps-wish" target="_blank">Website für Logic Apps-Benutzerfeedback</a>.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Erfahren Sie mehr über die [Integration virtueller Netzwerke für Azure-Dienste](../virtual-network/virtual-network-for-azure-services.md).
