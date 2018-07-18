---
title: Includedatei
description: Includedatei
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c04514218c7ed8dfd72b94345d2deb88e663fda1
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740560"
---
[Azure-Richtlinien](/azure/azure-policy/) helfen Ihnen dabei, sicherzustellen, dass alle Ressourcen im Abonnement die Unternehmensstandards erfüllen. Verwenden Sie Richtlinien, um die Kosten zu reduzieren, indem Sie die Bereitstellungsoptionen auf genehmigte Ressourcentypen und SKUs beschränken. Sie definieren die Regeln und Aktionen für Ihre Ressourcen, und diese Regeln werden während der Bereitstellung automatisch erzwungen. Sie können beispielsweise die Typen von Ressourcen steuern, die bereitgestellt werden. Sie können auch die zulässigen Standorte für Ressourcen eingrenzen. Einige Richtlinien verweigern bestimmte Aktionen, während andere Richtlinien die Überwachung einer Aktion festlegen.

Richtlinien ergänzen die rollenbasierte Zugriffssteuerung (RBAC). Die RBAC gilt speziell für den Benutzerzugriff. Es handelt sich dabei um ein System für das standardmäßige Verweigern und explizite Zulassen. Bei einer Richtlinie stehen Ressourceneigenschaften während und nach der Bereitstellung im Mittelpunkt. Es handelt sich also um ein System für das standardmäßige Zulassen und explizite Verweigern.

Sie sollten im Zusammenhang mit Richtlinien zwei Konzepte verstanden haben – *Richtliniendefinitionen* und *Richtlinienzuweisungen*. Eine Richtliniendefinition beschreibt die Verwaltungsbedingungen, die Sie erzwingen möchten. Eine Richtlinienzuweisung aktiviert eine Richtliniendefinition für einen bestimmten Bereich.

![Zuweisen von Richtlinien](./media/resource-manager-governance-policy/policy-concepts.png)

Azure bietet mehrere integrierte Richtliniendefinitionen, die Sie ohne Änderungen verwenden können. Sie übergeben Parameterwerte, um die Werte anzugeben, die in Ihrem Bereich zulässig sind. Wenn die integrierte Richtliniendefinition Ihre Anforderungen nicht erfüllt, können Sie [benutzerdefinierte Richtliniendefinitionen erstellen](../articles/azure-policy/create-manage-policy.md).
