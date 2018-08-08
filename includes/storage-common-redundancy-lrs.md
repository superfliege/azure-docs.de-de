---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 58d81bd4c1ce4b3af91a335039f62df08b340576
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399990"
---
Lokal redundanter Speicher (LRS) ist darauf ausgelegt, für Objekte eine Dauerhaftigkeit von mindestens 99,999999999 % (elf Neunen) in einem bestimmten Jahr bereitzustellen. Dies wird durch die Replikation von Daten innerhalb einer Speicherskalierungseinheit erreicht. Eine Speicherskalierungseinheit wird in einem Rechenzentrum in der Region gehostet, in der Sie Ihr Speicherkonto erstellt haben. Eine Schreibanforderung an ein LRS-Speicherkonto wird erst dann erfolgreich zurückgegeben, wenn die Daten in alle Replikate geschrieben wurden. Diese Replikate befinden sich in separaten Fehler- und Updatedomänen innerhalb einer Speicherskalierungseinheit.

Eine Speicherskalierungseinheit ist eine Sammlung von Speicherknotengestellen (Racks). Eine Fehlerdomäne (FD) ist eine Gruppe von Knoten, die eine physische Fehlereinheit darstellen und als Knoten angesehen werden können, die zum gleichen physischen Gestell gehören. Eine Upgradedomäne (UD) ist eine Gruppe von Knoten, die während des Vorgangs eines Dienstupgrades (Rollouts) gemeinsam aktualisiert werden. Die Replikate werden auf UDs und FDs in einer Speicherskalierungseinheit verteilt. Diese Architektur stellt sicher, dass Ihre Daten verfügbar sind, wenn ein Hardwareausfall ein einzelnes Rack betrifft, oder wenn Knoten bei einem Rollout aktualisiert werden.

LRS ist die kostengünstigste Replikationsmöglichkeit und bietet im Vergleich zu anderen Optionen die geringste Dauerhaftigkeit. Bei einem Katastrophenfall in einem Rechenzentrum (Feuer, Überschwemmung usw.) gehen ggf. alle Replikate verloren oder können nicht mehr wiederhergestellt werden. Um dieses Risiko zu minimieren, wird empfohlen, entweder zonenredundante Speicher (ZRS) oder georedundante Speicher (GRS) zu verwenden.

* Wenn Ihre Anwendung Daten speichert, die bei Datenverlust problemlos wiederhergestellt werden können, können Sie sich für LRS entscheiden.
* Einige Anwendungen sind aufgrund von Datenvorschriften auf die Replikation von Daten innerhalb eines Lands beschränkt. In einigen Fällen können sich die gekoppelten Regionen, in denen Daten für GRS-Konten repliziert werden, in einem anderen Land befinden. Weitere Informationen zu gekoppelten Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).
