---
title: Includedatei
description: Includedatei
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 03/22/2018
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: e01eebe41010135d0dc0a2cb4170e6b6687ff546
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292684"
---
| Ressource | Begrenzung |
| --- | --- |
| Maximale Anzahl von Skalierungseinheiten | Zehn pro Region<sup>1</sup> |
| Cachegröße | 5 GB pro Einheit<sup>2</sup> |
| Gleichzeitige Back-End-Verbindungen<sup>3</sup> pro HTTP-Autorität | 2.048 pro Einheit<sup>4</sup> |
| Maximale Größe zwischengespeicherter Antworten | 2 MB |
| Maximale Größe eines Richtliniendokuments | 256 KB<sup>5</sup> | 
| Maximale Anzahl benutzerdefinierter Gatewaydomänen pro Dienstinstanz<sup>6</sup> | 20 | 
| Maximale Anzahl von Instanzen pro Abonnement<sup>7</sup> | 5 | 
| Maximale Anzahl von Abonnements pro Dienstinstanz<sup>7</sup> | 500 |
| Maximale Anzahl von Clientzertifikaten pro Dienstinstanz<sup>7</sup> | 50 | 
| Maximale Anzahl von APIs pro Dienstinstanz<sup>7</sup> | 50 | 
| Maximale Anzahl von API-Vorgängen pro Dienstinstanz<sup>7</sup> | 1000 | 
| Maximale Gesamtdauer der Anforderungen<sup>7</sup> | 30 Sekunden | 
| Maximale gepufferte Nutzlastgröße<sup>7</sup> | 2 MB | 


<sup>1</sup> Die Skalierungsgrenzen sind vom Tarif abhängig. Die Tarife und die dazugehörigen Grenzwerte für die Skalierung finden Sie unter [API Management – Preise](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup> Die Cachegröße pro Einheit ist vom Tarif abhängig. Die Tarife und die dazugehörigen Grenzwerte für die Skalierung finden Sie unter [API Management – Preise](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup> Verbindungen werden in Pools zusammengefasst und wiederverwendet, sofern sie nicht explizit durch das Back-End geschlossen wurden.<br/>
<sup>4</sup> Pro Einheit der Tarife „Basic“, „Standard“ und „Premium“. Der Developer-Tarif ist auf 1.024 beschränkt. Der Tarif „Consumption“ ist nicht betroffen.<br/> 
<sup>5</sup> In den Tarifen „Basic“, „Standard“ und „Premium“. Im Tarif „Consumption“ ist die Größe des Richtliniendokuments auf 4 KB beschränkt.<br/>
<sup>6</sup> Nur im Tarif „Premium“ verfügbar.<br/>
<sup>7</sup> Gilt für nur den Tarif „Consumption“.<br/>



