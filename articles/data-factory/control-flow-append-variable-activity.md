---
title: Aktivität „Variable anfügen“ in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Aktivität „Variable anfügen“ so festlegen, dass ein Wert einer vorhandenen Arrayvariablen hinzugefügt wird, die in einer Data Factory-Pipeline definiert ist.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: douglasl
ms.openlocfilehash: 03652ce20d82565d5714cdc43a01a9e7c3074f6a
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48903821"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Aktivität „Variable anfügen“ in Azure Data Factory

Verwenden Sie die Aktivität „Variable anfügen“ zum Hinzufügen eines Werts zu einer vorhandenen Arrayvariablen, die in einer Data Factory-Pipeline definiert ist.

## <a name="type-properties"></a>Typeigenschaften

Eigenschaft | BESCHREIBUNG | Erforderlich
-------- | ----------- | --------
name | Der Name der Aktivität in der Pipeline. | JA
Beschreibung | Text, der beschreibt, welche Aktion die Aktivität ausführt. | no
type | Der Aktivitätstyp ist AppendVariable. | Ja
value | Ein Zeichenfolgenliteral- oder Ausdrucksobjektwert, der zum Anfügen an die angegebene Variable verwendet wird. | Ja
variableName | Der Name der Variablen, die von der Aktivität geändert wird. Die Variable muss vom Typ „Array“ sein. | Ja

## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit einer verwandten Ablaufsteuerungsaktivität vertraut, die von Data Factory unterstützt wird: 

- [Aktivität „Variable festlegen“](control-flow-set-variable-activity.md)
