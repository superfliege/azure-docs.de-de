---
title: "Übersicht über Azure serverlose | Microsoft Docs"
description: "Erstellen Sie leistungsstarke Lösungen in der Cloud, ohne im Wesentlichen Infrastruktur."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 6803e22a78e27c15ff4fec301cd5bdd55aacd3e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-azure-serverless-with-functions-and-logic-apps"></a>Übersicht über Azure serverlose mit Funktionen und Logik-Apps

Serverlose Anwendungen bieten Vorteile einer Zunahme der Geschwindigkeit der Entwicklung, Reduzierung der erforderliche Code und Einfachheit mit Skalierung.  In diesem Artikel wechselt in den verschiedenen Attributen von serverlose Lösungen und Azure serverlose Angebote.

## <a name="what-is-serverless"></a>Was ist serverlose?

Serverlose bedeutet nicht, es sind keine Server - bedeutet nur, dass der Entwickler nicht Servern kümmern muss.  Ein großer Teil der herkömmlichen Anwendungsentwicklung ist Beantwortung von Fragen zu skalieren, hosten und überwachungslösungen zu übergeben, um die Anforderungen der Anwendung zu erfüllen.  Mit serverlose werden diese Fragen als Teil der Lösung durchgeführt.  Darüber hinaus sind serverlose Anwendungen in einem Plan nutzungsbasierte Abrechnung.  Wenn die Anwendung nie verwendet wird, niemals Gebühren fallen.  Diese Funktionen ermöglichen Entwicklern das ausschließlich auf die Geschäftslogik der Lösung konzentrieren.

Die Hauptdienste in Azure, um serverlose sind [Azure Funktionen](https://azure.microsoft.com/services/functions/) und [Azure-Logik-Apps](https://azure.microsoft.com/services/logic-apps/).  Beide dieser Lösungen führen Sie die oben genannten Prinzipien und ermöglichen Entwicklern das Erstellen robuster Cloud-Anwendungen mit minimalem Codeeinsatz.

## <a name="what-are-azure-functions"></a>Was sind Funktionen für Azure?

Azure-Funktionen ist eine Lösung für die Ausführung von problemlos kleine Teile des Codes "Funktionen" in der Cloud. Sie können den Code schreiben benötigten für das Problem zur hand ohne Rücksicht auf eine gesamte Anwendung oder der Infrastruktur, um es auszuführen. Funktionen können Ihre Produktivität Entwicklung sogar noch stärker, und Sie können Ihre Entwicklungssprache Wahl, z. B. c#, f#, Node.js, Python oder PHP verwenden. Sie Zahlen nur für die Zeit, die der Code ausgeführt wird und Azure skaliert nach Bedarf.

Wenn Sie direkt los und erste Schritte mit Azure-Funktionen möchten, beginnen Sie mit [Erstellen Ihrer ersten Azure-Funktion](../azure-functions/functions-create-first-azure-function.md). Wenn Sie mehr technische Informationen zu Funktionen suchen, finden Sie unter der [Entwicklerreferenz](../azure-functions/functions-reference.md).

## <a name="what-are-azure-logic-apps"></a>Was sind Azure-Logik-Apps?

Azure-Logik-Apps bietet eine Möglichkeit zur Vereinfachung und skalierbare Integrationen und Workflows in der Cloud zu implementieren. Es bietet es sich um einen visuellen Designer zum Modellieren und Automatisieren des Prozesses als eine Reihe von Schritten, die einen Workflow aufgerufen.  Es gibt [viele Connectors](../connectors/apis-list.md) für Cloud- und lokale Dienste eine serverlose-app für andere APIs schnell zu verbinden.  Eine Logik-app beginnt mit einem Trigger (wie "beim ein Konto auf Dynamics CRM hinzufügen") und nach dem Auslösen von Ereignissen viele Kombinationen von Aktionen, Konvertierungen und Bedingung Logik beginnen kann.  Logik-Apps ist eine gute Wahl, wenn verschiedenen Azure-Funktionen in einem Prozess - orchestriert, insbesondere wenn muss Interaktion mit einem externen System oder der API.

Starten Sie zum Einstieg in die Logik-Apps mit [Erstellen Ihrer erste Logik-app](logic-apps-create-a-logic-app.md).  Wenn Sie mehr technische Informationen zur Logik-Apps suchen, finden Sie unter der [Entwicklerreferenz](logic-apps-workflow-actions-triggers.md).

## <a name="how-can-i-build-and-deploy-serverless-applications-in-azure"></a>Wie kann ich erstellen und Bereitstellen von serverlose Anwendungen in Azure?

Azure bietet einen umfangreichen Satz von Tools für die Entwicklung, Bereitstellung und Verwaltung von serverlose apps.  Apps können erstellt werden, direkt im Azure-Portal oder mit [aus Visual Studio-Tools](logic-apps-serverless-get-started-vs.md).  Sobald eine Anwendung entwickelt wurde möglich [sofort bereitgestellt](logic-apps-create-deploy-template.md).  Azure stellt auch die Überwachung für serverlose apps bereit.  Diese Überwachung kann vom Azure-Portal, über die API oder SDKs oder mit integrierten Tools OMS und Application Insights zugegriffen werden.

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte zum Erstellen einer serverlosen app in Visual Studio](logic-apps-serverless-get-started-vs.md)
* [Erstellen eines Kunden Insights-Dashboards mit serverlose](logic-apps-scenario-social-serverless.md)
* [Erstellen Sie eine Vorlage für eine Logik-app](logic-apps-create-deploy-template.md)