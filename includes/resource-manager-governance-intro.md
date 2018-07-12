---
title: Includedatei
description: Includedatei
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 2c16e82ccf259a4cc5ae8fcf35b2dd6b5d50ee2d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740572"
---
Bei der Bereitstellung von Ressourcen in Azure sind Sie äußerst flexibel in Bezug auf die bereitzustellenden Ressourcentypen sowie den Speicherort und die Einrichtung der Ressourcen. Durch diese Flexibilität können jedoch mehr Optionen bereitgestellt werden, als Sie in Ihrer Organisation zulassen möchten. Bei der Bereitstellung von Ressourcen in Azure stellen Sie unter Umständen folgende Überlegungen an:

* Wie erfülle ich die rechtlichen Vorgaben hinsichtlich der Datenhoheit in bestimmten Ländern?
* Wie können Kosten gesteuert werden?
* Wie stelle ich sicher, dass kritische Systeme nicht versehentlich geändert werden?
* Wie verfolge ich Ressourcenkosten nach und rechne sie korrekt ab?

In diesem Artikel werden diese Fragen beantwortet. Sie erhalten insbesondere Informationen zu den folgenden Schritten:

> [!div class="checklist"]
* Zuweisen von Benutzern zu Rollen und Zuweisen der Rollen zu einem Bereich, sodass Benutzer Berechtigungen zum Ausführen der erwarteten Aktionen (und keinen weiteren Aktionen) erhalten
* Anwenden von Richtlinien, die Konventionen für Ressourcen in Ihrem Abonnement vorgeben
* Sperren von Ressourcen, die wichtig für Ihr System sind
* Kennzeichnen von Ressourcen, sodass Sie sie anhand von Werten nachverfolgen können, die sinnvoll für Ihre Organisation sind

Dieser Artikel konzentriert sich auf die Aufgaben, die Sie zum Implementieren der Governance ausführen. Eine umfassendere Erläuterung der Konzepte finden Sie unter [Governance (Kontrolle) in Azure](../articles/security/governance-in-azure.md). 
