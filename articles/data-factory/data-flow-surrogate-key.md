---
title: 'Azure Data Factory Mapping Data Flow: Transformation für Ersatzschlüssel'
description: 'Azure Data Factory Mapping Data Flow: Transformation für Ersatzschlüssel'
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 62e9879f6bc75f042669ecb931ca7459d1317cc7
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271256"
---
# <a name="azure-data-factory-mapping-data-flow-surrogate-key-transformation"></a>Azure Data Factory Mapping Data Flow: Transformation für Ersatzschlüssel

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verwenden Sie die Transformation für Ersatzschlüssel, um Ihrem Datenflussrowset einen inkrementellen, nicht geschäftlichen beliebigen Schlüsselwert hinzuzufügen. Dies ist nützlich, wenn Sie Dimensionstabellen in einem analytischen Sternschema-Datenmodell entwerfen, bei dem jedes Element in Ihren Dimensionstabellen über einen eindeutigen Schlüssel verfügen muss, der kein geschäftlicher Schlüssel ist (Bestandteil der Kimball-DW-Methode).

![Transformation für Ersatzschlüssel](media/data-flow/surrogate.png "Transformation für Ersatzschlüssel")

Die „Schlüsselspalte“ ist der Name, den Sie Ihrer neuen Ersatzschlüsselspalte zuweisen.

„Startwert“ ist der Anfangspunkt des inkrementellen Werts.
