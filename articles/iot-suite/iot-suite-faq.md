---
title: "Häufig gestellte Fragen zu Azure IoT Suite | Microsoft Azure | Microsoft Docs"
description: "Häufig gestellte Fragen zu IoT Suite"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2017
ms.author: dobett
ms.openlocfilehash: 9dfb0c898e74063719b3fd242d1fa9be2b89246c
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a>Häufig gestellte Fragen zu IoT Suite

Informationen finden Sie auch in den [häufig gestellten Fragen](iot-suite-faq-cf.md) zur verbundenen Factory.

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a>Wo kann ich den Quellcode für die vorkonfigurierte Lösung finden?

Der Quellcode wird in den folgenden GitHub-Repositorys gespeichert:

* [Remote monitoring preconfigured solution (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) (Vorkonfigurierte Remoteüberwachungslösung [.NET])
* [Remote monitoring preconfigured solution (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java) (Vorkonfigurierte Remoteüberwachungslösung [.NET])
* [Predictive maintenance preconfigured solution](https://github.com/Azure/azure-iot-predictive-maintenance) (Vorkonfigurierte Predictive Maintenance-Lösung)
* [Connected factory preconfigured solution](https://github.com/Azure/azure-iot-connected-factory) (Vorkonfigurierte Lösung für eine verbundene Factory)

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Wie hoch sind die Kosten für die Bereitstellung der neuen Remoteüberwachungslösung?

Die neue vorkonfigurierte Lösung bietet zwei Bereitstellungsoptionen:

* Die Option *Basic* für Entwickler, die niedrigere Entwicklungskosten wünschen, oder für Kunden, die eine Demonstrationsversion oder ein Proof of Concept erstellen möchten.
* Die Option *Standard* für Unternehmen, die eine produktionsbereite Infrastruktur bereitstellen möchten.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>Wie kann ich sicherstellen, dass ich die Kosten beim Entwickeln meiner Lösung gering halte?

Zusätzlich zur Bereitstellung von zwei unterschiedlichen Bereitstellungen verfügt die neue Remoteüberwachungslösung über eine Einstellung zum Aktivieren oder Deaktivieren aller simulierten Geräte nach Bedarf. Durch die Deaktivierung der Simulation werden die in der Lösung erfassten Daten und somit die Gesamtkosten reduziert.

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-preconfigured-solutions"></a>Ist die neue Microservices-Architektur für alle drei vorkonfigurierten Lösungen verfügbar?

Derzeit wird die Microservices-Architektur nur in der Remoteüberwachungslösung verwendet, da sie das umfangreichste Szenario abdeckt.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Welche Vorteile bietet die neue auf Microservices basierende Open-Source-Architektur im neuen Update?

In den letzten beiden Jahren hat sich die Cloudarchitektur stark weiterentwickelt. Microservices haben sich als hervorragendes Muster für mehr Skalierbarkeit und Flexibilität ohne Einbußen bei der Entwicklungsgeschwindigkeit erwiesen. Mehrere Microsoft-Dienste nutzen dieses Architekturmuster intern mit hervorragenden Ergebnissen bei Zuverlässigkeit und Skalierbarkeit. Wir setzen diese Erkenntnisse in die Praxis um, damit unsere Kunden davon profitieren können.

### <a name="is-the-new-preconfigured-solution-available-in-the-same-geographic-region-as-the-existing-solution"></a>Ist die neue vorkonfigurierte Lösung in den gleichen geografischen Regionen wie die vorhandene Lösung verfügbar?

Ja, die neue Remoteüberwachung ist in den gleichen geografischen Regionen verfügbar.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Worin besteht der Unterschied zwischen der Basic- und der Standard-Bereitstellung? Wie treffe ich die Wahl zwischen den beiden Bereitstellungsoptionen?

Jede Bereitstellungsoption entspricht unterschiedlichen Anforderungen. Die Basic-Bereitstellung dient dem Einstieg und der Entwicklung eines Proof of Concept und kleiner Pilotprojekte. Sie bietet eine optimierte Architektur mit den erforderlichen Mindestressourcen und geringen Kosten. Die Standard-Bereitstellung dient dem Erstellen und Anpassen einer produktionsbereiten Lösung und bietet eine Bereitstellung mit allen dafür erforderlichen Elementen. Für Zuverlässigkeit und Skalierbarkeit werden Anwendungsmicroservices als Docker-Container erstellt und über einen Orchestrator bereitgestellt (standardmäßig ist Kubernetes ausgewählt). Der Orchestrator ist für die Bereitstellung, Skalierung und Verwaltung der Anwendung zuständig. Sie sollten eine Option basierend auf Ihren aktuellen Anforderungen auswählen. Je nach Projektphase können Sie jeweils eine der beiden Optionen oder eine Kombination aus beiden verwenden.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-suite"></a>Kann ich meine bestehenden Investitionen in Azure IoT Suite weiterhin nutzen?

Ja. Jede aktuell vorhandene Lösung wird in Ihrem Azure-Abonnement weiterhin ausgeführt, und der Quellcode bleibt in GitHub verfügbar.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Was ist der Unterschied zwischen dem Löschen einer Ressourcengruppe im Azure-Portal und dem Klicken auf "Löschen" für eine vorkonfigurierte Lösung in "azureiotsuite.com"?

* Wenn Sie in [azureiotsuite.com](https://www.azureiotsuite.com/) eine vorkonfigurierte Lösung löschen, werden alle Ressourcen gelöscht, die beim Erstellen der vorkonfigurierten Lösung bereitgestellt waren. Wenn Sie dieser Ressourcengruppe weitere Ressourcen hinzugefügt haben, werden diese ebenfalls gelöscht.
* Wenn Sie die Ressourcengruppe im [Azure-Portal](https://portal.azure.com) löschen, werden nur die Ressourcen in dieser Ressourcengruppe gelöscht. Sie müssen auch die Azure Active Directory-Anwendung löschen, die der vorkonfigurierten Lösung zugeordnet ist.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Wie viele IoT Hub-Instanzen kann ich in einem Abonnement bereitstellen?

Standardmäßig können Sie [10 IoT Hubs pro Abonnement](../azure-subscription-service-limits.md#iot-hub-limits) bereitstellen. Sie können ein [Azure-Supportticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) erstellen, um dieses Limit zu erhöhen. Da jede vorkonfigurierte Lösung einen neuen IoT Hub bereitstellt, können Sie somit in einem Abonnement nur bis zu zehn vorkonfigurierte Lösungen bereitstellen.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Wie viele Instanzen von Cosmos DB kann ich in einem Abonnement bereitstellen?

50. Sie können ein [Azure-Supportticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) erstellen, um dieses Limit zu erhöhen. Standardmäßig können Sie jedoch nur 50 Cosmos DB-Instanzen pro Abonnement bereitstellen.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Wie viele Bing Maps-APIs im Tarif „Free“ kann ich in einem Abonnement bereitstellen?

Zwei. In einem Azure-Abonnement können für Bing Karten für Unternehmen nur zwei Tarife für die erste interne Transaktionsebene erstellt werden. Die Remoteüberwachungslösung wird standardmäßig mit dem Tarif für die erste interne Transaktionsebene bereitgestellt. Daher können Sie in einem Abonnement ohne Modifikationen nur bis zu zwei vorkonfigurierte Remoteüberwachungslösungen bereitstellen.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Kann ich eine vorkonfigurierte Lösung erstellen, wenn ich Microsoft Azure für DreamSpark besitze?

Derzeit können Sie keine vorkonfigurierte Lösung mit einem Konto für [Microsoft Azure für DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99) erstellen. Sie können jedoch in wenigen Minuten ein [kostenloses Testkonto für Azure](https://azure.microsoft.com/free/) einrichten, mit dem Sie eine vorkonfigurierte Lösung erstellen können.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Kann ich eine vorkonfigurierte Lösung erstellen, wenn ich über ein CSP-Abonnement (Cloud Solution Provider, Cloudlösungsanbieter) verfüge?

Derzeit können Sie keine vorkonfigurierte Lösung erstellen, wenn Sie über ein CSP-Abonnement (Cloud Solution Provider, Cloudlösungsanbieter) verfügen. Sie können jedoch in wenigen Minuten ein [kostenloses Testkonto für Azure](https://azure.microsoft.com/free/) einrichten, mit dem Sie eine vorkonfigurierte Lösung erstellen können.

### <a name="how-do-i-delete-an-aad-tenant"></a>Wie lösche ich einen AAD-Mandanten?

Informationen hierzu finden Sie im Blogbeitrag von Eric Golpe: [Walkthrough of Deleting an Azure AD Tenant](http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx) (Exemplarische Vorgehensweise zum Löschen eines Azure AD-Mandanten).

### <a name="next-steps"></a>Nächste Schritte

Sie können auch einige andere Features und Funktionen der vorkonfigurierten IoT Suite-Lösungen ausprobieren:

* [Übersicht über die vorkonfigurierte Predictive Maintenance-Lösung](iot-suite-predictive-overview.md)
* [Übersicht über die vorkonfigurierte Lösung für eine verbundene Factory](iot-suite-connected-factory-overview.md)
* [Sicherheit im Internet der Dinge von Anfang an](securing-iot-ground-up.md)