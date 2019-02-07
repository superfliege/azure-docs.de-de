---
title: Includedatei
description: Includedatei
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 08328b3c872ce9e8a25db6075b00e0cee2fdf4fd
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55480507"
---
Die Clientanwendung muss wissen, ob eine Äußerung für die Anwendung unsinnig oder ungeeignet ist. Die Absicht **None** (Keine) wird jeder Anwendung bei ihrer Erstellung hinzugefügt, um zu bestimmen, ob eine Äußerung nicht von der Clientanwendung beantwortet werden kann.

Wenn LUIS für eine Äußerung die Absicht **None** (Keine) zurückgibt, kann Ihre Clientanwendung fragen, ob der Benutzer die Unterhaltung beenden oder weitere Anweisungen zum Fortsetzen der Unterhaltung angeben möchte. 

> [!CAUTION] 
> Belassen Sie die Absicht **None** (Keine) nicht leer. 

1. Klicken Sie im linken Bereich auf **Intents** (Absichten).

2. Wählen Sie die Absicht **None** (Keine) aus. Fügen Sie drei Äußerungen hinzu, die der Benutzer unter Umständen eingibt, die für Ihre Personalwesen-App jedoch nicht relevant sind:

    | Beispiele für Äußerungen|
    |--|
    |Bellende Hunde sind nervig|
    |Bestell mir eine Pizza|
    |Pinguine im Ozean|
