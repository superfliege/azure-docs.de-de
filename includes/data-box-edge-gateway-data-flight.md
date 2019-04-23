---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: bea14544949f90290bf3878295b49f1e08be9eea
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684412"
---
Während der Übertragung gilt für Daten Folgendes:

- Für Daten, die zwischen dem Gerät und Azure übertragen werden, wird TLS 1.2 (Standard) verwendet. Es erfolgt kein Fallback auf TLS 1.1 oder auf ältere Versionen. Sollte TLS 1.2 nicht unterstützt werden, wird die Agent-Kommunikation blockiert. TLS 1.2 wird auch für portal- und SDK-basierte Verwaltungsvorgänge benötigt.
- Wenn die Clients über die lokale Webbenutzeroberfläche in einem Browser auf Ihr Gerät zugreifen, wird TLS 1.2 (Standard) als sicheres Standardprotokoll verwendet.

    - Es empfiehlt sich, Ihren Browser für die Verwendung von TLS 1.2 zu konfigurieren.
    - Sollte der Browser TLS 1.2 nicht unterstützen, können Sie TLS 1.1 oder TLS 1.0 verwenden.
- Um die Daten während des Kopierens von Ihren Datenservern zu schützen, empfiehlt sich die Verwendung von SMB 3.0 mit Verschlüsselung.
