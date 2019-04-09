---
title: Includedatei
description: Includedatei
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/21/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ad4b244b58d741ad45463297df5bd358f3ae9918
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449829"
---
Bevor Sie einem Sicherheitsprinzipal eine RBAC-Rolle zuweisen, legen Sie den Zugriffsbereich fest, den der Sicherheitsprinzipal haben soll. Es hat sich als am besten bewährt, stets nur den kleinstmöglichen Umfang an Zugriffsrechten zu gewähren.

In der folgenden Liste werden die Ebenen beschrieben, auf denen Sie den Zugriff auf Azure-Blob- und -Warteschlangenressourcen einschränken können, beginnend mit dem kleinstmöglichen Umfang:

- **Ein einzelner Container**. Bei diesem Umfang hat ein Sicherheitsprinzipal Zugriff auf alle Blobs im Container sowie auf Containereigenschaften und Metadaten.
- **Eine einzelne Warteschlange**. Bei diesem Umfang hat ein Sicherheitsprinzipal Zugriff auf Nachrichten in der Warteschlange sowie auf Warteschlangeneigenschaften und Metadaten.
- **Das Speicherkonto**. Bei diesem Umfang hat ein Sicherheitsprinzipal Zugriff auf alle Container und deren Blobs oder auf alle Warteschlangen und deren Nachrichten.
- **Die Ressourcengruppe**. Bei diesem Umfang hat ein Sicherheitsprinzipal Zugriff auf alle Container oder Warteschlangen in allen Speicherkonten der Ressourcengruppe.
- **Das Abonnement**. Bei diesem Umfang hat ein Sicherheitsprinzipal Zugriff auf alle Container oder Warteschlangen in allen Speicherkonten in allen Ressourcengruppen des Abonnements.
