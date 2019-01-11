---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: e2c8b4e74933df593dbc023dcb886dff80554a1a
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977375"
---
Die Einstellungen für den Container sind hierarchisch, und alle Container auf dem Hostcomputer verwenden gemeinsam eine Hierarchie.

Sie können eine der folgenden Methoden verwenden, um Einstellungen anzugeben:

* [Umgebungsvariablen](#environment-variable-settings)
* [Befehlszeilenargumente](#command-line-argument-settings)

Die Werte für Umgebungsvariablen haben Vorrang vor den Werten für Befehlszeilenargumente, die wiederum Vorrang vor den Standardwerten für das Containerimage haben. Wenn Sie in einer Umgebungsvariable und einem Befehlszeilenargument unterschiedliche Werte für die gleiche Konfigurationseinstellung angeben, wird vom instanziierten Container der Wert aus der Umgebungsvariable verwendet.

|Rangfolge|Ort der Einstellung|
|--|--|
|1|Umgebungsvariable| 
|2|Befehlszeile|
|3|Standardwert des Containerimages|

### <a name="environment-variable-settings"></a>Einstellungen in der Umgebungsvariable

Umgebungsvariablen bieten folgende Vorteile:

* Es können mehrere Einstellungen konfiguriert werden.
* Mehrere Container können die gleichen Einstellungen verwenden.

### <a name="command-line-argument-settings"></a>Einstellungen über Befehlszeilenargumente

Die Verwendung von Befehlszeilenargumenten hat den Vorteil, dass jeder Container andere Einstellungen verwenden kann.
