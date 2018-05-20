---
title: Includedatei
description: Includedatei
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 03/28/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: e035b49d9e386287baf67bba756f7b58a764acc5
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
Melden Sie sich zum Erstellen eines Caches zunächst beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie dann auf **Ressource erstellen** > **Datenbanken** > **Redis Cache**.

![Neuer Cache](media/redis-cache-create/redis-cache-new-cache-menu.png)

Konfigurieren Sie unter **Neuer Redis Cache** die Einstellungen für den neuen Cache.

| Einstellung      | Empfohlener Wert  | BESCHREIBUNG |
| ------------ |  ------- | -------------------------------------------------- |
| **DNS-Name** | Global eindeutiger Name | Der Cachename muss zwischen 1 und 63 Zeichen lang sein und darf nur Zahlen, Buchstaben und das Zeichen `-` enthalten. Der Cachename darf weder mit dem Zeichen `-` beginnen oder enden, noch mehrere aufeinanderfolgende Zeichen vom Typ `-` enthalten.  | 
| **Abonnement** | Ihr Abonnement | Das Abonnement, unter dem dieser neue Azure Redis Cache erstellt wird. | 
| **Ressourcengruppe** |  *TestResources* | Der Name der neuen Ressourcengruppe, in der Ihr Cache erstellt wird. Indem Sie alle Ressourcen für eine App in einer Gruppe zusammenfassen, können Sie sie zusammen verwalten. Wenn Sie beispielsweise die Ressourcengruppe löschen, werden alle Ressourcen gelöscht, die der App zugeordnet sind. | 
| **Location** | USA (Ost) | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in der Nähe anderer Dienste aus, die Ihren Cache verwenden. |
| **[Preisstufe](https://azure.microsoft.com/pricing/details/cache/)** |  Basic C0 (250 MB Cache) |  Der Tarif bestimmt Größe, Leistung und verfügbare Features für den Cache. Weitere Informationen finden Sie unter [Azure Redis Cache – Übersicht](../articles/redis-cache/cache-overview.md). |
| **An Dashboard anheften** |  Aktiviert | Indem Sie den neuen Cache an Ihr Dashboard anheften, können Sie ihn leichter wiederzufinden. |

![Cache erstellen](media/redis-cache-create/redis-cache-cache-create.png) 

Klicken Sie nach der Konfiguration der Einstellungen für den neuen Cache auf **Erstellen**. 

Die Erstellung des Caches kann einige Minuten dauern. Sie können den Fortschritt im Dashboard überwachen, um den Status zu überprüfen. Nach der Erstellung des Caches hat der neue Cache den Status **Wird ausgeführt** und kann verwendet werden.

![Cache erstellt](media/redis-cache-create/redis-cache-cache-created.png)

