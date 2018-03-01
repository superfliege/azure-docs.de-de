---
title: "Häufig gestellte Fragen zu Azure IoT Suite | Microsoft Azure | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu IoT Suite"
services: iot-suite
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
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: c79c90c4f6c28153d4d299015a06a6bc37145081
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="frequently-asked-questions-for-iot-suite"></a>Häufig gestellte Fragen zu IoT Suite

Siehe auch die [häufig gestellten Fragen zur verbundenen Factory](iot-suite-faq-cf.md) und die [häufig gestellten Fragen zur Remoteüberwachung](iot-suite-faq-rm-v2.md).

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a>Wo kann ich den Quellcode für die vorkonfigurierte Lösung finden?

Der Quellcode wird in den folgenden GitHub-Repositorys gespeichert:

* [Remote monitoring preconfigured solution (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) (Vorkonfigurierte Remoteüberwachungslösung [.NET])
* [Remote monitoring preconfigured solution (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java) (Vonkonfigurierte Remoteüberwachungslösung (Java))
* [Predictive maintenance preconfigured solution](https://github.com/Azure/azure-iot-predictive-maintenance) (Vorkonfigurierte Predictive Maintenance-Lösung)
* [Connected factory preconfigured solution](https://github.com/Azure/azure-iot-connected-factory) (Vorkonfigurierte Lösung für eine verbundene Factory)

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-preconfigured-solutions"></a>Ist die neue Microservices-Architektur für alle drei vorkonfigurierten Lösungen verfügbar?

Derzeit wird die Microservices-Architektur nur in der Remoteüberwachungslösung verwendet, da sie das umfangreichste Szenario abdeckt.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Welche Vorteile bietet die neue auf Microservices basierende Open-Source-Architektur im neuen Update?

In den letzten beiden Jahren hat sich die Cloudarchitektur stark weiterentwickelt. Microservices haben sich als hervorragendes Muster für mehr Skalierbarkeit und Flexibilität ohne Einbußen bei der Entwicklungsgeschwindigkeit erwiesen. Mehrere Microsoft-Dienste nutzen dieses Architekturmuster intern mit hervorragenden Ergebnissen bei Zuverlässigkeit und Skalierbarkeit. Wir setzen diese Erkenntnisse in die Praxis um, damit unsere Kunden davon profitieren können.

### <a name="is-the-new-preconfigured-solution-available-in-the-same-geographic-region-as-the-existing-solution"></a>Ist die neue vorkonfigurierte Lösung in den gleichen geografischen Regionen wie die vorhandene Lösung verfügbar?

Ja, die neue Remoteüberwachung ist in den gleichen geografischen Regionen verfügbar.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Was ist der Unterschied zwischen dem Löschen einer Ressourcengruppe im Azure-Portal und dem Klicken auf "Löschen" für eine vorkonfigurierte Lösung in "azureiotsuite.com"?

* Wenn Sie in [azureiotsuite.com](https://www.azureiotsuite.com/) eine vorkonfigurierte Lösung löschen, werden alle Ressourcen gelöscht, die beim Erstellen der vorkonfigurierten Lösung bereitgestellt waren. Wenn Sie dieser Ressourcengruppe weitere Ressourcen hinzugefügt haben, werden diese ebenfalls gelöscht.
* Wenn Sie die Ressourcengruppe im [Azure-Portal](https://portal.azure.com) löschen, werden nur die Ressourcen in dieser Ressourcengruppe gelöscht. Sie müssen auch die Azure Active Directory-Anwendung löschen, die der vorkonfigurierten Lösung zugeordnet ist.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-suite"></a>Kann ich meine bestehenden Investitionen in Azure IoT Suite weiterhin nutzen?

Ja. Jede aktuell vorhandene Lösung wird in Ihrem Azure-Abonnement weiterhin ausgeführt, und der Quellcode bleibt in GitHub verfügbar.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Wie viele IoT Hub-Instanzen kann ich in einem Abonnement bereitstellen?

Standardmäßig können Sie [10 IoT Hubs pro Abonnement](../azure-subscription-service-limits.md#iot-hub-limits) bereitstellen. Sie können ein [Azure-Supportticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) erstellen, um dieses Limit zu erhöhen. Da jede vorkonfigurierte Lösung einen neuen IoT Hub bereitstellt, können Sie somit in einem Abonnement nur bis zu zehn vorkonfigurierte Lösungen bereitstellen.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Wie viele Instanzen von Cosmos DB kann ich in einem Abonnement bereitstellen?

50. Sie können ein [Azure-Supportticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) erstellen, um dieses Limit zu erhöhen. Standardmäßig können Sie jedoch nur 50 Cosmos DB-Instanzen pro Abonnement bereitstellen.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Wie viele Bing Maps-APIs im Tarif „Free“ kann ich in einem Abonnement bereitstellen?

Zwei. In einem Azure-Abonnement können für Bing Karten für Unternehmen nur zwei Tarife für die erste interne Transaktionsebene erstellt werden. Die Remoteüberwachungslösung wird standardmäßig mit dem Tarif für die erste interne Transaktionsebene bereitgestellt. Daher können Sie in einem Abonnement ohne Modifikationen nur bis zu zwei vorkonfigurierte Remoteüberwachungslösungen bereitstellen.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Kann ich eine vorkonfigurierte Lösung erstellen, wenn ich Microsoft Azure für DreamSpark besitze?

> [!NOTE]
> Microsoft Azure für DreamSpark wird jetzt als Microsoft Imagine für Schüler und Studenten bezeichnet.

Derzeit können Sie keine vorkonfigurierte Lösung mit einem Konto für [Microsoft Azure für DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/) erstellen. Sie können jedoch in wenigen Minuten ein [kostenloses Testkonto für Azure](https://azure.microsoft.com/free/) einrichten, mit dem Sie eine vorkonfigurierte Lösung erstellen können.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Kann ich eine vorkonfigurierte Lösung erstellen, wenn ich über ein CSP-Abonnement (Cloud Solution Provider, Cloudlösungsanbieter) verfüge?

Derzeit können Sie keine vorkonfigurierte Lösung erstellen, wenn Sie über ein CSP-Abonnement (Cloud Solution Provider, Cloudlösungsanbieter) verfügen. Sie können jedoch in wenigen Minuten ein [kostenloses Testkonto für Azure](https://azure.microsoft.com/free/) einrichten, mit dem Sie eine vorkonfigurierte Lösung erstellen können.

### <a name="how-do-i-delete-an-aad-tenant"></a>Wie lösche ich einen AAD-Mandanten?

Informationen hierzu finden Sie im Blogbeitrag von Eric Golpe: [Walkthrough of Deleting an Azure AD Tenant](http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx) (Exemplarische Vorgehensweise zum Löschen eines Azure AD-Mandanten).

### <a name="next-steps"></a>Nächste Schritte

Sie können auch einige andere Features und Funktionen der vorkonfigurierten IoT Suite-Lösungen ausprobieren:

* [Erkunden der Funktionen der vorkonfigurierten Remoteüberwachungslösung](iot-suite-remote-monitoring-explore.md)
* [Übersicht über die vorkonfigurierte Predictive Maintenance-Lösung](iot-suite-predictive-overview.md)
* [Übersicht über die vorkonfigurierte Lösung für eine verbundene Factory](iot-suite-connected-factory-overview.md)
* [Sicherheit im Internet der Dinge von Anfang an](securing-iot-ground-up.md)