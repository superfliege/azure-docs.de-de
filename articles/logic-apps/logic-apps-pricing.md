---
title: Preise und Abrechnung – Azure Logic Apps | Microsoft-Dokumentation
description: Erfahren Sie, wie Preisdetails und Abrechnung für Azure Logic Apps funktionieren.
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
manager: carmonm
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.topic: article
ms.date: 02/26/2019
ms.openlocfilehash: 9b5452f112c6325dafd5edbe693b90ec2a94abc0
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2019
ms.locfileid: "56990236"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Preismodell für Azure Logic Apps

Sie können automatisierte Integrationsworkflows erstellen und ausführen, die in der Cloud skaliert werden können, wenn Sie Azure Logic Apps verwenden. Hier sind die Details zur Abrechnung und zu den Preisen für Logic Apps angegeben. 

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Modell für nutzungsbasierte Preise

Bei neuen Logik-Apps, die unter dem öffentlichen oder „globalen“ Logic Apps-Dienst ausgeführt werden, bezahlen Sie nur für die tatsächliche Nutzung. Für diese Logik-Apps werden ein nutzungsbasierter Tarif und ein entsprechendes Preismodell verwendet. In Ihrer Logik-App-Definition ist jeder Schritt eine Aktion. Aktionen umfassen den Trigger, die Schritte der Ablaufsteuerung, integrierte Aktionen und Connectoraufrufe. Logic Apps rechnet alle Aktionen ab, die in Ihrer Logik-App ausgeführt werden.  
Weitere Informationen finden Sie unter [Logik-Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Feststehendes Preismodell

Für neue Logik-Apps, die in einer [*Integrationsdienstumgebung*  (Integration Service Environment, ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) ausgeführt werden, zahlen Sie einen festen monatlichen Preis für integrierte Aktionen und Standardconnectors. Eine ISE ermöglicht Ihnen das Erstellen und Ausführen von isolierten Logik-Apps, die auf Ressourcen in einem virtuellen Azure-Netzwerk zugreifen können. 

Ihre ISE-Basiseinheit hat eine feste Kapazität – wenn Sie mehr Durchsatz benötigen, können Sie entweder während der Erstellung oder danach [weitere Skalierungseinheiten hinzufügen](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#add-capacity). Ihre ISE enthält einen kostenlosen Unternehmensconnector, der eine beliebige Anzahl von Verbindungen umfassen kann. Die Nutzung für weitere Unternehmensconnectors wird basierend auf dem Preis für die Unternehmensnutzung berechnet. 

> [!NOTE]
> Die ISE befindet sich in der Phase der [*öffentlichen Vorschau*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Weitere Informationen finden Sie unter [Logik-Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="triggers"></a>

## <a name="triggers"></a>Trigger

Trigger sind spezielle Aktionen, mit denen eine Logik-App-Instanz erstellt wird, wenn ein bestimmtes Ereignis eintritt. Trigger agieren auf verschiedene Weise, wovon abhängt, wie die Logik-App nutzungsbezogen abgerechnet wird.

* **Abfragetrigger**: Mit diesem Trigger wird ein Endpunkt fortlaufend auf Nachrichten überprüft, die die Kriterien für das Erstellen einer Logik-App-Instanz und Starten des Workflows erfüllen. Auch wenn keine Logik-App-Instanz erstellt wird, rechnet Logic Apps jede Abrufanforderung als Ausführung ab. Um das Abrufintervall festzulegen, richten Sie den Trigger über den Logik-App-Designer ein.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Webhooktrigger**: Dieser Trigger wartet darauf, dass von einem Client eine Anforderung an einen bestimmten Endpunkt gesendet wird. Jede Anforderung, die an den Webhookendpunkt gesendet wird, wird als eine Aktionsausführung gezählt. Sowohl beim Anforderungstrigger als auch beim HTTP-Webhooktrigger handelt es sich beispielsweise um Webhooktrigger.

* **Wiederholungstrigger**: Dieser Trigger erstellt eine Logik-App-Instanz basierend auf dem Wiederholungsintervall, das Sie im Trigger einrichten. Sie können beispielsweise einen Wiederholungstrigger einrichten, der alle drei Tage oder nach einem komplexeren Zeitplan ausgeführt wird.

## <a name="actions"></a>Aktionen

Logic Apps rechnet integrierte Aktionen als native Aktionen ab. Integrierte Aktionen enthalten beispielsweise Aufrufe per HTTP, Aufrufe von Azure Functions oder API Management und Schritte der Ablaufsteuerung, z.B. Schleifen und Bedingungen. 
- Diese verfügen jeweils über einen eigenen Aktionstyp. Aktionen, mit denen [Connectors](https://docs.microsoft.com/connectors) aufgerufen werden, haben den Typ „ApiConnection“. Diese Connectors sind als Standard- oder Unternehmensconnectors klassifiziert, deren Verbrauch zu den jeweiligen [Preisen][pricing] abgerechnet wird. Unternehmensconnectors in der *Vorschau* werden als Standardconnectors in Rechnung gestellt.

Logic Apps rechnet alle erfolgreich und nicht erfolgreich ausgeführten Aktionen als Aktionsausführungen ab. Die folgenden Aktionen werden von Logic Apps nicht abgerechnet: 

* Aktionen, die aufgrund von nicht erfüllten Bedingungen übersprungen werden
* Aktionen, die nicht ausgeführt werden, weil die Logik-App vor dem Abschluss beendet wurde

Deaktivierte Logik-Apps werden nicht berechnet, während sie deaktiviert sind, da damit keine neuen Instanzen erstellt werden können.

> [!NOTE]
> Nachdem Sie eine Logik-App deaktiviert haben, kann es einige Zeit dauern, bis die derzeit ausgeführten Instanzen vollständig beendet werden.

Für Aktionen, die innerhalb von Schleifen ausgeführt werden, zählt Logic Apps jede Aktion pro Zyklus der Schleife. Angenommen, Sie verfügen über eine „for each“-Schleife, mit der eine Liste verarbeitet wird. Logic Apps rechnet eine Aktion in dieser Schleife ab, indem die Anzahl von Listenelementen mit der Anzahl von Aktionen der Schleife multipliziert wird. Anschließend wird die Aktion hinzugefügt, mit der die Schleife gestartet wird. Die Berechnung für eine Liste mit zehn Elementen lautet (10 * 1) + 1, und es ergeben sich elf Aktionsausführungen.

## <a name="integration-account-usage"></a>Nutzung des Integrationskontos

Die verbrauchsbasierte Nutzung gilt für [Integrationskonten](logic-apps-enterprise-integration-create-integration-account.md), mit denen Sie die Features [B2B/EDI](logic-apps-enterprise-integration-b2b.md) und [XML-Verarbeitung](logic-apps-enterprise-integration-xml.md) in Logic Apps ohne zusätzliche Kosten erkunden, entwickeln und testen können. Sie können ein Integrationskonto pro Region verwenden. Für jedes Integrationskonto kann eine bestimmte maximale [Anzahl von Artefakten](../logic-apps/logic-apps-limits-and-config.md) gespeichert werden, z.B. Handelspartner, Vereinbarungen, Karten, Schemas, Assemblys, Zertifikate, Batchkonfigurationen usw.

Für Logic Apps sind außerdem Basic- und Standard-Integrationskonten mit unterstützter Logic Apps-SLA verfügbar. Sie können Basic-Integrationskonten verwenden, wenn Sie nur die Behandlung von Nachrichten nutzen oder als Partner für ein kleines Unternehmen fungieren möchten, das eine Handelspartnerbeziehung mit einem größeren Unternehmen eingegangen ist. Standard-Integrationskonten unterstützen komplexere B2B-Beziehungen und eine höhere Anzahl von Entitäten, die Sie verwalten können. Weitere Informationen finden Sie in der [Azure-Preisübersicht](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Nächste Schritte

* [Informieren Sie sich ausführlicher über Logic Apps.][whatis]
* [Erstellen Ihrer ersten Logik-App][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

