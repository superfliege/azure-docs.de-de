---
title: Includedatei
description: Includedatei
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 01/02/2019
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 6eb7993b4dbec3ab4901dc7071d18eae98ab8ae4
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54079254"
---
> [!NOTE]
> Mehrteilige Anforderungen umfassen in der Regel drei Informationselemente:
> * Einen Header vom Typ **Content-Type**:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * Eine **Content-Disposition**:
>   * `form-data; name="metadata"`
> * Der hochzuladende Dateiinhalt
>
> **Content-Type** und **Content-Disposition** variieren je nach Verwendungsszenario.

Mehrteilige Anforderungen können programmgesteuert vorgenommen werden (über C#), über einen REST-Client oder Tools wie z.B. [Postman](https://www.getpostman.com/). REST-Clienttools weisen möglicherweise verschiedene Ebenen der Unterstützung für komplexe mehrteiligen Anforderungen auf. Überprüfen Sie, welches Tool am besten für Ihre Anforderungen geeignet ist.

> [!IMPORTANT]
> Mehrteilige Anforderungen an Verwaltungs-APIs von Azure Digital Twins bestehen aus zwei Teilen:
> * Blobmetadaten (z.B. ein zugeordneter MIME-Typ), der von **Content-Type** und **Content-Disposition** deklariert wird.
> * Blobinhalt einschließlich der unstrukturierten Inhalte einer hochzuladenden Datei
>
> Bei **PATCH**-Anforderungen wird keiner der beiden Teile benötigt. Bei **POST**- oder Erstellungsvorgängen werden dagegen beide Teile benötigt.

Der [Occupancy Quickstart-Quellcode](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/api/update.cs) enthält vollständige C#-Beispiele, die zeigen, wie mehrteilige Anforderungen an die Verwaltungs-APIs von digitalen Zwillingen in Azure gerichtet werden.