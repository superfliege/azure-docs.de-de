---
title: "Übersicht über verwaltete Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier werden die Begriffe für verwaltete Azure-Anwendungen beschrieben."
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/26/2017
ms.author: tomfitz
ms.openlocfilehash: 682b7577135e327457976dc77ce4b4364bd12e48
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="azure-managed-applications-overview"></a>Übersicht über verwaltete Azure-Anwendungen

Verwaltete Azure-Anwendungen ermöglichen es Ihnen, Ihren Kunden Cloudlösungen anzubieten, die diese einfach bereitstellen und betreiben können. Sie implementieren die Infrastruktur und bieten kontinuierlichen Support. Wenn Sie eine verwaltete Anwendung für alle Kunden verfügbar machen möchten, können Sie sie im Azure Marketplace veröffentlichen. Wenn Sie sie nur Benutzern in Ihrer Organisation zur Verfügung stellen möchten, können Sie sie in einem internen Katalog veröffentlichen. 

Eine verwaltete Anwendung ähnelt bis auf einen wichtigen Unterschied einer Marketplace-Lösungsvorlage. Bei einer verwalteten Anwendung werden die Ressourcen in einer Ressourcengruppe bereitgestellt, die vom Herausgeber der App verwaltet wird. Die Ressourcengruppe befindet sich zwar im Abonnement des Verbrauchers, eine Identität im Mandanten des Herausgebers hat jedoch Zugriff auf die Ressourcengruppe. Als Herausgeber geben Sie die Kosten für den kontinuierlichen Support der Lösung an.

## <a name="advantages-of-managed-applications"></a>Vorteile von verwalteten Anwendungen

Verwaltete Anwendungen bauen Hürden für Verbraucher ab, die Ihre Lösungen verwenden. Verbraucher benötigen keine Cloudinfrastrukturkenntnisse, um Ihre Lösung verwenden zu können. Ihr Zugriff auf kritische Ressourcen ist beschränkt. Der Verbraucher muss nicht befürchten, bei der Verwaltung einen Fehler zu machen. 

Mithilfe von verwalteten Anwendungen können Sie eine anhaltende Beziehung zu Ihren Verbrauchen aufbauen. Sie definieren die Bedingungen für die Verwaltung der Anwendung, und sämtliche Gebühren werden über Azure abgerechnet.

Obwohl Kunden diese verwalteten Anwendungen in ihren Abonnements bereitstellen, müssen sie diese nicht verwalten, aktualisieren oder betreuen. Sie können sicherstellen, dass alle Kunden genehmigte Versionen verwenden. Für die Verwaltung dieser Anwendungen müssen sich Kunden kein anwendungsspezifisches Domänenwissen aneignen. Kunden erhalten automatisch Anwendungsupdates, ohne sich mit der Behandlung und Diagnose von Anwendungsproblemen befassen zu müssen. 

Für IT-Teams können Sie mit verwalteten Anwendungen vorab genehmigte Lösungen für Benutzer in der Organisation anbieten. Sie gewährleisten, dass diese Lösungen mit den Standards der Organisation konform sind.

## <a name="types-of-managed-applications"></a>Arten von verwalteten Anwendungen

Sie können Ihre verwaltete Anwendung entweder extern oder intern veröffentlichen.

![Interne oder externe Veröffentlichung](./media/overview/manage_app_options.png)

### <a name="service-catalog"></a>Dienstkatalog

Der Dienstkatalog ist ein interner Katalog mit freigegebenen Lösungen für Benutzer in einer Organisation. Mithilfe des Katalogs können Sie die Einhaltung bestimmter Organisationsstandards gewährleisten und gleichzeitig Lösungen für die Organisationen bereitstellen. Mitarbeiter können über den Katalog komfortabel das umfangreiche Spektrum von Anwendungen erkunden, die die IT-Abteilung empfohlen und freigegeben hat. Sie sehen die verwalteten Anwendungen, die auch von anderen Personen in der Organisation verwendet werden.

Informationen zum Veröffentlichen einer verwalteten Dienstkataloganwendung finden Sie unter [Erstellen einer Dienstkataloganwendung](publish-service-catalog-app.md).

### <a name="marketplace"></a>Marketplace

Anbieter, die ihre Dienste kostenpflichtig anbieten möchten, können eine verwaltete Anwendung über den Azure Marketplace verfügbar machen. Die veröffentlichte Anwendung ist für Benutzer außerhalb der Organisation verfügbar. So können Anbieter verwalteter Dienste (Managed Service Providers, MSPs), unabhängige Softwarehersteller (Independent Software Vendors, ISVs) und Systemintegratoren (SIs) ihre Lösung allen Azure-Kunden anbieten.

Informationen zum Veröffentlichen einer verwalteten Anwendung im Marketplace finden Sie unter [Erstellen einer Marketplace-Anwendung](publish-marketplace-app.md).

## <a name="resource-groups-for-managed-applications"></a>Ressourcengruppen für verwaltete Anwendungen

Die Ressourcen für eine verwaltete Anwendung befinden sich üblicherweise in zwei Ressourcengruppen. Eine wird vom Verbraucher verwaltet, die andere vom Herausgeber. Beim Definieren der verwalteten Anwendung gibt der Herausgeber die Zugriffsebenen an. Die folgende Abbildung zeigt ein Szenario, in dem der Herausgeber die Besitzerrolle für die verwaltete Ressourcengruppe anfordert. Der Herausgeber hat die Ressourcengruppe für den Verbraucher mit einer Schreibschutzsperre belegt.

![Ressourcengruppenzugriff](./media/overview/access.png)

### <a name="application-resource-group"></a>Anwendungsressourcengruppe

Diese Ressourcengruppe enthält die Instanz der verwalteten Anwendung. Diese Ressourcengruppe darf nur eine einzelne Ressource enthalten. Der Ressourcentyp der verwalteten Anwendung lautet **Microsoft.Solutions/applications**.

Der Verbraucher hat uneingeschränkten Zugriff auf die Ressourcengruppe und verwendet sie zum Verwalten des Lebenszyklus der verwalteten Anwendung.

### <a name="managed-resource-group"></a>Verwaltete Ressourcengruppe

Diese Ressourcengruppe enthält alle Ressourcen, die von der verwalteten Anwendung benötigt werden. Hierzu zählen beispielsweise die virtuellen Computer, Speicherkonten und virtuellen Netzwerke für die Lösung. Der Verbraucher hat eingeschränkten Zugriff auf diese Ressourcengruppe, da er nicht die einzelnen Ressourcen für die verwaltete Anwendung verwaltet. Der Zugriff des Herausgebers auf diese Ressourcengruppe entspricht der Rolle, die in der Definition der verwalteten Anwendung angegeben ist. Der Herausgeber kann für diese Ressourcengruppe beispielsweise die Rolle „Besitzer“ oder „Mitwirkender" anfordern.

Wenn der Verbraucher die verwaltete Anwendung löscht, wird auch die verwaltete Ressourcengruppe gelöscht.

## <a name="next-steps"></a>Nächste Schritte

* Grundlegende Informationen zum Definieren und Bereitstellen einer verwalteten Anwendung finden Sie unter [Erstellen und Bereitstellen einer verwalteten Azure-Anwendung per Azure CLI](managed-apps-quickstart-cli.md).
* Informationen zum Veröffentlichen einer internen Anwendung finden Sie unter [Erstellen einer Dienstkataloganwendung](publish-service-catalog-app.md).
* Informationen zum Veröffentlichen von verwalteten Anwendungen im Marketplace finden Sie unter [Erstellen einer Marketplace-Anwendung](publish-marketplace-app.md).

