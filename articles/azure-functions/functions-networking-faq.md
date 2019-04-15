---
title: Häufig gestellte Fragen zu Netzwerken in Azure Functions
description: Hier finden Sie Antworten auf einige der häufigsten Fragen und Szenarien im Zusammenhang mit Netzwerken in Azure Functions.
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: troubleshooting
ms.date: 2/26/2019
ms.author: alkarche
ms.openlocfilehash: 7946b7f45ff3df9225a27b70ccfbdb895bfd03c4
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896693"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>Häufig gestellte Fragen zu Netzwerken in Azure Functions

Im Folgenden finden Sie eine Liste der häufig gestellten Fragen zu Netzwerken. Eine ausführlichere Übersicht finden Sie unter [Netzwerkoptionen von Azure Functions](functions-networking-options.md).

## <a name="how-do-i-set-a-static-ip-in-functions"></a>Wie richte ich eine statische IP-Adresse in Azure Functions ein?

Das Bereitstellen einer Funktion in einer App Service-Umgebung (App Service Environment, ASE) ist derzeit die einzige Möglichkeit, um eine statische ein- und ausgehende IP-Adresse für Ihre Funktion einzurichten. Ausführliche Informationen zur Verwendung einer ASE finden Sie hier: [Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung](../app-service/environment/create-ilb-ase.md)

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>Wie schränke ich den Internetzugriff auf meine Funktion ein?

Zum Einschränken des Internetzugriffs stehen Ihnen die im Folgenden aufgeführten Methoden zur Verfügung.

* [IP-Einschränkungen](../app-service/app-service-ip-restrictions.md): Sie können den eingehenden Datenverkehr Ihrer Funktions-App mit einem IP-Adressbereich einschränken.
* Entfernen aller HTTP-Trigger: Bei einigen Anwendungen genügt es, HTTP-Trigger einfach zu vermeiden und eine andere Ereignisquelle zum Auslösen der Funktion zu verwenden.

Hierbei müssen Sie vor allem beachten, dass der Editor im Azure-Portal direkten Zugriff auf Ihre ausgeführte Funktion erfordert. Alle über das Azure-Portal vorgenommenen Codeänderungen erfordern, dass die IP-Adresse des Geräts, das Sie zum Navigieren im Portal verwenden, in der Whitelist enthalten ist. Sie können jedoch weiterhin alle Funktionen auf der Registerkarte „Plattformfeatures“ verwenden, wenn Netzwerkeinschränkungen angewendet wurden.

## <a name="how-do-i-restrict-my-function-app-to-a-vnet"></a>Wie beschränke ich meine Funktions-App auf ein VNET?

Die einzige Möglichkeit, eine Funktion so einzuschränken, dass der gesamte Datenverkehr durch ein VNET fließt, ist die Verwendung einer App Service-Umgebung (ASE) mit internem Lastenausgleich (Internal Load Balancer, ILB). In diesem Fall wird Ihre Website auf einer dedizierten Infrastruktur in einem VNET bereitgestellt, und alle Trigger sowie der gesamte Datenverkehr werden über das VNET gesendet. Ausführliche Informationen zur Verwendung einer ASE finden Sie hier: [Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung](../app-service/environment/create-ilb-ase.md)

## <a name="how-can-i-access-resources-in-a-vnet-from-a-function-app"></a>Wie kann ich über eine Funktions-App auf Ressourcen in einem VNET zugreifen?

Sie können mithilfe der VNET-Integration über eine ausgeführte Funktion auf Ressourcen in einem VNET zugreifen. Weitere Informationen finden Sie unter [VNET-Integration](functions-networking-options.md#vnet-integration).

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>Wie greife ich auf Ressourcen zu, die durch Dienstendpunkte geschützt sind?

Mit der neuen VNET-Integration (zurzeit als Vorschauversion verfügbar) können Sie über eine ausgeführte Funktion auf Ressourcen zugreifen, die per Dienstendpunkt geschützt sind. Weitere Informationen finden Sie unter [Vorschauversion der VNET-Integration](functions-networking-options.md#preview-vnet-integration).

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-vnet"></a>Wie kann ich eine Funktion über eine Ressource in einem VNET auslösen?

Sie können eine Funktion nur über eine Ressource in einem VNET auslösen, indem Sie Ihre Funktions-App in einer App Service-Umgebung bereitstellen. Ausführliche Informationen zur Verwendung einer ASE finden Sie unter [Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung](../app-service/environment/create-ilb-ase.md).


## <a name="how-can-i-deploy-my-function-app-in-a-vnet"></a>Wie kann ich meine Funktions-App in einem VNET bereitstellen?

Die Bereitstellung in einer App Service-Umgebung ist die einzige Möglichkeit zum Erstellen einer Funktions-App, die sich vollständig innerhalb eines VNET befindet. Ausführliche Informationen zur Verwendung einer ASE mit internem Lastenausgleich finden Sie hier: [Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung](https://docs.microsoft.com/azure/app-service/environment/create-ilb-ase).

Informationen zu Szenarien, in denen Sie nur unidirektionalen Zugriff auf VNET-Ressourcen oder eine weniger umfassende Netzwerkisolation benötigen, finden Sie in der [Übersicht über die Netzwerkoptionen von Azure Functions](functions-networking-options.md).
