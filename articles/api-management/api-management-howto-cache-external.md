---
title: Verwenden eines externen Caches in Azure API Management | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen externen Cache in Azure API Management konfigurieren und verwenden.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/27/2018
ms.author: apimpm
ms.openlocfilehash: f57b6b35ffff85aad4d970cf9aa908d2a80eadf1
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620896"
---
# <a name="use-an-external-redis-cache-in-azure-api-management"></a>Verwenden eines externen Redis-Caches in Azure API Management

Neben der Nutzung des integrierten Caches ermöglicht Azure API Management auch das Zwischenspeichern von Antworten in einem externen Redis-Cache.

Durch die Verwendung eines externen Caches lassen sich einige Einschränkungen des integrierten Caches umgehen. Dies hat insbesondere folgende Vorteile:

* Vermeidung der regelmäßigen Löschung Ihres Caches im Zuge von API Management-Updates
* Mehr Kontrolle über Ihre Cachekonfiguration
* Zwischenspeicherung von mehr Daten als mit dem API Management-Tarif
* Verwendung der Zwischenspeicherung mit dem Verbrauchstarif von API Management

Ausführlichere Informationen zum Zwischenspeichern finden Sie unter [Cacherichtlinien für API Management](api-management-caching-policies.md) sowie unter [Benutzerdefiniertes Caching in Azure API Management](api-management-sample-cache-by-key.md).

![Verwenden eines eigenen Caches in API Management](media/api-management-howto-cache-external/overview.png)

Sie lernen Folgendes:

> [!div class="checklist"]
> * Hinzufügen eines externen Caches in API Management

## <a name="availability"></a>Verfügbarkeit

> [!NOTE]
> Dieses Feature steht derzeit nur im **Verbrauchstarif** von API Management zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

+ [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
+ Verständnis der [Zwischenspeicherung in Azure API Management](api-management-howto-cache.md)

## <a name="create-cache"> </a> Erstellen von Azure Redis Cache

In diesem Abschnitt erfahren Sie, wie Sie einen Redis-Cache in Azure erstellen. Falls Sie bereits über einen Redis-Cache verfügen (innerhalb oder außerhalb von Azure), können Sie <a href="#add-external-cache">direkt mit dem nächsten Abschnitt fortfahren</a>.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="add-external-cache"> </a>Hinzufügen eines externen Caches

Gehen Sie wie folgt vor, um in Azure API Management einen externen Redis-Cache hinzuzufügen:

![Verwenden eines eigenen Caches in API Management](media/api-management-howto-cache-external/add-external-cache.png)

> [!NOTE]
> Die Einstellung **Use from** (Verwenden aus) gibt an, welche regionale API Management-Bereitstellung mit dem konfigurierten Cache kommuniziert, wenn eine API Management-Konfiguration mit mehreren Regionen verwendet wird. Die als **Standard** angegebenen Caches werden durch Caches mit einem regionalen Wert überschrieben.
>
> Ein Beispiel: Angenommen, API Management wird in den Regionen „USA, Osten“, „Asien, Südosten“ und „Europa, Westen“ gehostet, und es sind zwei Caches konfiguriert: einer für **Standard** und einer für **Asien, Südosten**. In diesem Fall verwendet API Management in **Asien, Südosten** den eigenen Cache, während in den anderen beiden Regionen der Cacheeintrag **Standard** verwendet wird.

### <a name="add-an-azure-redis-cache-from-the-same-subscription"></a>Hinzufügen einer Azure Redis Cache-Instanz aus dem gleichen Abonnement

1. Navigieren Sie im Azure-Portal zu Ihrer API Management-Instanz.
2. Wählen Sie im Menü auf der linken Seite die Registerkarte **External cache** (Externer Cache) aus.
3. Klicken Sie auf die Schaltfläche **+ Hinzufügen**.
4. Wählen Sie im Dropdownfeld **Cache instance** (Cacheinstanz) Ihren Cache aus.
5. Wählen Sie **Standard** aus, oder geben Sie im Dropdownfeld **Use from** (Verwenden aus) die gewünschte Region an.
6. Klicken Sie auf **Speichern**.

### <a name="add-a-redis-cache-hosted-outside-of-the-current-azure-subscription-or-azure-in-general"></a>Hinzufügen eines Redis-Caches, der außerhalb des aktuellen Azure-Abonnements oder außerhalb von Azure gehostet wird

1. Navigieren Sie im Azure-Portal zu Ihrer API Management-Instanz.
2. Wählen Sie im Menü auf der linken Seite die Registerkarte **External cache** (Externer Cache) aus.
3. Klicken Sie auf die Schaltfläche **+ Hinzufügen**.
4. Wählen Sie im Dropdownfeld **Cache instance** (Cacheinstanz) die Option **Benutzerdefiniert** aus.
5. Wählen Sie **Standard** aus, oder geben Sie im Dropdownfeld **Use from** (Verwenden aus) die gewünschte Region an.
6. Geben Sie im Feld **Verbindungszeichenfolge** die Verbindungszeichenfolge Ihres Redis-Caches an.
7. Klicken Sie auf **Speichern**.

## <a name="use-the-external-cache"></a>Verwenden des externen Caches

Nachdem der externe Cache in Azure API Management konfiguriert wurde, kann er über Zwischenspeicherungsrichtlinien verwendet werden. Ausführliche Informationen finden Sie unter [Hinzufügen der Zwischenspeicherung zum Verbessern der Leistung in Azure API Management](api-management-howto-cache.md).

## <a name="next-steps"> </a>Nächste Schritte
* Weitere Informationen zu Richtlinien für die Zwischenspeicherung finden Sie unter [Richtlinien für die Zwischenspeicherung][Caching policies] in der [Richtlinienreferenz für API Management][API Management policy reference].
* Informationen zum Zwischenspeichern von Elementen nach Schlüssel mithilfe von Richtlinienausdrücken finden Sie unter [Benutzerdefiniertes Zwischenspeichern in Azure API Management](api-management-sample-cache-by-key.md).

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx